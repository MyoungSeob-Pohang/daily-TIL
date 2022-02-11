# Open Graph

Open Graph는 쉽게 설명하면 head안에 들어가는 meta태그들이다.
카카오톡 같은 곳에 url을 공유할 때 보여지는 이미지나 설명같은 태그들이다.

```
<!DOCTYPE html>
<html lang="ko">

<head>
  <meta charset="utf-8" />
  <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="theme-color" content="#000000" />
  <meta name="description" content="나만의 감정 일기장" />
  <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
  <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
  <meta property="'og:image" content="%PUBLIC_URL%/thumbnail.png" />
  <meta property="'og:site_name" content="감정 일기장" />
  <meta property="'og:description" content="나만의 작은 감정 일기장" />

  <title>감정 일기장</title>
</head>

<body>
  <noscript>You need to enable JavaScript to run this app.</noscript>
  <div id="root"></div>
</body>

</html>
```
