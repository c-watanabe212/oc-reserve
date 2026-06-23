# oc-reserve[open_campus_tool (1).html](https://github.com/user-attachments/files/29236765/open_campus_tool.1.html)
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>オープンキャンパス予約管理</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:sans-serif;font-size:15px;background:#f5f5f3;color:#1a1a1a;min-height:100vh}
.header{background:#fff;border-bottom:1px solid #e0e0e0;padding:12px 20px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:100;flex-wrap:wrap;gap:8px}
.header h1{font-size:16px;font-weight:500}
.tab-bar{display:flex;gap:4px}
.tab{padding:6px 14px;border-radius:6px;border:none;background:none;cursor:pointer;font-size:14px;color:#666;transition:all .15s}
.tab.active{background:#1a1a1a;color:#fff}
.page{display:none;padding:16px}
.page.active{display:block}

.search-bar{background:#fff;border:1px solid #ddd;border-radius:8px;padding:8px 12px;width:100%;font-size:14px;margin-bottom:12px}
.filter-row{display:flex;gap:8px;margin-bottom:12px;flex-wrap:wrap}
.filter-btn{padding:5px 12px;border-radius:20px;border:1px solid #ddd;background:#fff;font-size:13px;cursor:pointer;transition:all .15s}
.filter-btn.active{background:#1a1a1a;color:#fff;border-color:#1a1a1a}
.content-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(270px,1fr));gap:10px}
.content-card{background:#fff;border:1.5px solid #e0e0e0;border-radius:10px;padding:14px;cursor:pointer;transition:all .15s;position:relative}
.content-card:hover{border-color:#999}
.content-card.selected-normal{border:2px solid #1a6ef5;background:#f0f5ff}
.content-card.selected-wait{border:2px solid #7c3aed;background:#f5f0ff}
.card-top{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:8px;gap:8px}
.card-name{font-weight:500;font-size:14px;line-height:1.4}
.badge{padding:3px 8px;border-radius:20px;font-size:11px;font-weight:500;white-space:nowrap;flex-shrink:0}
.badge-ok{background:#e6f4ea;color:#1e6e38}
.badge-few{background:#fff3e0;color:#b06000}
.badge-full{background:#fde8e8;color:#c0392b}
.badge-wait{background:#ede7f6;color:#512da8}
.seat-bar{height:6px;background:#eee;border-radius:3px;overflow:hidden;margin:8px 0}
.seat-fill{height:100%;border-radius:3px;transition:width .3s}
.seat-info{display:flex;justify-content:space-between;font-size:12px;color:#666}
.time-tag{font-size:12px;color:#888;margin-top:6px}
.sel-label{position:absolute;top:10px;right:10px;font-size:11px;font-weight:500;padding:2px 8px;border-radius:10px}
.sel-label.normal{background:#1a6ef5;color:#fff}
.sel-label.wait{background:#7c3aed;color:#fff}

.form-card{background:#fff;border:1px solid #e0e0e0;border-radius:10px;padding:20px;margin-bottom:12px}
.form-card h2{font-size:15px;font-weight:500;margin-bottom:14px}
.form-row{margin-bottom:12px}
.form-row label{display:block;font-size:13px;color:#555;margin-bottom:4px}
.form-row input{width:100%;padding:9px 12px;border:1px solid #ddd;border-radius:7px;font-size:14px;outline:none}
.form-row input:focus{border-color:#1a6ef5}
.form-row-half{display:flex;gap:10px;margin-bottom:12px}
.form-row-half .form-row{flex:1;margin-bottom:0}
.no-select-msg{text-align:center;color:#999;padding:30px 20px;font-size:14px}
.selected-list{display:flex;flex-direction:column;gap:8px;margin-bottom:14px}
.selected-item{display:flex;align-items:center;gap:8px;padding:10px 12px;border-radius:8px;font-size:14px}
.selected-item.normal{background:#f0f5ff;border:1px solid #b0c8ff}
.selected-item.wait{background:#f5f0ff;border:1px solid #c4b5fd}
.selected-item-name{flex:1;font-weight:500}
.selected-item-badge{font-size:11px;padding:2px 7px;border-radius:10px;white-space:nowrap}
.sit-normal{background:#1a6ef5;color:#fff}
.sit-wait{background:#7c3aed;color:#fff}
.remove-sel{border:none;background:none;color:#aaa;cursor:pointer;font-size:16px;padding:0 4px;line-height:1}
.remove-sel:hover{color:#c0392b}
.submit-btn{width:100%;padding:12px;background:#1a1a1a;color:#fff;border:none;border-radius:8px;font-size:15px;font-weight:500;cursor:pointer;margin-top:4px;transition:background .15s}
.submit-btn:hover{background:#333}
.hint{font-size:12px;color:#888;margin-top:8px;text-align:center}

.list-tab{padding:6px 14px;border-radius:6px;border:1px solid #ddd;background:#fff;cursor:pointer;font-size:13px;color:#555;transition:all .15s}
.list-tab.active{background:#1a1a1a;color:#fff;border-color:#1a1a1a}
.content-section{margin-bottom:12px}
.section-header{display:flex;align-items:center;justify-content:space-between;padding:10px 14px;background:#fff;border:1px solid #e0e0e0;border-radius:8px 8px 0 0;cursor:pointer}
.section-header h3{font-size:14px;font-weight:500}
.section-body{background:#fff;border:1px solid #e0e0e0;border-top:none;border-radius:0 0 8px 8px}
.reservation-row{display:flex;align-items:center;padding:10px 14px;border-bottom:1px solid #f0f0f0;gap:8px;flex-wrap:wrap}
.reservation-row:last-child{border-bottom:none}
.res-num{font-size:12px;font-weight:500;color:#888;min-width:28px}
.res-name{flex:1;font-size:14px;min-width:80px}
.res-time-slot{font-size:12px;color:#1a6ef5;background:#f0f5ff;padding:2px 7px;border-radius:10px}
.res-type{font-size:11px;padding:2px 7px;border-radius:10px}
.res-type.normal{background:#e6f4ea;color:#1e6e38}
.res-type.waiting{background:#ede7f6;color:#512da8}
.res-reg-time{font-size:12px;color:#aaa}
.cancel-btn{padding:4px 10px;border:1px solid #eee;background:#fff;border-radius:5px;font-size:12px;cursor:pointer;color:#c0392b;transition:all .15s}
.cancel-btn:hover{background:#fde8e8;border-color:#f5b5b5}
.promote-btn{padding:4px 10px;border:1px solid #b0c8ff;background:#f0f5ff;border-radius:5px;font-size:12px;cursor:pointer;color:#1a6ef5;transition:all .15s}
.promote-btn:hover{background:#ddeaff}
.empty-row{padding:20px;text-align:center;color:#aaa;font-size:13px}

.setting-card{background:#fff;border:1px solid #e0e0e0;border-radius:10px;padding:20px;margin-bottom:12px}
.setting-card h2{font-size:15px;font-weight:500;margin-bottom:6px}
.setting-card .sub{font-size:12px;color:#888;margin-bottom:14px}
.col-labels{display:flex;gap:6px;padding:0 0 4px;align-items:flex-end}
.col-label{font-size:11px;color:#999}
.content-setting-row{display:flex;align-items:center;padding:8px 0;border-bottom:1px solid #f0f0f0;gap:6px}
.content-setting-row:last-child{border-bottom:none}
.csrow-name{flex:2;min-width:100px;padding:6px 10px;border:1px solid #ddd;border-radius:6px;font-size:13px}
.csrow-time{flex:1;min-width:80px;padding:6px 8px;border:1px solid #ddd;border-radius:6px;font-size:13px}
.csrow-num{width:54px;padding:5px 6px;border:1px solid #ddd;border-radius:6px;font-size:13px;text-align:center}
.csrow-num.rem-input{border-color:#1a6ef5;background:#f0f5ff}
.add-btn{width:100%;padding:10px;border:1px dashed #ccc;background:#fafafa;border-radius:8px;font-size:14px;color:#666;cursor:pointer;margin-top:8px}
.add-btn:hover{background:#f0f0f0}
.del-btn{padding:4px 8px;border:none;background:none;color:#ccc;cursor:pointer;font-size:16px;flex-shrink:0}
.del-btn:hover{color:#c0392b}
.save-btn{padding:10px 24px;background:#1a1a1a;color:#fff;border:none;border-radius:7px;font-size:14px;cursor:pointer}

.toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%) translateY(80px);background:#1a1a1a;color:#fff;padding:10px 20px;border-radius:8px;font-size:14px;opacity:0;transition:all .3s;z-index:999;white-space:nowrap;max-width:90vw;text-align:center}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0)}
.modal-bg{position:fixed;inset:0;background:rgba(0,0,0,.4);display:none;align-items:center;justify-content:center;z-index:200}
.modal-bg.show{display:flex}
.modal{background:#fff;border-radius:12px;padding:24px;width:90%;max-width:360px}
.modal h3{font-size:16px;font-weight:500;margin-bottom:8px}
.modal p{font-size:14px;color:#555;margin-bottom:20px}
.modal-btns{display:flex;gap:8px}
.modal-btns button{flex:1;padding:10px;border-radius:7px;border:1px solid #ddd;font-size:14px;cursor:pointer}
.modal-confirm{background:#c0392b!important;color:#fff!important;border-color:#c0392b!important}

/* 予約完了票 */
.rcpt-modal-bg{position:fixed;inset:0;background:rgba(0,0,0,.55);display:flex;align-items:center;justify-content:center;z-index:300;padding:16px}
.rcpt-modal{background:#fff;border-radius:16px;width:100%;max-width:400px;overflow:hidden;box-shadow:0 8px 32px rgba(0,0,0,.25)}
.rcpt-header{background:linear-gradient(135deg,#1a1a2e,#16213e);color:#fff;padding:20px;text-align:center}
.rcpt-title{font-size:18px;font-weight:700;letter-spacing:.05em}
.rcpt-subtitle{font-size:13px;color:#aac4ff;margin-top:4px}
.rcpt-guest{display:flex;align-items:center;justify-content:space-between;padding:14px 20px;background:#f8f9ff;border-bottom:1px solid #e8eaf6}
.rcpt-guest-name{font-size:18px;font-weight:700}
.rcpt-guest-time{font-size:12px;color:#888}
.rcpt-section-label{font-size:12px;font-weight:600;color:#555;padding:10px 20px 4px;background:#f5f5f5;letter-spacing:.05em}
.rcpt-section-label.wait-label{color:#7c3aed}
.rcpt-row{display:flex;align-items:center;gap:12px;padding:14px 20px;border-bottom:1px solid #f0f0f0}
.rcpt-normal{background:#fff}
.rcpt-wait{background:#fdf8ff}
.rcpt-icon{font-size:20px;flex-shrink:0}
.rcpt-info{flex:1;min-width:0}
.rcpt-name{font-size:15px;font-weight:600;line-height:1.3}
.rcpt-time{font-size:12px;color:#666;margin-top:3px}
.rcpt-badge{flex-shrink:0;padding:5px 12px;border-radius:20px;font-size:13px;font-weight:700}
.rcpt-badge-normal{background:#e6f4ea;color:#1e6e38}
.rcpt-badge-wait{background:#ede7f6;color:#6d28d9;min-width:72px;text-align:center}
.rcpt-notice{font-size:12px;color:#666;padding:14px 20px;background:#fffbeb;border-top:1px solid #fde68a;line-height:1.7;text-align:center}
.rcpt-close{display:block;width:100%;padding:14px;background:#1a1a1a;color:#fff;border:none;font-size:15px;font-weight:500;cursor:pointer}
.rcpt-close:hover{background:#333}
</style>
</head>
<body>

<div class="header">
  <h1>🎓 オープンキャンパス予約管理</h1>
  <div class="tab-bar">
    <button class="tab active" onclick="showPage('list',this)">コンテンツ</button>
    <button class="tab" onclick="showPage('book',this)">予約受付</button>
    <button class="tab" onclick="showPage('manage',this)">予約一覧</button>
    <button class="tab" onclick="showPage('settings',this)">設定</button>
  </div>
</div>

<div class="page active" id="page-list">
  <input class="search-bar" type="text" id="search" placeholder="🔍 コンテンツを検索..." oninput="renderContentList()">
  <div class="filter-row">
    <button class="filter-btn active" onclick="setFilter('all',this)">すべて</button>
    <button class="filter-btn" onclick="setFilter('ok',this)">空きあり</button>
    <button class="filter-btn" onclick="setFilter('few',this)">残りわずか</button>
    <button class="filter-btn" onclick="setFilter('full',this)">満席</button>
    <button class="filter-btn" onclick="setFilter('wait',this)">キャンセル待ち</button>
  </div>
  <div class="content-grid" id="content-grid"></div>
</div>

<div class="page" id="page-book">
  <div class="form-card">
    <h2>① コンテンツを選択（複数可）</h2>
    <p style="font-size:13px;color:#888;margin-bottom:14px">コンテンツ一覧タブからタップで選択。残席0は自動でキャンセル待ちになります。</p>
    <div id="selected-list-area"><div class="no-select-msg">コンテンツが選択されていません</div></div>
    <button style="font-size:13px;color:#1a6ef5;border:none;background:none;cursor:pointer;text-decoration:underline;margin-top:4px" onclick="goToList()">＋ コンテンツ一覧から選ぶ</button>
  </div>
  <div class="form-card">
    <h2>② お客様情報</h2>
    <div class="form-row-half">
      <div class="form-row">
        <label>氏名 <span style="font-size:11px;color:#aaa">（キャンセル待ちの場合は必須）</span></label>
        <input type="text" id="input-name" placeholder="例：山田 太郎">
      </div>
      <div class="form-row">
        <label>希望時間帯 <span style="font-size:11px;color:#aaa">（任意）</span></label>
        <input type="text" id="input-timeslot" placeholder="例：10:00〜">
      </div>
    </div>
    <button class="submit-btn" onclick="submitReservation()">選択したコンテンツを一括登録する</button>
    <p class="hint" id="submit-hint"></p>
  </div>
</div>

<div class="page" id="page-manage">
  <div class="filter-row">
    <button class="list-tab active" id="lt-all" onclick="setListTab('all')">全コンテンツ</button>
    <button class="list-tab" id="lt-wait" onclick="setListTab('wait')">キャンセル待ちのみ</button>
  </div>
  <div id="manage-list"></div>
</div>

<div class="page" id="page-settings">
  <div class="setting-card">
    <h2>コンテンツ設定</h2>
    <p class="sub">「当日残席」は当日開始時点の空き枠数（0も入力可）。定員は参考表示用。</p>
    <div class="col-labels">
      <div style="flex:2;min-width:100px"><span class="col-label">コンテンツ名</span></div>
      <div style="flex:1;min-width:80px"><span class="col-label">時間帯</span></div>
      <div style="width:54px;text-align:center"><span class="col-label">定員</span></div>
      <div style="width:54px;text-align:center"><span class="col-label" style="color:#1a6ef5">当日残席</span></div>
      <div style="width:28px"></div>
    </div>
    <div id="settings-list"></div>
    <button class="add-btn" onclick="addContent()">＋ コンテンツを追加</button>
  </div>
  <div style="display:flex;gap:10px;align-items:center;flex-wrap:wrap">
    <button class="save-btn" onclick="saveSettings()">保存する</button>
    <span style="font-size:13px;color:#888">※保存後に予約受付が開始できます</span>
  </div>
</div>

<div class="modal-bg" id="modal">
  <div class="modal">
    <h3 id="modal-title"></h3>
    <p id="modal-msg"></p>
    <div class="modal-btns">
      <button onclick="closeModal()">キャンセル</button>
      <button class="modal-confirm" id="modal-confirm-btn">確認</button>
    </div>
  </div>
</div>
<div class="toast" id="toast"></div>

<script>
var contents = [
  {id:1,name:"キャンパスツアー（午前）",time:"10:00〜10:45",cap:20,remaining:5,reservations:[],waiting:[]},
  {id:2,name:"学科説明会（情報）",time:"10:00〜11:00",cap:15,remaining:0,reservations:[],waiting:[]},
  {id:3,name:"模擬授業：プログラミング",time:"11:00〜11:50",cap:25,remaining:12,reservations:[],waiting:[]},
  {id:4,name:"学食体験",time:"12:00〜13:00",cap:30,remaining:2,reservations:[],waiting:[]},
  {id:5,name:"キャンパスツアー（午後）",time:"13:00〜13:45",cap:20,remaining:8,reservations:[],waiting:[]},
  {id:6,name:"学科説明会（経営）",time:"13:00〜14:00",cap:15,remaining:0,reservations:[],waiting:[]},
  {id:7,name:"模擬授業：マーケティング",time:"14:00〜14:50",cap:25,remaining:10,reservations:[],waiting:[]},
  {id:8,name:"個別相談（入試）",time:"随時",cap:10,remaining:3,reservations:[],waiting:[]},
];
var nextId = 100;
var selectedItems = [];
var listTab = 'all';
var filterMode = 'all';
var modalCallback = null;

function getContent(id){ return contents.find(function(c){return c.id===id;}); }

function getStatus(c){
  if(c.remaining>0) return c.remaining<=3?'few':'ok';
  if(c.waiting.length>0) return 'wait';
  return 'full';
}
function getStatusLabel(c){
  var st=getStatus(c);
  if(st==='ok') return{cls:'badge-ok',text:'残 '+c.remaining+'席'};
  if(st==='few') return{cls:'badge-few',text:'残 '+c.remaining+'席'};
  if(st==='full') return{cls:'badge-full',text:'満席'};
  return{cls:'badge-wait',text:'待 '+c.waiting.length+'名'};
}
function getPct(c){ return c.cap>0?Math.min(100,Math.round((c.cap-c.remaining)/c.cap*100)):100; }
function isSelected(id){ return selectedItems.some(function(s){return s.contentId===id;}); }
function isSelectedNormal(id){ return selectedItems.some(function(s){return s.contentId===id&&s.type==='normal';}); }
function isSelectedWait(id){ return selectedItems.some(function(s){return s.contentId===id&&s.type==='wait';}); }

function showPage(name,el){
  document.querySelectorAll('.page').forEach(function(p){p.classList.remove('active');});
  document.querySelectorAll('.tab').forEach(function(t){t.classList.remove('active');});
  document.getElementById('page-'+name).classList.add('active');
  if(el) el.classList.add('active');
  if(name==='manage') renderManage();
  if(name==='settings') renderSettings();
  if(name==='list') renderContentList();
  if(name==='book'){renderBookSelectionArea();updateSubmitHint();}
}

function setFilter(mode,el){
  filterMode=mode;
  document.querySelectorAll('.filter-btn').forEach(function(b){b.classList.remove('active');});
  el.classList.add('active');
  renderContentList();
}

function renderContentList(){
  var q=document.getElementById('search').value.toLowerCase();
  var grid=document.getElementById('content-grid');
  var filtered=contents.filter(function(c){
    if(q&&c.name.toLowerCase().indexOf(q)<0) return false;
    var st=getStatus(c);
    if(filterMode==='all') return true;
    return st===filterMode;
  });
  grid.innerHTML=filtered.map(function(c){
    var lbl=getStatusLabel(c);
    var pct=getPct(c);
    var barColor=c.remaining===0?'#e74c3c':c.remaining<=3?'#e67e22':'#27ae60';
    var cls=isSelectedNormal(c.id)?'selected-normal':isSelectedWait(c.id)?'selected-wait':'';
    var selBadge=isSelectedNormal(c.id)?'<span class="sel-label normal">✓ 予約</span>':isSelectedWait(c.id)?'<span class="sel-label wait">✓ 待機</span>':'';
    return '<div class="content-card '+cls+'" onclick="toggleSelect('+c.id+')">'
      +selBadge
      +'<div class="card-top"><div class="card-name">'+c.name+'</div>'
      +'<span class="badge '+lbl.cls+'">'+lbl.text+'</span></div>'
      +'<div class="seat-bar"><div class="seat-fill" style="width:'+pct+'%;background:'+barColor+'"></div></div>'
      +'<div class="seat-info"><span>残席 '+c.remaining+'席</span><span>待機：'+c.waiting.length+'名</span></div>'
      +(c.time?'<div class="time-tag">🕐 '+c.time+'</div>':'')
      +'</div>';
  }).join('');
  if(filtered.length===0) grid.innerHTML='<div style="grid-column:1/-1;text-align:center;color:#aaa;padding:30px;font-size:14px">該当するコンテンツがありません</div>';
}

function toggleSelect(id){
  var c=getContent(id);
  if(isSelected(id)){
    selectedItems=selectedItems.filter(function(s){return s.contentId!==id;});
  } else {
    var type=c.remaining>0?'normal':'wait';
    selectedItems.push({contentId:id,type:type});
  }
  renderContentList();
  if(document.getElementById('page-book').classList.contains('active')){
    renderBookSelectionArea();
    updateSubmitHint();
  }
}

function goToList(){
  document.querySelectorAll('.page').forEach(function(p){p.classList.remove('active');});
  document.querySelectorAll('.tab').forEach(function(t){t.classList.remove('active');});
  document.getElementById('page-list').classList.add('active');
  document.querySelectorAll('.tab')[0].classList.add('active');
  renderContentList();
}

function renderBookSelectionArea(){
  var area=document.getElementById('selected-list-area');
  if(selectedItems.length===0){
    area.innerHTML='<div class="no-select-msg">コンテンツが選択されていません</div>';
    return;
  }
  var rows=selectedItems.map(function(s){
    var c=getContent(s.contentId);
    if(!c) return '';
    var cls=s.type==='normal'?'normal':'wait';
    var badgeCls=s.type==='normal'?'sit-normal':'sit-wait';
    var badgeText=s.type==='normal'?'通常予約':'キャンセル待ち';
    return '<div class="selected-item '+cls+'">'
      +'<span class="selected-item-name">'+c.name+(c.time?' <span style="font-size:12px;color:#888">（'+c.time+'）</span>':'')+'</span>'
      +'<span class="selected-item-badge '+badgeCls+'">'+badgeText+'</span>'
      +'<button class="remove-sel" onclick="removeSelected('+c.id+')">×</button>'
      +'</div>';
  }).join('');
  area.innerHTML='<div class="selected-list">'+rows+'</div>';
}

function removeSelected(id){
  selectedItems=selectedItems.filter(function(s){return s.contentId!==id;});
  renderBookSelectionArea();
  updateSubmitHint();
  renderContentList();
}

function updateSubmitHint(){
  var hint=document.getElementById('submit-hint');
  if(!hint) return;
  var n=selectedItems.filter(function(s){return s.type==='normal';}).length;
  var w=selectedItems.filter(function(s){return s.type==='wait';}).length;
  var parts=[];
  if(n>0) parts.push('通常予約 '+n+'件');
  if(w>0) parts.push('キャンセル待ち '+w+'件');
  hint.textContent=parts.length>0?'登録予定：'+parts.join(' ／ '):'';
}

function submitReservation(){
  var name=document.getElementById('input-name').value.trim();
  var timeslot=document.getElementById('input-timeslot').value.trim();
  var hasWaitItem=selectedItems.some(function(s){return s.type==='wait';});
  if(!name&&hasWaitItem){showToast('⚠️ キャンセル待ち登録には氏名が必要です');return;}
  if(selectedItems.length===0){showToast('⚠️ コンテンツを選択してください');return;}

  var regTime=new Date().toLocaleTimeString('ja-JP',{hour:'2-digit',minute:'2-digit'});
  var receiptData=[];
  var toProcess=selectedItems.slice();

  toProcess.forEach(function(s){
    var c=getContent(s.contentId);
    if(!c) return;
    var entry={id:nextId++,name:name,timeslot:timeslot,regTime:regTime};
    if(s.type==='normal'&&c.remaining>0){
      c.reservations.push(entry);
      c.remaining--;
      receiptData.push({contentId:c.id,type:'normal',timeslot:timeslot});
    } else {
      c.waiting.push(entry);
      receiptData.push({contentId:c.id,type:'wait',waitNo:c.waiting.length,timeslot:timeslot});
    }
  });

  document.getElementById('input-name').value='';
  document.getElementById('input-timeslot').value='';
  selectedItems=[];
  renderBookSelectionArea();
  updateSubmitHint();
  renderContentList();
  showReceiptModal(name,timeslot,receiptData);
}

function showReceiptModal(name,timeslot,receiptData){
  var normals=receiptData.filter(function(d){return d.type==='normal';});
  var waits=receiptData.filter(function(d){return d.type==='wait';});
  var now=new Date().toLocaleString('ja-JP',{month:'numeric',day:'numeric',hour:'2-digit',minute:'2-digit'});

  var normalRows=normals.map(function(d){
    var c=getContent(d.contentId);
    return '<div class="rcpt-row rcpt-normal">'
      +'<div class="rcpt-icon">✅</div>'
      +'<div class="rcpt-info">'
      +'<div class="rcpt-name">'+c.name+'</div>'
      +(c.time?'<div class="rcpt-time">🕐 '+c.time+'</div>':'')
      +(d.timeslot?'<div class="rcpt-time">希望：'+d.timeslot+'</div>':'')
      +'</div>'
      +'<div class="rcpt-badge rcpt-badge-normal">予約済</div>'
      +'</div>';
  }).join('');

  var waitRows=waits.map(function(d){
    var c=getContent(d.contentId);
    return '<div class="rcpt-row rcpt-wait">'
      +'<div class="rcpt-icon">🕐</div>'
      +'<div class="rcpt-info">'
      +'<div class="rcpt-name">'+c.name+'</div>'
      +(c.time?'<div class="rcpt-time">🕐 '+c.time+'</div>':'')
      +(d.timeslot?'<div class="rcpt-time">希望：'+d.timeslot+'</div>':'')
      +'</div>'
      +'<div class="rcpt-badge rcpt-badge-wait">待機 '+d.waitNo+'番</div>'
      +'</div>';
  }).join('');

  var hasWait=waits.length>0;
  var html='<div class="rcpt-modal-bg" id="rcpt-modal" onclick="closeReceiptOnBg(event)">'
    +'<div class="rcpt-modal">'
    +'<div class="rcpt-header"><div class="rcpt-title">🎓 オープンキャンパス</div><div class="rcpt-subtitle">予約確認票</div></div>'
    +'<div class="rcpt-guest"><span class="rcpt-guest-name">'+(name?name+' さん':'（氏名未入力）')+'</span><span class="rcpt-guest-time">'+now+' 受付</span></div>'
    +(normals.length>0?'<div class="rcpt-section-label">▼ ご予約のコンテンツ</div>'+normalRows:'')
    +(hasWait?'<div class="rcpt-section-label wait-label">▼ キャンセル待ち</div>'+waitRows:'')
    +'<div class="rcpt-notice">'
    +(hasWait?'キャンセル待ちの方は、5分前に総合インフォメーションにお越しください。<br>':'')
    +'この画面を 📸 スクリーンショットして保存してください。</div>'
    +'<button class="rcpt-close" onclick="closeReceipt()">閉じる</button>'
    +'</div></div>';

  var wrap=document.createElement('div');
  wrap.innerHTML=html;
  document.body.appendChild(wrap.firstElementChild);
}

function closeReceipt(){
  var el=document.getElementById('rcpt-modal');
  if(el) el.remove();
}
function closeReceiptOnBg(e){
  if(e.target.id==='rcpt-modal') closeReceipt();
}

function setListTab(t){
  listTab=t;
  document.getElementById('lt-all').classList.toggle('active',t==='all');
  document.getElementById('lt-wait').classList.toggle('active',t==='wait');
  renderManage();
}

function renderManage(){
  var el=document.getElementById('manage-list');
  var list=listTab==='wait'?contents.filter(function(c){return c.waiting.length>0;}):contents;
  if(list.length===0){el.innerHTML='<div style="text-align:center;color:#aaa;padding:30px;font-size:14px">該当データがありません</div>';return;}
  el.innerHTML=list.map(function(c){
    var timeStr=c.time?' ／ '+c.time+' ':'';
    var resRows=c.reservations.map(function(r,i){
      return '<div class="reservation-row">'
        +'<span class="res-num">'+(i+1)+'</span>'
        +'<span class="res-name">'+r.name+'</span>'
        +(r.timeslot?'<span class="res-time-slot">'+r.timeslot+'</span>':'')
        +'<span class="res-type normal">予約</span>'
        +'<span class="res-reg-time">'+r.regTime+'</span>'
        +'<button class="cancel-btn" onclick="cancelReservation('+c.id+','+r.id+',\'normal\')">取消</button>'
        +'</div>';
    }).join('');
    var waitRows=c.waiting.map(function(r,i){
      return '<div class="reservation-row">'
        +'<span class="res-num">待'+(i+1)+'</span>'
        +'<span class="res-name">'+r.name+'</span>'
        +(r.timeslot?'<span class="res-time-slot">'+r.timeslot+'</span>':'')
        +'<span class="res-type waiting">待機</span>'
        +'<span class="res-reg-time">'+r.regTime+'</span>'
        +(i===0?'<button class="promote-btn" onclick="promoteWaiting('+c.id+')">繰上げ</button>':'')
        +'<button class="cancel-btn" onclick="cancelReservation('+c.id+','+r.id+',\'wait\')">取消</button>'
        +'</div>';
    }).join('');
    var emptyRow=(c.reservations.length===0&&c.waiting.length===0)?'<div class="empty-row">予約なし</div>':'';
    var waitHeader=c.waiting.length>0?'<div style="padding:6px 14px;font-size:12px;color:#888;background:#faf8ff;border-top:1px solid #f0f0f0">--- キャンセル待ち ---</div>':'';
    return '<div class="content-section">'
      +'<div class="section-header" onclick="toggleSection('+c.id+')">'
      +'<h3>'+c.name+'<span style="color:#888;font-size:11px;font-weight:400">'+timeStr+'（残席'+c.remaining+'・予約'+c.reservations.length+'件・待機'+c.waiting.length+'名）</span></h3>'
      +'<span style="font-size:12px;color:#999" id="toggle-'+c.id+'">▼</span>'
      +'</div>'
      +'<div class="section-body" id="body-'+c.id+'">'
      +emptyRow+resRows+waitHeader+waitRows
      +'</div></div>';
  }).join('');
}

function toggleSection(id){
  var body=document.getElementById('body-'+id);
  var tog=document.getElementById('toggle-'+id);
  var hidden=body.style.display==='none';
  body.style.display=hidden?'block':'none';
  tog.textContent=hidden?'▼':'▶';
}

function cancelReservation(cid,rid,type){
  showModal('予約を取り消しますか？','この操作は元に戻せません。',function(){
    var c=getContent(cid);
    if(type==='normal'){
      c.reservations=c.reservations.filter(function(r){return r.id!==rid;});
      c.remaining++;
    } else {
      c.waiting=c.waiting.filter(function(r){return r.id!==rid;});
    }
    renderManage();renderContentList();
    showToast('✅ 予約を取り消しました');
  });
}

function promoteWaiting(cid){
  showModal('キャンセル待ちを繰り上げますか？','待機1番目の方を正式予約に変更します。',function(){
    var c=getContent(cid);
    if(c.waiting.length===0) return;
    var person=c.waiting.shift();
    c.reservations.push(person);
    if(c.remaining>0) c.remaining--;
    renderManage();renderContentList();
    showToast('✅ '+person.name+' さんを正式予約に繰り上げました');
  });
}

function renderSettings(){
  var el=document.getElementById('settings-list');
  el.innerHTML=contents.map(function(c){
    return '<div class="content-setting-row">'
      +'<input class="csrow-name" value="'+c.name+'" id="sname-'+c.id+'">'
      +'<input class="csrow-time" value="'+(c.time||'')+'" id="stime-'+c.id+'" placeholder="10:00〜10:45">'
      +'<input class="csrow-num" type="number" min="0" value="'+c.cap+'" id="scap-'+c.id+'">'
      +'<input class="csrow-num rem-input" type="number" min="0" value="'+c.remaining+'" id="srem-'+c.id+'">'
      +'<button class="del-btn" onclick="deleteContent('+c.id+')">✕</button>'
      +'</div>';
  }).join('');
}

function addContent(){
  contents.push({id:nextId++,name:"新しいコンテンツ",time:"",cap:20,remaining:20,reservations:[],waiting:[]});
  renderSettings();
}

function deleteContent(id){
  showModal('コンテンツを削除しますか？','予約・待機データも削除されます。',function(){
    contents=contents.filter(function(c){return c.id!==id;});
    selectedItems=selectedItems.filter(function(s){return s.contentId!==id;});
    renderSettings();renderContentList();
    showToast('削除しました');
  });
}

function saveSettings(){
  contents.forEach(function(c){
    var n=document.getElementById('sname-'+c.id);
    var t=document.getElementById('stime-'+c.id);
    var cp=document.getElementById('scap-'+c.id);
    var rm=document.getElementById('srem-'+c.id);
    if(n) c.name=n.value;
    if(t) c.time=t.value;
    if(cp) c.cap=parseInt(cp.value)||0;
    if(rm) c.remaining=Math.max(0,parseInt(rm.value)||0);
  });
  renderContentList();
  showToast('✅ 設定を保存しました');
}

function showModal(title,msg,cb){
  document.getElementById('modal-title').textContent=title;
  document.getElementById('modal-msg').textContent=msg;
  document.getElementById('modal').classList.add('show');
  modalCallback=cb;
  document.getElementById('modal-confirm-btn').onclick=function(){closeModal();if(modalCallback)modalCallback();};
}
function closeModal(){ document.getElementById('modal').classList.remove('show'); }

function showToast(msg){
  var t=document.getElementById('toast');
  t.textContent=msg;
  t.classList.add('show');
  setTimeout(function(){t.classList.remove('show');},3000);
}

renderContentList();
</script>
</body>
</html>
