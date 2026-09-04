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
  
  .card-emoji.reversed {
    transform: rotate(180deg);
    display: inline-block;
  }
  
  .card-name {
    font-size: 2rem;
    font-weight: bold;
    margin: 1rem 0;
  }
  
  .card-direction {
    font-size: 0.85rem;
    font-weight: 600;
    color: #ffd700;
    margin-top: 0.5rem;
    text-transform: uppercase;
    letter-spacing: 1px;
  }
  
  .card-direction.reversed {
    color: #ff6b6b;
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
      <div class="card-direction" id="card-direction"></div>
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
      { name: 'The Fool', emoji: '🤡', upright: { keywords: ['시작', '모험', '용기'], description: '새로운 시작과 모험을 준비하세요. 두려움 없이 첫 걸음을 내디디는 시간입니다.' }, reversed: { keywords: ['무모함', '뒤틀림', '위기'], description: '무모한 결정을 피하세요. 신중함이 필요한 시점입니다. 현재의 계획을 다시 검토해보세요.' } },
      { name: 'The Magician', emoji: '🎩', upright: { keywords: ['창의성', '재능', '자신감'], description: '당신의 재능과 능력을 발휘할 시간입니다. 창의적인 해결책이 필요한 순간입니다.' }, reversed: { keywords: ['사기', '혼란', '유능함부족'], description: '당신의 능력에 의심을 품지 마세요. 하지만 현재는 신중함과 진실함이 더 중요합니다.' } },
      { name: 'The High Priestess', emoji: '👑', upright: { keywords: ['직관', '신비', '내면'], description: '당신의 직감을 믿으세요. 내면의 목소리가 답을 알고 있습니다.' }, reversed: { keywords: ['혼란', '도피', '억압'], description: '당신의 직감을 무시하지 마세요. 진실과 마주할 용기가 필요합니다.' } },
      { name: 'The Empress', emoji: '👸', upright: { keywords: ['풍요', '창조성', '아름다움'], description: '풍요로움과 창조적인 에너지가 당신을 감싸고 있습니다. 새로운 프로젝트에 좋은 시기입니다.' }, reversed: { keywords: ['정체', '의존성', '공허함'], description: '독립성을 되찾을 시간입니다. 과도한 의존성에서 벗어나세요.' } },
      { name: 'The Emperor', emoji: '🤴', upright: { keywords: ['권력', '리더십', '안정성'], description: '리더십을 발휘할 때입니다. 자신감 있게 결정하고 책임을 지세요.' }, reversed: { keywords: ['약함', '통제상실', '혼란'], description: '현재는 완전한 통제가 어렵습니다. 겸손함을 배우고 도움을 청하세요.' } },
      { name: 'The Hierophant', emoji: '🙏', upright: { keywords: ['전통', '지혜', '영성'], description: '전통의 지혜와 영적 깨달음이 당신을 인도하고 있습니다.' }, reversed: { keywords: ['반항', '신앙상실', '해방'], description: '기존의 규칙과 전통에 의문을 제기할 시간입니다. 자신만의 길을 찾으세요.' } },
      { name: 'The Lovers', emoji: '💕', upright: { keywords: ['사랑', '조화', '선택'], description: '조화롭고 의미 있는 관계와 선택이 당신을 기다리고 있습니다.' }, reversed: { keywords: ['불화', '분리', '갈등'], description: '관계에 어려움이 있습니다. 솔직한 대화가 필요합니다.' } },
      { name: 'The Chariot', emoji: '🏎️', upright: { keywords: ['승리', '결단력', '진행'], description: '당신의 의지로 장애물을 극복하세요. 승리가 가까워지고 있습니다.' }, reversed: { keywords: ['지연', '통제상실', '낭비'], description: '진행이 지연되고 있습니다. 방향을 재조정해야 할 시간입니다.' } },
      { name: 'Strength', emoji: '💪', upright: { keywords: ['내적강함', '인내', '용감함'], description: '당신은 생각보다 강합니다. 내적 힘으로 어려움을 극복할 수 있습니다.' }, reversed: { keywords: ['약함', '의심', '패배감'], description: '자신의 약함을 인정하고 도움을 받으세요. 이것도 용감함입니다.' } },
      { name: 'The Hermit', emoji: '🕯️', upright: { keywords: ['성찰', '고독', '탐색'], description: '자신과의 시간이 필요합니다. 잠시 멈추고 내면을 탐색해보세요.' }, reversed: { keywords: ['고립', '도피', '고독'], description: '고립에서 벗어나세요. 타인과의 연결이 필요합니다.' } },
      { name: 'The Wheel of Fortune', emoji: '🎡', upright: { keywords: ['운명', '변화', '순환'], description: '삶의 흐름이 순환하고 있습니다. 지금의 변화를 받아들이세요.' }, reversed: { keywords: ['불운', '정체', '거역'], description: '현재는 어렵지만, 이것도 순환의 일부입니다. 견디세요.' } },
      { name: 'Justice', emoji: '⚖️', upright: { keywords: ['공정성', '진실', '균형'], description: '정직함과 공정성이 중요합니다. 객관적 판단을 해보세요.' }, reversed: { keywords: ['부정의', '편견', '복수'], description: '편견을 버리고 공정함을 추구하세요. 보상은 뒤따를 것입니다.' } },
      { name: 'The Hanged Man', emoji: '🔄', upright: { keywords: ['포기', '관점', '휴식'], description: '다른 각도에서 상황을 봐보세요. 잠시의 포기가 새로운 관점을 줄 것입니다.' }, reversed: { keywords: ['집착', '계속진행', '완고함'], description: '계속 나아가야 할 시간입니다. 과거에 집착하지 마세요.' } },
      { name: 'Death', emoji: '🦋', upright: { keywords: ['변화', '재생', '시작'], description: '무언가가 끝나고 새로운 것이 시작됩니다. 변화를 두려워하지 마세요.' }, reversed: { keywords: ['정체', '저항', '거절'], description: '변화에 저항하고 있습니다. 시간이 변할 것을 강요하지 말고 흐름을 따르세요.' } },
      { name: 'Temperance', emoji: '🌊', upright: { keywords: ['조절', '균형', '조화'], description: '균형과 조절이 필요합니다. 극단적인 행동을 피하고 중간을 찾으세요.' }, reversed: { keywords: ['과다', '불균형', '분쟁'], description: '균형을 잃었습니다. 모든 것을 절제하고 조절해보세요.' } },
      { name: 'The Devil', emoji: '😈', upright: { keywords: ['제약', '검토', '자유'], description: '당신을 제약하는 무언가를 인식하세요. 진정한 자유를 원한다면 행동하세요.' }, reversed: { keywords: ['해방', '자유', '거절'], description: '마침내 제약에서 벗어날 시간이 왔습니다. 자유를 향해 나아가세요.' } },
      { name: 'The Tower', emoji: '🌩️', upright: { keywords: ['급변', '혼란', '기초'], description: '급격한 변화가 오고 있거나 이미 와 있습니다. 새로운 기초를 다지세요.' }, reversed: { keywords: ['지연', '회피', '불안'], description: '변화가 지연되고 있습니다. 불안감이 있지만, 대면해야 합니다.' } },
      { name: 'The Star', emoji: '⭐', upright: { keywords: ['희망', '영감', '미래'], description: '희망의 빛이 당신을 비추고 있습니다. 꿈을 잃지 마세요.' }, reversed: { keywords: ['절망', '혼란', '방향상실'], description: '길을 잃은 것 같지만, 희망을 잃지 마세요. 별은 여전히 당신을 비추고 있습니다.' } },
      { name: 'The Moon', emoji: '🌙', upright: { keywords: ['직관', '불안', '환상'], description: '당신의 직감을 따르되, 환상과 현실을 구분하세요.' }, reversed: { keywords: ['명확함', '진실', '해명'], description: '혼란이 걷히고 진실이 드러날 것입니다. 명확함이 옵니다.' } },
      { name: 'The Sun', emoji: '☀️', upright: { keywords: ['행복', '성공', '에너지'], description: '밝고 긍정적인 에너지가 당신을 감싸고 있습니다. 행복과 성공이 온다는 신호입니다.' }, reversed: { keywords: ['그림자', '지연', '슬픔'], description: '현재는 어둡지만, 태양은 반드시 다시 뜹니다. 희망을 잃지 마세요.' } },
      { name: 'Judgement', emoji: '📯', upright: { keywords: ['각성', '회복', '시대'], description: '새로운 시대의 시작입니다. 당신의 소명을 깨달을 때가 왔습니다.' }, reversed: { keywords: ['의심', '자책', '연기'], description: '자신을 의심하지 마세요. 이제 일어설 시간입니다.' } },
      { name: 'The World', emoji: '🌍', upright: { keywords: ['완성', '성취', '주기'], description: '한 주기가 완성되고 새로운 시작을 앞두고 있습니다. 축하합니다!' }, reversed: { keywords: ['미완성', '지연', '순환'], description: '아직 끝나지 않았습니다. 마지막 단계를 완성하세요.' } }
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
        const isReversed = Math.random() < 0.5;
        const kst = new Date(Date.now() + 9 * 60 * 60 * 1000);
        const dateStr = kst.toLocaleDateString('ko-KR', { year: 'numeric', month: 'long', day: 'numeric' });
        
        const key = getTodayKey();
        localStorage.setItem(key, JSON.stringify({ card: randomCard, date: dateStr, reversed: isReversed }));
        
        displayCard(randomCard, dateStr, isReversed);
      }, 600);
    }

    function displayCard(card, savedDate, isReversed) {
      const resultContainer = document.getElementById('result-container');
      const initialContainer = document.getElementById('initial-container');
      
      const cardData = isReversed ? card.reversed : card.upright;
      const emojiEl = document.getElementById('card-emoji');
      const directionEl = document.getElementById('card-direction');
      
      emojiEl.textContent = card.emoji;
      if (isReversed) {
        emojiEl.classList.add('reversed');
        directionEl.textContent = '역방향';
        directionEl.classList.add('reversed');
      } else {
        emojiEl.classList.remove('reversed');
        directionEl.textContent = '정방향';
        directionEl.classList.remove('reversed');
      }
      
      document.getElementById('card-name').textContent = card.name;
      document.getElementById('card-meaning').textContent = cardData.keywords.join(' · ');
      document.getElementById('card-description').textContent = cardData.description;
      document.getElementById('date-info').textContent = '📅 ' + savedDate;
      
      const keywordsDiv = document.getElementById('card-keywords');
      keywordsDiv.innerHTML = cardData.keywords.map(k => `<span class="keyword-tag">#${k}</span>`).join('');
      
      initialContainer.style.display = 'none';
      resultContainer.style.display = 'block';
    }

    function showSavedCard() {
      const key = getTodayKey();
      const saved = localStorage.getItem(key);
      
      if (saved) {
        const data = JSON.parse(saved);
        displayCard(data.card, data.date, data.reversed || false);
        return true;
      }
      return false;
    }

    if (!showSavedCard()) {
      renderCardSelection();
    }
  })();
</script>
