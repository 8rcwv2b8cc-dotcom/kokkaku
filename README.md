<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0"> <!-- スマホ対応 -->
  <title>骨格診断</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Kosugi+Maru&display=swap');

    body {
      font-family: 'Kosugi Maru', sans-serif;
      background: linear-gradient(120deg, #ffe4ec, #ffd6e0); /* ピンクグラデ */
      max-width: 600px;
      margin: 20px auto;
      padding: 15px;
      border-radius: 20px;
      box-shadow: 0 0 25px rgba(0,0,0,0.1);
      color: #333;
      overflow: hidden;
      position: relative;
    }

    /* 背景ハート */
    body::before {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-image: url('https://i.ibb.co/FzJ6dSn/heart-pattern.png');
      background-repeat: repeat;
      background-size: 40px 40px;
      opacity: 0.15;
      pointer-events: none;
      z-index: 0;
    }

    h1 {
      text-align: center;
      color: #ff1493;
      font-size: 2em;
      margin-bottom: 25px;
      position: relative;
      z-index: 1;
    }

    .question {
      margin-bottom: 20px;
      position: relative;
      z-index: 1;
    }

    button {
      margin: 5px;
      padding: 12px 20px;
      border-radius: 30px;
      border: none;
      background-color: #ff69b4;
      color: white;
      font-weight: bold;
      cursor: pointer;
      box-shadow: 0 3px 6px rgba(0,0,0,0.1);
      transition: 0.3s;
      position: relative;
      z-index: 1;
      overflow: visible;
      font-size: 1em;
    }

    button:hover {
      background-color: #ff1493;
    }

    /* ハートアニメーション */
    .heart {
      position: absolute;
      font-size: 24px;
      color: #ff1493;
      animation: pop 0.6s ease-out forwards;
      pointer-events: none;
    }

    @keyframes pop {
      0% { transform: scale(0); opacity: 1; }
      50% { transform: scale(1.5); opacity: 1; }
      100% { transform: scale(1); opacity: 0; }
    }

    #result {
      font-weight: bold;
      margin-top: 25px;
      padding: 20px;
      border-radius: 20px;
      background-color: #fff0f5;
      box-shadow: 0 0 15px rgba(0,0,0,0.1);
      text-align: center;
      position: relative;
      z-index: 1;
    }

    p {
      line-height: 1.5;
      font-size: 1.1em;
    }

    @media screen and (max-width: 400px) {
      button {
        padding: 10px 15px;
        font-size: 0.95em;
      }
      h1 {
        font-size: 1.8em;
      }
    }
  </style>
</head>
<body>
  <h1>骨格診断</h1>
  <div id="quiz"></div>
  <div id="result"></div>

  <script>
    const questions = [
      { q: "肩幅は？", a: ["広い → ストレート", "普通 → ナチュラル", "狭い → ウェーブ"] },
      { q: "手首は？", a: ["太い → ストレート", "普通 → ナチュラル", "細い → ウェーブ"] },
      { q: "腰の位置は？", a: ["高い → ウェーブ", "普通 → ストレート", "低い → ナチュラル"] },
      { q: "体全体の印象は？", a: ["直線的 → ストレート", "普通 → ナチュラル", "曲線的 → ウェーブ"] },
      { q: "手の指は？", a: ["関節しっかり → ストレート", "普通 → ナチュラル", "華奢 → ウェーブ"] },
      { q: "鎖骨は？", a: ["目立つ → ウェーブ", "普通 → ナチュラル", "あまり目立たない → ストレート"] },
      { q: "体の厚みは？", a: ["厚い → ストレート", "普通 → ナチュラル", "薄い → ウェーブ"] },
      { q: "首の長さは？", a: ["短い → ストレート", "普通 → ナチュラル", "長い → ウェーブ"] },
      { q: "足首は？", a: ["しっかり → ストレート", "普通 → ナチュラル", "華奢 → ウェーブ"] },
      { q: "体全体の印象は？", a: ["直線的 → ストレート", "普通 → ナチュラル", "骨格しっかり → ナチュラル"] }
    ];

    let scores = { "ストレート":0, "ウェーブ":0, "ナチュラル":0 };
    let current = 0;

    function showQuestion() {
      if (current >= questions.length) {
        showResult();
        return;
      }
      const q = questions[current];
      let html = `<div class="question"><p>${q.q}</p>`;
      q.a.forEach((ans)=>{
        html += `<button onclick="answer('${ans.split(' → ')[1]}', this)">${ans.split(' → ')[0]}</button>`;
      });
      html += `</div>`;
      document.getElementById("quiz").innerHTML = html;
    }

    function answer(type, btn){
      // ボタン中央にハート出す
      const heart = document.createElement('span');
      heart.className = 'heart';
      heart.innerHTML = '❤';
      heart.style.left = (btn.offsetWidth/2 - 12) + 'px';
      heart.style.top = '-30px';
      btn.appendChild(heart);

      scores[type]++;
      setTimeout(() => {
        heart.remove();
        current++;
        showQuestion();
      }, 500);
    }

    function showResult(){
      let maxType = Object.keys(scores).reduce((a,b)=> scores[a]>scores[b]?a:b);
      let desc = "";
      if(maxType==="ストレート") desc="骨格ストレート。肩幅しっかりで直線的な印象。シンプルな服やジャストサイズの服が似合います。";
      if(maxType==="ウェーブ") desc="骨格ウェーブ。華奢で曲線的な印象。柔らかい素材やフリル、スカートが似合います。";
      if(maxType==="ナチュラル") desc="骨格ナチュラル。骨格が大きめでラフな印象。カジュアルやオーバーサイズ、ラフコーデがぴったり。";

      document.getElementById("quiz").innerHTML = "";
      document.getElementById("result").innerHTML = `<h2>あなたの骨格タイプ：${maxType}</h2><p>${desc}</p>`;
    }

    showQuestion();
  </script>
</body>
</html>
