---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

개발을 공부하면서 배운 내용과 일상을 정리합니다

{% if site.bootcamp.start_date and site.bootcamp.end_date %}
<style>
  #bootcamp-progress { max-width: 480px; margin: 1.5rem 0; }
  #bootcamp-progress .bootcamp-progress-track {
    width: 100%;
    height: 10px;
    border-radius: 5px;
    background-color: rgba(128, 128, 128, 0.25);
    overflow: hidden;
  }
  #bootcamp-progress-fill {
    height: 100%;
    width: 0%;
    border-radius: 5px;
    background-color: #6f42c1;
    transition: width 0.3s ease;
  }
  #bootcamp-progress-text {
    margin: 0.5rem 0 0;
    font-size: 0.85rem;
    opacity: 0.8;
  }
</style>

<div id="bootcamp-progress" data-start="{{ site.bootcamp.start_date }}" data-end="{{ site.bootcamp.end_date }}">
  <div class="bootcamp-progress-track" role="progressbar" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100">
    <div id="bootcamp-progress-fill"></div>
  </div>
  <p id="bootcamp-progress-text"></p>
</div>

<script>
  (function () {
    var el = document.getElementById('bootcamp-progress');
    if (!el) return;

    /* 부트캠프 기간(2026-08-26 ~ 2027-02-16) 안에 있는 대한민국 공휴일·대체공휴일.
       기간이 바뀌거나 임시공휴일이 새로 지정되면 이 목록도 함께 업데이트해야 한다. */
    var holidays = [
      '2026-09-24', '2026-09-25', '2026-09-26', /* 추석 연휴 */
      '2026-10-03', '2026-10-05', /* 개천절 + 대체공휴일 */
      '2026-10-09', /* 한글날 */
      '2026-12-25', /* 성탄절 */
      '2027-01-01', /* 신정 */
      '2027-02-06', '2027-02-07', '2027-02-08', '2027-02-09' /* 설날 연휴 + 대체공휴일 */
    ];

    function parseYMD(str) {
      var p = str.split('-').map(Number);
      return new Date(Date.UTC(p[0], p[1] - 1, p[2]));
    }

    function formatYMD(date) {
      return date.toISOString().slice(0, 10);
    }

    function countBusinessDays(from, to) {
      var count = 0;
      var cur = new Date(from.getTime());
      while (cur <= to) {
        var day = cur.getUTCDay();
        if (day !== 0 && day !== 6 && holidays.indexOf(formatYMD(cur)) === -1) {
          count++;
        }
        cur = new Date(cur.getTime() + 86400000);
      }
      return count;
    }

    var start = parseYMD(el.dataset.start);
    var end = parseYMD(el.dataset.end);

    /* 브라우저의 로컬 시간대와 무관하게, 한국 시간(KST) 기준 오늘 날짜를 구한다. */
    var todayKST = parseYMD(formatYMD(new Date(Date.now() + 9 * 60 * 60 * 1000)));
    var today = todayKST < start ? start : (todayKST > end ? end : todayKST);

    var totalDays = countBusinessDays(start, end);
    var elapsedDays = countBusinessDays(start, today);
    var percent = totalDays > 0 ? Math.min(100, Math.round((elapsedDays / totalDays) * 100)) : 0;

    var fill = document.getElementById('bootcamp-progress-fill');
    var text = document.getElementById('bootcamp-progress-text');
    var track = el.querySelector('.bootcamp-progress-track');
    fill.style.width = percent + '%';
    track.setAttribute('aria-valuenow', percent);
    text.textContent = percent + '% 진행 (' + elapsedDays + '/' + totalDays + '일, 공휴일·주말 제외)';
  })();
</script>
{% endif %}