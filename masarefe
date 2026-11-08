<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ميزانيتي | My Budget | Mon Budget</title>
<style>
*{box-sizing:border-box;font-family:system-ui,Arial}
body{margin:0;background:#0f3d2e;color:#fff;padding:16px}
.card{background:#fff;color:#111;border-radius:18px;padding:16px;margin-bottom:14px}
h1{margin:0;font-size:28px} h1 span{color:#f5b301}
.lang{display:flex;gap:8px;margin:12px 0}
.lang button{flex:1;padding:10px;border:none;border-radius:12px;font-weight:800;cursor:pointer;background:#fff;color:#0f3d2e}
.lang button.active{background:#f5b301}
input,select{width:100%;padding:12px;border:1px solid #ddd;border-radius:12px;margin:6px 0;font-size:16px}
.btn{width:100%;padding:13px;border:none;border-radius:12px;background:#0f3d2e;color:#fff;font-weight:800;font-size:16px;cursor:pointer;margin-top:6px}
.btn-gold{background:#f5b301;color:#111}
.row{display:flex;gap:10px} .row>div{flex:1;background:#f3f3f3;border-radius:14px;padding:12px;text-align:center}
.row b{font-size:20px;display:block}
#alert{display:none;background:#ffdddd;color:#a00;padding:12px;border-radius:12px;font-weight:700;margin-bottom:12px}
#list div{display:flex;justify-content:space-between;padding:10px;border-bottom:1px solid #eee}
canvas{width:100%;height:140px}
</style>
</head>
<body>
<h1>💰 <span id="t-title">ميزانيتي</span></h1>
<p id="t-sub">إدارة مصاريفك بذكاء</p>

<div class="lang">
<button onclick="setLang('ar')" id="b-ar" class="active">العربية</button>
<button onclick="setLang('en')" id="b-en">English</button>
<button onclick="setLang('fr')" id="b-fr">Français</button>
</div>

<div id="alert">⚠️ <span id="t-alert">قربت تخلص ميزانيتك!</span></div>

<div class="card">
<label id="t-budgetLabel">ميزانية الشهر</label>
<input type="number" id="budget" placeholder="100000" oninput="save()">
<label id="t-nameLabel">اسم المصروف</label>
<input type="text" id="name" placeholder="مثال: خضر، كراء...">
<label id="t-amountLabel">المبلغ</label>
<input type="number" id="amount" placeholder="0">
<label id="t-catLabel">التصنيف</label>
<select id="cat"><option>🍔 أكل</option><option>🚗 نقل</option><option>🏠 كراء</option><option>🛍️ تسوق</option><option>💡 فواتير</option><option>✨ أخرى</option></select>
<button class="btn" onclick="addExp()" id="t-add">+ إضافة مصروف</button>
</div>

<div class="card">
<div class="row">
<div><span id="t-b">الميزانية</span><b id="showB">0</b></div>
<div><span id="t-s">صرفت</span><b id="showS" style="color:red">0</b></div>
<div><span id="t-r">بقى</span><b id="showR" style="color:green">0</b></div>
</div>
<canvas id="chart"></canvas>
</div>

<div class="card">
<h3 id="t-list">📋 مصاريفي</h3>
<div id="list"></div>
<button class="btn btn-gold" onclick="clearAll()" id="t-clear">مسح الكل</button>
</div>

<script>
const T={
ar:{title:"ميزانيتي",sub:"إدارة مصاريفك بذكاء",budgetLabel:"ميزانية الشهر",nameLabel:"اسم المصروف",amountLabel:"المبلغ",catLabel:"التصنيف",add:"+ إضافة مصروف",b:"الميزانية",s:"صرفت",r:"بقى",list:"📋 مصاريفي",clear:"مسح الكل",alert:"قربت تخلص ميزانيتك!",del:"حذف"},
en:{title:"My Budget",sub:"Manage your money smartly",budgetLabel:"Monthly Budget",nameLabel:"Expense Name",amountLabel:"Amount",catLabel:"Category",add:"+ Add Expense",b:"Budget",s:"Spent",r:"Left",list:"📋 My Expenses",clear:"Clear All",alert:"You are close to finishing your budget!",del:"Delete"},
fr:{title:"Mon Budget",sub:"Gérez votre argent intelligemment",budgetLabel:"Budget du mois",nameLabel:"Nom de la dépense",amountLabel:"Montant",catLabel:"Catégorie",add:"+ Ajouter une dépense",b:"Budget",s:"Dépensé",r:"Reste",list:"📋 Mes Dépenses",clear:"Tout effacer",alert:"Vous allez bientôt finir votre budget !",del:"Supprimer"}
};
let lang=localStorage.getItem('lang')||'ar';
let exps=JSON.parse(localStorage.getItem('exps')||'[]');
function setLang(l){lang=l;localStorage.setItem('lang',l);apply();document.documentElement.dir=l==='ar'?'rtl':'ltr';document.querySelectorAll('.lang button').forEach(b=>b.classList.remove('active'));document.getElementById('b-'+l).classList.add('active');}
function apply(){
let t=T[lang];
document.getElementById('t-title').innerText=t.title;
document.getElementById('t-sub').innerText=t.sub;
document.getElementById('t-budgetLabel').innerText=t.budgetLabel;
document.getElementById('t-nameLabel').innerText=t.nameLabel;
document.getElementById('t-amountLabel').innerText=t.amountLabel;
document.getElementById('t-catLabel').innerText=t.catLabel;
document.getElementById('t-add').innerText=t.add;
document.getElementById('t-b').innerText=t.b;
document.getElementById('t-s').innerText=t.s;
document.getElementById('t-r').innerText=t.r;
document.getElementById('t-list').innerText=t.list;
document.getElementById('t-clear').innerText=t.clear;
document.getElementById('t-alert').innerText=t.alert;
render();
}
function save(){localStorage.setItem('budget',document.getElementById('budget').value);render();}
function addExp(){
let n=document.getElementById('name').value||T[lang].nameLabel;
let a=parseFloat(document.getElementById('amount').value)||0;
if(!a) return;
exps.push({n,a,c:document.getElementById('cat').value});
localStorage.setItem('exps',JSON.stringify(exps));
document.getElementById('amount').value='';document.getElementById('name').value='';
render();
}
function delExp(i){exps.splice(i,1);localStorage.setItem('exps',JSON.stringify(exps));render();}
function clearAll(){exps=[];localStorage.setItem('exps','[]');render();}
function render(){
let b=parseFloat(document.getElementById('budget').value||localStorage.getItem('budget')||0);
document.getElementById('budget').value=b||'';
let s=exps.reduce((x,e)=>x+e.a,0);
let r=b-s;
document.getElementById('showB').innerText=b.toLocaleString();
document.getElementById('showS').innerText=s.toLocaleString();
document.getElementById('showR').innerText=r.toLocaleString();
document.getElementById('alert').style.display=(b>0 && s>=b*0.8)?'block':'none';
document.getElementById('list').innerHTML=exps.map((e,i)=>`<div><span>${e.c} ${e.n}</span><span>${e.a.toLocaleString()} <button onclick="delExp(${i})">❌</button></span></div>`).join('')||'—';
let cv=document.getElementById('chart'),ctx=cv.getContext('2d');cv.width=300;cv.height=140;
ctx.clearRect(0,0,300,140);
let p=b?Math.min(s/b,1):0;
ctx.fillStyle='#eee';ctx.fillRect(10,60,280,28);
ctx.fillStyle=p>0.8?'red':'#0f9d63';ctx.fillRect(10,60,280*p,28);
ctx.fillStyle='#111';ctx.font='bold 14px Arial';ctx.fillText(Math.round(p*100)+'% '+(lang==='ar'?'مستعمل':lang==='en'?'used':'utilisé'),10,40);
}
setLang(lang);
</script>
</body>
</html>
