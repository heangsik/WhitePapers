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
- pytest -v -k "실행시키는 테스트 이름" : 실행 하고자 하는 테스트 만 실행 이름은 like로 걸린다.

### 저장시 자동 테스트

- poetry add pytest-watch -> 추가

#### 실행

- ptw : 전체 자동 실행
- ptw tests/ : tests 디렉토리 감시
- ptw -- -v : 테스트 메서드 출력
- ptw -- -k "메서드명": 감시하고자 하는 메서드 지정

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
