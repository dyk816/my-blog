---
icon: fas fa-star
order: 5
---

<style>
  #fortune-container {
    max-width: 600px;
    margin: 2rem auto;
    text-align: center;
  }
  
  .fortune-card {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    border-radius: 12px;
    padding: 2rem;
    color: white;
    margin: 1rem 0;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  }
  
  .card-emoji {
    font-size: 4rem;
    margin: 1rem 0;
  }
  
  .card-name {
    font-size: 1.8rem;
    font-weight: bold;
    margin: 1rem 0;
  }
  
  .card-meaning {
    font-size: 1rem;
    line-height: 1.6;
    opacity: 0.95;
  }
  
  #draw-button {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    border: none;
    padding: 0.75rem 2rem;
    font-size: 1rem;
    border-radius: 8px;
    cursor: pointer;
    transition: transform 0.2s, box-shadow 0.2s;
    margin: 1rem 0;
  }
  
  #draw-button:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
  }
  
  #draw-button:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }
  
  .date-info {
    font-size: 0.9rem;
    opacity: 0.8;
    margin-top: 1rem;
  }
  
  .reset-note {
    font-size: 0.85rem;
    color: #667eea;
    margin-top: 1rem;
  }
</style>

<div id="fortune-container">
  <h2>오늘의 운세 🔮</h2>
  <p>오늘의 타로 운세를 봐보세요. 한 번 점을 보면 그날 결과는 고정됩니다.</p>
  
  <div id="result-container" style="display: none;">
    <div class="fortune-card">
      <div class="card-emoji" id="card-emoji"></div>
      <div class="card-name" id="card-name"></div>
      <div class="card-meaning" id="card-meaning"></div>
    </div>
    <div class="date-info" id="date-info"></div>
    <button id="draw-button" disabled>오늘의 운세는 이미 정해졌습니다</button>
    <div class="reset-note">자정이 지나면 새로운 운세를 볼 수 있습니다</div>
  </div>
  
  <div id="initial-container">
    <button id="draw-button">오늘의 운세 보기</button>
  </div>
</div>

<script>
  (function () {
    const TAROT_CARDS = [
      { name: 'The Fool', emoji: '🤡', meaning: '새로운 시작, 모험, 용기' },
      { name: 'The Magician', emoji: '🎩', meaning: '창의성, 재능, 자신감' },
      { name: 'The High Priestess', emoji: '👑', meaning: '직관, 신비, 내면의 목소리' },
      { name: 'The Empress', emoji: '👸', meaning: '풍요로움, 창조성, 아름다움' },
      { name: 'The Emperor', emoji: '🤴', meaning: '권력, 리더십, 안정성' },
      { name: 'The Hierophant', emoji: '🙏', meaning: '전통, 지혜, 영성' },
      { name: 'The Lovers', emoji: '💕', meaning: '사랑, 조화, 선택' },
      { name: 'The Chariot', emoji: '🏎️', meaning: '승리, 결단력, 진행' },
      { name: 'Strength', emoji: '💪', meaning: '내적 강함, 인내, 용감함' },
      { name: 'The Hermit', emoji: '🕯️', meaning: '성찰, 고독, 내면 탐색' },
      { name: 'The Wheel of Fortune', emoji: '🎡', meaning: '운명, 변화, 순환' },
      { name: 'Justice', emoji: '⚖️', meaning: '공정성, 진실, 균형' },
      { name: 'The Hanged Man', emoji: '🔄', meaning: '포기, 새로운 관점, 휴식' },
      { name: 'Death', emoji: '🦋', meaning: '변화, 재생, 새로운 시작' },
      { name: 'Temperance', emoji: '🌊', meaning: '조절, 균형, 조화' },
      { name: 'The Devil', emoji: '😈', meaning: '매혹, 제약, 검토 필요' },
      { name: 'The Tower', emoji: '🌩️', meaning: '급변, 혼란, 새로운 기초' },
      { name: 'The Star', emoji: '⭐', meaning: '희망, 영감, 밝은 미래' },
      { name: 'The Moon', emoji: '🌙', meaning: '직관, 불안감, 환상' },
      { name: 'The Sun', emoji: '☀️', meaning: '행복, 성공, 긍정적 에너지' },
      { name: 'Judgement', emoji: '📯', meaning: '각성, 새로운 시대, 회복' },
      { name: 'The World', emoji: '🌍', meaning: '완성, 성취, 새로운 시작' }
    ];

    function getTodayKey() {
      const kst = new Date(Date.now() + 9 * 60 * 60 * 1000);
      return 'fortune_' + kst.toISOString().slice(0, 10);
    }

    function displayCard(card, savedDate) {
      const resultContainer = document.getElementById('result-container');
      const initialContainer = document.getElementById('initial-container');
      const drawButton = document.getElementById('draw-button');
      
      document.getElementById('card-emoji').textContent = card.emoji;
      document.getElementById('card-name').textContent = card.name;
      document.getElementById('card-meaning').textContent = card.meaning;
      document.getElementById('date-info').textContent = '📅 ' + savedDate;
      
      resultContainer.style.display = 'block';
      initialContainer.style.display = 'none';
      drawButton.disabled = true;
    }

    function drawCard() {
      const key = getTodayKey();
      const saved = localStorage.getItem(key);
      
      if (saved) {
        const data = JSON.parse(saved);
        displayCard(data.card, data.date);
      } else {
        const card = TAROT_CARDS[Math.floor(Math.random() * TAROT_CARDS.length)];
        const kst = new Date(Date.now() + 9 * 60 * 60 * 1000);
        const dateStr = kst.toLocaleDateString('ko-KR', { year: 'numeric', month: 'long', day: 'numeric' });
        
        localStorage.setItem(key, JSON.stringify({ card: card, date: dateStr }));
        displayCard(card, dateStr);
      }
    }

    document.getElementById('draw-button').addEventListener('click', drawCard);

    const key = getTodayKey();
    if (localStorage.getItem(key)) {
      drawCard();
    }
  })();
</script>
