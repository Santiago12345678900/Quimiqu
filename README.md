
<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>COMING SOON — La puerta secreta</title>
<style>
*{box-sizing:border-box}
html,body{margin:0;width:100%;height:100%;overflow:hidden;background:#080909;font-family:Arial,sans-serif}
#scene{position:fixed;inset:0;width:100%;height:100%;display:block}
#hud{position:fixed;inset:0;pointer-events:none;color:#f2eee7}
#status,#sound{position:absolute;top:15px;background:#000b;padding:10px 12px;border:1px solid #ffffff25;border-radius:6px;font-size:11px}
#status{left:15px}
#sound{right:15px;color:#ccc}
#hint{position:absolute;left:50%;bottom:20px;transform:translateX(-50%);max-width:92vw;width:max-content;text-align:center;background:#000c;padding:12px 16px;border:1px solid #ffffff2a;border-radius:8px;font-size:14px}
#flash{position:fixed;inset:0;background:white;opacity:0;pointer-events:none;z-index:3}
#ending{display:none;position:fixed;z-index:5;inset:0;background:white;color:#171717;align-items:center;justify-content:center;flex-direction:column;text-align:center;padding:20px}
#ending.show{display:flex}
#ending h1{font-size:clamp(32px,6vw,68px);margin:4px 0}
#cookieCanvas{width:min(340px,70vw);height:min(240px,32vh)}
#countdown{font-size:clamp(23px,4vw,38px);font-weight:bold;margin:14px}
#restart{border:0;border-radius:99px;background:#171717;color:white;padding:13px 22px;font-weight:bold;cursor:pointer}
</style>
</head>
<body>
<canvas id="scene"></canvas>
<div id="hud">
 <div id="status">ARCHIVO 001 · ACCESO RESTRINGIDO</div>
 <div id="sound">Haz clic para activar el sonido</div>
 <div id="hint">Haz clic en el candado para golpearlo. La llave dorada está en el suelo.</div>
</div>
<div id="flash"></div>
<section id="ending">
 <canvas id="cookieCanvas"></canvas>
 <h1>TEN UNA COOKIE</h1>
 <p>La puerta se abrió. El siguiente capítulo llegará pronto.</p>
 <div id="countdown">03 : 00 : 00 : 00</div>
 <p>DÍAS · HORAS · MINUTOS · SEGUNDOS</p>
 <button id="restart">REINICIAR EXPERIENCIA</button>
</section>

<script type="module">
import * as THREE from 'https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js';

const renderer = new THREE.WebGLRenderer({
  canvas: document.querySelector('#scene'),
  antialias: true
});
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
renderer.setSize(innerWidth, innerHeight);
renderer.shadowMap.enabled = true;
renderer.shadowMap.type = THREE.PCFSoftShadowMap;
renderer.toneMapping = THREE.ACESFilmicToneMapping;
renderer.outputColorSpace = THREE.SRGBColorSpace;

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x080909);
scene.fog = new THREE.FogExp2(0x080909, .027);

const camera = new THREE.PerspectiveCamera(48, innerWidth / innerHeight, .1, 100);
camera.position.set(0, 3.1, 11);
camera.lookAt(0, 2.1, -1);

scene.add(new THREE.HemisphereLight(0xaab8c5, 0x21170f, 1.15));

const sun = new THREE.DirectionalLight(0xffd9ad, 2.6);
sun.position.set(-4, 8, 5);
sun.castShadow = true;
sun.shadow.mapSize.set(2048, 2048);
scene.add(sun);

const blueLight = new THREE.PointLight(0x54788a, 16, 16);
blueLight.position.set(4, 4, 2);
scene.add(blueLight);

const M = (c, r=.8, m=0) =>
  new THREE.MeshStandardMaterial({color:c, roughness:r, metalness:m});

const rust=M(0x503426,.9,.3);
const rust2=M(0x80543a,.85,.25);
const steel=M(0x292d2d,.42,.75);
const floorM=M(0x1a1918,1);
const wallM=M(0x292623,1);
const gold=M(0xf0bd42,.22,.9);
const dark=M(0x111718,.45,.3);

function box(parent,name,size,pos,material){
  const o=new THREE.Mesh(new THREE.BoxGeometry(...size),material);
  o.name=name;
  o.position.set(...pos);
  o.castShadow=true;
  o.receiveShadow=true;
  parent.add(o);
  return o;
}

function cyl(parent,name,r1,r2,h,pos,material,seg=16,rot=[0,0,0]){
  const o=new THREE.Mesh(
    new THREE.CylinderGeometry(r1,r2,h,seg),material
  );
  o.name=name;
  o.position.set(...pos);
  o.rotation.set(...rot);
  o.castShadow=true;
  o.receiveShadow=true;
  parent.add(o);
  return o;
}

// ESCENARIO
box(scene,'suelo',[200,.2,200],[0,-.13,-1],floorM);
box(scene,'pared vieja',[18,9,.35],[0,4,-3.9],wallM);

for(let i=0;i<24;i++){
  const stain=box(scene,'mancha de humedad',
    [.2+Math.random()*.9,.025,.015],
    [-8+Math.random()*16,.3+Math.random()*7,-3.7],
    M(Math.random()>.5?0x38302b:0x171819));
  stain.rotation.z=Math.random()*2;
}

for(let i=0;i<24;i++){
  const crack=box(scene,'grieta del suelo',
    [.25+Math.random()*1.2,.012,.025],
    [-7+Math.random()*14,0,-1+Math.random()*5],
    M(0x080909));
  crack.rotation.y=Math.random()*1.5;
}

// PUERTA OXIDADA
const gatePivot=new THREE.Group();
gatePivot.position.set(-2.15,0,-2.95);
scene.add(gatePivot);

box(gatePivot,'puerta oxidada',[4.3,5,.22],[2.15,2.5,0],rust);

for(let x=.16;x<4.3;x+=.32){
  box(gatePivot,'barra vertical',[.05,4.84,.05],[x,2.5,.15],rust2);
}

for(let y=.45;y<4.9;y+=.7){
  box(gatePivot,'refuerzo metálico',[4.17,.06,.06],[2.15,y,.19],steel);
}

box(scene,'marco izquierdo',[.24,5.3,.4],[-2.32,2.62,-2.9],rust2);
box(scene,'marco derecho',[.24,5.3,.4],[2.32,2.62,-2.9],rust2);
box(scene,'marco superior',[4.8,.25,.4],[0,5.23,-2.9],rust2);

// CINTAS COMING SOON
function tape(y,angle){
  const g=new THREE.Group();
  g.position.set(0,y,-2.73);
  g.rotation.z=angle;
  scene.add(g);

  const c=document.createElement('canvas');
  c.width=1024;
  c.height=128;
  const x=c.getContext('2d');

  x.fillStyle='#d8b32e';
  x.fillRect(0,0,1024,128);

  for(let k=-40;k<1100;k+=300){
    x.save();
    x.translate(k,64);
    x.rotate(-.5);
    x.fillStyle='#171513';
    x.fillRect(-22,-100,34,200);
    x.restore();
  }

  x.font='bold 58px Arial';
  x.textAlign='center';
  x.textBaseline='middle';
  x.fillStyle='#171513';
  x.fillText('COMING SOON  •  COMING SOON',520,66);

  const tex=new THREE.CanvasTexture(c);
  tex.colorSpace=THREE.SRGBColorSpace;

  box(g,'cinta COMING SOON',[4.5,.34,.06],[0,0,0],
    new THREE.MeshStandardMaterial({map:tex,roughness:.85}));
}
tape(3.75,-.035);
tape(2,.025);

// PANTALLA DIGITAL FÍSICA
const screenCanvas=document.createElement('canvas');
screenCanvas.width=1024;
screenCanvas.height=256;
const screenCtx=screenCanvas.getContext('2d');
const screenTexture=new THREE.CanvasTexture(screenCanvas);
screenTexture.colorSpace=THREE.SRGBColorSpace;

const sign=new THREE.Group();
sign.position.set(0,4.52,-2.56);
scene.add(sign);

box(sign,'carcasa pantalla',[2.3,.82,.22],[0,0,0],steel);
box(sign,'cristal pantalla',[2.08,.54,.025],[0,0,.13],
  new THREE.MeshBasicMaterial({map:screenTexture}));

const screenGlow=new THREE.PointLight(0x65ffad,2,3);
screenGlow.position.set(0,0,.4);
sign.add(screenGlow);

function drawScreen(text){
  screenCtx.fillStyle='#03100b';
  screenCtx.fillRect(0,0,1024,256);
  screenCtx.strokeStyle='#246044';
  screenCtx.lineWidth=8;
  screenCtx.strokeRect(12,12,1000,232);
  screenCtx.fillStyle='#77ffae';
  screenCtx.font='bold 108px monospace';
  screenCtx.textAlign='center';
  screenCtx.textBaseline='middle';
  screenCtx.fillText(text,512,130);
  screenTexture.needsUpdate=true;
}

let screenToggle=0;
drawScreen('FALTA POCO');
setInterval(()=>{
  screenToggle++;
  drawScreen(screenToggle%2?'3 DIAS':'FALTA POCO');
},2000);

// CANDADO
const lockGroup=new THREE.Group();
lockGroup.position.set(.1,2.7,-2.42);
scene.add(lockGroup);

box(lockGroup,'cuerpo del candado',[.6,.72,.2],[0,-.04,0],steel);
box(lockGroup,'frente del candado',[.43,.54,.035],[0,-.04,.12],
  M(0x4a5050,.25,.8));

const shackle=new THREE.Mesh(
  new THREE.TorusGeometry(.19,.055,12,30,Math.PI),gold
);
shackle.position.set(0,.34,.04);
shackle.rotation.z=Math.PI;
shackle.castShadow=true;
lockGroup.add(shackle);

cyl(lockGroup,'cerradura',.045,.045,.02,[0,-.1,.15],
  M(0x080909,.4,.2),16,[Math.PI/2,0,0]);

for(const x of [-.21,.21]){
  for(const y of [-.28,.2]){
    cyl(lockGroup,'tornillo',.023,.023,.02,[x,y,.15],
      M(0x999d97,.3,.8),12,[Math.PI/2,0,0]);
  }
}

// MÁQUINAS ARCADE
function arcade(x,z,rot){
  const g=new THREE.Group();
  g.position.set(x,0,z);
  g.rotation.y=rot;
  scene.add(g);

  box(g,'máquina arcade',[1.15,2.35,.85],[0,1.2,0],dark);
  box(g,'base arcade',[1.22,.25,.9],[0,.13,0],steel);
  box(g,'marquesina',[.95,.27,.08],[0,2.12,.46],M(0x54201b,.45,.2));
  box(g,'bisel de pantalla',[.91,.8,.1],[0,1.56,.46],steel);

  const c=document.createElement('canvas');
  c.width=256;c.height=256;
  const q=c.getContext('2d');
  q.fillStyle='#06110e';
  q.fillRect(0,0,256,256);
  q.fillStyle=x<0?'#43e7b5':'#ff5347';
  q.font='bold 31px monospace';
  q.textAlign='center';
  q.fillText('INSERT',128,104);
  q.fillText('COIN',128,147);

  for(let i=0;i<12;i++){
    q.fillStyle='#ffffff22';
    q.fillRect(0,i*22,256,2);
  }

  const tex=new THREE.CanvasTexture(c);
  tex.colorSpace=THREE.SRGBColorSpace;
  box(g,'pantalla arcade',[.76,.63,.025],[0,1.56,.52],
    new THREE.MeshBasicMaterial({map:tex}));

  const panel=box(g,'panel de controles',[.88,.12,.42],[0,1.03,.42],steel);
  panel.rotation.x=-.15;

  for(let i=0;i<3;i++){
    cyl(g,'botón arcade',.052,.052,.035,
      [-.2+i*.2,1.11,.52],
      M([0xd34b3b,0xe3c44b,0x49a6dc][i],.35,.2),
      16,[Math.PI/2,0,0]);
  }

  cyl(g,'base joystick',.08,.08,.04,[.25,1.11,.48],
    steel,16,[Math.PI/2,0,0]);
  cyl(g,'palanca joystick',.023,.023,.16,[.25,1.2,.48],
    M(0x777777,.3,.7),12);

  const glow=new THREE.PointLight(x<0?0x38ffb7:0xff4538,2,3);
  glow.position.set(0,1.7,.7);
  g.add(glow);
}

arcade(-4.4,-.7,.2);
arcade(4.25,-.5,-.18);
arcade(-5.25,1.25,.35);
arcade(5.15,1.55,-.3);

// FAJOS DE BILLETES
function cash(x,z,r){
  const g=new THREE.Group();
  g.position.set(x,.04,z);
  g.rotation.y=r;
  scene.add(g);

  const paper=M(0x6b8b61,.96);
  const band=M(0xd5c8a0,.85);

  for(let i=0;i<5;i++){
    box(g,'billete',[.48,.024,.24],[0,i*.025,0],paper);
  }

  box(g,'banda de billetes',[.11,.15,.26],[0,.05,0],band);
  box(g,'impresión de billete',[.025,.008,.14],[.14,.14,0],M(0x36583c));
}
cash(-2.9,.9,.4);
cash(2.6,1.3,-.7);
cash(3.2,-.2,.25);
cash(-1.9,1.9,-.2);

// LLAVE DORADA VISIBLE EN EL SUELO
const keyRoot=new THREE.Group();
keyRoot.name='llave dorada';
const keyMat=M(0xf3bf46,.2,.92);

const ring=new THREE.Mesh(new THREE.TorusGeometry(.16,.045,12,32),keyMat);
ring.rotation.x=Math.PI/2;
ring.position.set(-.23,.09,0);
ring.castShadow=true;
keyRoot.add(ring);

cyl(keyRoot,'vástago de llave',.038,.038,.52,[.08,.09,0],
  keyMat,16,[0,0,Math.PI/2]);

box(keyRoot,'diente de llave',[.13,.075,.075],[.29,.09,0],keyMat);
box(keyRoot,'diente superior',[.07,.12,.075],[.22,.13,0],keyMat);

keyRoot.position.set(1.05,.015,.65);
keyRoot.rotation.set(.02,.2,.04);
scene.add(keyRoot);

const keyHitbox=new THREE.Mesh(
  new THREE.BoxGeometry(.85,.35,.48),
  new THREE.MeshBasicMaterial({transparent:true,opacity:0,depthWrite:false})
);
keyHitbox.position.set(.04,.12,0);
keyRoot.add(keyHitbox);

// POLVO AMBIENTAL
const dustCount=180;
const dustPositions=new Float32Array(dustCount*3);

for(let i=0;i<dustCount;i++){
  dustPositions[i*3]=-7+Math.random()*14;
  dustPositions[i*3+1]=Math.random()*6;
  dustPositions[i*3+2]=-2+Math.random()*7;
}

const dustGeo=new THREE.BufferGeometry();
dustGeo.setAttribute('position',new THREE.BufferAttribute(dustPositions,3));
const dust=new THREE.Points(dustGeo,
  new THREE.PointsMaterial({color:0xb6a28a,size:.025,transparent:true,opacity:.4}));
scene.add(dust);

// SONIDOS GENERADOS POR EL NAVEGADOR
let audioCtx=null;

function enableAudio(){
  if(!audioCtx) audioCtx=new (window.AudioContext||window.webkitAudioContext)();
  if(audioCtx.state==='suspended') audioCtx.resume();
  document.querySelector('#sound').textContent='Sonido activado';
}

function hitSound(){
  enableAudio();
  const now=audioCtx.currentTime;
  const osc=audioCtx.createOscillator();
  const gain=audioCtx.createGain();

  osc.type='triangle';
  osc.frequency.setValueAtTime(175,now);
  osc.frequency.exponentialRampToValueAtTime(48,now+.34);
  gain.gain.setValueAtTime(.001,now);
  gain.gain.exponentialRampToValueAtTime(.3,now+.015);
  gain.gain.exponentialRampToValueAtTime(.001,now+.48);
  osc.connect(gain);
  gain.connect(audioCtx.destination);
  osc.start(now);
  osc.stop(now+.5);

  const buffer=audioCtx.createBuffer(1,audioCtx.sampleRate*.15,audioCtx.sampleRate);
  const data=buffer.getChannelData(0);
  for(let i=0;i<data.length;i++){
    data[i]=(Math.random()*2-1)*Math.pow(1-i/data.length,2);
  }

  const noise=audioCtx.createBufferSource();
  const ng=audioCtx.createGain();
  noise.buffer=buffer;
  ng.gain.value=.12;
  noise.connect(ng);
  ng.connect(audioCtx.destination);
  noise.start();
}

function angelSound(){
  enableAudio();
  const now=audioCtx.currentTime;

  [261.63,329.63,392,523.25,659.25].forEach((f,i)=>{
    const o=audioCtx.createOscillator();
    const g=audioCtx.createGain();
    o.type='sine';
    o.frequency.value=f;
    g.gain.setValueAtTime(.001,now);
    g.gain.linearRampToValueAtTime(.055,now+.6);
    g.gain.exponentialRampToValueAtTime(.001,now+4.5);
    o.connect(g);
    g.connect(audioCtx.destination);
    o.start(now+i*.12);
    o.stop(now+4.7);
  });
}

// IMPACTOS, HUMO Y CHISPAS
const particles=[];
let lockKick=0,hitCount=0,keyCollected=false,doorOpening=false,ended=false;

function burst(pos,count=22){
  for(let i=0;i<count;i++){
    const mesh=new THREE.Mesh(
      new THREE.SphereGeometry(.018+Math.random()*.035,6,6),
      new THREE.MeshBasicMaterial({
        color:Math.random()>.45?0xc8b7a1:0xffbd5c,
        transparent:true,opacity:.8
      })
    );
    mesh.position.copy(pos);
    scene.add(mesh);
    particles.push({
      mesh,age:0,life:.45+Math.random()*.6,
      vel:new THREE.Vector3((Math.random()-.5)*1.8,Math.random()*1.7,(Math.random()-.5)*1.2)
    });
  }
}

function hitLock(){
  if(doorOpening||ended)return;
  hitSound();
  hitCount++;
  lockKick=.28;
  burst(lockGroup.position.clone().add(new THREE.Vector3(0,0,.2)),18);
  document.querySelector('#hint').textContent=
    hitCount<3
    ?'¡CLANG! El candado resiste. Sigue golpeándolo o encuentra la llave.'
    :'El candado vibra. Haz clic en la llave dorada del suelo para abrir la puerta.';
}

function unlockDoor(){
  if(doorOpening||ended)return;
  doorOpening=true;
  enableAudio();
  document.querySelector('#hint').textContent='La llave encaja. La puerta se está abriendo…';
  burst(new THREE.Vector3(0,2.5,-2.4),45);

  const start=performance.now();
  const duration=2800;
  const camStart=camera.position.clone();

  function openStep(now){
    const t=Math.min((now-start)/duration,1);
    const ease=1-Math.pow(1-t,3);

    gatePivot.rotation.y=-1.48*ease;
    camera.position.lerpVectors(camStart,new THREE.Vector3(0,2.5,5.3),ease);
    camera.lookAt(0,2.25,-3.4);
    blueLight.intensity=16+ease*30;

    if(t<1){
      requestAnimationFrame(openStep);
    }else{
      const flash=document.querySelector('#flash');
      flash.style.transition='opacity 1.1s';
      flash.style.opacity='1';

      setTimeout(()=>{
        document.querySelector('#ending').classList.add('show');
        document.querySelector('#hud').style.display='none';
        flash.style.opacity='0';
        angelSound();
        ended=true;
        startCountdown();
      },1000);
    }
  }

  requestAnimationFrame(openStep);
}

// INTERACCIÓN DIRECTA CON CLICS
const raycaster=new THREE.Raycaster();
const pointer=new THREE.Vector2();

renderer.domElement.addEventListener('pointerdown',event=>{
  if(ended||doorOpening)return;
  enableAudio();

  const rect=renderer.domElement.getBoundingClientRect();
  pointer.x=((event.clientX-rect.left)/rect.width)*2-1;
  pointer.y=-((event.clientY-rect.top)/rect.height)*2+1;
  raycaster.setFromCamera(pointer,camera);

  const keyHits=raycaster.intersectObject(keyHitbox,true);

  if(!keyCollected&&keyHits.length){
    keyCollected=true;
    document.querySelector('#hint').textContent='¡Encontraste la llave!';

    const start=performance.now();
    const duration=850;
    const from=keyRoot.position.clone();
    const to=new THREE.Vector3(.05,2.55,-2.1);

    function pick(now){
      const t=Math.min((now-start)/duration,1);
      keyRoot.position.lerpVectors(from,to,t);
      keyRoot.rotation.y+=.045;
      keyRoot.scale.setScalar(1-.65*t);

      if(t<1){
        requestAnimationFrame(pick);
      }else{
        keyRoot.visible=false;
        unlockDoor();
      }
    }

    requestAnimationFrame(pick);
    return;
  }

  const lockHits=raycaster.intersectObject(lockGroup,true);
  if(lockHits.length){
    hitLock();
    return;
  }
});

// COOKIE 3D EN LA ESCENA FINAL
const cookieCanvas=document.querySelector('#cookieCanvas');
const cookieRenderer=new THREE.WebGLRenderer({
  canvas:cookieCanvas,alpha:true,antialias:true
});
cookieRenderer.setPixelRatio(Math.min(devicePixelRatio,2));
cookieRenderer.setSize(340,240);
cookieRenderer.setClearColor(0xffffff,0);

const cookieScene=new THREE.Scene();
const cookieCamera=new THREE.PerspectiveCamera(38,340/240,.1,50);
cookieCamera.position.set(0,1.2,5.4);
cookieCamera.lookAt(0,0,0);
cookieScene.add(new THREE.HemisphereLight(0xffffff,0x8b5830,2));

const cookieLight=new THREE.DirectionalLight(0xffffff,3);
cookieLight.position.set(-3,5,5);
cookieScene.add(cookieLight);

const cookieGroup=new THREE.Group();
cookieScene.add(cookieGroup);

const cookieMat=new THREE.MeshStandardMaterial({color:0xb8793d,roughness:.92});
const cookie=new THREE.Mesh(new THREE.CylinderGeometry(1.15,1.12,.28,64),cookieMat);
cookie.rotation.x=.12;
cookieGroup.add(cookie);

const chipsMat=new THREE.MeshStandardMaterial({color:0x432317,roughness:.7});
for(let i=0;i<18;i++){
  const a=i*2.399;
  const r=.18+Math.sqrt(i/18)*.83;
  const chip=new THREE.Mesh(new THREE.SphereGeometry(.075+Math.random()*.025,10,8),chipsMat);
  chip.scale.y=.45;
  chip.position.set(Math.cos(a)*r,.17,Math.sin(a)*r);
  cookieGroup.add(chip);
}
cookieGroup.rotation.x=-.18;

// CUENTA REGRESIVA DE 3 DÍAS
function startCountdown(){
  const end=Date.now()+3*24*60*60*1000;

  function update(){
    let s=Math.max(0,Math.floor((end-Date.now())/1000));
    const d=Math.floor(s/86400);s%=86400;
    const h=Math.floor(s/3600);s%=3600;
    const m=Math.floor(s/60);s%=60;
    document.querySelector('#countdown').textContent=
      [d,h,m,s].map(n=>String(n).padStart(2,'0')).join(' : ');
  }

  update();
  setInterval(update,1000);
}

document.querySelector('#restart').addEventListener('click',()=>location.reload());

// BUCLE DE ANIMACIÓN
const clock=new THREE.Clock();
let elapsed=0;

function animate(){
  requestAnimationFrame(animate);
  const dt=Math.min(clock.getDelta(),.04);
  elapsed+=dt;

  if(!doorOpening&&!ended){
    lockGroup.rotation.z=Math.sin(elapsed*1.8)*.012;

    if(lockKick>0){
      lockGroup.position.x=.1+(Math.random()-.5)*lockKick;
      lockGroup.rotation.z+=(Math.random()-.5)*lockKick;
      lockKick=Math.max(0,lockKick-dt);

      if(lockKick===0){
        lockGroup.position.x=.1;
        lockGroup.rotation.z=0;
      }
    }
  }

  if(!keyCollected)keyRoot.rotation.z=Math.sin(elapsed*2)*.025;
  dust.rotation.y=elapsed*.015;

  for(let i=particles.length-1;i>=0;i--){
    const p=particles[i];
    p.age+=dt;
    p.mesh.position.addScaledVector(p.vel,dt);
    p.vel.y-=.7*dt;
    p.mesh.material.opacity=Math.max(0,.8*(1-p.age/p.life));
    p.mesh.scale.multiplyScalar(1+dt*.3);

    if(p.age>=p.life){
      scene.remove(p.mesh);
      p.mesh.geometry.dispose();
      p.mesh.material.dispose();
      particles.splice(i,1);
    }
  }

  renderer.render(scene,camera);

  if(document.querySelector('#ending').classList.contains('show')){
    cookieGroup.rotation.y=elapsed*.45;
    cookieRenderer.render(cookieScene,cookieCamera);
  }
}

animate();

addEventListener('resize',()=>{
  camera.aspect=innerWidth/innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(innerWidth,innerHeight);
});
</script>
</body>
</html>

