# re-project-eden
<title>PROJECT EDEN</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Chakra+Petch:wght@500;600;700&family=IBM+Plex+Sans+KR:wght@400;500;600&display=swap">
<style>
/* Layout: 한 화면 한 장면. 상단 HUD(시점, 챕터 5칸, 접이식 지표), 본문, 하단 선택지. 시점이 바뀌면 포인트 색이 바뀐다. */
:root {
  --bg: #080c11;
  --surface: #0e151c;
  --raise: #151e28;
  --line: #213040;
  --fg: #e6edf3;
  --muted: #9aa8b7;
  --accent: #b7c4d1;
  --kael: #9ad7ff;
  --riwa: #ff9a3d;
  --joint: #7fd6d0;
  --deep: #8ea3bb;
  --good: #58d3a0;
  --bad: #ff6b6b;
  --violet: #b99aff;
  --gold: #ffd166;
  --f-display: 'Chakra Petch', 'IBM Plex Sans KR', system-ui, sans-serif;
  --f-body: 'IBM Plex Sans KR', 'Noto Sans KR', system-ui, -apple-system, 'Apple SD Gothic Neo', 'Malgun Gothic', sans-serif;
  color-scheme: dark;
}
:root[data-pov="kael"] { --accent: var(--kael); }
:root[data-pov="riwa"] { --accent: var(--riwa); }
:root[data-pov="joint"] { --accent: var(--joint); }
:root[data-pov="end-good"] { --accent: var(--good); }
:root[data-pov="end-bad"] { --accent: var(--bad); }
:root[data-pov="end-violet"] { --accent: var(--violet); }
:root[data-pov="end-gold"] { --accent: var(--gold); }
:root[data-pov="end-deep"] { --accent: var(--deep); }

* { box-sizing: border-box; }
html, body { min-height: 100%; }
body {
  margin: 0;
  background-color: var(--bg);
  background-image: radial-gradient(120% 55% at 50% -8%, color-mix(in srgb, var(--accent) 13%, transparent), transparent 62%);
  background-repeat: no-repeat;
  color: var(--fg);
  font-family: var(--f-body);
  font-size: 16px;
  line-height: 1.7;
  padding-block: 16px 48px;
  padding-left: max(16px, env(safe-area-inset-left, 0px));
  padding-right: max(16px, env(safe-area-inset-right, 0px));
}
#app { max-width: 560px; margin: 0 auto; }

/* HUD */
#hud { display: grid; gap: 10px; margin-bottom: 22px; }
.hudrow { display: flex; align-items: center; gap: 10px; min-width: 0; flex-wrap: wrap; }
#pov {
  font-family: var(--f-display); font-weight: 600; font-size: 12px; letter-spacing: .12em;
  text-transform: uppercase; color: var(--accent); border: 1px solid var(--accent);
  padding: 2px 9px; border-radius: 4px; white-space: nowrap;
}
#chapter { color: var(--muted); font-size: 13px; min-width: 0; }
#steps { display: grid; grid-template-columns: repeat(5, 1fr); gap: 4px; }
#steps i { height: 3px; background: var(--line); border-radius: 2px; }
#steps i.on { background: var(--accent); }
details#stat summary { cursor: pointer; color: var(--muted); font-size: 13px; min-height: 28px; list-style: none; }
details#stat summary::-webkit-details-marker { display: none; }
details#stat summary::before { content: '▸ '; }
details#stat[open] summary::before { content: '▾ '; }
#meters { display: grid; gap: 6px; padding: 6px 0 2px; }
.mrow { display: grid; grid-template-columns: 8.5em 1fr 3ch; gap: 10px; align-items: center; font-size: 13px; color: var(--muted); }
.mrow .bar { height: 4px; background: var(--line); border-radius: 2px; overflow: hidden; }
.mrow .bar b { display: block; height: 100%; background: var(--accent); }
.mrow.danger .bar b { background: var(--bad); }
.mrow .n { font-family: var(--f-display); font-variant-numeric: tabular-nums; color: var(--fg); text-align: right; }

/* Story */
#story h1 {
  font-family: var(--f-display); font-weight: 700; letter-spacing: .06em;
  font-size: clamp(34px, 11vw, 56px); line-height: 1.05; margin: 8px 0 12px; text-wrap: balance;
}
#story h2 {
  font-family: var(--f-display); font-weight: 600; font-size: 22px; line-height: 1.3;
  margin: 0 0 14px; text-wrap: balance;
}
#story p { margin: 0 0 14px; max-width: 65ch; font-size: 17px; line-height: 1.8; }
#story p.res { margin-top: 18px; padding: 12px 14px; background: var(--surface); border: 1px solid var(--line); border-radius: 10px; font-size: 16.5px; }
#story p.dlg { display: grid; grid-template-columns: 36px minmax(0, 1fr); gap: 12px; align-items: start; margin-bottom: 16px; }
#story p.dlg .box { display: grid; gap: 1px; min-width: 0; }
#story p.dlg .who { font-family: var(--f-display); font-weight: 600; font-size: 13.5px; color: var(--fc); letter-spacing: .02em; }
#story p.dlg .say { color: var(--fg); font-size: 17px; line-height: 1.65; overflow-wrap: anywhere; }
.av { width: 36px; height: 36px; border-radius: 50%; display: grid; place-items: center; font-family: var(--f-display); font-weight: 700; font-size: 16px; line-height: 1;
  color: var(--fc); background: color-mix(in srgb, var(--fc) 18%, var(--surface)); border: 1px solid var(--fc); }
.f-kael { --fc: var(--kael); }
.f-deep { --fc: var(--deep); }
.f-riwa { --fc: var(--riwa); }
.cast { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 8px 12px; margin: 14px 0 6px; padding: 0; list-style: none; }
.cast li { display: grid; grid-template-columns: 36px minmax(0, 1fr); gap: 10px; align-items: center; }
.cast .nm { font-weight: 600; font-size: 15px; line-height: 1.3; }
.cast .rl { color: var(--muted); font-size: 13px; line-height: 1.3; }
#story p.warn { color: var(--bad); font-size: 15px; }
#story p.status { font-family: var(--f-display); font-weight: 600; letter-spacing: .04em; }
#story p.status.good { color: var(--good); }
#story p.status.bad { color: var(--bad); }
#story p.sub { color: var(--muted); font-size: 15px; }
#story p.etag {
  font-family: var(--f-display); font-weight: 600; letter-spacing: .16em; font-size: 13px;
  color: var(--accent); margin: 0; text-transform: uppercase;
}
#story p.cast { color: var(--muted); font-size: 15px; margin-bottom: 4px; }
@media (prefers-reduced-motion: no-preference) {
  #story > * { animation: rise .45s ease both; }
  @keyframes rise { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: none; } }
}

.factions { display: grid; gap: 10px; margin: 20px 0 22px; padding: 0; list-style: none; }
.factions li { display: grid; grid-template-columns: 10px 1fr; gap: 12px; align-items: baseline; font-size: 15px; line-height: 1.55; }
.factions .dot { width: 10px; height: 10px; border-radius: 2px; display: inline-block; }
.factions b { font-weight: 600; }
.factions span.d { color: var(--muted); }
.practice { display: flex; align-items: center; gap: 10px; margin: 6px 0 18px; font-size: 15px; color: var(--muted); min-height: 44px; }
.practice input { width: 20px; height: 20px; accent-color: var(--accent); }

/* Choices */
#choices { margin-top: 24px; display: grid; gap: 12px; }
.opts { display: grid; gap: 10px; }
.opt {
  display: grid; grid-template-columns: 30px 1fr; gap: 12px; align-items: start; text-align: left;
  background: var(--surface); color: var(--fg); border: 1px solid var(--line); border-radius: 10px;
  padding: 14px; min-height: 56px; font: inherit; line-height: 1.5; cursor: pointer; width: 100%;
  -webkit-tap-highlight-color: transparent;
}
.opt .n { font-family: var(--f-display); font-weight: 600; color: var(--accent); font-size: 17px; line-height: 1.5; }
.opt .t { display: grid; gap: 2px; min-width: 0; }
.opt .l { font-weight: 500; font-size: 17px; overflow-wrap: anywhere; }
.opt .h { color: var(--muted); font-size: 14.5px; line-height: 1.5; overflow-wrap: anywhere; }
.opt:hover:not(:disabled) { border-color: var(--accent); background: var(--raise); }
.opt:focus-visible, .next:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }
.opt.picked { border-color: var(--accent); background: var(--raise); }
.opt.dim { opacity: .42; }
.opt.locked { border-style: dashed; color: var(--muted); cursor: not-allowed; }
.next {
  width: 100%; min-height: 52px; background: transparent; color: var(--accent); border: 1px solid var(--accent);
  border-radius: 10px; font-family: var(--f-display); font-weight: 600; font-size: 16px; letter-spacing: .08em; cursor: pointer;
  -webkit-tap-highlight-color: transparent;
}
.next:hover { background: color-mix(in srgb, var(--accent) 12%, transparent); }
.timer { display: flex; align-items: center; gap: 12px; }
.timer .track { flex: 1; height: 4px; background: var(--line); border-radius: 2px; overflow: hidden; }
.timer .fill { height: 100%; width: 100%; background: var(--accent); }
.timer .fill.urgent { background: var(--bad); }
.timer .sec { font-family: var(--f-display); font-weight: 600; font-variant-numeric: tabular-nums; min-width: 4ch; text-align: right; font-size: 15px; }

/* Ending log, route */
.coll { margin-top: 26px; }
.coll .head { font-family: var(--f-display); font-weight: 600; letter-spacing: .14em; font-size: 12px; color: var(--muted); text-transform: uppercase; margin: 0 0 10px; }
.slots { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 8px; }
.slot { border: 1px solid var(--line); border-radius: 8px; padding: 10px 12px; display: grid; gap: 2px; min-width: 0; }
.slot .k { font-family: var(--f-display); font-size: 12px; letter-spacing: .1em; color: var(--muted); text-transform: uppercase; }
.slot .v { font-size: 14px; line-height: 1.4; overflow-wrap: anywhere; color: var(--muted); }
.slot.got .v { color: var(--fg); }
.slot.new { border-color: var(--accent); }
.slot.new .k { color: var(--accent); }
.route { list-style: none; margin: 8px 0 0; padding: 0; display: grid; }
.route li { padding: 8px 0; border-bottom: 1px solid var(--line); font-size: 14.5px; color: var(--muted); line-height: 1.5; }
.route li:last-child { border-bottom: 0; }
h3.sec-h { font-family: var(--f-display); font-weight: 600; letter-spacing: .12em; font-size: 12px; color: var(--muted); text-transform: uppercase; margin: 26px 0 0; }

/* 미션 수치 타일 */
.stats { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 8px; margin: 16px 0 8px; }
.stat { background: var(--surface); border: 1px solid var(--line); border-radius: 8px; padding: 10px 6px; display: grid; gap: 2px; text-align: center; min-width: 0; }
.stat .k { font-size: 12px; color: var(--muted); line-height: 1.3; overflow-wrap: anywhere; }
.stat .v { font-family: var(--f-display); font-weight: 700; font-size: 28px; line-height: 1.1; font-variant-numeric: tabular-nums; }
.stat.ok .v { color: var(--good); }
.stat.bad .v { color: var(--bad); }

/* 미션 단계 이동 막대 */
.mnav { display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 6px; list-style: none; padding: 0; margin: 0 0 18px; }
.mnav li { font-size: 12.5px; text-align: center; padding: 8px 4px; border: 1px solid var(--line); border-radius: 8px; color: var(--muted); line-height: 1.3; overflow-wrap: anywhere; }
.mnav li.cur { color: var(--fg); border-color: var(--accent); background: var(--surface); font-weight: 600; }
.mnav li.done { color: var(--accent); }
.mnav li.done::before { content: '✓ '; }

/* 슬라이더 입력 */
.slider { display: grid; gap: 12px; }
.sl-info { display: grid; gap: 2px; padding: 12px 14px; background: var(--surface); border: 1px solid var(--line); border-radius: 10px; }
.sl-info b { font-size: 18px; font-weight: 600; }
.sl-info span { color: var(--muted); font-size: 14.5px; line-height: 1.5; }
.slider input[type=range] { width: 100%; height: 36px; accent-color: var(--accent); margin: 0; }
.sl-ticks { display: flex; justify-content: space-between; font-size: 13.5px; color: var(--muted); margin-top: -4px; }
.next:disabled { opacity: .45; cursor: not-allowed; }

/* 비커 그림 */
.beaker { margin: 14px 0 10px; display: grid; gap: 8px; justify-items: center; }
.beaker svg { width: 100%; max-width: 360px; height: auto; background: var(--surface); border: 1px solid var(--line); border-radius: 12px; }
.legend { display: flex; flex-wrap: wrap; justify-content: center; gap: 6px 16px; font-size: 13.5px; color: var(--muted); }
.legend i { display: inline-block; width: 10px; height: 10px; border-radius: 50%; margin-right: 6px; }
@media (prefers-reduced-motion: no-preference) {
  .dot { animation: drift 3.6s ease-in-out infinite alternate; }
  @keyframes drift { from { transform: translate(-2px, 2px); } to { transform: translate(3px, -3px); } }
}

/* 판정 표시 */
.pill { display: inline-block; padding: 3px 14px; border-radius: 999px; border: 1px solid currentColor; font-family: var(--f-display); font-weight: 600; font-size: 15px; letter-spacing: .04em; }
.pill.good { color: var(--good); }
.pill.mid { color: var(--gold); }
.pill.bad { color: var(--bad); }
</style>

<div id="app">
  <header id="hud" hidden>
    <div class="hudrow"><span id="pov">EDEN</span><span id="chapter"></span></div>
    <div id="steps" aria-hidden="true"></div>
    <details id="stat"><summary>상태 보기</summary><div id="meters"></div></details>
  </header>
  <main>
    <div id="story" aria-live="polite"></div>
    <div id="choices"></div>
  </main>
</div>

<script>
(function () {
  'use strict';

  var ENDINGS = {
    HARMONY: { tag: 'THE HARMONY', name: '공존', pov: 'end-good' },
    ISOLATION: { tag: 'THE ISOLATION', name: '고립', pov: 'end-deep' },
    DEPENDENCE: { tag: 'THE DEPENDENCE', name: '의존', pov: 'end-gold' },
    NEWRULERS: { tag: 'THE NEW RULERS', name: '새로운 지배자', pov: 'end-violet' },
    COLLAPSE: { tag: 'THE COLLAPSE', name: '붕괴', pov: 'end-bad' }
  };
  var ORDER = ['HARMONY', 'ISOLATION', 'DEPENDENCE', 'NEWRULERS', 'COLLAPSE'];
  var CAST = [
    ['카엘', '제1인류 · 보안수사관', 'kael'], ['리와', '제3인류 · 잠입 요원', 'riwa'],
    ['에이든', '제1인류 · 생체 칩 연구원', 'kael'], ['세린', '제2인류 · 급진파 기술 담당', 'deep'],
    ['바렌', '제2인류 · 급진파 지도자', 'deep'], ['레온', '제1인류 · 보안국장', 'kael'],
    ['이안', '제1인류 · 카엘의 동료', 'kael'], ['마이라', '제3인류 · 공동체 대표', 'riwa'],
    ['노아', '제2인류 · 노동자', 'deep']
  ];
  var FAC = {};
  CAST.forEach(function (c) { FAC[c[0]] = c[2]; });
  var STORE = 'eden-endings-v2';
  var memEndings = {};

  var METERS = [
    ['trust', '집단 간 신뢰', 10, false],
    ['autonomy', '제3인류 자율성', 10, false],
    ['tech', '기술 완성도', 10, false],
    ['truth', '진실·증거', 10, false],
    ['stability', '주민 생존 안정성', 10, false],
    ['risk', '추적·공격 위험', 10, true],
    ['support', '급진파 지지율', 100, true]
  ];

  function $(id) { return document.getElementById(id); }
  var root = document.documentElement;
  var el = { hud: $('hud'), pov: $('pov'), chapter: $('chapter'), steps: $('steps'), meters: $('meters'), story: $('story'), choices: $('choices') };

  var stepEls = [];
  for (var si = 0; si < 5; si++) {
    var seg = document.createElement('i');
    el.steps.appendChild(seg);
    stepEls.push(seg);
  }

  var practice = false;
  var state = null;

  /* ---------- 상태 도구 ---------- */
  function newState() {
    return {
      trust: 2, autonomy: 2, tech: 0, truth: 0, stability: 3, risk: 0, support: 45,
      noa: null, data: null, aiden: null, aidenSus: false, aidenTrust: 0, leon: false,
      serin: false, p2: null, grade: null, f: null, startRes: 100, triedTalk: false,
      route: []
    };
  }
  function chg(k, d) {
    var max = k === 'support' ? 100 : 10;
    state[k] = Math.max(0, Math.min(max, state[k] + d));
  }
  function tag(id, text) { state.route.push('[' + id + '] ' + text); }
  function renderMeters() {
    el.meters.innerHTML = '';
    METERS.forEach(function (m) {
      var row = document.createElement('div');
      var v = state ? state[m[0]] : 0;
      var danger = m[3] && (m[0] === 'risk' ? v >= 3 : v >= 60);
      row.className = 'mrow' + (danger ? ' danger' : '');
      var l = document.createElement('span'); l.textContent = m[1];
      var bar = document.createElement('span'); bar.className = 'bar';
      var b = document.createElement('b'); b.style.width = (v / m[2] * 100) + '%';
      bar.appendChild(b);
      var n = document.createElement('span'); n.className = 'n'; n.textContent = String(v);
      row.appendChild(l); row.appendChild(bar); row.appendChild(n);
      el.meters.appendChild(row);
    });
  }

  /* ---------- 저장 (엔딩 도감) ---------- */
  function loadEndings() {
    try { return JSON.parse(localStorage.getItem(STORE) || '{}') || {}; }
    catch (e) { return memEndings; }
  }
  function saveEnding(code) {
    var d = loadEndings();
    d[code] = true;
    memEndings = d;
    try { localStorage.setItem(STORE, JSON.stringify(d)); } catch (e) {}
    return Object.keys(d).length;
  }

  /* ---------- 화면 도구 ---------- */
  function hud(pov, label, ch) {
    root.dataset.pov = pov;
    el.hud.hidden = false;
    var names = { kael: 'KAEL · 제1인류', riwa: 'RIWA · 제3인류', joint: '공동 시점', sys: 'EDEN' };
    el.pov.textContent = pov.indexOf('end') === 0 ? 'ENDING' : (names[pov] || 'EDEN');
    el.chapter.textContent = label;
    for (var i = 0; i < stepEls.length; i++) stepEls[i].classList.toggle('on', i < ch);
    if (state) renderMeters();
  }
  function clearStage() { el.story.innerHTML = ''; el.choices.innerHTML = ''; }
  function addHeading(t) { var h = document.createElement('h2'); h.textContent = t; el.story.appendChild(h); }
  function addText(str, cls) {
    String(str).split('\n').filter(Boolean).forEach(function (line) {
      var p = document.createElement('p');
      var m = !cls && line.match(/^(카엘|리와|에이든|세린|바렌|레온|이안|마이라|노아):\s*["“](.*)["”]$/);
      if (m) {
        p.className = 'dlg f-' + FAC[m[1]];
        var av = document.createElement('span'); av.className = 'av'; av.textContent = m[1].charAt(0);
        var box = document.createElement('span'); box.className = 'box';
        var who = document.createElement('span'); who.className = 'who'; who.textContent = m[1];
        var say = document.createElement('span'); say.className = 'say'; say.textContent = '“' + m[2] + '”';
        box.appendChild(who); box.appendChild(say);
        p.appendChild(av); p.appendChild(box);
      } else {
        if (cls) p.className = cls;
        p.textContent = line;
      }
      el.story.appendChild(p);
    });
  }
  function addStatus(ok, t) { addText(t, 'status ' + (ok ? 'good' : 'bad')); }
  function motionOk() {
    return typeof matchMedia === 'function' && !matchMedia('(prefers-reduced-motion: reduce)').matches;
  }
  function scrollToChoices() {
    try { el.choices.scrollIntoView({ block: 'nearest', behavior: motionOk() ? 'smooth' : 'auto' }); } catch (e) {}
  }
  function fmt(sec) {
    var s = Math.ceil(sec);
    if (s >= 60) return Math.floor(s / 60) + ':' + ('0' + (s % 60)).slice(-2);
    return s + '초';
  }

  function cont(label) {
    return new Promise(function (resolve) {
      el.choices.innerHTML = '';
      var b = document.createElement('button');
      b.type = 'button';
      b.className = 'next';
      b.textContent = (label || '계속') + '  ▸';
      el.choices.appendChild(b);
      var done = false;
      function go() {
        if (done) return;
        done = true;
        document.removeEventListener('keydown', kh);
        resolve();
      }
      function kh(e) { if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); go(); } }
      b.onclick = go;
      document.addEventListener('keydown', kh);
      try { b.focus({ preventScroll: true }); } catch (e) {}
      scrollToChoices();
    });
  }

  /* options: [{key,label,hint,locked,lockHint}], seconds 0이면 제한시간 없음, def는 시간 초과 시 선택될 인덱스 */
  function choose(options, seconds, def) {
    return new Promise(function (resolve) {
      el.choices.innerHTML = '';
      if (options.every(function (o) { return o.locked; })) options[0].locked = false;
      var timed = !practice && seconds > 0;
      var done = false, deadline = 0, tick = null, bar = null, secEl = null;

      if (timed) {
        var wrap = document.createElement('div'); wrap.className = 'timer';
        var track = document.createElement('div'); track.className = 'track';
        bar = document.createElement('div'); bar.className = 'fill';
        track.appendChild(bar);
        secEl = document.createElement('span'); secEl.className = 'sec';
        wrap.appendChild(track); wrap.appendChild(secEl);
        el.choices.appendChild(wrap);
        deadline = Date.now() + seconds * 1000;
      }

      var list = document.createElement('div'); list.className = 'opts';
      var btns = [];
      options.forEach(function (o, i) {
        var b = document.createElement('button');
        b.type = 'button';
        b.className = 'opt' + (o.locked ? ' locked' : '');
        var n = document.createElement('span'); n.className = 'n'; n.textContent = String(i + 1);
        var t = document.createElement('span'); t.className = 't';
        var l = document.createElement('span'); l.className = 'l'; l.textContent = o.label;
        t.appendChild(l);
        var hintText = o.locked ? (o.lockHint || '조건을 채우면 열려요') : o.hint;
        if (hintText) {
          var h = document.createElement('span'); h.className = 'h'; h.textContent = hintText;
          t.appendChild(h);
        }
        b.appendChild(n); b.appendChild(t);
        if (o.locked) { b.disabled = true; }
        else { b.onclick = function () { finish(o.key, false); }; }
        list.appendChild(b);
        btns.push(b);
      });
      el.choices.appendChild(list);

      function finish(key, timedOut) {
        if (done) return;
        done = true;
        if (tick) clearInterval(tick);
        document.removeEventListener('keydown', kh);
        var idx = -1;
        for (var i = 0; i < options.length; i++) if (options[i].key === key) idx = i;
        btns.forEach(function (b, i) {
          b.disabled = true;
          b.classList.add(i === idx ? 'picked' : 'dim');
        });
        if (timedOut) addText('시간 초과. 망설이는 사이 몸이 먼저 움직였다.', 'warn');
        setTimeout(function () { resolve(key); }, timedOut ? 800 : 220);
      }
      function kh(e) {
        var n = parseInt(e.key, 10);
        if (n >= 1 && n <= options.length && !options[n - 1].locked) finish(options[n - 1].key, false);
      }
      function update() {
        var remain = Math.max(0, (deadline - Date.now()) / 1000);
        bar.style.width = (remain / seconds * 100) + '%';
        secEl.textContent = fmt(remain);
        if (remain <= 5) bar.classList.add('urgent');
        if (remain <= 0) finish(options[def].key, true);
      }
      document.addEventListener('keydown', kh);
      if (timed) { update(); tick = setInterval(update, 100); }
      scrollToChoices();
    });
  }

  function renderCollection(container, highlight) {
    var d = loadEndings();
    var box = document.createElement('div'); box.className = 'coll';
    var got = Object.keys(d).length;
    var head = document.createElement('p'); head.className = 'head'; head.textContent = 'Ending log  ' + got + ' / ' + ORDER.length;
    box.appendChild(head);
    var grid = document.createElement('div'); grid.className = 'slots';
    ORDER.forEach(function (code) {
      var slot = document.createElement('div');
      slot.className = 'slot' + (d[code] ? ' got' : '') + (code === highlight ? ' new' : '');
      var k = document.createElement('span'); k.className = 'k'; k.textContent = ENDINGS[code].tag;
      var v = document.createElement('span'); v.className = 'v'; v.textContent = d[code] ? ENDINGS[code].name : '미발견';
      slot.appendChild(k); slot.appendChild(v);
      grid.appendChild(slot);
    });
    box.appendChild(grid);
    container.appendChild(box);
  }

  async function endingScreen(code, body) {
    var e = ENDINGS[code];
    saveEnding(code);
    hud(e.pov, '엔딩', 5);
    clearStage();
    var tg = document.createElement('p'); tg.className = 'etag'; tg.textContent = e.tag;
    var h = document.createElement('h2'); h.textContent = e.name;
    el.story.appendChild(tg); el.story.appendChild(h);
    addText(body);
    var rh = document.createElement('h3'); rh.className = 'sec-h'; rh.textContent = '당신이 지나온 길';
    el.story.appendChild(rh);
    var ul = document.createElement('ul'); ul.className = 'route';
    state.route.forEach(function (r) {
      var li = document.createElement('li'); li.textContent = r.replace(/^\[[^\]]+\]\s*/, ''); ul.appendChild(li);
    });
    el.story.appendChild(ul);
    renderCollection(el.story, code);
    await cont('처음부터 다시');
  }

  async function startScreen() {
    el.hud.hidden = true;
    root.dataset.pov = 'sys';
    state = null;
    clearStage();
    el.story.innerHTML =
      '<p class="etag">SF 디스토피아 · 추리 · 잠입 · 과학 시뮬레이션</p>' +
      '<h1>PROJECT EDEN</h1>' +
      '<p class="sub">과학기술은 인류를 자유롭게 하는가, 기술을 통제하는 자에게 새로운 권력을 주는가.</p>' +
      '<ul class="factions">' +
      '<li><span class="dot" style="background:var(--kael)"></span><div><b>제1인류</b> <span class="d">첨단 도시 에덴. 과학기술과 정치권력을 독점했다.</span></div></li>' +
      '<li><span class="dot" style="background:var(--deep)"></span><div><b>제2인류</b> <span class="d">오염 지역 노동을 위해 개량된 인류. 생체 칩으로 통제받는다.</span></div></li>' +
      '<li><span class="dot" style="background:var(--riwa)"></span><div><b>제3인류</b> <span class="d">지하 공동체. 식수, 전력, 물류가 모자란다.</span></div></li>' +
      '</ul>' +
      '<p class="etag">등장인물</p>' + castHtml() +
      '<p class="sub">카엘과 리와를 번갈아 플레이해요. 선택과 미션 결과가 쌓여 5가지 엔딩 중 하나로 이어져요.</p>' +
      '<label class="practice" for="practice"><input type="checkbox" id="practice"> 연습 모드 (선택지 제한시간 없음)</label>';
    renderCollection(el.story, null);
    await cont('게임 시작');
    practice = !!$('practice').checked;
  }

  function castHtml() {
    return '<ul class="cast">' + CAST.map(function (c) {
      return '<li class="f-' + c[2] + '"><span class="av">' + c[0].charAt(0) + '</span><span><span class="nm">' + c[0] +
        '</span><br><span class="rl">' + c[1] + '</span></span></li>';
    }).join('') + '</ul>';
  }

  function shuffle(a) {
    for (var i = a.length - 1; i > 0; i--) {
      var j = Math.floor(Math.random() * (i + 1));
      var t = a[i]; a[i] = a[j]; a[j] = t;
    }
    return a;
  }

  /* ================= 미션 공통 도구 ================= */
  function addTiles(list) {
    var box = document.createElement('div'); box.className = 'stats';
    list.forEach(function (p) {
      var s = document.createElement('div'); s.className = 'stat' + (p.cls ? ' ' + p.cls : '');
      var k = document.createElement('span'); k.className = 'k'; k.textContent = p.k;
      var v = document.createElement('span'); v.className = 'v'; v.textContent = String(p.v);
      s.appendChild(k); s.appendChild(v); box.appendChild(s);
    });
    el.story.appendChild(box);
  }
  function addStats(m, resLabel) {
    addTiles([
      { k: '중금속', v: m.metal, cls: m.metal <= 30 ? 'ok' : 'bad' },
      { k: '유기오염물', v: m.organic, cls: m.organic <= 30 ? 'ok' : 'bad' },
      { k: resLabel, v: m.resource }
    ]);
  }

  /* 점수형 판정 미션: base + 선택 점수 >= target 이면 성공, margin 이내면 부분 성공 */
  async function mission(o) {
    hud(o.pov, o.label, o.ch); clearStage();
    addHeading(o.title);
    o.intro.forEach(function (t) { addText(t); });
    var score = o.base;
    addText('현재 상태가 기본 점수 ' + o.base + '점으로 반영돼요. 목표는 ' + o.target + '점이에요.', 'sub');
    for (var i = 0; i < o.steps.length; i++) {
      var s = o.steps[i];
      addText(s.q, 'sub');
      var k = await choose(s.opts.map(function (x, j) {
        return { key: String(j), label: x.label, hint: x.hint, locked: x.lock ? x.lock() : false, lockHint: x.lockHint };
      }), 0, 0);
      var op = s.opts[parseInt(k, 10)];
      score += op.score;
      if (op.fx) op.fx();
    }
    var grade = score >= o.target ? 'full' : (o.margin && score >= o.target - o.margin ? 'partial' : 'fail');
    addText('최종 판정 ' + score + ' / 목표 ' + o.target, 'res');
    return { grade: grade, score: score };
  }

  /* 순서 배열 퍼즐: items는 올바른 순서. 눌러서 한 칸씩 배열한다. 모두 맞으면 true */
  async function orderPuzzle(prompt, items) {
    var pool = shuffle(items.slice());
    var picked = [];
    addText(prompt, 'sub');
    while (pool.length) {
      var k = await choose(pool.map(function (x, i) { return { key: String(i), label: x }; }), 0, 0);
      var one = pool.splice(parseInt(k, 10), 1)[0];
      picked.push(one);
      addText((picked.length) + '. ' + one, 'sub');
    }
    var right = 0;
    picked.forEach(function (x, i) { if (x === items[i]) right++; });
    return right === items.length;
  }

  function mh(label) { hud('riwa', '챕터 4 · ' + label, 4); clearStage(); }

  /* ---- 미션 화면 도구: 단계 막대, 슬라이더, 비커 그림 ---- */
  function addNav(list, idx) {
    var ol = document.createElement('ol'); ol.className = 'mnav';
    list.forEach(function (t, i) {
      var li = document.createElement('li');
      li.className = i < idx ? 'done' : (i === idx ? 'cur' : '');
      li.textContent = t;
      ol.appendChild(li);
    });
    el.story.appendChild(ol);
  }

  /* opts: [{key,label,tick,hint,locked,lockHint}] 단계가 있는 슬라이더. 확정하면 key를 돌려준다 */
  function pickSlider(opts, startIdx) {
    return new Promise(function (resolve) {
      el.choices.innerHTML = '';
      var cur = startIdx, done = false;
      var wrap = document.createElement('div'); wrap.className = 'slider';
      var info = document.createElement('div'); info.className = 'sl-info';
      var name = document.createElement('b');
      var hint = document.createElement('span');
      info.appendChild(name); info.appendChild(hint);
      var rng = document.createElement('input');
      rng.type = 'range'; rng.id = 'slide'; rng.min = '0'; rng.max = String(opts.length - 1); rng.step = '1';
      rng.value = String(startIdx);
      rng.setAttribute('aria-label', '값 선택');
      var ticks = document.createElement('div'); ticks.className = 'sl-ticks';
      opts.forEach(function (o) {
        var t = document.createElement('span'); t.textContent = o.tick || o.label; ticks.appendChild(t);
      });
      var ok = document.createElement('button'); ok.type = 'button'; ok.className = 'next';
      wrap.appendChild(info); wrap.appendChild(rng); wrap.appendChild(ticks); wrap.appendChild(ok);
      el.choices.appendChild(wrap);

      function sync() {
        var o = opts[cur];
        name.textContent = o.label;
        hint.textContent = o.locked ? (o.lockHint || '선택할 수 없어요') : (o.hint || '');
        ok.disabled = !!o.locked;
        ok.textContent = (o.locked ? '선택할 수 없음' : '이 값으로 정하기') + '  ▸';
      }
      function finish() {
        var o = opts[cur];
        if (done || o.locked) return;
        done = true;
        document.removeEventListener('keydown', kh);
        rng.disabled = true; ok.disabled = true;
        resolve(o.key);
      }
      function kh(e) {
        var n = parseInt(e.key, 10);
        if (n >= 1 && n <= opts.length) { cur = n - 1; rng.value = String(cur); sync(); }
        else if (e.key === 'Enter') { finish(); }
      }
      rng.addEventListener('input', function () { cur = parseInt(rng.value, 10) || 0; sync(); });
      ok.onclick = finish;
      document.addEventListener('keydown', kh);
      sync();
      scrollToChoices();
    });
  }

  /* 비커 그림: 입자 수는 오염도에 비례한다. mode: plain | mix | uv */
  function addBeaker(m, mode) {
    var dots = '';
    function put(n, fill, r, extra, base) {
      var seed = base;
      function rnd() { seed = (seed * 9301 + 49297) % 233280; return seed / 233280; }
      for (var i = 0; i < n; i++) {
        var x = 96 + rnd() * 128, y = 78 + rnd() * 78;
        dots += '<circle class="dot' + (extra || '') + '" cx="' + x.toFixed(1) + '" cy="' + y.toFixed(1) + '" r="' + r +
          '" style="fill:' + fill + ';animation-delay:' + (rnd() * 3).toFixed(2) + 's"/>';
      }
    }
    put(Math.round(m.metal / 8), 'var(--riwa)', 4.5, '', 11);
    put(Math.round(m.organic / 8), 'var(--violet)', 4, '', 23);
    if (mode === 'mix') put(9, 'var(--kael)', 2.6, ' nano', 37);
    var extra = '';
    if (mode === 'mix') {
      extra = '<rect x="240" y="96" width="26" height="40" rx="4" style="fill:var(--bad)"/>' +
        '<rect x="240" y="96" width="26" height="20" rx="4" style="fill:var(--kael)"/>';
    } else if (mode === 'uv') {
      extra = '<rect x="132" y="8" width="56" height="14" rx="7" style="fill:var(--violet)"/>' +
        '<path d="M142 26 L134 70 M160 26 L160 70 M178 26 L186 70" style="stroke:var(--violet);stroke-opacity:.55;stroke-width:2;fill:none"/>';
    }
    var box = document.createElement('div'); box.className = 'beaker';
    box.innerHTML =
      '<svg viewBox="0 0 320 190" role="img" aria-label="비커 속 입자 그림. 중금속 ' + m.metal + ', 유기오염물 ' + m.organic + '">' +
      '<path d="M86 58 L234 58 L234 152 Q234 166 220 166 L100 166 Q86 166 86 152 Z" style="fill:var(--kael);fill-opacity:.14"/>' +
      '<path d="M84 34 L84 152 Q84 168 100 168 L220 168 Q236 168 236 152 L236 34" style="fill:none;stroke:var(--muted);stroke-width:3"/>' +
      dots + extra + '</svg>' +
      '<div class="legend"><span><i style="background:var(--riwa)"></i>중금속</span>' +
      '<span><i style="background:var(--violet)"></i>유기오염물</span>' +
      (mode === 'mix' ? '<span><i style="background:var(--kael)"></i>자성 나노입자</span>' : '') + '</div>';
    el.story.appendChild(box);
  }

  /* ================= M05-N : WATER LAB (OPERATION: CLEAN WATER) ================= */
  var DOSE = { a: { n: '적게', rem: 25, cost: 10 }, b: { n: '보통', rem: 45, cost: 20 }, c: { n: '많이', rem: 58, cost: 35 } };
  var MIX = { a: { n: '짧게', rem: -12, cost: 5 }, b: { n: '보통', rem: 0, cost: 10 }, c: { n: '길게', rem: 8, cost: 20 } };
  var UV = { a: { n: '짧게', organic: 45, cost: 10 }, b: { n: '보통', organic: 25, cost: 25 }, c: { n: '길게', organic: 10, cost: 40 } };
  var MAGNET_COST = 5;
  var WL_STEPS = ['가이드', '중금속 제거', '광촉매', '최종 평가'];

  function wl(label, idx) {
    hud('riwa', '챕터 4 · ' + label, 4); clearStage();
    addNav(WL_STEPS, idx);
  }

  async function waterGuide(m) {
    wl('WATER LAB', 0);
    addHeading('OPERATION: CLEAN WATER');
    addText('에덴에서 가져온 나노화학 자료가 더 디프의 연구실에서 열린다. 『나노화학』 목차를 바탕으로 한 연구 파일이다.');
    addText('리와: "에덴의 연구 자료를 확보했어. 어디부터 살펴보는 게 좋을까?"');
    addText('마이라: "리와, 정수 시설의 물이 점점 줄어들고 있어. 먼저 식수 문제부터 해결해 줘."');
    addText('이 미션의 목표', 'etag');
    addText('① 중금속 오염도 30 이하\n② 유기오염도 30 이하\n③ 자성 나노입자 회수\n④ 연구 자원을 최대한 남기기');
    addText('시작 수치', 'etag');
    addTiles([{ k: '중금속', v: m.metal }, { k: '유기오염물', v: m.organic }, { k: '연구 자원', v: m.resource }]);
    addBeaker(m, 'plain');
    addText('모든 수치는 게임용 가상 점수예요. 실제 중금속 농도나 식수 안전 기준을 뜻하지 않아요.', 'sub');
    var CH = [
      { n: 3, t: '나노 물질을 만드는 법', h: '나노입자의 표면에 기능성 물질을 결합하는 기술' },
      { n: 4, t: '나노물질의 특성', h: '자성 나노입자를 자석으로 회수하는 원리' },
      { n: 7, t: '환경을 지키는 나노 기술', h: '킬레이트, 자성 나노입자, 광촉매 정수 기술' },
      { n: 8, t: '화학반응을 지배하다', h: '촉매와 반응 효율에 관한 개념' }
    ];
    var seen = {};
    for (;;) {
      var opts = CH.map(function (x) {
        return { key: String(x.n), label: x.n + '장 · ' + x.t, hint: seen[x.n] ? '열람함' : '눌러서 힌트 보기' };
      });
      opts.push({ key: 'x', label: '자료를 닫고 실험을 시작한다', hint: '4장과 7장은 읽어 두면 도움이 돼요' });
      var c = await choose(opts, 0, 0);
      if (c === 'x') return;
      seen[c] = true;
      var ch = CH.filter(function (x) { return String(x.n) === c; })[0];
      addText(ch.n + '장 · ' + ch.t + ': ' + ch.h, 'sub');
    }
  }

  async function waterTrial(m, t) {
    wl('중금속 제거', 1);
    addHeading('중금속 제거 · 모의 실험 ' + (t + 1) + '/2');
    addText(t === 0
      ? '첫 실험은 시험용 소량 샘플이에요. 모의 실험은 자원이 줄지 않고, 최종으로 고른 설계만 실제 시설에 적용돼요.'
      : '조건을 바꿔 한 번 더 시험해요. 이번이 마지막 모의 실험이에요.');
    addStats(m, '남은 자원');
    addBeaker(m, 'mix');
    addText('① 나노입자 투입량', 'etag');
    var dk = await pickSlider(['a', 'b', 'c'].map(function (k) {
      return { key: k, label: DOSE[k].n + ' 투입', tick: DOSE[k].n, hint: '중금속 제거 +' + DOSE[k].rem + ' · 자원 -' + DOSE[k].cost };
    }), 1);
    addText('투입량 · ' + DOSE[dk].n, 'sub');
    addText('② 혼합 시간', 'etag');
    var mk = await pickSlider(['a', 'b', 'c'].map(function (k) {
      var r = MIX[k].rem;
      return { key: k, label: MIX[k].n + ' 혼합', tick: MIX[k].n, hint: '제거량 보정 ' + (r > 0 ? '+' : '') + r + ' · 자원 -' + MIX[k].cost };
    }), 1);
    addText('혼합 시간 · ' + MIX[mk].n, 'sub');
    addText('③ 자석 회수', 'etag');
    addText('혼합이 끝났어요. 자석을 작동해 자성 나노입자를 회수해야 다음 단계로 갈 수 있어요.');
    await cont('자석 작동');
    var metal = Math.max(0, m.metal - (DOSE[dk].rem + MIX[mk].rem));
    var cost = DOSE[dk].cost + MIX[mk].cost + MAGNET_COST;
    addText('리와: "중금속을 포집한 입자들이 자석 쪽으로 모이고 있어!"', 'res');
    addStats({ metal: metal, organic: m.organic, resource: m.resource - cost }, '예상 남은 자원');
    addBeaker({ metal: metal, organic: m.organic }, 'plain');
    if (metal <= 30) addText('리와: "중금속 제거 목표를 달성했어. 다음 공정으로 넘어가자."');
    else addText('마이라: "아직 오염이 남아 있어. 투입량이나 혼합 시간을 다시 조절해 봐."');
    if (m.resource - cost < 40) addText('리와: "자원이 많이 줄었어. 다음 실험에서 전력을 아껴야겠어."');
    return { dose: DOSE[dk].n, mix: MIX[mk].n, metal: metal, cost: cost };
  }

  async function waterTrials(m) {
    var results = [];
    for (var t = 0; t < 2; t++) {
      var r = await waterTrial(m, t);
      results.push(r);
      if (t === 0) {
        var go = await choose([
          { key: 'keep', label: '이 설계로 확정한다', hint: '중금속 ' + r.metal + ' · 자원 -' + r.cost },
          { key: 'again', label: '조건을 바꿔 한 번 더 시험한다', hint: '모의 실험은 자원이 들지 않아요 (마지막 1회)' }
        ], 0, 0);
        if (go === 'keep') break;
      }
    }
    var pick = results[0];
    if (results.length === 2) {
      addText('두 번의 모의 실험 결과를 비교해 실제 시설에 적용할 설계를 골라요.', 'sub');
      var p = await choose(results.map(function (x, i) {
        return { key: String(i), label: (i + 1) + '차 설계: ' + x.dose + ' 투입 + ' + x.mix + ' 혼합', hint: '중금속 ' + x.metal + ' · 자원 -' + x.cost };
      }), 0, 0);
      pick = results[parseInt(p, 10)];
    }
    m.trial = results.length; m.metal = pick.metal; m.recovery = 1; m.resource = Math.max(0, m.resource - pick.cost);
    tag('M05-N', '나노 분리: ' + pick.dose + ' 투입 + ' + pick.mix + ' 혼합 (모의 ' + results.length + '회)');
    addText('확정한 설계를 실제 정수 시설에 적용했어요. 자원이 ' + pick.cost + ' 줄었어요.', 'res');
    await cont();
  }

  async function waterPhoto(m) {
    wl('광촉매', 2);
    addHeading('광촉매로 유기오염물 제거');
    addText('1단계에서 정한 중금속 오염도와 남은 자원을 그대로 가져왔어요. 이산화 타이타늄(TiO₂) 광촉매에 자외선을 비추는 시간을 정해 유기오염물을 분해해요.');
    addText('세린: "중금속을 제거해도 물속 유기오염물은 남아 있어. 에덴의 광촉매 연구를 이용해 보자."');
    addText('리와: "자외선을 오래 쬐면 분해가 더 진행되겠지만, 전력이 부족해질 수도 있어."');
    addStats(m, '남은 자원');
    addBeaker(m, 'uv');
    addText('목표는 유기오염도 30 이하예요. 남은 자원보다 비싼 조사 시간은 고를 수 없어요.', 'sub');
    var k = await pickSlider(['a', 'b', 'c'].map(function (key) {
      var u = UV[key];
      return { key: key, label: '자외선 ' + u.n + ' 조사', tick: u.n, hint: '유기오염도 ' + u.organic + ' · 자원 -' + u.cost,
        locked: u.cost > m.resource && key !== 'a', lockHint: '남은 자원이 부족해요 (필요 ' + u.cost + ')' };
    }), 1);
    m.organic = UV[k].organic; m.resource = Math.max(0, m.resource - UV[k].cost);
    tag('M05-N', '광촉매: 자외선 ' + UV[k].n + ' 조사');
    addText('자외선 조사 시간 · ' + UV[k].n, 'sub');
    addText('자외선 램프가 켜지고 유기오염물 수치가 내려가요.', 'res');
    addStats(m, '남은 자원');
    addBeaker(m, 'plain');
    await cont('최종 수질 검사');
  }

  async function waterVerdict(m, attempt) {
    wl('최종 평가', 3);
    addHeading('최종 평가');
    var ok = m.metal <= 30 && m.organic <= 30 && m.recovery === 1;
    var grade = !ok ? 'fail' : (m.resource >= 20 ? 'full' : 'partial');
    var label = grade === 'full' ? '완전 성공' : grade === 'partial' ? '부분 성공' : '실패';
    var pill = document.createElement('p');
    var sp = document.createElement('span');
    sp.className = 'pill ' + (grade === 'full' ? 'good' : grade === 'partial' ? 'mid' : 'bad');
    sp.textContent = label;
    pill.appendChild(sp);
    el.story.appendChild(pill);
    addStats(m, '남은 자원');
    addBeaker(m, 'plain');
    if (grade === 'full') {
      addText('마이라: "정화 목표도 달성했고 운영 자원도 남았어. 이제 우리 힘으로 시설을 유지할 수 있겠어."');
    } else if (grade === 'partial') {
      addText('세린: "정화 성능은 뛰어나지만 운영할 자원이 없어. 바렌이 지원을 제안했어."');
    } else if (m.metal > 30) {
      addText('리와: "자원은 충분히 남겼지만 중금속 제거가 부족했어. 광촉매만으로는 이 문제를 해결할 수 없어."');
    } else {
      addText('세린: "조사 시간이 부족했어. 아직 유기오염물이 남아 있어."');
    }
    addText('위 수치는 게임용 가상 점수예요. 실제 수질이나 식수 안전을 보장하지 않아요.', 'sub');
    var c = await choose([
      { key: 'go', label: '이 결과로 계속', hint: '이 결과가 이야기에 반영돼요' },
      { key: 'retry', label: '처음부터 다시 설계', hint: '자원을 되돌려 한 번 더 시도해요 (1회)', locked: attempt >= 1, lockHint: '이미 한 번 다시 설계했어요' }
    ], 0, 0);
    if (c === 'retry') return { retry: true };
    tag('M05-N', '정수 시설 판정: ' + label + ' (중금속 ' + m.metal + ' / 유기 ' + m.organic + ' / 자원 ' + m.resource + ')');
    return { grade: grade };
  }

  async function waterLab() {
    var attempt = 0;
    for (;;) {
      var m = { metal: 80, organic: 60, resource: state.startRes, recovery: 0, trial: 0 };
      if (attempt === 0) await waterGuide(m);
      await waterTrials(m);
      await waterPhoto(m);
      var r = await waterVerdict(m, attempt);
      if (r.retry) { attempt++; tag('M05-N', '처음부터 다시 설계'); continue; }
      return r.grade;
    }
  }

  /* ================= M05-E : 열전 발전 ================= */
  var HEAT = { a: { n: '깊은 지열', pw: 60, cost: 35 }, b: { n: '중간 지열', pw: 45, cost: 20 }, c: { n: '얕은 지열', pw: 30, cost: 10 } };
  var COOL = { a: { n: '강한 냉각', pw: 30, cost: 25 }, b: { n: '보통 냉각', pw: 18, cost: 15 }, c: { n: '약한 냉각', pw: 6, cost: 5 } };

  async function energyLab() {
    var res = state.startRes;
    mh('열전 발전 설계');
    addHeading('열전 발전 설계');
    addText('에덴에서 가져온 물리학 자료에는 온도 차로 전기를 만드는 열전 모듈 설계가 들어 있다.');
    addText('마이라: "지하 전력이 모자라서 의료와 정수 시설을 번갈아 끄고 있어. 발전량 70이면 모두 돌릴 수 있어."');
    addTiles([{ k: '발전량', v: 0 }, { k: '필요 발전량', v: 70 }, { k: '남은 자원', v: res }]);
    addText('① 열원 선택', 'sub');
    var hk = await choose(['a', 'b', 'c'].map(function (k) {
      return { key: k, label: HEAT[k].n, hint: '발전 +' + HEAT[k].pw + ' · 자원 -' + HEAT[k].cost, locked: HEAT[k].cost > res, lockHint: '남은 자원이 부족해요' };
    }), 0, 1);
    res -= HEAT[hk].cost;
    addText('② 냉각 장치 선택', 'sub');
    var ck = await choose(['a', 'b', 'c'].map(function (k) {
      return { key: k, label: COOL[k].n, hint: '발전 +' + COOL[k].pw + ' · 자원 -' + COOL[k].cost, locked: COOL[k].cost > res, lockHint: '남은 자원이 부족해요' };
    }), 0, 1);
    res -= COOL[ck].cost;
    var power = HEAT[hk].pw + COOL[ck].pw;
    tag('M05-E', '열전 발전: ' + HEAT[hk].n + ' + ' + COOL[ck].n);
    addText('모듈이 온도 차를 전기로 바꾸기 시작한다.', 'res');
    addTiles([{ k: '발전량', v: power, cls: power >= 70 ? 'ok' : 'bad' }, { k: '필요 발전량', v: 70 }, { k: '남은 자원', v: res }]);
    if (power >= 50) {
      addText('③ 전력 분배', 'sub');
      var ak = await choose([
        { key: 'a', label: '의료와 정수에 우선 배분한다', hint: '주민 생존 안정성이 오른다' },
        { key: 'b', label: '균형 있게 나눠 배분한다', hint: '모두에게서 신뢰를 얻는다' },
        { key: 'c', label: '방어 시설에 우선 배분한다', hint: '추적 위험이 내려간다' }
      ], 0, 1);
      if (ak === 'a') chg('stability', 1); else if (ak === 'b') chg('trust', 1); else chg('risk', -1);
      tag('M05-E', '전력 분배: ' + (ak === 'a' ? '의료·정수 우선' : ak === 'b' ? '균형 분배' : '방어 우선'));
    }
    var grade = power >= 70 && res >= 20 ? 'full' : (power >= 50 ? 'partial' : 'fail');
    if (grade === 'full') { addStatus(true, '완전 성공'); addText('세린: "전력이 남아. 이제 시설을 쉬지 않고 돌릴 수 있어."'); }
    else if (grade === 'partial') { addText('부분 성공', 'status'); addText('세린: "불은 켜졌지만 유지할 자원이 빠듯해. 바렌이 지원을 제안했어."'); }
    else { addStatus(false, '실패'); addText('리와: "발전량이 모자라. 시설은 여전히 불안정해."'); }
    await cont();
    return grade;
  }

  /* ================= M05-S : 지하망 설계 ================= */
  var TROUTE = { a: { n: '직선 단거리 터널', dist: 20, risk: 45, cost: 20 }, b: { n: '우회 안전 터널', dist: 45, risk: 15, cost: 30 }, c: { n: '입체 연결 (고층 갱도)', dist: 30, risk: 25, cost: 45 } };
  var BRACE = { a: { n: '보강 없음', risk: 0, cost: 0 }, b: { n: '부분 보강', risk: -10, cost: 15 }, c: { n: '전면 보강', risk: -25, cost: 35 } };

  async function spaceLab() {
    var res = state.startRes;
    mh('지하망 설계');
    addHeading('지하망 설계');
    addText('에덴에서 가져온 공간기하학 자료로 2D 지도와 높이 정보를 겹쳐 3D 터널망을 설계한다.');
    addText('마이라: "물류와 피난로가 끊겨 있어. 이동 거리는 40 이하, 붕괴 위험은 30 이하로 맞춰 줘."');
    addTiles([{ k: '이동 거리', v: '-' }, { k: '붕괴 위험', v: '-' }, { k: '남은 자원', v: res }]);
    addText('① 터널 경로', 'sub');
    var rk = await choose(['a', 'b', 'c'].map(function (k) {
      var r = TROUTE[k];
      return { key: k, label: r.n, hint: '이동 거리 ' + r.dist + ' · 붕괴 위험 ' + r.risk + ' · 자원 -' + r.cost, locked: r.cost > res, lockHint: '남은 자원이 부족해요' };
    }), 0, 0);
    res -= TROUTE[rk].cost;
    addText('② 자재 보강', 'sub');
    var bk = await choose(['a', 'b', 'c'].map(function (k) {
      var b = BRACE[k];
      return { key: k, label: b.n, hint: '붕괴 위험 ' + b.risk + ' · 자원 -' + b.cost, locked: b.cost > res, lockHint: '남은 자원이 부족해요' };
    }), 0, 0);
    res -= BRACE[bk].cost;
    var dist = TROUTE[rk].dist;
    var collapse = Math.max(0, TROUTE[rk].risk + BRACE[bk].risk);
    tag('M05-S', '지하망: ' + TROUTE[rk].n + ' + ' + BRACE[bk].n);
    addText('터널이 이어진다. 갱도 벽이 하중을 견디는지 시뮬레이터가 계산한다.', 'res');
    addTiles([{ k: '이동 거리', v: dist, cls: dist <= 40 ? 'ok' : 'bad' }, { k: '붕괴 위험', v: collapse, cls: collapse <= 30 ? 'ok' : 'bad' }, { k: '남은 자원', v: res }]);
    var grade = collapse <= 30 && dist <= 40 && res >= 20 ? 'full' : (collapse < 45 ? 'partial' : 'fail');
    if (grade === 'full') { addStatus(true, '완전 성공'); addText('마이라: "물류도 피난로도 열렸어. 갱도 유지비도 감당할 만해."'); }
    else if (grade === 'partial') { addText('부분 성공', 'status'); addText('세린: "길은 이어졌지만 보수비가 계속 들어. 바렌이 지원을 제안했어."'); }
    else { addStatus(false, '실패'); addText('리와: "갱도가 불안해. 언제 무너져도 이상하지 않아."'); }
    await cont();
    return grade;
  }

  /* ================= 프롤로그 ================= */
  async function prologue() {
    hud('sys', '프롤로그 · 세 인류의 역사와 갈등', 0); clearStage();
    addHeading('세 인류');
    addText('먼 과거, 인류는 에덴이라는 빛의 도시를 세웠다. 제1인류는 과학기술과 정치권력을 독점했고, 오염 지역의 노동은 유전적으로 개량된 제2인류에게 맡겼다. 제2인류는 생체 칩으로 통제받았다.');
    addText('세월이 흘러 제2인류의 후손은 지하로 내려가 독립 공동체를 이뤘다. 제3인류다. 그들은 식수와 전력과 물류가 모자라 서서히 줄어들고 있다.');
    addText('제2인류 급진파는 에덴으로부터의 해방을 준비한다. 그리고 어느 밤, 연구소가 침입당한다.');
    await cont('시작');
  }

  /* ================= 챕터 1 · 카엘 ================= */
  async function ch1() {
    hud('kael', '챕터 1 · 연구소 침입 조사', 1); clearStage();
    addHeading('연구소 침입');
    addText('새벽. Central Lab 보안 구역에서 데이터 유출이 확인되었다. 에덴 보안수사관 카엘은 동료 이안과 함께 현장에 도착한다.');
    addText('이안: "시스템은 내부 오류라고 하는데, 기록이 이상해. 일단 로그부터 맞춰 보자."');
    addText('카엘: "내부 오류라면 로그가 이렇게 깔끔할 리 없어. 누가 지운 거야."');
    addText('M01 · 보안 기록 수사', 'etag');
    addText('이안이 단말에 수사 메모를 띄운다. 기록에는 시각이 지워져 있다.', 'sub');
    addText('· 노아는 교대 직후 가장 먼저 정비 구역에 들어왔다.\n· 카메라가 멈춘 것은 서버룸 접근 직전이다.\n· 덕트 경보는 침입자가 빠져나갈 때 울렸다.', 'sub');
    var solved = await orderPuzzle('메모를 단서로 일어난 순서를 추리해 하나씩 눌러 배열하세요.', [
      '정비 구역 출입 · 제2인류 작업자 노아',
      '보안 카메라 정지',
      '서버룸 접근 · 연구원 신분 카드 사용',
      '환기 덕트 센서 경보'
    ]);
    if (solved) {
      state.aidenSus = true; chg('truth', 1);
      tag('M01', '보안 기록 배열 성공: 에이든을 의심');
      addText('기록이 맞물리자 모순이 드러난다. 서버룸에 쓰인 카드의 주인은 이미 퇴근 처리된 연구원 에이든이다.', 'res');
      addText('이안: "에이든이라고? 보안국에서도 평판이 좋은 연구원인데."');
      addText('카엘: "그래서 더 이상한 거야."');
    } else {
      tag('M01', '보안 기록 배열 실패');
      addText('순서가 맞지 않아 결정적인 모순을 놓쳤다. 단서가 흐릿하다.', 'res');
      addText('이안: "순서가 안 맞잖아. 다시 보고 싶어도 시간이 없어."');
      addText('카엘: "...일단 현장부터 보자."');
    }
    await cont();

    clearStage(); hud('kael', '챕터 1 · 노동자를 구할 것인가?', 1);
    addHeading('K04 · 노동자를 구할 것인가?');
    addText('그때 경보가 울린다. 정비 구역에서 격벽이 떨어져 제2인류 노동자 노아가 그 아래에 깔렸다. 같은 순간 이안이 외친다.');
    addText('이안: "침입자가 외곽 통로로 도망쳤어. 지금 쫓아야 해!"');
    addText('노아: "으윽... 살려 주세요... 다리가 안 움직여요."');
    var c = await choose([
      { key: 'save', label: '노아를 구한다', hint: '침입자를 놓칠 수 있다' },
      { key: 'chase', label: '침입자를 쫓는다', hint: '노아를 두고 간다' }
    ], 25, 1);
    if (c === 'save') {
      state.noa = 'rescued'; chg('trust', 1);
      tag('W1', '노아를 구조, 노동자 신뢰 획득');
      addText('카엘은 격벽을 들어 올려 노아를 끌어낸다. 노아는 숨을 몰아쉬며 카엘의 얼굴을 기억에 새긴다. 침입자는 이미 멀리 사라졌다.', 'res');
      addText('노아: "당신... 보안국 사람이잖아요. 왜 나를..."');
      addText('카엘: "움직이지 마. 지금은 살아 있는 게 먼저야."');
    } else {
      state.noa = 'injured'; chg('truth', 1); chg('risk', 1); chg('trust', -1);
      tag('W2', '침입 흔적 확보, 노아 부상과 불신');
      addText('카엘은 노아를 뒤로하고 외곽 통로까지 침입자의 흔적을 쫓는다. 결정적인 흔적은 얻었지만, 뒤에서 노아의 신음이 들린다.', 'res');
      addText('이안: "놓치지 마, 카엘! 저쪽이야!"');
      addText('노아: "(신음) 가지... 마요..."');
    }
    addText('레온: "카엘, 현장 보고는 내게 직접 올려라. 외부에는 단순 오류로 발표할 거다."');
    addText('카엘: "...알겠습니다, 국장님."');
    await cont();
  }

  /* ================= 챕터 2 · 리와 ================= */
  async function ch2() {
    hud('riwa', '챕터 2 · 연구소 잠입', 2); clearStage();
    addHeading('잠입');
    addText('같은 시각. 리와는 인간으로 위장해 Central Lab 최심부로 향한다. 연구원 에이든과 제2인류 세린이 그녀의 잠입을 돕고 있다.');
    addText('에이든: "내 출입 권한은 02:10까지만 유효해. 서둘러."');
    addText('세린: "정비 통로 센서는 내가 5분간 꺼 둘게. 그 이상은 장담 못 해."');
    addText('리와: "알았어. 5분이면 충분해."');
    addText('M02 · 연구소 잠입', 'etag');
    var r = await choose([
      { key: 'a', label: '세린이 꺼 둔 정비 통로로 들어간다', hint: '조용하지만 5분 안에 끝내야 한다' },
      { key: 'b', label: '환기 덕트를 직접 해킹한다', hint: '빠르지만 흔적이 남는다' },
      { key: 'c', label: '에이든의 권한으로 정문을 통과한다', hint: '안전하지만 에이든의 이름이 남는다' }
    ], 20, 1);
    if (r === 'a') { chg('trust', 1); tag('M02', '세린의 정비 통로로 잠입'); addText('센서가 꺼진 5분 동안 리와는 그림자처럼 복도를 지난다.', 'res'); addText('세린: "3분 남았어. 서둘러, 리와."'); }
    else if (r === 'b') { chg('risk', 1); tag('M02', '환기 덕트를 해킹해 잠입'); addText('덕트 안에서 센서를 하나씩 속여 가며 전진한다. 빠르지만 흔적이 곳곳에 남는다.', 'res'); addText('세린: "덕트는 위험하다고 했잖아! 흔적이 남을 거야."'); }
    else { tag('M02', '에이든의 권한으로 정문 통과'); addText('게이트가 초록빛으로 바뀐다. 에이든의 권한 기록이 서버에 남는다.', 'res'); addText('에이든: "내 이름이 남겠지만... 상관없어. 가."'); }
    await cont();

    clearStage(); hud('riwa', '챕터 2 · R03 연구 자료 선택', 2);
    addHeading('R03 · 연구 자료 선택');
    addText('연구소 최심부. 서버룸의 냉기가 폐 속까지 스며든다. 한 번에 한 종류의 자료만 가져갈 수 있다. 이 선택이 지하문명이 개발할 기술을 정한다.');
    var d = await choose([
      { key: 'NANO', label: '나노화학', hint: '막과 입자로 물을 정화하는 기술' },
      { key: 'ENERGY', label: '물리학', hint: '온도 차로 전력을 만드는 열전 발전' },
      { key: 'SPACE', label: '공간기하학', hint: '3D 지하 터널과 피난망 설계' }
    ], 180, 0);
    state.data = d;
    var dn = { NANO: '나노화학', ENERGY: '물리학', SPACE: '공간기하학' };
    tag('R03', '연구 자료 선택: ' + dn[d]);
    addText(dn[d] + ' 자료가 리와의 손에 들어왔다. 되돌릴 수는 없다.', 'res');
    var ad = { NANO: '나노화학 자료군이군. 물을 다루는 사람들에겐 생명줄이야.', ENERGY: '열전 발전이라... 지하의 전력난을 알고 있었나 보군.', SPACE: '공간기하학 자료라니. 지하를 아는 사람의 선택이야.' };
    addText('에이든: "' + ad[d] + '"');
    await cont();

    clearStage(); hud('riwa', '챕터 2 · R05 에이든을 보호할까?', 2);
    addHeading('R05 · 에이든을 보호할까?');
    addText('경보가 울린다. 서버룸 카메라가 에이든의 얼굴을 잡았다. 보안 요원들이 그의 연구실로 달려가고 있다.');
    addText('에이든: "리와, 나는 괜찮아! 먼저 가!"');
    var a = await choose([
      { key: 'hide', label: '추적을 끌어 에이든이 숨을 시간을 번다', hint: '위험이 커지지만 내부 협력자를 지킨다' },
      { key: 'leave', label: '에이든을 두고 탈출한다', hint: '혼자 빠져나가는 편이 안전하다' }
    ], 25, 1);
    if (a === 'hide') {
      state.aiden = 'hidden'; chg('trust', 1); chg('risk', 1);
      tag('A1', '에이든 은신, 내부 협력자 유지');
      addText('리와는 일부러 반대쪽 센서를 건드려 요원들을 끌어낸다. 에이든은 그 틈에 지하 창고로 몸을 숨긴다.', 'res');
      addText('에이든: "고마워... 이 빚은 꼭 갚을게."');
    } else {
      state.aiden = 'arrested'; chg('trust', -1);
      tag('A2', '에이든 체포, 보안국 조사');
      addText('리와는 뒤돌아보지 않는다. 에이든이 보안 요원들에게 붙잡히는 소리가 복도에 울린다.', 'res');
      addText('에이든: "리와...! 기다려!"');
    }
    await cont();

    clearStage(); hud('riwa', '챕터 2 · M03 연구소 탈출', 2);
    addHeading('M03 · 연구소 탈출');
    addText('입체 통로 지도를 펼친다. 층마다 높이와 연결이 다르다. 어디로 빠져나갈 것인가?');
    addText('세린: "하층 배수로는 내가 수문을 열어 둘게. 어느 쪽이든 빨리 정해."');
    var e = await choose([
      { key: 'up', label: '환기 샤프트 상층으로 오른다 (+12m)', hint: '빠르지만 열 감지에 노출된다' },
      { key: 'down', label: '배수 통로 하층으로 내려간다 (-8m)', hint: '느리지만 조용하다' },
      { key: 'lift', label: '정비 승강기로 직결한다', hint: '에이든이 살아 있을 때만 쓸 수 있다',
        locked: state.aiden !== 'hidden', lockHint: '에이든의 권한이 있어야 열려요' }
    ], 20, 1);
    if (e === 'up') { chg('risk', 1); tag('M03', '상층 샤프트로 탈출'); addText('샤프트를 기어올라 옥상 환풍구로 나온다. 열 감지 드론이 한 번 스쳐 간다.', 'res'); }
    else if (e === 'down') { tag('M03', '하층 배수 통로로 탈출'); addText('물이 허리까지 차오르는 통로를 지나 외곽 배수구로 빠져나온다.', 'res'); }
    else { chg('risk', -1); tag('M03', '정비 승강기로 탈출'); addText('에이든이 남겨 둔 권한으로 승강기가 열린다. 리와는 추적 요원들을 지나쳐 지상으로 나온다.', 'res'); }
    await cont();
  }

  /* ================= 챕터 3 · 카엘 ================= */
  async function ch3() {
    if (state.aiden === 'hidden') await ch3a(); else await ch3b();
    hud('kael', '챕터 3 · 지하문명의 위치', 3);
    addHeading('지하문명의 위치');
    var lab ={ SAFE: 'P2-SAFE · 지하문명의 위치가 은폐됨', WATCH: 'P2-WATCH · 에덴이 지하문명을 추적 중', EXPOSE: 'P2-EXPOSE · 지하문명의 위치가 노출됨' };
    addText(lab[state.p2], 'status');
    if (state.p2 === 'SAFE') addText('이안: "지하문명의 위치는 아직 아무도 몰라. 하지만 오래는 못 갈 거야."');
    else if (state.p2 === 'WATCH') addText('레온: "지하 어딘가 있다. 수색 범위를 넓혀라. 놓치지 마."');
    else addText('레온: "찾았다. 지하문명에 봉쇄령을 내린다."');
    tag('P2-' + state.p2, lab[state.p2].split(' · ')[1]);
    await cont('챕터 4로');
  }

  async function ch3a() {
    hud('kael', '챕터 3 · 카엘의 비밀 수사', 3); clearStage();
    addHeading('비밀 수사');
    addText('침입자는 사라졌지만 카엘은 보고서가 마음에 걸린다. 생체 칩 개발 기록이 연구소 서버에서 통째로 지워져 있었고, 카드의 주인 에이든은 그날 이후 이상하리만치 침착하다.');
    addText('이안: "보안국장 레온은 이 사건을 빨리 덮고 싶어 해. 그래도 계속 파 볼 거야?"');
    addText('카엘: "덮이는 사건일수록 이유가 있어. 에이든부터 만나 볼게."');
    var h = await choose([
      { key: 'honest', label: '신분을 밝히고 에이든과 직접 대화한다', hint: '신뢰를 얻을 수 있지만 위험하다' },
      { key: 'secret', label: '에이든 몰래 연구실 기록을 열람한다', hint: '증거는 얻지만 신뢰는 얻지 못한다' }
    ], 25, 1);
    var honest = h === 'honest';
    if (honest) {
      addText('카엘: "에이든 연구원, 그날 서버룸 카드에 대해 묻고 싶습니다."');
      addText('에이든: "질문을 하러 오신 겁니까, 진실을 들으러 오신 겁니까?"');
    }
    if (!honest) chg('truth', 1);
    var score = (state.noa === 'rescued' ? 1 : 0) + (honest ? 1 : 0) + (state.aidenSus ? 0 : 1);
    if (score >= 2) {
      tag('CH3-A', '에이든이 카엘을 신뢰, 생체 칩 자료 제공');
      chg('truth', 2); chg('trust', 1);
      addText('에이든은 한참 카엘을 바라본다. 그리고 낡은 저장 장치를 내민다. 제2인류의 생체 칩은 노동 효율과 복종을 위해 설계되었다는 기록이다.', 'res');
      addText('에이든: "' + (state.noa === 'rescued' ? '노아를 구한 사람이라는 건 들었소.' : '당신은 정직하게 찾아왔소.') + ' 그러니 말하죠. 이건 내가 평생 후회해 온 일이오."');
    } else {
      tag('CH3-A', '에이든이 카엘을 불신, 삭제 기록 복원 시도');
      addText('에이든은 입을 열지 않는다. 카엘은 삭제된 문서 조각을 복원해 직접 진실을 추론해야 한다.', 'res');
      addText('에이든: "나는 할 말이 없소. 돌아가시오."');
      addText('이안: "카엘, 레온 국장이 네 위치를 묻고 있어."');
      await cont();
      clearStage(); hud('kael', '챕터 3 · M04 역사 기록 복원', 3);
      addHeading('M04 · 역사 기록 복원');
      var ok = await orderPuzzle('삭제된 문서 조각을 시간 순서대로 이어 붙이세요.', [
        '칩 개발 승인 · 노동 효율 개선 프로젝트',
        '시험 단계 · 복종 신호 기능 추가',
        '보안국장 레온의 서명 · 은폐 지시',
        '삭제 명령 · 지하 이주 기록 포함'
      ]);
      if (ok) { chg('truth', 2); tag('M04', '삭제 기록 복원 성공'); addText('조각이 맞춰지자 진실이 드러난다. 에덴은 칩으로 제2인류를 통제했고, 그 기록을 지우라고 지시했다.', 'res'); }
      else { chg('truth', 1); tag('M04', '삭제 기록 일부만 복원'); addText('일부만 복원됐다. 은폐 지시가 있었다는 사실만은 분명하다.', 'res'); }
    }
    state.p2 = 'SAFE';
    await cont();
    clearStage(); hud('kael', '챕터 3 · 카엘의 비밀 수사', 3);
  }

  async function ch3b() {
    hud('kael', '챕터 3 · 에이든 조사실', 3); clearStage();
    addHeading('에이든 조사실');
    addText('보안국에 체포된 에이든이 조사실에 앉아 있다. 보안국장 레온이 다가온다.');
    addText('레온: "카엘, 이 자는 내가 직접 조사하지. 자네는 보고서만 올리게."');
    addText('에이든: "카엘 수사관... 그 기록은 지워져선 안 되는 것이었습니다. 제발 들어 주세요."');
    addHeading('K06 · 카엘이 에이든을 보호하는가?');
    var k = await choose([
      { key: 'protect', label: '정식 절차를 요구해 에이든의 진술을 직접 받는다', hint: '레온과 충돌한다' },
      { key: 'hand', label: '에이든을 레온에게 인계한다', hint: '조직의 명령을 따른다' }
    ], 25, 1);
    if (k === 'protect') {
      state.p2 = 'SAFE'; chg('truth', 1); chg('trust', 1);
      tag('K06', '카엘이 에이든을 보호, 진술 확보');
      addText('카엘은 레온 앞을 가로막고 정식 절차를 요구한다. 에이든은 지하문명의 위치는 끝내 말하지 않았지만, 생체 칩의 진실을 털어놓는다. 정보는 새지 않았다.', 'res');
      addText('에이든: "고맙소. 하지만 레온은 멈추지 않을 거요."');
      addText('레온: "카엘, 이건 명백한 월권이다. 기억해 두지."');
    } else {
      state.leon = true; chg('risk', 1);
      tag('K06', '에이든을 레온에게 인계, 강압 조사');
      addText('레온의 강압 조사가 이어진다. 에이든은 끝내 입을 열고, 지하로 향하는 보급 경로가 보안국에 새어 나간다.', 'res');
      addText('레온: "현명한 판단이군, 카엘."');
      addText('에이든: "카엘... 당신도 결국 규정을 택하는군."');
      addText('이안: "카엘, 이건 규정이야. 어쩔 수 없었어."');
      await cont();
      clearStage(); hud('kael', '챕터 3 · 보안국 추적', 3);
      addHeading('보안국 추적 판정');
      addText('보급 경로를 따라 보안국의 추적이 시작된다. 이제까지의 흔적이 쌓인 만큼 추적은 정확해진다.');
      addText('추적·공격 위험 ' + state.risk + ' / 성공 기준 3', 'sub');
      if (state.risk >= 3) { state.p2 = 'EXPOSE'; addStatus(false, '추적 성공'); addText('보안국은 지하문명의 입구를 찾아냈다.', 'res'); addText('이안: "카엘, 입구가 열렸어. 국장님이 봉쇄령을 준비 중이야."'); }
      else { state.p2 = 'WATCH'; addStatus(true, '추적 실패'); addText('보안국은 정확한 위치를 찾지 못했지만 주변 지역을 수색하고 있다.', 'res'); }
    }
    await cont();
    clearStage(); hud('kael', '챕터 3 · 에이든 조사실', 3);
  }

  /* ================= 챕터 4 · 리와 : 기술 개발 ================= */
  var F_INFO = {
    F4: { t: '독립 운영 · 자율성 상승', s: '제3인류가 시설을 스스로 운영하기 시작한다. 서툴지만 누구의 허락도 필요 없다.\n마이라: "스스로 운영해 보자. 틀려도 우리가 책임질 거야."', fx: function () { chg('autonomy', 2); } },
    F5: { t: '공동 운영 준비 · 협상 조건 확보', s: '세 집단이 시설을 함께 운영하기 위한 조건을 두고 마주 앉을 준비가 됐다.\n세린: "마주 앉을 자리는 마련했어. 이제 서로 믿는 일만 남았지."', fx: function () { chg('trust', 1); chg('autonomy', 1); } },
    F6: { t: '급진파 통제력 증가', s: '시설의 열쇠가 바렌의 손으로 넘어간다. 해방은 약속되었지만 기술을 쥔 자는 바뀌었다.\n바렌: "이제 이 시설의 열쇠는 우리 것이다."\n세린: "...이럴 줄 알았어. 바렌."', fx: function () { chg('support', 10); chg('autonomy', -2); } },
    F7: { t: '조건부 이양 · 권한 회수 가능', s: '운영권은 급진파가 쥐었지만 감사권과 철회 조항이 남았다. 언제든 되찾을 길이 있다.\n세린: "철회 조항은 지켜 냈어. 바렌도 쉽게 움직이진 못할 거야."', fx: function () { chg('truth', 1); } },
    F8: { t: '에덴에 의존 · 외부 통제 위험', s: '에덴의 자재와 인력이 시설을 돌린다. 그 대가로 지하의 모든 것이 에덴의 감시를 받는다.\n레온: "에덴의 지원에는 조건이 있다. 지하 전체를 우리 감시 아래 둔다."', fx: function () { chg('autonomy', -2); chg('risk', 1); } },
    F9: { t: '생존 위기 심화', s: '복구는 실패했다. 정수와 전력이 불안정해지고 주민들의 불안이 커진다.\n마이라: "정수도 전력도 불안해. 사람들이 흔들리고 있어."', fx: function () { chg('stability', -2); chg('risk', 1); } }
  };
  async function fstate(id) {
    var f = F_INFO[id];
    hud('riwa', '챕터 4 · ' + id, 4); clearStage();
    addHeading(id + ' · ' + f.t);
    addText(f.s);
    f.fx();
    tag(id, f.t);
    state.f = id;
    await cont('챕터 5로');
  }
  async function stepNote(id, title, text, fx) {
    hud('riwa', '챕터 4 · ' + id, 4); clearStage();
    addHeading(id + ' · ' + title);
    addText(text);
    if (fx) fx();
    tag(id, title);
    await cont();
  }

  async function ch4() {
    var base = { SAFE: 100, WATCH: 90, EXPOSE: 80 };
    state.startRes = base[state.p2];
    hud('riwa', '챕터 4 · 기술 개발', 4); clearStage();
    if (state.p2 === 'SAFE') {
      addHeading('주민 생존 기반 개선');
      addText('지하문명의 위치는 아직 숨겨져 있다. 마이라는 이 시간을 주민의 생존 기반을 고치는 데 쓰자고 말한다.');
      addText('마이라: "기술은 우리의 것이어야 해. 누구도 이 시설을 대신 운영해선 안 돼."');
      tag('CH4-A', '평화로운 기술 개발: 주민 생존 기반 개선 미션');
    } else if (state.p2 === 'WATCH') {
      addHeading('추적 교란과 연구의 병행');
      addText('에덴이 지하문명을 추적하고 있다. 연구를 계속하려면 추적을 따돌려야 한다.');
      var w = await choose([
        { key: 'a', label: '추적 신호를 교란한다', hint: '자원 -10 · 추적·공격 위험 -1' },
        { key: 'b', label: '연구에 집중한다', hint: '자원을 아끼지만 추적·공격 위험 +1' },
        { key: 'c', label: '가짜 연구소를 지상에 설치한다', hint: '자원 -15 · 추적·공격 위험 -2' }
      ], 0, 1);
      if (w === 'a') { state.startRes -= 10; chg('risk', -1); }
      else if (w === 'b') { chg('risk', 1); }
      else { state.startRes -= 15; chg('risk', -2); }
      tag('CH4-B', '은폐·추적 회피: 추적 교란과 연구 병행');
      addText('작업장 불빛을 줄이고 연구를 이어 간다.', 'res');
    } else {
      addHeading('긴급 방어와 피난');
      addText('지하문명의 위치가 노출됐다. 에덴의 봉쇄가 시작됐고 주민들은 피난을 준비한다. 연구는 이 위기 속에서 해야 한다.');
      var x = await choose([
        { key: 'a', label: '입구 방어를 강화한다', hint: '자원 -15 · 추적·공격 위험 -1 · 안정성 +1' },
        { key: 'b', label: '피난로를 확보한다', hint: '자원 -10 · 안정성 +1' },
        { key: 'c', label: '노아에게 에덴 내부의 도움을 청한다', hint: '노아를 구했을 때만 가능 · 신뢰 +1 · 자원 -5',
          locked: state.noa !== 'rescued', lockHint: '노아를 구했을 때만 열려요' }
      ], 0, 0);
      if (x === 'a') { state.startRes -= 15; chg('risk', -1); chg('stability', 1); }
      else if (x === 'b') { state.startRes -= 10; chg('stability', 1); }
      else { state.startRes -= 5; chg('trust', 1); }
      tag('CH4-C', '봉쇄·피난 위기: 긴급 방어·피난 미션');
      addText('방어선이 갖춰지는 동안 연구실의 불은 꺼지지 않는다.', 'res');
    }
    addText('연구에 쓸 수 있는 자원: ' + state.startRes, 'sub');
    await cont();

    clearStage(); hud('riwa', '챕터 4 · 세린의 의심', 4);
    addHeading('세린의 의심');
    addText('세린이 연구실 구석으로 리와를 불러낸다.');
    addText('세린: "바렌의 해방 계획 말이야. 기술이 완성되면 운영권을 급진파가 가져가겠다고 하더라. \'프로메테우스 계획\'이라는 문서를 봤어."');
    var s = await choose([
      { key: 'copy', label: '계획 문서를 몰래 복사해 둔다', hint: '추적·공격 위험 +1 · 훗날 증거가 된다' },
      { key: 'skip', label: '지금은 기술 개발에 집중한다', hint: '급진파와의 관계를 유지한다' }
    ], 25, 1);
    if (s === 'copy') { state.serin = true; chg('risk', 1); chg('truth', 1); tag('CH4', '세린과 함께 프로메테우스 계획 문서를 확보'); addText('세린이 건넨 저장 장치에는 바렌의 계획이 고스란히 담겨 있다.', 'res'); }
    else { tag('CH4', '세린의 의심을 보류'); addText('리와는 고개를 끄덕이지만 아무것도 받지 않는다. 지금은 기술이 먼저다.', 'res'); }
    await cont();

    var grade = state.data === 'NANO' ? await waterLab() : (state.data === 'ENERGY' ? await energyLab() : await spaceLab());
    state.grade = grade;
    if (grade === 'full') { chg('tech', 3); chg('stability', 1); }
    else if (grade === 'partial') { chg('tech', 2); }
    else { chg('stability', -1); }
    await afterTech();
  }

  async function afterTech() {
    var g = state.grade;
    if (g === 'full') {
      await stepNote('F1', '기술 자립, 유지 비용 안정', '기술은 안정적으로 돌아간다. 주민들은 리와를 믿고, 급진파에 대한 기대는 줄어든다.', function () { chg('trust', 1); chg('support', -10); });
      await fstate(await operationRights());
    } else if (g === 'partial') {
      await stepNote('F2', '기술 확보, 유지 비용 급증', '기술은 손에 넣었지만 유지비가 감당하기 어려울 만큼 늘었다. 바렌이 유지비 지원을 제안한다.', null);
      hud('riwa', '챕터 4 · 급진파 지지율', 4); clearStage();
      addHeading('급진파 지지율');
      addText('바렌의 유지비 지원 제안 · 지지율 +15');
      chg('support', 15);
      if (state.p2 === 'EXPOSE') { chg('support', 10); addText('위기 속에서 급진파의 무력에 기대는 주민이 늘었다. · +10', 'sub'); }
      if (state.noa === 'rescued') { chg('support', -5); addText('노아가 에덴 내부에도 사람이 있다고 알렸다. · -5', 'sub'); }
      if (state.truth >= 3) { chg('support', -5); addText('밝혀진 진실이 주민들을 설득했다. · -5', 'sub'); }
      if (state.trust >= 4) { chg('support', -5); addText('집단 간 신뢰가 주민들의 불안을 낮췄다. · -5', 'sub'); }
      addText('급진파 지지율 ' + state.support + ' / 기준 60', 'res');
      if (state.support < 60) {
        addStatus(true, '기준 미만');
        await cont();
        await fstate(await operationRights());
      } else {
        addStatus(false, '기준 이상');
        tag('F2', '급진파 지지율 60 이상');
        addText('운영권 이양을 거부하기 어려워졌다. 주민 의회 특별회의가 열린다.', 'sub');
        await cont();
        await specialCouncil();
      }
    } else {
      await stepNote('F3', '시설 불안정, 생존 위기', '시설이 불안정하다. 주민들의 불안이 커지고, 지하 곳곳에서 긴급 지원을 요청한다.', function () { chg('stability', -1); });
      hud('riwa', '챕터 4 · 긴급 지원', 4); clearStage();
      addHeading('긴급 지원 선택');
      addText('이대로는 오래 버티지 못한다. 어디에 손을 내밀 것인가.');
      var a = await choose([
        { key: 'self', label: '주민들과 함께 시설을 직접 복구한다', hint: '복구 미니게임 · 성공하면 길이 열린다' },
        { key: 'eden', label: '에덴의 지원을 받아들인다', hint: '빠르지만 에덴의 통제를 받는다' },
        { key: 'radical', label: '급진파의 지원을 받아들인다', hint: '빠르지만 운영권이 넘어간다' }
      ], 30, 0);
      if (a === 'eden') { tag('F3', '에덴의 지원을 선택'); await fstate('F8'); }
      else if (a === 'radical') { tag('F3', '급진파의 지원을 선택'); await fstate('F6'); }
      else {
        tag('F3', '자체 복구를 선택');
        var r = await mission({
          pov: 'riwa', label: '챕터 4 · 시설 복구', ch: 4, title: '시설 복구 미니게임',
          intro: ['정수 시설의 막이 갈라졌고 전력 선로가 끊겼다. 주민들은 지친 얼굴로 리와의 지시를 기다린다.'],
          base: state.stability + state.tech, target: 7, margin: 0,
          steps: [
            { q: '① 복구 우선순위', opts: [
              { label: '식수 시설부터 고친다', hint: '주민 생존이 우선', score: 3 },
              { label: '전력 선로부터 잇는다', hint: '다른 설비를 돌릴 수 있다', score: 2 },
              { label: '갱도 보강부터 한다', hint: '붕괴를 막는다', score: 2 }] },
            { q: '② 인력 동원', opts: [
              { label: '주민 총동원', hint: '신뢰가 쌓여 있어야 따라 준다', score: state.trust >= 3 ? 4 : 1 },
              { label: '숙련자만 소수 정예로', hint: '안정적이지만 느리다', score: 2 },
              { label: '세린의 급진파 기술자를 부른다', hint: '효율이 좋다', score: 3 }] }
          ]
        });
        if (r.grade === 'full') { addStatus(true, '복구 성공'); tag('F3', '복구 성공'); await cont(); await fstate('F7'); }
        else { addStatus(false, '복구 실패'); tag('F3', '복구 실패'); await cont(); await fstate('F9'); }
      }
    }
  }

  async function operationRights() {
    hud('riwa', '챕터 4 · 운영권 결정', 4); clearStage();
    addHeading('운영권 결정');
    addText('시설의 운영권을 누구에게 맡길 것인가. 이 결정이 지하문명의 다음 세대를 정한다.');
    var k = await choose([
      { key: 'F4', label: '제3인류가 스스로 운영한다', hint: '자율성이 오르지만 부담도 크다' },
      { key: 'F5', label: '세 집단이 공동으로 운영한다', hint: '협상의 조건을 확보한다' },
      { key: 'F6', label: '급진파에 운영권을 넘긴다', hint: '당장은 안정적이지만 통제력을 내준다' }
    ], 30, 1);
    return k;
  }

  async function specialCouncil() {
    hud('riwa', '챕터 4 · 주민 의회 특별회의', 4); clearStage();
    addHeading('주민 의회 특별회의');
    addText('마이라: "급진파가 운영권을 요구하고 있어. 우리는 지금 거절할 수 없어. 리와, 어떻게 대응할 거야?"');
    var c = await choose([
      { key: 'persuade', label: '주민을 설득한다', hint: '공청회 · 증거 제시 미션' },
      { key: 'cost', label: '유지비를 줄일 방법을 다시 설계한다', hint: '추가 과학 설계 미션' },
      { key: 'cond', label: '조건부로 이양한다', hint: '급진파와 운영 계약' }
    ], 30, 2);
    if (c === 'persuade') {
      tag('F2', '리와의 대응: 주민 설득');
      var r = await mission({
        pov: 'riwa', label: '챕터 4 · 공청회', ch: 4, title: '공청회 · 증거 제시',
        intro: ['주민 의회 특별회의장. 급진파는 운영권 이양을 요구하고, 주민들은 유지비 부담에 지쳐 있다.', '리와는 주민들 앞에 서서 직접 설득해야 한다.'],
        base: state.truth + state.trust, target: 8, margin: 0,
        steps: [
          { q: '① 무엇을 근거로 제시할 것인가?', opts: [
            { label: '찾아낸 생체 칩의 진실을 공개한다', hint: '증거가 충분할수록 강력하다', score: state.truth >= 3 ? 3 : 1 },
            { label: '시설의 현재 성능을 보여 준다', hint: '기술 완성도가 높을수록 설득력이 크다', score: state.tech >= 3 ? 3 : 1 },
            { label: '노아와 에이든 같은 사람들의 증언을 세운다', hint: '신뢰가 쌓였을수록 효과가 크다', score: state.trust >= 3 ? 3 : 1 }] },
          { q: '② 발언 방식', opts: [
            { label: '감정에 호소한다', hint: '무난하지만 약하다', score: 1 },
            { label: '비용과 계획을 숫자로 설명한다', hint: '신뢰를 얻는다', score: 2 },
            { label: '바렌의 프로메테우스 계획을 폭로한다', hint: '세린의 증거가 필요하다', score: 4, lock: function () { return !state.serin; }, lockHint: '세린과 확보한 증거가 있어야 열려요' }] }
        ]
      });
      if (r.grade === 'full') { addStatus(true, '설득 성공'); tag('F2', '공청회 설득 성공'); await cont(); await fstate('F4'); }
      else { addStatus(false, '설득 실패'); tag('F2', '공청회 설득 실패'); await cont(); await radicalContract(); }
    } else if (c === 'cost') {
      tag('F2', '리와의 대응: 유지비 개선');
      var r2 = await mission({
        pov: 'riwa', label: '챕터 4 · 추가 설계', ch: 4, title: '추가 과학 설계 · 유지비 절감',
        intro: ['유지비가 문제라면 설계를 고쳐 줄이면 된다. 시설을 멈추지 않으면서 비용을 깎아야 한다.'],
        base: state.tech, target: 6, margin: 0,
        steps: [
          { q: '① 절감 방법', opts: [
            { label: '세척과 점검 주기를 늘린다', hint: '비용이 줄지만 고장 위험이 있다', score: 2 },
            { label: '설비 배치를 다시 짜 효율을 올린다', hint: '기술 완성도가 높을수록 효과가 크다', score: state.tech >= 3 ? 4 : 2 },
            { label: '주민이 직접 정비하도록 교육한다', hint: '신뢰가 쌓였을수록 효과가 크다', score: state.trust >= 3 ? 3 : 1 }] },
          { q: '② 어디서 아낄 것인가', opts: [
            { label: '연구 예산을 줄인다', hint: '당장은 안정적이다', score: 1 },
            { label: '주민 복지를 줄인다', hint: '큰 폭으로 아끼지만 안정성이 내려간다', score: 3, fx: function () { chg('stability', -1); } },
            { label: '에덴과 자재를 비밀리에 교환한다', hint: '효과가 크지만 위험이 늘어난다', score: 3, fx: function () { chg('risk', 1); } }] }
        ]
      });
      if (r2.grade === 'full') { addStatus(true, '유지비 절감 성공'); tag('F2', '유지비 절감 성공'); await cont(); await fstate('F4'); }
      else { addStatus(false, '유지비 절감 실패'); tag('F2', '유지비 절감 실패'); await cont(); await radicalContract(); }
    } else {
      tag('F2', '리와의 대응: 조건부 이양');
      await radicalContract();
    }
  }

  async function radicalContract() {
    var r = await mission({
      pov: 'riwa', label: '챕터 4 · 급진파와 운영 계약', ch: 4, title: '급진파와 운영 계약',
      intro: ['바렌과 마주 앉았다. 운영권을 넘기더라도 감시와 철회의 길만은 계약서에 남겨 두어야 한다.'],
      base: state.truth + (state.serin ? 2 : 0), target: 6, margin: 0,
      steps: [
        { q: '① 어떤 조항을 요구할 것인가?', opts: [
          { label: '외부 감사권', hint: '운영을 감시할 수 있다', score: 2 },
          { label: '운영권 철회 조항', hint: '언제든 되찾을 근거가 된다', score: 3 },
          { label: '운영 기간 제한', hint: '무난하지만 약하다', score: 1 }] },
        { q: '② 협상 카드', opts: [
          { label: '프로메테우스 계획 문서를 제시한다', hint: '세린의 증거가 필요하다', score: 4, lock: function () { return !state.serin; }, lockHint: '세린과 확보한 증거가 있어야 열려요' },
          { label: '주민 의회의 지지를 앞세운다', hint: '안정적인 카드', score: 2 },
          { label: '에덴과의 비밀 접촉을 암시한다', hint: '효과는 크지만 위험이 늘어난다', score: 3, fx: function () { chg('risk', 1); } }] }
      ]
    });
    if (r.grade === 'full') { addStatus(true, '감사권·철회 조항 확보'); tag('F7', '감사권·철회 조항 확보'); await cont(); await fstate('F7'); }
    else { addStatus(false, '조항 확보 실패'); tag('F6', '감사권·철회 조항 확보 실패'); await cont(); await fstate('F6'); }
  }

  /* ================= 챕터 5 · 공동 시점 : 엔딩 분기 ================= */
  var END_TEXT = {
    HARMONY: '세 인류가 같은 테이블에 앉았다. 에덴은 제2인류의 생체 칩을 해제하고, 지하는 시설의 운영권을 나눈다. 기술은 누구도 독점하지 않는다.\n리와와 카엘은 처음으로 같은 편에서 악수를 나눈다.\n카엘: "처음부터 이렇게 앉았어야 했어."\n리와: "늦지 않아서 다행이야."\n노아: "이제 우리 모두의 이야기가 시작되는 거죠?"',
    ISOLATION: '지하문명은 스스로 서는 법을 익혔다. 에덴과 급진파와의 교류는 끊겼다.\n자유롭지만 닫힌 세계에서 아이들은 지상을 모르고 자란다.\n마이라: "우리끼리도 살 수 있어. 그래도... 지상은 어떨까."',
    DEPENDENCE: '지하문명은 살아남았다. 그러나 에덴의 자재와 급진파의 인력 없이는 시설을 돌릴 수 없다.\n생존은 의존의 다른 이름이 되었다.\n세린: "살아남긴 했는데, 우린 누구의 것이지?"',
    NEWRULERS: '바렌은 해방을 선언했고 기술과 시설의 열쇠를 쥐었다. 에덴의 통제는 사라졌지만, 새로운 통제가 그 자리를 채웠다.\n바렌: "이제 이 아래의 법은 내가 정한다."\n카엘은 에덴의 창가에서 지하로 이어진 새 지배자의 불빛을 바라본다.',
    COLLAPSE: '갱도가 무너지고 시설이 멈췄다. 주민들은 피난을 떠나지만 갈 곳은 많지 않다.\n지하의 불빛이 하나씩 꺼진다.\n리와: "미안해... 지키지 못했어."'
  };

  async function ch5() {
    var intro = {
      F4: '지하문명은 스스로 시설을 운영하기 시작했다. 그러나 에덴과 급진파는 이를 가만히 두고 보지 않는다.',
      F5: '세 집단이 협상 테이블에 앉을 준비를 마쳤다. 그러나 누구도 서로를 완전히 믿지는 않는다.\n카엘: "리와, 이번엔 서로의 패를 먼저 꺼내야 해."',
      F6: '바렌이 시설의 운영권을 장악했다. 카엘은 에덴에서 그 소식을 듣는다.\n바렌: "해방은 시작됐다. 이제 돌아가는 길은 없다."',
      F7: '운영권은 급진파가 쥐었지만 철회의 길이 남아 있다. 협상 테이블이 마련된다.\n리와: "카엘, 이번엔 약속을 문서로 남기자."',
      F8: '에덴의 통제 아래 시설이 돌아간다. 그러나 에덴의 보안국이 지하 깊숙이 들어오기 시작한다.\n레온: "보안국 전 병력은 지하로 진입한다. 한 명도 놓치지 마라."',
      F9: '시설은 불안정하고 주민들은 흔들린다. 그때 에덴의 봉쇄선이 좁혀 온다.\n레온: "봉쇄선을 좁혀라. 지하에서 한 발짝도 나오지 못하게."'
    };
    hud('joint', '챕터 5 · 최종 위기', 5); clearStage();
    addHeading('최종 위기');
    addText(intro[state.f]);
    tag('CH5', '챕터 5 시작 상태: ' + state.f);
    await cont();
    var map = { F6: 'B', F9: 'A', F8: 'A', F5: 'C', F7: 'C', F4: 'D' };
    var node = map[state.f];
    for (;;) {
      var r = node === 'A' ? await ch5A() : node === 'B' ? await ch5B() : node === 'C' ? await ch5C() : await ch5D();
      if (r.end) { tag(r.end, ENDINGS[r.end].tag); await endingScreen(r.end, END_TEXT[r.end]); return; }
      node = r.next;
    }
  }

  async function ch5A() {
    tag('CH5-A', '지하문명 최후의 방어');
    var r = await mission({
      pov: 'joint', label: '챕터 5 · 피난·방어 미션', ch: 5, title: 'CH5-A · 지하문명 최후의 방어',
      intro: ['에덴의 봉쇄선이 지하 입구를 좁혀 온다. 카엘은 에덴 안에서, 리와는 지하에서 같은 밤을 보낸다.', '마이라: "우리가 가진 걸 전부 써야 해. 한 사람도 놓치지 마."'],
      base: state.stability + state.tech - state.risk, target: 6, margin: 0,
      steps: [
        { q: '① 방어선', opts: [
          { label: '입구 갱도를 스스로 무너뜨려 봉쇄한다', hint: '지하망 기술이 있으면 안전하다', score: state.data === 'SPACE' ? 4 : 2 },
          { label: '전력 방어 시설을 가동한다', hint: '기술 완성도가 높을수록 오래 버틴다', score: state.tech >= 3 ? 3 : 1 },
          { label: '주민들을 안쪽 구역으로 분산한다', hint: '안정성이 높을수록 혼란이 적다', score: state.stability >= 4 ? 3 : 1 }] },
        { q: '② 피난 순서', opts: [
          { label: '아이와 환자부터 먼저 옮긴다', hint: '신뢰가 쌓였을수록 질서가 유지된다', score: state.trust >= 3 ? 3 : 1 },
          { label: '기술자와 설비를 먼저 옮긴다', hint: '시설을 지킨다', score: 2 },
          { label: '급진파 병력에게 질서를 맡긴다', hint: '확실하지만 통제권을 내준다', score: 3, fx: function () { chg('support', 10); } }] }
      ]
    });
    if (r.grade !== 'full') { addStatus(false, '방어 실패'); tag('CH5-A', '피난·방어 실패'); await cont(); return { end: 'COLLAPSE' }; }
    addStatus(true, '방어 성공'); tag('CH5-A', '피난·방어 성공');
    await cont();
    clearStage(); hud('joint', '챕터 5 · 누구의 도움으로 생존했는가', 5);
    addHeading('누구의 도움으로 생존했는가?');
    addText('봉쇄선이 물러났다. 사람들은 서로를 부둥켜안는다. 이 생존을 누구의 몫으로 기억할 것인가.');
    var k = await choose([
      { key: 'self', label: '우리의 기술로 버텨 냈다', hint: '자립을 택한다' },
      { key: 'talk', label: '세 인류가 서로 도운 덕분이다', hint: '신뢰 4 이상, 진실 2 이상이어야 열려요',
        locked: !(state.trust >= 4 && state.truth >= 2), lockHint: '신뢰 4 이상, 진실 2 이상이어야 열려요' },
      { key: 'ext', label: '에덴과 급진파의 도움 덕분이다', hint: '도움을 받아들인다' }
    ], 30, 0);
    if (k === 'self') return { next: 'D' };
    if (k === 'talk') return { next: 'C' };
    return { end: 'DEPENDENCE' };
  }

  async function ch5B() {
    hud('joint', '챕터 5 · 새로운 지배자의 탄생', 5); clearStage();
    addHeading('CH5-B · 새로운 지배자의 탄생');
    addText('바렌은 해방을 선언했다. 그러나 시설의 모든 설비는 급진파의 승인 없이는 움직이지 않는다. 카엘은 이 소식을 에덴에서 듣는다.');
    tag('CH5-B', '새로운 지배자의 탄생');
    if (state.noa !== 'rescued') {
      tag('CH5-B', '카엘의 설득 실패, 급진파 내부 불신');
      addText('카엘에게는 급진파 안에서 말을 들어 줄 연줄이 없다. 노아를 구하지 못했던 대가였다. 급진파 내부에는 불신만 번진다.', 'res');
      await cont();
      return { end: 'NEWRULERS' };
    }
    tag('CH5-B', '노아가 카엘을 변호, 급진파 온건파와 접촉');
    addText('노아가 급진파 사이에서 카엘을 변호한다. "저 사람은 나를 구했어." 온건파와 닿는 길이 열린다.', 'res');
    await cont();
    clearStage(); hud('joint', '챕터 5 · 세린의 증거와 협력', 5);
    addHeading('세린의 증거와 리와의 협력이 있는가?');
    if (!(state.serin && state.trust >= 3)) {
      tag('CH5-B', '증거와 협력이 부족함');
      addText('바렌의 독점 계획을 입증할 증거도, 리와와 카엘이 손잡을 만큼의 신뢰도 부족하다. 온건파는 주저하다 침묵한다.', 'res');
      await cont();
      return { end: 'NEWRULERS' };
    }
    addText('세린이 확보한 프로메테우스 계획 문서가 있다. 리와와 카엘 사이에도 신뢰가 있다. 이제 바렌의 독점을 저지할 특별 협상 자리를 만들 수 있다.');
    var r = await mission({
      pov: 'joint', label: '챕터 5 · 특별 협상 미션', ch: 5, title: '특별 협상 · 바렌의 독점권 저지',
      intro: ['급진파 온건파, 노아, 세린이 한 자리에 모였다. 바렌 없이 열리는 비공식 회의다.'],
      base: state.truth + state.trust, target: 9, margin: 0,
      steps: [
        { q: '① 먼저 무엇을 내놓을 것인가?', opts: [
          { label: '프로메테우스 계획 문서를 공개한다', hint: '증거가 확실하다', score: 4 },
          { label: '운영권 공동 헌장을 제안한다', hint: '신뢰가 높을수록 받아들여진다', score: state.trust >= 4 ? 4 : 2 },
          { label: '에덴의 칩 해제 약속을 보여 준다', hint: '진실이 쌓였을수록 설득력이 크다', score: state.truth >= 3 ? 4 : 2 }] },
        { q: '② 바렌의 반격에 어떻게 대응할 것인가?', opts: [
          { label: '주민들의 지지를 호소한다', hint: '안정적인 선택', score: 2 },
          { label: '노아의 증언을 앞세운다', hint: '노아를 구했을 때의 카드', score: 3 },
          { label: '위협에는 위협으로 맞선다', hint: '위험하다', score: 1, fx: function () { chg('risk', 1); } }] }
      ]
    });
    if (r.grade === 'full') { addStatus(true, '협상 성공'); tag('CH5-B', '바렌의 독점권 저지 성공'); await cont(); return { next: 'C' }; }
    addStatus(false, '협상 실패'); tag('CH5-B', '바렌의 독점권 저지 실패'); await cont();
    return { end: 'NEWRULERS' };
  }

  async function ch5C() {
    tag('CH5-C', '세 인류의 협상');
    var r = await mission({
      pov: 'joint', label: '챕터 5 · 협상 미션', ch: 5, title: 'CH5-C · 세 인류의 협상',
      intro: ['에덴, 급진파, 제3인류 대표가 한 테이블에 앉았다. 카엘과 리와가 처음으로 같은 방에서 서로를 마주 본다.', '공존의 조건은 높은 신뢰, 기술, 증거가 모두 갖춰져 있을 때만 완성된다.'],
      base: state.trust + state.truth + state.tech, target: 12, margin: 3,
      steps: [
        { q: '① 협상 의제', opts: [
          { label: '생체 칩 해제와 진실 공개', hint: '진실이 많을수록 강력하다', score: state.truth >= 3 ? 4 : 2 },
          { label: '기술과 시설의 공동 운영 헌장', hint: '기술이 높을수록 설득력이 크다', score: state.tech >= 3 ? 4 : 2 },
          { label: '지하와 에덴 사이 교류 통로 개설', hint: '신뢰가 높을수록 받아들여진다', score: state.trust >= 4 ? 4 : 2 }] },
        { q: '② 협상 태도', opts: [
          { label: '모든 집단의 이익을 균형 있게 다룬다', hint: '시간은 걸리지만 안정적이다', score: 3 },
          { label: '제3인류의 자율성을 앞세운다', hint: '자율성이 높을수록 효과가 있다', score: state.autonomy >= 3 ? 3 : 1 },
          { label: '증거를 압박 수단으로 쓴다', hint: '효과는 크지만 신뢰가 흔들린다', score: 4, fx: function () { chg('trust', -1); } }] }
      ]
    });
    if (r.grade === 'full' && state.trust >= 4 && state.tech >= 3 && state.truth >= 2) {
      addStatus(true, '협상 성공'); tag('CH5-C', '협상 성공'); await cont(); return { end: 'HARMONY' };
    }
    if (r.grade !== 'fail') {
      addText('부분 성공', 'status');
      addText('합의는 이루어졌지만 조건이 한쪽으로 기울었다. 일부 권한은 외부에 남는다.', 'res');
      tag('CH5-C', '협상 부분 성공'); await cont(); return { end: 'DEPENDENCE' };
    }
    addStatus(false, '협상 실패'); tag('CH5-C', '협상 실패');
    state.triedTalk = true;
    await cont();
    return { next: 'D' };
  }

  async function ch5D() {
    hud('joint', '챕터 5 · 불완전한 독립', 5); clearStage();
    addHeading('CH5-D · 불완전한 독립');
    addText('지하문명은 불완전하지만 홀로 서 있다. 이제 이 독립이 오래갈지 시험받는다.');
    tag('CH5-D', '불완전한 독립');
    var enough = state.tech >= 3 && state.autonomy >= 3;
    addText('독자 생존 기술 ' + (enough ? '충분' : '부족') + ' (기술 ' + state.tech + ' / 자율성 ' + state.autonomy + ', 각각 3 이상 필요)', 'sub');
    if (!enough) {
      addText('시설은 점점 낡아 가고, 필요한 자재는 구할 곳이 없다. 마이라는 어렵게 외부에 지원을 요청한다.');
      if (state.trust >= 3) {
        addStatus(true, '지원 수락');
        addText('그동안 쌓아 온 신뢰 덕분에 에덴 온건파와 급진파가 지원을 수락한다.', 'res');
        tag('CH5-D', '외부 지원 요청, 지원 수락'); await cont(); return { end: 'DEPENDENCE' };
      }
      addStatus(false, '지원 거부');
      addText('아무도 지하문명을 믿지 않았다. 지원은 오지 않고 시설은 복구되지 못한다.', 'res');
      tag('CH5-D', '외부 지원 요청, 지원 거부와 복구 실패'); await cont(); return { end: 'COLLAPSE' };
    }
    addText('이제 선택해야 한다. 외부와 교류할 것인가, 홀로 설 것인가.');
    var k = await choose([
      { key: 'cut', label: '교류를 끊고 지하에서 홀로 선다', hint: '자유롭지만 닫힌 세계가 된다' },
      { key: 'talk', label: '에덴, 급진파와 협상을 시도한다', hint: state.triedTalk ? '이미 한 번 실패했어요' : '세 인류의 협상으로 이어진다',
        locked: state.triedTalk, lockHint: '이미 협상에 실패했어요' }
    ], 30, 0);
    if (k === 'cut') { tag('CH5-D', '외부 교류 단절'); return { end: 'ISOLATION' }; }
    tag('CH5-D', '협상을 시도'); return { next: 'C' };
  }

  /* ================= 진행 ================= */
  async function play() {
    state = newState();
    await prologue();
    await ch1();
    await ch2();
    await ch3();
    await ch4();
    await ch5();
  }

  async function main() {
    for (;;) {
      await startScreen();
      await play();
    }
  }
  main();
})();
</script>
