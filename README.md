<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>DTF Halftone Studio</title>
<style>
:root{--bg:#111;--pn:#1a1a1a;--bd:#2a2a2a;--tx:#fff;--t2:#999;--ac:#7c5cfc}
*{box-sizing:border-box;margin:0}
html,body{height:100%}
body{background:var(--bg);color:var(--tx);font:13px/1.4 system-ui,Segoe UI,Roboto,sans-serif;display:flex;flex-direction:column;overflow:hidden}
header{height:48px;display:flex;align-items:center;gap:8px;padding:0 14px;background:var(--pn);border-bottom:1px solid var(--bd);flex:none}
header h1{font-size:15px;font-weight:600;margin-right:auto}header h1 span{color:var(--ac)}
button,select,input[type=text],input[type=number]{font:inherit;color:var(--tx);background:#222;border:1px solid var(--bd);border-radius:6px;padding:6px 10px}
button{cursor:pointer}button:hover{border-color:var(--ac)}
.pri{background:var(--ac);border-color:var(--ac);font-weight:600}
main{flex:1;display:grid;grid-template-columns:280px 1fr 280px;min-height:0}
aside{background:var(--pn);overflow:auto;padding:10px;display:flex;flex-direction:column;gap:10px}
aside.l{border-right:1px solid var(--bd)}aside.r{border-left:1px solid var(--bd)}
.card{background:#151515;border:1px solid var(--bd);border-radius:8px;padding:10px;display:flex;flex-direction:column;gap:8px}
.card h3{font-size:11px;text-transform:uppercase;letter-spacing:.08em;color:var(--t2);font-weight:600}
label{display:flex;justify-content:space-between;color:var(--t2);font-size:12px}label b{color:var(--tx);font-weight:500}
input[type=range]{width:100%;accent-color:var(--ac)}
input[type=color]{width:38px;height:32px;padding:0;border:1px solid var(--bd);border-radius:6px;background:none}
.row{display:flex;gap:6px;align-items:center}.row>*{flex:1;min-width:0}
.seg{display:flex}.seg button{flex:1;border-radius:0;padding:6px 4px;font-size:12px}
.seg button:first-child{border-radius:6px 0 0 6px}.seg button:last-child{border-radius:0 6px 6px 0}
.seg button.on{background:var(--ac);border-color:var(--ac)}
.sw{display:flex;gap:6px;flex-wrap:wrap}.sw button{width:26px;height:26px;padding:0;border-radius:50%}
.tgl{display:flex;gap:8px;align-items:center;color:var(--t2)}
.ctr{display:flex;flex-direction:column;min-width:0;min-height:0}
.bar{display:flex;gap:6px;padding:8px;background:var(--pn);border-bottom:1px solid var(--bd);flex-wrap:wrap;align-items:center}
.bar .seg{flex:none}.bar .sp{margin-left:auto;display:flex;gap:6px;align-items:center}
#vp{flex:1;position:relative;background:#0b0b0b;min-height:0;cursor:grab;touch-action:none}
#vp:active{cursor:grabbing}#cv{position:absolute;inset:0;width:100%;height:100%}
#dz{position:absolute;inset:16px;border:2px dashed var(--bd);border-radius:12px;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:10px;color:var(--t2);text-align:center;cursor:pointer;z-index:2;background:#0b0b0b}
#dz.over{border-color:var(--ac);color:var(--tx)}
.th{width:100%;height:auto;border-radius:6px;border:1px solid var(--bd);cursor:pointer;background:#0b0b0b}
.info{color:var(--t2);font-size:12px;word-break:break-all}.info b{color:var(--tx);font-weight:500}
.priv{color:var(--t2);font-size:11px;text-align:center;margin-top:auto}
@media(max-width:900px){body{overflow:auto}main{display:flex;flex-direction:column}.ctr{order:-1;height:62vh;flex:none}aside{overflow:visible}}
</style>
</head>
<body>
<header>
  <h1>DTF <span>Halftone</span> Studio</h1>
  <button id="bNew" title="Descartar a imagem e começar de novo">Novo projeto</button>
  <button id="bReset" title="Voltar todos os parâmetros ao padrão">Restaurar</button>
  <button id="bExp" class="pri" title="Exportar PNG transparente">Exportar</button>
</header>
<main>
<aside class="l">
  <div class="card"><h3>Camiseta</h3>
    <div class="row"><input type="color" id="cp" value="#000000" style="flex:none"><input type="text" id="hx" value="#000000" maxlength="7" title="Cor HEX"></div>
    <div class="sw" id="sw"></div>
  </div>
  <div class="card"><h3>Aproveitamento da cor</h3>
    <div class="seg" data-k="mode"><button data-v="1" title="Pixels próximos da camiseta ficam transparentes">Knockout</button><button data-v="2" title="Transparência progressiva conforme a proximidade">Suavizado</button><button data-v="3" title="Transição em halftone perto da cor da camiseta">Halftone</button></div>
    <div class="sl" data-s="tol"></div>
  </div>
  <div class="card"><h3>Halftone</h3>
    <div class="seg" data-k="color"><button data-v="0" title="Uma única retícula">Monocromático</button><button data-v="1" title="Mantém as cores originais">Mantendo cores</button></div>
    <select data-k="type" title="Tipo de retícula"><option value="round">Pontos circulares</option><option value="ellipse">Pontos elípticos</option><option value="line">Linhas</option><option value="grid">Grid</option></select>
    <select data-k="ink" title="Cor da tinta no modo monocromático"><option value="auto">Tinta mono: automática</option><option value="white">Tinta mono: branca</option><option value="black">Tinta mono: preta</option></select>
    <div class="sl" data-s="inten"></div><div class="sl" data-s="size"></div><div class="sl" data-s="freq"></div>
    <label>Ângulo</label>
    <select data-k="angle"><option>0</option><option>15</option><option>22.5</option><option>30</option><option>45</option><option>60</option><option>75</option><option>90</option></select>
    <div class="sl" data-s="thr"></div><div class="sl" data-s="contrast"></div>
    <div class="tgl"><input type="checkbox" data-k="inv" id="inv"><label for="inv" style="display:inline">Inverter retícula</label></div>
  </div>
  <div class="priv">Sua arte é processada localmente no navegador.</div>
</aside>

<section class="ctr">
  <div class="bar">
    <div class="seg" id="views"><button data-v="orig">Original</button><button data-v="proc" class="on">Processada</button><button data-v="shirt">Camiseta</button><button data-v="cmp">Comparação</button></div>
    <div class="sp"><button id="zo" title="Reduzir zoom">−</button><span id="zl" style="min-width:42px;text-align:center">100%</span><button id="zi" title="Aumentar zoom">+</button><button id="zf">Ajustar</button><button id="z1">100%</button></div>
  </div>
  <div id="vp">
    <div id="dz"><div style="font-size:15px;color:var(--tx)">Arraste a estampa aqui</div><div>PNG, JPG, JPEG ou WebP</div><button class="pri">Selecionar arquivo</button></div>
    <canvas id="cv"></canvas>
  </div>
  <input type="file" id="fi" accept="image/png,image/jpeg,image/webp" hidden>
</section>

<aside class="r">
  <div class="card"><h3>Pré-visualizações</h3>
    <canvas class="th" id="t1" width="240" height="140" title="Original"></canvas>
    <canvas class="th" id="t2" width="240" height="140" title="Processado"></canvas>
    <canvas class="th" id="t3" width="240" height="140" title="Simulação na camiseta"></canvas>
  </div>
  <div class="card"><h3>Arquivo</h3><div class="info" id="info">Nenhuma imagem carregada.</div></div>
  <div class="card"><h3>Presets</h3>
    <div class="row"><button data-p="suave" title="Transição discreta e pontos menores">Suave</button><button data-p="medio" title="Equilíbrio entre detalhe e economia">Médio</button><button data-p="forte" title="Maior uso da cor da camiseta">Forte</button></div>
    <div class="row"><button data-p="preto">Preto</button><button data-p="branco">Branco</button></div>
  </div>
  <div class="card"><h3>Exportação</h3>
    <select id="exp"><option value="100">100% da resolução</option><option value="75">75%</option><option value="50">50%</option><option value="c">Dimensão personalizada</option></select>
    <input type="number" id="cw" min="16" max="12000" placeholder="Largura (px)" style="display:none">
    <div class="info" id="esz">—</div>
    <button class="pri" id="bDl">BAIXAR ESTAMPA</button>
  </div>
</aside>
</main>

<script>
/* ============ DTF Halftone Studio — módulos: Cor, Processamento, Preview, UI, Exportação ============ */
const $=s=>document.querySelector(s),$$=s=>[...document.querySelectorAll(s)];

/* ---------- Estado e padrões ---------- */
const DEF={shirt:'#000000',tol:30,mode:3,type:'round',color:1,ink:'auto',inten:100,size:100,freq:30,angle:45,contrast:0,thr:50,inv:0};
const PRESETS={
 suave:{mode:3,tol:25,inten:70,size:80,freq:40},
 medio:{mode:3,tol:35,inten:85,size:100,freq:30},
 forte:{mode:3,tol:55,inten:100,size:110,freq:22},
 preto:{shirt:'#000000',mode:3,color:1,tol:35,inten:90,size:100,freq:30},
 branco:{shirt:'#ffffff',mode:3,color:1,tol:30,inten:90,size:100,freq:30}};
const SL={tol:['Tolerância de cor','%',0,100,'Faixa de cores consideradas próximas da camiseta'],
 inten:['Intensidade','%',0,100,'Quanto a retícula atua (0% = sólido)'],
 size:['Tamanho dos pontos','%',30,200,'Ganho/diâmetro dos pontos'],
 freq:['Frequência','lpi',8,80,'Densidade da retícula (linhas por polegada a 300 dpi)'],
 thr:['Threshold','%',0,100,'Limite usado na geração dos pontos'],
 contrast:['Contraste','%',0,100,'Contraste entre áreas claras e escuras']};
let P={...DEF},orig=null,OW=0,OH=0,fname='',prev=null,prevData=null,procC=document.createElement('canvas');
const V={z:1,x:0,y:0,view:'proc'};

/* ---------- Módulo Cor: sRGB → LAB (D65) e Delta E (CIE76) ---------- */
const LIN=Float64Array.from({length:256},(_,i)=>{const c=i/255;return c<=.04045?c/12.92:((c+.055)/1.055)**2.4});
const LAB=[0,0,0],fl=t=>t>.008856?Math.cbrt(t):7.787*t+16/116;
function lab(r,g,b){const R=LIN[r],G=LIN[g],B=LIN[b];
 const x=fl((.4124*R+.3576*G+.1805*B)/.95047),y=fl(.2126*R+.7152*G+.0722*B),z=fl((.0193*R+.1192*G+.9505*B)/1.08883);
 LAB[0]=116*y-16;LAB[1]=500*(x-y);LAB[2]=200*(y-z)}
const hex2rgb=h=>[1,3,5].map(i=>parseInt(h.slice(i,i+2),16));

/* ---------- Módulo Processamento: knockout + halftone (puro, sem DOM) ---------- */
/* src: ImageData | sc: escala relativa à resolução original (preview ou exportação) */
function process(src,W,H,sc,P){
 const d=src.data,out=new ImageData(W,H),o=out.data;
 const [sr,sg,sb]=hex2rgb(P.shirt);lab(sr,sg,sb);const sL=LAB[0],sA=LAB[1],sB=LAB[2];
 const ink=P.ink==='white'?[255,255,255]:P.ink==='black'?[0,0,0]:(sL<=55?[255,255,255]:[0,0,0]);
 const inkL=ink[0]>127,T=Math.max(P.tol,.5),kk=1+P.contrast/100*4,thr=P.thr/100,inten=P.inten/100;
 const cell=Math.max(2,300/P.freq*sc),gam=1/(P.size/100),ang=P.angle*Math.PI/180,cs=Math.cos(ang),sn=Math.sin(ang);
 const de=()=>Math.sqrt((LAB[0]-sL)**2+(LAB[1]-sA)**2+(LAB[2]-sB)**2);
 let cI=1e9,cJ=1e9,cc=1;
 for(let y=0;y<H;y++)for(let x=0;x<W;x++){
  const i=(y*W+x)*4,a0=d[i+3];if(!a0)continue;
  /* 1) Aproveitamento da cor da camiseta (modos 1 e 2) */
  let ak=1;
  if(P.mode<3){lab(d[i],d[i+1],d[i+2]);const e=de();
   ak=P.mode===1?(e<=T?0:1):Math.min(1,Math.max(0,(e-T*.35)/(T*.65)));if(ak<=0)continue}
  /* 2) Retícula rotacionada; cobertura amostrada no centro da célula */
  const u=x*cs+y*sn,v=y*cs-x*sn,I=Math.floor(u/cell),J=Math.floor(v/cell);
  if(I!==cI||J!==cJ){cI=I;cJ=J;
   const cu=(I+.5)*cell,cv=(J+.5)*cell;
   let px=Math.round(cu*cs-cv*sn),py=Math.round(cu*sn+cv*cs);
   px=px<0?0:px>=W?W-1:px;py=py<0?0:py>=H?H-1:py;
   let k=(py*W+px)*4;if(!d[k+3])k=i;
   lab(d[k],d[k+1],d[k+2]);const prox=Math.min(1,de()/T),tone=inkL?LAB[0]/100:1-LAB[0]/100;
   const val=P.mode===3?(P.color?prox:tone*prox):tone;
   let t=Math.min(1,Math.max(0,.5+(val-thr)*kk));if(P.inv)t=1-t;
   cc=Math.pow(1+(t-1)*inten,gam)}
  if(cc<.02)continue;
  let m=1;
  if(cc<.995){const du=u-(I+.5)*cell,dv=v-(J+.5)*cell;
   if(P.type==='line')m=cc*cell*.5-Math.abs(dv)+.5;
   else if(P.type==='grid')m=cell*.5*Math.sqrt(cc)-Math.max(Math.abs(du),Math.abs(dv))+.5;
   else{const r=cell*.7071*Math.sqrt(cc),dd=P.type==='ellipse'?Math.hypot(du*.7,dv*1.43):Math.hypot(du,dv);m=r-dd+.5}
   m=m>1?1:m}
  const a=a0*ak*m;if(a<=0)continue;
  if(P.color){o[i]=d[i];o[i+1]=d[i+1];o[i+2]=d[i+2]}else{o[i]=ink[0];o[i+1]=ink[1];o[i+2]=ink[2]}
  o[i+3]=a}
 return out}

/* ---------- Módulo Preview ---------- */
const cv=$('#cv'),cx=cv.getContext('2d'),vp=$('#vp'),DPR=Math.min(2,window.devicePixelRatio||1);
const PAT=(()=>{const c=document.createElement('canvas');c.width=c.height=16;const g=c.getContext('2d');g.fillStyle='#262626';g.fillRect(0,0,16,16);g.fillStyle='#1c1c1c';g.fillRect(0,0,8,8);g.fillRect(8,8,8,8);return cx.createPattern(c,'repeat')})();
function bg(x,dx,dy,w,h,shirt){x.save();x.beginPath();x.rect(dx,dy,w,h);x.clip();
 if(shirt){x.fillStyle=shirt;x.fillRect(dx,dy,w,h)}else{x.setTransform(1,0,0,1,0,0);x.fillStyle=PAT;x.fillRect(0,0,x.canvas.width,x.canvas.height)}x.restore()}
/* kinds: orig | proc | shirt (processada sobre camiseta) | oshirt (original sobre camiseta) */
function drawKind(x,k,dx,dy,w,h){bg(x,dx,dy,w,h,k.endsWith('shirt')?P.shirt:null);x.drawImage(k[0]==='o'?prev:procC,dx,dy,w,h)}
function draw(){
 cx.setTransform(1,0,0,1,0,0);cx.clearRect(0,0,cv.width,cv.height);if(!prev)return;
 cx.setTransform(DPR*V.z,0,0,DPR*V.z,DPR*V.x,DPR*V.y);cx.imageSmoothingEnabled=V.z<3;
 if(V.view==='cmp'){drawKind(cx,'oshirt',0,0,OW,OH);drawKind(cx,'shirt',OW*1.04,0,OW,OH)}
 else drawKind(cx,V.view,0,0,OW,OH);
 $('#zl').textContent=Math.round(V.z*100)+'%'}
function thumbs(){[['#t1','orig'],['#t2','proc'],['#t3','shirt']].forEach(([id,k])=>{
 const c=$(id),x=c.getContext('2d');x.setTransform(1,0,0,1,0,0);x.clearRect(0,0,c.width,c.height);if(!prev)return;
 const s=Math.min(c.width/OW,c.height/OH);drawKind(x,k,(c.width-OW*s)/2,(c.height-OH*s)/2,OW*s,OH*s)})}
function fit(){if(!prev)return;const w=V.view==='cmp'?OW*2.04:OW,vw=vp.clientWidth,vh=vp.clientHeight;
 V.z=Math.min(vw/w,vh/OH)*.92;V.x=(vw-w*V.z)/2;V.y=(vh-OH*V.z)/2;draw()}
function zoomAt(f,px,py){V.x=px-(px-V.x)*f;V.y=py-(py-V.y)*f;V.z=Math.min(32,Math.max(.02,V.z*f));draw()}
function resize(){cv.width=vp.clientWidth*DPR;cv.height=vp.clientHeight*DPR;draw()}
new ResizeObserver(resize).observe(vp);
vp.addEventListener('wheel',e=>{e.preventDefault();const r=vp.getBoundingClientRect();zoomAt(e.deltaY<0?1.15:1/1.15,e.clientX-r.left,e.clientY-r.top)},{passive:false});
let drag=null;
vp.addEventListener('pointerdown',e=>{if(e.target.closest('#dz'))return;drag=[e.clientX,e.clientY];vp.setPointerCapture(e.pointerId)});
vp.addEventListener('pointermove',e=>{if(!drag)return;V.x+=e.clientX-drag[0];V.y+=e.clientY-drag[1];drag=[e.clientX,e.clientY];draw()});
vp.addEventListener('pointerup',()=>drag=null);
$('#zi').onclick=()=>zoomAt(1.25,vp.clientWidth/2,vp.clientHeight/2);
$('#zo').onclick=()=>zoomAt(1/1.25,vp.clientWidth/2,vp.clientHeight/2);
$('#zf').onclick=fit;
$('#z1').onclick=()=>{V.z=1;V.x=(vp.clientWidth-OW)/2;V.y=(vp.clientHeight-OH)/2;draw()};
function setView(v){V.view=v;$$('#views button').forEach(b=>b.classList.toggle('on',b.dataset.v===v));fit()}
$('#views').onclick=e=>{if(e.target.dataset.v)setView(e.target.dataset.v)};
$('#t1').onclick=()=>setView('orig');$('#t2').onclick=()=>setView('proc');$('#t3').onclick=()=>setView('shirt');

/* ---------- Pipeline do preview (debounce + resolução reduzida) ---------- */
let tmr;const schedule=()=>{clearTimeout(tmr);tmr=setTimeout(run,100)};
function run(){if(!prevData)return;
 const out=process(prevData,prev.width,prev.height,prev.width/OW,P);
 procC.width=prev.width;procC.height=prev.height;procC.getContext('2d').putImageData(out,0,0);
 draw();thumbs();info()}
function info(){if(!orig)return;const e=expSize();
 $('#info').innerHTML=`<b>${fname}</b><br>Original: <b>${OW}×${OH}px</b><br>Preview: ${prev.width}×${prev.height}px<br>Camiseta: <b>${P.shirt}</b>`;
 $('#esz').innerHTML=`Largura <b>${e.w}</b> px · Altura <b>${e.h}</b> px · <b>${e.w}×${e.h}</b>`}

/* ---------- Carregamento de imagem ---------- */
function load(file){if(!file||!/^image\/(png|jpe?g|webp)$/.test(file.type))return;
 const img=new Image();img.onload=()=>{
  let w=img.naturalWidth,h=img.naturalHeight;const mx=Math.sqrt(64e6/(w*h));if(mx<1){w=Math.floor(w*mx);h=Math.floor(h*mx)}
  orig=document.createElement('canvas');orig.width=OW=w;orig.height=OH=h;orig.getContext('2d').drawImage(img,0,0,w,h);
  URL.revokeObjectURL(img.src);fname=file.name;
  const s=Math.min(1,1400/Math.max(w,h));prev=document.createElement('canvas');prev.width=Math.round(w*s);prev.height=Math.round(h*s);
  const g=prev.getContext('2d',{willReadFrequently:true});g.imageSmoothingQuality='high';g.drawImage(orig,0,0,prev.width,prev.height);
  prevData=g.getImageData(0,0,prev.width,prev.height);
  $('#dz').style.display='none';run();fit()};
 img.src=URL.createObjectURL(file)}
$('#dz').onclick=()=>$('#fi').click();
$('#fi').onchange=e=>{load(e.target.files[0]);e.target.value=''};
['dragover','dragenter'].forEach(t=>document.addEventListener(t,e=>{e.preventDefault();$('#dz').classList.add('over')}));
['dragleave','drop'].forEach(t=>document.addEventListener(t,e=>{e.preventDefault();$('#dz').classList.remove('over')}));
document.addEventListener('drop',e=>load(e.dataTransfer.files[0]));
$('#bNew').onclick=()=>{orig=prev=prevData=null;$('#dz').style.display='';$('#info').textContent='Nenhuma imagem carregada.';$('#esz').textContent='—';draw();thumbs()};

/* ---------- Controles ---------- */
$$('.sl').forEach(el=>{const k=el.dataset.s,[n,,mn,mx,tip]=SL[k];
 el.innerHTML=`<label title="${tip}">${n}<b data-o="${k}"></b></label><input type="range" data-k="${k}" min="${mn}" max="${mx}" step="1">`});
const SW={Preto:'#000000',Branco:'#ffffff',Cinza:'#808080','Azul-marinho':'#1b2a49',Vermelho:'#d32f2f',Verde:'#2e7d32',Bege:'#d9c7a3'};
$('#sw').innerHTML=Object.entries(SW).map(([n,c])=>`<button title="${n}" data-c="${c}" style="background:${c}"></button>`).join('');
function sync(){$$('[data-k]').forEach(el=>{const k=el.dataset.k;
 if(el.tagName==='DIV')[...el.children].forEach(b=>b.classList.toggle('on',+b.dataset.v===+P[k]));
 else if(el.type==='checkbox')el.checked=!!P[k];else el.value=P[k]});
 $$('[data-o]').forEach(el=>el.textContent=P[el.dataset.o]+SL[el.dataset.o][1]);
 $('#cp').value=P.shirt;$('#hx').value=P.shirt;
 $('[data-k=ink]').style.display=P.color?'none':''}
function setShirt(h){h=h.trim();if(/^#?[0-9a-f]{3}$/i.test(h))h='#'+h.replace('#','').split('').map(c=>c+c).join('');
 if(!/^#?[0-9a-f]{6}$/i.test(h))return;P.shirt=(h[0]==='#'?h:'#'+h).toLowerCase();sync();schedule()}
document.addEventListener('input',e=>{const k=e.target.dataset&&e.target.dataset.k;if(!k||e.target.tagName==='DIV')return;
 const t=e.target;P[k]=t.type==='checkbox'?+t.checked:(isNaN(t.value)?t.value:+t.value);sync();schedule()});
$$('.seg[data-k]').forEach(s=>s.onclick=e=>{if(e.target.dataset.v==null)return;P[s.dataset.k]=+e.target.dataset.v;sync();schedule()});
$('#cp').oninput=e=>setShirt(e.target.value);
$('#hx').oninput=e=>setShirt(e.target.value);
$('#sw').onclick=e=>{if(e.target.dataset.c)setShirt(e.target.dataset.c)};
$$('[data-p]').forEach(b=>b.onclick=()=>{Object.assign(P,PRESETS[b.dataset.p]);sync();schedule()});
$('#bReset').onclick=()=>{P={...DEF};sync();schedule()};

/* ---------- Exportação (PNG transparente, processamento em resolução cheia) ---------- */
function expSize(){const o=$('#exp').value;$('#cw').style.display=o==='c'?'':'none';
 let w=o==='c'?(+$('#cw').value||OW):Math.round(OW*o/100);w=Math.max(16,Math.min(12000,w));
 return{w,h:Math.max(1,Math.round(OH*w/OW))}}
$('#exp').onchange=()=>{if($('#exp').value==='c'&&!$('#cw').value)$('#cw').value=OW;info()};
$('#cw').oninput=info;
function download(){if(!orig)return;const b=$('#bDl'),t=b.textContent;b.textContent='Processando…';
 setTimeout(()=>{const{w,h}=expSize(),c=document.createElement('canvas');c.width=w;c.height=h;
  const g=c.getContext('2d',{willReadFrequently:true});g.imageSmoothingQuality='high';g.drawImage(orig,0,0,w,h);
  g.putImageData(process(g.getImageData(0,0,w,h),w,h,w/OW,P),0,0);
  c.toBlob(bl=>{const a=document.createElement('a');a.href=URL.createObjectURL(bl);
   a.download=fname.replace(/\.[^.]+$/,'')+'_dtf_halftone.png';a.click();setTimeout(()=>URL.revokeObjectURL(a.href),4000);b.textContent=t},'image/png')},30)}
$('#bDl').onclick=download;$('#bExp').onclick=download;

sync();
</script>
</body>
</html>
