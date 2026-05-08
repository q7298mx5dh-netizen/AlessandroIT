<!DOCTYPE html>

<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Alessandro Passarello — IT Network & Security</title>
<meta name="description" content="Blog di networking e sicurezza IT. Guide pratiche su Check Point, VPN, VLAN scritte da chi lavora ogni giorno in ambiente enterprise bancario.">
<link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@400;500;600;700&family=Orbitron:wght@400;700;900&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
<style>
:root {
  --bg:        #03010a;
  --bg2:       #07030f;
  --surface:   #0d0720;
  --surface2:  #130a2e;
  --violet:    #7c3aed;
  --violet2:   #a855f7;
  --neon:      #c084fc;
  --blue:      #38bdf8;
  --blue2:     #7dd3fc;
  --cyan:      #22d3ee;
  --text:      #f1f0ff;
  --muted:     #6b7280;
  --muted2:    #9ca3af;
  --border:    rgba(124,58,237,0.18);
  --border2:   rgba(124,58,237,0.35);
  --glow-v:    rgba(124,58,237,0.4);
  --glow-b:    rgba(56,189,248,0.3);
}

*{margin:0;padding:0;box-sizing:border-box;}
html{scroll-behavior:smooth;}
body{
background:var(–bg);
color:var(–text);
font-family:‘Rajdhani’,sans-serif;
overflow-x:hidden;
cursor:none;
}

/* ── AMBIENT BG ── */
body::before{
content:’’;
position:fixed;inset:0;
background:
radial-gradient(ellipse 60% 50% at 20% 20%, rgba(124,58,237,0.12) 0%, transparent 60%),
radial-gradient(ellipse 50% 40% at 80% 80%, rgba(56,189,248,0.08) 0%, transparent 60%),
radial-gradient(ellipse 40% 60% at 50% 50%, rgba(168,85,247,0.05) 0%, transparent 70%);
pointer-events:none;z-index:0;
}

/* GRID BG */
body::after{
content:’’;
position:fixed;inset:0;
background-image:
linear-gradient(rgba(124,58,237,0.04) 1px, transparent 1px),
linear-gradient(90deg, rgba(124,58,237,0.04) 1px, transparent 1px);
background-size:50px 50px;
pointer-events:none;z-index:0;
}

/* ── CURSOR ── */
.cur{
width:16px;height:16px;
border:2px solid var(–neon);
border-radius:50%;
position:fixed;pointer-events:none;z-index:9999;
transform:translate(-50%,-50%);
transition:width .25s,height .25s,border-color .25s,background .25s;
box-shadow:0 0 10px var(–neon),0 0 20px rgba(192,132,252,0.3);
}
.cur.hovering{
width:48px;height:48px;
background:rgba(124,58,237,0.15);
border-color:var(–violet2);
}
.cur-dot{
width:4px;height:4px;
background:var(–neon);
border-radius:50%;
position:fixed;pointer-events:none;z-index:10000;
transform:translate(-50%,-50%);
box-shadow:0 0 6px var(–neon);
}

/* ── SCANLINE ── */
.scanline{
position:fixed;inset:0;
background:repeating-linear-gradient(
0deg,transparent,transparent 2px,
rgba(0,0,0,0.03) 2px,rgba(0,0,0,0.03) 4px
);
pointer-events:none;z-index:1;
}

/* ── NAV ── */
nav{
position:fixed;top:0;left:0;right:0;z-index:500;
padding:1rem 2.5rem;
display:flex;align-items:center;justify-content:space-between;
background:rgba(3,1,10,0.85);
backdrop-filter:blur(20px);
border-bottom:1px solid var(–border);
box-shadow:0 1px 30px rgba(124,58,237,0.1);
}
.logo{
font-family:‘Orbitron’,monospace;
font-size:.9rem;font-weight:900;
color:var(–text);text-decoration:none;cursor:none;
letter-spacing:.05em;
text-shadow:0 0 20px var(–neon);
}
.logo .dot{color:var(–neon);}
.nav-links{display:flex;gap:2rem;list-style:none;}
.nav-links a{
font-size:.7rem;font-weight:600;
text-transform:uppercase;letter-spacing:.15em;
color:var(–muted);text-decoration:none;
transition:color .2s,text-shadow .2s;cursor:none;
}
.nav-links a:hover{
color:var(–neon);
text-shadow:0 0 10px rgba(192,132,252,0.6);
}
.nav-cta{
font-family:‘Orbitron’,monospace;
font-size:.65rem;font-weight:700;letter-spacing:.1em;
color:var(–bg);background:linear-gradient(135deg,var(–violet),var(–violet2));
padding:.5rem 1.2rem;text-decoration:none;cursor:none;
clip-path:polygon(8px 0%, 100% 0%, calc(100% - 8px) 100%, 0% 100%);
transition:box-shadow .2s,filter .2s;
box-shadow:0 0 20px rgba(124,58,237,0.4);
}
.nav-cta:hover{
box-shadow:0 0 35px rgba(124,58,237,0.7);
filter:brightness(1.15);
}

/* ── PAGES ── */
.page{display:none;position:relative;z-index:2;}
.page.active{display:block;}

/* ══ HOME ══════════════════════════════════════ */

/* HERO */
.hero{
min-height:100vh;
padding:9rem 2.5rem 5rem;
display:flex;flex-direction:column;justify-content:center;
position:relative;overflow:hidden;
}

/* animated orbs */
.orb{
position:absolute;border-radius:50%;
pointer-events:none;filter:blur(80px);
}
.orb1{width:600px;height:600px;background:rgba(124,58,237,0.15);top:-200px;left:-200px;animation:orbFloat 8s ease-in-out infinite;}
.orb2{width:400px;height:400px;background:rgba(56,189,248,0.1);bottom:-100px;right:-100px;animation:orbFloat 10s ease-in-out 2s infinite reverse;}
.orb3{width:300px;height:300px;background:rgba(168,85,247,0.12);top:50%;left:50%;animation:orbFloat 12s ease-in-out 1s infinite;}

@keyframes orbFloat{
0%,100%{transform:translate(0,0) scale(1);}
33%{transform:translate(30px,-20px) scale(1.05);}
66%{transform:translate(-20px,30px) scale(0.95);}
}

.hero-eyebrow{
display:flex;align-items:center;gap:.75rem;
margin-bottom:1.75rem;
}
.eyebrow-line{width:40px;height:1px;background:linear-gradient(90deg,var(–violet),var(–neon));}
.eyebrow-txt{
font-family:‘JetBrains Mono’,monospace;
font-size:.65rem;color:var(–neon);
text-transform:uppercase;letter-spacing:.2em;
text-shadow:0 0 15px rgba(192,132,252,0.5);
}

.hero h1{
font-family:‘Orbitron’,monospace;
font-size:clamp(2.5rem,7vw,7rem);
font-weight:900;line-height:.95;
letter-spacing:-.02em;
margin-bottom:1.75rem;
position:relative;z-index:1;
}
.hero h1 .line-glow{
display:block;
background:linear-gradient(135deg,var(–text) 0%,var(–neon) 50%,var(–blue2) 100%);
-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
filter:drop-shadow(0 0 30px rgba(192,132,252,0.4));
}
.hero h1 .line-outline{
display:block;
-webkit-text-stroke:1px rgba(124,58,237,0.6);
color:transparent;
text-shadow:none;
filter:none;
-webkit-text-fill-color:transparent;
}
.hero h1 .line-solid{display:block;color:var(–text);}

.hero-desc{
font-size:1.05rem;line-height:1.8;
color:var(–muted2);
max-width:520px;margin-bottom:2.5rem;
position:relative;z-index:1;
font-weight:500;
border-left:2px solid var(–violet);
padding-left:1.25rem;
}
.hero-desc strong{color:var(–neon);}

.hero-btns{display:flex;gap:1rem;flex-wrap:wrap;position:relative;z-index:1;margin-bottom:4rem;}

.btn-primary{
font-family:‘Orbitron’,monospace;
font-size:.7rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;
color:var(–text);
background:linear-gradient(135deg,var(–violet),var(–violet2));
padding:.9rem 2rem;text-decoration:none;cursor:none;
clip-path:polygon(10px 0%,100% 0%,calc(100% - 10px) 100%,0% 100%);
border:none;
box-shadow:0 0 25px rgba(124,58,237,0.5),inset 0 1px 0 rgba(255,255,255,0.1);
transition:box-shadow .3s,filter .2s;display:inline-block;
}
.btn-primary:hover{
box-shadow:0 0 45px rgba(124,58,237,0.8),0 0 80px rgba(168,85,247,0.3);
filter:brightness(1.1);
}
.btn-ghost{
font-family:‘Orbitron’,monospace;
font-size:.7rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;
color:var(–neon);background:transparent;
padding:.9rem 2rem;text-decoration:none;cursor:none;
border:1px solid var(–border2);
clip-path:polygon(10px 0%,100% 0%,calc(100% - 10px) 100%,0% 100%);
box-shadow:0 0 15px rgba(192,132,252,0.15),inset 0 0 15px rgba(124,58,237,0.05);
transition:all .3s;display:inline-block;
}
.btn-ghost:hover{
background:rgba(124,58,237,0.15);
box-shadow:0 0 30px rgba(192,132,252,0.3),inset 0 0 20px rgba(124,58,237,0.1);
}

/* STATS BAR */
.hero-stats{
display:flex;align-items:center;gap:0;
border:1px solid var(–border);
background:rgba(13,7,32,0.6);
backdrop-filter:blur(10px);
width:fit-content;
position:relative;z-index:1;
}
.hero-stat{
padding:1.25rem 2rem;
border-right:1px solid var(–border);
position:relative;
}
.hero-stat:last-child{border-right:none;}
.hero-stat::before{
content:’’;position:absolute;top:0;left:0;right:0;height:2px;
background:linear-gradient(90deg,var(–violet),var(–neon));
}
.stat-num{
font-family:‘Orbitron’,monospace;
font-size:1.75rem;font-weight:900;
color:var(–neon);line-height:1;
text-shadow:0 0 20px rgba(192,132,252,0.6);
}
.stat-lbl{
font-family:‘JetBrains Mono’,monospace;
font-size:.58rem;color:var(–muted);
text-transform:uppercase;letter-spacing:.12em;
margin-top:.25rem;
}

/* MARQUEE */
.marquee-wrap{
border-top:1px solid var(–border);
border-bottom:1px solid var(–border);
background:rgba(13,7,32,0.8);
padding:.8rem 0;overflow:hidden;
}
.marquee-track{
display:flex;gap:2.5rem;
animation:marquee 28s linear infinite;
white-space:nowrap;
}
.mi{
font-family:‘JetBrains Mono’,monospace;
font-size:.65rem;text-transform:uppercase;letter-spacing:.15em;
color:rgba(156,163,175,0.5);
display:flex;align-items:center;gap:.6rem;flex-shrink:0;
}
.mi .d{
width:4px;height:4px;
background:var(–violet2);border-radius:50%;
box-shadow:0 0 6px var(–violet2);
}

/* SECTION LABEL */
.slabel{
font-family:‘JetBrains Mono’,monospace;
font-size:.62rem;color:var(–violet2);
text-transform:uppercase;letter-spacing:.2em;
margin-bottom:1.25rem;
display:flex;align-items:center;gap:.75rem;
}
.slabel::after{content:’’;flex:1;height:1px;background:var(–border);}

/* ABOUT */
.about-section{
display:grid;grid-template-columns:1fr 1fr;
border-top:1px solid var(–border);border-bottom:1px solid var(–border);
}
.about-left{
padding:5rem 2.5rem;
border-right:1px solid var(–border);
}
.about-left h2{
font-family:‘Orbitron’,monospace;
font-size:clamp(1.5rem,3vw,2.5rem);
font-weight:900;line-height:1.1;
letter-spacing:-.01em;margin-bottom:1.5rem;
}
.about-left h2 .glow-txt{
background:linear-gradient(135deg,var(–neon),var(–blue2));
-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
}
.about-left p{font-size:.95rem;line-height:1.85;color:var(–muted2);margin-bottom:.85rem;font-weight:500;}
.about-left p strong{color:var(–neon);}
.about-right{padding:5rem 2.5rem;display:flex;flex-direction:column;gap:2rem;}

.skill-pills{display:flex;flex-wrap:wrap;gap:.4rem;}
.sp{
font-family:‘JetBrains Mono’,monospace;
font-size:.62rem;letter-spacing:.08em;text-transform:uppercase;
padding:.28rem .7rem;
border:1px solid var(–border);
color:var(–muted2);
background:rgba(124,58,237,0.05);
transition:all .2s;
}
.sp:hover{
border-color:var(–neon);color:var(–neon);
background:rgba(192,132,252,0.08);
box-shadow:0 0 10px rgba(192,132,252,0.2);
}

.stats-grid{
display:grid;grid-template-columns:repeat(3,1fr);
gap:1px;background:var(–border);border:1px solid var(–border);
}
.stat-cell{
padding:1.5rem;background:var(–surface);
position:relative;overflow:hidden;
}
.stat-cell::before{
content:’’;position:absolute;top:0;left:0;right:0;height:2px;
background:linear-gradient(90deg,var(–violet),var(–neon));
}
.stat-v{
font-family:‘Orbitron’,monospace;
font-size:2rem;font-weight:900;
color:var(–neon);line-height:1;
text-shadow:0 0 20px rgba(192,132,252,0.5);
}
.stat-k{
font-family:‘JetBrains Mono’,monospace;
font-size:.58rem;color:var(–muted);
text-transform:uppercase;letter-spacing:.1em;margin-top:.3rem;
}

/* ARTICLES */
.articles-section{padding:5rem 2.5rem;}
.sec-header{display:flex;align-items:flex-end;justify-content:space-between;margin-bottom:2rem;}
.sec-header h2{
font-family:‘Orbitron’,monospace;
font-size:clamp(1.5rem,3vw,2.2rem);
font-weight:900;letter-spacing:-.01em;
}
.sec-header h2 span{
background:linear-gradient(135deg,var(–neon),var(–blue2));
-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
}

/* FILTER */
.filter-wrap{display:flex;gap:.4rem;flex-wrap:wrap;margin-bottom:2.5rem;}
.filter-btn{
font-family:‘JetBrains Mono’,monospace;
font-size:.6rem;font-weight:700;
text-transform:uppercase;letter-spacing:.12em;
padding:.3rem .8rem;
border:1px solid var(–border);
color:var(–muted);background:none;cursor:none;transition:all .25s;
}
.filter-btn:hover,.filter-btn.active{
border-color:var(–violet2);color:var(–neon);
background:rgba(124,58,237,0.12);
box-shadow:0 0 15px rgba(124,58,237,0.2);
}

/* FEATURED */
.featured{
display:grid;grid-template-columns:1fr 400px;
border:1px solid var(–border);
margin-bottom:1.5rem;cursor:none;
transition:border-color .3s,box-shadow .3s;
overflow:hidden;
background:var(–surface);
}
.featured:hover{
border-color:var(–violet2);
box-shadow:0 0 40px rgba(124,58,237,0.2),0 0 80px rgba(124,58,237,0.08);
}
.feat-visual{
min-height:320px;display:flex;align-items:center;justify-content:center;
position:relative;overflow:hidden;
background:linear-gradient(135deg,rgba(124,58,237,0.15),rgba(56,189,248,0.08));
}
.feat-visual::before{
content:’’;position:absolute;inset:0;
background:repeating-linear-gradient(
45deg,transparent,transparent 30px,
rgba(124,58,237,0.04) 30px,rgba(124,58,237,0.04) 31px
);
}
.feat-icon{font-size:5rem;z-index:1;filter:drop-shadow(0 0 30px rgba(192,132,252,0.6));}
.feat-corner{
position:absolute;top:1.25rem;left:1.25rem;
font-family:‘JetBrains Mono’,monospace;
font-size:.58rem;color:rgba(192,132,252,.4);
text-transform:uppercase;letter-spacing:.15em;
}
.feat-body{
padding:2.5rem;display:flex;flex-direction:column;justify-content:space-between;
border-left:1px solid var(–border);
}
.tag{
display:inline-block;
font-family:‘JetBrains Mono’,monospace;
font-size:.58rem;text-transform:uppercase;letter-spacing:.12em;
padding:.22rem .65rem;margin-bottom:1rem;font-weight:700;
}
.t-sec{background:rgba(168,85,247,.12);color:var(–violet2);border:1px solid rgba(168,85,247,.25);}
.t-net{background:rgba(56,189,248,.08);color:var(–blue);border:1px solid rgba(56,189,248,.2);}
.t-inf{background:rgba(34,211,238,.07);color:var(–cyan);border:1px solid rgba(34,211,238,.18);}
.t-cis{background:rgba(192,132,252,.08);color:var(–neon);border:1px solid rgba(192,132,252,.2);}
.t-win{background:rgba(125,211,252,.07);color:var(–blue2);border:1px solid rgba(125,211,252,.18);}

.feat-body h3{
font-family:‘Rajdhani’,sans-serif;
font-size:1.3rem;font-weight:700;
line-height:1.25;margin-bottom:.85rem;
}
.feat-body p{font-size:.9rem;color:var(–muted2);line-height:1.75;margin-bottom:1.25rem;}
.post-meta{
font-family:‘JetBrains Mono’,monospace;
font-size:.62rem;color:var(–muted);
display:flex;gap:.75rem;margin-bottom:1.25rem;flex-wrap:wrap;
}

/* POST GRID */
.post-grid{
display:grid;grid-template-columns:repeat(3,1fr);
gap:1px;background:var(–border);border:1px solid var(–border);
}
.post-card{
background:var(–surface);padding:1.85rem;cursor:none;
transition:background .2s;position:relative;overflow:hidden;
}
.post-card:hover{background:var(–surface2);}
.post-card::before{
content:’’;position:absolute;top:0;left:0;right:0;height:2px;
background:linear-gradient(90deg,var(–violet),var(–neon));
transform:scaleX(0);transform-origin:left;transition:transform .35s;
}
.post-card:hover::before{transform:scaleX(1);}
.card-num{
font-family:‘JetBrains Mono’,monospace;
font-size:.56rem;color:rgba(124,58,237,.3);
margin-bottom:1rem;letter-spacing:.1em;
}
.post-card h3{
font-size:.95rem;font-weight:700;
line-height:1.35;margin-bottom:.65rem;
}
.post-card p{font-size:.8rem;color:var(–muted2);line-height:1.65;margin-bottom:1rem;}

/* SERVICES */
.services-section{
padding:5rem 2.5rem;
background:var(–surface);
border-top:1px solid var(–border);
border-bottom:1px solid var(–border);
}
.services-section h2{
font-family:‘Orbitron’,monospace;
font-size:clamp(1.5rem,3vw,2.2rem);font-weight:900;
margin-bottom:.5rem;
}
.services-section h2 span{
background:linear-gradient(135deg,var(–neon),var(–blue2));
-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
}
.srv-sub{font-size:.95rem;color:var(–muted2);margin-bottom:2.5rem;max-width:500px;line-height:1.7;}
.srv-grid{display:grid;grid-template-columns:1fr 1fr;gap:1.5rem;max-width:820px;}
.srv-card{
border:1px solid var(–border);padding:2.25rem 2rem;
background:var(–bg2);
transition:border-color .3s,box-shadow .3s,transform .2s;
position:relative;overflow:hidden;
}
.srv-card::before{
content:’’;position:absolute;top:0;left:0;right:0;height:2px;
background:linear-gradient(90deg,var(–violet),var(–neon));
transform:scaleX(0);transform-origin:left;transition:transform .4s;
}
.srv-card:hover::before{transform:scaleX(1);}
.srv-card:hover{
border-color:var(–violet2);
box-shadow:0 0 30px rgba(124,58,237,0.2);
transform:translateY(-4px);
}
.srv-card.hot{
border-color:var(–violet2);
box-shadow:0 0 25px rgba(124,58,237,0.15);
}
.srv-card.hot::before{transform:scaleX(1);}
.srv-badge{
position:absolute;top:1.25rem;right:1.25rem;
font-family:‘JetBrains Mono’,monospace;
font-size:.55rem;font-weight:700;
background:linear-gradient(135deg,var(–violet),var(–violet2));
color:#fff;padding:.2rem .65rem;
text-transform:uppercase;letter-spacing:.1em;
box-shadow:0 0 15px rgba(124,58,237,0.4);
}
.srv-icon{font-size:2rem;margin-bottom:1rem;display:block;filter:drop-shadow(0 0 10px rgba(192,132,252,0.4));}
.srv-name{
font-family:‘Orbitron’,monospace;
font-size:1rem;font-weight:700;margin-bottom:.3rem;
color:var(–text);
}
.srv-price{
font-family:‘Orbitron’,monospace;
font-size:1.75rem;font-weight:900;
background:linear-gradient(135deg,var(–neon),var(–blue2));
-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
margin-bottom:.7rem;line-height:1;
}
.srv-price small{
font-family:‘JetBrains Mono’,monospace;
font-size:.75rem;
-webkit-text-fill-color:var(–muted);color:var(–muted);
background:none;
}
.srv-desc{font-size:.85rem;color:var(–muted2);line-height:1.7;margin-bottom:1rem;}
.srv-list{list-style:none;margin-bottom:1rem;display:flex;flex-direction:column;gap:.35rem;}
.srv-list li{font-size:.8rem;color:var(–muted2);display:flex;align-items:flex-start;gap:.5rem;}
.srv-list li::before{content:‘▸’;color:var(–violet2);flex-shrink:0;}
.srv-note{
font-family:‘JetBrains Mono’,monospace;
font-size:.6rem;color:var(–muted);
margin-bottom:1.25rem;padding:.5rem .75rem;
background:rgba(124,58,237,0.05);
border-left:2px solid var(–border2);line-height:1.6;
}
.srv-btn{
display:block;width:100%;text-align:center;
font-family:‘Orbitron’,monospace;
font-size:.65rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;
color:var(–text);
background:linear-gradient(135deg,var(–violet),var(–violet2));
padding:.8rem;text-decoration:none;cursor:none;
border:none;
clip-path:polygon(6px 0%,100% 0%,calc(100% - 6px) 100%,0% 100%);
box-shadow:0 0 20px rgba(124,58,237,0.3);
transition:box-shadow .2s,filter .2s;
}
.srv-btn:hover{box-shadow:0 0 35px rgba(124,58,237,0.6);filter:brightness(1.1);}
.srv-btn.ghost{
background:transparent;color:var(–neon);
border:1px solid var(–border2);
clip-path:none;
box-shadow:0 0 10px rgba(192,132,252,0.1);
}
.srv-btn.ghost:hover{background:rgba(124,58,237,0.15);box-shadow:0 0 20px rgba(192,132,252,0.25);}

.kofi-strip{
border:1px solid var(–border);padding:1.75rem 2rem;margin-top:1.75rem;
display:flex;align-items:center;justify-content:space-between;gap:2rem;flex-wrap:wrap;
background:rgba(13,7,32,0.5);
}
.kofi-strip p{font-size:.9rem;color:var(–muted2);line-height:1.6;}
.kofi-strip strong{color:var(–neon);}
.kofi-btn{
font-family:‘JetBrains Mono’,monospace;
font-size:.68rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;
color:#fff;background:#ff5e5b;
padding:.65rem 1.4rem;text-decoration:none;cursor:none;
transition:box-shadow .2s;white-space:nowrap;
box-shadow:0 0 15px rgba(255,94,91,0.3);
}
.kofi-btn:hover{box-shadow:0 0 30px rgba(255,94,91,0.6);}

/* FAQ */
.faq-section{
padding:5rem 2.5rem;
border-bottom:1px solid var(–border);
}
.faq-section>h2{
font-family:‘Orbitron’,monospace;
font-size:clamp(1.5rem,3vw,2.2rem);font-weight:900;
margin-bottom:2.5rem;text-align:center;
}
.faq-section>h2 span{
background:linear-gradient(135deg,var(–neon),var(–blue2));
-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
}
.faq-list{
max-width:760px;margin:0 auto;
border:1px solid var(–border);border-bottom:none;
}
.faq-item{border-bottom:1px solid var(–border);overflow:hidden;}
.faq-question{
width:100%;background:none;border:none;
padding:1.35rem 1.75rem;
display:flex;align-items:center;justify-content:space-between;gap:1rem;
cursor:none;text-align:left;transition:background .2s;
}
.faq-question:hover{background:rgba(124,58,237,0.06);}
.faq-q-text{font-family:‘Rajdhani’,sans-serif;font-size:1rem;font-weight:700;color:var(–text);}
.faq-arrow{
font-family:‘JetBrains Mono’,monospace;
font-size:.9rem;color:var(–neon);
transition:transform .3s;flex-shrink:0;
text-shadow:0 0 10px rgba(192,132,252,0.5);
}
.faq-item.open .faq-arrow{transform:rotate(90deg);}
.faq-answer{max-height:0;overflow:hidden;transition:max-height .35s ease;}
.faq-item.open .faq-answer{max-height:200px;}
.faq-a-inner{
padding:0 1.75rem 1.35rem;
font-size:.9rem;color:var(–muted2);line-height:1.8;font-weight:500;
}

/* MODAL */
.modal-overlay{
position:fixed;inset:0;background:rgba(3,1,10,.85);
z-index:1000;display:none;align-items:center;justify-content:center;
backdrop-filter:blur(10px);
}
.modal-overlay.open{display:flex;}
.modal{
background:var(–surface2);
border:1px solid var(–border2);
padding:2.75rem;max-width:500px;width:92%;
position:relative;animation:fadeUp .3s ease both;
box-shadow:0 0 60px rgba(124,58,237,0.3),0 0 120px rgba(124,58,237,0.1);
}
.modal::before{
content:’’;position:absolute;top:0;left:0;right:0;height:2px;
background:linear-gradient(90deg,var(–violet),var(–neon),var(–blue));
}
.modal-close{
position:absolute;top:1.25rem;right:1.25rem;
background:none;border:none;font-size:1.1rem;
color:var(–muted);cursor:none;transition:color .2s;
}
.modal-close:hover{color:var(–neon);}
.modal h3{
font-family:‘Orbitron’,monospace;
font-size:1.5rem;font-weight:900;margin-bottom:.4rem;
}
.modal-sub{font-size:.88rem;color:var(–muted2);margin-bottom:1.75rem;line-height:1.6;}
.form-group{margin-bottom:1rem;}
.form-group label{
display:block;font-family:‘JetBrains Mono’,monospace;
font-size:.58rem;color:var(–muted);
text-transform:uppercase;letter-spacing:.15em;margin-bottom:.35rem;
}
.form-input,.form-textarea,.form-select{
width:100%;background:rgba(13,7,32,0.8);
border:1px solid var(–border);
padding:.75rem 1rem;color:var(–text);
font-family:‘Rajdhani’,sans-serif;font-size:.95rem;
outline:none;transition:border-color .2s,box-shadow .2s;cursor:text;
}
.form-input:focus,.form-textarea:focus,.form-select:focus{
border-color:var(–violet2);
box-shadow:0 0 15px rgba(124,58,237,0.2);
}
.form-textarea{resize:vertical;min-height:90px;}
.form-select{
cursor:none;-webkit-appearance:none;
background-image:url(“data:image/svg+xml,%3Csvg xmlns=‘http://www.w3.org/2000/svg’ width=‘12’ height=‘8’ viewBox=‘0 0 12 8’%3E%3Cpath d=‘M1 1l5 5 5-5’ stroke=’%237c3aed’ stroke-width=‘1.5’ fill=‘none’/%3E%3C/svg%3E”);
background-repeat:no-repeat;background-position:right 1rem center;
}
.form-submit{
width:100%;border:none;padding:1rem;
font-family:‘Orbitron’,monospace;
font-size:.7rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;
color:var(–text);
background:linear-gradient(135deg,var(–violet),var(–violet2));
cursor:none;transition:box-shadow .2s,filter .2s;margin-top:.5rem;
box-shadow:0 0 20px rgba(124,58,237,0.3);
}
.form-submit:hover{box-shadow:0 0 40px rgba(124,58,237,0.6);filter:brightness(1.1);}
.form-msg{font-family:‘JetBrains Mono’,monospace;font-size:.7rem;margin-top:.75rem;padding:.6rem;display:none;}
.form-msg.success{display:block;background:rgba(34,197,94,.08);border-left:2px solid #22c55e;color:#4ade80;}
.form-msg.error{display:block;background:rgba(239,68,68,.08);border-left:2px solid #ef4444;color:#f87171;}

/* NEWSLETTER */
.newsletter-section{
padding:5rem 2.5rem;
display:grid;grid-template-columns:1fr 1fr;gap:5rem;align-items:center;
border-bottom:1px solid var(–border);
}
.newsletter-section h2{
font-family:‘Orbitron’,monospace;
font-size:clamp(1.5rem,3vw,2.2rem);font-weight:900;
line-height:1.1;margin-bottom:1rem;
}
.newsletter-section h2 span{
background:linear-gradient(135deg,var(–neon),var(–blue2));
-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
}
.newsletter-section p{font-size:.95rem;color:var(–muted2);line-height:1.85;font-weight:500;}
.email-form{display:flex;flex-direction:column;gap:1rem;}
.email-wrap{
display:flex;
border:1px solid var(–border2);overflow:hidden;
transition:border-color .2s,box-shadow .2s;
}
.email-wrap:focus-within{
border-color:var(–violet2);
box-shadow:0 0 20px rgba(124,58,237,0.25);
}
.email-input{
flex:1;background:var(–surface);border:none;
padding:.9rem 1.1rem;color:var(–text);
font-family:‘Rajdhani’,sans-serif;font-size:.95rem;outline:none;cursor:text;
}
.email-input::placeholder{color:var(–muted);}
.email-btn{
background:linear-gradient(135deg,var(–violet),var(–violet2));
border:none;padding:.9rem 1.35rem;
font-family:‘JetBrains Mono’,monospace;
font-size:.65rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;
color:var(–text);cursor:none;transition:filter .2s,box-shadow .2s;
}
.email-btn:hover{filter:brightness(1.1);box-shadow:0 0 20px rgba(124,58,237,0.4);}
.email-note{font-family:‘JetBrains Mono’,monospace;font-size:.6rem;color:var(–muted);text-transform:uppercase;letter-spacing:.1em;}
.sub-count{font-family:‘JetBrains Mono’,monospace;font-size:.68rem;color:var(–neon);margin-top:.2rem;text-shadow:0 0 10px rgba(192,132,252,0.4);}

/* FOOTER */
footer{
background:var(–bg2);
padding:2.5rem;
display:flex;align-items:center;justify-content:space-between;
border-top:1px solid var(–border);
position:relative;
}
footer::before{
content:’’;position:absolute;top:0;left:0;right:0;height:1px;
background:linear-gradient(90deg,transparent,var(–violet),var(–neon),var(–blue),transparent);
}
.footer-logo{
font-family:‘Orbitron’,monospace;
font-size:.9rem;font-weight:900;color:var(–text);
text-shadow:0 0 20px rgba(192,132,252,0.3);
}
.footer-logo .dot{color:var(–neon);}
.footer-links{display:flex;gap:1.75rem;list-style:none;}
.footer-links a{
font-family:‘JetBrains Mono’,monospace;
font-size:.6rem;color:var(–muted);text-decoration:none;
text-transform:uppercase;letter-spacing:.12em;cursor:none;transition:color .2s,text-shadow .2s;
}
.footer-links a:hover{color:var(–neon);text-shadow:0 0 10px rgba(192,132,252,0.4);}
.footer-copy{font-family:‘JetBrains Mono’,monospace;font-size:.58rem;color:var(–muted);}

/* READING BAR */
.reading-bar{
position:fixed;top:63px;left:0;height:2px;
background:linear-gradient(90deg,var(–violet),var(–neon),var(–blue));
z-index:499;width:0%;
box-shadow:0 0 10px rgba(192,132,252,0.5);
}

/* BACK TOP */
.back-top{
position:fixed;bottom:2rem;right:2rem;
width:44px;height:44px;
background:linear-gradient(135deg,var(–violet),var(–violet2));
color:var(–text);border:none;font-size:1rem;cursor:none;
display:none;align-items:center;justify-content:center;z-index:200;
box-shadow:0 0 20px rgba(124,58,237,0.4);
transition:box-shadow .2s;
clip-path:polygon(6px 0%,100% 0%,calc(100% - 6px) 100%,0% 100%);
}
.back-top.visible{display:flex;}
.back-top:hover{box-shadow:0 0 35px rgba(124,58,237,0.7);}

/* TOAST */
.toast{
position:fixed;bottom:2rem;left:50%;
transform:translateX(-50%) translateY(80px);
background:var(–surface2);
border:1px solid var(–violet2);
color:var(–neon);
padding:.75rem 1.5rem;
font-family:‘JetBrains Mono’,monospace;font-size:.72rem;
z-index:2000;transition:transform .3s ease;white-space:nowrap;
box-shadow:0 0 25px rgba(124,58,237,0.3);
}
.toast.show{transform:translateX(-50%) translateY(0);}

/* ── ARTICLE ── */
.article-wrapper{
max-width:720px;margin:0 auto;
padding:8rem 2rem 7rem;
animation:fadeUp .5s ease both;
}
.back-link{
display:inline-flex;align-items:center;gap:.5rem;
font-family:‘JetBrains Mono’,monospace;
font-size:.65rem;color:var(–muted);text-decoration:none;
text-transform:uppercase;letter-spacing:.12em;
margin-bottom:3rem;cursor:none;transition:color .2s;
}
.back-link:hover{color:var(–neon);}
.article-tags{display:flex;gap:.5rem;margin-bottom:1.5rem;}
.article-wrapper>h1{
font-family:‘Rajdhani’,sans-serif;
font-size:clamp(1.75rem,5vw,3rem);
font-weight:700;letter-spacing:-.01em;line-height:1.1;margin-bottom:1.25rem;
}
.art-meta{
display:flex;gap:1.5rem;flex-wrap:wrap;
padding:1rem 0;
border-top:1px solid var(–border);border-bottom:1px solid var(–border);
margin-bottom:3rem;
font-family:‘JetBrains Mono’,monospace;font-size:.65rem;color:var(–muted);
}
.art-body h2{
font-family:‘Orbitron’,monospace;
font-size:1.2rem;font-weight:700;
margin:2.75rem 0 .9rem;
background:linear-gradient(135deg,var(–neon),var(–blue2));
-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;
}
.art-body p{font-size:.97rem;line-height:1.9;color:var(–muted2);margin-bottom:1.25rem;font-weight:500;}
.art-body strong{color:var(–neon);}
.art-body code{
font-family:‘JetBrains Mono’,monospace;
font-size:.82em;background:var(–surface);
color:var(–neon);padding:.12rem .38rem;
border:1px solid var(–border);
}
.codeblock{
background:rgba(3,1,10,.95);border:1px solid var(–border2);
padding:1.75rem;margin:1.75rem 0;overflow-x:auto;position:relative;
box-shadow:0 0 30px rgba(124,58,237,0.15),inset 0 0 30px rgba(124,58,237,0.03);
}
.codeblock::before{
content:’’;position:absolute;top:0;left:0;right:0;height:2px;
background:linear-gradient(90deg,var(–violet),var(–neon));
}
.codeblock-lang{
position:absolute;top:.7rem;right:1rem;
font-family:‘JetBrains Mono’,monospace;
font-size:.58rem;color:rgba(124,58,237,.5);
text-transform:uppercase;letter-spacing:.12em;
}
.codeblock pre{
font-family:‘JetBrains Mono’,monospace;
font-size:.8rem;line-height:1.75;color:#a5d8a0;
}
.callout{
border-left:2px solid var(–violet2);
background:rgba(124,58,237,.07);
padding:1.1rem 1.5rem;margin:1.75rem 0;
font-size:.92rem;line-height:1.8;color:var(–text);font-weight:500;
box-shadow:inset 0 0 20px rgba(124,58,237,.03);
}
.callout.warn{
border-left-color:var(–cyan);
background:rgba(34,211,238,.05);
}

/* ANIMATIONS */
@keyframes marquee{from{transform:translateX(0)}to{transform:translateX(-50%)}}
@keyframes fadeUp{from{opacity:0;transform:translateY(24px)}to{opacity:1;transform:translateY(0)}}

/* GLITCH anim for logo on hover */
.logo:hover{animation:glitch .3s ease;}
@keyframes glitch{
0%{text-shadow:0 0 20px var(–neon);}
20%{text-shadow:-2px 0 var(–violet2),2px 0 var(–blue);}
40%{text-shadow:2px 0 var(–violet2),-2px 0 var(–blue);}
60%{text-shadow:-1px 0 var(–neon),1px 0 var(–violet2);}
100%{text-shadow:0 0 20px var(–neon);}
}

/* RESPONSIVE */
@media(max-width:900px){
nav{padding:1rem 1.25rem;}
.nav-links{display:none;}
.hero{padding:7rem 1.25rem 4rem;}
.orb{display:none;}
.hero-stats{width:100%;overflow-x:auto;}
.about-section{grid-template-columns:1fr;}
.about-left{border-right:none;border-bottom:1px solid var(–border);padding:3.5rem 1.25rem;}
.about-right{padding:3rem 1.25rem;}
.articles-section,.faq-section,.newsletter-section{padding:3.5rem 1.25rem;}
.featured{grid-template-columns:1fr;}
.feat-visual{min-height:180px;}
.feat-body{border-left:none;border-top:1px solid var(–border);padding:1.75rem 1.25rem;}
.post-grid{grid-template-columns:1fr;}
.services-section{padding:3.5rem 1.25rem;}
.srv-grid{grid-template-columns:1fr;}
.newsletter-section{grid-template-columns:1fr;gap:2.5rem;}
footer{flex-direction:column;gap:1.25rem;text-align:center;padding:2rem 1.25rem;}
.footer-links{flex-wrap:wrap;justify-content:center;}
.article-wrapper{padding:7rem 1.25rem 4rem;}
}
</style>

</head>
<body>
<div class="scanline"></div>
<div class="cur" id="cur"></div>
<div class="cur-dot" id="curDot"></div>
<div class="reading-bar" id="readingBar"></div>
<button class="back-top" id="backTop" onclick="window.scrollTo({top:0,behavior:'smooth'})">↑</button>
<div class="toast" id="toast"></div>

<!-- MODAL -->

<div class="modal-overlay" id="contactModal">
  <div class="modal">
    <button class="modal-close" onclick="closeModal()">✕</button>
    <h3>Richiedi Consulenza</h3>
    <p class="modal-sub">Compila il form — ti apro WhatsApp con il messaggio già pronto.</p>
    <div class="form-group">
      <label>Nome *</label>
      <input class="form-input" id="c-name" type="text" placeholder="Mario Rossi">
    </div>
    <div class="form-group">
      <label>Email *</label>
      <input class="form-input" id="c-email" type="email" placeholder="mario@email.com">
    </div>
    <div class="form-group">
      <label>Tipo richiesta</label>
      <select class="form-select" id="c-type">
        <option value="">Seleziona...</option>
        <option value="Consulenza rete">Consulenza rete / firewall</option>
        <option value="Audit rete">Audit rete domestica / ufficio</option>
        <option value="Altro">Altro</option>
      </select>
    </div>
    <div class="form-group">
      <label>Descrivi il problema</label>
      <textarea class="form-textarea" id="c-msg" placeholder="Es: ho problemi di connettività nel mio ufficio..."></textarea>
    </div>
    <button class="form-submit" onclick="submitContact()">Invia via WhatsApp →</button>
    <div class="form-msg" id="form-msg"></div>
  </div>
</div>

<nav>
  <a class="logo" href="#" onclick="goHome()">Alessandro<span class="dot">.</span>IT</a>
  <ul class="nav-links">
    <li><a href="#" onclick="goHome()">Blog</a></li>
    <li><a href="#" onclick="goHome();setTimeout(()=>scrollTo('servizi'),80)">Servizi</a></li>
    <li><a href="#" onclick="goHome();setTimeout(()=>scrollTo('faq'),80)">FAQ</a></li>
    <li><a href="#" onclick="goHome();setTimeout(()=>scrollTo('newsletter'),80)">Newsletter</a></li>
  </ul>
  <a class="nav-cta" href="#" onclick="openModal()">Contattami</a>
</nav>

<!-- HOME -->

<div id="page-home" class="page active">
<section class="hero">
  <div class="orb orb1"></div><div class="orb orb2"></div><div class="orb orb3"></div>
  <div class="hero-eyebrow">
    <div class="eyebrow-line"></div>
    <span class="eyebrow-txt">IT Network Specialist · Banca Mediolanum · Milano</span>
  </div>
  <h1>
    <span class="line-outline">NETWORKING</span>
    <span class="line-glow">SICUREZZA</span>
    <span class="line-solid">DAL CAMPO.</span>
  </h1>
  <p class="hero-desc">
    Guide tecniche <strong>vere</strong>, scritte da chi configura firewall e VPN ogni giorno in ambienti enterprise bancari. <strong>Check Point, Cisco, LAN/WAN:</strong> quello che non trovi sui libri.
  </p>
  <div class="hero-btns">
    <button class="btn-primary" onclick="document.querySelector('.articles-section').scrollIntoView({behavior:'smooth'})">Leggi le guide</button>
    <button class="btn-ghost" onclick="scrollTo('servizi')">I miei servizi</button>
  </div>
  <div class="hero-stats">
    <div class="hero-stat"><div class="stat-num">1</div><div class="stat-lbl">Banca enterprise</div></div>
    <div class="hero-stat"><div class="stat-num">6+</div><div class="stat-lbl">Mesi in produzione</div></div>
    <div class="hero-stat"><div class="stat-num">0</div><div class="stat-lbl">Contenuti copiati</div></div>
    <div class="hero-stat"><div class="stat-num">∞</div><div class="stat-lbl">Guide gratuite</div></div>
  </div>
</section>

<div class="marquee-wrap">
  <div class="marquee-track">
    <div class="mi"><span class="d"></span>Check Point R81.20</div><div class="mi"><span class="d"></span>VPN Site-to-Site</div>
    <div class="mi"><span class="d"></span>VLAN 802.1Q</div><div class="mi"><span class="d"></span>Cisco IOS</div>
    <div class="mi"><span class="d"></span>TCP/IP</div><div class="mi"><span class="d"></span>DNS · DHCP · NAT</div>
    <div class="mi"><span class="d"></span>Firewall Policy</div><div class="mi"><span class="d"></span>LAN / WAN</div>
    <div class="mi"><span class="d"></span>Wireshark</div><div class="mi"><span class="d"></span>Windows Server</div>
    <div class="mi"><span class="d"></span>Linux</div><div class="mi"><span class="d"></span>Packet Tracer</div>
    <div class="mi"><span class="d"></span>Check Point R81.20</div><div class="mi"><span class="d"></span>VPN Site-to-Site</div>
    <div class="mi"><span class="d"></span>VLAN 802.1Q</div><div class="mi"><span class="d"></span>Cisco IOS</div>
    <div class="mi"><span class="d"></span>TCP/IP</div><div class="mi"><span class="d"></span>DNS · DHCP · NAT</div>
    <div class="mi"><span class="d"></span>Firewall Policy</div><div class="mi"><span class="d"></span>LAN / WAN</div>
    <div class="mi"><span class="d"></span>Wireshark</div><div class="mi"><span class="d"></span>Windows Server</div>
    <div class="mi"><span class="d"></span>Linux</div><div class="mi"><span class="d"></span>Packet Tracer</div>
  </div>
</div>

<section class="about-section">
  <div class="about-left">
    <div class="slabel">// chi sono</div>
    <h2><span class="glow-txt">Network Specialist.</span><br>Esperienza enterprise reale.</h2>
    <p>Sono Alessandro Passarello, IT Network Specialist con esperienza diretta in ambiente bancario enterprise presso <strong>Banca Mediolanum</strong>. Gestisco firewall Check Point, VPN e infrastrutture di rete in produzione ogni giorno.</p>
    <p>Questo blog nasce dalle mie note di campo — quello che imparo trasformato in guide che vorrei aver trovato io. Zero copia-incolla.</p>
  </div>
  <div class="about-right">
    <div><div class="slabel">// stack tecnico</div>
    <div class="skill-pills">
      <span class="sp">Check Point</span><span class="sp">VPN IPSec/SSL</span><span class="sp">VLAN 802.1Q</span>
      <span class="sp">Cisco IOS</span><span class="sp">TCP/IP</span><span class="sp">DNS · DHCP · NAT</span>
      <span class="sp">LAN / WAN</span><span class="sp">Windows Server</span><span class="sp">Linux</span>
      <span class="sp">Python</span><span class="sp">Wireshark</span><span class="sp">Packet Tracer</span>
    </div></div>
    <div class="stats-grid">
      <div class="stat-cell"><div class="stat-v">1</div><div class="stat-k">Banca enterprise</div></div>
      <div class="stat-cell"><div class="stat-v">6+</div><div class="stat-k">Mesi in prod.</div></div>
      <div class="stat-cell"><div class="stat-v">0</div><div class="stat-k">Fluff</div></div>
    </div>
  </div>
</section>

<section class="articles-section">
  <div class="sec-header"><h2>Guide &amp; <span>Articoli</span></h2></div>
  <div class="filter-wrap">
    <button class="filter-btn active" onclick="filterPosts('all',this)">Tutti</button>
    <button class="filter-btn" onclick="filterPosts('security',this)">Security</button>
    <button class="filter-btn" onclick="filterPosts('networking',this)">Networking</button>
    <button class="filter-btn" onclick="filterPosts('cisco',this)">Cisco</button>
    <button class="filter-btn" onclick="filterPosts('infra',this)">Infrastruttura</button>
  </div>
  <div class="featured" id="featured-post" data-cat="security" onclick="showArticle('checkpoint')">
    <div class="feat-visual">
      <div class="feat-corner">Featured · Mag 2025</div>
      <div class="feat-icon">🔐</div>
    </div>
    <div class="feat-body">
      <div>
        <span class="tag t-sec">Security</span>
        <h3>Check Point: configurare policy Access Control e VPN site-to-site da zero</h3>
        <p>Dalla creazione degli oggetti di rete al debug del tunnel IPSec. Come lo faccio ogni giorno in produzione a Banca Mediolanum.</p>
        <div class="post-meta"><span>15 Mag 2025</span><span>·</span><span>8 min</span><span>·</span><span>Check Point · IPSec</span></div>
      </div>
      <button class="btn-primary" onclick="showArticle('checkpoint')">Leggi →</button>
    </div>
  </div>
  <div class="post-grid" id="postGrid">
    <div class="post-card" data-cat="networking" onclick="showArticle('vlan')">
      <div class="card-num">// 001</div><span class="tag t-net">Networking</span>
      <h3>VLAN e trunking su Cisco: configurazione pratica</h3>
      <p>802.1Q, porte access e trunk, inter-VLAN routing con esempi reali su IOS.</p>
      <div class="post-meta"><span>10 Mag 2025</span><span>·</span><span>6 min</span></div>
    </div>
    <div class="post-card" data-cat="cisco" onclick="showArticle('nat')">
      <div class="card-num">// 002</div><span class="tag t-cis">Cisco</span>
      <h3>NAT e PAT spiegati bene: config e casi d'uso</h3>
      <p>NAT statico, dinamico e PAT con comandi show, debug e gli errori classici.</p>
      <div class="post-meta"><span>3 Mag 2025</span><span>·</span><span>7 min</span></div>
    </div>
    <div class="post-card" data-cat="infra" onclick="showArticle('incident')">
      <div class="card-num">// 003</div><span class="tag t-inf">Infrastruttura</span>
      <h3>Rack e ridondanza: come un alimentatore ha fermato un servizio</h3>
      <p>Caso reale di incident in produzione. Cosa è successo e come l'ho gestito.</p>
      <div class="post-meta"><span>28 Apr 2025</span><span>·</span><span>5 min</span></div>
    </div>
    <div class="post-card" data-cat="networking" onclick="showArticle('wireshark')">
      <div class="card-num">// 004</div><span class="tag t-net">Networking</span>
      <h3>Wireshark: i filtri che uso davvero ogni giorno</h3>
      <p>Display filter per troubleshooting reale su reti enterprise.</p>
      <div class="post-meta"><span>20 Apr 2025</span><span>·</span><span>5 min</span></div>
    </div>
    <div class="post-card" data-cat="networking" onclick="showArticle('dns')">
      <div class="card-num">// 005</div><span class="tag t-win">Windows/Linux</span>
      <h3>DNS troubleshooting: metodo e comandi pratici</h3>
      <p>nslookup, dig, ipconfig /flushdns — quando usarli e come interpretarli.</p>
      <div class="post-meta"><span>14 Apr 2025</span><span>·</span><span>6 min</span></div>
    </div>
    <div class="post-card" data-cat="security" onclick="showArticle('checkpoint')">
      <div class="card-num">// 006</div><span class="tag t-sec">Security</span>
      <h3>TCP/IP: il three-way handshake spiegato davvero</h3>
      <p>SYN, SYN-ACK, ACK con Wireshark. Essenziale per il troubleshooting.</p>
      <div class="post-meta"><span>7 Apr 2025</span><span>·</span><span>7 min</span></div>
    </div>
  </div>
</section>

<section class="services-section" id="servizi">
  <div class="slabel">// cosa offro</div>
  <h2>Servizi <span>Onesti</span></h2>
  <p class="srv-sub">Solo cose che so fare davvero. Prezzi trasparenti, niente sorprese. Ideale per reti domestiche, piccoli uffici e PMI.</p>
  <div class="srv-grid">
    <div class="srv-card">
      <span class="srv-icon">🔧</span>
      <div class="srv-name">Consulenza Rete</div>
      <div class="srv-price">25€ <small>/ ora</small></div>
      <div class="srv-desc">Supporto tecnico su reti LAN/WAN, VLAN, switch e troubleshooting. Da remoto o area Milano.</div>
      <ul class="srv-list">
        <li>Analisi e diagnosi problemi di rete</li>
        <li>Configurazione VLAN e switch</li>
        <li>Troubleshooting connettività</li>
        <li>Via call o in presenza (Milano)</li>
      </ul>
      <div class="srv-note">⚠ Per reti domestiche e piccoli uffici. Non gestisco ambienti enterprise con SLA critica.</div>
      <button class="srv-btn ghost" onclick="openModal()">Richiedi consulenza</button>
    </div>
    <div class="srv-card hot">
      <div class="srv-badge">⚡ più richiesto</div>
      <span class="srv-icon">🛡️</span>
      <div class="srv-name">Audit Rete Base</div>
      <div class="srv-price">50€ <small>/ sessione</small></div>
      <div class="srv-desc">Analisi della tua rete: sicurezza Wi-Fi, configurazione router, dispositivi connessi. Report PDF incluso.</div>
      <ul class="srv-list">
        <li>Analisi sicurezza Wi-Fi</li>
        <li>Verifica configurazione router</li>
        <li>Identificazione dispositivi connessi</li>
        <li>Report PDF con consigli pratici</li>
        <li>100% disponibile da remoto</li>
      </ul>
      <div class="srv-note">⚠ Per reti domestiche e piccoli uffici (max ~20 dispositivi).</div>
      <button class="srv-btn" onclick="openModal()">Prenota audit →</button>
    </div>
  </div>
  <div class="kofi-strip">
    <p>☕ <strong>Le guide ti sono utili?</strong> Supporta il blog con un caffè su Ko-fi. Mantiene tutto gratuito.</p>
    <a class="kofi-btn" href="https://ko-fi.com" target="_blank" rel="noopener">Offrimi un caffè →</a>
  </div>
</section>

<section class="faq-section" id="faq">
  <h2>Domande <span>Frequenti</span></h2>
  <div class="faq-list">
    <div class="faq-item">
      <button class="faq-question" onclick="toggleFaq(this)"><span class="faq-q-text">Fai assistenza da remoto?</span><span class="faq-arrow">›</span></button>
      <div class="faq-answer"><div class="faq-a-inner">Sì, per la maggior parte delle consulenze uso call + condivisione schermo. Per sessioni in presenza sono disponibile nell'area di Milano.</div></div>
    </div>
    <div class="faq-item">
      <button class="faq-question" onclick="toggleFaq(this)"><span class="faq-q-text">Quanto dura una sessione?</span><span class="faq-arrow">›</span></button>
      <div class="faq-answer"><div class="faq-a-inner">Dipende dal problema. Di solito 1-2 ore bastano. Ti faccio sempre un preventivo prima, nessuna sorpresa.</div></div>
    </div>
    <div class="faq-item">
      <button class="faq-question" onclick="toggleFaq(this)"><span class="faq-q-text">Come avviene il pagamento?</span><span class="faq-arrow">›</span></button>
      <div class="faq-answer"><div class="faq-a-inner">Accetto bonifico o PayPal a fine sessione. Per l'audit: 50% alla prenotazione e 50% alla consegna del report.</div></div>
    </div>
    <div class="faq-item">
      <button class="faq-question" onclick="toggleFaq(this)"><span class="faq-q-text">Puoi aiutarmi con il router di casa?</span><span class="faq-arrow">›</span></button>
      <div class="faq-answer"><div class="faq-a-inner">Sì, è esattamente per questo tipo di intervento che mi propongo. Configurazione router, Wi-Fi, connettività e sicurezza di base.</div></div>
    </div>
    <div class="faq-item">
      <button class="faq-question" onclick="toggleFaq(this)"><span class="faq-q-text">Le guide sono davvero gratis?</span><span class="faq-arrow">›</span></button>
      <div class="faq-answer"><div class="faq-a-inner">Sì, tutto il contenuto del blog è gratuito. Si mantiene grazie a Ko-fi e ai servizi. Nessun paywall, mai.</div></div>
    </div>
    <div class="faq-item">
      <button class="faq-question" onclick="toggleFaq(this)"><span class="faq-q-text">Puoi aiutarmi su Check Point enterprise?</span><span class="faq-arrow">›</span></button>
      <div class="faq-answer"><div class="faq-a-inner">Ho esperienza su Check Point in ambiente bancario. Per aspetti specifici posso aiutarti, ma per ambienti enterprise con SLA critica non mi propongo come gestore principale. Parliamone prima.</div></div>
    </div>
  </div>
</section>

<section class="newsletter-section" id="newsletter">
  <div>
    <div class="slabel">// gratis</div>
    <h2>Una guida ogni settimana.<br><span>Zero spam.</span></h2>
    <p>Solo contenuti tecnici: networking, sicurezza, firewall. Scritti da chi ci lavora sul campo. Niente marketing.</p>
  </div>
  <div class="email-form">
    <div>
      <div style="font-family:'Orbitron',monospace;font-size:.8rem;font-weight:700;margin-bottom:.3rem;">Iscriviti alla newsletter</div>
      <div class="sub-count" id="subCount">// 0 iscritti</div>
    </div>
    <div class="email-wrap" id="emailWrap">
      <input class="email-input" id="emailInput" type="email" placeholder="la-tua@email.com">
      <button class="email-btn" onclick="handleNewsletter()">Iscriviti →</button>
    </div>
    <div class="email-note" id="emailNote">// Zero spam · Disiscrizione in 1 click</div>
  </div>
</section>

<footer>
  <div class="footer-logo">Alessandro<span class="dot">.</span>IT</div>
  <ul class="footer-links">
    <li><a href="#" onclick="goHome()">Blog</a></li>
    <li><a href="#" onclick="scrollTo('servizi')">Servizi</a></li>
    <li><a href="https://linkedin.com/in/alessandro-passarello" target="_blank" rel="noopener">LinkedIn</a></li>
    <li><a href="https://ko-fi.com" target="_blank" rel="noopener">Ko-fi</a></li>
    <li><a href="https://wa.me/393441305820" target="_blank" rel="noopener">WhatsApp</a></li>
  </ul>
  <div class="footer-copy">© 2025 Alessandro Passarello · Milano</div>
</footer>
</div>

<!-- ARTICLES -->

<div id="page-checkpoint" class="page"><div class="article-wrapper">
  <a class="back-link" href="#" onclick="goHome()">Torna al blog</a>
  <div class="article-tags"><span class="tag t-sec">Security</span><span class="tag t-net">Networking</span></div>
  <h1>Check Point: configurare policy Access Control e VPN site-to-site da zero</h1>
  <div class="art-meta"><span>15 Maggio 2025</span><span>·</span><span>8 min</span><span>·</span><span>Check Point · IPSec · R81.20</span></div>
  <div class="art-body">
    <p>Questa guida nasce da esperienza diretta. La prima volta che ho configurato un tunnel VPN site-to-site in produzione su Check Point ci ho messo il doppio del necessario. Ecco quello che avrei voluto leggere.</p>
    <h2>Prerequisiti</h2>
    <p>Servono: accesso a <strong>SmartConsole</strong> con permessi admin, IP del Security Management Server, credenziali dei due gateway. Guida per <strong>Check Point R81.20</strong>.</p>
    <div class="callout">💡 Se lavori su un gateway in produzione, assicurati di avere una finestra di manutenzione concordata prima di installare nuove policy.</div>
    <h2>1. Access Control Policy</h2>
    <p>Da SmartConsole: <code>Security Policies → Access Control → Policy</code>. Le regole si applicano <strong>dall'alto verso il basso</strong>.</p>
    <div class="codeblock"><span class="codeblock-lang">Policy</span><pre>Rule 1 — Stealth
  Src: Any  →  Dst: [GW_Object]  →  Drop + Log
Rule 2 — Outbound
  Src: Internal_Net  →  Svc: HTTP,HTTPS,DNS  →  Accept + Log
Rule N — Cleanup
  Src: Any  →  Dst: Any  →  Drop + Log</pre></div>
    <h2>2. VPN site-to-site IPSec</h2>
    <p>Vai su <code>VPN → Site To Site → New VPN Community → Meshed</code>. Aggiungi i gateway e configura le encryption domain.</p>
    <div class="codeblock"><span class="codeblock-lang">IPSec</span><pre>IKE Phase 1: AES-256 | SHA-256 | Group 14 | 86400s
IPSec Phase 2: AES-256 | SHA-256 | PFS Group 14 | 3600s</pre></div>
    <h2>3. Installazione e verifica</h2>
    <p>Usa sempre <strong>Verify Policy</strong> prima di installare. Verifica da <code>Logs & Monitor → VPN Tunnels</code>.</p>
    <div class="callout warn">⚠ L'installazione può causare una breve interruzione. In ambienti critici, avvisa il team prima.</div>
    <h2>Troubleshooting CLI</h2>
    <div class="codeblock"><span class="codeblock-lang">CLI</span><pre>vpn tu                          # stato tunnel
vpn debug ikeon                 # debug IKE
tail -f $FWDIR/log/ike.elg      # log in tempo reale</pre></div>
    <p>Il 90% dei problemi: PSK diversa, encryption domain errata, routing mancante. Controlla in quest'ordine.</p>
  </div>
</div></div>

<div id="page-vlan" class="page"><div class="article-wrapper">
  <a class="back-link" href="#" onclick="goHome()">Torna al blog</a>
  <div class="article-tags"><span class="tag t-net">Networking</span><span class="tag t-cis">Cisco</span></div>
  <h1>VLAN e trunking su Cisco: configurazione pratica passo per passo</h1>
  <div class="art-meta"><span>10 Maggio 2025</span><span>·</span><span>6 min</span><span>·</span><span>Cisco IOS · VLAN · 802.1Q</span></div>
  <div class="art-body">
    <p>Le VLAN sono fondamentali per segmentare una rete aziendale. Configurazione completa su Cisco IOS dalla creazione al trunking.</p>
    <h2>Creare VLAN</h2>
    <div class="codeblock"><span class="codeblock-lang">IOS</span><pre>Switch(config)# vlan 10
Switch(config-vlan)# name UFFICI
Switch(config)# vlan 20
Switch(config-vlan)# name SERVER</pre></div>
    <h2>Porte access e trunk</h2>
    <div class="codeblock"><span class="codeblock-lang">IOS</span><pre># Access (un solo VLAN)
interface fa0/1
 switchport mode access
 switchport access vlan 10
# Trunk (più VLAN)
interface gig0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20</pre></div>
    <div class="callout">💡 Verifica con <code>show vlan brief</code> e <code>show interfaces trunk</code> dopo ogni modifica.</div>
    <h2>Inter-VLAN routing</h2>
    <div class="codeblock"><span class="codeblock-lang">IOS Router</span><pre>interface gig0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
interface gig0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0</pre></div>
  </div>
</div></div>

<div id="page-nat" class="page"><div class="article-wrapper">
  <a class="back-link" href="#" onclick="goHome()">Torna al blog</a>
  <div class="article-tags"><span class="tag t-cis">Cisco</span></div>
  <h1>NAT e PAT spiegati bene: differenze, casi d'uso e config su router</h1>
  <div class="art-meta"><span>3 Maggio 2025</span><span>·</span><span>7 min</span><span>·</span><span>Cisco IOS · NAT · PAT</span></div>
  <div class="art-body">
    <p><strong>NAT statico</strong>: mappa 1:1 un IP privato a un IP pubblico. <strong>PAT</strong>: molti IP privati → un IP pubblico con porte diverse. È quello che usa ogni router.</p>
    <h2>NAT statico</h2>
    <div class="codeblock"><span class="codeblock-lang">IOS</span><pre>interface gig0/0
 ip nat inside
interface gig0/1
 ip nat outside
ip nat inside source static 192.168.1.10 203.0.113.10</pre></div>
    <h2>PAT (overload)</h2>
    <div class="codeblock"><span class="codeblock-lang">IOS</span><pre>access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface gig0/1 overload</pre></div>
    <h2>Verifica</h2>
    <div class="codeblock"><span class="codeblock-lang">IOS</span><pre>show ip nat translations
show ip nat statistics</pre></div>
    <div class="callout warn">⚠ Non usare <code>debug ip nat</code> su router in produzione con traffico elevato.</div>
  </div>
</div></div>

<div id="page-incident" class="page"><div class="article-wrapper">
  <a class="back-link" href="#" onclick="goHome()">Torna al blog</a>
  <div class="article-tags"><span class="tag t-inf">Infrastruttura</span></div>
  <h1>Rack e ridondanza: come un alimentatore guasto ha fermato un servizio critico</h1>
  <div class="art-meta"><span>28 Aprile 2025</span><span>·</span><span>5 min</span><span>·</span><span>Incident · Infrastruttura</span></div>
  <div class="art-body">
    <p>Caso reale gestito durante il tirocinio a Banca Mediolanum. Lo racconto perché spiega concretamente cosa significa lavorare in un ambiente enterprise con requisiti di continuità.</p>
    <h2>Cosa è successo</h2>
    <p>Un dispositivo in rack ha perso la ridondanza di alimentazione. Causa: una <strong>PDU difettosa</strong> che ha smesso di erogare corrente a uno dei due alimentatori. Il device era ancora attivo grazie al secondo, ma era una situazione di rischio reale.</p>
    <div class="callout">💡 La ridondanza N+1 non serve "se qualcosa va storto" — serve per avere tempo di intervenire quando va storto.</div>
    <h2>Come l'ho gestito</h2>
    <p>Apertura ticket P2, comunicazione al team con descrizione tecnica del rischio, coordinamento per sostituzione PDU nella finestra di manutenzione.</p>
    <div class="callout">📌 Monitora sempre gli alimentatori nei device critici con SNMP trap.</div>
  </div>
</div></div>

<div id="page-wireshark" class="page"><div class="article-wrapper">
  <a class="back-link" href="#" onclick="goHome()">Torna al blog</a>
  <div class="article-tags"><span class="tag t-net">Networking</span></div>
  <h1>Wireshark per network engineer: i filtri che uso davvero ogni giorno</h1>
  <div class="art-meta"><span>20 Aprile 2025</span><span>·</span><span>5 min</span><span>·</span><span>Wireshark · Troubleshooting</span></div>
  <div class="art-body">
    <p>Non il tutorial base. Questi sono i display filter che uso realmente per troubleshooting su reti enterprise.</p>
    <h2>Filtri essenziali</h2>
    <div class="codeblock"><span class="codeblock-lang">Wireshark</span><pre>dns                          # solo DNS
tcp.port == 443              # solo HTTPS
ip.addr == 192.168.1.10      # da/verso un IP
tcp.analysis.flags           # pacchetti con errori</pre></div>
    <h2>Filtri avanzati</h2>
    <div class="codeblock"><span class="codeblock-lang">Wireshark</span><pre>tcp.flags.reset == 1         # TCP reset
tcp.analysis.retransmission  # ritrasmissioni
tcp.analysis.ack_rtt > 0.2   # latenza alta
icmp.type == 3               # ICMP unreachable</pre></div>
    <div class="callout">💡 SYN senza SYN-ACK = problema lato server o firewall. SYN-ACK senza ACK = problema lato client.</div>
  </div>
</div></div>

<div id="page-dns" class="page"><div class="article-wrapper">
  <a class="back-link" href="#" onclick="goHome()">Torna al blog</a>
  <div class="article-tags"><span class="tag t-win">Windows/Linux</span><span class="tag t-net">Networking</span></div>
  <h1>DNS troubleshooting: comandi e metodo per risolvere problemi di risoluzione</h1>
  <div class="art-meta"><span>14 Aprile 2025</span><span>·</span><span>6 min</span><span>·</span><span>DNS · Windows · Linux</span></div>
  <div class="art-body">
    <p>I problemi DNS sono tra i più comuni in rete e spesso vengono confusi con problemi di connettività.</p>
    <h2>Svuota la cache</h2>
    <div class="codeblock"><span class="codeblock-lang">Windows</span><pre>ipconfig /flushdns
ipconfig /displaydns</pre></div>
    <div class="codeblock"><span class="codeblock-lang">Linux</span><pre>sudo systemd-resolve --flush-caches</pre></div>
    <h2>nslookup e dig</h2>
    <div class="codeblock"><span class="codeblock-lang">CMD / Terminal</span><pre>nslookup google.com
nslookup google.com 8.8.8.8    # DNS specifico
dig google.com @8.8.8.8
dig +trace google.com           # percorso completo</pre></div>
    <div class="callout">💡 Se <code>nslookup 8.8.8.8</code> funziona ma il dominio no → problema DNS. Se anche l'IP diretto non risponde → connettività.</div>
  </div>
</div></div>

<script>
// Cursor
const cur=document.getElementById('cur'),dot=document.getElementById('curDot');
let mx=0,my=0,rx=0,ry=0;
document.addEventListener('mousemove',e=>{
  mx=e.clientX;my=e.clientY;
  dot.style.left=mx+'px';dot.style.top=my+'px';
});
(function tick(){
  rx+=(mx-rx)*.1;ry+=(my-ry)*.1;
  cur.style.left=rx+'px';cur.style.top=ry+'px';
  requestAnimationFrame(tick);
})();
document.querySelectorAll('a,button,.post-card,.featured,.srv-card,.faq-question,.sp,.htag,.filter-btn').forEach(el=>{
  el.addEventListener('mouseenter',()=>cur.classList.add('hovering'));
  el.addEventListener('mouseleave',()=>cur.classList.remove('hovering'));
});

// Scroll
window.addEventListener('scroll',()=>{
  document.getElementById('backTop').classList.toggle('visible',window.scrollY>500);
  const bar=document.getElementById('readingBar');
  if(bar){const t=document.body.scrollHeight-window.innerHeight;bar.style.width=(t>0?(window.scrollY/t*100):0)+'%';}
});

// Nav
function goHome(){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById('page-home').classList.add('active');
  window.scrollTo({top:0,behavior:'smooth'});
}
function scrollTo(id){const el=document.getElementById(id);if(el)el.scrollIntoView({behavior:'smooth'});}
function showArticle(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById('page-'+id).classList.add('active');
  window.scrollTo({top:0,behavior:'smooth'});
}

// Filter
function filterPosts(cat,btn){
  document.querySelectorAll('.filter-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  const fp=document.getElementById('featured-post');
  document.querySelectorAll('#postGrid .post-card').forEach(c=>{
    c.style.display=(cat==='all'||c.dataset.cat===cat)?'block':'none';
  });
  fp.style.display=(cat==='all'||fp.dataset.cat===cat)?'grid':'none';
}

// FAQ
function toggleFaq(btn){
  const item=btn.closest('.faq-item'),isOpen=item.classList.contains('open');
  document.querySelectorAll('.faq-item').forEach(i=>i.classList.remove('open'));
  if(!isOpen)item.classList.add('open');
}

// Modal
function openModal(){document.getElementById('contactModal').classList.add('open');document.body.style.overflow='hidden';}
function closeModal(){document.getElementById('contactModal').classList.remove('open');document.body.style.overflow='';}
document.getElementById('contactModal').addEventListener('click',function(e){if(e.target===this)closeModal();});
document.addEventListener('keydown',e=>{if(e.key==='Escape')closeModal();});
function submitContact(){
  const name=document.getElementById('c-name').value.trim();
  const email=document.getElementById('c-email').value.trim();
  const type=document.getElementById('c-type').value||'richiesta generica';
  const msg=document.getElementById('c-msg').value.trim();
  const msgEl=document.getElementById('form-msg');
  msgEl.className='form-msg';
  if(!name||!email||!email.includes('@')){msgEl.textContent='⚠ Inserisci nome ed email valida.';msgEl.classList.add('error');return;}
  const reqs=JSON.parse(localStorage.getItem('contact_reqs')||'[]');
  reqs.push({name,email,type,msg,date:new Date().toISOString()});
  localStorage.setItem('contact_reqs',JSON.stringify(reqs));
  const waText=`Ciao Alessandro, mi chiamo ${name}. Richiesta: ${type}.${msg?' Dettagli: '+msg:''}`;
  window.open('https://wa.me/393441305820?text='+encodeURIComponent(waText),'_blank');
  msgEl.textContent='✓ Ti sto aprendo WhatsApp!';
  msgEl.classList.add('success');
  setTimeout(()=>{
    ['c-name','c-email','c-msg'].forEach(id=>document.getElementById(id).value='');
    document.getElementById('c-type').value='';
    msgEl.className='form-msg';
    closeModal();showToast('✓ Richiesta inviata via WhatsApp!');
  },2500);
}

// Newsletter
function updateSubCount(){
  const n=JSON.parse(localStorage.getItem('subs')||'[]').length;
  const el=document.getElementById('subCount');if(el)el.textContent='// '+n+' iscritti';
}
updateSubCount();
function handleNewsletter(){
  const input=document.getElementById('emailInput');
  const note=document.getElementById('emailNote');
  const wrap=document.getElementById('emailWrap');
  const email=input.value.trim();
  if(!email||!email.includes('@')){note.textContent='// Email non valida';note.style.color='#f87171';return;}
  const subs=JSON.parse(localStorage.getItem('subs')||'[]');
  if(subs.includes(email)){showToast('Sei già iscritto!');return;}
  subs.push(email);localStorage.setItem('subs',JSON.stringify(subs));
  wrap.innerHTML='<div style="padding:.9rem 1.1rem;color:#4ade80;font-family:\'JetBrains Mono\',monospace;font-size:.8rem;font-weight:700;">✓ Iscritto! Benvenuto.</div>';
  note.textContent='// Grazie!';note.style.color='#4ade80';
  updateSubCount();showToast('✓ Iscrizione confermata!');
}

// Toast
function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg;t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),3000);
}
</script>

</body>
</html>
