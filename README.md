<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>JARVIS – Prithivi's Personal AI Assistant</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;800;900&family=Exo+2:ital,wght@0,300;0,400;0,500;0,600;1,400&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
:root {
  --p:#a259ff; --p2:#7c3aed; --acc:#ff6d00; --acc2:#ffab00;
  --bg:#09010f; --bg2:#120420; --bg3:#1a0635;
  --txt:#e8d5ff; --dim:#7a5a9a; --dim2:#4a2d6a;
  --ok:#00ff99; --warn:#ffcc00; --err:#ff4455;
  --glow:0 0 18px rgba(162,89,255,0.45);
  --glow2:0 0 40px rgba(162,89,255,0.65),0 0 80px rgba(162,89,255,0.2);
  --r:10px;
}
*{margin:0;padding:0;box-sizing:border-box;}
html,body{height:100%;overflow:hidden;}
body{font-family:'Exo 2',sans-serif;background:var(--bg);color:var(--txt);display:flex;flex-direction:column;}

body::before{
  content:'';position:fixed;inset:0;
  background:
    radial-gradient(ellipse 70% 50% at 15% 40%,rgba(100,0,200,0.1) 0%,transparent 55%),
    radial-gradient(ellipse 50% 40% at 85% 60%,rgba(162,89,255,0.07) 0%,transparent 55%),
    repeating-linear-gradient(0deg,transparent,transparent 49px,rgba(162,89,255,0.02) 49px,rgba(162,89,255,0.02) 50px),
    repeating-linear-gradient(90deg,transparent,transparent 49px,rgba(162,89,255,0.02) 49px,rgba(162,89,255,0.02) 50px);
  pointer-events:none;z-index:0;
}

/* ── SETUP ── */
#setup{position:fixed;inset:0;z-index:999;background:var(--bg);display:flex;align-items:center;justify-content:center;}
.setup-card{background:linear-gradient(145deg,rgba(18,4,32,0.98),rgba(9,1,15,0.98));border:1px solid rgba(162,89,255,0.35);border-radius:18px;padding:44px 40px;width:92%;max-width:460px;text-align:center;box-shadow:var(--glow2);}
.setup-orb{width:88px;height:88px;border-radius:50%;background:radial-gradient(circle,rgba(162,89,255,0.3),rgba(100,0,200,0.15));border:2px solid var(--p);margin:0 auto 22px;display:flex;align-items:center;justify-content:center;font-size:38px;animation:orbPulse 3s ease-in-out infinite;box-shadow:var(--glow2);}
@keyframes orbPulse{0%,100%{box-shadow:var(--glow2);}50%{box-shadow:0 0 60px rgba(162,89,255,0.9),0 0 120px rgba(162,89,255,0.35);}}
.setup-card h2{font-family:'Orbitron',monospace;font-size:20px;font-weight:900;letter-spacing:5px;color:var(--p);text-shadow:var(--glow);margin-bottom:6px;}
.setup-card p{color:var(--dim);font-size:12px;line-height:1.7;margin-bottom:22px;}
.setup-card p a{color:var(--p);text-decoration:none;}
.inp-wrap{position:relative;margin-bottom:12px;}
.inp-wrap input{width:100%;padding:13px 44px 13px 16px;background:rgba(162,89,255,0.07);border:1px solid rgba(162,89,255,0.25);border-radius:var(--r);color:var(--txt);font-size:12.5px;font-family:'Share Tech Mono',monospace;outline:none;transition:border-color .2s;}
.inp-wrap input:focus{border-color:var(--p);box-shadow:0 0 14px rgba(162,89,255,0.15);}
.inp-wrap input::placeholder{color:var(--dim);}
.eye{position:absolute;right:13px;top:50%;transform:translateY(-50%);background:none;border:none;cursor:pointer;font-size:15px;}
.launch{width:100%;padding:13px;background:linear-gradient(135deg,var(--p2),var(--p));border:none;border-radius:var(--r);cursor:pointer;color:#fff;font-family:'Orbitron',monospace;font-size:12px;letter-spacing:3px;font-weight:800;transition:all .2s;box-shadow:0 4px 18px rgba(162,89,255,0.45);margin-bottom:14px;}
.launch:hover:not(:disabled){transform:scale(1.02);box-shadow:0 6px 28px rgba(162,89,255,0.65);}
.launch:disabled{opacity:.35;cursor:not-allowed;}
.setup-note{font-size:10px;color:var(--dim);font-family:'Share Tech Mono',monospace;line-height:1.9;}
.setup-note b{color:var(--ok);}

/* ── HEADER ── */
header{position:relative;z-index:10;padding:12px 28px;display:flex;align-items:center;justify-content:space-between;border-bottom:1px solid rgba(162,89,255,0.15);background:rgba(9,1,15,0.92);backdrop-filter:blur(14px);flex-shrink:0;}
.logo{display:flex;align-items:center;gap:12px;}
.logo-orb{width:42px;height:42px;border-radius:50%;border:2px solid var(--p);display:flex;align-items:center;justify-content:center;font-size:18px;box-shadow:var(--glow);animation:spin 10s linear infinite;position:relative;}
.logo-orb::before{content:'';position:absolute;inset:-5px;border-radius:50%;border:1px dashed rgba(162,89,255,0.3);animation:spin 5s linear infinite reverse;}
@keyframes spin{to{transform:rotate(360deg);}}
.logo-text h1{font-family:'Orbitron',monospace;font-size:18px;font-weight:900;letter-spacing:6px;color:var(--p);text-shadow:var(--glow);}
.logo-text p{font-size:9px;letter-spacing:3px;color:var(--dim);font-family:'Share Tech Mono',monospace;}
.hdr-r{display:flex;gap:10px;align-items:center;}
.badge{display:flex;align-items:center;gap:6px;font-size:9.5px;font-family:'Share Tech Mono',monospace;}
.dot{width:6px;height:6px;border-radius:50%;animation:dotPulse 2s infinite;}
.dot.g{background:var(--ok);box-shadow:0 0 6px var(--ok);}
.dot.b{background:var(--p);box-shadow:0 0 6px var(--p);}
@keyframes dotPulse{0%,100%{opacity:1;}50%{opacity:.4;}}
.hdr-btn{padding:5px 12px;border-radius:6px;background:rgba(162,89,255,0.08);border:1px solid rgba(162,89,255,0.2);color:var(--dim);font-size:10px;cursor:pointer;font-family:'Share Tech Mono',monospace;transition:all .2s;}
.hdr-btn:hover{border-color:var(--p);color:var(--p);}

/* ── LAYOUT ── */
.app{position:relative;z-index:1;display:grid;grid-template-columns:260px 1fr 244px;flex:1;overflow:hidden;}

/* ── SIDEBAR ── */
.sidebar{background:rgba(12,2,22,0.7);border-right:1px solid rgba(162,89,255,0.12);padding:14px;display:flex;flex-direction:column;gap:9px;overflow-y:auto;backdrop-filter:blur(8px);}
.sidebar::-webkit-scrollbar{width:3px;}
.sidebar::-webkit-scrollbar-thumb{background:rgba(162,89,255,0.18);border-radius:2px;}
.sec-title{font-family:'Orbitron',monospace;font-size:8.5px;letter-spacing:4px;color:var(--dim);text-transform:uppercase;padding-bottom:6px;border-bottom:1px solid rgba(162,89,255,0.1);}
.mb{display:flex;align-items:center;gap:9px;padding:8px 12px;background:rgba(162,89,255,0.04);border:1px solid rgba(162,89,255,0.09);border-radius:8px;cursor:pointer;transition:all .2s;color:var(--dim);font-family:'Exo 2',sans-serif;width:100%;text-align:left;}
.mb:hover,.mb.on{background:rgba(162,89,255,0.12);border-color:var(--p);color:var(--p);box-shadow:inset 0 0 16px rgba(162,89,255,0.05),var(--glow);}
.mb .ic{width:26px;height:26px;border-radius:5px;display:flex;align-items:center;justify-content:center;background:rgba(162,89,255,0.08);font-size:13px;flex-shrink:0;}
.mb.on .ic{background:rgba(162,89,255,0.2);}
.mb .ml{font-size:11.5px;font-weight:500;}
.mb .ms{font-size:9.5px;color:var(--dim);margin-top:1px;}
.hist-item{padding:7px 9px;background:rgba(162,89,255,0.03);border:1px solid rgba(162,89,255,0.08);border-radius:6px;font-size:9.5px;color:var(--dim);cursor:pointer;transition:all .2s;font-family:'Share Tech Mono',monospace;margin-bottom:3px;}
.hist-item:hover{background:rgba(162,89,255,0.09);color:var(--txt);}

/* ── CENTER ── */
.center{display:flex;flex-direction:column;overflow:hidden;}
.chat{flex:1;overflow-y:auto;padding:18px 24px;display:flex;flex-direction:column;gap:14px;scrollbar-width:thin;scrollbar-color:rgba(162,89,255,0.2) transparent;}
.chat::-webkit-scrollbar{width:4px;}
.chat::-webkit-scrollbar-thumb{background:rgba(162,89,255,0.2);border-radius:2px;}

/* Welcome */
.welcome{display:flex;flex-direction:column;align-items:center;justify-content:center;flex:1;text-align:center;padding:24px;}
.w-orb{width:120px;height:120px;position:relative;display:flex;align-items:center;justify-content:center;margin-bottom:22px;}
.w-orb::before,.w-orb::after{content:'';position:absolute;border-radius:50%;border:1px solid rgba(162,89,255,0.3);animation:ringExp 3s ease-out infinite;}
.w-orb::before{inset:0;animation-delay:0s;}
.w-orb::after{inset:-18px;animation-delay:1.5s;}
@keyframes ringExp{0%{opacity:.8;transform:scale(.9);}100%{opacity:0;transform:scale(1.35);}}
.w-core{width:92px;height:92px;border-radius:50%;background:radial-gradient(circle,rgba(162,89,255,0.28) 0%,rgba(100,0,200,0.15) 50%,transparent 70%);border:2px solid var(--p);box-shadow:var(--glow2);display:flex;align-items:center;justify-content:center;font-size:36px;animation:orbPulse 3s ease-in-out infinite;}
.welcome h2{font-family:'Orbitron',monospace;font-size:22px;font-weight:900;color:var(--p);text-shadow:var(--glow);letter-spacing:4px;margin-bottom:7px;}
.welcome p{color:var(--dim);font-size:12px;max-width:420px;line-height:1.75;margin-bottom:22px;}
.qgrid{display:grid;grid-template-columns:repeat(3,1fr);gap:7px;width:100%;max-width:560px;}
.qbtn{padding:9px 10px;background:rgba(162,89,255,0.04);border:1px solid rgba(162,89,255,0.12);border-radius:8px;cursor:pointer;font-size:10.5px;color:var(--dim);text-align:left;transition:all .2s;font-family:'Exo 2',sans-serif;}
.qbtn:hover{background:rgba(162,89,255,0.12);border-color:var(--p);color:var(--txt);}
.qbtn span{display:block;font-size:18px;margin-bottom:3px;}

/* Messages */
.mrow{display:flex;gap:10px;align-items:flex-start;animation:fadeUp .3s ease;}
@keyframes fadeUp{from{opacity:0;transform:translateY(9px);}to{opacity:1;transform:translateY(0);}}
.mrow.user{flex-direction:row-reverse;}
.av{width:30px;height:30px;border-radius:50%;flex-shrink:0;display:flex;align-items:center;justify-content:center;font-size:13px;}
.av.ai{background:radial-gradient(circle,rgba(162,89,255,0.32),rgba(100,0,200,0.2));border:1px solid rgba(162,89,255,0.4);box-shadow:0 0 9px rgba(162,89,255,0.3);}
.av.user{background:rgba(255,109,0,0.18);border:1px solid rgba(255,109,0,0.38);}
.bub{max-width:74%;padding:11px 14px;border-radius:11px;font-size:12.5px;line-height:1.78;}
.mrow.ai .bub{background:rgba(162,89,255,0.07);border:1px solid rgba(162,89,255,0.15);border-top-left-radius:2px;}
.mrow.user .bub{background:rgba(255,109,0,0.07);border:1px solid rgba(255,109,0,0.18);border-top-right-radius:2px;color:#ffd5aa;}
.mmeta{font-size:9px;color:var(--dim);font-family:'Share Tech Mono',monospace;margin-top:4px;display:flex;gap:8px;align-items:center;flex-wrap:wrap;}
.mrow.user .mmeta{justify-content:flex-end;}
.tag{padding:1px 7px;border-radius:3px;background:rgba(162,89,255,0.12);border:1px solid rgba(162,89,255,0.25);color:var(--p);font-size:8px;letter-spacing:1px;}
.ibtn{background:none;border:none;cursor:pointer;color:var(--dim);font-size:11px;padding:0 2px;transition:color .2s;font-family:'Share Tech Mono',monospace;}
.ibtn:hover{color:var(--p);}

/* Typing */
.typing{display:flex;gap:4px;align-items:center;padding:3px 0;}
.td{width:5px;height:5px;border-radius:50%;background:var(--p);animation:tdBounce 1.2s ease-in-out infinite;}
.td:nth-child(2){animation-delay:.2s;} .td:nth-child(3){animation-delay:.4s;}
@keyframes tdBounce{0%,60%,100%{transform:translateY(0);opacity:.4;}30%{transform:translateY(-7px);opacity:1;}}

/* Code */
.cb{background:rgba(0,0,0,0.55);border:1px solid rgba(162,89,255,0.18);border-radius:8px;padding:12px;margin-top:8px;font-family:'Share Tech Mono',monospace;font-size:11px;color:#c4a0ff;white-space:pre-wrap;line-height:1.6;position:relative;}
.cl{position:absolute;top:7px;right:9px;font-size:8px;color:var(--dim);letter-spacing:1px;}

/* Special cards */
.card{background:rgba(162,89,255,0.05);border:1px solid rgba(162,89,255,0.14);border-radius:10px;padding:13px;margin-top:9px;}
.card-title{font-family:'Orbitron',monospace;font-size:10px;letter-spacing:2px;color:var(--p);margin-bottom:10px;}
.music-item{display:flex;align-items:center;gap:10px;padding:8px;background:rgba(162,89,255,0.04);border-radius:7px;margin-bottom:6px;cursor:pointer;transition:all .2s;border:1px solid rgba(162,89,255,0.09);}
.music-item:hover{background:rgba(162,89,255,0.1);border-color:var(--p);}
.music-thumb{width:44px;height:44px;border-radius:6px;object-fit:cover;background:rgba(162,89,255,0.12);flex-shrink:0;display:flex;align-items:center;justify-content:center;font-size:20px;}
.music-info .mt{font-size:12px;font-weight:500;color:var(--txt);}
.music-info .ma{font-size:10px;color:var(--dim);margin-top:2px;}
.play-btn{margin-left:auto;width:32px;height:32px;border-radius:50%;background:linear-gradient(135deg,var(--p2),var(--p));border:none;cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:12px;flex-shrink:0;transition:all .2s;}
.play-btn:hover{transform:scale(1.1);box-shadow:0 0 12px rgba(162,89,255,0.55);}

/* Action card */
.action-card{background:rgba(162,89,255,0.06);border:1px solid rgba(162,89,255,0.2);border-radius:10px;padding:14px;margin-top:9px;}
.action-card .ac-title{font-family:'Orbitron',monospace;font-size:10px;letter-spacing:2px;color:var(--p);margin-bottom:10px;}
.action-btn{display:inline-block;padding:8px 18px;background:linear-gradient(135deg,var(--p2),var(--p));border:none;border-radius:8px;color:#fff;font-size:11px;cursor:pointer;font-family:'Orbitron',monospace;letter-spacing:1px;text-decoration:none;transition:all .2s;margin-right:7px;margin-top:5px;}
.action-btn:hover{transform:scale(1.04);box-shadow:0 4px 14px rgba(162,89,255,0.5);}
.action-btn.sec{background:rgba(162,89,255,0.12);border:1px solid rgba(162,89,255,0.3);color:var(--p);}

/* Dialog overlay */
#dialogOverlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.75);z-index:800;align-items:center;justify-content:center;}
#dialogOverlay.show{display:flex;}
.dialog-box{background:linear-gradient(145deg,rgba(18,4,32,0.99),rgba(9,1,15,0.99));border:1px solid rgba(162,89,255,0.4);border-radius:14px;padding:28px 28px 22px;width:92%;max-width:420px;text-align:center;box-shadow:var(--glow2);}
.dialog-box h3{font-family:'Orbitron',monospace;font-size:14px;color:var(--p);margin-bottom:8px;letter-spacing:2px;}
.dialog-box p{font-size:12px;color:var(--dim);margin-bottom:16px;line-height:1.6;}
.dialog-inp{width:100%;padding:11px 14px;background:rgba(162,89,255,0.07);border:1px solid rgba(162,89,255,0.25);border-radius:8px;color:var(--txt);font-size:12.5px;font-family:'Share Tech Mono',monospace;outline:none;margin-bottom:12px;}
.dialog-inp:focus{border-color:var(--p);}
.dialog-btns{display:flex;gap:8px;justify-content:center;}
.dbtn{padding:9px 22px;border-radius:8px;border:none;cursor:pointer;font-size:11px;font-family:'Orbitron',monospace;letter-spacing:1px;transition:all .2s;}
.dbtn.ok{background:linear-gradient(135deg,var(--p2),var(--p));color:#fff;}
.dbtn.cancel{background:rgba(162,89,255,0.1);border:1px solid rgba(162,89,255,0.28);color:var(--p);}
.dbtn:hover{transform:scale(1.04);}

/* Joke card */
.joke-card{background:rgba(255,171,0,0.07);border:1px solid rgba(255,171,0,0.22);border-radius:10px;padding:14px;margin-top:8px;}
.joke-card p{font-size:13px;line-height:1.7;color:#ffe0a0;font-style:italic;}

/* Input area */
.inp-area{padding:13px 24px;border-top:1px solid rgba(162,89,255,0.1);background:rgba(9,1,15,0.85);backdrop-filter:blur(14px);flex-shrink:0;}
.toolbar{display:flex;gap:5px;margin-bottom:8px;align-items:center;flex-wrap:wrap;}
.pill{padding:3px 10px;border-radius:20px;border:1px solid rgba(162,89,255,0.15);background:rgba(162,89,255,0.05);color:var(--dim);font-size:10px;cursor:pointer;font-family:'Exo 2',sans-serif;transition:all .2s;}
.pill:hover,.pill.on{border-color:var(--p);background:rgba(162,89,255,0.12);color:var(--p);}
.pill.son{border-color:var(--acc);background:rgba(255,109,0,0.09);color:var(--acc);}
#mTag{margin-left:auto;font-size:9px;color:var(--dim);font-family:'Share Tech Mono',monospace;}
.irow{display:flex;gap:8px;align-items:flex-end;}
.ibox{flex:1;background:rgba(162,89,255,0.05);border:1px solid rgba(162,89,255,0.2);border-radius:11px;padding:11px 14px;color:var(--txt);font-size:13px;font-family:'Exo 2',sans-serif;resize:none;outline:none;min-height:46px;max-height:120px;transition:border-color .2s,box-shadow .2s;line-height:1.5;}
.ibox:focus{border-color:var(--p);box-shadow:0 0 14px rgba(162,89,255,0.1);}
.ibox::placeholder{color:var(--dim);}
.sbtn{width:46px;height:46px;border-radius:11px;flex-shrink:0;background:linear-gradient(135deg,var(--p2),var(--p));border:none;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:all .2s;box-shadow:0 4px 14px rgba(162,89,255,0.35);}
.sbtn:hover{transform:scale(1.06);box-shadow:0 6px 24px rgba(162,89,255,0.55);}
.sbtn:active{transform:scale(.95);}
.sbtn svg{color:#fff;}
.vbtn{width:46px;height:46px;border-radius:11px;flex-shrink:0;background:rgba(255,109,0,0.09);border:1px solid rgba(255,109,0,0.28);cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:18px;transition:all .2s;}
.vbtn:hover{background:rgba(255,109,0,0.18);box-shadow:0 0 14px rgba(255,109,0,0.3);}
.vbtn.rec{background:rgba(255,68,85,0.18);border-color:var(--err);animation:recPulse 1s infinite;}
@keyframes recPulse{0%,100%{box-shadow:0 0 0 0 rgba(255,68,85,.4);}50%{box-shadow:0 0 0 7px rgba(255,68,85,0);}}
#imgBar{margin-bottom:6px;position:relative;display:inline-block;}
#imgPrev{height:50px;border-radius:6px;border:1px solid rgba(162,89,255,0.3);display:block;}
.rmImg{position:absolute;top:-5px;left:46px;background:rgba(255,68,85,.9);border:none;color:#fff;border-radius:50%;width:15px;height:15px;font-size:9px;cursor:pointer;line-height:15px;text-align:center;}

/* Right panel */
.rp{background:rgba(12,2,22,0.7);border-left:1px solid rgba(162,89,255,0.12);padding:14px;display:flex;flex-direction:column;gap:12px;overflow-y:auto;backdrop-filter:blur(8px);}
.rp::-webkit-scrollbar{width:3px;}
.rp::-webkit-scrollbar-thumb{background:rgba(162,89,255,0.16);border-radius:2px;}
.scard{background:rgba(162,89,255,0.04);border:1px solid rgba(162,89,255,0.1);border-radius:9px;padding:11px;}
.sv{font-family:'Orbitron',monospace;font-size:19px;font-weight:700;color:var(--p);}
.ss{font-size:9.5px;color:var(--dim);margin-top:2px;}
.pb{height:3px;background:rgba(162,89,255,0.1);border-radius:2px;margin-top:6px;overflow:hidden;}
.pf{height:100%;border-radius:2px;background:linear-gradient(90deg,var(--p2),var(--p));transition:width 1s ease;}
.ci{display:flex;align-items:center;gap:7px;padding:5px 0;border-bottom:1px solid rgba(162,89,255,0.06);font-size:10.5px;color:var(--dim);}
.cd{width:5px;height:5px;border-radius:50%;flex-shrink:0;}
.cd.on{background:var(--ok);box-shadow:0 0 5px var(--ok);}
.tip{font-size:9.5px;color:var(--dim);line-height:1.75;font-family:'Share Tech Mono',monospace;padding:8px;background:rgba(162,89,255,0.03);border:1px solid rgba(162,89,255,0.08);border-radius:7px;}
.uz{border:1px dashed rgba(162,89,255,0.25);border-radius:9px;padding:13px;text-align:center;cursor:pointer;transition:all .2s;background:rgba(162,89,255,0.02);}
.uz:hover{border-color:var(--p);background:rgba(162,89,255,0.07);}
.uz p{font-size:10px;color:var(--dim);margin-top:5px;}

/* Toast */
.toast{position:fixed;bottom:22px;right:22px;padding:9px 15px;background:rgba(162,89,255,0.15);border:1px solid var(--p);border-radius:8px;color:var(--p);font-size:11px;z-index:600;animation:tIn .3s ease,tOut .3s ease 2.7s forwards;box-shadow:var(--glow);font-family:'Share Tech Mono',monospace;}
@keyframes tIn{from{transform:translateX(90px);opacity:0;}to{transform:translateX(0);opacity:1;}}
@keyframes tOut{to{opacity:0;transform:translateX(90px);}}
.chat-img{max-width:230px;border-radius:7px;border:1px solid rgba(162,89,255,0.2);margin-top:6px;display:block;}
@media(max-width:1100px){.app{grid-template-columns:230px 1fr;}.rp{display:none;}}
@media(max-width:700px){.app{grid-template-columns:1fr;}.sidebar{display:none;}}
</style>
</head>
<body>

<!-- DIALOG OVERLAY -->
<div id="dialogOverlay">
  <div class="dialog-box">
    <h3 id="dlgTitle">INPUT REQUIRED</h3>
    <p id="dlgMsg">Please provide the following:</p>
    <input class="dialog-inp" id="dlgInp" placeholder="Type here..." onkeydown="if(event.key==='Enter')dlgOK()">
    <div class="dialog-btns">
      <button class="dbtn ok" onclick="dlgOK()">✔ CONFIRM</button>
      <button class="dbtn cancel" onclick="dlgCancel()">✖ CANCEL</button>
    </div>
  </div>
</div>

<!-- SETUP -->
<div id="setup">
  <div class="setup-card">
    <div class="setup-orb">🤖</div>
    <h2>JARVIS</h2>
    <p>Enter your free <b style="color:var(--p)">Groq API key</b> to activate, Prithivi.<br>
      Get free key: <a href="https://console.groq.com" target="_blank">console.groq.com</a> → API Keys → Create → <b style="color:var(--ok)">No Expiration</b>
    </p>
    <div class="inp-wrap">
      <input type="password" id="keyInp" placeholder="gsk_xxxxxxxxxxxxxxxxxxxxxxxx" oninput="onKI()">
      <button class="eye" onclick="toggleEye()">👁️</button>
    </div>
    <button class="launch" id="lBtn" onclick="launch()" disabled>⚡ ACTIVATE JARVIS</button>
    <div class="setup-note">
      <b>✔</b> Groq is 100% free forever — no credit card ever<br>
      <b>✔</b> Key saved in your browser only — never shared<br>
      <b>✔</b> Run via: python -m http.server 8080 → localhost:8080
    </div>
  </div>
</div>

<!-- HEADER -->
<header>
  <div class="logo">
    <div class="logo-orb">🌟</div>
    <div class="logo-text"><h1>JARVIS</h1><p>Prithivi's Personal AI Assistant — Free Forever</p></div>
  </div>
  <div class="hdr-r">
    <div class="badge"><div class="dot g"></div><span style="color:var(--ok)">ONLINE</span></div>
    <div class="badge"><div class="dot b"></div><span style="color:var(--p)">LLAMA 3 · FREE</span></div>
    <button class="hdr-btn" onclick="changeKey()">🔑 Key</button>
    <button class="hdr-btn" onclick="clearChat()">🗑️ Clear</button>
  </div>
</header>

<!-- APP -->
<div class="app">
  <!-- SIDEBAR -->
  <div class="sidebar">
    <div class="sec-title">⚡ What can I do?</div>
    <button class="mb on" onclick="setMode('chat',this)"><div class="ic">💬</div><div><div class="ml">Chat & Talk</div><div class="ms">Friendly conversation</div></div></button>
    <button class="mb" onclick="setMode('code',this)"><div class="ic">💻</div><div><div class="ml">Write Code</div><div class="ms">Any language</div></div></button>
    <button class="mb" onclick="setMode('cooking',this)"><div class="ic">🍳</div><div><div class="ml">Cooking</div><div class="ms">Recipes & tips</div></div></button>
    <button class="mb" onclick="setMode('music',this)"><div class="ic">🎵</div><div><div class="ml">Music</div><div class="ms">Songs & lyrics</div></div></button>
    <button class="mb" onclick="setMode('health',this)"><div class="ic">💊</div><div><div class="ml">Health & Fitness</div><div class="ms">Tips & advice</div></div></button>
    <button class="mb" onclick="setMode('study',this)"><div class="ic">📚</div><div><div class="ml">Study Help</div><div class="ms">Learn anything</div></div></button>
    <button class="mb" onclick="setMode('travel',this)"><div class="ic">✈️</div><div><div class="ml">Travel</div><div class="ms">Plans & guides</div></div></button>
    <button class="mb" onclick="setMode('finance',this)"><div class="ic">💰</div><div><div class="ml">Finance</div><div class="ms">Money & budgeting</div></div></button>
    <button class="mb" onclick="setMode('fun',this)"><div class="ic">😄</div><div><div class="ml">Fun & Jokes</div><div class="ms">Entertain me</div></div></button>
    <button class="mb" onclick="setMode('image',this)"><div class="ic">🖼️</div><div><div class="ml">Image Analysis</div><div class="ms">Upload & describe</div></div></button>
    <button class="mb" onclick="setMode('email',this)"><div class="ic">✉️</div><div><div class="ml">Write Email</div><div class="ms">Professional drafts</div></div></button>
    <button class="mb" onclick="setMode('research',this)"><div class="ic">🔬</div><div><div class="ml">Research</div><div class="ms">Deep analysis</div></div></button>
    <div class="sec-title" style="margin-top:4px">🗂 Recent</div>
    <div id="histList"><div style="font-size:9.5px;color:var(--dim);font-family:'Share Tech Mono',monospace">No history yet...</div></div>
  </div>

  <!-- CENTER -->
  <div class="center">
    <div class="chat" id="chat">
      <div class="welcome" id="ws">
        <div class="w-orb"><div class="w-core">🤖</div></div>
        <h2>JARVIS READY</h2>
        <p>Welcome back, Prithivi! I'm your personal AI assistant. Ask me anything, or try a quick action below!</p>
        <div class="qgrid">
          <div class="qbtn" onclick="sq('Tell me a funny joke')"><span>😄</span>Tell a Joke</div>
          <div class="qbtn" onclick="sq('Give me a simple recipe for pasta carbonara with steps')"><span>🍳</span>Cook Pasta</div>
          <div class="qbtn" onclick="sq('Recommend 5 popular songs right now across different genres')"><span>🎵</span>Song Recs</div>
          <div class="qbtn" onclick="sq('Give me a 7-day morning workout plan for beginners at home')"><span>💪</span>Workout Plan</div>
          <div class="qbtn" onclick="sq('open website youtube')"><span>🌐</span>Open YouTube</div>
          <div class="qbtn" onclick="sq('compose email to my boss subject meeting today')"><span>✉️</span>Compose Email</div>
          <div class="qbtn" onclick="sq('send whatsapp message to my friend')"><span>💬</span>WhatsApp Msg</div>
          <div class="qbtn" onclick="sq('Write a Python function to sort a list of numbers')"><span>💻</span>Write Code</div>
          <div class="qbtn" onclick="sq('Give me daily health tips for a healthy lifestyle')"><span>🌿</span>Health Tips</div>
        </div>
      </div>
    </div>

    <!-- INPUT -->
    <div class="inp-area">
      <div class="toolbar">
        <div class="pill on" onclick="selPill(this)">💬 Chat</div>
        <div class="pill" onclick="selPill(this)">💻 Code</div>
        <div class="pill" onclick="selPill(this)">🍳 Cook</div>
        <div class="pill" onclick="selPill(this)">🎵 Music</div>
        <div class="pill" onclick="trigFile()">📎 File</div>
        <div class="pill" onclick="trigImg()">🖼️ Image</div>
        <div class="pill" id="ttsP" onclick="togTTS()">🔊 Speak</div>
        <span id="mTag">MODE: CHAT</span>
      </div>
      <input type="file" id="fInp" style="display:none" accept=".txt,.csv,.json,.md,.pdf" onchange="handleFile(this)">
      <input type="file" id="iInp" style="display:none" accept="image/*" onchange="handleImg(this)">
      <div id="imgBar" style="display:none"><img id="imgPrev" alt=""><button class="rmImg" onclick="clearImg()">×</button></div>
      <div class="irow">
        <button class="vbtn" id="vBtn" onclick="togVoice()" title="Speak — auto sends">🎙️</button>
        <textarea class="ibox" id="ibox" rows="1"
          placeholder="Prithivi, ask me anything... open website, compose email, WhatsApp, code, health..."
          onkeydown="onKey(event)" oninput="autoR(this)"></textarea>
        <button class="sbtn" onclick="send()">
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
            <line x1="22" y1="2" x2="11" y2="13"/>
            <polygon points="22 2 15 22 11 13 2 9 22 2"/>
          </svg>
        </button>
      </div>
    </div>
  </div>

  <!-- RIGHT PANEL -->
  <div class="rp">
    <div class="sec-title">📡 Status</div>
    <div class="scard"><div class="s-title" style="font-size:8px;letter-spacing:2px;color:var(--dim);text-transform:uppercase;margin-bottom:4px;font-family:'Share Tech Mono',monospace">Messages</div><div class="sv" id="mc">0</div><div class="ss">This session</div><div class="pb"><div class="pf" id="mp" style="width:0%"></div></div></div>
    <div class="scard"><div class="s-title" style="font-size:8px;letter-spacing:2px;color:var(--dim);text-transform:uppercase;margin-bottom:4px;font-family:'Share Tech Mono',monospace">Mode</div><div class="sv" style="font-size:12px;letter-spacing:1px" id="mDisp">CHAT</div><div class="ss">Free forever</div></div>
    <div class="sec-title">🔧 Services</div>
    <div class="ci"><div class="cd on"></div>AI Brain (Groq · Free)</div>
    <div class="ci"><div class="cd on"></div>Voice Input</div>
    <div class="ci"><div class="cd on"></div>Text to Speech</div>
    <div class="ci"><div class="cd on"></div>Image Analysis</div>
    <div class="ci"><div class="cd on"></div>Music Search (iTunes)</div>
    <div class="ci"><div class="cd on"></div>Open Websites</div>
    <div class="ci"><div class="cd on"></div>Gmail Compose</div>
    <div class="ci"><div class="cd on"></div>WhatsApp Messaging</div>
    <div class="ci"><div class="cd on"></div>File Processing</div>
    <div class="sec-title">💡 Tip</div>
    <div class="tip" id="tipEl">🎙️ Say "open youtube" — I'll open it instantly, Prithivi!</div>
    <div class="sec-title">📎 Upload</div>
    <div class="uz" onclick="trigImg()"><div style="font-size:24px">🖼️</div><p>Upload image to analyze</p></div>
    <div class="sec-title">🗂 History</div>
    <div id="histR"><div style="font-size:9.5px;color:var(--dim);font-family:'Share Tech Mono',monospace">No history yet...</div></div>
  </div>
</div>

<script>
// ═══ STATE ═══
let KEY = localStorage.getItem('sj_key') || '';
let mode = 'chat';
let msgs = [];
let mc = 0;
let recog = null;
let isRec = false;
let tts = false;
let hist = [];
let pendImg = null, pendImgT = 'image/jpeg';
let dlgResolve = null, dlgReject = null;

const TIPS = [
  '🎙️ Say "open youtube" — I\'ll open it, Prithivi!',
  '✉️ Say "compose email to [name] about [topic]"',
  '💬 Say "send whatsapp to [friend] saying [message]"',
  '🍳 Say "recipe for biryani" and get step-by-step',
  '🎵 Say "recommend songs like Arijit Singh"',
  '💊 Say "daily diet plan for weight loss"',
  '✈️ Say "plan 3-day trip to Goa"',
  '😄 Say "tell me a joke" for instant fun',
  '💻 Say "write Python code for calculator"',
  '📚 Say "explain photosynthesis simply"',
];
let ti = 0;
setInterval(() => { ti=(ti+1)%TIPS.length; const e=document.getElementById('tipEl'); if(e)e.textContent=TIPS[ti]; }, 5500);

const SYS = {
  chat: `You are JARVIS, a warm, friendly, intelligent personal AI assistant for Prithivi. You always address him as "Prithivi". You talk like a helpful friend — casual, clear, and caring. Help with any daily life question. Be conversational, use emojis occasionally, and be genuinely helpful.`,
  code: `You are JARVIS, an expert programmer and personal assistant to Prithivi. Write clean, working, well-commented code. Always use code blocks with language tags. Explain briefly before and after.`,
  cooking: `You are JARVIS, a friendly expert chef and assistant to Prithivi. Give detailed recipes with: Ingredients list, Step-by-step instructions numbered clearly, Cooking time, Tips and variations. Be encouraging and practical.`,
  music: `You are JARVIS, a music expert and assistant to Prithivi. Recommend songs, explain lyrics, suggest playlists, discuss artists. Be enthusiastic and knowledgeable. Format recommendations as numbered lists with artist name and why to listen.`,
  health: `You are JARVIS, a wellness advisor and assistant to Prithivi. Give practical health, fitness, diet, and mental wellbeing advice. Be encouraging, safe, and practical. Always recommend consulting a doctor for medical issues.`,
  study: `You are JARVIS, an expert tutor and assistant to Prithivi. Explain any topic clearly and simply. Use examples, analogies, and step-by-step explanations. Adapt to the user's level. Make learning fun and engaging.`,
  travel: `You are JARVIS, an expert travel guide and assistant to Prithivi. Give detailed travel plans, tips, places to visit, food to try, budgets. Be specific and practical with real recommendations.`,
  finance: `You are JARVIS, a friendly financial advisor and assistant to Prithivi. Give practical money advice — budgeting, saving, investing basics. Be clear and actionable. Note you're not a certified financial advisor.`,
  fun: `You are JARVIS, the funniest, most entertaining AI and assistant to Prithivi. Tell jokes, play word games, be creative and amusing. Keep it family-friendly and genuinely funny.`,
  image: `You are JARVIS, an expert visual analyst and assistant to Prithivi. Analyze images in rich detail — describe all objects, people, text, colors, emotions, context. Give useful insights.`,
  email: `You are JARVIS, a professional writer and assistant to Prithivi. Write complete polished emails with Subject, Greeting, Body, Closing, Signature placeholder. Match the tone requested.`,
  research: `You are JARVIS, an expert researcher and assistant to Prithivi. Provide structured deep analysis with Overview, Key Points, Details, Conclusion. Be thorough and accurate.`,
};

// ═══ WEBSITE MAPPINGS ═══
const SITES = {
  youtube: 'https://youtube.com',
  google: 'https://google.com',
  gmail: 'https://mail.google.com',
  facebook: 'https://facebook.com',
  instagram: 'https://instagram.com',
  twitter: 'https://twitter.com',
  x: 'https://x.com',
  whatsapp: 'https://web.whatsapp.com',
  spotify: 'https://open.spotify.com',
  netflix: 'https://netflix.com',
  amazon: 'https://amazon.in',
  flipkart: 'https://flipkart.com',
  github: 'https://github.com',
  stackoverflow: 'https://stackoverflow.com',
  wikipedia: 'https://wikipedia.org',
  reddit: 'https://reddit.com',
  linkedin: 'https://linkedin.com',
  maps: 'https://maps.google.com',
  'google maps': 'https://maps.google.com',
  news: 'https://news.google.com',
  'google news': 'https://news.google.com',
  chat: 'https://chat.openai.com',
  chatgpt: 'https://chat.openai.com',
  zoom: 'https://zoom.us',
  drive: 'https://drive.google.com',
  'google drive': 'https://drive.google.com',
  docs: 'https://docs.google.com',
  sheets: 'https://sheets.google.com',
  slides: 'https://slides.google.com',
  meet: 'https://meet.google.com',
  'google meet': 'https://meet.google.com',
  udemy: 'https://udemy.com',
  coursera: 'https://coursera.org',
  leetcode: 'https://leetcode.com',
  hackerrank: 'https://hackerrank.com',
  paytm: 'https://paytm.com',
  phonepe: 'https://phonepe.com',
  swiggy: 'https://swiggy.com',
  zomato: 'https://zomato.com',
};

// ═══ DIALOG SYSTEM ═══
function showDialog(title, msg, placeholder) {
  return new Promise((resolve, reject) => {
    dlgResolve = resolve;
    dlgReject = reject;
    document.getElementById('dlgTitle').textContent = title;
    document.getElementById('dlgMsg').textContent = msg;
    document.getElementById('dlgInp').value = '';
    document.getElementById('dlgInp').placeholder = placeholder || 'Type here...';
    document.getElementById('dialogOverlay').classList.add('show');
    setTimeout(() => document.getElementById('dlgInp').focus(), 100);
  });
}
function dlgOK() {
  const val = document.getElementById('dlgInp').value.trim();
  document.getElementById('dialogOverlay').classList.remove('show');
  if (dlgResolve) dlgResolve(val);
}
function dlgCancel() {
  document.getElementById('dialogOverlay').classList.remove('show');
  if (dlgReject) dlgReject('cancelled');
}

// ═══ SETUP ═══
window.addEventListener('DOMContentLoaded', () => {
  if (KEY && KEY.length > 15) {
    document.getElementById('setup').style.display = 'none';
    showToast('⚡ Welcome back, Prithivi! JARVIS is ready.');
  }
  window.speechSynthesis && window.speechSynthesis.getVoices();
});
function onKI() { document.getElementById('lBtn').disabled = document.getElementById('keyInp').value.trim().length < 10; }
function toggleEye() { const i=document.getElementById('keyInp'); i.type=i.type==='password'?'text':'password'; }
function launch() {
  const k = document.getElementById('keyInp').value.trim();
  if (!k) return;
  KEY = k; localStorage.setItem('sj_key', k);
  document.getElementById('setup').style.display = 'none';
  showToast('⚡ JARVIS Activated! Welcome, Prithivi!');
}
function changeKey() { document.getElementById('keyInp').value=KEY; document.getElementById('lBtn').disabled=false; document.getElementById('setup').style.display='flex'; }

// ═══ UI ═══
function setMode(m, btn) {
  mode = m;
  document.querySelectorAll('.mb').forEach(b=>b.classList.remove('on'));
  btn.classList.add('on');
  document.getElementById('mDisp').textContent = m.toUpperCase();
  document.getElementById('mTag').textContent = 'MODE: '+m.toUpperCase();
  showToast('⚡ Mode: '+m.toUpperCase());
}
function selPill(el) { document.querySelectorAll('.pill').forEach(p=>p.classList.remove('on')); el.classList.add('on'); }
function togTTS() {
  tts=!tts;
  const b=document.getElementById('ttsP');
  b.classList.toggle('son',tts);
  b.textContent=tts?'🔊 ON':'🔊 Speak';
  if(!tts) window.speechSynthesis&&window.speechSynthesis.cancel();
  showToast(tts?'🔊 I will speak responses, Prithivi':'🔇 Speak off');
}
function autoR(el) { el.style.height='auto'; el.style.height=Math.min(el.scrollHeight,120)+'px'; }
function onKey(e) { if(e.key==='Enter'&&!e.shiftKey){e.preventDefault();send();} }
function showToast(msg) {
  document.querySelectorAll('.toast').forEach(t=>t.remove());
  const t=document.createElement('div'); t.className='toast'; t.textContent=msg;
  document.body.appendChild(t); setTimeout(()=>t.remove(),3200);
}
function addHist(txt) {
  const s=txt.substring(0,38)+(txt.length>38?'...':'');
  hist.unshift(s); if(hist.length>7) hist.pop();
  const h=hist.map((x,i)=>`<div class="hist-item" onclick="reuse(${i})">▹ ${x}</div>`).join('');
  document.getElementById('histList').innerHTML=h;
  const r=document.getElementById('histR'); if(r) r.innerHTML=h;
}
function reuse(i) { document.getElementById('ibox').value=hist[i].replace('...',''); document.getElementById('ibox').focus(); }
function hideWs() { const w=document.getElementById('ws'); if(w) w.style.display='none'; }

// ═══ RENDER ═══
function render(raw) {
  let h = raw.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
  h = h.replace(/```(\w*)\n?([\s\S]*?)```/g,(_,lang,code)=>{
    const ec=code.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
    return `<div class="cb"><span class="cl">${lang||'code'}</span>${ec}</div>`;
  });
  h = h.replace(/`([^`\n]+)`/g,'<code style="background:rgba(162,89,255,0.12);padding:1px 5px;border-radius:3px;font-family:\'Share Tech Mono\',monospace;font-size:11px;color:#c4a0ff">$1</code>');
  h = h.replace(/\*\*(.+?)\*\*/g,'<strong style="color:var(--p)">$1</strong>');
  h = h.replace(/\n/g,'<br>');
  return h;
}

function appendMsg(role, content, tag, imgSrc, extraCard) {
  hideWs();
  const area=document.getElementById('chat');
  const row=document.createElement('div');
  row.className='mrow '+role;
  const time=new Date().toLocaleTimeString([],{hour:'2-digit',minute:'2-digit'});
  const id='m_'+Date.now()+'_'+Math.random().toString(36).slice(2,5);
  const imgH=imgSrc?`<img src="${imgSrc}" class="chat-img" alt="uploaded">` :'';
  const m2=role==='ai'?`<button class="ibtn" onclick="spkId('${id}')">🔊</button><button class="ibtn" onclick="cpId('${id}')">📋</button>`:'';
  row.innerHTML=`
    <div class="av ${role}">${role==='ai'?'🤖':'👤'}</div>
    <div style="max-width:74%">
      <div class="bub" id="${id}">${imgH}${render(content)}${extraCard||''}</div>
      <div class="mmeta"><span>${time}</span>${tag?`<span class="tag">${tag}</span>`:''} ${m2}</div>
    </div>`;
  area.appendChild(row);
  area.scrollTop=area.scrollHeight;
  mc++;
  document.getElementById('mc').textContent=mc;
  document.getElementById('mp').style.width=Math.min(mc*3,100)+'%';
  if(role==='ai'&&tts) spkTxt(content);
}

function appendTyping() {
  hideWs();
  const area=document.getElementById('chat');
  const r=document.createElement('div');
  r.className='mrow ai'; r.id='tr';
  r.innerHTML=`<div class="av ai">🤖</div><div class="bub"><div class="typing"><div class="td"></div><div class="td"></div><div class="td"></div></div></div>`;
  area.appendChild(r); area.scrollTop=area.scrollHeight;
}
function rmTyping() { const t=document.getElementById('tr'); if(t)t.remove(); }

// ═══ TTS ═══
function spkId(id) { const e=document.getElementById(id); if(e) spkTxt(e.innerText||e.textContent); }
function spkTxt(txt) {
  if(!window.speechSynthesis){showToast('⚠️ TTS not supported');return;}
  window.speechSynthesis.cancel();
  const clean=txt.replace(/[#*`>_~]/g,'').replace(/\n+/g,' ').substring(0,900);
  const u=new SpeechSynthesisUtterance(clean);
  u.rate=0.93; u.pitch=1.05; u.volume=1;
  const vs=window.speechSynthesis.getVoices();
  const pick=vs.find(v=>/google|premium|natural|samantha|karen/i.test(v.name)&&v.lang.startsWith('en'));
  if(pick) u.voice=pick;
  window.speechSynthesis.speak(u);
  showToast('🔊 Speaking...');
}
function cpId(id) {
  const e=document.getElementById(id); if(!e) return;
  navigator.clipboard.writeText(e.innerText||e.textContent)
    .then(()=>showToast('✅ Copied!'))
    .catch(()=>showToast('⚠️ Copy failed'));
}

// ═══ VOICE ═══
function togVoice() {
  const btn=document.getElementById('vBtn');
  const SR=window.SpeechRecognition||window.webkitSpeechRecognition;
  if(!SR){showToast('⚠️ Voice needs Chrome browser');return;}
  if(isRec){recog&&recog.stop();return;}
  recog=new SR();
  recog.continuous=false; recog.interimResults=true; recog.lang='en-US';
  recog.onstart=()=>{isRec=true;btn.classList.add('rec');btn.textContent='⏹️';showToast('🎙️ Listening, Prithivi... speak now');};
  recog.onresult=(e)=>{
    let f='',ix='';
    for(let i=e.resultIndex;i<e.results.length;i++){
      if(e.results[i].isFinal)f+=e.results[i][0].transcript;
      else ix+=e.results[i][0].transcript;
    }
    const v=(f||ix).trim();
    if(v){document.getElementById('ibox').value=v;autoR(document.getElementById('ibox'));}
  };
  recog.onerror=(e)=>{showToast('⚠️ Mic: '+e.error);isRec=false;btn.classList.remove('rec');btn.textContent='🎙️';};
  recog.onend=()=>{
    isRec=false;btn.classList.remove('rec');btn.textContent='🎙️';
    const t=document.getElementById('ibox').value.trim();
    if(t){showToast(`🎙️ "${t.substring(0,40)}"`);setTimeout(()=>send(),350);}
    else showToast('⚠️ Nothing heard — try again, Prithivi');
  };
  recog.start();
}

// ═══ SPECIAL FEATURES ═══

async function fetchJoke() {
  try {
    const r=await fetch('https://v2.jokeapi.dev/joke/Any?safe-mode&type=twopart');
    const d=await r.json();
    if(d.setup&&d.delivery) return `😄 **${d.setup}**\n\n${d.delivery}`;
    return null;
  } catch(e){ return null; }
}

async function fetchMusic(query) {
  try {
    const r=await fetch(`https://itunes.apple.com/search?term=${encodeURIComponent(query)}&media=music&limit=5`);
    const d=await r.json();
    if(d.results&&d.results.length>0){
      return d.results.map(t=>({
        name:t.trackName,
        artist:t.artistName,
        album:t.collectionName,
        thumb:t.artworkUrl60,
        preview:t.previewUrl,
        url:t.trackViewUrl
      }));
    }
    return null;
  } catch(e){return null;}
}

function buildMusicCard(tracks) {
  if(!tracks||!tracks.length) return '';
  let h=`<div class="card"><div class="card-title">🎵 MUSIC RESULTS</div>`;
  tracks.forEach(t=>{
    h+=`<div class="music-item" onclick="window.open('${t.url}','_blank')">
      <div class="music-thumb">${t.thumb?`<img src="${t.thumb}" width="44" height="44" style="border-radius:5px">`:'🎵'}</div>
      <div class="music-info"><div class="mt">${t.name||'Unknown'}</div><div class="ma">${t.artist||''} · ${t.album||''}</div></div>
      ${t.preview?`<button class="play-btn" onclick="event.stopPropagation();playPreview('${t.preview}')">▶</button>`:''}
    </div>`;
  });
  h+=`</div>`;
  return h;
}

let curAudio=null;
function playPreview(url) {
  if(curAudio){curAudio.pause();curAudio=null;}
  curAudio=new Audio(url);
  curAudio.play().catch(()=>showToast('⚠️ Preview unavailable'));
  showToast('▶ Playing 30s preview...');
}

// ═══ INTENT DETECTION ═══
function detectIntent(txt) {
  const t = txt.toLowerCase();
  if (/joke|funny|laugh|humor|comedy|entertain/i.test(t)) return 'joke';
  if (/song|music|recommend.*song|play.*song|artist|playlist|listen|album|singer/i.test(t)) return 'music';
  if (/open\s+(website|site|page|app|browser)?\s*\w|launch\s+\w|go to\s+\w/i.test(t)) return 'website';
  if (/compose\s+email|write\s+email|send\s+email|draft\s+email|new\s+email/i.test(t)) return 'email_compose';
  if (/whatsapp|whats app|send\s+(message|msg|text)\s+(to|in)\s+whatsapp|whatsapp\s+(message|msg)/i.test(t)) return 'whatsapp';
  return 'ai';
}

function extractSiteName(txt) {
  const t = txt.toLowerCase();
  const m = t.match(/open\s+(?:website\s+|site\s+|page\s+|app\s+)?(.+?)(?:\s+website|\s+site|\s+app|\s+page|$)/i)
         || t.match(/launch\s+(.+)/i)
         || t.match(/go to\s+(.+)/i);
  return m ? m[1].trim().replace(/[.!?]$/, '') : null;
}

function extractMusicQuery(txt) {
  return txt.replace(/recommend|songs?|music|play|find|search|give me|suggest/gi,'').trim()||txt;
}

// ═══ WEBSITE HANDLER ═══
async function handleWebsite(txt) {
  const siteName = extractSiteName(txt);
  if (!siteName) {
    appendMsg('ai', '🌐 Prithivi, which website would you like me to open? Just say "open youtube" or "open google"!', 'WEB');
    return;
  }

  const key = siteName.toLowerCase();
  let url = SITES[key];

  // Try partial match
  if (!url) {
    for (const [k, v] of Object.entries(SITES)) {
      if (k.includes(key) || key.includes(k)) { url = v; break; }
    }
  }

  // Build URL if not found
  if (!url) {
    if (key.includes('.')) url = 'https://' + key;
    else url = 'https://' + key + '.com';
  }

  const card = `<div class="action-card">
    <div class="ac-title">🌐 OPENING WEBSITE</div>
    <p style="font-size:12px;color:var(--dim);margin-bottom:10px;">Ready to open: <strong style="color:var(--p)">${url}</strong></p>
    <a class="action-btn" href="${url}" target="_blank" rel="noopener">🚀 Open ${siteName.toUpperCase()}</a>
  </div>`;
  appendMsg('ai', `Sure, Prithivi! Opening **${siteName}** for you right now! 🌐`, 'WEB', null, card);
  setTimeout(() => window.open(url, '_blank'), 600);
}

// ═══ EMAIL COMPOSE HANDLER ═══
async function handleEmailCompose(txt) {
  appendTyping();
  await new Promise(r => setTimeout(r, 400));
  rmTyping();

  const t = txt.toLowerCase();
  let to = '', subject = '', body = '';

  // Extract "to" from text
  const toMatch = txt.match(/to\s+([A-Za-z0-9._%+\-@]+)/i);
  if (toMatch) to = toMatch[1];

  // Extract subject
  const subMatch = txt.match(/subject\s+([^,]+)/i) || txt.match(/about\s+([^,]+)/i) || txt.match(/regarding\s+([^,]+)/i);
  if (subMatch) subject = subMatch[1].trim();

  // Ask for missing info with dialog
  if (!to) {
    try {
      to = await showDialog('📧 RECIPIENT', 'Prithivi, who should I send this email to? (Enter email address or name)', 'e.g. boss@company.com or friend@gmail.com');
    } catch(e) {
      appendMsg('ai', '✉️ Email compose cancelled, Prithivi!', 'EMAIL');
      return;
    }
  }

  if (!subject) {
    try {
      subject = await showDialog('📝 SUBJECT', 'What is the email subject, Prithivi?', 'e.g. Meeting Tomorrow');
    } catch(e) {
      appendMsg('ai', '✉️ Email compose cancelled, Prithivi!', 'EMAIL');
      return;
    }
  }

  // Ask for body/message
  let bodyPrompt = txt.replace(/compose|write|send|draft|email|to\s+\S+|subject\s+[^,]+|about\s+[^,]+/gi, '').trim();
  if (bodyPrompt.length < 5) {
    try {
      bodyPrompt = await showDialog('✍️ MESSAGE', 'What should the email say, Prithivi? (Brief description — I\'ll write it professionally)', 'e.g. Inform about project delay, apologies');
    } catch(e) {
      appendMsg('ai', '✉️ Email compose cancelled, Prithivi!', 'EMAIL');
      return;
    }
  }

  // Use AI to write the email body
  appendTyping();
  try {
    const res = await fetch('https://api.groq.com/openai/v1/chat/completions', {
      method: 'POST',
      headers: {'Content-Type':'application/json','Authorization':'Bearer '+KEY},
      body: JSON.stringify({
        model: 'llama-3.3-70b-versatile',
        max_tokens: 600,
        temperature: 0.7,
        messages: [
          {role:'system', content:'You are a professional email writer. Write only the email body (no subject line). Be professional and clear.'},
          {role:'user', content: `Write a professional email body about: ${bodyPrompt}. Keep it concise, friendly, and professional.`}
        ]
      })
    });
    const data = await res.json();
    body = data.choices[0]?.message?.content || bodyPrompt;
  } catch(e) {
    body = bodyPrompt;
  }
  rmTyping();

  // Build Gmail compose URL
  const gmailUrl = `https://mail.google.com/mail/?view=cm&fs=1&to=${encodeURIComponent(to)}&su=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;

  const card = `<div class="action-card">
    <div class="ac-title">✉️ EMAIL READY</div>
    <div style="font-size:11px;color:var(--dim);margin-bottom:6px;line-height:1.8">
      <strong style="color:var(--p)">To:</strong> ${to}<br>
      <strong style="color:var(--p)">Subject:</strong> ${subject}
    </div>
    <div style="background:rgba(0,0,0,0.3);border-radius:7px;padding:10px;font-size:11px;color:var(--txt);line-height:1.6;margin-bottom:10px;max-height:80px;overflow-y:auto;">${body.substring(0,300)}...</div>
    <a class="action-btn" href="${gmailUrl}" target="_blank" rel="noopener">📨 Open in Gmail & Send</a>
  </div>`;

  appendMsg('ai', `✉️ I've drafted the email for you, Prithivi! Click below to open Gmail with everything pre-filled — just hit Send!`, 'EMAIL', null, card);
}

// ═══ WHATSAPP HANDLER ═══
async function handleWhatsApp(txt) {
  appendTyping();
  await new Promise(r => setTimeout(r, 400));
  rmTyping();

  let contact = '', message = '';

  // Extract contact name from text
  const cMatch = txt.match(/(?:to|message)\s+(?:my\s+)?([A-Za-z\s]+?)(?:\s+saying|\s+that|\s+message|\s+:|$)/i);
  if (cMatch) contact = cMatch[1].trim();

  // Extract message from text
  const mMatch = txt.match(/(?:saying|that|message:)\s+(.+)/i);
  if (mMatch) message = mMatch[1].trim();

  if (!contact) {
    try {
      contact = await showDialog('👤 CONTACT NAME', 'Prithivi, who would you like to send a WhatsApp message to?', 'e.g. Rahul, Mom, Best Friend');
    } catch(e) {
      appendMsg('ai', '💬 WhatsApp message cancelled, Prithivi!', 'WHATSAPP');
      return;
    }
  }

  if (!message) {
    try {
      message = await showDialog('💬 YOUR MESSAGE', `What should I tell ${contact} on WhatsApp, Prithivi?`, 'Type your message here...');
    } catch(e) {
      appendMsg('ai', '💬 WhatsApp message cancelled, Prithivi!', 'WHATSAPP');
      return;
    }
  }

  // WhatsApp Web URL with message (opens chat search — user must select contact)
  const waUrl = `https://web.whatsapp.com/`;
  const waSearch = `https://web.whatsapp.com/`;

  const card = `<div class="action-card">
    <div class="ac-title">💬 WHATSAPP MESSAGE</div>
    <div style="font-size:11px;color:var(--dim);margin-bottom:8px;line-height:1.8">
      <strong style="color:var(--p)">To:</strong> ${contact}<br>
      <strong style="color:var(--p)">Message:</strong> "${message}"
    </div>
    <div style="font-size:10px;color:var(--warn);margin-bottom:10px;background:rgba(255,204,0,0.07);border-radius:6px;padding:8px;">
      ⚠️ WhatsApp Web doesn't allow direct sending via browser automation for security. Click below — WhatsApp Web will open. Search for <strong>${contact}</strong>, paste the message and send!
    </div>
    <button class="action-btn" onclick="copyAndOpenWA('${message.replace(/'/g,"\\'")}', '${waUrl}')">📋 Copy Message & Open WhatsApp Web</button>
  </div>`;

  appendMsg('ai', `💬 Ready to send your WhatsApp message to **${contact}**, Prithivi! Due to WhatsApp's security, I'll copy the message and open WhatsApp Web for you — just find ${contact} and paste!`, 'WHATSAPP', null, card);
}

function copyAndOpenWA(message, url) {
  navigator.clipboard.writeText(message)
    .then(() => {
      showToast('✅ Message copied! Opening WhatsApp Web...');
      setTimeout(() => window.open(url, '_blank'), 500);
    })
    .catch(() => {
      window.open(url, '_blank');
      showToast('⚠️ Please manually copy the message');
    });
}

// ═══ MAIN SEND ═══
async function send() {
  if(!KEY){showToast('⚠️ No API key! Click Key button');changeKey();return;}
  const el=document.getElementById('ibox');
  const txt=el.value.trim();
  if(!txt&&!pendImg) return;
  const userTxt=txt||'[Image sent for analysis]';
  el.value=''; autoR(el);

  appendMsg('user',userTxt,mode.toUpperCase(),pendImg?`data:${pendImgT};base64,${pendImg}`:null);
  addHist(userTxt);
  const savedImg=pendImg, savedImgT=pendImgT;
  clearImg();

  const intent = detectIntent(userTxt);

  // Website intent
  if (intent === 'website' && !savedImg) {
    await handleWebsite(userTxt);
    return;
  }

  // Email compose intent
  if (intent === 'email_compose' && !savedImg) {
    await handleEmailCompose(userTxt);
    return;
  }

  // WhatsApp intent
  if (intent === 'whatsapp' && !savedImg) {
    await handleWhatsApp(userTxt);
    return;
  }

  // Joke intent
  if(intent==='joke'&&!savedImg) {
    appendTyping();
    const joke=await fetchJoke();
    rmTyping();
    if(joke){
      appendMsg('ai',joke,'FUN');
      msgs.push({role:'user',content:userTxt});
      msgs.push({role:'assistant',content:joke});
      return;
    }
  }

  // Music intent
  if(intent==='music'&&!savedImg) {
    appendTyping();
    const query=extractMusicQuery(userTxt);
    const tracks=await fetchMusic(query);
    rmTyping();
    if(tracks&&tracks.length){
      const card=buildMusicCard(tracks);
      const aiTxt=`Here are some **${query}** recommendations for you, Prithivi! 🎵\n\nClick any song to open in iTunes, or hit ▶ for a 30-second preview!`;
      appendMsg('ai',aiTxt,'MUSIC',null,card);
      msgs.push({role:'user',content:userTxt});
      msgs.push({role:'assistant',content:aiTxt});
      return;
    }
  }

  // Build API messages
  let userContent=[];
  if(savedImg) userContent.push({type:'image_url',image_url:{url:`data:${savedImgT};base64,${savedImg}`}});
  const prompt=buildPrompt(txt||'Please analyze this image in detail.');
  userContent.push({type:'text',text:prompt});
  const apiMsg=userContent.length===1?userContent[0].text:userContent;
  msgs.push({role:'user',content:apiMsg});
  appendTyping();

  try {
    const res=await fetch('https://api.groq.com/openai/v1/chat/completions',{
      method:'POST',
      headers:{
        'Content-Type':'application/json',
        'Authorization':'Bearer '+KEY
      },
      body:JSON.stringify({
        model: savedImg ? 'llama-3.2-11b-vision-preview' : 'llama-3.3-70b-versatile',
        max_tokens:2048,
        temperature:0.85,
        messages:[
          {role:'system',content:SYS[mode]||SYS.chat},
          ...msgs
        ]
      })
    });
    rmTyping();
    if(!res.ok){
      let em='HTTP '+res.status;
      try{const j=await res.json();em=j.error?.message||em;}catch(_){}
      msgs.pop();
      if(res.status===401) appendMsg('ai','🔑 **Invalid Groq API Key, Prithivi!**\n\nClick the 🔑 Key button and enter your valid key from console.groq.com → API Keys','ERROR');
      else if(res.status===429) appendMsg('ai','⏳ **Rate limit hit, Prithivi.** Wait 30 seconds and try again. Groq free tier allows ~30 requests/minute.','ERROR');
      else appendMsg('ai',`⚠️ **Error ${res.status}:** ${em}`,'ERROR');
      return;
    }
    const data=await res.json();
    const reply=data.choices[0]?.message?.content||'Sorry, Prithivi, I could not generate a response.';
    msgs.push({role:'assistant',content:reply});
    appendMsg('ai',reply,mode.toUpperCase());
  } catch(err) {
    rmTyping(); msgs.pop();
    if(err.message&&(err.message.includes('fetch')||err.message.includes('Failed'))){
      appendMsg('ai',
        '🌐 **CORS / Network Error, Prithivi**\n\n'+
        'You opened the file directly. Fix:\n'+
        '1. Open Terminal\n'+
        '2. Run: `python -m http.server 8080`\n'+
        '3. Open: `http://localhost:8080/super_jarvis.html`\n\n'+
        'Or use VS Code → Live Server extension → Right-click → Open with Live Server',
        'ERROR');
    } else {
      appendMsg('ai','⚠️ **Error:** '+err.message,'ERROR');
    }
  }
}

function buildPrompt(txt) {
  const p={
    code:'Write complete working well-commented code for: '+txt,
    cooking:'Give me a complete recipe with ingredients and step-by-step instructions for: '+txt,
    music:'Recommend and discuss music about: '+txt,
    health:'Give practical health/fitness advice about: '+txt,
    study:'Explain clearly and simply: '+txt,
    travel:'Give travel advice and recommendations for: '+txt,
    finance:'Give practical financial advice about: '+txt,
    fun:'Be funny and entertaining about: '+txt,
    email:'Write a professional email for: '+txt,
    research:'Research and analyze in depth: '+txt,
    image:txt,
  };
  return p[mode]||txt;
}

function sq(txt){document.getElementById('ibox').value=txt;send();}

// ═══ FILES ═══
function trigFile(){document.getElementById('fInp').click();}
function trigImg(){document.getElementById('iInp').click();}
function handleFile(inp){
  const f=inp.files[0];if(!f)return;
  const r=new FileReader();
  r.onload=e=>{
    const c=e.target.result;
    const p=c.substring(0,3500)+(c.length>3500?'\n...[truncated]':'');
    document.getElementById('ibox').value=`Analyze this file "${f.name}":\n\n${p}`;
    showToast(`📄 Loaded: ${f.name}`);
    autoR(document.getElementById('ibox'));
  };
  r.readAsText(f);inp.value='';
}
function handleImg(inp){
  const f=inp.files[0];if(!f)return;
  const r=new FileReader();
  r.onload=e=>{
    const d=e.target.result;
    pendImg=d.split(',')[1]; pendImgT=f.type||'image/jpeg';
    document.getElementById('imgPrev').src=d;
    document.getElementById('imgBar').style.display='block';
    setMode('image',document.querySelectorAll('.mb')[9]);
    showToast(`🖼️ Image ready: ${f.name}`);
  };
  r.readAsDataURL(f);inp.value='';
}
function clearImg(){
  pendImg=null;
  document.getElementById('imgBar').style.display='none';
  document.getElementById('imgPrev').src='';
}

// ═══ CLEAR ═══
function clearChat(){
  msgs=[];mc=0;
  document.getElementById('mc').textContent='0';
  document.getElementById('mp').style.width='0%';
  hist=[];
  const empty='<div style="font-size:9.5px;color:var(--dim);font-family:\'Share Tech Mono\',monospace">No history yet...</div>';
  document.getElementById('histList').innerHTML=empty;
  const r=document.getElementById('histR');if(r)r.innerHTML=empty;
  window.speechSynthesis&&window.speechSynthesis.cancel();
  if(curAudio){curAudio.pause();curAudio=null;}
  document.getElementById('chat').innerHTML=`
    <div class="welcome" id="ws">
      <div class="w-orb"><div class="w-core">🤖</div></div>
      <h2>JARVIS READY</h2>
      <p>Session cleared, Prithivi. Ask me anything — I'm here to help!</p>
      <div class="qgrid">
        <div class="qbtn" onclick="sq('Tell me a funny joke')"><span>😄</span>Tell a Joke</div>
        <div class="qbtn" onclick="sq('Give me a simple recipe for pasta carbonara with steps')"><span>🍳</span>Cook Pasta</div>
        <div class="qbtn" onclick="sq('Recommend 5 popular songs right now')"><span>🎵</span>Song Recs</div>
        <div class="qbtn" onclick="sq('Give me a 7-day morning workout plan for beginners')"><span>💪</span>Workout Plan</div>
        <div class="qbtn" onclick="sq('open website youtube')"><span>🌐</span>Open YouTube</div>
        <div class="qbtn" onclick="sq('compose email to boss about meeting')"><span>✉️</span>Compose Email</div>
        <div class="qbtn" onclick="sq('send whatsapp message to my friend')"><span>💬</span>WhatsApp Msg</div>
        <div class="qbtn" onclick="sq('Write a Python function to sort numbers')"><span>💻</span>Write Code</div>
        <div class="qbtn" onclick="sq('Daily health tips for healthy lifestyle')"><span>🌿</span>Health Tips</div>
      </div>
    </div>`;
  showToast('🗑️ Fresh start — ready, Prithivi!');
}
</script>
</body>
</html>
