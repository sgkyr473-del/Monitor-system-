<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>کامپیوتر سه‌بعدی | سازنده: محمد متین ارامتن</title>
<style>
  :root{
    --paper:#f2f5f8; --ink:#16222e; --muted:#566676; --line:#c8d3de;
    --accent:#2f7cf6; --accent-ink:#ffffff;
  }
  *{box-sizing:border-box}
  html,body{margin:0;height:100%;overflow:hidden}
  body{
    font-family:Vazirmatn,Vazir,"Segoe UI",Tahoma,sans-serif;
    color:var(--ink);
    background:linear-gradient(180deg,#3a566f 0%,#223547 55%,#16232f 100%);
  }
  #stage{position:fixed;inset:0}
  #stage canvas{display:block;width:100%;height:100%;touch-action:none;cursor:grab}
  #stage canvas:active{cursor:grabbing}

  header{
    position:fixed;top:0;left:0;right:0;z-index:2;
    padding:max(10px,env(safe-area-inset-top)) 14px 8px;
    display:flex;flex-direction:column;gap:8px;pointer-events:none;
  }
  .brand{color:#eef4fa}
  .brand h1{margin:0;font-size:20px;font-weight:800;line-height:1.4}
  .brand p{margin:0;font-size:14px;color:#b9cde0}
  .brand b{color:#fff;font-weight:700}
  .bar{display:flex;flex-wrap:wrap;gap:6px;pointer-events:auto}
  .bar button{
    font:inherit;font-size:13px;font-weight:600;color:var(--ink);
    background:var(--paper);border:1px solid var(--line);border-radius:999px;
    padding:7px 14px;cursor:pointer;
    transition:background .15s,color .15s,border-color .15s;
  }
  .bar button:hover{border-color:var(--accent)}
  .bar button[aria-pressed="true"]{background:var(--accent);border-color:var(--accent);color:var(--accent-ink)}
  button:focus-visible{outline:3px solid #ffd166;outline-offset:2px}

  #dock{
    position:fixed;left:0;right:0;bottom:0;z-index:2;
    padding:8px 10px max(10px,env(safe-area-inset-bottom));
    display:flex;flex-direction:column;align-items:center;gap:8px;pointer-events:none;
  }
  #chips{
    width:100%;max-width:900px;display:flex;align-items:center;gap:6px;
    overflow-x:auto;pointer-events:auto;padding:2px 2px 6px;scrollbar-width:thin;
  }
  .gl{flex:none;font-size:12px;color:#b9cde0;padding:0 4px 0 2px}
  .chip{
    flex:none;font:inherit;font-size:13px;color:var(--ink);
    background:rgba(242,245,248,.92);border:1px solid var(--line);
    border-radius:8px;padding:6px 11px;cursor:pointer;
  }
  .chip[aria-pressed="true"]{background:var(--accent);border-color:var(--accent);color:#fff}
  #card{
    width:100%;max-width:900px;max-height:44vh;overflow:auto;pointer-events:auto;
    background:var(--paper);border-radius:14px;padding:12px 16px 14px;
    box-shadow:0 10px 30px rgba(0,0,0,.35);
  }
  #card .tag{display:inline-block;font-size:12px;color:var(--muted);margin-bottom:2px}
  #card h2{margin:0 0 6px;font-size:18px;font-weight:800;line-height:1.5}
  #card p{margin:0;font-size:14.5px;line-height:2;max-width:70ch}
  #card .head{display:flex;align-items:flex-start;justify-content:space-between;gap:10px}
  #cToggle{
    flex:none;font:inherit;font-size:12.5px;color:var(--ink);background:#e2e9f0;
    border:1px solid var(--line);border-radius:999px;padding:4px 12px;cursor:pointer;
  }
  #cMore{margin-top:10px;display:grid;gap:12px}
  #cMore section[hidden]{display:none}
  #cMore h3{margin:0 0 4px;font-size:14px;font-weight:800;color:#1c3f78}
  #cMore ul{margin:0;padding:0 18px 0 0;font-size:14px;line-height:1.95}
  #cMore li{margin-bottom:2px}
  #cMore p{font-size:14px;line-height:1.95}
  #sFact,#sTip{padding:8px 12px;border-radius:10px}
  #sFact{background:#e6eefb}
  #sTip{background:#fff3d6}
  #sTip h3{color:#7a5200}
  #card.min #cMore{display:none}
  @media (min-width:760px){
    #cMore{grid-template-columns:1fr 1fr;align-items:start}
    #sHow{grid-row:span 2}
  }

  #err{
    position:fixed;inset:0;z-index:5;display:flex;align-items:center;justify-content:center;
    background:#16232f;color:#fff;padding:24px;text-align:center;line-height:2;
  }
  #err[hidden]{display:none}

  @media (max-width:520px){
    .brand h1{font-size:17px}
    #card{max-height:32vh}
    #card p{font-size:14px;line-height:1.9}
  }
</style>
</head>
<body>
<div id="stage" aria-label="نمای سه‌بعدی مانیتور و کیس کامپیوتر"></div>

<header>
  <div class="brand">
    <h1>مانیتور و کیس کامپیوتر (سه‌بعدی)</h1>
    <p>سازنده: <b>محمد متین ارامتن</b></p>
  </div>
  <div class="bar" role="toolbar" aria-label="کنترل‌ها">
    <button id="bOpen" aria-pressed="false">باز کردن کیس</button>
    <button id="bExp" aria-pressed="false">نمای انفجاری</button>
    <button id="bAuto" aria-pressed="false">چرخش خودکار</button>
    <button id="bReset">نمای کلی</button>
  </div>
</header>

<section id="dock">
  <div id="chips" role="list" aria-label="فهرست قطعات"></div>
  <article id="card" aria-live="polite">
    <div class="head">
      <div>
        <span class="tag" id="cTag">راهنما</span>
        <h2 id="cTitle">اجزای کامپیوتر</h2>
      </div>
      <button id="cToggle" type="button" aria-expanded="true">کوچک کردن</button>
    </div>
    <p id="cDesc">روی هر قسمت بزنید یا از فهرست بالا انتخاب کنید تا توضیح کاملش را ببینید: این قطعه چیست، چطور کار می‌کند، مشخصات رایجش چیست و چه نکته‌ی جالبی دارد. برای چرخاندن بکشید و برای نزدیک و دور کردن از دو انگشت یا چرخ ماوس استفاده کنید. با «باز کردن کیس» درب شیشه‌ای کنار می‌رود و داخل کیس دیده می‌شود.</p>
    <div id="cMore">
      <section id="sHow" hidden><h3>چطور کار می‌کند؟</h3><ul id="cHow"></ul></section>
      <section id="sSpec" hidden><h3>مشخصات رایج</h3><ul id="cSpec"></ul></section>
      <section id="sFact" hidden><h3>نکته‌ی جالب</h3><p id="cFact"></p></section>
      <section id="sTip" hidden><h3>نکته‌ی کاربردی</h3><p id="cTip"></p></section>
    </div>
  </article>
</section>

<div id="err" hidden>برای نمایش سه‌بعدی، کتابخانه‌ی Three.js باید بارگذاری شود. اتصال اینترنت را بررسی کنید و صفحه را دوباره باز کنید.</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
(function(){
'use strict';
if(typeof THREE==='undefined'){document.getElementById('err').hidden=false;return;}

const $=s=>document.querySelector(s);
const clamp=(v,a,b)=>Math.min(b,Math.max(a,v));
const reduceMotion=window.matchMedia&&matchMedia('(prefers-reduced-motion: reduce)').matches;
const FONT='Vazirmatn,Vazir,Tahoma,sans-serif';

/* ---------- صحنه ---------- */
const stage=$('#stage');
const renderer=new THREE.WebGLRenderer({antialias:true,alpha:true});
renderer.setPixelRatio(Math.min(window.devicePixelRatio||1,2));
stage.appendChild(renderer.domElement);
const cv=renderer.domElement;
const scene=new THREE.Scene();
const camera=new THREE.PerspectiveCamera(38,1,0.1,100);

scene.add(new THREE.HemisphereLight(0xe3edff,0x2a3b4d,0.95));
const key=new THREE.DirectionalLight(0xffffff,0.85);key.position.set(4,8,7);scene.add(key);
const rim=new THREE.DirectionalLight(0x8fb4ff,0.45);rim.position.set(-6,4,-5);scene.add(rim);

/* ---------- ابزارهای کمکی ---------- */
const mat=(c,o)=>new THREE.MeshStandardMaterial(Object.assign({color:c,roughness:.5,metalness:.3},o||{}));
const box=(w,h,d,m,x,y,z)=>{const b=new THREE.Mesh(new THREE.BoxGeometry(w,h,d),m);b.position.set(x||0,y||0,z||0);return b;};
function rr(g,x,y,w,h,r){g.beginPath();g.moveTo(x+r,y);g.arcTo(x+w,y,x+w,y+h,r);g.arcTo(x+w,y+h,x,y+h,r);g.arcTo(x,y+h,x,y,r);g.arcTo(x,y,x+w,y,r);g.closePath();}
function canvasTex(w,h,fn){const c=document.createElement('canvas');c.width=w;c.height=h;fn(c.getContext('2d'),w,h);const t=new THREE.CanvasTexture(c);t.anisotropy=4;return t;}
function seeded(s){return()=>{s=(s*1664525+1013904223)%4294967296;return s/4294967296;};}

const rgb=[];            // مواد نورپردازی RGB
const spinners=[];       // پره‌های فن
const ledMat=off=>{const m=new THREE.MeshBasicMaterial({color:0xffffff});rgb.push({m,o:off||0});return m;};

function makeFan(r,off){
  const g=new THREE.Group();
  g.add(new THREE.Mesh(new THREE.TorusGeometry(r,r*.08,8,40),mat(0x14181e,{roughness:.6})));
  const led=new THREE.Mesh(new THREE.TorusGeometry(r*.9,r*.035,6,40),ledMat(off));
  g.add(led);
  const rotor=new THREE.Group();g.add(rotor);
  const bm=mat(0x2a313c,{roughness:.5,metalness:.2});
  for(let i=0;i<7;i++){
    const p=new THREE.Group();p.rotation.z=i*Math.PI*2/7;
    const b=box(r*.38,r*.72,.012,bm,0,r*.55,0);b.rotation.y=.55;
    p.add(b);rotor.add(p);
  }
  const hub=new THREE.Mesh(new THREE.CylinderGeometry(r*.24,r*.24,.04,20),mat(0x0f1217));
  hub.rotation.x=Math.PI/2;rotor.add(hub);
  spinners.push(rotor);
  return g;
}

/* ---------- ثبت قطعات ---------- */
const parts={},pickables=[];
function reg(id,obj,info,explode){
  obj.userData.base=obj.position.clone();
  obj.userData.explode=explode||new THREE.Vector3();
  obj.traverse(o=>{if(o.isMesh){o.userData.partId=id;pickables.push(o);}});
  parts[id]=Object.assign({obj},info);
}
const V=(x,y,z)=>new THREE.Vector3(x,y,z);

/* ---------- میز ---------- */
const desk=box(10.4,.12,4.4,mat(0x34475a,{roughness:.8,metalness:.1}),0,-.06,.3);
scene.add(desk);
const shadowTex=canvasTex(128,128,(g,w,h)=>{
  const gr=g.createRadialGradient(w/2,h/2,4,w/2,h/2,w/2);
  gr.addColorStop(0,'rgba(0,0,0,.55)');gr.addColorStop(1,'rgba(0,0,0,0)');
  g.fillStyle=gr;g.fillRect(0,0,w,h);
});
function blob(w,d,x,y,z){
  const m=new THREE.Mesh(new THREE.PlaneGeometry(w,d),new THREE.MeshBasicMaterial({map:shadowTex,transparent:true,depthWrite:false}));
  m.rotation.x=-Math.PI/2;m.position.set(x,y,z);return m;
}

/* ================= مانیتور ================= */
const monitor=new THREE.Group();
monitor.position.set(-2.4,0,.2);monitor.rotation.y=.45;scene.add(monitor);
monitor.add(blob(3.4,1.9,0,.01,.0));

const dark=mat(0x151a21,{roughness:.45,metalness:.5});
const silver=mat(0x9aa7b4,{roughness:.35,metalness:.8});

// صفحه
const sc=document.createElement('canvas');sc.width=1024;sc.height=576;
const stex=new THREE.CanvasTexture(sc);stex.anisotropy=4;
function drawScreen(){
  const g=sc.getContext('2d'),w=1024,h=576;
  const bg=g.createLinearGradient(0,0,w,h);
  bg.addColorStop(0,'#1e40af');bg.addColorStop(.6,'#5b21b6');bg.addColorStop(1,'#0e7490');
  g.fillStyle=bg;g.fillRect(0,0,w,h);
  g.fillStyle='rgba(255,255,255,.07)';
  g.beginPath();g.arc(120,500,230,0,7);g.fill();
  g.beginPath();g.arc(930,90,190,0,7);g.fill();
  // پنجره
  g.fillStyle='rgba(245,248,252,.97)';rr(g,150,64,724,390,16);g.fill();
  g.save();rr(g,150,64,724,390,16);g.clip();g.fillStyle='#dfe6ee';g.fillRect(150,64,724,50);g.restore();
  ['#ef4444','#f59e0b','#22c55e'].forEach((c,i)=>{g.fillStyle=c;g.beginPath();g.arc(182+i*28,89,8,0,7);g.fill();});
  g.direction='rtl';g.textAlign='center';
  g.fillStyle='#16222e';g.font='700 46px '+FONT;g.fillText('کامپیوتر سه‌بعدی',512,222);
  g.fillStyle='#2f5fd0';g.font='600 32px '+FONT;g.fillText('سازنده: محمد متین ارامتن',512,292);
  g.fillStyle='#566676';g.font='400 24px '+FONT;
  g.fillText('کیس را باز کنید و اجزای داخلی را ببینید',512,360);
  // نوار وظیفه
  g.fillStyle='rgba(15,23,42,.88)';g.fillRect(0,h-52,w,52);
  g.fillStyle='#38bdf8';rr(g,484,h-42,18,14,3);g.fill();rr(g,506,h-42,18,14,3);g.fill();
  rr(g,484,h-24,18,14,3);g.fill();rr(g,506,h-24,18,14,3);g.fill();
  g.fillStyle='#e2e8f0';g.font='500 22px '+FONT;g.textAlign='left';
  let t='';try{t=new Date().toLocaleTimeString('fa-IR',{hour:'2-digit',minute:'2-digit'});}catch(e){t=new Date().toTimeString().slice(0,5);}
  g.fillText(t,28,h-18);
  stex.needsUpdate=true;
}
drawScreen();setInterval(drawScreen,15000);

const screen=new THREE.Mesh(new THREE.PlaneGeometry(2.5,1.4),new THREE.MeshBasicMaterial({map:stex}));
screen.position.set(0,1.74,.056);
monitor.add(screen);
const bezel=new THREE.Group();bezel.position.set(0,0,0);
bezel.add(box(2.7,1.6,.1,dark,0,1.7,0));
bezel.add(box(1.2,.9,.2,dark,0,1.7,-.14));
const monBtn=box(.07,.025,.02,new THREE.MeshBasicMaterial({color:0x37e06b}),1.1,.93,.055);
bezel.add(monBtn);
monitor.add(bezel);
const neck=box(.22,1.0,.1,silver,0,.56,-.14);monitor.add(neck);
const mbase=box(1.3,.06,.8,silver,0,.03,.0);monitor.add(mbase);
const monLight=new THREE.PointLight(0x7aa7ff,.55,3.2);monLight.position.set(0,1.5,.9);monitor.add(monLight);

reg('screen',screen,{g:'مانیتور',n:'صفحه نمایش (پنل LCD)',
  d:'تصویر روی این بخش ساخته می‌شود. صفحه از میلیون‌ها پیکسل تشکیل شده که هر کدام با ترکیب سه رنگ قرمز، سبز و آبی رنگ دلخواه را نشان می‌دهد. در مانیتورهای امروزی یک لایه‌ی نور پس‌زمینه‌ی LED پشت پنل، تصویر را روشن می‌کند.',
  t:'نرخ نوسازی (مثلاً ۶۰ یا ۱۴۴ هرتز) مشخص می‌کند تصویر در هر ثانیه چند بار تازه می‌شود.'},V(0,.15,.9));
reg('bezel',bezel,{g:'مانیتور',n:'قاب و بدنه‌ی مانیتور',
  d:'بدنه‌ی پلاستیکی یا فلزی که پنل، برد الکترونیکی کنترل تصویر و منبع تغذیه‌ی داخلی مانیتور را نگه می‌دارد. پورت‌های HDMI، DisplayPort و برق معمولاً در پشت همین قسمت هستند.',
  t:'کابل HDMI یا DisplayPort، مانیتور را به کارت گرافیک وصل می‌کند.'});
reg('monbtn',monBtn,{g:'مانیتور',n:'دکمه‌ی روشن/خاموش مانیتور',
  d:'مانیتور را روشن یا خاموش می‌کند. چراغ کوچک آن معمولاً نشان می‌دهد مانیتور روشن است یا در حالت آماده‌باش قرار دارد.',t:''});
reg('neck',neck,{g:'مانیتور',n:'ستون نگه‌دارنده',
  d:'پنل را بالای میز نگه می‌دارد و بسته به مدل، امکان تغییر ارتفاع و زاویه‌ی مانیتور را فراهم می‌کند.',
  t:'ارتفاع مناسب: لبه‌ی بالای صفحه هم‌سطح یا کمی پایین‌تر از چشم باشد.'},V(0,0,-.5));
reg('base',mbase,{g:'مانیتور',n:'پایه‌ی مانیتور',
  d:'سطح تماس مانیتور با میز است و وزن پنل را متعادل می‌کند تا مانیتور واژگون نشود. بعضی مانیتورها به‌جای آن از بازوی نگه‌دارنده یا پایه‌ی دیواری (VESA) استفاده می‌کنند.',t:''},V(0,0,-1.0));

/* ---------- کیبورد و ماوس ---------- */
const kbTex=canvasTex(512,190,(g,w,h)=>{
  g.fillStyle='#c5ced8';g.fillRect(0,0,w,h);
  const rows=[14,14,13,12,6];
  rows.forEach((n,r)=>{
    const y=12+r*34,kw=(w-24)/n;
    for(let i=0;i<n;i++){
      g.fillStyle='#f5f7f9';
      if(r===4){ if(i===2){rr(g,12+i*kw,y,kw*4.2,28,5);g.fill();i+=3;continue;} }
      rr(g,12+i*kw+2,y,kw-4,28,5);g.fill();
    }
  });
});
const kbd=box(1.6,.07,.55,[mat(0x8b97a4),mat(0x8b97a4),new THREE.MeshStandardMaterial({map:kbTex,roughness:.7}),mat(0x8b97a4),mat(0x8b97a4),mat(0x8b97a4)],0,0,0);
kbd.position.set(-1.93,.035,1.19);kbd.rotation.y=.45;scene.add(kbd);
const mouse3=new THREE.Mesh(new THREE.SphereGeometry(1,24,16),mat(0x20262e,{roughness:.4,metalness:.4}));
mouse3.scale.set(.15,.075,.24);mouse3.position.set(-.85,.07,.67);mouse3.rotation.y=.45;scene.add(mouse3);
reg('kbd',kbd,{g:'میز',n:'کیبورد',
  d:'ابزار اصلی نوشتن و دادن فرمان به کامپیوتر است. با فشردن هر کلید یک سیگنال الکتریکی ساخته می‌شود که با کابل یا بلوتوث به کامپیوتر می‌رسد.',t:''});
reg('mouse',mouse3,{g:'میز',n:'ماوس',
  d:'حرکت دست شما را با یک حسگر نوری روی میز می‌خواند و به حرکت نشانگر روی صفحه تبدیل می‌کند.',t:''});

/* ================= کیس ================= */
const caseG=new THREE.Group();
caseG.position.set(2.7,1.2,0);caseG.rotation.y=-.5;scene.add(caseG);
caseG.add(blob(3.4,2.0,0,-1.19,0));

const body=mat(0x1d232c,{roughness:.5,metalness:.5});
const slatM=mat(0x2b3441,{roughness:.6,metalness:.4});

// بدنه
const chassis=new THREE.Group();
chassis.add(box(2.0,2.2,.04,body,0,0,-.5));      // دیواره‌ی پشتی
chassis.add(box(2.0,.04,1.0,body,0,1.1,0));      // سقف
chassis.add(box(2.0,.04,1.0,body,0,-1.1,0));     // کف
chassis.add(box(.04,2.2,1.0,body,-1.0,0,0));     // پنل عقب
[[-.85,-.4],[.85,-.4],[-.85,.4],[.85,.4]].forEach(p=>chassis.add(box(.12,.08,.12,mat(0x0d1014,{roughness:.9}),p[0],-1.16,p[1])));
caseG.add(chassis);

// پنل جلو
const front=new THREE.Group();front.position.set(1.0,0,0);
front.add(box(.05,2.2,1.0,body,0,0,0));
for(let i=0;i<15;i++)front.add(box(.015,2.0,.012,slatM,.032,0,-.43+i*.0615));
front.add(box(.02,2.0,.022,ledMat(.1),.035,0,.47));
const cBtn=new THREE.Mesh(new THREE.CylinderGeometry(.06,.06,.03,24),mat(0xb8c2cd,{metalness:.8,roughness:.3}));
cBtn.rotation.z=Math.PI/2;cBtn.position.set(.05,.92,-.05);front.add(cBtn);
const cBtnLed=new THREE.Mesh(new THREE.TorusGeometry(.065,.01,6,24),ledMat(.3));
cBtnLed.rotation.y=Math.PI/2;cBtnLed.position.set(.066,.92,-.05);front.add(cBtnLed);
caseG.add(front);

// مادربرد
const pcb=canvasTex(512,585,(g,w,h)=>{
  g.fillStyle='#0f1d27';g.fillRect(0,0,w,h);
  const rnd=seeded(7);g.strokeStyle='#1d3d4d';g.lineWidth=2;
  for(let i=0;i<90;i++){
    let x=rnd()*w,y=rnd()*h;g.beginPath();g.moveTo(x,y);
    for(let k=0;k<4;k++){ if(k%2)y+=(rnd()-.5)*160;else x+=(rnd()-.5)*160; g.lineTo(x,y);}
    g.stroke();
  }
  g.fillStyle='#2f5a70';
  for(let i=0;i<70;i++)g.fillRect(rnd()*w,rnd()*h,6,6);
});
const mobo=new THREE.Group();mobo.position.set(-.05,.1,-.44);
const edge=mat(0x0b141b,{roughness:.8});
mobo.add(box(1.4,1.6,.04,[edge,edge,edge,edge,new THREE.MeshStandardMaterial({map:pcb,roughness:.7,metalness:.2}),edge],0,0,0));
const alu=mat(0x6d7886,{roughness:.4,metalness:.8});
mobo.add(box(.14,.42,.07,alu,-.5,.35,.05));
mobo.add(box(.5,.12,.07,alu,-.05,.68,.05));
mobo.add(box(.28,.28,.07,mat(0x2a3340,{metalness:.7,roughness:.4}),.3,-.45,.05));
mobo.add(box(.05,.6,.03,mat(0x0c0f13),.4,.4,.035));
mobo.add(box(.05,.6,.03,mat(0x0c0f13),.5,.4,.035));
caseG.add(mobo);

// پردازنده و خنک‌کننده
const cpu=new THREE.Group();cpu.position.set(-.1,.45,-.41);
cpu.add(box(.3,.3,.02,mat(0xb9c1ca,{metalness:.8,roughness:.3}),0,0,.01));
cpu.add(box(.42,.42,.2,mat(0x8b96a3,{metalness:.85,roughness:.35}),0,0,.12));
const cpuFan=makeFan(.17,.2);cpuFan.position.z=.235;cpu.add(cpuFan);
caseG.add(cpu);

// رم
const ram=new THREE.Group();ram.position.set(.35,.4,-.35);
[0,.1].forEach((dx,i)=>{
  ram.add(box(.04,.55,.14,mat(0x3a4250,{metalness:.6,roughness:.4}),dx,0,0));
  ram.add(box(.05,.03,.12,ledMat(.4+i*.08),dx,.285,0));
});
caseG.add(ram);

// کارت گرافیک
const gpu=new THREE.Group();gpu.position.set(-.1,-.05,-.24);
gpu.add(box(1.1,.3,.2,mat(0x232a34,{metalness:.7,roughness:.35}),0,0,0));
gpu.add(box(.02,.34,.22,silver,-.56,0,0));
gpu.add(box(.1,.03,.06,mat(0x0c0f13),.42,.165,0));
gpu.add(box(1.0,.015,.012,ledMat(.55),0,.1,.103));
const gf1=makeFan(.12,.6),gf2=makeFan(.12,.65);
gf1.position.set(-.25,-.02,.108);gf2.position.set(.2,-.02,.108);
gpu.add(gf1,gf2);
caseG.add(gpu);

// SSD
const ssd=new THREE.Group();ssd.position.set(.1,.16,-.41);
ssd.add(box(.34,.08,.012,mat(0x1e3a5f,{roughness:.5}),0,0,0));
ssd.add(box(.16,.07,.016,mat(0x8e9aa8,{metalness:.8,roughness:.35}),.08,0,.004));
caseG.add(ssd);

// منبع تغذیه
const psuTex=canvasTex(512,128,(g,w,h)=>{
  g.fillStyle='#232a34';g.fillRect(0,0,w,h);
  g.fillStyle='#e8c14a';g.fillRect(0,h-10,w,10);
  g.fillStyle='#f2f5f8';g.font='700 56px '+FONT;g.textAlign='center';g.direction='ltr';
  g.fillText('850W  80+ GOLD',w/2,72);
});
const psu=new THREE.Group();psu.position.set(-.05,-.89,0);
psu.add(box(1.7,.38,.96,mat(0x161b22,{roughness:.55,metalness:.5}),0,0,0));
const psuLabel=new THREE.Mesh(new THREE.PlaneGeometry(.8,.2),new THREE.MeshBasicMaterial({map:psuTex}));
psuLabel.position.set(.2,0,.482);psu.add(psuLabel);
caseG.add(psu);

// کابل‌ها
const cables=new THREE.Group();
function cable(pts,color,r){
  const c=new THREE.CatmullRomCurve3(pts.map(p=>new THREE.Vector3(p[0],p[1],p[2])));
  return new THREE.Mesh(new THREE.TubeGeometry(c,24,r,8,false),mat(color,{roughness:.8,metalness:.1}));
}
cables.add(cable([[.5,-.7,-.2],[.62,-.45,-.36],[.62,0,-.36],[.56,.5,-.37]],0x262b33,.035));
cables.add(cable([[-.2,-.7,-.2],[-.45,-.45,-.3],[-.62,-.2,-.2],[-.6,-.06,-.14]],0x3a2f2f,.025));
cables.add(cable([[-.7,-.7,-.3],[-.82,-.1,-.4],[-.82,.7,-.4],[-.45,.8,-.4]],0x262b33,.022));
caseG.add(cables);

// فن‌ها
const frontFans=new THREE.Group();frontFans.position.set(.88,0,0);
const ff1=makeFan(.26,.0),ff2=makeFan(.26,.08);
ff1.rotation.y=ff2.rotation.y=Math.PI/2;ff1.position.y=.6;ff2.position.y=-.05;
frontFans.add(ff1,ff2);caseG.add(frontFans);
const rearFan=makeFan(.17,.5);rearFan.rotation.y=Math.PI/2;rearFan.position.set(-.9,.6,-.1);caseG.add(rearFan);

// درب شیشه‌ای کناری
const glass=new THREE.Group();glass.position.set(0,0,.52);
glass.add(new THREE.Mesh(new THREE.BoxGeometry(2.0,2.2,.03),
  new THREE.MeshPhysicalMaterial({color:0x9fd0ff,transparent:true,opacity:.16,roughness:.05,metalness:0,depthWrite:false})));
const edges=new THREE.LineSegments(new THREE.EdgesGeometry(new THREE.BoxGeometry(2.0,2.2,.03)),new THREE.LineBasicMaterial({color:0xcfe6ff}));
glass.add(edges);
caseG.add(glass);

// نور داخل کیس
const glow=new THREE.PointLight(0x5aa0ff,1.5,4.5);glow.position.set(.1,.2,.15);caseG.add(glow);

reg('sidepanel',glass,{g:'کیس',n:'درب کناری (شیشه‌ای)',
  d:'برای دیدن و دسترسی به قطعات داخلی باز می‌شود. با برداشتن آن می‌توان قطعه اضافه یا تعویض کرد و داخل کیس را از گرد و غبار پاک کرد. شیشه یا اکریلیک آن اجازه می‌دهد نورپردازی RGB دیده شود.',
  t:'دکمه‌ی «باز کردن کیس» را بزنید تا درب بالا برود.'},V(0,2.7,.9));
reg('chassis',chassis,{g:'کیس',n:'بدنه‌ی کیس',
  d:'اسکلت فلزی که همه‌ی قطعات را نگه می‌دارد، آن‌ها را از ضربه و گرد و غبار محافظت می‌کند و مسیر عبور هوا را مشخص می‌کند. پایه‌های لاستیکی زیر کیس لرزش را کم می‌کنند.',t:''});
reg('frontpanel',front,{g:'کیس',n:'پنل جلوی کیس',
  d:'محل ورود هوای خنک است و معمولاً دکمه‌ی پاور، درگاه‌های USB و جک هدفون روی آن قرار دارد. شبکه‌ی آن هوا را عبور می‌دهد و جلوی ورود گرد و غبار درشت را می‌گیرد.',t:''},V(1.6,0,0));
reg('casebtn',cBtn,{g:'کیس',n:'دکمه‌ی پاور',
  d:'با فشردن آن، سیگنال روشن شدن به مادربرد می‌رسد و منبع تغذیه برق قطعات را برقرار می‌کند. نگه داشتن طولانی آن کامپیوتر را به‌اجبار خاموش می‌کند.',t:''});
parts.casebtn.obj.userData.explode=V(0,0,0);
reg('frontfans',frontFans,{g:'کیس',n:'فن‌های جلو',
  d:'هوای خنک بیرون را به داخل کیس می‌کشند تا قطعات داغ مثل کارت گرافیک و پردازنده خنک بمانند. نورهای رنگی روی آن‌ها فقط جنبه‌ی ظاهری دارند.',t:''},V(.9,0,0));
reg('rearfan',rearFan,{g:'کیس',n:'فن عقب',
  d:'هوای گرم داخل کیس را به بیرون می‌فرستد. ورود هوا از جلو و خروج آن از عقب یک جریان هوای ساده و مؤثر می‌سازد.',t:''},V(-.9,0,0));
reg('mobo',mobo,{g:'کیس',n:'مادربرد (Motherboard)',
  d:'برد اصلی که همه‌ی قطعات را به هم وصل می‌کند. پردازنده، رم، کارت گرافیک و حافظه روی آن نصب می‌شوند و ارتباطشان از طریق مسیرهای مسی روی برد انجام می‌شود. چیپست و مدارهای تغذیه‌ی پردازنده (VRM) هم روی آن هستند.',t:''},V(0,0,.3));
reg('cpu',cpu,{g:'کیس',n:'پردازنده (CPU) و خنک‌کننده',
  d:'مغز کامپیوتر است و دستورهای برنامه‌ها را اجرا می‌کند. چون هنگام کار گرم می‌شود، روی آن هیت‌سینک فلزی و فن نصب می‌کنند تا گرما را دور کند.',
  t:'بین پردازنده و هیت‌سینک لایه‌ی نازکی خمیر سیلیکون می‌مالند تا گرما بهتر منتقل شود.'},V(-.35,.55,1.5));
reg('ram',ram,{g:'کیس',n:'حافظه‌ی رم (RAM)',
  d:'حافظه‌ی موقت و سریع که برنامه‌ها و داده‌های در حال استفاده در آن نگه داشته می‌شوند. با خاموش شدن کامپیوتر محتوای آن پاک می‌شود. رم بیشتر یعنی اجرای راحت‌تر چند برنامه به‌طور هم‌زمان.',t:''},V(.5,.35,1.0));
reg('gpu',gpu,{g:'کیس',n:'کارت گرافیک (GPU)',
  d:'تصویر، بازی و ویدئو را پردازش می‌کند و خروجی را به مانیتور می‌فرستد. پردازنده‌ی گرافیکی، حافظه‌ی مخصوص (VRAM)، خنک‌کننده و فن‌های خودش را دارد و یکی از پرمصرف‌ترین قطعات کیس است.',
  t:'کابل مانیتور را به پورت‌های همین کارت وصل کنید، نه به پشت مادربرد.'},V(-.3,-.25,1.2));
reg('ssd',ssd,{g:'کیس',n:'حافظه‌ی SSD (M.2 NVMe)',
  d:'محل ذخیره‌ی دائمی ویندوز، برنامه‌ها و فایل‌هاست. برخلاف رم، با خاموش شدن کامپیوتر اطلاعاتش می‌ماند. SSD از نوع M.2 مستقیم روی مادربرد نصب می‌شود و بسیار سریع‌تر از هارد دیسک معمولی است.',t:''},V(.25,.6,.9));
reg('psu',psu,{g:'کیس',n:'منبع تغذیه (PSU)',
  d:'برق شهری (AC) را به ولتاژهای پایدار (DC) تبدیل می‌کند و به همه‌ی قطعات می‌رساند. توان آن (مثلاً ۶۵۰ یا ۸۵۰ وات) باید برای کارت گرافیک و پردازنده کافی باشد. برچسب 80+ یعنی بازده تبدیل برق بالاست.',t:''},V(0,-.2,1.3));
reg('cables',cables,{g:'کیس',n:'کابل‌ها',
  d:'برق را از منبع تغذیه به مادربرد (کابل ۲۴ پین)، پردازنده و کارت گرافیک می‌رسانند. مرتب بودن کابل‌ها جریان هوا در کیس را بهتر می‌کند.',t:''},V(.4,-.2,.6));

/* ================= توضیحات کامل قطعات ================= */
const INFO={
screen:{
 l:'صفحه نمایش بخشی است که خروجی کامپیوتر را به تصویر قابل دیدن تبدیل می‌کند. این صفحه شبکه‌ای از میلیون‌ها نقطه‌ی ریز به نام پیکسل است و هر پیکسل از سه زیرپیکسل قرمز، سبز و آبی ساخته شده. هر چه پیکسل‌ها بیشتر و ریزتر باشند، تصویر تیزتر دیده می‌شود.',
 h:['کارت گرافیک تصویر را به‌صورت سیگنال دیجیتال از طریق کابل HDMI یا DisplayPort به مانیتور می‌فرستد.','برد داخلی مانیتور سیگنال را می‌خواند و تعیین می‌کند هر زیرپیکسل چقدر روشن باشد.','در پنل‌های LCD، لایه‌ی کریستال مایع مثل دریچه عمل می‌کند و تعیین می‌کند چه مقدار از نور LED پشت صفحه از هر زیرپیکسل رد شود.','ترکیب شدت سه رنگ در هر پیکسل، میلیون‌ها رنگ مختلف می‌سازد.'],
 s:['رزولوشن رایج: Full HD (۱۹۲۰×۱۰۸۰)، QHD (۲۵۶۰×۱۴۴۰) و 4K (۳۸۴۰×۲۱۶۰)','نرخ نوسازی: از ۶۰ تا ۲۴۰ هرتز','انواع پنل: IPS (رنگ دقیق)، VA (کنتراست بالا)، TN (سرعت بالا)','زمان پاسخ: حدود ۱ تا ۵ میلی‌ثانیه'],
 f:'یک مانیتور 4K حدود ۸.۳ میلیون پیکسل دارد؛ یعنی چهار برابر Full HD.',
 t:'بعد از اتصال مانیتور پرسرعت، در تنظیمات ویندوز (Advanced display) نرخ نوسازی را روی بیشترین مقدار بگذارید؛ گاهی روی ۶۰ هرتز می‌ماند.'},
bezel:{
 l:'قاب، بدنه‌ی مانیتور است که پنل را در بر می‌گیرد و قطعات الکترونیکی پشت آن را پنهان می‌کند. هر چه قاب باریک‌تر باشد مانیتور مدرن‌تر دیده می‌شود و در چیدمان چندمانیتوری فاصله‌ی کمتری بین تصاویر می‌افتد.',
 h:['برد اصلی مانیتور (Scaler) ورودی‌های تصویر را دریافت و به قالب مناسب پنل تبدیل می‌کند.','منبع تغذیه‌ی داخلی یا آداپتور بیرونی، برق لازم پنل و نور پس‌زمینه را تأمین می‌کند.','منوی تنظیمات (OSD) که روشنایی، کنتراست و ورودی را عوض می‌کند، روی همین برد اجرا می‌شود.'],
 s:['پورت‌های رایج: HDMI، DisplayPort، USB-C و جک هدفون','استاندارد نصب روی بازو یا دیوار: VESA (مثلاً ۱۰۰×۱۰۰ میلی‌متر)','نسبت تصویر رایج: ۱۶:۹ و در مدل‌های اولترواید ۲۱:۹'],
 f:'در مانیتورهای مدرن، قاب گاهی فقط چند میلی‌متر است، اما قطعات الکترونیکی هنوز پشت پنل جا می‌شوند.',
 t:'برای کم شدن خستگی چشم، مانیتور را حدود ۵۰ تا ۷۰ سانتی‌متر از چشم دور نگه دارید.'},
monbtn:{
 l:'دکمه‌ی فیزیکی روشن و خاموش مانیتور که در بسیاری از مدل‌ها برای ورود به منوی تنظیمات هم استفاده می‌شود. چراغ کوچک کنار آن وضعیت مانیتور را نشان می‌دهد.',
 h:['با فشردن آن، برد مانیتور نور پس‌زمینه را روشن می‌کند.','وقتی کامپیوتر خاموش است یا سیگنالی نمی‌آید، مانیتور وارد حالت آماده‌باش (Standby) می‌شود.','در حالت آماده‌باش فقط بخش کوچکی از مدار فعال می‌ماند و برق بسیار کمی مصرف می‌شود.'],
 s:['مصرف آماده‌باش: معمولاً کمتر از ۱ وات','مصرف حین کار: بسته به اندازه و روشنایی، از حدود ۱۵ تا بیش از ۵۰ وات'],
 f:'پیام «No Signal» معمولاً یعنی مشکل از کابل یا ورودی انتخاب‌شده است، نه خود مانیتور.',
 t:'اگر تصویر نیست، اول مطمئن شوید ورودی مانیتور (Input) روی HDMI یا DisplayPort درست انتخاب شده باشد.'},
neck:{
 l:'ستونی که پنل را بالای میز نگه می‌دارد. در مدل‌های بهتر ارتفاع، زاویه‌ی بالا و پایین، چرخش و حتی حالت عمودی (Pivot) قابل تنظیم است.',
 h:['وزن پنل را به پایه منتقل می‌کند.','لولای داخلی زاویه‌ی صفحه را بعد از تنظیم ثابت نگه می‌دارد.','بعضی مدل‌ها فنر یا گازلیفت دارند تا ارتفاع را بتوان با یک دست تنظیم کرد.'],
 s:['تنظیم ارتفاع: در مدل‌های دارای این قابلیت حدود ۱۰ تا ۱۵ سانتی‌متر','Tilt: چند درجه به جلو و عقب','Pivot: چرخش ۹۰ درجه برای کار با صفحه‌های بلند'],
 f:'یک مانیتور ۲۴ اینچ معمولاً حدود ۳ تا ۵ کیلوگرم وزن دارد.',
 t:'اگر پایه‌ی مانیتور کوتاه است، بازوی مانیتور هم ارتفاع را درست می‌کند و هم جای میز را آزاد می‌کند.'},
base:{
 l:'پایه، مرکز ثقل مانیتور را پایین می‌آورد و جلوی افتادن آن را می‌گیرد. شکل آن مستطیلی، V شکل یا گرد است و زیرش معمولاً لاستیک دارد تا روی میز نلغزد.',
 h:['سطح تماس بزرگ‌تر یعنی تعادل بیشتر.','لاستیک‌های زیر پایه لغزش و لرزش را کم می‌کنند.','در بیشتر مدل‌ها با باز کردن چند پیچ، پایه جدا می‌شود و مانیتور روی بازوی VESA قرار می‌گیرد.'],
 s:['مواد: پلاستیک، آلومینیوم یا فولاد','اندازه: متناسب با وزن و عرض مانیتور'],
 f:'پایه معمولاً سنگین‌ترین بخش مانیتور نسبت به اندازه‌اش است، چون کارش تعادل است.',
 t:'پایه‌ی کوچک‌تر فضای بیشتری روی میز می‌گذارد تا کیبورد را هر وقت لازم شد جلو یا عقب ببرید.'},
kbd:{
 l:'کیبورد مهم‌ترین ابزار ورودی برای نوشتن، برنامه‌نویسی و بازی است. هر کلید در واقع یک کلید فشاری الکتریکی است که با فشار دادن، یک کد مخصوص به کامپیوتر می‌فرستد.',
 h:['زیر هر کلید یک سوییچ (مکانیکی، غشایی یا اپتیکال) قرار دارد.','با فشردن کلید، مدار بسته می‌شود و کنترلر کیبورد کد آن کلید را می‌سازد.','کد از طریق USB، بلوتوث یا گیرنده‌ی بی‌سیم به کامپیوتر می‌رسد.','سیستم‌عامل کد را به حرف، عدد یا فرمان تبدیل می‌کند.'],
 s:['انواع سوییچ: مکانیکی، غشایی، اپتیکال','اندازه‌ها: فول‌سایز، TKL و ۶۰ درصد','N-Key Rollover: تشخیص هم‌زمان چند کلید فشرده‌شده'],
 f:'چیدمان QWERTY در قرن نوزدهم برای ماشین‌تحریر طراحی شد و هنوز استاندارد رایج است.',
 t:'کیبورد را هم‌سطح آرنج بگذارید و مچ دست را روی لبه‌ی میز خم نکنید.'},
mouse:{
 l:'ماوس حرکت دست شما را به حرکت نشانگر روی صفحه تبدیل می‌کند. چرخ و دکمه‌هایش برای پیمایش و انتخاب هستند.',
 h:['یک LED یا لیزر کوچک سطح زیر ماوس را روشن می‌کند.','حسگر نوری هزاران بار در ثانیه از سطح عکس می‌گیرد.','پردازنده‌ی ماوس جابه‌جایی بین عکس‌ها را حساب و به کامپیوتر ارسال می‌کند.'],
 s:['DPI: از ۸۰۰ تا چند ده‌هزار','نرخ گزارش: تا ۱۰۰۰ هرتز یا بیشتر در ماوس‌های گیمینگ','اتصال: سیمی، بلوتوث یا گیرنده‌ی ۲.۴ گیگاهرتز'],
 f:'اولین ماوس در دهه‌ی ۱۹۶۰ توسط داگلاس انگلبارت ساخته شد و بدنه‌ی آن چوبی بود.',
 t:'DPI بالاتر همیشه بهتر نیست؛ DPI متوسط با حساسیت مناسب معمولاً دقیق‌تر است.'},
sidepanel:{
 l:'درب کناری برای دیدن و دسترسی به قطعات داخلی برداشته می‌شود. با باز کردن آن می‌توان قطعه اضافه کرد، تعویض کرد و داخل کیس را از گرد و غبار پاک کرد. شیشه یا اکریلیک آن اجازه می‌دهد نورپردازی RGB داخل کیس دیده شود.',
 h:['درب با پیچ، لولا یا گیره روی بدنه بسته می‌شود.','شیشه یا اکریلیک قطعات را نشان می‌دهد و جلوی ورود گرد و غبار را می‌گیرد.','با برداشتن آن، به همه‌ی قطعات داخل کیس دسترسی دارید.'],
 s:['جنس: شیشه‌ی تمپرد یا اکریلیک','پیچ انگشتی (Thumbscrew): باز کردن بدون ابزار','شیشه سنگین‌تر و مقاوم‌تر در برابر خط است'],
 f:'شیشه‌ی تمپرد خراش را خوب تحمل می‌کند، اما لبه‌هایش در برابر ضربه حساس است.',
 t:'قبل از باز کردن درب، کامپیوتر را خاموش کنید و کابل برق را بکشید.'},
chassis:{
 l:'بدنه، اسکلت فلزی کیس است. همه‌ی قطعات را نگه می‌دارد، از ضربه و گرد و غبار محافظت می‌کند و مسیر عبور هوا را مشخص می‌کند. پایه‌های لاستیکی زیر آن لرزش را کم می‌کنند.',
 h:['ورق‌های فولادی استحکام و ثبات می‌دهند.','محل فن‌ها و توری‌ها مسیر جریان هوا را می‌سازد.','بدنه‌ی فلزی به زمین منبع تغذیه وصل است و ایمنی را بالا می‌برد.'],
 s:['اندازه‌ها: Mini-ITX، Micro-ATX، ATX و E-ATX','Mid Tower: رایج‌ترین اندازه‌ی کیس','ضخامت فولاد: معمولاً حدود ۰.۶ تا ۱ میلی‌متر'],
 f:'استاندارد ATX در سال ۱۹۹۵ معرفی شد و اندازه‌ی مادربرد و محل پیچ‌ها را یکسان کرد.',
 t:'قبل از خرید کیس، طول کارت گرافیک و ارتفاع خنک‌کننده‌ی پردازنده را با مشخصات کیس مقایسه کنید.'},
frontpanel:{
 l:'پنل جلو محل ورود هوای خنک است. معمولاً دکمه‌ی پاور، درگاه‌های USB و جک هدفون روی آن قرار دارد. شبکه‌ی جلو هوا را رد می‌کند و ذرات درشت گرد و غبار را می‌گیرد.',
 h:['فن‌های پشت پنل هوای بیرون را از شبکه‌ی جلو می‌کشند.','فیلتر گرد و غبار ذرات درشت را قبل از رسیدن به قطعات می‌گیرد.','درگاه‌های جلو با کابل به مادربرد وصل‌اند.'],
 s:['درگاه‌های رایج: USB 3.0، USB-C و جک ۳.۵ میلی‌متری','جلوی مشبک معمولاً کمترین مقاومت را در برابر هوا دارد'],
 f:'کیس‌های با جلوی شیشه‌ای زیبا هستند، اما می‌توانند جریان هوا را محدود کنند.',
 t:'فیلتر گرد و غبار را هر چند ماه بشویید؛ فیلتر کثیف دمای قطعات را بالا می‌برد.'},
casebtn:{
 l:'دکمه‌ی پاور کامپیوتر را روشن می‌کند. فشار کوتاه آن کامپیوتر را روشن یا خاموش می‌کند و فشار طولانی، خاموشی اجباری است.',
 h:['دکمه با کابلی به پین‌های PWR_SW روی مادربرد وصل است.','فشردن آن مدار را لحظه‌ای وصل می‌کند و مادربرد سیگنال روشن شدن را به منبع تغذیه می‌دهد.','منبع تغذیه ولتاژها را فعال می‌کند و کامپیوتر بوت می‌شود.'],
 s:['فشار طولانی: حدود ۴ تا ۵ ثانیه برای خاموشی اجباری','چراغ دور دکمه معمولاً وضعیت روشن بودن را نشان می‌دهد'],
 f:'بعضی مادربردها یک دکمه‌ی روشن‌کننده‌ی کوچک روی خود برد هم دارند، برای وقتی که کیس هنوز وصل نیست.',
 t:'اگر با دکمه روشن نمی‌شود، اتصال کابل پاور کیس به پین‌های مادربرد را بررسی کنید.'},
frontfans:{
 l:'فن‌های جلو هوای خنک بیرون را به داخل کیس می‌کشند تا قطعات داغ مثل کارت گرافیک و پردازنده خنک بمانند. نورهای رنگی روی آن‌ها بیشتر جنبه‌ی ظاهری دارد.',
 h:['موتور بدون جاروبک پره‌ها را می‌چرخاند.','پره‌های منحنی هوا را به داخل می‌رانند.','وقتی هوای ورودی بیشتر از خروجی باشد، فشار مثبت داخل کیس جلوی ورود گرد و غبار از شکاف‌ها را می‌گیرد.'],
 s:['اندازه‌های رایج: ۱۲۰ و ۱۴۰ میلی‌متر','سرعت: حدود ۵۰۰ تا ۲۰۰۰ دور در دقیقه','کانکتور: ۳ پین (ولتاژی) یا ۴ پین (PWM)'],
 f:'یک فن بزرگ‌تر با دور کمتر می‌تواند همان‌قدر هوا بدهد و ساکت‌تر باشد.',
 t:'در BIOS یا نرم‌افزار مادربرد منحنی سرعت فن را تنظیم کنید تا صدا کم شود.'},
rearfan:{
 l:'فن عقب هوای گرم داخل کیس را بیرون می‌دهد. هوای ورودی از جلو و خروجی از عقب یک جریان هوای ساده و مؤثر می‌سازد.',
 h:['هوای گرم سبک‌تر است و در بالای کیس جمع می‌شود.','فن عقب آن را از پشت کیس بیرون می‌فرستد.','اگر هوای خروجی بیشتر از ورودی باشد، هوا از شکاف‌های ریز وارد می‌شود و گرد و غبار بیشتری جمع می‌شود.'],
 s:['اندازه‌ی رایج: ۹۲ یا ۱۲۰ میلی‌متر','محل: معمولاً بالای پورت‌های پشت مادربرد'],
 f:'یک فن عقب و دو فن جلو برای بیشتر سیستم‌های معمولی تعادل خوبی بین دما و صدا می‌سازد.',
 t:'جهت وزش فن را از فلش روی لبه‌ی آن بررسی کنید تا برعکس نصب نشود.'},
mobo:{
 l:'مادربرد برد اصلی است که همه‌ی قطعات را به هم وصل می‌کند. پردازنده، رم، کارت گرافیک و حافظه روی آن نصب می‌شوند و ارتباطشان از طریق مسیرهای مسی داخل برد انجام می‌شود. چیپست و مدارهای تغذیه‌ی پردازنده (VRM) هم روی آن قرار دارند.',
 h:['برد چندلایه است و مسیرهای مسی بین قطعات می‌سازد.','VRM ولتاژ ۱۲ ولت را به ولتاژ پایین و دقیق پردازنده تبدیل می‌کند.','چیپست ارتباط پردازنده با حافظه و درگاه‌ها را مدیریت می‌کند.','UEFI/BIOS اولین برنامه‌ای است که هنگام روشن شدن اجرا می‌شود.'],
 s:['اندازه‌ها: ATX، Micro-ATX و Mini-ITX','سوکت پردازنده بسته به برند فرق دارد (مثلاً AM5 یا LGA)','اسلات‌ها: PCIe برای گرافیک، DIMM برای رم و M.2 برای SSD'],
 f:'روی مادربرد یک باتری سکه‌ای کوچک ساعت و تنظیمات BIOS را موقع خاموشی نگه می‌دارد.',
 t:'قبل از خرید، سازگاری سوکت پردازنده و نوع رم (DDR4 یا DDR5) را با مادربرد چک کنید.'},
cpu:{
 l:'پردازنده مغز کامپیوتر است و دستورهای برنامه‌ها را اجرا می‌کند. چون هنگام کار گرم می‌شود، روی آن هیت‌سینک فلزی و فن نصب می‌کنند تا گرما را دور کند.',
 h:['دستورها و داده‌ها را از رم می‌خواند.','هر هسته در هر ثانیه میلیاردها چرخه‌ی کاری انجام می‌دهد.','حافظه‌ی کش (L1، L2، L3) داده‌های پرکاربرد را نزدیک هسته نگه می‌دارد.','گرما از طریق هیت‌پایپ‌ها به فین‌ها می‌رسد و فن آن را بیرون می‌دهد.'],
 s:['تعداد هسته: از ۴ تا ۲۴ یا بیشتر','فرکانس: حدود ۳ تا ۵.۸ گیگاهرتز','مصرف: حدود ۶۵ تا ۲۵۰ وات بسته به مدل'],
 f:'یک پردازنده‌ی امروزی میلیاردها ترانزیستور دارد، روی سطحی به اندازه‌ی ناخن.',
 t:'بین پردازنده و هیت‌سینک خمیر سیلیکون می‌مالند تا گرما بهتر منتقل شود. دمای بار بهتر است زیر حدود ۸۰ درجه بماند.'},
ram:{
 l:'رم حافظه‌ی موقت و سریع کامپیوتر است. برنامه‌ها و داده‌هایی که در حال استفاده‌اند در آن نگه داشته می‌شوند. با خاموش شدن کامپیوتر محتوای آن پاک می‌شود. رم بیشتر یعنی اجرای راحت‌تر چند برنامه به‌طور هم‌زمان.',
 h:['داده‌ی در حال استفاده از SSD به رم کپی می‌شود تا پردازنده سریع‌تر به آن برسد.','سلول‌های DRAM باید مدام تازه‌سازی شوند تا داده‌ها حفظ شوند.','با قطع برق، همه‌ی اطلاعات رم از بین می‌رود.'],
 s:['نوع رایج: DDR4 و DDR5','ظرفیت کل برای کاربر عادی و گیمر: حدود ۱۶ تا ۶۴ گیگابایت','Dual Channel: دو ماژول هم‌زمان کار می‌کنند و پهنای باند بیشتر می‌شود'],
 f:'DDR4 و DDR5 شکاف پایین متفاوتی دارند و در اسلات یکدیگر جا نمی‌شوند.',
 t:'دو ماژول را در اسلات‌های پیشنهادی مادربرد بگذارید و در BIOS گزینه‌ی XMP یا EXPO را روشن کنید تا رم به سرعت تبلیغ‌شده برسد.'},
gpu:{
 l:'کارت گرافیک تصویر، بازی و ویدئو را پردازش می‌کند و خروجی را به مانیتور می‌فرستد. پردازنده‌ی گرافیکی، حافظه‌ی مخصوص (VRAM)، خنک‌کننده و فن‌های خودش را دارد و یکی از پرمصرف‌ترین قطعات کیس است.',
 h:['پردازنده دستورهای رسم تصویر را به کارت گرافیک می‌دهد.','هزاران هسته‌ی کوچک GPU محاسبه‌ی میلیون‌ها پیکسل را هم‌زمان انجام می‌دهند.','نتیجه در حافظه‌ی VRAM ذخیره و به مانیتور فرستاده می‌شود.','هیت‌پایپ‌ها و فن‌ها گرمای زیاد GPU را دفع می‌کنند.'],
 s:['VRAM: از ۸ تا ۲۴ گیگابایت و بیشتر','مصرف: از حدود ۱۰۰ تا ۴۵۰ وات','رابط: PCIe ×16','خروجی‌ها: HDMI و DisplayPort'],
 f:'GPUها فقط برای بازی نیستند؛ در رندر سه‌بعدی، ویرایش ویدئو و آموزش هوش مصنوعی هم استفاده می‌شوند.',
 t:'کابل برق PCIe را کامل جا بزنید؛ اتصال شل می‌تواند باعث ریست شدن یا آسیب شود. کابل مانیتور را هم به پورت‌های خود کارت گرافیک وصل کنید.'},
ssd:{
 l:'SSD محل ذخیره‌ی دائمی ویندوز، برنامه‌ها و فایل‌هاست. برخلاف رم، با خاموش شدن کامپیوتر اطلاعاتش می‌ماند. SSD از نوع M.2 مستقیم روی مادربرد نصب می‌شود و بسیار سریع‌تر از هارد دیسک معمولی است.',
 h:['داده در سلول‌های حافظه‌ی فلش NAND ذخیره می‌شود.','کنترلر داخلی خواندن، نوشتن و تصحیح خطا را مدیریت می‌کند.','پروتکل NVMe با خطوط PCIe مستقیم با پردازنده صحبت می‌کند و تأخیر کمی دارد.'],
 s:['سرعت خواندن M.2 NVMe: حدود ۳۵۰۰ تا بیش از ۱۰۰۰۰ مگابایت بر ثانیه','ظرفیت رایج: ۵۰۰ گیگابایت تا ۴ ترابایت','عمر: با شاخص TBW (حجم کل نوشتن) سنجیده می‌شود'],
 f:'SSD قطعه‌ی متحرک ندارد؛ بی‌صدا است و در برابر ضربه از هارد دیسک مقاوم‌تر است.',
 t:'حدود ۱۰ تا ۲۰ درصد از فضای SSD را خالی بگذارید تا سرعت و عمرش بهتر بماند.'},
psu:{
 l:'منبع تغذیه برق شهری (AC) را به ولتاژهای پایدار (DC) تبدیل می‌کند و به همه‌ی قطعات می‌رساند. توان آن باید برای کارت گرافیک و پردازنده کافی باشد. برچسب 80 PLUS یعنی بازده تبدیل برق بالاست.',
 h:['برق شهری را یکسو می‌کند.','مبدل سوییچینگ آن را به ولتاژهای ۱۲، ۵ و ۳.۳ ولت DC تبدیل می‌کند.','فن داخلی بسته به دما سرعتش را تنظیم می‌کند.','مدارهای حفاظتی در برابر اضافه‌ولتاژ و اضافه‌جریان از قطعات محافظت می‌کنند.'],
 s:['توان: حدود ۴۵۰ تا ۱۰۰۰ وات یا بیشتر','گواهی 80 PLUS: از Bronze تا Titanium','حفاظت‌ها: OVP، OCP و SCP'],
 f:'منبع تغذیه‌ی بی‌کیفیت می‌تواند به قطعات دیگر هم آسیب بزند؛ پایه‌ی پایداری سیستم همین قطعه است.',
 t:'توان منبع را ۲۰ تا ۳۰ درصد بیشتر از مجموع مصرف قطعات انتخاب کنید.'},
cables:{
 l:'کابل‌ها برق را از منبع تغذیه به مادربرد (کابل ۲۴ پین)، پردازنده و کارت گرافیک می‌رسانند. مرتب بودن کابل‌ها جریان هوا در کیس را بهتر می‌کند.',
 h:['رنگ سیم‌ها استاندارد است: زرد ۱۲ ولت، قرمز ۵ ولت، نارنجی ۳.۳ ولت و مشکی زمین.','شکل کانکتورها طوری است که فقط یک طرف جا می‌رود تا اشتباه وصل نشود.','منبع‌های ماژولار فقط کابل‌های لازم را به کیس وصل می‌کنند.'],
 s:['کابل ۲۴ پین ATX: تغذیه‌ی مادربرد','کابل EPS: تغذیه‌ی پردازنده','کابل‌های PCIe: تغذیه‌ی کارت گرافیک','کابل SATA: تغذیه‌ی دیسک‌ها'],
 f:'یک کیس با کابل‌کشی مرتب فقط زیبا نیست؛ هوا هم راحت‌تر در آن جریان پیدا می‌کند.',
 t:'کابل‌ها را با بست مرتب کنید و جلوی فن‌ها را نگیرید.'}
};
for(const id in INFO){ if(parts[id]) Object.assign(parts[id],INFO[id]); }

/* ================= رابط کاربری ================= */
const order=['screen','bezel','monbtn','neck','base','kbd','mouse','sidepanel','chassis','frontpanel','casebtn','frontfans','rearfan','mobo','cpu','ram','gpu','ssd','psu','cables'];
const chipsEl=$('#chips');
let lastG='';
order.forEach(id=>{
  const p=parts[id];
  if(p.g!==lastG){const s=document.createElement('span');s.className='gl';s.textContent=p.g+':';chipsEl.appendChild(s);lastG=p.g;}
  const b=document.createElement('button');
  b.className='chip';b.type='button';b.dataset.id=id;b.setAttribute('aria-pressed','false');b.textContent=p.n.replace(/\s*\(.*\)/,'');
  b.addEventListener('click',()=>select(id,true));
  chipsEl.appendChild(b);
});

let selId=null,selBox=null,follow=false;
const tmpBox=new THREE.Box3();
const DEF={tag:'راهنما',t:'اجزای کامپیوتر',d:$('#cDesc').textContent};
function fillList(sec,list,arr){
  const l=$(list);l.innerHTML='';
  (arr||[]).forEach(x=>{const li=document.createElement('li');li.textContent=x;l.appendChild(li);});
  $(sec).hidden=!(arr&&arr.length);
}
function fillText(sec,el,txt){$(sec).hidden=!txt;$(el).textContent=txt||'';}
function showMore(p){
  fillList('#sHow','#cHow',p&&p.h);
  fillList('#sSpec','#cSpec',p&&p.s);
  fillText('#sFact','#cFact',p&&p.f);
  fillText('#sTip','#cTip',p&&p.t);
  $('#card').scrollTop=0;
}
$('#cToggle').addEventListener('click',()=>{
  const min=$('#card').classList.toggle('min');
  $('#cToggle').textContent=min?'نمایش جزئیات':'کوچک کردن';
  $('#cToggle').setAttribute('aria-expanded',!min);
});
function select(id,focus){
  selId=id;
  document.querySelectorAll('.chip').forEach(c=>{
    const on=c.dataset.id===id;c.setAttribute('aria-pressed',on);
    if(on)c.scrollIntoView({inline:'center',block:'nearest',behavior:reduceMotion?'auto':'smooth'});
  });
  if(!id){
    if(selBox){scene.remove(selBox);selBox=null;}
    $('#cTag').textContent=DEF.tag;$('#cTitle').textContent=DEF.t;$('#cDesc').textContent=DEF.d;showMore(null);
    follow=false;return;
  }
  const p=parts[id];
  $('#cTag').textContent=p.g;$('#cTitle').textContent=p.n;$('#cDesc').textContent=p.l||p.d;showMore(p);
  if(!selBox){selBox=new THREE.BoxHelper(p.obj,0x4da3ff);scene.add(selBox);}
  if(focus){
    tmpBox.setFromObject(p.obj);
    zoomGoal=clamp(tmpBox.getSize(new THREE.Vector3()).length()*1.7/baseR(),.22,1);
    follow=true;
  }
}

/* دکمه‌ها */
let openGoal=0,exGoal=0,auto=false;
const bOpen=$('#bOpen'),bExp=$('#bExp'),bAuto=$('#bAuto');
bOpen.addEventListener('click',()=>{openGoal=openGoal?0:1;bOpen.setAttribute('aria-pressed',!!openGoal);bOpen.textContent=openGoal?'بستن کیس':'باز کردن کیس';});
bExp.addEventListener('click',()=>{exGoal=exGoal?0:1;bExp.setAttribute('aria-pressed',!!exGoal);});
bAuto.addEventListener('click',()=>{auto=!auto;bAuto.setAttribute('aria-pressed',auto);});
$('#bReset').addEventListener('click',()=>{
  thetaGoal=0;phiGoal=1.32;zoomGoal=1;targetGoal.set(.1,1.4,.4);follow=false;select(null);
});

/* ---------- دوربین ---------- */
let theta=0,phi=1.32,zoom=1,thetaGoal=0,phiGoal=1.32,zoomGoal=1;
const target=new THREE.Vector3(.1,1.4,.4),targetGoal=target.clone();
const baseR=()=>Math.max(10.5,4.6/(Math.tan(camera.fov*Math.PI/360)*camera.aspect));
function resize(){
  const w=stage.clientWidth||innerWidth,h=stage.clientHeight||innerHeight;
  renderer.setSize(w,h,false);camera.aspect=w/h;camera.updateProjectionMatrix();
}
addEventListener('resize',resize);resize();

/* ---------- ورودی (لمس و ماوس) ---------- */
const ptrs=new Map();let moved=0,pinch0=0,zoom0=1,dragging=false;
cv.addEventListener('pointerdown',e=>{
  cv.setPointerCapture(e.pointerId);ptrs.set(e.pointerId,{x:e.clientX,y:e.clientY});
  moved=0;dragging=true;
  if(ptrs.size===2){const [a,b]=[...ptrs.values()];pinch0=Math.hypot(a.x-b.x,a.y-b.y);zoom0=zoomGoal;}
});
cv.addEventListener('pointermove',e=>{
  const p=ptrs.get(e.pointerId);if(!p)return;
  const dx=e.clientX-p.x,dy=e.clientY-p.y;p.x=e.clientX;p.y=e.clientY;
  if(ptrs.size===1){
    moved+=Math.abs(dx)+Math.abs(dy);
    if(moved>6){follow=false;}
    thetaGoal-=dx*.006;phiGoal=clamp(phiGoal-dy*.005,.25,1.65);
  }else if(ptrs.size===2){
    const [a,b]=[...ptrs.values()];const d=Math.hypot(a.x-b.x,a.y-b.y);
    if(pinch0)zoomGoal=clamp(zoom0*pinch0/d,.22,1.6);moved+=10;
  }
});
function endPtr(e){
  if(!ptrs.has(e.pointerId))return;
  const tap=ptrs.size===1&&moved<6&&e.type==='pointerup';
  ptrs.delete(e.pointerId);if(!ptrs.size)dragging=false;
  if(tap)pick(e);
}
cv.addEventListener('pointerup',endPtr);
cv.addEventListener('pointercancel',endPtr);
cv.addEventListener('wheel',e=>{e.preventDefault();zoomGoal=clamp(zoomGoal*Math.exp(e.deltaY*.001),.22,1.6);},{passive:false});

const ray=new THREE.Raycaster(),m2=new THREE.Vector2();
function pick(e){
  const r=cv.getBoundingClientRect();
  m2.set(((e.clientX-r.left)/r.width)*2-1,-((e.clientY-r.top)/r.height)*2+1);
  ray.setFromCamera(m2,camera);
  const hits=ray.intersectObjects(pickables,false);
  if(!hits.length){select(null);return;}
  let id=hits[0].object.userData.partId;
  if(id==='sidepanel'&&hits[1]&&hits[1].object.userData.partId!=='chassis')id=hits[1].object.userData.partId;
  select(id,true);
}

/* ---------- انیمیشن ---------- */
let openT=0,exT=0,last=performance.now();
const spin=reduceMotion?1.2:5;
function frame(now){
  requestAnimationFrame(frame);
  const dt=Math.min(.05,(now-last)/1000);last=now;
  openT+=(openGoal-openT)*Math.min(1,dt*5);
  exT+=(exGoal-exT)*Math.min(1,dt*4);
  for(const id in parts){
    const o=parts[id].obj,k=id==='sidepanel'?Math.max(openT,exT):exT;
    o.position.copy(o.userData.base).addScaledVector(o.userData.explode,k);
  }
  spinners.forEach((r,i)=>{r.rotation.z-=dt*(spin+(i%3)*.6);});
  const hue=(now*.00012)%1;
  rgb.forEach(x=>x.m.color.setHSL((hue+x.o)%1,1,.55));
  glow.color.setHSL((hue+.2)%1,.9,.6);

  if(auto&&!dragging)thetaGoal+=dt*.25;
  if(follow&&selId){tmpBox.setFromObject(parts[selId].obj);tmpBox.getCenter(targetGoal);}
  const s=Math.min(1,dt*6);
  theta+=(thetaGoal-theta)*s;phi+=(phiGoal-phi)*s;zoom+=(zoomGoal-zoom)*s;
  target.lerp(targetGoal,s);
  const R=baseR()*zoom;
  camera.position.set(
    target.x+R*Math.sin(phi)*Math.sin(theta),
    target.y+R*Math.cos(phi),
    target.z+R*Math.sin(phi)*Math.cos(theta));
  camera.lookAt(target);
  if(selBox&&selId)selBox.setFromObject(parts[selId].obj);
  renderer.render(scene,camera);
}
requestAnimationFrame(frame);
})();
</script>
</body>
</html>
