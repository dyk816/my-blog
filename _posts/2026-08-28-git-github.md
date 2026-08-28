---
layout: null
title: "어제 배운 Git 정리"
date: 2026-08-28
categories: [부트캠프]
---

<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>어제 배운 Git 정리</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      min-height: 100vh;
      font-family: 'Gulim', '굴림', 'Comic Sans MS', sans-serif;
      background: linear-gradient(
        to bottom,
        #ff0000 0%, #ff0000 14.28%,
        #ff7f00 14.28%, #ff7f00 28.56%,
        #ffff00 28.56%, #ffff00 42.84%,
        #00ff00 42.84%, #00ff00 57.12%,
        #00ffff 57.12%, #00ffff 71.4%,
        #0000ff 71.4%, #0000ff 85.68%,
        #8b00ff 85.68%, #8b00ff 100%
      );
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 30px 20px;
    }

    /* 상단 홈으로 돌아가기 버튼 */
    .home-btn {
      align-self: flex-start;
      background: #ffeb3b;
      border: 3px solid #000;
      box-shadow: 4px 4px 0px #000;
      color: #000;
      text-decoration: none;
      font-weight: bold;
      padding: 8px 16px;
      margin-bottom: 20px;
      border-radius: 5px;
    }

    .home-btn:hover {
      background: #ff00ff;
      color: #fff;
    }

    /* 메인 제목 상자 */
    .title-box {
      background-color: #fffb8f;
      border: 6px solid #0000ff;
      box-shadow: 8px 8px 0px #ff00ff;
      padding: 15px 40px;
      text-align: center;
      margin-bottom: 30px;
    }

    .title-box h1 {
      font-size: 2.3rem;
      color: #ff0000;
      text-shadow: 2px 2px 0px #000;
      letter-spacing: 2px;
    }

    .title-box .post-date {
      margin-top: 8px;
      font-size: 0.95rem;
      color: #0000cc;
      font-weight: bold;
    }

    /* 포스트 본문 카드 */
    .post-container {
      width: 100%;
      max-width: 800px;
      background: rgba(255, 255, 255, 0.96);
      border: 5px dashed #ff0080;
      border-radius: 15px;
      padding: 30px 25px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      line-height: 1.7;
      color: #111;
    }

    .post-container h2 {
      color: #008000;
      font-size: 1.5rem;
      border-bottom: 3px solid #ffcc00;
      padding-bottom: 6px;
      margin-top: 25px;
      margin-bottom: 12px;
    }

    .post-container h2:first-of-type {
      margin-top: 0;
    }

    .post-container h3 {
      color: #0044ff;
      font-size: 1.2rem;
      margin-top: 18px;
      margin-bottom: 8px;
    }

    .post-container p {
      margin-bottom: 12px;
      font-size: 1.05rem;
    }

    .post-container strong {
      background-color: #ffff77;
      color: #d13030;
      padding: 0 4px;
    }

    /* 알록달록 표 스타일 */
    .post-container table {
      width: 100%;
      border-collapse: collapse;
      margin: 15px 0;
      background: #fff;
    }

    .post-container th, .post-container td {
      border: 2px solid #000;
      padding: 10px;
      text-align: center;
      font-size: 0.95rem;
    }

    .post-container th {
      background-color: #ffcc00;
      color: #000;
      font-weight: bold;
    }

    .post-container code {
      background: #eee;
      border: 1px solid #ccc;
      padding: 2px 6px;
      border-radius: 4px;
      color: #c7254e;
      font-family: monospace;
      font-size: 0.95rem;
    }

    /* 리스트 스타일 */
    .post-container ul {
      list-style-type: '👉 ';
      padding-left: 20px;
      margin-bottom: 15px;
    }

    .post-container ul li {
      margin-bottom: 6px;
      font-size: 1.05rem;
    }
  </style>
</head>
<body>

  <!-- 메인 홈으로 돌아가기 -->
  <a href="{{ '/' | relative_url }}" class="home-btn">⬅ 메인 화면으로 가기</a>

  <!-- 제목 네모 상자 -->
  <div class="title-box">
    <h1>어제 배운 Git 정리</h1>
    <div class="post-date">작성일: 2026-08-28 | 분류: 부트캠프</div>
  </div>

  <!-- 포스트 본문 -->
  <div class="post-container">
    <h2>🔥 오늘 배운 것</h2>
    <p>Git은 <strong>저장 지점을 남기고 되돌릴 수 있게</strong> 해주는 도구다.</p>

    <h3>커밋까지의 세 단계</h3>
    <table>
      <thead>
        <tr>
          <th>단계</th>
          <th>명령</th>
          <th>하는 일</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>1</td>
          <td><code>git add</code></td>
          <td>커밋에 담을 것을 고른다</td>
        </tr>
        <tr>
          <td>2</td>
          <td><code>git commit</code></td>
          <td>고른 것을 한 덩어리로 기록한다</td>
        </tr>
        <tr>
          <td>3</td>
          <td><code>git push</code></td>
          <td>기록을 GitHub로 올린다</td>
        </tr>
      </tbody>
    </table>

    <p>가장 헷갈렸던 건 <code>add</code>와 <code>commit</code>이 왜 나뉘어 있는가였다.</p>
    <p>파일을 두 개 고쳤는데 <strong>하나만 커밋하고 싶을 때</strong>가 있기 때문이었다.</p>

    <h2>🚨 막혔던 것</h2>
    <p><code>git log</code>를 쳤더니 화면이 멈춘 것처럼 보였다.</p>
    <p>알고 보니 기록을 보여주는 화면이 열린 것이었고, <code>q</code>를 누르면 빠져나온다.</p>

    <h2>🎯 내일 볼 것</h2>
    <ul>
      <li>마크다운으로 글 쓰기</li>
      <li>만든 것을 인터넷에 올리기</li>
    </ul>
  </div>

</body>
</html>