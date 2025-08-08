<!doctype html>
<html lang="hi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Haryana GK Quiz - हरियाणा GK क्विज</title>
  <style>
    body { font-family: "Noto Sans", Arial, sans-serif; background:#f7f9fc; color:#111; padding:20px; }
    .container { max-width:760px; margin:20px auto; background:#fff; border-radius:8px; padding:22px; box-shadow:0 6px 18px rgba(20,20,50,0.08); }
    h1 { margin:0 0 10px 0; font-size:24px; }
    .meta { color:#666; margin-bottom:18px; }
    .question { font-weight:600; margin:16px 0; }
    .options { list-style:none; padding:0; }
    .options li { margin:8px 0; }
    button { background:#2563eb; color:#fff; border:0; padding:10px 14px; border-radius:6px; cursor:pointer; font-weight:600; }
    button[disabled]{ background:#99b5ff; cursor:default; }
    .result { margin-top:18px; padding:12px; border-radius:6px; background:#f1f8ff; color:#0b3b91; font-weight:600; }
    .small { font-size:13px; color:#666; margin-top:6px; }
    .answers { margin-top:14px; }
    .correct { color:green; }
    .wrong { color:red; }
    footer { text-align:center; margin-top:18px; color:#666; font-size:13px; }
  </style>
</head>
<body>
  <div class="container">
    <h1>हरियाणा GK Quiz</h1>
    <div class="meta">कुल प्रश्न: <span id="totalQ">0</span> · अभी का प्रश्न: <span id="curQ">0</span></div>

    <div id="quizArea">
      <div class="question" id="questionText">लोड कर रहे हैं...</div>
      <ul class="options" id="optionsList"></ul>

      <div style="margin-top:12px;">
        <button id="submitBtn">जवाब दें</button>
        <button id="nextBtn" style="margin-left:8px;">अगला प्रश्न</button>
      </div>

      <div id="feedback" class="small"></div>
    </div>

    <div id="finalResult" style="display:none;">
      <div class="result" id="scoreText"></div>
      <div class="answers" id="reviewAnswers"></div>
      <div style="margin-top:12px;">
        <button id="retryBtn">फिर से खेलें</button>
      </div>
    </div>

    <footer>स्रोत: सामान्य ज्ञान संग्रह • अभ्यास उद्देश्य के लिए</footer>
  </div>

<script>
  const questions = [
    {
      q: "हरियाणा की राजधानी कौन-सी है?",
      options: ["चंडीगढ़", "पंजाब", "रोहतक", "गुरुग्राम"],
      ans: 0
    },
    {
      q: "हरियाणा कब बनाया गया था (राज्य के रूप में)?",
      options: ["1 नवम्बर 1966", "15 अगस्त 1947", "26 जनवरी 1950", "1 अप्रैल 1965"],
      ans: 0
    },
    {
      q: "हरियाणा का राष्ट्रीय पक्षी कौन सा है?",
      options: ["मोर (Peacock)", "बत्तख", "कोयल", "घूँघट वाला चिड़िया"],
      ans: 0
    },
    {
      q: "हरियाणा का सबसे बड़ा शहर (आबादी के हिसाब से) कौन सा है?",
      options: ["फरीदाबाद", "गुरुग्राम", "रोहतक", "कैथल"],
      ans: 0
    },
    {
      q: "किस नदी को हरियाणा का प्रमुख नदी माना जाता है?",
      options: ["यमुना", "गंगा", "सरस्वती", "सतलज"],
      ans: 0
    },
    {
      q: "हरियाणा के कितने जिले हैं? (2024 के आसपास)",
      options: ["22", "21", "20", "19"],
      ans: 0
    },
    {
      q: "हरियाणा का लोकगीत या पारंपरिक नृत्य क्या है?",
      options: ["गरबा", "घूमर", "राइ", "भांगड़ा"],
      ans: 2
    },
    {
      q: "हरियाणा कौन-सा कृषि प्रधान राज्य माना जाता है?",
      options: ["हाँ", "नहीं", "कुछ हिस्सों में", "केवल पीठासीन क्षेत्रों में"],
      ans: 0
    },
    {
      q: "हरियाणा का प्रमुख फल किसे कहा जाता है?",
      options: ["सेब", "मूसमी", "आम", "अनार"],
      ans: 2
    },
    {
      q: "हरियाणा में स्थित मशहूर हिसार जिला किस लिए जाना जाता है?",
      options: ["कपड़ा उद्योग (textiles)", "मछली पालन", "ज्वेलरी", "सीमेंट"],
      ans: 0
    }
  ];

  let cur = 0, score = 0, answered = false;

  const totalQ = document.getElementById('totalQ');
  const curQ = document.getElementById('curQ');
  const questionText = document.getElementById('questionText');
  const optionsList = document.getElementById('optionsList');
  const submitBtn = document.getElementById('submitBtn');
  const nextBtn = document.getElementById('nextBtn');
  const feedback = document.getElementById('feedback');
  const finalResult = document.getElementById('finalResult');
  const quizArea = document.getElementById('quizArea');
  const scoreText = document.getElementById('scoreText');
  const reviewAnswers = document.getElementById('reviewAnswers');
  const retryBtn = document.getElementById('retryBtn');

  totalQ.textContent = questions.length;

  function loadQuestion() {
    answered = false;
    feedback.textContent = '';
    const q = questions[cur];
    curQ.textContent = cur + 1;
    questionText.textContent = q.q;
    optionsList.innerHTML = '';
    q.options.forEach((opt, idx) => {
      const li = document.createElement('li');
      li.innerHTML = `<label style="cursor:pointer;"><input type="radio" name="opt" value="${idx}"> ${opt}</label>`;
      optionsList.appendChild(li);
    });
    submitBtn.disabled = false;
    nextBtn.disabled = true;
  }

  submitBtn.addEventListener('click', () => {
    if (answered) return;
    const sel = document.querySelector('input[name="opt"]:checked');
    if (!sel) { feedback.textContent = 'कृपया एक विकल्प चुनें।'; return; }
    const chosen = parseInt(sel.value, 10);
    const correct = questions[cur].ans;
    answered = true;
    submitBtn.disabled = true;
    nextBtn.disabled = false;
    if (chosen === correct) {
      score += 1;
      feedback.innerHTML = `<span class="correct">सही उत्तर ✅</span>`;
    } else {
      feedback.innerHTML = `<span class="wrong">गलत ❌</span> सही उत्तर: <strong>${questions[cur].options[correct]}</strong>`;
    }
  });

  nextBtn.addEventListener('click', () => {
    cur++;
    if (cur >= questions.length) {
      showResult();
    } else {
      loadQuestion();
    }
  });

  function showResult() {
    quizArea.style.display = 'none';
    finalResult.style.display = 'block';
    scoreText.textContent = `आपका स्कोर: ${score} / ${questions.length}`;
    reviewAnswers.innerHTML = '';
    questions.forEach((q, i) => {
      const div = document.createElement('div');
      const correct = q.ans;
      div.innerHTML = `<strong>Q${i+1}.</strong> ${q.q} <br> सही उत्तर: <span class="correct">${q.options[correct]}</span>`;
      reviewAnswers.appendChild(div);
    });
  }

  retryBtn.addEventListener('click', () => {
    cur = 0; score = 0;
    quizArea.style.display = 'block';
    finalResult.style.display = 'none';
    loadQuestion();
  });

  // Init
  loadQuestion();
</script>
</body>
</html>
