# 셀레니움(Selenium) 사용 가이드

## 목차

- [셀레니움(Selenium) 사용 가이드](#셀레니움selenium-사용-가이드)
  - [목차](#목차)
  - [1. 파이썬 및 환경 설정](#1-파이썬-및-환경-설정)
    - [파이썬 설치](#파이썬-설치)
    - [가상환경 생성 및 활성화](#가상환경-생성-및-활성화)
    - [필요한 패키지 설치](#필요한-패키지-설치)
  - [2. 셀레니움 기본 설정](#2-셀레니움-기본-설정)
  - [3. 웹 요소 찾기](#3-웹-요소-찾기)
  - [4. 요소와 상호작용](#4-요소와-상호작용)
  - [5. 대기 기능](#5-대기-기능)
  - [6. 페이지 제어 및 탐색](#6-페이지-제어-및-탐색)
  - [7. 스크롤 기능](#7-스크롤-기능)
  - [8. 자바스크립트 실행](#8-자바스크립트-실행)
  - [9. 경고창(Alert) 처리](#9-경고창alert-처리)
  - [10. 프레임 및 창 전환](#10-프레임-및-창-전환)
  - [11. 브라우저 종료](#11-브라우저-종료)
  - [셀레니움 실전 예제](#셀레니움-실전-예제)
    - [로그인 자동화](#로그인-자동화)
    - [웹 스크레이핑 예제](#웹-스크레이핑-예제)

## 1. 파이썬 및 환경 설정

### 파이썬 설치

```bash
# 파이썬 버전 확인
python --version
```

### 가상환경 생성 및 활성화

```bash
# Windows
python -m venv selenium_env
selenium_env\Scripts\activate

# macOS/Linux
python3 -m venv selenium_env
source selenium_env/bin/activate
```

### 필요한 패키지 설치

```bash
pip install selenium
pip install webdriver-manager
```

## 2. 셀레니움 기본 설정

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# 브라우저 드라이버 설정
service = Service(ChromeDriverManager().install())
driver = webdriver.Chrome(service=service)

# 웹페이지 접속
driver.get("https://www.example.com")
```

## 3. 웹 요소 찾기

```python
# ID로 요소 찾기
element = driver.find_element(By.ID, "element_id")

# 클래스 이름으로 요소 찾기
elements = driver.find_elements(By.CLASS_NAME, "class_name")

# CSS 선택자로 요소 찾기
element = driver.find_element(By.CSS_SELECTOR, "div.class_name > p")

# XPath로 요소 찾기
element = driver.find_element(By.XPATH, "//div[@class='class_name']/p")

# 링크 텍스트로 요소 찾기
element = driver.find_element(By.LINK_TEXT, "Click here")

# 부분 링크 텍스트로 요소 찾기
element = driver.find_element(By.PARTIAL_LINK_TEXT, "Click")

# 태그 이름으로 요소 찾기
elements = driver.find_elements(By.TAG_NAME, "a")
```

## 4. 요소와 상호작용

```python
# 텍스트 입력
element.send_keys("텍스트 입력")

# 요소 클릭
element.click()

# 요소의 내용 가져오기
text = element.text

# 속성 값 가져오기
attribute = element.get_attribute("href")

# 폼 제출
form_element.submit()

# 텍스트 지우기
element.clear()

# 드롭다운 메뉴 선택
from selenium.webdriver.support.ui import Select
select = Select(driver.find_element(By.ID, "dropdown_id"))
select.select_by_visible_text("옵션 텍스트")
select.select_by_value("value")
select.select_by_index(1)
```

## 5. 대기 기능

```python
# 명시적 대기: 특정 조건 만족할 때까지 대기
element = WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.ID, "element_id"))
)

# 요소가 클릭 가능할 때까지 대기
element = WebDriverWait(driver, 10).until(
    EC.element_to_be_clickable((By.ID, "element_id"))
)

# 요소가 보일 때까지 대기
element = WebDriverWait(driver, 10).until(
    EC.visibility_of_element_located((By.ID, "element_id"))
)

# 암시적 대기: 모든 요소 검색에 적용
driver.implicitly_wait(10)  # 최대 10초 대기

# 페이지 로드 완료까지 대기
driver.set_page_load_timeout(30)
```

## 6. 페이지 제어 및 탐색

```python
# 이전 페이지로 이동
driver.back()

# 다음 페이지로 이동
driver.forward()

# 페이지 새로고침
driver.refresh()

# 현재 URL 가져오기
current_url = driver.current_url

# 페이지 제목 가져오기
title = driver.title

# 페이지 소스 가져오기
page_source = driver.page_source
```

## 7. 스크롤 기능

```python
# 페이지의 특정 요소로 스크롤
driver.execute_script("arguments[0].scrollIntoView();", element)

# 페이지 끝까지 스크롤
driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")

# 특정 위치로 스크롤
driver.execute_script("window.scrollTo(0, 500);")

# 무한 스크롤 페이지 처리 예시
last_height = driver.execute_script("return document.body.scrollHeight")
while True:
    # 페이지 끝까지 스크롤
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")

    # 페이지 로딩 대기
    import time
    time.sleep(2)

    # 새 스크롤 높이 계산
    new_height = driver.execute_script("return document.body.scrollHeight")
    if new_height == last_height:
        break
    last_height = new_height
```

## 8. 자바스크립트 실행

```python
# 자바스크립트 코드 실행
driver.execute_script("alert('Hello, Selenium!');")

# 요소 숨기기
driver.execute_script("arguments[0].style.display = 'none';", element)

# 요소 속성 변경
driver.execute_script("arguments[0].setAttribute('value', '새로운 값');", element)
```

## 9. 경고창(Alert) 처리

```python
# 경고창 수락
alert = driver.switch_to.alert
alert.accept()

# 경고창 거부
alert.dismiss()

# 경고창 텍스트 가져오기
alert_text = alert.text

# 프롬프트 경고창에 텍스트 입력
alert.send_keys("텍스트 입력")
```

## 10. 프레임 및 창 전환

```python
# 프레임으로 전환 (이름으로)
driver.switch_to.frame("frame_name")

# 프레임으로 전환 (인덱스로)
driver.switch_to.frame(0)

# 프레임으로 전환 (요소로)
frame_element = driver.find_element(By.ID, "frame_id")
driver.switch_to.frame(frame_element)

# 부모 프레임으로 돌아가기
driver.switch_to.parent_frame()

# 기본 컨텐츠로 돌아가기
driver.switch_to.default_content()

# 새 창 열기 및 전환
driver.execute_script("window.open('');")
tabs = driver.window_handles
driver.switch_to.window(tabs[1])  # 새 탭으로 전환

# 특정 창으로 전환
driver.switch_to.window("window_name")

# 모든 창 핸들 가져오기
all_windows = driver.window_handles
```

## 11. 브라우저 종료

```python
# 현재 탭 닫기
driver.close()

# 브라우저 완전히 종료
driver.quit()
```

## 셀레니움 실전 예제

### 로그인 자동화

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# 브라우저 설정
service = Service(ChromeDriverManager().install())
driver = webdriver.Chrome(service=service)

# 로그인 페이지 접속
driver.get("https://example.com/login")

# 사용자 이름 및 비밀번호 입력
username_field = driver.find_element(By.ID, "username")
password_field = driver.find_element(By.ID, "password")

username_field.send_keys("your_username")
password_field.send_keys("your_password")

# 로그인 버튼 클릭
login_button = driver.find_element(By.CSS_SELECTOR, "button[type='submit']")
login_button.click()

# 로그인 성공 확인
WebDriverWait(driver, 10).until(
    EC.presence_of_element_located((By.CLASS_NAME, "dashboard"))
)

print("로그인 성공!")
driver.quit()
```

### 웹 스크레이핑 예제

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By
import pandas as pd
import time

# 브라우저 설정
service = Service(ChromeDriverManager().install())
driver = webdriver.Chrome(service=service)

# 웹페이지 접속
driver.get("https://example.com/products")

# 데이터 저장을 위한 리스트
products = []

# 페이지 내의 모든 상품 정보 수집
product_elements = driver.find_elements(By.CLASS_NAME, "product-item")

for product in product_elements:
    try:
        name = product.find_element(By.CLASS_NAME, "product-name").text
        price = product.find_element(By.CLASS_NAME, "product-price").text
        rating = product.find_element(By.CLASS_NAME, "product-rating").text

        products.append({
            "name": name,
            "price": price,
            "rating": rating
        })
    except:
        continue

# 데이터프레임으로 변환
df = pd.DataFrame(products)

# CSV 파일로 저장
df.to_csv("products.csv", index=False)

print(f"{len(products)}개의 상품 정보가 수집되었습니다.")
driver.quit()
```
