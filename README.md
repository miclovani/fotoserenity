# <!DOCTYPE html>
<html lang="it">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Collage Cerchi 5cm - A4 Orizzontale</title>
<style>
@page { size: A4 landscape; margin: 1cm; }
body { font-family: Arial, Helvetica, sans-serif; text-align: center; padding: 12px; }
h1 { margin-bottom:8px; }
.info { font-size:13px; color:#333; margin-bottom:6px; }
.grid {
  display: grid;
  grid-template-columns: repeat(4, 5cm);
  grid-template-rows: repeat(4, 5cm);
  gap: 0.5cm;
  justify-content: center;
  align-content: center;
  margin-top: 10px;
}
.circle { width:5cm; height:5cm; border:1px dashed #333; border-radius:50%; position:relative; overflow:hidden; background:#f9f9f9; }
.circle img { position:absolute; top:0; left:0; cursor:grab; user-select:none; -webkit-user-drag:none; width:120%; height:auto; transform-origin:0 0; border-radius:50%; }
.overlay { position:absolute; inset:0; display:flex; align-items:center; justify-content:center; color:#666; font-size:13px; pointer-events:none; }
.fileinput { position:absolute; inset:0; opacity:0; cursor:pointer; }
.controls { margin-top:12px; }
button { padding:8px 12px; font-size:14px; cursor:pointer; }
@media print { .controls, .info, h1 { display:none; } body { margin:0; } .circle { border:1px solid #000; } }
</style>
</head>
<body>
<h1>Collage Cerchi 5 cm – A4 Orizzontale</h1>
<div class="info">Tocca un cerchio per caricare una foto. Poi trascina per centrarla. Stampa in A4 orizzontale (i segni di taglio appaiono in stampa).</div>
<div class="grid" id="grid">
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
<div class="circle"><div class="overlay">Tocca per caricare</div><input class="fileinput" type="file" accept="image/*" onchange="loadImage(event,this)"></div>
</div>
<div class="controls">
  <button onclick="window.print()">🖨️ Stampa / Salva come PDF</button>
  <button onclick="clearAll()">🗑️ Pulisci tutte</button>
</div>
<script>
function loadImage(event, input){
  const f = event.target.files[0];
  if(!f) return;
  const reader = new FileReader();
  reader.onload = function(e){
    const parent = input.parentNode;
    parent.querySelectorAll('img').forEach(i=>i.remove());
    parent.querySelectorAll('.overlay').forEach(o=>o.style.display='none');
    const img = document.createElement('img');
    img.src = e.target.result;
    img.draggable = false;
    img.style.width = '120%';
    img.style.height = 'auto';
    img.style.left = '-10%';
    img.style.top = '0px';
    parent.appendChild(img);
    img.addEventListener('load', ()=> enablePan(img, parent));
  };
  reader.readAsDataURL(f);
}
function enablePan(img, container){
  let isDown=false, startX=0, startY=0, imgStartLeft=0, imgStartTop=0;
  function clamp(v,min,max){return Math.min(Math.max(v,min),max);}
  function getBounds(){
    const crect = container.getBoundingClientRect();
    const irect = img.getBoundingClientRect();
    const minLeft = Math.min(0, crect.width - irect.width);
    const maxLeft = 0;
    const minTop = Math.min(0, crect.height - irect.height);
    const maxTop = 0;
    return {minLeft, maxLeft, minTop, maxTop};
  }
  img.addEventListener('pointerdown', function(ev){
    ev.preventDefault(); isDown=true; img.setPointerCapture(ev.pointerId);
    startX = ev.clientX; startY = ev.clientY;
    imgStartLeft = parseFloat(img.style.left || 0); imgStartTop = parseFloat(img.style.top || 0);
    img.style.cursor = 'grabbing';
  });
  window.addEventListener('pointermove', function(ev){
    if(!isDown) return;
    const dx = ev.clientX - startX, dy = ev.clientY - startY;
    const b = getBounds();
    let newLeft = imgStartLeft + dx, newTop = imgStartTop + dy;
    newLeft = clamp(newLeft, b.minLeft, b.maxLeft); newTop = clamp(newTop, b.minTop, b.maxTop);
    img.style.left = newLeft + 'px'; img.style.top = newTop + 'px';
  });
  window.addEventListener('pointerup', function(ev){
    if(!isDown) return;
    isDown=false; img.releasePointerCapture && img.releasePointerCapture(ev.pointerId);
    img.style.cursor='grab';
  });
}
function clearAll(){
  document.querySelectorAll('.circle').forEach(s=>{
    s.querySelectorAll('img').forEach(i=>i.remove());
    s.querySelectorAll('.overlay').forEach(o=>o.style.display='flex');
    s.querySelectorAll('input').forEach(inp=>inp.value='');
  });
}
</script>
</body>
</html>fotoserenity<html>
<body style="background:orange;text-align:center;padding:100px;">
<h1>🧲 FOTO SERENITY</h1>
<button style="font-size:20px;padding:20px;" 
        onclick="alert('FUNZIONA! 🎉')">
    CLICCA QUI
</button>
</body>
</html><html>
<body style="background:orange;text-align:center;padding:100px;">
<h1>🧲 FOTO SERENITY</h1>
<button style="font-size:20px;padding:20px;" 
        onclick="alert('FUNZIONA! 🎉')">
    CLICCA QUI
</button>
</body>
</html>
