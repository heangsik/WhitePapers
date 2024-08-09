# 목차

- [목차](#목차)
- [백엔드](#백엔드)
  - [프로젝트 생성](#프로젝트-생성)
  - [orm 설정](#orm-설정)
  - [pytest](#pytest)
  - [테스트 방법](#테스트-방법)
    - [저장시 자동 테스트](#저장시-자동-테스트)
      - [실행](#실행)
  - [MOCK사용](#mock사용)
    - [설치](#설치)
    - [DB Mock](#db-mock)
    - [Method Mock](#method-mock)
- [프론트](#프론트)
  - [프로젝트 생성](#프로젝트-생성-1)

# 백엔드

## 프로젝트 생성

```Shell
poetry new back
cd back
code .
poetry install
poetry add fastapi uvicorn[standard] python-dotenv loguru "python-jose[cryptography]" passlib[bcrypt] sqlalchemy
```

## orm 설정

- 필요 라이브러리

  ```Shell
  poetry add sqlalchemy alembic
  ```

- database.py

  ```py
  from sqlalchemy import create_engine
  from sqlalchemy.orm import sessionmaker
  from sqlalchemy.ext.declarative import declarative_base
  SQLALCHEMY_DATABASE_URL = "sqlite:///./myapi.db"

  engine = create_engine(
      SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False}
  )

  SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

  Base = declarative_base()
  ```

- database_model.py

  ```py
    from database import Base
    from sqlalchemy import Column, DateTime, ForeignKey, Integer, String
    from sqlalchemy.orm import backref
    from sqlalchemy.orm import relationship

    class Member(Base):
        __tablename__ = 'member'
        seq = Column(Integer, primary_key=True)
        id = Column(String, nullable=False)
        name = Column(String, nullable=False)
        password = Column(String, nullable=False)
        user_stat = Column(String)
        create_date = Column(DateTime, nullable=False)


    class MmemberLog(Base):
        __tablename__ = 'member_log'
        seq = Column(Integer, primary_key=True)
        member_id = Column(String, ForeignKey('member.id'))
        create_date = Column(DateTime, nullable=False)
        content = Column(String)
        member = relationship('member', backref=backref('member_log', order_by=seq))

    class Question(Base):
        __tablename__ = 'question'
        seq = Column(Integer, primary_key=True)
        member_id = Column(String, ForeignKey('member.id'))
        create_date = Column(DateTime, nullable=False)
        content = Column(String)
        member = relationship('member', backref=backref('content', order_by=seq))

    class Answer(Base):
        __tablename__ = 'answer'
        seq = Column(Integer, primary_key=True)
        question_seq = Column(Integer, ForeignKey('question.seq'))
        create_date = Column(DateTime, nullable=False)
        content = Column(String)
        question = relationship('question', backref=backref('answer', order_by=seq))
  ```

- alembic init migration -> 초기화
  - alembic.ini 수정
    > sqlalchemy.url = sqlite:///./myapi.db -> database.py 파일의 SQLALCHEMY_DATABASE_URL 와 동일 한 값
  - env.py 수정
    > target_metadata = models.Base.metadata -> database_model.파일
- alembic revision --autogenerate
  - 모델이 변경되면 재실행 필요
- alembic upgrade head -> 테이블이 생성된다.

## pytest

## 테스트 방법

- pytest : 전체 테스트
- pytest -v : 전체 테스트 + 테스트 이름 출력
- pytest -k "실행시키는 테스트 이름" : 실행 하고자 하는 테스트 만 실행 이름은 like로 걸린다.
- pytest -s : 콘솔 출력

### 저장시 자동 테스트

- poetry add pytest-watch -> 추가

#### 실행

- ptw : 전체 자동 실행
- ptw tests/ : tests 디렉토리 감시
- ptw -- -v : 테스트 메서드 출력
- ptw -- -k "메서드명": 감시하고자 하는 메서드 지정
- 예시
  - ptw tests/ -- -s -v -k test_A

## MOCK사용

### 설치

- poetry add pytest-mock

### DB Mock

```python
import unittest
from unittest.mock import MagicMock
from back.member import find_user_of_id
from back.database_model import Member

class TestMemberFunctions(unittest.TestCase):

    def setUp(self):
        self.db = MagicMock()  # Mocked database session

    def test_find_user_of_id_found(self):
        # Mocking the return value of the query
        mock_member = Member(id="testuser", name="Test User", password="hashed_password")
        self.db.query.return_value.filter.return_value.first.return_value = mock_member

        result = find_user_of_id("testuser", self.db)

        self.assertIsNotNone(result)
        self.assertEqual(result.id, "testuser")
        self.assertEqual(result.name, "Test User")

    def test_find_user_of_id_not_found(self):
        # Mocking the return value when no member is found
        self.db.query.return_value.filter.return_value.first.return_value = None

        result = find_user_of_id("nonexistentuser", self.db)

        self.assertIsNone(result)

if __name__ == "__main__":
    unittest.main()
```

### Method Mock

```python

@pytest.fixture
def test_apps():
    pass

def test_U_유저_중복_확인_withMock(test_apps, mocker):
    '''
    디비 저장 없이 중복 체크
    '''
    mock_testReturnValue = mocker.patch('back.member.testReturnValue', return_value={"mocked_key": "mocked_value"})

    # 테스트용 데이터베이스 세션 생성
    db = MagicMock() # Mock database

    # 테스트용 회원 데이터
    member_data = MemberCreate(
        id="newuser",
        name="New User",
        password1="newpassword",
        password2="newpassword",
        email="newuser@test.com"
    )
    # mocking return data
    mock_testReturnValue.return_value = member_data


    with pytest.raises(HTTPException) as exc_info:
        check_duple_id(member_data.id, db)

    print(exc_info)
    # HTTPException(status_code=400, detail="이미 등록되어 있는 ID")
    assert exc_info.value.status_code == 400, "상태코드는 400 이어야 한다."
    assert exc_info.value.detail == "이미 등록되어 있는 ID", "상세 메시지는 '이미 등록되어 있는 ID' 이어야 한다."

```

# 프론트

## 프로젝트 생성

- npx create-next-app@latest front
- √ Would you like to use TypeScript? ... No / Yes
- √ Would you like to use ESLint? ... No / Yes
- √ Would you like to use Tailwind CSS? ... No / Yes
- √ Would you like to use `src/` directory? ... No / Yes
- √ Would you like to use App Router? (recommended) ... No / Yes
- √ Would you like to customize the default import alias (@/\*)? ... No / Yes
- √ What import alias would you like configured? ... @/\*
- cd front
- code .
