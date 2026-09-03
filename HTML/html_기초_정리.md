# HTML 기초 정리

## 1. HTML이란?

**HTML(HyperText Markup Language)**은 웹 페이지의 **구조와 내용**을 만드는 마크업 언어입니다.

---

# 2. HTML의 기본 구조

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>웹 페이지 제목</title>
</head>
<body>
    <h1>안녕하세요</h1>
    <p>HTML을 공부합니다.</p>
</body>
</html>
```

---

# 3. 가장 기본적인 용어

## 태그 (Tag)

HTML에서 특정 기능이나 의미를 표시하는 표시입니다.

예시:

```html
<p>
<h1>
<a>
```

`<p>`는 문단을 의미하는 태그입니다.

---

## 요소 (Element)

일반적으로 **시작 태그 + 내용 + 종료 태그** 전체를 요소라고 합니다.

```html
<p>안녕하세요</p>
```

위 전체가 하나의 `p 요소`입니다.

---

## 속성 (Attribute)

태그에 추가적인 정보를 제공하는 것입니다.

```html
<a href="https://example.com">링크</a>
```

여기서 `href`가 속성입니다.

---

## 속성값 (Attribute Value)

속성에 실제로 설정하는 값입니다.

```html
<a href="https://example.com">
```

여기서 `"https://example.com"`이 속성값입니다.

---

# 4. 문서 구조 관련 태그

## `<head>`

웹 페이지의 설정과 정보를 작성하는 영역입니다.

화면에 직접 표시되는 내용은 일반적으로 넣지 않습니다.

```html
<head>
    <title>웹 페이지</title>
</head>
```

주요 정보:

- 웹 페이지 제목
- 문자 인코딩
- CSS 연결
- 검색 엔진 관련 정보

---

## `<body>`

실제로 사용자 화면에 표시되는 내용을 작성합니다.

```html
<body>
    <h1>제목</h1>
    <p>내용</p>
</body>
```

---

# 5. 제목과 문단

## `<h1>` ~ `<h6>`

제목을 만들 때 사용합니다.

```html
<h1>가장 중요한 제목</h1>
<h2>두 번째 제목</h2>
<h3>세 번째 제목</h3>
```

- `h1` → 가장 중요한 제목
- `h6` → 가장 낮은 수준의 제목

숫자가 작을수록 문서에서 중요도가 높습니다.

---

## `<p>`

문단을 작성할 때 사용합니다.

`p`는 **Paragraph(문단)**의 약자입니다.

```html
<p>HTML을 공부합니다.</p>
```

---

## `<br>`

줄바꿈을 할 때 사용합니다.

```html
안녕하세요<br>
반갑습니다
```

---

## `<hr>`

내용을 구분할 때 사용합니다.

```html
<hr>
```

`hr`은 **Horizontal Rule**에서 나온 이름입니다.

---

# 6. 텍스트 관련 태그

## `<strong>`

내용이 중요하다는 의미를 표현합니다.

```html
<strong>중요한 내용</strong>
```

일반적으로 굵게 표시됩니다.

---

## `<b>`

글자를 굵게 표시합니다.

```html
<b>굵은 글씨</b>
```

`strong`과 달리 의미적인 중요성을 나타내는 목적은 아닙니다.

---

## `<em>`

내용을 강조할 때 사용합니다.

```html
<em>강조할 내용</em>
```

일반적으로 기울어진 글자로 표시됩니다.

---

## `<i>`

글자를 기울여 표시합니다.

```html
<i>기울임 글씨</i>
```

---

# 7. 링크

## `<a>`

다른 페이지나 웹사이트로 연결하는 링크를 만듭니다.

```html
<a href="https://example.com">사이트 방문</a>
```

`a`는 **Anchor(앵커)**에서 나온 이름입니다.

---

## `href`

링크가 이동할 주소를 지정하는 속성입니다.

```html
<a href="주소">
```

`href`는 **Hypertext Reference**와 관련된 표현입니다.

---

## `target="_blank"`

링크를 새로운 탭에서 열도록 설정합니다.

```html
<a href="https://example.com" target="_blank">
    새 탭에서 열기
</a>
```

---

# 8. 이미지

## `<img>`

웹 페이지에 이미지를 표시합니다.

```html
<img src="image.jpg" alt="이미지 설명">
```

---

## `src`

이미지 파일의 위치를 지정합니다.

`src`는 **Source(출처 또는 원본 위치)**의 약자입니다.

```html
<img src="image.jpg">
```

---

## `alt`

이미지를 설명하는 대체 텍스트입니다.

`alt`는 **Alternative Text**의 약자입니다.

```html
<img src="image.jpg" alt="고양이 사진">
```

이미지가 표시되지 않을 때나 스크린 리더를 사용할 때 중요합니다.

---

# 9. 목록

## `<ul>`

순서가 없는 목록입니다.

`ul`은 **Unordered List**의 약자입니다.

```html
<ul>
    <li>사과</li>
    <li>바나나</li>
</ul>
```

---

## `<ol>`

순서가 있는 목록입니다.

`ol`은 **Ordered List**의 약자입니다.

```html
<ol>
    <li>첫 번째</li>
    <li>두 번째</li>
</ol>
```

---

## `<li>`

목록의 각 항목입니다.

`li`는 **List Item**의 약자입니다.

```html
<li>사과</li>
```

---

# 10. 영역을 나누는 태그

## `<div>`

여러 HTML 요소를 하나의 영역으로 묶을 때 사용합니다.

```html
<div>
    <h2>제목</h2>
    <p>내용</p>
</div>
```

주로 여러 요소를 그룹화할 때 사용합니다.

---

## `<span>`

문장 내부의 일부 내용을 묶을 때 주로 사용합니다.

```html
<p>
    이것은 <span>중요한 내용</span>입니다.
</p>
```

---

# 11. 블록 요소와 인라인 요소

## 블록 요소 (Block Element)

일반적으로 새로운 줄에서 시작하며 하나의 영역을 크게 차지합니다.

대표적인 예:

- `div`
- `p`
- `h1`
- `section`

---

## 인라인 요소 (Inline Element)

문장 흐름 안에서 필요한 공간만 차지합니다.

대표적인 예:

- `span`
- `a`
- `strong`
- `em`

---

# 12. 시맨틱 태그

**Semantic(시맨틱)**은 '의미를 가진'이라는 뜻입니다.

HTML 코드에서 각 영역의 역할을 명확하게 표현하는 태그입니다.

---

## `<header>`

페이지나 특정 영역의 머리말입니다.

```html
<header>
    <h1>웹사이트 제목</h1>
</header>
```

---

## `<nav>`

메뉴나 페이지 이동을 위한 링크 영역입니다.

```html
<nav>
    <a href="#">홈</a>
    <a href="#">소개</a>
</nav>
```

`nav`는 **Navigation(탐색, 이동)**의 줄임말입니다.

---

## `<main>`

웹 페이지의 핵심 내용을 나타냅니다.

```html
<main>
    주요 콘텐츠
</main>
```

---

## `<section>`

관련된 콘텐츠를 하나의 구역으로 나눌 때 사용합니다.

```html
<section>
    <h2>소개</h2>
    <p>내용</p>
</section>
```

---

## `<article>`

독립적으로 사용할 수 있는 콘텐츠를 나타냅니다.

예:

- 뉴스 기사
- 블로그 글
- 게시물

```html
<article>
    <h2>게시글 제목</h2>
    <p>게시글 내용</p>
</article>
```

---

## `<aside>`

주요 내용과 관련은 있지만 부가적인 정보를 나타냅니다.

예:

- 광고
- 사이드바
- 관련 링크

---

## `<footer>`

페이지나 특정 영역의 하단 정보를 나타냅니다.

```html
<footer>
    저작권 정보
</footer>
```

---

# 13. 표

## `<table>`

표 전체를 만듭니다.

```html
<table>
</table>
```

---

## `<tr>`

표의 한 행을 의미합니다.

`tr`은 **Table Row**의 약자입니다.

---

## `<th>`

표의 제목 셀입니다.

`th`은 **Table Header**의 약자입니다.

---

## `<td>`

일반적인 데이터 셀입니다.

`td`는 **Table Data**의 약자입니다.

예시:

```html
<table>
    <tr>
        <th>이름</th>
        <th>나이</th>
    </tr>

    <tr>
        <td>홍길동</td>
        <td>20</td>
    </tr>
</table>
```

---

# 14. 입력 양식(Form)

## `<form>`

사용자로부터 입력받은 정보를 서버로 전달하기 위한 양식입니다.

```html
<form>
</form>
```

---

## `<input>`

사용자가 데이터를 입력하는 요소입니다.

```html
<input type="text">
```

---

## `type`

입력창의 종류를 지정합니다.

### 자주 사용하는 type

```html
<input type="text">
```

일반적인 텍스트 입력

```html
<input type="password">
```

비밀번호 입력

```html
<input type="email">
```

이메일 입력

```html
<input type="number">
```

숫자 입력

```html
<input type="date">
```

날짜 선택

```html
<input type="checkbox">
```

여러 항목 선택

```html
<input type="radio">
```

하나의 항목 선택

```html
<input type="file">
```

파일 선택

---

## `<label>`

입력 요소가 무엇을 의미하는지 설명하는 태그입니다.

```html
<label>이름</label>
<input type="text">
```

---

## `<button>`

사용자가 클릭할 수 있는 버튼입니다.

```html
<button>확인</button>
```

---

## `submit`

폼에 입력한 데이터를 제출하는 기능입니다.

```html
<button type="submit">전송</button>
```

---

# 15. id와 class

## `id`

특정 HTML 요소를 고유하게 식별하는 이름입니다.

```html
<p id="title">제목</p>
```

일반적으로 하나의 페이지에서 같은 `id`를 여러 번 사용하지 않습니다.

---

## `class`

여러 요소를 같은 그룹으로 묶을 때 사용합니다.

```html
<p class="text">내용1</p>
<p class="text">내용2</p>
```

CSS나 JavaScript에서 같은 종류의 요소를 선택할 때 많이 사용합니다.

---

# 16. 주석

HTML 코드에 설명을 작성할 때 사용합니다.

```html
<!-- 여기는 주석입니다 -->
```

주석은 일반적으로 웹 페이지 화면에 표시되지 않습니다.

---

# 17. 경로 (Path)

**Path(경로)**는 파일이 위치한 장소를 나타냅니다.

## 상대 경로

현재 파일을 기준으로 다른 파일의 위치를 표시합니다.

```html
<img src="images/photo.jpg">
```

---

## 절대 경로

전체 주소를 사용합니다.

```html
<a href="https://example.com/page.html">
    링크
</a>
```

---

# 18. HTML 핵심 용어 

| 용어                뜻 
|------------------|------------------------------
| HTML             | 웹 페이지의 구조를 만드는 언어    
| Tag              | HTML 기능을 나타내는 표시         
| Element          | 태그와 내용을 포함하는 구성 단위  
| Attribute        | 태그에 추가 정보를 제공하는 것    
| Attribute Value  | 속성에 설정하는 실제 값           
| Head             | 문서 설정과 정보를 담는 영역      
| Body             | 화면에 표시되는 내용을 담는 영역  
| Semantic         | 콘텐츠의 의미를 명확하게 표현     
| Block            | 새로운 줄에서 영역을 차지하는 요소
| Inline           | 문장 흐름 안에서 표시되는 요소    
| Hyperlink        | 다른 페이지로 이동하는 연결       
| Path             | 파일 또는 주소의 위치             
| Form             | 사용자 입력을 받는 양식           

---

# 19. 가장 많이 사용하는 태그 요약

```text
<html>      HTML 문서 전체
<head>      문서 설정
<body>      화면에 표시되는 내용

<h1>~<h6>  제목
<p>         문단
<br>        줄바꿈
<hr>        내용 구분

<a>         링크
<img>       이미지

<ul>        순서 없는 목록
<ol>        순서 있는 목록
<li>        목록 항목

<div>       블록 영역
<span>      인라인 영역

<header>    머리말
<nav>       메뉴
<main>      주요 내용
<section>   콘텐츠 구역
<article>   독립적인 콘텐츠
<aside>     부가 콘텐츠
<footer>    바닥글

<table>     표
<tr>        표의 행
<th>        표 제목
<td>        표 데이터

<form>      입력 양식
<input>     입력창
<label>     입력 설명
<button>    버튼
```

---


# 마지막 정리

HTML은 웹 페이지의 **구조를 만드는 언어**

핵심 개념

- **태그(Tag)**: HTML의 기본 표시
- **요소(Element)**: 태그와 내용을 포함한 구성 단위
- **속성(Attribute)**: 태그에 추가 정보를 제공
- **head**: 웹 페이지의 설정
- **body**: 실제 화면에 표시되는 내용
- **시맨틱 태그**: 콘텐츠의 의미를 명확하게 표현
- **CSS**: 디자인 담당
- **JavaScript**: 동작과 기능 담당
