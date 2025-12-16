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
  font-family:-apple-system, BlinkMacSystemFont, sans-serif;
  background:url('sena.jpg') no-repeat center center fixed;
  background-size:cover;
}

/* BLUR */
body.blurred::before{
  content:"";
  position:fixed;
  inset:0;
  backdrop-filter: blur(8px);
  background:rgba(0,0,0,.25);
  z-index:998;
}

/* FOTOĞRAFLAR */
.photo{
  position:absolute;
  width:70px;
  border-radius:14px;
  cursor:pointer;
  box-shadow:0 0 15px rgba(200,0,255,.6);
  z-index:5;
  touch-action:none;
}

/* 📱 iPhone 11–13 */
@media (max-width: 430px){
  .photo{
    width:52px;
    border-radius:12px;
  }
}

/* MODAL */
.modal{
  position:fixed;
  inset:0;
  background:rgba(0,0,0,.78);
  display:none;
  align-items:center;
  justify-content:center;
  z-index:999;
}
.modal-content{
  background:#2b0033;
  padding:16px;
  border-radius:18px;
  color:#fff;
  width:92%;
  max-height:82vh;
  text-align:center;
  overflow:auto;
}

/* 📱 Mesaj okunurluğu */
#modalText{
  font-size:15px;
  line-height:1.45;
  margin-top:8px;
  padding:0 6px;
}

/* 📱 iPhone 11–13 mesaj ayarı */
@media (max-width: 430px){
  #modalText{
    font-size:14.5px;
    line-height:1.5;
  }
}

.close{
  margin-top:10px;
  padding:7px 18px;
  background:#a64dff;
  border-radius:20px;
  cursor:pointer;
}

/* MODAL FOTO */
#modalImg{
  max-width:100%;
  max-height:50vh;
  border-radius:14px;
  margin-bottom:6px;
  touch-action:none;
  transition:transform .2s ease;
  transform-origin:center center;
}

/* KALPLER */
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

/* ŞİİR */
.poem-container{
  position:fixed;
  width:100%;
  bottom:-100%;
  color:#ff99ff;
  font-size:1.05rem;
  text-align:center;
  white-space:pre-wrap;
  line-height:1.4rem;
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
let scale=1;

/* KALPLER */
for(let i=0;i<20;i++){
  const h=document.createElement('div');
  h.className='heart';
  h.innerText='❤';
  h.style.left=Math.random()*100+'vw';
  h.style.color=colors[Math.floor(Math.random()*2)];
  h.style.fontSize=14+Math.random()*18+'px';
  h.style.animationDuration=10+Math.random()*10+'s';
  document.body.appendChild(h);
}

/* 50 ÖZEL MESAJ */
const messages=[
"Yüzüne baktığımda içim sakinleşiyor 💜",
"Gülüşün dünyadaki en güzel manzara 😊",
"Bakışların kalbime dokunuyor ✨",
"Gözlerinle insanın içini ısıtıyorsun 🌸",
"Saçlarının her hali ayrı güzel 💫",
"Yüzündeki o ifade her şeyi unutturuyor 🤍",
"Gülüşün karanlık günleri aydınlatıyor ☀️",
"Bakışlarında huzur var 🌿",
"Gözlerine baktığımda dünya susuyor 🌌",
"Saçlarının rüzgârda savruluşu çok güzel 🍃",
"Yüzün mutluluğun tanımı gibi 💖",
"Gülüşün kalbimi yumuşatıyor 💕",
"Bakışların insana kendini özel hissettiriyor ✨",
"Gözlerin gecenin yıldızları gibi 🌙",
"Saçların yüzüne ayrı bir zarafet katıyor 🌸",
"Yüzüne her baktığımda gülümsüyorum 😊",
"Gülüşün içimi ısıtıyor 🔥",
"Bakışlarında sevgi var 💌",
"Gözlerin insanı kendine çekiyor 💫",
"Saçlarının dokusu bile huzur veriyor 🌿",
"Yüzün aklımdan çıkmıyor 🤍",
"Gülüşün en sevdiğim detay 💜",
"Bakışların kalbimi durduruyor ✨",
"Gözlerinle başka bir dünya var 🌌",
"Saçların seni daha da özel yapıyor 💎",
"Yüzün rüya gibi 🌙",
"Gülüşün hayatı güzelleştiriyor 🌈",
"Bakışların içimi sakinleştiriyor 🕊️",
"Gözlerinle her şey daha anlamlı 💖",
"Saçlarının her teli ayrı güzel ✨",
"Yüzün bana huzuru hatırlatıyor 🌿",
"Gülüşün kalbimin ritmi 💓",
"Bakışların ruhuma iyi geliyor 🤍",
"Gözlerinle zaman yavaşlıyor ⏳",
"Saçların çok zarif duruyor 🌸",
"Yüzündeki masumluk çok güzel 😊",
"Gülüşünle her şey yoluna giriyor ☀️",
"Bakışların sevildiğimi hissettiriyor 💌",
"Gözlerin kalbimin en sevdiği yer 💜",
"Yüzün, gülüşün ve bakışların… hepsi çok özel 💖"
];

/* FOTOĞRAFLAR */
for(let i=1;i<=50;i++){
  const img=document.createElement('img');
  img.src=i+'.jpg';
  img.className='photo';

  let x=Math.random()*(innerWidth-80);
  let y=Math.random()*(innerHeight-80);
  let dx=(Math.random()-.5)*0.3;
  let dy=(Math.random()-.5)*0.3;
  let isDragging=false, offsetX=0, offsetY=0;

  function move(){
    if(!isDragging){
      x+=dx; y+=dy;
      if(x<0||x>innerWidth-60) dx*=-1;
      if(y<0||y>innerHeight-60) dy*=-1;
      img.style.left=x+'px';
      img.style.top=y+'px';
    }
    requestAnimationFrame(move);
  }

  img.onclick=()=>{
    if(isDragging) return;
    currentIndex=i;
    openModal(messages[i-1],img.src);
  };

  /* MOUSE + MOBİL DRAG */
  img.addEventListener("mousedown",e=>{
    isDragging=true;
    offsetX=e.clientX-x;
    offsetY=e.clientY-y;
  });
  img.addEventListener("touchstart",e=>{
    if(e.touches.length!==1) return;
    isDragging=true;
    offsetX=e.touches[0].clientX-x;
    offsetY=e.touches[0].clientY-y;
  });
  document.addEventListener("mousemove",e=>{
    if(!isDragging) return;
    x=e.clientX-offsetX; y=e.clientY-offsetY;
  });
  document.addEventListener("touchmove",e=>{
    if(!isDragging||e.touches.length!==1) return;
    e.preventDefault();
    x=e.touches[0].clientX-offsetX;
    y=e.touches[0].clientY-offsetY;
  });
  document.addEventListener("mouseup",()=>isDragging=false);
  document.addEventListener("touchend",()=>isDragging=false);

  document.body.appendChild(img);
  move();
}

/* MODAL */
function openModal(text,src){
  scale=1;
  modalImg.src=src;
  modalImg.style.transform='scale(1)';
  modalText.innerText=text;
  modal.style.display='flex';
  document.body.classList.add('blurred');
}
function closeModal(){
  modal.style.display='none';
  document.body.classList.remove('blurred');
}
</script>

</body>
</html>

