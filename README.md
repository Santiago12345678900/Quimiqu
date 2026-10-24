<title>QUIMIQUNSINI DINERINI</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Bungee&family=Chakra+Petch:wght@500;600;700&family=Press+Start+2P&family=Creepster&display=swap">
<style>
/* Layout: full-bleed arcade stage (canvas) with a slim top bar, machine hotspots, and a bottom dock for chat + navigation. Committed single dark neon look. */
:root{--ink:#0c0824;--panel:#1c1246;--panel2:#2b1a66;--line:#6a52d6;--pink:#ff3fa6;--cyan:#2ff3ff;--yellow:#ffe14d;--lime:#8cff5a;--red:#ff4b4b;--blue:#3d7bff;--orange:#ff9a2e;--text:#ffffff;--mute:#c8bcf2;--fd:'Bungee','Impact',sans-serif;--fp:'Press Start 2P','Courier New',monospace;--fb:'Chakra Petch','Segoe UI',sans-serif;--gut:max(16px,env(safe-area-inset-left,0px));--gutr:max(16px,env(safe-area-inset-right,0px));color-scheme:dark}
*{box-sizing:border-box}
html,body{height:100%}
body{margin:0;background:var(--ink);color:var(--text);font:500 15px/1.4 var(--fb);overflow:hidden;-webkit-tap-highlight-color:transparent;touch-action:manipulation}
button,input,select,textarea{font:inherit;color:inherit}
:focus-visible{outline:3px solid var(--cyan);outline-offset:2px}
#app{height:100%;display:flex;flex-direction:column;position:relative}
#top{display:flex;align-items:center;gap:10px;padding:8px var(--gutr) 8px var(--gut);background:linear-gradient(#0d0820,#0d0820cc);border-bottom:1px solid var(--line);z-index:5;min-height:52px}
.brand{font-family:var(--fd);font-size:clamp(14px,3.4vw,22px);letter-spacing:.04em;background:linear-gradient(90deg,var(--pink),var(--yellow),var(--cyan));-webkit-background-clip:text;background-clip:text;color:transparent;white-space:nowrap;filter:drop-shadow(0 0 8px #ff2e9355)}
.brand small{display:block;font-family:var(--fp);font-size:7px;color:var(--mute);-webkit-text-fill-color:var(--mute);letter-spacing:.12em;margin-top:2px}
.sp{flex:1}
.chip{display:flex;align-items:center;gap:8px;background:var(--panel2);border:1px solid var(--line);border-radius:999px;padding:4px 12px 4px 4px;cursor:pointer;min-height:40px}
.chip.coin{padding:4px 12px;font-family:var(--fp);font-size:10px;color:var(--yellow);cursor:pointer}
.chip:hover{border-color:var(--cyan)}
.chip canvas{width:32px;height:32px;border-radius:50%}
.chip b{font-size:13px;max-width:110px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
#stage{flex:1;position:relative;min-height:0;overflow:hidden}
#scene{position:absolute;inset:0;width:100%;height:100%;display:block}
.mach{position:absolute;background:transparent;border:0;border-radius:14px;cursor:pointer;padding:0;transform-origin:50% 100%}
.mach span{position:absolute;left:50%;bottom:-2px;transform:translate(-50%,100%);font-family:var(--fp);font-size:8px;background:#0b0618e6;border:1px solid var(--line);border-radius:6px;padding:5px 7px;white-space:nowrap;display:none}
.mach:hover span,.mach:focus-visible span{display:block}
.mach:active{transform:scale(.985)}
#bubble{position:absolute;max-width:min(360px,calc(100% - 32px));min-width:120px;background:linear-gradient(180deg,#fff,#efe6ff);color:#1a0b3d;border-radius:20px;padding:12px 16px 12px;font-weight:600;font-size:15px;box-shadow:0 10px 30px #000a,0 0 0 3px #ff2e93,0 0 24px #ff2e9366;transform-origin:20% 100%;transform:scale(.6);opacity:0;pointer-events:none;transition:transform .28s cubic-bezier(.2,1.5,.4,1),opacity .2s;z-index:3;max-height:42%;overflow:auto}
#bubble.on{transform:scale(1);opacity:1;pointer-events:auto}
#bubble:after{content:"";position:absolute;left:var(--tail,24px);bottom:-12px;border:10px solid transparent;border-top-color:#efe6ff;border-bottom:0;filter:drop-shadow(0 3px 0 #ff2e93)}
#bubble .who{font-family:var(--fp);font-size:7px;color:#ff2e93;margin-bottom:6px;letter-spacing:.1em}
#bubble .dots i{display:inline-block;width:7px;height:7px;border-radius:50%;background:#ff2e93;margin-right:4px;animation:bn .9s infinite}
#bubble .dots i:nth-child(2){animation-delay:.15s}#bubble .dots i:nth-child(3){animation-delay:.3s}
@keyframes bn{50%{transform:translateY(-6px)}}
#bx{position:absolute;right:6px;top:4px;border:0;background:none;color:#7a5bb5;font-size:16px;cursor:pointer;line-height:1}
#cd{position:absolute;left:var(--gut);top:10px;z-index:2;background:#0b0618d9;border:1px solid var(--line);border-radius:10px;padding:7px 10px;cursor:pointer;text-align:left;font-family:var(--fp);font-size:7px;color:var(--mute);line-height:1.7}
#cd b{display:block;font-size:11px;color:var(--cyan);letter-spacing:.06em;font-variant-numeric:tabular-nums}
#cd:hover{border-color:var(--yellow)}
#guest{position:absolute;right:var(--gutr);top:10px;z-index:2;max-width:230px;background:#2a1448e6;border:1px solid var(--yellow);border-radius:10px;padding:7px 10px;font-size:12px;display:none}
#dock{background:linear-gradient(#0d0820cc,#0d0820);border-top:1px solid var(--line);padding:8px var(--gutr) calc(8px + env(safe-area-inset-bottom,0px)) var(--gut);z-index:5;display:grid;gap:8px}
#askf{display:flex;gap:8px}
#askin{flex:1;min-width:0;background:#0a0618;border:2px solid var(--line);border-radius:12px;padding:10px 14px;font-size:15px}
#askin:focus{border-color:var(--pink);outline:none;box-shadow:0 0 0 3px #ff2e9344}
#chips{display:flex;gap:6px;overflow-x:auto;scrollbar-width:none}
#chips button{flex:none;background:var(--panel2);border:1px solid var(--line);border-radius:999px;padding:5px 11px;font-size:12px;cursor:pointer;color:var(--mute)}
#chips button:hover{color:var(--text);border-color:var(--pink)}
#nav{display:grid;grid-template-columns:repeat(9,1fr);gap:4px}
.nb{display:flex;flex-direction:column;align-items:center;gap:3px;background:var(--panel2);border:1px solid var(--line);border-radius:10px;padding:7px 2px 6px;cursor:pointer;font-size:11px;font-weight:700;color:var(--mute);position:relative}
.nb svg{width:20px;height:20px}
.nb:hover{color:var(--text);border-color:var(--cyan);background:#251650}
.nb:active{transform:translateY(1px)}
.nb i{position:absolute;top:3px;right:8px;background:var(--pink);color:#fff;border-radius:99px;min-width:16px;height:16px;font-style:normal;font-size:10px;display:grid;place-items:center;padding:0 4px}
.btn{font-family:var(--fd);font-size:13px;letter-spacing:.03em;border:0;border-radius:11px;padding:10px 18px;cursor:pointer;color:#1a0b3d;background:linear-gradient(180deg,#fff27a,var(--yellow) 55%,#ffa11f);box-shadow:0 4px 0 #a35a00,0 8px 18px #0008;transition:transform .08s,box-shadow .08s,filter .15s;min-height:42px}
.btn:hover{filter:brightness(1.08)}
.btn:active{transform:translateY(3px);box-shadow:0 1px 0 #a35a00,0 3px 8px #0008}
.btn:disabled{filter:grayscale(1) brightness(.6);cursor:not-allowed;transform:none;box-shadow:0 2px 0 #333}
.btn.pink{background:linear-gradient(180deg,#ff9ccc,var(--pink) 55%,#c4106a);color:#fff;box-shadow:0 4px 0 #6d0a3a,0 8px 18px #0008}
.btn.cyan{background:linear-gradient(180deg,#aef6ff,var(--cyan) 55%,#0aa0c0);box-shadow:0 4px 0 #04566a,0 8px 18px #0008}
.btn.ghost{background:var(--panel2);color:var(--text);box-shadow:0 3px 0 #0c0618;border:1px solid var(--line)}
.btn.sm{font-size:11px;padding:7px 12px;min-height:34px;border-radius:9px}
#modalRoot{position:fixed;inset:0;pointer-events:none;z-index:20}
.modal{position:absolute;inset:0;pointer-events:auto;display:flex;align-items:stretch;justify-content:center;background:#0a0620d9;animation:fi .2s}
@keyframes fi{from{opacity:0}}
.mwin{width:min(980px,100%);margin:auto;max-height:100%;height:100%;display:flex;flex-direction:column;background:linear-gradient(180deg,#1a0f33,#100820);border:1px solid var(--line);box-shadow:0 0 60px #ff2e9333;animation:pop .3s cubic-bezier(.2,1.3,.4,1)}
@media(min-width:900px){.mwin{height:auto;max-height:92%;border-radius:18px}}
@keyframes pop{from{transform:translateY(24px) scale(.97);opacity:0}}
.mh{display:flex;align-items:center;gap:10px;padding:12px var(--gutr) 12px var(--gut);border-bottom:1px solid var(--line);padding-top:max(12px,env(safe-area-inset-top,0px))}
.mh h2{margin:0;font-family:var(--fd);font-size:clamp(16px,4vw,22px);font-weight:400;color:var(--cyan);text-shadow:0 0 12px #27e6ff77}
.mb{padding:16px var(--gutr) 24px var(--gut);overflow:auto;flex:1;min-height:0}
.x{background:var(--panel2);border:1px solid var(--line);border-radius:10px;width:40px;height:40px;cursor:pointer;font-size:20px;line-height:1}
.x:hover{border-color:var(--pink)}
.tabs{display:flex;gap:6px;margin-bottom:14px;overflow-x:auto;padding-bottom:2px}
.tabs button{flex:none;background:var(--panel2);border:1px solid var(--line);border-radius:999px;padding:7px 14px;cursor:pointer;font-weight:700;font-size:13px;color:var(--mute)}
.tabs button.on{background:var(--pink);border-color:var(--pink);color:#fff}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(210px,1fr));gap:14px}
.card{background:linear-gradient(180deg,#22134a,#150b2d);border:1px solid var(--line);border-radius:14px;overflow:hidden;display:flex;flex-direction:column;min-width:0;position:relative}
.card .th{aspect-ratio:16/10;width:100%;display:block;background:#0a0618}
.card .cb{padding:10px 12px 12px;display:grid;gap:6px;flex:1;align-content:start}
.card h3{margin:0;font-size:15px;font-weight:700}
.card p{margin:0;color:var(--mute);font-size:12.5px}
.tag{display:inline-block;font-family:var(--fp);font-size:7px;padding:4px 6px;border-radius:5px;background:#2b1a58;color:var(--cyan);letter-spacing:.05em}
.tag.ev{background:#ff8a1f;color:#240a00}.tag.soon{background:#2d6bff;color:#fff}.tag.own{background:#7dff4f;color:#052}.tag.code{background:#ff2e93;color:#fff}
.price{font-family:var(--fp);font-size:10px;color:var(--yellow)}
.price s{color:var(--mute);margin-right:6px}
.row{display:flex;align-items:center;gap:10px;flex-wrap:wrap}
.field{display:grid;gap:5px;margin-bottom:12px}
.field label{font-size:12px;color:var(--mute);font-weight:700;letter-spacing:.04em;text-transform:uppercase}
.field input,.field textarea,.field select{background:#0a0618;border:2px solid var(--line);border-radius:10px;padding:10px 12px;width:100%}
.field input:focus,.field textarea:focus,.field select:focus{border-color:var(--cyan);outline:none}
.msg{font-size:13px;min-height:18px}.msg.ok{color:var(--lime)}.msg.bad{color:#ff7a7a}
.sw{display:flex;align-items:center;justify-content:space-between;gap:12px;padding:10px 0;border-bottom:1px solid #2a1c4f}
.sw small{display:block;color:var(--mute);font-size:12px}
.tg{width:50px;height:28px;border-radius:99px;background:#2a1c4f;border:1px solid var(--line);position:relative;cursor:pointer;flex:none;transition:background .2s}
.tg:after{content:"";position:absolute;top:3px;left:3px;width:20px;height:20px;border-radius:50%;background:#fff;transition:transform .2s}
.tg[aria-checked=true]{background:var(--pink)}.tg[aria-checked=true]:after{transform:translateX(22px)}
input[type=range]{width:min(200px,45vw);accent-color:var(--pink)}
.sec{font-family:var(--fp);font-size:9px;color:var(--yellow);margin:18px 0 8px;letter-spacing:.1em}
.list{display:grid;gap:8px}
.li{display:flex;align-items:center;gap:12px;background:var(--panel2);border:1px solid var(--line);border-radius:12px;padding:8px 12px}
.li canvas{width:44px;height:44px;flex:none;border-radius:50%}
.li .t{flex:1;min-width:0}.li .t b{display:block;overflow:hidden;text-overflow:ellipsis}.li .t small{color:var(--mute)}
.on-dot{display:inline-block;width:8px;height:8px;border-radius:50%;background:var(--lime);box-shadow:0 0 8px var(--lime);margin-right:5px}
.off-dot{display:inline-block;width:8px;height:8px;border-radius:50%;background:#665a8a;margin-right:5px}
.story{background:var(--panel2);border:1px solid var(--line);border-radius:14px;padding:14px 16px;margin-bottom:12px}
.story h3{margin:0 0 6px;font-family:var(--fd);font-weight:400;color:var(--pink);font-size:16px}
.story p{margin:0;max-width:70ch}
.pg{display:grid;grid-template-columns:200px 1fr;gap:18px}
@media(max-width:640px){.pg{grid-template-columns:1fr}}
.stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(120px,1fr));gap:8px}
.stat{background:var(--panel2);border:1px solid var(--line);border-radius:10px;padding:8px 10px}.stat b{display:block;font-family:var(--fp);font-size:12px;color:var(--cyan);margin-top:4px}
.stat small{color:var(--mute);font-size:11px}
.badge{display:inline-flex;gap:6px;align-items:center;background:#2b1a58;border-radius:8px;padding:5px 9px;font-size:12px;margin:0 6px 6px 0}
.badge.lock{opacity:.4}
.titlepill{font-family:var(--fd);font-size:13px;padding:4px 12px;border-radius:99px;background:linear-gradient(90deg,#ffd23f,#ff8a1f);color:#2a1000;display:inline-block}
.titlepill.ceo{background:linear-gradient(90deg,#27e6ff,#ff2e93,#ffd23f);background-size:200% 100%;animation:sh 3s linear infinite;color:#12062e;box-shadow:0 0 16px #27e6ff88}
.titlepill.hw{background:linear-gradient(90deg,#9b5cff,#ff8a1f);color:#fff}
@keyframes sh{to{background-position:200% 0}}
.promo{position:absolute;right:var(--gutr);bottom:calc(100% + 8px);z-index:4}
#promo{position:absolute;left:50%;transform:translateX(-50%);bottom:10px;z-index:4;width:min(420px,calc(100% - 32px));background:linear-gradient(135deg,#2b0f55,#14082e);border:2px solid var(--orange);border-radius:14px;padding:12px 14px;box-shadow:0 0 30px #ff8a1f55;display:none;animation:pop .4s}
#promo h4{margin:0 0 4px;font-family:var(--fd);font-weight:400;color:var(--orange);font-size:16px}
#promo p{margin:0 0 8px;font-size:13px;color:#e6d9ff}
#toasts{position:fixed;left:50%;top:60px;transform:translateX(-50%);z-index:60;display:grid;gap:8px;pointer-events:none;width:min(420px,calc(100% - 32px))}
.toast{background:#0b0618f2;border:1px solid var(--cyan);border-radius:10px;padding:10px 14px;font-size:14px;animation:pop .25s;box-shadow:0 6px 24px #000a}
.toast.bad{border-color:var(--red)}.toast.ok{border-color:var(--lime)}
#intro{position:fixed;inset:0;z-index:80;background:#000;display:none}
#intro canvas{width:100%;height:100%;display:block}
#skip{position:absolute;right:var(--gutr);bottom:max(16px,env(safe-area-inset-bottom,0px));font-family:var(--fp);font-size:9px;background:#0008;color:#fff;border:1px solid #fff5;border-radius:8px;padding:10px 12px;cursor:pointer;display:none}
.gwrap{position:absolute;inset:0;display:flex;flex-direction:column;background:#04020a;z-index:2}
.ghud{display:flex;align-items:center;gap:12px;padding:8px var(--gutr) 8px var(--gut);background:#0d0820;border-bottom:1px solid var(--line);font-family:var(--fp);font-size:10px;padding-top:max(8px,env(safe-area-inset-top,0px))}
.ghud b{color:var(--yellow)}
.gbox{flex:1;min-height:0;position:relative;display:grid;place-items:center;padding:8px}
.gbox canvas{max-width:100%;max-height:100%;border:2px solid var(--line);border-radius:10px;background:#000;touch-action:none;box-shadow:0 0 40px #27e6ff33;image-rendering:auto}
.govl{position:absolute;inset:8px;display:grid;place-items:center;background:#05030bd9;border-radius:10px;text-align:center;padding:16px;overflow:auto}
.govl .in{max-width:420px;display:grid;gap:10px;justify-items:center}
.govl h3{margin:0;font-family:var(--fd);font-weight:400;font-size:26px;color:var(--yellow);text-shadow:0 0 14px #ffd23f88}
.govl p{margin:0;color:#d7c9ff}
.dpad{display:none;grid-template-columns:repeat(3,56px);grid-template-rows:repeat(2,50px);gap:6px;justify-content:center;padding:6px}
@media(pointer:coarse){.dpad.on{display:grid}}
.dpad button{background:var(--panel2);border:1px solid var(--line);border-radius:12px;font-size:20px;cursor:pointer;touch-action:none}
.dpad button:active{background:var(--pink)}
.redeem{position:fixed;inset:0;z-index:70;pointer-events:none}
.redeem canvas{width:100%;height:100%}
.crest{aspect-ratio:16/10;display:grid;place-items:center;font-family:var(--fd);font-size:20px;text-align:center;padding:8px}
.cdbig{font-family:var(--fp);font-size:clamp(14px,4vw,20px);color:var(--cyan);font-variant-numeric:tabular-nums;line-height:1.8}
.big-prev{width:100%;aspect-ratio:16/9;border-radius:14px;border:1px solid var(--line);display:block;background:#000}
@media(prefers-reduced-motion:reduce){*{animation-duration:.01ms!important;transition-duration:.01ms!important}}
@media(max-width:480px){#nav{gap:3px}.nb{font-size:7.5px;padding:6px 0 5px}.nb svg{width:18px;height:18px}.brand small{display:none}}
.nb i[hidden]{display:none}
.nb[data-k=pump]{color:var(--orange);border-color:#ff8a1f88;background:#2a1448}.nb[data-k=pump] svg{fill:#ff8a1f33}

/* ===== Tema Halloween: se aplica a TODA la interfaz cuando la interfaz Halloween está equipada ===== */
.gratis{position:absolute;right:10px;top:4px;width:92px;height:76px;z-index:3;pointer-events:none;animation:bob 1s ease-in-out infinite}
.gratis svg{width:100%;height:100%;filter:drop-shadow(0 0 10px #ff7a12)}
.gratis b{position:absolute;left:0;right:0;top:12px;text-align:center;font-family:var(--fd);font-size:17px;color:#fff;text-shadow:0 2px 0 #7a2c00}
.gratis-t{position:absolute;right:6px;top:80px;z-index:3;font-size:9px!important;background:#000b;border:1px solid #ff7a12;border-radius:6px;padding:3px 6px;pointer-events:none}
@keyframes bob{50%{transform:translateY(8px)}}
@keyframes shk{0%{transform:translate(0,0) rotate(0)}10%{transform:translate(-10px,6px) rotate(-.5deg)}20%{transform:translate(9px,-7px) rotate(.5deg)}30%{transform:translate(-8px,-6px) rotate(-.4deg)}40%{transform:translate(10px,7px) rotate(.4deg)}50%{transform:translate(-9px,3px) rotate(-.5deg)}60%{transform:translate(8px,-5px) rotate(.4deg)}70%{transform:translate(-10px,-4px) rotate(-.3deg)}80%{transform:translate(7px,6px) rotate(.5deg)}90%{transform:translate(-6px,-6px) rotate(-.4deg)}100%{transform:translate(0,0) rotate(0)}}
.shake{animation:shk .16s linear infinite}
.hwhero{position:absolute;left:16px;bottom:14px;pointer-events:none}
.hwhero b{display:block;font-family:var(--fd);font-weight:400;font-size:clamp(28px,6vw,54px);color:#ff9a3c;text-shadow:0 0 18px #ff7a12,0 4px 0 #3a1000;line-height:1}
.hwhero small{display:block;color:#e8d6ff;font-size:13px;margin-top:4px;text-shadow:0 2px 6px #000}
body.hw{--ink:#0a0418;--panel:#25104a;--panel2:#361766;--line:#9040e6;--pink:#ff8a1f;--cyan:#d2a2ff;--yellow:#ffc24d;--text:#fff6e8;--mute:#dcc6ff;--fd:'Creepster','Bungee','Impact',sans-serif}
body.hw #app::before,body.hw #app::after{content:"";position:absolute;top:52px;width:110px;height:110px;z-index:4;pointer-events:none;background:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Cg stroke='%23e6d8ff' stroke-opacity='.55' fill='none' stroke-width='1.3'%3E%3Cpath d='M0 0L100 100M0 0L100 38M0 0L38 100M0 0L100 70M0 0L70 100'/%3E%3Cpath d='M20 0Q18 18 0 20M40 0Q36 36 0 40M60 0Q54 54 0 60M80 0Q72 72 0 80'/%3E%3C/g%3E%3C/svg%3E") no-repeat;background-size:contain}
body.hw #app::before{left:0}
body.hw #app::after{right:0;transform:scaleX(-1)}
body.hw #top{background:linear-gradient(180deg,#2a0e4d,#12062a);border-bottom:2px solid #ff7a12;box-shadow:0 4px 22px #ff7a1255}
body.hw .brand{font-family:var(--fd);font-weight:400;letter-spacing:.06em;background:linear-gradient(90deg,#ff7a12,#ffd23f,#c08bff);-webkit-background-clip:text;background-clip:text;filter:drop-shadow(0 0 10px #ff7a1288)}
body.hw .brand small{color:#c9b0f0;-webkit-text-fill-color:#c9b0f0}
body.hw .chip{border:2px solid #ff7a12;background:linear-gradient(180deg,#34165f,#1a0a33);box-shadow:0 0 14px #ff7a1255;position:relative;overflow:visible}
body.hw .chip:not(.coin)::after{content:"";position:absolute;right:-6px;top:-12px;width:30px;height:18px;background:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 40 24'%3E%3Cpath d='M20 6l3-5 2 6c3-4 8-5 15-4-3 2-4 5-3 8-3-2-6-1-8 3-1-2-3-3-5-3-2 0-4 1-5 3-2-4-5-5-8-3 1-3 0-6-3-8 7-1 12 0 15 4l2-6z' fill='%2314061f' stroke='%239b5cff' stroke-width='1'/%3E%3C/svg%3E") no-repeat center/contain;animation:bob 1.4s ease-in-out infinite}
body.hw .chip.coin{color:#ffb13b}
body.hw #dock{background:linear-gradient(180deg,#1a0a33,#0a0316);border-top:2px solid #ff7a12;box-shadow:0 -4px 22px #ff7a1244}
body.hw .btn{font
