<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<title>방탈출 기록</title>
<style>
:root{
  --bg:#f4f4f6; --text:#1c1c1e; --sub:#6e6e73;
  --glass:rgba(255,255,255,.55); --glass-edge:rgba(255,255,255,.8); --hair:rgba(60,60,67,.12);
  --done:rgba(130,215,150,.30); --done-edge:rgba(100,195,125,.45); --green:#4cbf6b;
  --star-on:#636366; --star-off:#d1d1d6; --poster:rgba(120,120,128,.10);
  --blob1:rgba(140,215,160,.35); --blob2:rgba(170,190,235,.30);
}
@media (prefers-color-scheme:dark){
  :root:not([data-theme="light"]){
    --bg:#111113; --text:#f2f2f7; --sub:#98989f;
    --glass:rgba(60,60,67,.45); --glass-edge:rgba(255,255,255,.14); --hair:rgba(255,255,255,.12);
    --done:rgba(80,170,105,.30); --done-edge:rgba(110,200,135,.40); --green:#5fd07e;
    --star-on:#c7c7cc; --star-off:#48484a; --poster:rgba(255,255,255,.07);
    --blob1:rgba(70,150,95,.30); --blob2:rgba(80,100,170,.25);
  }
}
:root[data-theme="dark"]{
  --bg:#111113; --text:#f2f2f7; --sub:#98989f;
  --glass:rgba(60,60,67,.45); --glass-edge:rgba(255,255,255,.14); --hair:rgba(255,255,255,.12);
  --done:rgba(80,170,105,.30); --done-edge:rgba(110,200,135,.40); --green:#5fd07e;
  --star-on:#c7c7cc; --star-off:#48484a; --poster:rgba(255,255,255,.07);
  --blob1:rgba(70,150,95,.30); --blob2:rgba(80,100,170,.25);
}

:root{ box-sizing:border-box; padding-top:env(safe-area-inset-top,0px); padding-bottom:env(safe-area-inset-bottom,0px); }
html{ scroll-padding-top:env(safe-area-inset-top,0px); -webkit-text-size-adjust:100%; }
*,*::before,*::after{ box-sizing:inherit; -webkit-tap-highlight-color:transparent; }
body{
  margin:0; min-height:100vh; background:var(--bg); color:var(--text);
  font-family:-apple-system,BlinkMacSystemFont,"SF Pro Text","Apple SD Gothic Neo","Noto Sans KR",sans-serif;
  -webkit-font-smoothing:antialiased;
}
/* soft blurred shapes behind the glass so the blur has something to show */
body::before{
  content:""; position:fixed; inset:0; z-index:-1; pointer-events:none;
  background:
    radial-gradient(60% 40% at 15% 12%, var(--blob1), transparent 70%),
    radial-gradient(55% 40% at 90% 85%, var(--blob2), transparent 70%);
}
button{ font:inherit; color:inherit; border:0; background:none; padding:0; margin:0; cursor:pointer; touch-action:manipulation; }

#app{ max-width:520px; margin:0 auto; padding:0 16px 40px; }
.view{ min-height:calc(100vh - 32px); animation:inR .28s ease-out; }
.view.back{ animation-name:inL; }
@keyframes inR{ from{opacity:0; transform:translateX(18px)} to{opacity:1; transform:none} }
@keyframes inL{ from{opacity:0; transform:translateX(-18px)} to{opacity:1; transform:none} }

/* glass surface */
.glass{
  background:var(--glass);
  -webkit-backdrop-filter:blur(24px) saturate(180%); backdrop-filter:blur(24px) saturate(180%);
  border:1px solid var(--glass-edge);
  box-shadow:inset 0 1px 0 rgba(255,255,255,.35);
  transition:background .3s ease, border-color .3s ease, transform .15s ease, opacity .15s ease;
}
.press:active{ transform:scale(.97); opacity:.8; }

/* nav */
.nav{ display:grid; grid-template-columns:84px 1fr 84px; align-items:center; gap:8px; padding:12px 0 8px; min-height:60px; }
.back-btn{ height:36px; border-radius:18px; padding:0 14px 0 10px; font-size:16px; display:inline-flex; align-items:center; gap:2px; justify-self:start; }
.nav-title{ text-align:center; font-size:16px; font-weight:600; line-height:1.3; word-break:keep-all; overflow-wrap:break-word; }
.page-title{ font-size:32px; font-weight:700; letter-spacing:-.02em; margin:14px 4px 20px; }

/* home */
.home{ display:flex; flex-direction:column; justify-content:center; min-height:calc(100vh - 110px); padding-bottom:8vh; }
.home h1{ font-size:34px; font-weight:700; letter-spacing:-.02em; text-align:center; margin:0 0 32px; }

/* list buttons */
.list{ display:flex; flex-direction:column; gap:12px; }
.item{
  width:100%; min-height:64px; padding:16px 20px; border-radius:22px; text-align:center;
  font-size:18px; font-weight:600; line-height:1.4; word-break:keep-all; overflow-wrap:break-word;
}
.item.done{ background:var(--done); border-color:var(--done-edge); }
.empty{ padding:28px 20px; border-radius:22px; text-align:center; color:var(--sub); font-size:15px; }

/* theme card */
.card{ display:flex; gap:16px; padding:16px; border-radius:28px; margin-top:8px; }
.card.done{ background:var(--done); border-color:var(--done-edge); }
.poster-wrap{ position:relative; flex:none; width:120px; height:176px; }
.poster{
  position:relative; display:flex; align-items:center; justify-content:center; width:100%; height:100%;
  border-radius:16px; overflow:hidden; background:var(--poster); color:var(--sub); cursor:pointer;
}
.poster img{ width:100%; height:100%; object-fit:cover; display:block; }
.ph{ display:flex; flex-direction:column; align-items:center; gap:2px; font-size:12px; }
.ph b{ font-size:28px; font-weight:300; line-height:1; }
.file{ position:absolute; width:1px; height:1px; opacity:0; pointer-events:none; }
.poster-x{ position:absolute; top:6px; right:6px; width:30px; height:30px; border-radius:15px; font-size:18px; line-height:1; display:flex; align-items:center; justify-content:center; }
.memo{ margin-top:16px; padding:14px 16px; border-radius:24px; }
.memo .lbl{ margin-bottom:6px; }
.memo textarea{
  display:block; width:100%; height:120px; margin:0; padding:0; border:0; outline:0; border-radius:0;
  background:transparent; color:var(--text); font:inherit; font-size:16px; line-height:24px; resize:none; -webkit-appearance:none;
}
.memo textarea::placeholder{ color:var(--sub); }

/* + 버튼 & 추가 시트 */
.plus{ width:36px; height:36px; border-radius:18px; justify-self:end; display:flex; align-items:center; justify-content:center; font-size:26px; font-weight:300; line-height:1; padding-bottom:3px; }
.sheet-bg{ position:fixed; inset:0; z-index:10; display:flex; justify-content:center; align-items:flex-start; padding:16vh 20px 0; background:rgba(0,0,0,.22); animation:fade .2s ease; }
.sheet{ width:100%; max-width:340px; padding:20px; border-radius:28px; }
.sheet h3{ margin:0 0 14px; font-size:18px; font-weight:700; text-align:center; }
.sheet input{ display:block; width:100%; height:46px; padding:0 14px; border:0; outline:0; border-radius:14px; background:var(--poster); color:var(--text); font:inherit; font-size:17px; -webkit-appearance:none; }
.sheet input::placeholder{ color:var(--sub); }
.sheet-btns{ display:flex; gap:10px; margin-top:14px; }
.sheet-btns button{ flex:1; height:46px; border-radius:14px; font-size:16px; font-weight:600; background:var(--poster); }
.sheet-btns .primary{ background:var(--green); color:#fff; }
@keyframes fade{ from{opacity:0} to{opacity:1} }
.view.still{ animation:none; }
.item{ -webkit-touch-callout:none; -webkit-user-select:none; user-select:none; }
.sheet .note{ margin:-6px 0 14px; text-align:center; font-size:14px; color:var(--sub); }
.menu{ display:flex; flex-direction:column; gap:8px; }
.menu button{ height:48px; border-radius:14px; font-size:16px; font-weight:600; background:var(--poster); }
.menu .danger{ color:#ff453a; }
.pill{ padding:0 14px; }
.sheet-bg{ overflow-y:auto; }
.sheet textarea{ display:block; width:100%; height:120px; margin:0 0 12px; padding:10px 12px; border:0; outline:0; border-radius:14px; background:var(--poster); color:var(--text); font:inherit; font-size:16px; line-height:1.4; resize:none; -webkit-appearance:none; }
.sheet textarea::placeholder{ color:var(--sub); }
.sheet .check{ margin:0 0 4px; font-size:15px; }
.sheet .note.err{ color:#ff453a; }
.info{ flex:1; min-width:0; display:flex; flex-direction:column; justify-content:space-between; gap:12px; }
.info h2{ margin:0; font-size:21px; font-weight:700; line-height:1.3; letter-spacing:-.01em; word-break:keep-all; overflow-wrap:break-word; }
.check{ display:flex; align-items:center; gap:10px; font-size:16px; text-align:left; min-height:40px; transition:opacity .15s ease; }
.check:active{ opacity:.6; }
.box{ flex:none; position:relative; width:24px; height:24px; border-radius:8px; border:1.5px solid var(--sub); transition:background .25s ease, border-color .25s ease; }
.box::after{
  content:""; position:absolute; left:7px; top:2.5px; width:6px; height:11px;
  border:solid #fff; border-width:0 2px 2px 0; transform:rotate(45deg) scale(0); transition:transform .2s ease;
}
.check[aria-checked="true"] .box{ background:var(--green); border-color:var(--green); }
.check[aria-checked="true"] .box::after{ transform:rotate(45deg) scale(1); }
.lbl{ font-size:13px; color:var(--sub); margin-bottom:2px; }
.stars{ display:flex; margin-left:-4px; }
.star{ width:32px; height:40px; font-size:26px; line-height:40px; color:var(--star-off); transition:color .2s ease; }
.star.on{ color:var(--star-on); }
.star.pop{ animation:pop .3s ease; }
@keyframes pop{ 0%{transform:scale(1)} 45%{transform:scale(1.3)} 100%{transform:scale(1)} }

@media (max-width:359px){
  .poster-wrap{ width:100px; height:148px; }
  .card{ gap:12px; padding:14px; }
  .star{ width:28px; }
}
@media (prefers-reduced-motion:reduce){ .view{ animation:none; } .star.pop{ animation:none; } }
</style>
</head>
<body>
<div id="app"></div>

<script>
/* ============================================================
   DATA — 처음 실행할 때만 쓰이는 기본 목록입니다.
   이후 추가·수정·삭제는 앱 안에서 하세요 (+ 버튼 / 항목 꾹 누르기).
   id는 저장된 기록과 연결되니 바꾸지 마세요.
   나중에 필요한 항목(플레이 날짜, 메모 등)은 RECORD_DEFAULTS에 추가하세요.
   ============================================================ */
const BRANDS = [
  { id:'danpyeonseon', name:'단편선', themes:[
    { id:'t1', name:'그림자 없는 상자', image:'' },
    { id:'t2', name:'사람들은 그것을 행복이라 부르기로 했다', image:'' },
    { id:'t3', name:'쓰여진 문장 속에 구원이 없다면', image:'' },
    { id:'t4', name:'존재할 자격', image:'' },
    { id:'t5', name:'쥐와 파시스트와 마지막 한 장', image:'' },
    { id:'t6', name:'뱃사람의 별', image:'' }
  ]},
  { id:'caseescape', name:'케이스케이프', themes:[] }
];

/* 테마별 개인 기록의 기본값 (확장하려면 항목만 추가) */
const RECORD_DEFAULTS = {
  played:false,      // 플레이 완료
  difficulty:0,      // 난이도 0~5
  poster:'',         // 내가 넣은 포스터 사진 (압축된 data URL)
  memo:'',           // 후기
  // playDate:'', duration:'', companions:'', escaped:null, revisit:false
};

/* ============================================================
   STORAGE
   ============================================================ */
const STORE_KEY = 'escape-room-log-v1';
let records = {};
try { records = JSON.parse(localStorage.getItem(STORE_KEY)) || {}; } catch (e) { records = {}; }

function getRecord(brandId, themeId){
  return Object.assign({}, RECORD_DEFAULTS, records[brandId + '/' + themeId]);
}
function setRecord(brandId, themeId, patch){
  records[brandId + '/' + themeId] = Object.assign(getRecord(brandId, themeId), patch);
  try { localStorage.setItem(STORE_KEY, JSON.stringify(records)); return true; } catch (e) { return false; }
}

/* 업체/테마 데이터: 처음 실행할 때 BRANDS를 복사해 두고, 이후 앱에서 바꾼 내용이 저장됩니다 */
const DATA_KEY = 'escape-room-data-v2';
let store = null;
try { const d = JSON.parse(localStorage.getItem(DATA_KEY)); if (d && Array.isArray(d.brands)) store = d; } catch (e) {}
if (!store) {
  store = { brands: BRANDS.map(b => ({ id:b.id, name:b.name, themes:b.themes.map(t => ({ id:t.id, name:t.name, image:t.image || '' })) })) };
  try {   // 이전 버전에서 앱으로 추가한 업체/테마 가져오기
    const old = JSON.parse(localStorage.getItem('escape-room-custom-v1'));
    if (old) {
      (old.brands || []).forEach(b => store.brands.push({ id:b.id, name:b.name, themes:[] }));
      Object.keys(old.themes || {}).forEach(function(bid){
        const brand = store.brands.find(b => b.id === bid);
        if (brand) (old.themes[bid] || []).forEach(t => brand.themes.push({ id:t.id, name:t.name, image:'' }));
      });
    }
  } catch (e) {}
  try { localStorage.setItem(DATA_KEY, JSON.stringify(store)); } catch (e) {}
}
function saveData(){
  try { localStorage.setItem(DATA_KEY, JSON.stringify(store)); return true; } catch (e) { return false; }
}
const getBrands = () => store.brands;
/* 회사의 테마를 모두 클리어하면 회사도 초록색 */
const brandDone = b => b.themes.length > 0 && b.themes.every(t => getRecord(b.id, t.id).played);
const newId = p => p + Date.now().toString(36) + Math.random().toString(36).slice(2, 5);

/* ============================================================
   NAVIGATION — 화면 스택 (첫 화면 → 업체 → 테마)
   ============================================================ */
const app = document.getElementById('app');
let stack = [{ view:'home' }];

function go(screen){
  stack.push(screen);
  try { history.pushState({ d:stack.length }, ''); } catch (e) {}
  render(false);
}
function goBack(){
  if (stack.length > 1) { stack.pop(); render(true); }
}
/* Safari 스와이프 뒤로가기도 같은 방식으로 처리 */
window.addEventListener('popstate', goBack);

/* ============================================================
   RENDER
   ============================================================ */
const esc = s => String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const findBrand = id => getBrands().find(b => b.id === id);

function plusBtn(act, label){
  return '<button class="glass press plus" data-act="' + act + '" aria-label="' + label + '">+</button>';
}
function nav(title, right, noBack, left){
  return '<div class="nav">' +
    (noBack ? (left || '<span></span>') : '<button class="glass press back-btn" data-act="back" aria-label="뒤로">‹ 뒤로</button>') +
    '<div class="nav-title">' + esc(title || '') + '</div>' + (right || '<span></span>') + '</div>';
}

function homeView(){
  return nav('', plusBtn('add-brand', '회사 추가'), true,
      '<button class="glass press back-btn pill" data-act="backup">백업</button>') +
    '<div class="home"><h1>방탈출 기록</h1><div class="list">' +
    getBrands().map(b => '<button class="glass press item' + (brandDone(b) ? ' done' : '') + '" data-brand="' + esc(b.id) + '">' + esc(b.name) + '</button>').join('') +
    '</div></div>';
}

function brandView(brand){
  const items = brand.themes.length
    ? brand.themes.map(t => {
        const done = getRecord(brand.id, t.id).played;
        return '<button class="glass press item' + (done ? ' done' : '') + '" data-theme="' + esc(t.id) + '">' + esc(t.name) + '</button>';
      }).join('')
    : '<div class="glass empty">등록된 테마가 없어요<br>오른쪽 위 + 버튼으로 추가해 보세요</div>';
  return nav('', plusBtn('add-theme', '테마 추가')) + '<h1 class="page-title">' + esc(brand.name) + '</h1><div class="list">' + items + '</div>';
}

function starsHtml(n){
  let h = '';
  for (let i = 1; i <= 5; i++) {
    h += '<button class="star' + (i <= n ? ' on' : '') + '" data-star="' + i + '" aria-label="난이도 ' + i + '점">★</button>';
  }
  return h;
}

/* 포스터 칸: 탭하면 사진 선택, 사진이 있으면 오른쪽 위 × 로 삭제 */
function posterHtml(r, theme){
  const img = r.poster || theme.image;
  return '<div class="poster-wrap"><label class="poster press">' +
      (img ? '<img src="' + esc(img) + '" alt="' + esc(theme.name) + ' 포스터">' : '<span class="ph"><b>＋</b>사진 추가</span>') +
      '<input class="file" id="posterInput" type="file" accept="image/*"></label>' +
      (r.poster ? '<button class="poster-x glass press" data-act="rmposter" aria-label="포스터 삭제">×</button>' : '') +
    '</div>';
}

function themeView(brand, theme){
  const r = getRecord(brand.id, theme.id);
  return nav(theme.name) +
    '<section class="glass card' + (r.played ? ' done' : '') + '" id="card">' +
      posterHtml(r, theme) +
      '<div class="info">' +
        '<h2>' + esc(theme.name) + '</h2>' +
        '<button class="check" id="check" role="checkbox" aria-checked="' + r.played + '"><span class="box"></span>플레이 완료</button>' +
        '<div><div class="lbl">난이도</div><div class="stars" id="stars">' + starsHtml(r.difficulty) + '</div></div>' +
      '</div>' +
    '</section>' +
    '<section class="glass memo"><div class="lbl">후기</div>' +
      '<textarea id="memo" rows="5" maxlength="2000" placeholder="후기를 남겨보세요">\n' + esc(r.memo) + '</textarea></section>';
}

function render(isBack, still){
  const cur = stack[stack.length - 1];
  let html = '';
  const brand = cur.brand && findBrand(cur.brand);
  const theme = brand && cur.theme && brand.themes.find(t => t.id === cur.theme);

  if (cur.view === 'brand' && brand) html = brandView(brand);
  else if (cur.view === 'theme' && brand && theme) html = themeView(brand, theme);
  else { stack = [{ view:'home' }]; html = homeView(); }   // 예외 시 항상 첫 화면으로 복구

  app.innerHTML = '<div class="view' + (isBack ? ' back' : '') + (still ? ' still' : '') + '">' + html + '</div>';
  if (!still) window.scrollTo(0, 0);
}

/* ============================================================
   EVENTS (한 곳에서 처리)
   ============================================================ */
app.addEventListener('click', function(e){
  const t = e.target.closest('button');
  if (!t || lpFired) return;   // 꾹 누르기 직후의 클릭은 무시
  const cur = stack[stack.length - 1];

  if (t.dataset.act === 'back') return goBack();
  if (t.dataset.act === 'rmposter') { setRecord(cur.brand, cur.theme, { poster:'' }); return refreshPoster(); }
  if (t.dataset.act === 'add-brand') return openSheet('회사 추가', '회사 이름', addBrand);
  if (t.dataset.act === 'backup') return openMenu('백업', [
    { label:'내보내기', fn:openExport },
    { label:'불러오기', fn:openImport },
    { label:'닫기' }
  ]);
  if (t.dataset.act === 'add-theme') return openSheet('테마 추가', '테마 이름', function(name){ addTheme(cur, name); });
  if (t.dataset.brand) return go({ view:'brand', brand:t.dataset.brand });
  if (t.dataset.theme) return go({ view:'theme', brand:cur.brand, theme:t.dataset.theme });

  if (t.id === 'check') {
    const next = !getRecord(cur.brand, cur.theme).played;
    setRecord(cur.brand, cur.theme, { played:next });
    t.setAttribute('aria-checked', next);
    document.getElementById('card').classList.toggle('done', next);
    return;
  }
  if (t.dataset.star) {
    const n = Number(t.dataset.star);
    const value = getRecord(cur.brand, cur.theme).difficulty === n ? 0 : n;  // 같은 별을 다시 누르면 초기화
    setRecord(cur.brand, cur.theme, { difficulty:value });
    document.querySelectorAll('#stars .star').forEach(function(s, i){
      s.classList.toggle('on', i < value);
      s.classList.remove('pop');
    });
    if (value) { void t.offsetWidth; t.classList.add('pop'); }
  }
});

/* 팝업(시트) 공통 */
function showOverlay(inner){
  const el = document.createElement('div');
  el.className = 'sheet-bg';
  el.innerHTML = '<div class="glass sheet">' + inner + '</div>';
  el.addEventListener('click', function(e){ if (e.target === el && !lpFired) el.remove(); });  // 바깥을 누르면 닫기
  document.body.appendChild(el);
  return el;
}

/* 이름 입력 (추가 / 수정 공용) */
function openSheet(title, placeholder, onSubmit, opts){
  opts = opts || {};
  const el = showOverlay('<h3>' + esc(title) + '</h3>' +
    '<input type="text" maxlength="60" autocomplete="off" enterkeyhint="done" placeholder="' + esc(placeholder) + '" value="' + esc(opts.value || '') + '">' +
    '<div class="sheet-btns"><button class="press" data-sheet="cancel">취소</button>' +
    '<button class="press primary" data-sheet="ok">' + esc(opts.okText || '추가') + '</button></div>');
  const input = el.querySelector('input');
  const submit = function(){
    const v = input.value.trim();
    if (!v) { input.focus(); return; }
    el.remove();
    onSubmit(v);
  };
  el.addEventListener('click', function(e){
    const b = e.target.closest('button');
    if (!b || lpFired) return;
    if (b.dataset.sheet === 'cancel') el.remove();
    if (b.dataset.sheet === 'ok') submit();
  });
  input.addEventListener('keydown', function(e){ if (e.key === 'Enter') { e.preventDefault(); submit(); } });
  input.focus();
  try { input.setSelectionRange(input.value.length, input.value.length); } catch (e) {}
}

/* 선택 메뉴 (수정 / 삭제 / 취소) */
function openMenu(title, actions, note){
  const el = showOverlay('<h3>' + esc(title) + '</h3>' + (note ? '<p class="note">' + esc(note) + '</p>' : '') +
    '<div class="menu">' + actions.map((a, i) =>
      '<button class="press' + (a.danger ? ' danger' : '') + '" data-i="' + i + '">' + esc(a.label) + '</button>').join('') + '</div>');
  el.addEventListener('click', function(e){
    const b = e.target.closest('button');
    if (!b || lpFired) return;
    const a = actions[Number(b.dataset.i)];
    el.remove();
    if (a && a.fn) a.fn();
  });
}

/* 추가 / 수정 / 삭제 */
function commit(){
  if (!saveData()) alert('저장 공간이 부족해서 이번에만 보여요');
  render(false, true);
}
function addBrand(name){
  store.brands.push({ id:newId('b'), name:name, themes:[] });
  commit();
}
function addTheme(cur, name){
  const b = store.brands.find(x => x.id === cur.brand);
  if (b) b.themes.push({ id:newId('t'), name:name, image:'' });
  commit();
}
function deleteItem(brand, theme){
  if (theme) {
    brand.themes = brand.themes.filter(t => t !== theme);
    delete records[brand.id + '/' + theme.id];
  } else {
    store.brands = store.brands.filter(b => b !== brand);
    Object.keys(records).forEach(function(k){ if (k.indexOf(brand.id + '/') === 0) delete records[k]; });
  }
  try { localStorage.setItem(STORE_KEY, JSON.stringify(records)); } catch (e) {}
  commit();
}
function onLongPress(el){
  const cur = stack[stack.length - 1];
  const isTheme = !!el.dataset.theme;
  const brand = store.brands.find(b => b.id === (isTheme ? cur.brand : el.dataset.brand));
  const theme = brand && isTheme ? brand.themes.find(t => t.id === el.dataset.theme) : null;
  const item = isTheme ? theme : brand;
  if (!item) return;
  openMenu(item.name, [
    { label:'이름 수정', fn:function(){
        openSheet('이름 수정', '이름', function(v){ item.name = v; commit(); }, { value:item.name, okText:'저장' });
    } },
    { label:'삭제', danger:true, fn:function(){
        openMenu('정말 삭제할까요?', [
          { label:'삭제', danger:true, fn:function(){ deleteItem(brand, theme); } },
          { label:'취소' }
        ], isTheme ? '이 테마의 기록도 함께 지워져요.' : '이 회사의 테마와 기록이 모두 지워져요.');
    } },
    { label:'취소' }
  ]);
}

/* 백업: 모든 기록을 텍스트 하나로 내보내고, 붙여넣어서 불러옵니다 */
function buildBackup(withPhotos){
  const recs = {};
  Object.keys(records).forEach(function(k){
    const r = Object.assign({}, records[k]);
    if (!withPhotos) delete r.poster;
    recs[k] = r;
  });
  return JSON.stringify({ app:'escape-room-log', version:2, exportedAt:new Date().toISOString(), store:store, records:recs });
}

function parseBackup(text){
  let o;
  try { o = JSON.parse(String(text).trim()); } catch (e) { return null; }
  const ok = o && o.app === 'escape-room-log' && o.store && Array.isArray(o.store.brands) &&
    o.records && typeof o.records === 'object' &&
    o.store.brands.every(b => b && typeof b.id === 'string' && typeof b.name === 'string' && Array.isArray(b.themes) &&
      b.themes.every(t => t && typeof t.id === 'string' && typeof t.name === 'string'));
  return ok ? o : null;
}

function copyText(ta, done){
  const text = ta.value;
  const fallback = function(){
    let ok = false;
    ta.focus(); ta.select();
    try { ta.setSelectionRange(0, text.length); ok = document.execCommand('copy'); } catch (e) {}
    done(ok);
  };
  if (navigator.clipboard && navigator.clipboard.writeText) navigator.clipboard.writeText(text).then(function(){ done(true); }, fallback);
  else fallback();
}

function openExport(){
  let withPhotos = false;
  const el = showOverlay('<h3>백업 내보내기</h3><p class="note">복사해서 메모 앱 등에 보관하세요.</p>' +
    '<textarea readonly></textarea>' +
    '<button class="check" role="checkbox" aria-checked="false" data-sheet="photos"><span class="box"></span>사진 포함 (내용이 길어져요)</button>' +
    '<div class="sheet-btns"><button class="press" data-sheet="cancel">닫기</button>' +
    '<button class="press primary" data-sheet="copy">복사</button></div>');
  const ta = el.querySelector('textarea');
  const fill = function(){ ta.value = buildBackup(withPhotos); };
  fill();
  el.addEventListener('click', function(e){
    const b = e.target.closest('button');
    if (!b || lpFired) return;
    const act = b.dataset.sheet;
    if (act === 'cancel') el.remove();
    if (act === 'photos') { withPhotos = !withPhotos; b.setAttribute('aria-checked', withPhotos); fill(); }
    if (act === 'copy') copyText(ta, function(ok){
      b.textContent = ok ? '복사됨 ✓' : '길게 눌러 복사';
      setTimeout(function(){ b.textContent = '복사'; }, 2500);
    });
  });
}

function applyBackup(o){
  store = { brands:o.store.brands };
  records = o.records;
  let ok = saveData();
  try { localStorage.setItem(STORE_KEY, JSON.stringify(records)); } catch (e) { ok = false; }
  if (!ok) alert('저장 공간이 부족해서 이번에만 반영돼요');
  stack = [{ view:'home' }];
  render(false, true);
}

function openImport(){
  const el = showOverlay('<h3>백업 불러오기</h3><p class="note" id="impNote">백업 내용을 붙여넣어 주세요.</p>' +
    '<textarea placeholder="여기에 붙여넣기" autocapitalize="off" autocorrect="off" spellcheck="false"></textarea>' +
    '<div class="sheet-btns"><button class="press" data-sheet="cancel">취소</button>' +
    '<button class="press primary" data-sheet="ok">불러오기</button></div>');
  const ta = el.querySelector('textarea');
  el.addEventListener('click', function(e){
    const b = e.target.closest('button');
    if (!b || lpFired) return;
    if (b.dataset.sheet === 'cancel') el.remove();
    if (b.dataset.sheet === 'ok') {
      const o = parseBackup(ta.value);
      const note = el.querySelector('#impNote');
      if (!o) { note.textContent = '올바른 백업 내용이 아니에요'; note.classList.add('err'); return; }
      const themes = o.store.brands.reduce((n, br) => n + br.themes.length, 0);
      el.remove();
      openMenu('현재 기록을 덮어쓸까요?', [
        { label:'덮어쓰기', danger:true, fn:function(){ applyBackup(o); } },
        { label:'취소' }
      ], '회사 ' + o.store.brands.length + '개, 테마 ' + themes + '개 (' + String(o.exportedAt || '').slice(0, 10) + ' 백업). 지금 기록은 사라져요.');
    }
  });
}

/* 꾹 누르기(2초) → 수정/삭제 메뉴. 손가락이 움직이거나 스크롤하면 취소 */
const LONG_PRESS_MS = 2000;
let lpTimer = null, lpStart = null, lpFired = false;
document.addEventListener('pointerdown', function(){ lpFired = false; }, true);
app.addEventListener('pointerdown', function(e){
  const el = e.target.closest('.item');
  if (!el || !(el.dataset.brand || el.dataset.theme)) return;
  lpStart = { x:e.clientX, y:e.clientY };
  clearTimeout(lpTimer);
  lpTimer = setTimeout(function(){ lpTimer = null; lpFired = true; onLongPress(el); }, LONG_PRESS_MS);
});
app.addEventListener('pointermove', function(e){
  if (lpTimer && lpStart && Math.hypot(e.clientX - lpStart.x, e.clientY - lpStart.y) > 10) { clearTimeout(lpTimer); lpTimer = null; }
});
function endPress(){
  if (lpTimer) { clearTimeout(lpTimer); lpTimer = null; }
  if (lpFired) setTimeout(function(){ lpFired = false; }, 400);
}
document.addEventListener('pointerup', endPress);
document.addEventListener('pointercancel', endPress);
app.addEventListener('contextmenu', function(e){ if (e.target.closest('.item')) e.preventDefault(); });

/* 사진 선택: 큰 사진은 줄여서(긴 변 720px, JPEG) 저장 공간을 아낍니다 */
function shrinkImage(file, done){
  const reader = new FileReader();
  reader.onerror = function(){ done(null); };
  reader.onload = function(){
    const img = new Image();
    img.onerror = function(){ done(null); };
    img.onload = function(){
      const s = Math.min(1, 720 / Math.max(img.naturalWidth, img.naturalHeight));
      const c = document.createElement('canvas');
      c.width = Math.round(img.naturalWidth * s);
      c.height = Math.round(img.naturalHeight * s);
      c.getContext('2d').drawImage(img, 0, 0, c.width, c.height);
      done(c.toDataURL('image/jpeg', 0.82));
    };
    img.src = reader.result;
  };
  reader.readAsDataURL(file);
}

function refreshPoster(){
  const cur = stack[stack.length - 1];
  const brand = findBrand(cur.brand);
  const theme = brand && brand.themes.find(t => t.id === cur.theme);
  const wrap = document.querySelector('.poster-wrap');
  if (wrap && theme) wrap.outerHTML = posterHtml(getRecord(cur.brand, cur.theme), theme);
}

app.addEventListener('change', function(e){
  if (e.target.id !== 'posterInput' || !e.target.files[0]) return;
  const cur = stack[stack.length - 1];
  shrinkImage(e.target.files[0], function(data){
    if (!data) return alert('사진을 불러오지 못했어요');
    if (!setRecord(cur.brand, cur.theme, { poster:data })) alert('저장 공간이 부족해서 이번에만 보여요');
    if (cur === stack[stack.length - 1]) refreshPoster();
  });
});

/* 후기: 입력하는 즉시 자동 저장 */
app.addEventListener('input', function(e){
  if (e.target.id !== 'memo') return;
  const cur = stack[stack.length - 1];
  setRecord(cur.brand, cur.theme, { memo:e.target.value });
});

render(false);
</script>
</body>
</html>
