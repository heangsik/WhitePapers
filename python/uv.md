# UV

## 설치

1. powershell 관리자 모드 실행
2. powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

## 프로젝트 셋팅

1. uv 초기화
   - uv init proc_01
   - cd proc_01
   - uv venv

## package설치

## 추가 명령어

### requirements.txt생성

- uv export -o requirements.txt

### 의존성 추가

- uv add package_name

### 의존성 삭제

- uv remove package_name

### 실행

- uv run hellp.py
