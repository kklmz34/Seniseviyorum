
<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sena 💜</title>

<style>
body{
  margin:0;
  overflow:hidden;
  font-family:sans-serif;
  background:url('sena.jpg') no-repeat center center fixed;
  background-size:cover;
}

/* BLUR */
body.blurred::before{
  content:"";
  position:fixed;
  inset:0;
  backdrop-filter: blur(8px);
  background:rgba(0,0,0,.2);
  z-index:998;
}

/* Fotoğraflar */
.photo{
  position:absolute;
  width:70px;
  border-radius:14px;
  cursor:pointer;
  box-shadow:0 0 15px rgba(200,0,255,.6);
  z-index:5;
}

/* MODAL */
.modal{
  position:fixed;
  inset:0;
  background:rgba(0,0,0,.75);
  display:none;
  align-items:center;
  justify-content:center;
  z-index:999;
}
.modal-content{
  background:#2b0033;
  padding:20px;
  border-radius:18px;
  color:#fff;
  max-width:90%;
  max-height:85vh;
  text-align:center;
  overflow:auto; /* 🔴 ZOOM FIX */
}
.close{
  margin-top:12px;
  padding:8px 18px;
  background:#a64dff;
  border-radius:20px;
  cursor:pointer;
}

/* MODAL IMAGE */
#modalImg{
  max-width:100%;
  border-radius:14px;
  margin-bottom:12px;
  touch-action:none;
  transition:transform .2s ease;
  transform-origin:center center; /* 🔴 ZOOM FIX */
}

/* Kalpler */
.heart{
  position:fixed;
  bottom:-20px;
  font-size:18px;
  animation:float 14s linear infinite;
  pointer-events:none;
  z-index:0;
}
@keyframes float{
  from{transform:translateY(0);opacity:.8}
  to{transform:translateY(-120vh);opacity:0}
}

/* Şiir */
.poem-container{
  position:fixed;
  width:100%;
  bottom:-100%;
  color:#ff99ff;
  font-size:1.2rem;
  text-align:center;
  white-space:pre-wrap;
  line-height:1.5rem;
  z-index:0;
  pointer-events:none;
  text-shadow:1px 1px 3px #990066;
  animation:scrollPoem 60s linear infinite;
}
@keyframes scrollPoem{
  0%{bottom:-100%}
  30%{bottom:20%}
  70%{bottom:50%}
  100%{bottom:120%}
}
</style>
</head>

<body>

<div class="poem-container">
Her güzel şeyin bir fazlası sensin,
Ama her güzel şeyde senin eserin.
</div>

<!-- MODAL -->
<div class="modal" id="modal">
  <div class="modal-content">
    <img id="modalImg">
    <div id="modalText"></div>
    <div class="close" onclick="closeModal()">Kapat</div>
  </div>
</div>

<script>
const colors=['#ff4fd8','#c77dff'];
let currentIndex=1;
let scale=1,startX=0,startDist=0,startScale=1;

/* Kalpler */
for(let i=0;i<20;i++){
  const h=document.createElement('div');
  h.className='heart';
  h.innerText='❤';
  h.style.left=Math.random()*100+'vw';
  h.style.color=colors[Math.floor(Math.random()*2)];
  h.style.fontSize=14+Math.random()*20+'px';
  h.style.animationDuration=10+Math.random()*10+'s';
  document.body.appendChild(h);
}

/* Mesajlar */
const messages=[...Array(50)].map((_,i)=>`Seni çok seviyorum 💜 ${i+1}`);

/* Fotoğraflar */
for(let i=1;i<=50;i++){
  const img=document.createElement('img');
  img.src=i+'.jpg';
  img.className='photo';

  let x=Math.random()*(innerWidth-80);
  let y=Math.random()*(innerHeight-80);
  let dx=(Math.random()-.5)*0.3;
  let dy=(Math.random()-.5)*0.3;

  function move(){
    x+=dx; y+=dy;
    if(x<0||x>innerWidth-80) dx*=-1;
    if(y<0||y>innerHeight-80) dy*=-1;
    img.style.left=x+'px';
    img.style.top=y+'px';
    requestAnimationFrame(move);
  }

  img.onclick=()=>{
    currentIndex=i;
    openModal(messages[i-1],img.src);
  };

  document.body.appendChild(img);
  move();
}

/* MODAL */
function openModal(text,src){
  scale=1;
  const img=document.getElementById('modalImg');
  img.src=src;
  img.style.transform='scale(1)';
  document.getElementById('modalText').innerText=text;
  document.getElementById('modal').style.display='flex';
  document.body.classList.add('blurred');
}
function closeModal(){
  document.getElementById('modal').style.display='none';
  document.body.classList.remove('blurred');
}

/* ZOOM – MOUSE */
const modalImg=document.getElementById('modalImg');
modalImg.addEventListener('wheel',e=>{
  e.preventDefault();
  scale+=e.deltaY*-0.0015;
  scale=Math.min(3,Math.max(1,scale));
  modalImg.style.transform=`scale(${scale})`;
});

/* PINCH ZOOM */
modalImg.addEventListener('touchstart',e=>{
  if(e.touches.length===2){
    startDist=Math.hypot(
      e.touches[0].clientX-e.touches[1].clientX,
      e.touches[0].clientY-e.touches[1].clientY
    );
    startScale=scale;
  }
  startX=e.touches[0].clientX;
});
modalImg.addEventListener('touchmove',e=>{
  if(e.touches.length===2){
    const d=Math.hypot(
      e.touches[0].clientX-e.touches[1].clientX,
      e.touches[0].clientY-e.touches[1].clientY
    );
    scale=Math.min(3,Math.max(1,startScale*d/startDist));
    modalImg.style.transform=`scale(${scale})`;
  }
});
modalImg.addEventListener('touchend',e=>{
  if(!e.changedTouches[0]) return;
  const dx=e.changedTouches[0].clientX-startX;
  if(dx>80) showPrev();
  if(dx<-80) showNext();
});

/* GALERİ */
function showNext(){
  currentIndex=currentIndex%50+1;
  updateModal();
}
function showPrev(){
  currentIndex=currentIndex===1?50:currentIndex-1;
  updateModal();
}
function updateModal(){
  scale=1;
  modalImg.style.transform='scale(1)';
  modalImg.src=currentIndex+'.jpg';
  document.getElementById('modalText').innerText=messages[currentIndex-1];
}
</script>

</body>
</html>
