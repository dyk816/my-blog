---
icon: fas fa-star
order: 5
---

<style>
  #fortune-container {
    max-width: 700px;
    margin: 2rem auto;
    text-align: center;
    padding: 0 1rem;
  }
  
  #card-selection {
    display: flex;
    justify-content: center;
    gap: 1.5rem;
    margin: 2rem 0;
    flex-wrap: wrap;
  }
  
  .card-back {
    width: 100px;
    height: 140px;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    border: 2px solid #5a67d8;
    border-radius: 8px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    transition: all 0.3s ease;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    position: relative;
    perspective: 1000px;
  }
  
  .card-back:hover {
    transform: translateY(-5px) scale(1.05);
    box-shadow: 0 6px 16px rgba(102, 126, 234, 0.4);
  }
  
  .card-back.selected {
    animation: flipCard 0.6s ease-in-out;
  }
  
  @keyframes flipCard {
    0% {
      transform: rotateY(0deg);
    }
    50% {
      transform: rotateY(90deg);
    }
    100% {
      transform: rotateY(0deg);
    }
  }
  
  .fortune-card {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    border-radius: 12px;
    padding: 2rem;
    color: white;
    margin: 2rem 0;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    animation: slideIn 0.5s ease-out;
  }
  
  @keyframes slideIn {
    from {
      opacity: 0;
      transform: translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
  
  .card-emoji {
    font-size: 5rem;
    margin: 0.5rem 0;
  }
  
  .card-name {
    font-size: 2rem;
    font-weight: bold;
    margin: 1rem 0;
  }
  
  .card-meaning {
    font-size: 1.1rem;
    line-height: 1.8;
    opacity: 0.95;
    margin: 1rem 0;
  }
  
  .card-description {
    font-size: 0.95rem;
    line-height: 1.7;
    opacity: 0.9;
    text-align: left;
    background: rgba(255, 255, 255, 0.1);
    padding: 1.5rem;
    border-radius: 8px;
    margin: 1.5rem 0;
  }
  
  .card-keywords {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    justify-content: center;
    margin: 1rem 0;
  }
  
  .keyword-tag {
    background: rgba(255, 255, 255, 0.2);
    padding: 0.3rem 0.8rem;
    border-radius: 20px;
    font-size: 0.85rem;
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
  
  #draw-again-btn {
    background: white;
    color: #667eea;
    border: 2px solid #667eea;
    padding: 0.75rem 1.5rem;
    font-size: 0.95rem;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 600;
    transition: all 0.3s;
    margin-top: 1rem;
  }
  
  #draw-again-btn:hover {
    background: #667eea;
    color: white;
    transform: translateY(-2px);
  }
</style>

<div id="fortune-container">
  <h2>오늘의 운세 🔮</h2>
  <p>카드를 선택하여 오늘의 타로 운세를 봐보세요. 한 번 점을 보면 그날 결과는 고정됩니다.</p>
  
  <div id="initial-container">
    <p style="font-size: 0.95rem; opacity: 0.7; margin: 1.5rem 0;">카드를 하나 선택하세요</p>
    <div id="card-selection"></div>
  </div>
  
  <div id="result-container" style="display: none;">
    <div class="fortune-card">
      <div class="card-emoji" id="card-emoji"></div>
      <div class="card-name" id="card-name"></div>
      <div class="card-meaning" id="card-meaning"></div>
      <div class="card-keywords" id="card-keywords"></div>
      <div class="card-description" id="card-description"></div>
    </div>
    <div class="date-info" id="date-info"></div>
    <div class="reset-note">자정이 지나면 새로운 운세를 볼 수 있습니다</div>
  </div>
</div>

<script>
  (function () {
    const TAROT_CARDS = [
      { name: 'The Fool', emoji: '🤡', keywords: ['시작', '모험', '용기'], description: '새로운 시작과 모험을 준비하세요. 두려움 없이 첫 걸음을 내디디는 시간입니다.' },
      { name: 'The Magician', emoji: '🎩', keywords: ['창의성', '재능', '자신감'], description: '당신의 재능과 능력을 발휘할 시간입니다. 창의적인 해결책이 필요한 순간입니다.' },
      { name: 'The High Priestess', emoji: '👑', keywords: ['직관', '신비', '내면'], description: '당신의 직감을 믿으세요. 내면의 목소리가 답을 알고 있습니다.' },
      { name: 'The Empress', emoji: '👸', keywords: ['풍요', '창조성', '아름다움'], description: '풍요로움과 창조적인 에너지가 당신을 감싸고 있습니다. 새로운 프로젝트에 좋은 시기입니다.' },
      { name: 'The Emperor', emoji: '🤴', keywords: ['권력', '리더십', '안정성'], description: '리더십을 발휘할 때입니다. 자신감 있게 결정하고 책임을 지세요.' },
      { name: 'The Hierophant', emoji: '🙏', keywords: ['전통', '지혜', '영성'], description: '전통의 지혜와 영적 깨달음이 당신을 인도하고 있습니다.' },
      { name: 'The Lovers', emoji: '💕', keywords: ['사랑', '조화', '선택'], description: '조화롭고 의미 있는 관계와 선택이 당신을 기다리고 있습니다.' },
      { name: 'The Chariot', emoji: '🏎️', keywords: ['승리', '결단력', '진행'], description: '당신의 의지로 장애물을 극복하세요. 승리가 가까워지고 있습니다.' },
      { name: 'Strength', emoji: '💪', keywords: ['내적강함', '인내', '용감함'], description: '당신은 생각보다 강합니다. 내적 힘으로 어려움을 극복할 수 있습니다.' },
      { name: 'The Hermit', emoji: '🕯️', keywords: ['성찰', '고독', '탐색'], description: '자신과의 시간이 필요합니다. 잠시 멈추고 내면을 탐색해보세요.' },
      { name: 'The Wheel of Fortune', emoji: '🎡', keywords: ['운명', '변화', '순환'], description: '삶의 흐름이 순환하고 있습니다. 지금의 변화를 받아들이세요.' },
      { name: 'Justice', emoji: '⚖️', keywords: ['공정성', '진실', '균형'], description: '정직함과 공정성이 중요합니다. 객관적 판단을 해보세요.' },
      { name: 'The Hanged Man', emoji: '🔄', keywords: ['포기', '관점', '휴식'], description: '다른 각도에서 상황을 봐보세요. 잠시의 포기가 새로운 관점을 줄 것입니다.' },
      { name: 'Death', emoji: '🦋', keywords: ['변화', '재생', '시작'], description: '무언가가 끝나고 새로운 것이 시작됩니다. 변화를 두려워하지 마세요.' },
      { name: 'Temperance', emoji: '🌊', keywords: ['조절', '균형', '조화'], description: '균형과 조절이 필요합니다. 극단적인 행동을 피하고 중간을 찾으세요.' },
      { name: 'The Devil', emoji: '😈', keywords: ['제약', '검토', '자유'], description: '당신을 제약하는 무언가를 인식하세요. 진정한 자유를 원한다면 행동하세요.' },
      { name: 'The Tower', emoji: '🌩️', keywords: ['급변', '혼란', '기초'], description: '급격한 변화가 오고 있거나 이미 와 있습니다. 새로운 기초를 다지세요.' },
      { name: 'The Star', emoji: '⭐', keywords: ['희망', '영감', '미래'], description: '희망의 빛이 당신을 비추고 있습니다. 꿈을 잃지 마세요.' },
      { name: 'The Moon', emoji: '🌙', keywords: ['직관', '불안', '환상'], description: '당신의 직감을 따르되, 환상과 현실을 구분하세요.' },
      { name: 'The Sun', emoji: '☀️', keywords: ['행복', '성공', '에너지'], description: '밝고 긍정적인 에너지가 당신을 감싸고 있습니다. 행복과 성공이 온다는 신호입니다.' },
      { name: 'Judgement', emoji: '📯', keywords: ['각성', '회복', '시대'], description: '새로운 시대의 시작입니다. 당신의 소명을 깨달을 때가 왔습니다.' },
      { name: 'The World', emoji: '🌍', keywords: ['완성', '성취', '주기'], description: '한 주기가 완성되고 새로운 시작을 앞두고 있습니다. 축하합니다!' }
    ];

    function getTodayKey() {
      const kst = new Date(Date.now() + 9 * 60 * 60 * 1000);
      return 'fortune_' + kst.toISOString().slice(0, 10);
    }

    function renderCardSelection() {
      const container = document.getElementById('card-selection');
      container.innerHTML = '';
      
      for (let i = 0; i < 5; i++) {
        const cardDiv = document.createElement('div');
        cardDiv.className = 'card-back';
        cardDiv.textContent = '🎴';
        cardDiv.dataset.index = i;
        cardDiv.addEventListener('click', selectCard);
        container.appendChild(cardDiv);
      }
    }

    function selectCard(e) {
      const cards = document.querySelectorAll('.card-back');
      cards.forEach(c => c.removeEventListener('click', selectCard));
      
      const selectedDiv = e.target;
      selectedDiv.classList.add('selected');
      
      setTimeout(() => {
        const randomCard = TAROT_CARDS[Math.floor(Math.random() * TAROT_CARDS.length)];
        const kst = new Date(Date.now() + 9 * 60 * 60 * 1000);
        const dateStr = kst.toLocaleDateString('ko-KR', { year: 'numeric', month: 'long', day: 'numeric' });
        
        const key = getTodayKey();
        localStorage.setItem(key, JSON.stringify({ card: randomCard, date: dateStr }));
        
        displayCard(randomCard, dateStr);
      }, 600);
    }

    function displayCard(card, savedDate) {
      const resultContainer = document.getElementById('result-container');
      const initialContainer = document.getElementById('initial-container');
      
      document.getElementById('card-emoji').textContent = card.emoji;
      document.getElementById('card-name').textContent = card.name;
      document.getElementById('card-meaning').textContent = card.keywords.join(' · ');
      document.getElementById('card-description').textContent = card.description;
      document.getElementById('date-info').textContent = '📅 ' + savedDate;
      
      const keywordsDiv = document.getElementById('card-keywords');
      keywordsDiv.innerHTML = card.keywords.map(k => `<span class="keyword-tag">#${k}</span>`).join('');
      
      initialContainer.style.display = 'none';
      resultContainer.style.display = 'block';
    }

    function showSavedCard() {
      const key = getTodayKey();
      const saved = localStorage.getItem(key);
      
      if (saved) {
        const data = JSON.parse(saved);
        displayCard(data.card, data.date);
        return true;
      }
      return false;
    }

    if (!showSavedCard()) {
      renderCardSelection();
    }
  })();
</script>
