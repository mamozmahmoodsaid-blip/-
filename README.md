from pathlib import Path
from PIL import Image, ImageOps, ImageDraw
import json, zipfile, math, shutil

base = Path("/mnt/data/mansheya_pro")
assets = base / "assets"
assets.mkdir(parents=True, exist_ok=True)
src = Path("/mnt/data/1000203055.png")
img = Image.open(src).convert("RGB")
img.save(assets / "street-splash.jpg", quality=92)
for size, name in [(192, "icon-192.png"), (512, "icon-512.png")]:
    icon = ImageOps.fit(img, (size, size), method=Image.Resampling.LANCZOS, centering=(0.5, 0.5))
    icon.save(assets / name, quality=93)

html = r'''<!doctype html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<meta name="theme-color" content="#101a28"><link rel="manifest" href="manifest.json"><link rel="icon" href="assets/icon-192.png">
<title>المنشية | إصدار المطوّر</title>
<style>
:root{--bg:#0b1220;--panel:#162337;--panel2:#20324a;--gold:#ffd166;--line:#405675;--white:#f8fbff;--muted:#d4deed;--green:#5bd19a}
*{box-sizing:border-box}html{font-size:18px}body{margin:0;background:var(--bg);color:var(--white);font-family:Tahoma,Arial,sans-serif;font-size:18px}.app{max-width:1150px;margin:auto;min-height:100vh;background:linear-gradient(145deg,#192e45,#0a1220)}
button{font:inherit;touch-action:manipulation;cursor:pointer}header{display:flex;align-items:center;justify-content:space-between;gap:12px;padding:13px 16px;background:#0c1728;border-bottom:1px solid var(--line);position:sticky;top:0;z-index:3}.brand{display:flex;align-items:center;gap:11px}.logo{font-size:30px;background:linear-gradient(135deg,#ffe18a,#ff9650);padding:10px;border-radius:14px;color:#18243a}h1{font-size:clamp(27px,5vw,38px);margin:0;line-height:1.25}.sub{font-size:14px;color:var(--muted);margin-top:4px}.topstats{display:flex;gap:7px;flex-wrap:wrap}.chip{background:#20334b;border:1px solid var(--line);padding:9px 11px;border-radius:12px;font-size:15px;white-space:nowrap}
.hero{margin:13px;border:1px solid #61718a;border-radius:18px;min-height:170px;background:linear-gradient(0deg,#08111de8,#08111d00 85%),url('assets/street-splash.jpg') center/cover;display:flex;flex-direction:column;justify-content:flex-end;padding:17px}.hero h2{font-size:clamp(28px,6vw,40px);margin:0 0 7px}.hero p{font-size:17px;line-height:1.7;margin:0;max-width:760px}
main{padding:0 13px 20px}.title{font-size:23px;margin:17px 2px 11px}.tabs{display:grid;grid-template-columns:repeat(4,1fr);gap:8px}.tab{background:linear-gradient(145deg,#273f59,#18273c);border:1px solid var(--line);color:white;border-radius:13px;padding:13px 7px;font-size:16px;font-weight:bold;min-height:62px}.tab.active{border:2px solid var(--gold);background:#443b2c;color:#fff1c5}
.content{display:grid;grid-template-columns:minmax(0,1.65fr) minmax(270px,1fr);gap:13px}.card{background:#111e30;border:1px solid var(--line);border-radius:17px;overflow:hidden}.bar{padding:13px;display:flex;justify-content:space-between;align-items:center;gap:8px;flex-wrap:wrap;border-bottom:1px solid #354960;font-size:17px}.stats{display:flex;gap:6px;flex-wrap:wrap}.stat{background:#263950;padding:7px 9px;border-radius:9px;font-size:15px}canvas{width:100%;height:auto;display:block;aspect-ratio:16/10;touch-action:none;background:#c8dfe7}
.actionrow{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;padding:10px}.btn{border:1px solid #4c6583;background:#263d58;color:white;border-radius:12px;padding:13px 7px;font-size:17px;font-weight:bold;min-height:49px}.btn.gold{background:linear-gradient(135deg,#ffe18b,#ff9d4e);color:#182438;border:0}.btn.fire{background:linear-gradient(135deg,#ff8a70,#e23e58);border:0}.btn.green{background:linear-gradient(135deg,#6de0a6,#2eab78);color:#102a25;border:0}
.joystick-row{display:grid;grid-template-columns:1fr 1fr;gap:10px;padding:0 10px 10px}.pad{background:#1c2d43;border:1px solid #405775;border-radius:13px;padding:12px}.padlabel{text-align:center;font-size:14px;color:#c7d4e5;margin-bottom:7px}.directions{display:grid;grid-template-columns:repeat(3,1fr);gap:5px}.dir{border-radius:9px;background:#304964;border:1px solid #4e6987;color:white;min-height:43px;font-size:20px}.dir:active,.btn:active,.tab:active{transform:scale(.98)}.pad .fire{width:100%;margin-top:6px}
.side{padding:14px}.side h3{font-size:20px;margin:0 0 10px}.side p{font-size:16px;line-height:1.8;color:var(--muted);margin:0}.mission{background:#1d2e44;border:1px solid #3c536f;border-radius:12px;padding:12px;margin-bottom:13px}.mission strong{color:var(--gold);font-size:18px;display:block;margin-bottom:7px}.housegrid{display:grid;grid-template-columns:1fr 1fr;gap:8px}.house{background:#22364e;padding:11px;border-radius:10px;font-size:15px;line-height:1.5}.hudhelp{margin-top:12px;padding:11px;background:#172b40;border-radius:11px;color:#d5e0ef;font-size:14px;line-height:1.7}
.footer{text-align:center;padding:15px;color:#a8b7cc;font-size:13px}
.overlay{position:fixed;inset:0;z-index:10;background:#06101bea;display:flex;align-items:center;justify-content:center;padding:18px}.welcome{width:min(600px,100%);background:#142338;border:1px solid #6a7c95;border-radius:20px;overflow:hidden;box-shadow:0 22px 80px #000b}.welcomeimg{height:220px;background:url('assets/street-splash.jpg') center/cover}.welcomebody{padding:20px}.welcomebody h2{font-size:32px;margin:0 0 9px}.welcomebody p{font-size:17px;line-height:1.8;color:#d8e1ed}.welcomebody .btn{width:100%;margin-top:8px}
@media(max-width:800px){.content{grid-template-columns:1fr}.tabs{grid-template-columns:repeat(2,1fr)}.side{display:block}}
@media(max-width:430px){html{font-size:16px}.topstats{gap:4px}.chip{font-size:12px;padding:7px}.sub{font-size:12px}.hero{min-height:150px}.hero p{font-size:15px}.title{font-size:21px}.bar{font-size:15px}.stat{font-size:13px}.btn{font-size:15px}.joystick-row{grid-template-columns:1fr}.house{font-size:14px}}
</style></head>
<body><div class="app">
<header><div class="brand"><div class="logo">🏆</div><div><h1>المنشية</h1><div class="sub">إصدار المطوّر • مغامرة من 100 مستوى</div></div></div><div class="topstats"><span class="chip">⭐ <b id="topScore">0</b></span><span class="chip">🏅 مستوى <b id="topLevel">1</b></span></div></header>
<section class="hero"><h2>مرحبًا بك في المنشية</h2><p>واجهة عربية أوضح وخط أكبر، وتحكم بالحركة والجري والتصويب. تقدّم من مستوى إلى الأصعب حتى الكأس.</p></section>
<main>
<h2 class="title">📌 أقسام اللعبة</h2><div class="tabs"><button class="tab active" data-tab="game">🎮 اللعب</button><button class="tab" data-tab="map">🗺️ المناطق</button><button class="tab" data-tab="tasks">🎯 المهام</button><button class="tab" data-tab="settings">⚙️ الإعدادات</button></div>
<h2 class="title" id="sectionTitle">🎮 ساحة اللعب</h2>
<div class="content"><section class="card"><div class="bar"><b id="area">المستوى 1 — بحري البلد</b><div class="stats"><span class="stat">❤️ <b id="health">3</b></span><span class="stat">👾 <b id="remaining">4</b></span><span class="stat">🏆 <b id="level">1</b>/100</span></div></div><canvas id="game" width="900" height="560"></canvas>
<div class="actionrow"><button class="btn gold" id="start">▶ ابدأ اللعب</button><button class="btn" id="reset">↻ إعادة المستوى</button><button class="btn" id="music">🎵 تشغيل الموسيقى</button></div>
<div class="joystick-row"><div class="pad"><div class="padlabel">🕹️ الحركة والجري</div><div class="directions"><span></span><button class="dir" data-move="up">▲</button><span></span><button class="dir" data-move="left">◀</button><button class="dir" data-move="down">▼</button><button class="dir" data-move="right">▶</button></div><button class="btn green" id="run" style="width:100%;margin-top:6px">🏃 الجري السريع</button></div><div class="pad"><div class="padlabel">🎯 التصويب</div><div class="directions"><span></span><button class="dir" data-aim="up">▲</button><span></span><button class="dir" data-aim="left">◀</button><button class="dir" data-aim="down">▼</button><button class="dir" data-aim="right">▶</button></div><button class="btn fire" id="fire" style="width:100%;margin-top:6px">🎯 إطلاق كرتوني</button></div></div>
</section><aside class="card side"><div id="tabContent"><div class="mission"><strong id="mission">المهمة الأولى</strong><p id="description">اهزم الوحوش الكرتونية وأكمل المستوى. عند الفوز ينتقل اللعب تلقائيًا إلى مستوى أصعب.</p></div><h3>🏠 معالم الشارع</h3><div class="housegrid" id="houses"><div class="house">🛒 سوبر ماركت أبوحذيفة — أول الشارع</div><div class="house">🏡 بيت الحج علي أبوحجازي</div><div class="house">🏠 بيت عمي نمر</div><div class="house">🥙 شارع علي بتاع الطعمية</div></div><div class="hudhelp">💡 استخدم أزرار الحركة أو مفاتيح الأسهم. زر الجري يزيد سرعة اللاعب. زر الموسيقى يشغّل لحنًا إلكترونيًا بسيطًا.</div></div></aside></div>
</main><div class="footer">لعبة خيالية كرتونية. هذه نسخة ويب مطوّرة وليست لعبة ثلاثية الأبعاد كاملة مثل PUBG.</div></div>
<div class="overlay" id="welcome"><div class="welcome"><div class="welcomeimg"></div><div class="welcomebody"><h2>لعبة المنشية</h2><p>نسخة مطوّرة بخط كبير، أقسام إضافية، لاعب مرسوم يتحرك ويجري، ومراحل تزداد صعوبة حتى المستوى 100.</p><button class="btn gold" id="enter">ابدأ المغامرة 🚀</button></div></div></div>
<script>
const $=id=>document.getElementById(id),canvas=$('game'),ctx=canvas.getContext('2d');
const zones=[
{name:'بحري البلد',street:'شارع علي بتاع الطعمية',sky:'#c8e7f0',ground:'#9bab7c',build:'#d8b995',houses:['🛒 سوبر ماركت أبوحذيفة — أول الشارع','🏡 بيت الحج علي أبوحجازي','🏠 بيت عمي نمر','🥙 شارع علي بتاع الطعمية']},
{name:'منطقة السوق',street:'ممرات السوق',sky:'#d5e2f2',ground:'#9eae86',build:'#e2c87e',houses:['🥬 سوق الخضار','🧺 محل العطارة','🛍️ سوق المنشية','🚪 بوابة السوق']},
{name:'آخر المنشية',street:'طريق الجسر',sky:'#c7dced',ground:'#849f9c',build:'#c4c9d6',houses:['🌉 طريق الجسر','🏘️ البيوت القديمة','💡 ميدان الأنوار','🏆 ساحة الكأس']}];
let zone=0,level=Math.max(1,Math.min(100,Number(localStorage.mansheyaLevel||1))),score=Number(localStorage.mansheyaScore||0),health=3,playing=false,shots=[],monsters=[],px=450,py=455,frame=0,won=false,running=false,aim={x:0,y:-1},audioCtx=null,musicOn=false,musicTimer=null;
function rounded(x,y,w,h,r,color){ctx.fillStyle=color;ctx.beginPath();ctx.roundRect(x,y,w,h,r);ctx.fill()}
function person(x,y,scale=1){ // stylized drawn character, animated limbs
 let stride=playing?Math.sin(frame*.28)*(running?12:5):0;
 ctx.save();ctx.translate(x,y);ctx.scale(scale,scale);
 // shadow
 ctx.fillStyle='#18212c55';ctx.beginPath();ctx.ellipse(0,26,22,8,0,0,7);ctx.fill();
 // boots and legs
 ctx.strokeStyle='#202b3b';ctx.lineWidth=9;ctx.lineCap='round';
 ctx.beginPath();ctx.moveTo(-6,8);ctx.lineTo(-9+stride,24);ctx.lineTo(-13+stride,27);ctx.stroke();
 ctx.beginPath();ctx.moveTo(6,8);ctx.lineTo(9-stride,24);ctx.lineTo(14-stride,27);ctx.stroke();
 // torso/vest
 rounded(-13,-19,26,30,7,'#3e5e4a');rounded(-10,-16,20,22,4,'#526f57');
 ctx.fillStyle='#b8a47b';ctx.fillRect(-11,-5,22,4);
 // arms
 ctx.strokeStyle='#c99d77';ctx.lineWidth=7;
 ctx.beginPath();ctx.moveTo(-11,-13);ctx.lineTo(-19,-3);ctx.stroke();
 ctx.beginPath();ctx.moveTo(11,-13);ctx.lineTo(17,-8);ctx.stroke();
 // head + helmet
 ctx.fillStyle='#d6a27e';ctx.beginPath();ctx.arc(0,-28,10,0,7);ctx.fill();
 ctx.fillStyle='#485b50';ctx.beginPath();ctx.arc(0,-31,11,Math.PI,Math.PI*2);ctx.lineTo(11,-28);ctx.lineTo(-11,-28);ctx.fill();
 // toy-style blaster pointed toward aim
 ctx.save();ctx.translate(14,-10);ctx.rotate(Math.atan2(aim.y,aim.x));rounded(-1,-4,22,8,2,'#202a35');ctx.fillStyle='#a9b5bf';ctx.fillRect(14,-2,8,4);ctx.restore();
 ctx.restore()
}
function draw(){let z=zones[zone];ctx.clearRect(0,0,900,560);let g=ctx.createLinearGradient(0,0,0,560);g.addColorStop(0,z.sky);g.addColorStop(1,'#f0e1c5');ctx.fillStyle=g;ctx.fillRect(0,0,900,560);
ctx.fillStyle='#fff0a8';ctx.beginPath();ctx.arc(790,70,34,0,7);ctx.fill();
for(let i=0;i<9;i++){let bx=i*105-8,h=115+(i%3)*25;ctx.fillStyle=z.build;ctx.fillRect(bx,175-h/4,96,h+40);ctx.fillStyle='#5c8d9b';ctx.fillRect(bx+13,194-h/4,21,32);ctx.fillRect(bx+60,194-h/4,21,32);ctx.fillStyle='#9b6245';ctx.fillRect(bx+39,230-h/4,22,48);ctx.fillStyle='#986b48';ctx.fillRect(bx,175-h/4,96,7)}
ctx.fillStyle=z.ground;ctx.fillRect(0,325,900,235);ctx.fillStyle='#b7a48c';ctx.beginPath();ctx.moveTo(245,325);ctx.lineTo(655,325);ctx.lineTo(900,560);ctx.lineTo(0,560);ctx.fill();ctx.strokeStyle='#e7d7b6';ctx.lineWidth=5;ctx.setLineDash([25,22]);ctx.beginPath();ctx.moveTo(450,355);ctx.lineTo(450,560);ctx.stroke();ctx.setLineDash([]);
z.houses.slice(0,3).forEach((s,i)=>{let bx=18+i*295;rounded(bx,245,260,43,5,'#fff2d7');ctx.strokeStyle='#81583c';ctx.lineWidth=2;ctx.strokeRect(bx,245,260,43);ctx.fillStyle='#43372d';ctx.font='bold 16px Tahoma';ctx.textAlign='center';ctx.fillText(s.replace(/^[^ ]+ /,'').slice(0,23),bx+130,273)});
shots.forEach(s=>{ctx.fillStyle='#ffd66b';ctx.beginPath();ctx.arc(s.x,s.y,5,0,7);ctx.fill();ctx.strokeStyle='#fff5c2';ctx.lineWidth=2;ctx.beginPath();ctx.moveTo(s.x-8,s.y);ctx.lineTo(s.x-17,s.y);ctx.stroke()});
monsters.forEach(m=>{ // hand-drawn cartoon monster, not emoji
ctx.fillStyle='#25364c66';ctx.beginPath();ctx.ellipse(m.x,m.y+23,20,6,0,0,7);ctx.fill();rounded(m.x-18,m.y-15,36,35,9,'#7855ad');rounded(m.x-23,m.y-8,8,16,3,'#7855ad');rounded(m.x+15,m.y-8,8,16,3,'#7855ad');ctx.fillStyle='#fff';ctx.beginPath();ctx.arc(m.x-7,m.y-5,5,0,7);ctx.arc(m.x+7,m.y-5,5,0,7);ctx.fill();ctx.fillStyle='#1a2030';ctx.beginPath();ctx.arc(m.x-6,m.y-4,2,0,7);ctx.arc(m.x+8,m.y-4,2,0,7);ctx.fill();ctx.strokeStyle='#251a36';ctx.lineWidth=3;ctx.beginPath();ctx.moveTo(m.x-7,m.y+8);ctx.lineTo(m.x+7,m.y+8);ctx.stroke();ctx.fillStyle='#29384b';ctx.fillRect(m.x-20,m.y-26,40,5);ctx.fillStyle='#ff6c6c';ctx.fillRect(m.x-20,m.y-26,40*m.hp/m.max,5)});
person(px,py,running?1.03:1);rounded(15,15,355,43,10,'#101827dd');ctx.fillStyle='#fff';ctx.textAlign='right';ctx.font='bold 20px Tahoma';ctx.fillText('المستوى '+level+' — '+z.name,350,43);
if(!playing){ctx.fillStyle='#07101cbb';ctx.fillRect(0,0,900,560);ctx.fillStyle='#fff';ctx.textAlign='center';ctx.font='bold 38px Tahoma';ctx.fillText(won?(level===100?'🏆 فزت بالكأس!':'فزت! المستوى التالي'):'المنشية',450,225);ctx.font='bold 22px Tahoma';ctx.fillText(won?(level===100?'أكملت 100 مستوى بنجاح':'استعد لتحدٍ أصعب'):'اضغط «ابدأ اللعب»',450,273);ctx.font='18px Tahoma';ctx.fillText('تحرّك واجمع النقاط وأبعد الوحوش الكرتونية',450,310)}}
function update(){ $('topScore').textContent=score;$('topLevel').textContent=level;$('level').textContent=level;$('health').textContent=health;$('remaining').textContent=monsters.length;$('area').textContent='المستوى '+level+' — '+zones[zone].name}
function save(){localStorage.mansheyaLevel=level;localStorage.mansheyaScore=score}
function reset(){health=3;shots=[];monsters=[];px=450;py=455;frame=0;won=false;playing=false;let n=Math.min(4+Math.floor(level/5),18);for(let i=0;i<n;i++){let hp=1+Math.floor(level/10);monsters.push({x:65+(i*91)%760,y:345+(i%2)*42,hp,max:hp,speed:.3+level*.018})}update();draw()}
function move(dir){let d=running?42:23;if(dir==='left')px=Math.max(35,px-d);if(dir==='right')px=Math.min(865,px+d);if(dir==='up')py=Math.max(340,py-d);if(dir==='down')py=Math.min(520,py+d);if(dir==='left')aim={x:-1,y:0};if(dir==='right')aim={x:1,y:0};if(dir==='up')aim={x:0,y:-1};if(dir==='down')aim={x:0,y:1};draw()}
function fire(){if(!playing)return;shots.push({x:px+aim.x*25,y:py-15+aim.y*22,vx:aim.x*10,vy:aim.y*10})}
function loop(){if(!playing)return;frame++;shots.forEach(s=>{s.x+=s.vx;s.y+=s.vy});shots=shots.filter(s=>s.x>0&&s.x<900&&s.y>0&&s.y<560);monsters.forEach(m=>{m.x+=Math.sin((frame+m.y)/22)*m.speed;m.y+=Math.sin(frame/80+m.x)*.07});
for(let i=monsters.length-1;i>=0;i--){let m=monsters[i];for(let j=shots.length-1;j>=0;j--){let s=shots[j];if(Math.abs(m.x-s.x)<25&&Math.abs(m.y-s.y)<25){m.hp--;shots.splice(j,1);if(m.hp<=0){monsters.splice(i,1);score+=10}break}}}
if(frame%Math.max(100,270-level)===0&&monsters.length){health--;if(health<=0){health=0;playing=false;$('mission').textContent='حاول مرة أخرى';$('description').textContent='انتهت المحاولة. أعد المستوى لتستمر نحو الكأس.'}}
if(monsters.length===0&&playing){playing=false;won=true;if(level>=100){level=100;score+=500;$('mission').textContent='🏆 بطل المنشية!';$('description').textContent='مبروك! أكملت المستويات المئة وفزت بالكأس الذهبي.'}else{score+=25;level++;$('mission').textContent='فزت! وصلت إلى المستوى '+level;$('description').textContent='المستوى التالي أصعب. اضغط «ابدأ اللعب» للمتابعة.'}save()}
update();draw();if(playing)requestAnimationFrame(loop)}
document.querySelectorAll('.tab').forEach(b=>b.onclick=()=>{document.querySelectorAll('.tab').forEach(t=>t.classList.toggle('active',t===b));let t=b.dataset.tab;let titles={game:'🎮 ساحة اللعب',map:'🗺️ المناطق',tasks:'🎯 المهام',settings:'⚙️ الإعدادات'};$('sectionTitle').textContent=titles[t];if(t==='map'){$('description').textContent='بحري البلد: شارع علي بتاع الطعمية. منطقة السوق: ممرات المحلات. آخر المنشية: طريق الجسر وساحة الكأس.'}else if(t==='tasks'){$('description').textContent='المهمة: اهزم كل الوحوش في المستوى. الفوز يزيد النقاط وينقلك إلى المستوى التالي الأصعب حتى 100.'}else if(t==='settings'){$('description').textContent='يمكنك تشغيل الموسيقى أو إيقافها، واستخدام أزرار الجري والحركة والتصويب. الخط والأزرار مصممة لتكون واضحة على الهاتف.'}else{$('description').textContent='اهزم الوحوش الكرتونية وأكمل المستوى. عند الفوز ينتقل اللعب تلقائيًا إلى مستوى أصعب.'}});
$('enter').onclick=()=>{$('welcome').style.display='none';draw()};$('start').onclick=()=>{if(won)reset();playing=true;won=false;$('mission').textContent='المهمة — المستوى '+level;$('description').textContent='اهزم الوحوش الكرتونية. كل فوز يرفع مستوى الصعوبة.';draw();loop()};$('reset').onclick=reset;$('fire').onclick=fire;$('run').onpointerdown=()=>{running=true};$('run').onpointerup=$('run').onpointerleave=()=>{running=false};document.querySelectorAll('[data-move]').forEach(b=>b.onclick=()=>move(b.dataset.move));document.querySelectorAll('[data-aim]').forEach(b=>b.onclick=()=>{let d=b.dataset.aim;aim=d==='left'?{x:-1,y:0}:d==='right'?{x:1,y:0}:d==='up'?{x:0,y:-1}:{x:0,y:1};draw()});
canvas.addEventListener('pointerdown',e=>{if(!playing)return;let r=canvas.getBoundingClientRect();px=Math.max(30,Math.min(870,(e.clientX-r.left)/r.width*900));py=Math.max(340,Math.min(530,(e.clientY-r.top)/r.height*560));fire();draw()});document.addEventListener('keydown',e=>{if(e.key==='ArrowLeft')move('left');if(e.key==='ArrowRight')move('right');if(e.key==='ArrowUp')move('up');if(e.key==='ArrowDown')move('down');if(e.key===' ')fire();if(e.key==='Shift')running=true});document.addEventListener('keyup',e=>{if(e.key==='Shift')running=false});
function startMusic(){try{audioCtx=audioCtx||new(window.AudioContext||window.webkitAudioContext)();audioCtx.resume();musicOn=true;$('music').textContent='🎵 إيقاف الموسيقى';let notes=[261.63,329.63,392,329.63,293.66,349.23,440,349.23],i=0;function tick(){if(!musicOn)return;let o=audioCtx.createOscillator(),v=audioCtx.createGain();o.type='triangle';o.frequency.value=notes[i++%notes.length];v.gain.setValueAtTime(.0001,audioCtx.currentTime);v.gain.exponentialRampToValueAtTime(.035,audioCtx.currentTime+.05);v.gain.exponentialRampToValueAtTime(.0001,audioCtx.currentTime+.3);o.connect(v);v.connect(audioCtx.destination);o.start();o.stop(audioCtx.currentTime+.32);musicTimer=setTimeout(tick,340)}tick()}catch(e){alert('اضغط زر الموسيقى مرة أخرى في Chrome لتشغيلها')}}$('music').onclick=()=>{if(musicOn){musicOn=false;clearTimeout(musicTimer);$('music').textContent='🎵 تشغيل الموسيقى'}else startMusic()};
if('serviceWorker'in navigator)window.addEventListener('load',()=>navigator.serviceWorker.register('./sw.js').catch(()=>{}));reset();
</script></body></html>'''
(base / "index.html").write_text(html, encoding="utf-8")
manifest = {"name":"المنشية - إصدار المطور","short_name":"المنشية","description":"مغامرة المنشية العربية حتى المستوى 100","lang":"ar","dir":"rtl","start_url":"./index.html","scope":"./","display":"standalone","orientation":"portrait","background_color":"#0b1220","theme_color":"#101a28","icons":[{"src":"assets/icon-192.png","sizes":"192x192","type":"image/png"},{"src":"assets/icon-512.png","sizes":"512x512","type":"image/png"}]}
(base / "manifest.json").write_text(json.dumps(manifest,ensure_ascii=False,indent=2),encoding="utf-8")
(base / "sw.js").write_text("""const C='mansheya-pro-v2';const F=['./','./index.html','./manifest.json','./assets/icon-192.png','./assets/icon-512.png','./assets/street-splash.jpg'];self.addEventListener('install',e=>e.waitUntil(caches.open(C).then(c=>c.addAll(F))));self.addEventListener('activate',e=>e.waitUntil(caches.keys().then(a=>Promise.all(a.filter(k=>k!==C).map(k=>caches.delete(k))))));self.addEventListener('fetch',e=>e.respondWith(caches.match(e.request).then(r=>r||fetch(e.request).catch(()=>caches.match('./index.html')))));""",encoding="utf-8")
(base / "طريقة-التثبيت.txt").write_text("""لعبة المنشية - الإصدار المطوّر

التحسينات:
- خط عربي أكبر وخانات وأقسام أكثر.
- لاعب مرسوم يتحرك بأطراف متحركة، وزر جري سريع، واتجاهات للحركة والتصويب.
- 100 مستوى تزداد صعوبتها مع التقدم، ثم كأس عند المستوى 100.
- موسيقى إلكترونية اختيارية.
- الصورة التي أرسلها المستخدم في شاشة البداية وأيقونات التطبيق.
- حفظ المستوى والنقاط على الجهاز.

التثبيت كتطبيق:
1) فك ضغط mansheya_pro_pwa.zip.
2) ارفع كل الملفات والمجلدات كما هي إلى مستودع GitHub.
3) من Settings > Pages اختر Branch: main وFolder: /(root).
4) افتح رابط GitHub Pages في Chrome على الهاتف.
5) من قائمة Chrome اختر «تثبيت التطبيق» أو «إضافة إلى الشاشة الرئيسية».

تنبيه: هذا تطبيق ويب تقدمي برسومات ثنائية الأبعاد وتحكم فعلي بالحركة داخل اللعبة؛ ليس محرك 3D حقيقيًا مثل PUBG ولا APK أصليًا.
""",encoding="utf-8")
zip_path=Path("/mnt/data/mansheya_pro_pwa.zip")
with zipfile.ZipFile(zip_path,"w",zipfile.ZIP_DEFLATED) as zf:
    for f in base.rglob("*"):
        if f.is_file(): zf.write(f,f.relative_to(base.parent))
print("تم إنشاء الإصدار المطوّر وحزمة التثبيت.")
# -
my first mobile name 
