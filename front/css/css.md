# CSS

## SCSS 설정

### 설치

> npm install -D sass

### 사용

```css
<style lang="scss">
  header {
    text-align: center;
    padding: 25px;
    margin-bottom: 30px;
    .logo {
      margin-bottom: 30px;
    }
    nav {
      ul {
        list-style: none;
        li {
          display: inline-block;
          margin: 0 1em;
          a {
            text-decoration: none;
            color: #333;
            &:hover {
              opacity: 0.7;
            }
          }
        }
      }
    }
  }
</style>
```

## TIPS

### 하단에 베너 고정 시키기

```html
<footer>
  <small>Copyright B-Boy Brother. All Right reserved</small>
</footer>

<style lang="scss">
  footer {
    background-color: #444; // 배경을 검은색으로 한다.
    color: #fff; // 폰트 색은 흰색
    text-align: center; // 문자열을 가로 기준으로 가운데
    align-items: center; // 문자열을 세로 기준으로 가운데
    padding: 1rem; // 문자열에 페딩을 준다.
    position: fixed; // 아래에 고정 시킨다.
    bottom: 0; // 고정시키는 위치
    width: 100%; // 넓이
  }
</style>
```

### 화명 중앙에 배치 하기

```html
<body>
  <h2>테스트 문구</h2>
</body>
<style>
  body {
    display: grid;
    place-items: center;
    height: 100vh;
  }

  or body {
    display: grid;
    align-items: center;
    justify-items: center;
  }
</style>
```

- 둘다 같은 결과가 나온다.
