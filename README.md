<!DOCTYPE html>

<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LifeXP — Ta vie, ton jeu</title>
<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Rajdhani:wght@400;600;700&family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%;overflow-x:hidden}
:root{
  --bg:#0a0a0f;--bg2:#0f0f1a;--bg3:#13131f;--panel:#0d0d18;
  --border:#1e1e3a;--accent:#00ff88;--accent2:#7c3aed;
  --text:#e2e8f0;--text2:#94a3b8;--text3:#475569;
  --danger:#ef4444;--glow:rgba(0,255,136,0.25);
  --grad:linear-gradient(135deg,#7c3aed,#00ff88);
  --fhead:'Press Start 2P',monospace;--fmono:'Share Tech Mono',monospace;
}
[data-theme="retro"]{--bg:#1a0a00;--bg2:#220e00;--bg3:#2a1100;--panel:#1e0c00;--border:#5c2800;--accent:#ff6600;--accent2:#ffaa00;--text:#ffd0a0;--text2:#cc7733;--text3:#7a3d1a;--danger:#ff3300;--glow:rgba(255,102,0,0.3);--grad:linear-gradient(135deg,#ff3300,#ffaa00);--fhead:'Press Start 2P',monospace;}
[data-theme="neon"]{--bg:#000510;--bg2:#00051a;--bg3:#000820;--panel:#000412;--border:#001a4d;--accent:#ff00ff;--accent2:#00ffff;--text:#e0e8ff;--text2:#7090cc;--text3:#304070;--danger:#ff0055;--glow:rgba(255,0,255,0.35);--grad:linear-gradient(135deg,#ff00ff,#00ffff);--fhead:'Orbitron',sans-serif;}
[data-theme="matrix"]{--bg:#000800;--bg2:#010c01;--bg3:#011001;--panel:#000a00;--border:#0a3a0a;--accent:#00ff41;--accent2:#00cc33;--text:#b0ffb0;--text2:#4a9a4a;--text3:#1a4a1a;--danger:#ff4141;--glow:rgba(0,255,65,0.3);--grad:linear-gradient(135deg,#00cc33,#00ff41);--fhead:'Share Tech Mono',monospace;}

body{font-family:‘Rajdhani’,sans-serif;background:var(–bg);color:var(–text);transition:background .4s,color .4s}
body::before{content:’’;position:fixed;inset:0;background:repeating-linear-gradient(0deg,transparent,transparent 2px,rgba(0,0,0,.06) 2px,rgba(0,0,0,.06) 4px);pointer-events:none;z-index:1}

/* ── HEADER ── */
header{background:var(–bg2);border-bottom:2px solid var(–accent);padding:0 16px;height:58px;display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;z-index:100;box-shadow:0 0 20px var(–glow)}
.logo{font-family:var(–fhead);font-size:11px;color:var(–accent);text-shadow:0 0 16px var(–glow);letter-spacing:2px}
.header-right{display:flex;align-items:center;gap:10px}
.theme-wrap{position:relative}
.theme-btn{width:36px;height:36px;border-radius:8px;border:1.5px solid var(–border);background:var(–panel);cursor:pointer;font-size:16px;display:flex;align-items:center;justify-content:center;transition:all .2s}
.theme-btn:hover,.theme-btn.open{border-color:var(–accent);box-shadow:0 0 12px var(–glow)}
.theme-menu{position:absolute;top:calc(100% + 8px);right:0;background:var(–bg2);border:1.5px solid var(–border);border-radius:12px;padding:8px;min-width:210px;z-index:200;box-shadow:0 8px 32px rgba(0,0,0,.7);display:none;animation:dropIn .15s ease}
.theme-menu.open{display:block}
.theme-lbl{font-family:var(–fmono);font-size:9px;color:var(–text3);padding:3px 10px 8px;letter-spacing:2px}
.theme-opt{display:flex;align-items:center;gap:10px;padding:9px 12px;border-radius:8px;cursor:pointer;font-size:14px;font-weight:600;color:var(–text2);transition:background .15s}
.theme-opt:hover{background:var(–bg3)}
.theme-opt.active{color:var(–accent)}
.theme-dot{width:12px;height:12px;border-radius:50%;flex-shrink:0}

/* ── PLAYER BAR ── */
.player-bar{background:var(–bg2);border-bottom:1px solid var(–border);padding:10px 16px;display:flex;align-items:center;gap:12px}
.avatar{width:44px;height:44px;border-radius:12px;background:var(–grad);border:2px solid var(–accent);display:flex;align-items:center;justify-content:center;font-size:22px;box-shadow:0 0 14px var(–glow);flex-shrink:0}
.player-name-lbl{font-family:var(–fhead);font-size:7px;color:var(–accent);margin-bottom:2px}
.player-lvl{font-family:var(–fmono);font-size:10px;color:var(–text2)}
.xp-bar-wrap{text-align:right;margin-left:auto}
.xp-labels{font-family:var(–fmono);font-size:9px;color:var(–text3);display:flex;justify-content:space-between;gap:10px;margin-bottom:4px}
.xp-track{width:150px;height:5px;background:var(–border);border-radius:3px;overflow:hidden}
.xp-fill{height:100%;background:var(–grad);border-radius:3px;transition:width .6s}

/* ── STATS BANNER ── */
.stats-banner{display:grid;grid-template-columns:repeat(4,1fr);gap:1px;background:var(–border);border-bottom:1px solid var(–border)}
.stat-cell{background:var(–bg2);padding:12px 0;text-align:center}
.stat-val{font-family:var(–fhead);font-size:15px;color:var(–accent);display:block;text-shadow:0 0 10px var(–glow)}
.stat-lbl{font-family:var(–fmono);font-size:8px;color:var(–text3);text-transform:uppercase;letter-spacing:1px;margin-top:3px}

/* ── TABS ── */
.tabs{display:flex;background:var(–bg2);border-bottom:1px solid var(–border)}
.tab-btn{flex:1;padding:12px 4px;background:none;border:none;border-bottom:2px solid transparent;color:var(–text3);font-family:‘Rajdhani’,sans-serif;font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:.5px;cursor:pointer;transition:all .2s}
.tab-btn.active{color:var(–accent);border-bottom-color:var(–accent)}

/* ── CONTENT ── */
.content{padding:16px;max-width:860px;margin:0 auto;padding-bottom:40px}
.tab-panel{display:none}.tab-panel.active{display:block}
.section-header{display:flex;justify-content:space-between;align-items:center;margin-bottom:14px}
.section-title{font-family:var(–fhead);font-size:8px;color:var(–accent);letter-spacing:2px}

/* ── BUTTONS ── */
.btn{font-family:‘Rajdhani’,sans-serif;font-size:12px;font-weight:700;padding:7px 14px;border-radius:8px;border:1.5px solid var(–accent);background:transparent;color:var(–accent);cursor:pointer;text-transform:uppercase;letter-spacing:1px;transition:all .2s}
.btn:hover{background:var(–accent);color:var(–bg)}
.btn-danger{border-color:var(–danger);color:var(–danger)}
.btn-danger:hover{background:var(–danger);color:#fff}
.btn-ghost{border-color:var(–border);color:var(–text3)}
.btn-ghost:hover{background:var(–border);color:var(–text)}

/* ── CARDS ── */
.card{background:var(–panel);border:1px solid var(–border);border-radius:12px;padding:16px;margin-bottom:10px;transition:border-color .2s}
.card:hover{border-color:var(–accent)44}
.empty-state{text-align:center;padding:48px 0;color:var(–text3);font-family:var(–fmono);font-size:12px;line-height:2}
.empty-icon{font-size:36px;margin-bottom:10px}

/* ── CATEGORY ── */
.cat-header{display:flex;align-items:center;gap:10px;margin-bottom:10px}
.cat-emoji{font-size:22px}
.cat-name{flex:1;font-size:16px;font-weight:700}
.cat-xp{font-family:var(–fmono);font-size:12px;color:var(–accent)}
.cat-bar{height:4px;background:var(–border);border-radius:2px;overflow:hidden;margin-bottom:10px}
.cat-fill{height:100%;background:var(–grad);border-radius:2px;transition:width .5s}
.skill-list{display:flex;flex-wrap:wrap;gap:6px}
.skill-tag{background:var(–bg3);border:1px solid var(–border);border-radius:6px;padding:4px 10px;font-family:var(–fmono);font-size:11px;color:var(–text2);display:flex;gap:6px}
.skill-xp{color:var(–accent)}

/* ── QUEST CARDS ── */
.quest-card{background:var(–panel);border:1px solid var(–border);border-radius:12px;padding:12px 14px;margin-bottom:8px;display:flex;align-items:center;gap:10px;transition:all .3s;animation:fadeIn .25s ease}
.quest-card.done{opacity:.45}
.quest-check{width:26px;height:26px;border:2px solid var(–border);border-radius:6px;background:transparent;cursor:pointer;display:flex;align-items:center;justify-content:center;font-size:13px;flex-shrink:0;transition:all .2s;color:transparent}
.quest-check.checked{background:var(–accent);border-color:var(–accent);color:var(–bg)}
.quest-info{flex:1}
.quest-name{font-size:14px;font-weight:600}
.quest-meta{font-family:var(–fmono);font-size:10px;color:var(–text3);margin-top:2px}
.quest-xp{font-family:var(–fhead);font-size:8px;color:var(–accent)}
.icon-btn{background:none;border:none;color:var(–text3);cursor:pointer;font-size:14px;padding:4px;border-radius:4px;transition:color .2s}
.icon-btn:hover{color:var(–danger)}

/* ── UNLOCK ── */
.unlock-row{display:flex;align-items:center;gap:10px;padding:8px 0;margin-bottom:6px}
.unlock-line{flex:1;height:1px;background:var(–border)}
.unlock-btn{font-family:var(–fmono);font-size:10px;padding:6px 14px;border-radius:20px;border:1.5px dashed var(–accent)55;background:var(–accent)08;color:var(–accent)99;cursor:pointer;transition:all .25s;white-space:nowrap;letter-spacing:1px}
.unlock-btn:hover{border-color:var(–accent);color:var(–accent);background:var(–accent)18;box-shadow:0 0 10px var(–glow)}
.locked-preview{background:var(–bg3);border:1px dashed var(–border);border-radius:10px;padding:10px 14px;margin-bottom:8px;display:flex;align-items:center;gap:10px;opacity:.4}
.locked-text{font-family:var(–fmono);font-size:11px;color:var(–text3)}

/* ── QUEST VIEW HEADER ── */
.quest-view-header{background:var(–panel);border:1px solid var(–border);border-radius:12px;padding:14px 16px;margin-bottom:16px;display:flex;align-items:center;justify-content:space-between;gap:12px}
.quest-view-info{font-family:var(–fmono);font-size:11px;color:var(–text2)}
.quest-view-info strong{color:var(–accent)}
.quest-progress-bar{width:120px;height:4px;background:var(–border);border-radius:2px;overflow:hidden;margin-top:6px}
.quest-progress-fill{height:100%;background:var(–grad);border-radius:2px;transition:width .4s}

/* ── FILTER TABS ── */
.filter-tabs{display:flex;gap:6px;margin-bottom:14px;flex-wrap:wrap}
.filter-tab{padding:5px 12px;border-radius:20px;border:1px solid var(–border);background:transparent;color:var(–text3);font-family:var(–fmono);font-size:10px;cursor:pointer;transition:all .2s}
.filter-tab.active{border-color:var(–accent);color:var(–accent);background:var(–accent)12}

/* ── ACTIVITY ── */
.activity-item{display:flex;gap:12px;padding:11px 0;border-bottom:1px solid var(–border)}
.activity-icon{width:30px;height:30px;border-radius:8px;background:var(–bg3);border:1px solid var(–border);display:flex;align-items:center;justify-content:center;font-size:13px;flex-shrink:0}
.activity-time{font-family:var(–fmono);font-size:9px;color:var(–text3);margin-top:2px}
.activity-xp{font-family:var(–fmono);font-size:12px;font-weight:700}

/* ── PROFILE ── */
.profile-hero{background:var(–panel);border:1px solid var(–border);border-radius:16px;padding:28px;text-align:center;margin-bottom:14px}
.profile-avatar{width:70px;height:70px;border-radius:16px;background:var(–grad);border:3px solid var(–accent);display:flex;align-items:center;justify-content:center;font-size:30px;margin:0 auto 14px;box-shadow:0 0 20px var(–glow)}
.profile-name{font-family:var(–fhead);font-size:10px;color:var(–accent);margin-bottom:4px}
.profile-stats{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-top:18px}
.profile-stat{background:var(–bg3);border:1px solid var(–border);border-radius:10px;padding:12px;text-align:center}
.profile-stat-val{font-family:var(–fhead);font-size:14px;color:var(–accent);display:block}
.profile-stat-lbl{font-family:var(–fmono);font-size:9px;color:var(–text3);margin-top:4px}

/* ── MODALS ── */
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.8);z-index:400;display:none;align-items:center;justify-content:center;padding:20px}
.modal-overlay.open{display:flex}
.modal{background:var(–bg2);border:1.5px solid var(–accent);border-radius:16px;padding:24px;width:100%;max-width:400px;box-shadow:0 20px 60px rgba(0,0,0,.9),0 0 20px var(–glow);animation:modalIn .2s ease}
.modal-title{font-family:var(–fhead);font-size:8px;color:var(–accent);margin-bottom:18px;letter-spacing:2px}
.modal-field{margin-bottom:12px}
.modal-label{font-family:var(–fmono);font-size:10px;color:var(–text3);text-transform:uppercase;letter-spacing:1px;display:block;margin-bottom:5px}
.modal-input{width:100%;background:var(–bg3);border:1px solid var(–border);border-radius:8px;padding:9px 12px;color:var(–text);font-family:‘Rajdhani’,sans-serif;font-size:14px;outline:none;transition:border-color .2s}
.modal-input:focus{border-color:var(–accent)}
.modal-actions{display:flex;gap:8px;justify-content:flex-end;margin-top:18px}

/* ── CONFIRM MODAL ── */
.confirm-modal{background:var(–bg2);border:1.5px solid var(–danger);border-radius:16px;padding:24px;width:100%;max-width:340px;box-shadow:0 20px 60px rgba(0,0,0,.9);animation:modalIn .2s ease;text-align:center}

/* ── TOAST ── */
.toast{position:fixed;top:68px;right:14px;background:var(–bg2);border:1.5px solid var(–accent);border-radius:10px;padding:9px 14px;font-family:var(–fmono);font-size:11px;color:var(–accent);box-shadow:0 0 16px var(–glow);z-index:500;transform:translateX(120%);transition:transform .3s cubic-bezier(.34,1.56,.64,1)}
.toast.show{transform:translateX(0)}

/* ── LEVEL UP ── */
.levelup-popup{position:fixed;inset:0;background:rgba(0,0,0,.85);z-index:600;display:none;align-items:center;justify-content:center}
.levelup-popup.show{display:flex}
.levelup-box{background:var(–bg2);border:2px solid var(–accent);border-radius:20px;padding:36px 28px;text-align:center;box-shadow:0 0 40px var(–glow);animation:modalIn .3s ease}
.levelup-title{font-family:var(–fhead);font-size:11px;color:var(–accent);margin-bottom:6px;letter-spacing:2px;text-shadow:0 0 20px var(–glow)}

/* ══════════════════════════════════
ONBOARDING
══════════════════════════════════ */
#onboarding{position:fixed;inset:0;background:var(–bg);z-index:1000;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:24px;overflow-y:auto}
#onboarding.hidden{display:none}
.ob-step{display:none;flex-direction:column;align-items:center;text-align:center;max-width:520px;width:100%;animation:fadeIn .4s ease}
.ob-step.active{display:flex}
.ob-logo{font-family:var(–fhead);font-size:14px;color:var(–accent);letter-spacing:3px;margin-bottom:8px;animation:glow-pulse 2s ease-in-out infinite}
.ob-sub{font-family:var(–fmono);font-size:11px;color:var(–text3);margin-bottom:32px;letter-spacing:2px}
.ob-title{font-family:var(–fhead);font-size:10px;color:var(–text);margin-bottom:8px;letter-spacing:1px;line-height:1.8}
.ob-desc{font-family:var(–fmono);font-size:12px;color:var(–text2);margin-bottom:24px;line-height:1.8}
.ob-progress{display:flex;gap:6px;margin-bottom:28px}
.ob-dot{width:8px;height:8px;border-radius:50%;background:var(–border);transition:all .3s}
.ob-dot.active{background:var(–accent);box-shadow:0 0 8px var(–glow)}
.ob-dot.done{background:var(–accent)55}
.ob-input{width:100%;background:var(–bg3);border:1.5px solid var(–border);border-radius:10px;padding:12px 16px;color:var(–text);font-family:‘Rajdhani’,sans-serif;font-size:16px;outline:none;text-align:center;margin-bottom:16px;transition:border-color .2s}
.ob-input:focus{border-color:var(–accent)}
.ob-btn{font-family:‘Rajdhani’,sans-serif;font-size:13px;font-weight:700;padding:12px 32px;border-radius:10px;border:1.5px solid var(–accent);background:var(–accent);color:var(–bg);cursor:pointer;text-transform:uppercase;letter-spacing:2px;transition:all .2s;width:100%}
.ob-btn:hover{box-shadow:0 0 20px var(–glow)}
.ob-btn:disabled{opacity:.4;cursor:not-allowed}
.ob-btn-ghost{background:transparent;color:var(–accent)}
.ob-btn-ghost:hover{background:var(–accent);color:var(–bg)}
.ob-selected-count{font-family:var(–fmono);font-size:10px;color:var(–accent);margin-bottom:16px}

/* ── DOMAIN GRID ── */
.ob-domains{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;width:100%;margin-bottom:24px}
.ob-domain{background:var(–bg3);border:1.5px solid var(–border);border-radius:12px;padding:14px 10px;cursor:pointer;transition:all .2s;font-size:13px;font-weight:600;color:var(–text2);display:flex;flex-direction:column;align-items:center;gap:6px}
.ob-domain:hover{border-color:var(–accent)88;color:var(–text)}
.ob-domain.selected{border-color:var(–accent);background:var(–accent)15;color:var(–accent)}
.ob-domain-emoji{font-size:24px}
.ob-domain-name{font-family:‘Rajdhani’,sans-serif;font-weight:700;font-size:13px}
.ob-domain-desc{font-family:var(–fmono);font-size:9px;color:var(–text3);text-align:center;line-height:1.5}
.ob-domain.selected .ob-domain-desc{color:var(–accent)88}

/* ── CUSTOM DOMAINS ── */
.ob-custom-domains{width:100%;margin-bottom:16px}
.ob-custom-row{display:flex;gap:8px;margin-bottom:8px;align-items:center}
.ob-custom-emoji{width:50px;background:var(–bg3);border:1.5px solid var(–border);border-radius:8px;padding:10px 6px;color:var(–text);font-size:16px;text-align:center;outline:none}
.ob-custom-name{flex:1;background:var(–bg3);border:1.5px solid var(–border);border-radius:8px;padding:10px 12px;color:var(–text);font-family:‘Rajdhani’,sans-serif;font-size:14px;outline:none}
.ob-custom-name:focus,.ob-custom-emoji:focus{border-color:var(–accent)}
.ob-add-custom{background:none;border:1.5px dashed var(–border);border-radius:8px;color:var(–text3);font-family:‘Rajdhani’,sans-serif;font-size:12px;font-weight:700;padding:8px;width:100%;cursor:pointer;transition:all .2s;margin-bottom:16px}
.ob-add-custom:hover{border-color:var(–accent);color:var(–accent)}

/* ── LEVEL SELECTION ── */
.ob-levels{display:flex;flex-direction:column;gap:10px;width:100%;margin-bottom:24px}

/* ── CHARACTER BUILDER ── */
.char-builder{width:100%;margin-bottom:24px}
.char-preview{width:90px;height:90px;border-radius:18px;background:var(–grad);border:3px solid var(–accent);display:flex;align-items:center;justify-content:center;font-size:42px;margin:0 auto 20px;box-shadow:0 0 24px var(–glow);cursor:pointer;transition:transform .2s;position:relative}
.char-preview:hover{transform:scale(1.06)}
.char-preview-hint{position:absolute;bottom:-22px;left:50%;transform:translateX(-50%);font-family:var(–fmono);font-size:8px;color:var(–accent)88;white-space:nowrap;letter-spacing:1px}
.char-class-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:16px}
.char-class-card{background:var(–bg3);border:1.5px solid var(–border);border-radius:10px;padding:10px 8px;cursor:pointer;transition:all .2s;text-align:center}
.char-class-card:hover{border-color:var(–accent)88}
.char-class-card.selected{border-color:var(–accent);background:var(–accent)15}
.char-class-icon{font-size:20px;margin-bottom:4px}
.char-class-name{font-family:‘Rajdhani’,sans-serif;font-weight:700;font-size:12px;color:var(–text2)}
.char-class-card.selected .char-class-name{color:var(–accent)}
.char-class-desc{font-family:var(–fmono);font-size:8px;color:var(–text3);margin-top:2px;line-height:1.4}

/* ── AVATAR PICKER MODAL ── */
.avatar-modal{background:var(–bg2);border:1.5px solid var(–accent);border-radius:16px;padding:20px;width:100%;max-width:440px;box-shadow:0 20px 60px rgba(0,0,0,.9),0 0 20px var(–glow);animation:modalIn .2s ease;max-height:85vh;overflow-y:auto}
.avatar-cats{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:14px}
.avatar-cat-btn{padding:4px 12px;border-radius:16px;border:1px solid var(–border);background:transparent;color:var(–text3);font-family:var(–fmono);font-size:9px;cursor:pointer;transition:all .2s}
.avatar-cat-btn.active{border-color:var(–accent);color:var(–accent);background:var(–accent)12}
.avatar-grid{display:grid;grid-template-columns:repeat(6,1fr);gap:8px;margin-bottom:14px}
.avatar-option{width:100%;aspect-ratio:1;border-radius:10px;background:var(–bg3);border:2px solid var(–border);display:flex;align-items:center;justify-content:center;font-size:26px;cursor:pointer;transition:all .15s}
.avatar-option:hover{border-color:var(–accent)88;transform:scale(1.08)}
.avatar-option.selected{border-color:var(–accent);background:var(–accent)18;box-shadow:0 0 10px var(–glow)}

/* ── PROFILE EDIT ── */
.profile-edit-btn{display:inline-flex;align-items:center;gap:5px;font-family:var(–fmono);font-size:9px;color:var(–text3);background:var(–bg3);border:1px solid var(–border);border-radius:16px;padding:4px 12px;cursor:pointer;transition:all .2s;margin-top:8px;letter-spacing:1px}
.profile-edit-btn:hover{border-color:var(–accent);color:var(–accent)}
.char-title-badge{display:inline-block;background:var(–accent)18;border:1px solid var(–accent)44;border-radius:20px;padding:3px 12px;font-family:var(–fmono);font-size:9px;color:var(–accent)cc;margin-top:4px;letter-spacing:1px}

/* ANIMATIONS */
@keyframes dropIn{from{opacity:0;transform:translateY(-6px)}to{opacity:1;transform:translateY(0)}}
@keyframes modalIn{from{opacity:0;transform:scale(.95)}to{opacity:1;transform:scale(1)}}
@keyframes fadeIn{from{opacity:0;transform:translateY(16px)}to{opacity:1;transform:translateY(0)}}
@keyframes glow-pulse{0%,100%{text-shadow:0 0 10px var(–glow)}50%{text-shadow:0 0 30px var(–glow),0 0 60px var(–glow)}}
@keyframes unlockPop{0%{transform:scale(0.7);opacity:0}70%{transform:scale(1.05)}100%{transform:scale(1);opacity:1}}
.quest-card.new-unlock{animation:unlockPop .35s cubic-bezier(.34,1.56,.64,1)}

@media(max-width:480px){
.xp-bar-wrap{display:none}
.stats-banner{grid-template-columns:repeat(2,1fr)}
}
</style>

</head>
<body>

<div class="toast" id="toast"></div>

<!-- LEVEL UP -->

<div class="levelup-popup" id="levelupPopup">
  <div class="levelup-box">
    <div style="font-size:52px;margin-bottom:12px">🏆</div>
    <div class="levelup-title">LEVEL UP !</div>
    <div style="font-family:var(--fmono);font-size:14px;color:var(--text);margin-bottom:24px" id="levelupLabel">NIVEAU 2 — NOVICE</div>
    <button class="btn" onclick="document.getElementById('levelupPopup').classList.remove('show')">CONTINUER →</button>
  </div>
</div>

<!-- ONBOARDING -->

<div id="onboarding">

  <!-- ÉTAPE 1 : Prénom + Perso -->

  <div class="ob-step active" id="ob-step-1">
    <div class="ob-logo">LIFEXP</div>
    <div class="ob-sub">// TA VIE, TON JEU</div>
    <div class="ob-progress">
      <div class="ob-dot active"></div>
      <div class="ob-dot"></div>
      <div class="ob-dot"></div>
      <div class="ob-dot"></div>
      <div class="ob-dot"></div>
    </div>
    <div class="ob-title">BIENVENUE, JOUEUR</div>
    <div class="ob-desc">Gamifie ta vie réelle : catégories,<br>compétences et quêtes quotidiennes.<br><br>Comment tu t'appelles ?</div>
    <input class="ob-input" id="ob-name" placeholder="Ton prénom…" maxlength="20" autocomplete="off">
    <button class="ob-btn" onclick="obNext(1)">COMMENCER →</button>
  </div>

  <!-- ÉTAPE 1.5 : Créer son personnage -->

  <div class="ob-step" id="ob-step-char">
    <div class="ob-logo">LIFEXP</div>
    <div class="ob-progress">
      <div class="ob-dot done"></div>
      <div class="ob-dot active"></div>
      <div class="ob-dot"></div>
      <div class="ob-dot"></div>
      <div class="ob-dot"></div>
    </div>
    <div class="ob-title">CRÉE TON PERSONNAGE</div>
    <div class="ob-desc">Choisis ton avatar et ta classe.<br>Ça définit ton style de joueur !</div>
    <div class="char-builder">
      <div class="char-preview" id="ob-char-preview" onclick="openAvatarPicker('ob')">
        ⚔️
        <div class="char-preview-hint">CLIQUER POUR CHANGER</div>
      </div>
      <div style="font-family:var(--fmono);font-size:9px;color:var(--text3);text-align:center;margin-bottom:14px;letter-spacing:2px">// CHOISIS TA CLASSE</div>
      <div class="char-class-grid" id="ob-class-grid">
        <div class="char-class-card" data-class="Guerrier" onclick="selectClass(this,'ob')">
          <div class="char-class-icon">⚔️</div>
          <div class="char-class-name">Guerrier</div>
          <div class="char-class-desc">Discipliné,<br>persévérant</div>
        </div>
        <div class="char-class-card" data-class="Mage" onclick="selectClass(this,'ob')">
          <div class="char-class-icon">🧙</div>
          <div class="char-class-name">Mage</div>
          <div class="char-class-desc">Curieux,<br>analytique</div>
        </div>
        <div class="char-class-card" data-class="Rôdeur" onclick="selectClass(this,'ob')">
          <div class="char-class-icon">🏹</div>
          <div class="char-class-name">Rôdeur</div>
          <div class="char-class-desc">Stratège,<br>adaptable</div>
        </div>
        <div class="char-class-card" data-class="Alchimiste" onclick="selectClass(this,'ob')">
          <div class="char-class-icon">⚗️</div>
          <div class="char-class-name">Alchimiste</div>
          <div class="char-class-desc">Créatif,<br>innovant</div>
        </div>
        <div class="char-class-card" data-class="Barde" onclick="selectClass(this,'ob')">
          <div class="char-class-icon">🎸</div>
          <div class="char-class-name">Barde</div>
          <div class="char-class-desc">Sociable,<br>charismatique</div>
        </div>
        <div class="char-class-card" data-class="Moine" onclick="selectClass(this,'ob')">
          <div class="char-class-icon">🧘</div>
          <div class="char-class-name">Moine</div>
          <div class="char-class-desc">Zen,<br>mindful</div>
        </div>
      </div>
    </div>
    <button class="ob-btn" onclick="obNext('char')" id="ob-btn-char" disabled>SUIVANT →</button>
  </div>

  <!-- ÉTAPE 2 : Domaines -->

  <div class="ob-step" id="ob-step-2">
    <div class="ob-logo">LIFEXP</div>
    <div class="ob-progress">
      <div class="ob-dot done"></div>
      <div class="ob-dot done"></div>
      <div class="ob-dot active"></div>
      <div class="ob-dot"></div>
      <div class="ob-dot"></div>
    </div>
    <div class="ob-title">CHOISIS TES DOMAINES</div>
    <div class="ob-desc">Sélectionne les domaines à développer.<br>Tu démarres avec <strong style="color:var(--accent)">2 quêtes par domaine</strong> —<br>de nouvelles se déverrouilleront au fil de ta progression !</div>
    <div class="ob-selected-count" id="ob-count">0 domaine sélectionné</div>
    <div class="ob-domains" id="ob-domains">
      <div class="ob-domain" data-name="Tech & IA" data-emoji="💻" onclick="toggleDomain(this)"><div class="ob-domain-emoji">💻</div><div class="ob-domain-name">Tech & IA</div><div class="ob-domain-desc">Code, programmation,<br>intelligence artificielle</div></div>
      <div class="ob-domain" data-name="Business" data-emoji="🚀" onclick="toggleDomain(this)"><div class="ob-domain-emoji">🚀</div><div class="ob-domain-name">Business</div><div class="ob-domain-desc">Entrepreneuriat,<br>marketing, stratégie</div></div>
      <div class="ob-domain" data-name="Santé & Sport" data-emoji="💪" onclick="toggleDomain(this)"><div class="ob-domain-emoji">💪</div><div class="ob-domain-name">Santé & Sport</div><div class="ob-domain-desc">Fitness, nutrition,<br>bien-être mental</div></div>
      <div class="ob-domain" data-name="Langues" data-emoji="🌍" onclick="toggleDomain(this)"><div class="ob-domain-emoji">🌍</div><div class="ob-domain-name">Langues</div><div class="ob-domain-desc">Anglais, espagnol,<br>culture, voyages</div></div>
      <div class="ob-domain" data-name="Finance" data-emoji="💰" onclick="toggleDomain(this)"><div class="ob-domain-emoji">💰</div><div class="ob-domain-name">Finance</div><div class="ob-domain-desc">Budget, épargne,<br>investissement</div></div>
      <div class="ob-domain" data-name="Créativité" data-emoji="🎨" onclick="toggleDomain(this)"><div class="ob-domain-emoji">🎨</div><div class="ob-domain-name">Créativité</div><div class="ob-domain-desc">Design, écriture,<br>photo, vidéo</div></div>
      <div class="ob-domain" data-name="Études" data-emoji="📚" onclick="toggleDomain(this)"><div class="ob-domain-emoji">📚</div><div class="ob-domain-name">Études</div><div class="ob-domain-desc">Formation, cours,<br>révisions, diplômes</div></div>
      <div class="ob-domain" data-name="Social" data-emoji="🤝" onclick="toggleDomain(this)"><div class="ob-domain-emoji">🤝</div><div class="ob-domain-name">Social</div><div class="ob-domain-desc">Relations, réseau,<br>communication</div></div>
      <div class="ob-domain" data-name="Mindset" data-emoji="🧠" onclick="toggleDomain(this)"><div class="ob-domain-emoji">🧠</div><div class="ob-domain-name">Mindset</div><div class="ob-domain-desc">Discipline, habitudes,<br>développement perso</div></div>
      <div class="ob-domain" data-name="Musique" data-emoji="🎵" onclick="toggleDomain(this)"><div class="ob-domain-emoji">🎵</div><div class="ob-domain-name">Musique</div><div class="ob-domain-desc">Instrument, chant,<br>production musicale</div></div>
    </div>
    <button class="ob-btn" onclick="obNext(2)" id="ob-btn-2" disabled>SUIVANT →</button>
  </div>

  <!-- ÉTAPE 3 : Domaines perso -->

  <div class="ob-step" id="ob-step-3">
    <div class="ob-logo">LIFEXP</div>
    <div class="ob-progress">
      <div class="ob-dot done"></div>
      <div class="ob-dot done"></div>
      <div class="ob-dot done"></div>
      <div class="ob-dot active"></div>
      <div class="ob-dot"></div>
    </div>
    <div class="ob-title">DOMAINES PERSO ?</div>
    <div class="ob-desc">Tu as un domaine qui n'est pas dans la liste ?<br>Crée-le ici !</div>
    <div class="ob-custom-domains" id="ob-custom-domains"></div>
    <button class="ob-add-custom" onclick="addCustomDomain()">＋ Ajouter un domaine personnalisé</button>
    <div style="display:flex;gap:10px;width:100%">
      <button class="ob-btn ob-btn-ghost" onclick="obNext(3)" style="flex:1">PASSER</button>
      <button class="ob-btn" onclick="obNext(3)" style="flex:2">SUIVANT →</button>
    </div>
  </div>

  <!-- ÉTAPE 4 : Niveau -->

  <div class="ob-step" id="ob-step-4">
    <div class="ob-logo">LIFEXP</div>
    <div class="ob-progress">
      <div class="ob-dot done"></div>
      <div class="ob-dot done"></div>
      <div class="ob-dot done"></div>
      <div class="ob-dot done"></div>
      <div class="ob-dot active"></div>
    </div>
    <div class="ob-title">TON NIVEAU GÉNÉRAL</div>
    <div class="ob-desc">Dans la plupart de tes domaines,<br>tu te situes où ?</div>
    <div class="ob-levels">
      <div class="ob-domain" data-level="debutant" onclick="selectLevel(this)" style="flex-direction:row;justify-content:flex-start;gap:14px;padding:14px 18px">
        <div style="font-size:22px">🌱</div>
        <div style="text-align:left"><div class="ob-domain-name">Débutant</div><div class="ob-domain-desc" style="text-align:left">Je pars de zéro, j'explore</div></div>
      </div>
      <div class="ob-domain" data-level="intermediaire" onclick="selectLevel(this)" style="flex-direction:row;justify-content:flex-start;gap:14px;padding:14px 18px">
        <div style="font-size:22px">⚡</div>
        <div style="text-align:left"><div class="ob-domain-name">Intermédiaire</div><div class="ob-domain-desc" style="text-align:left">J'ai quelques bases, je progresse</div></div>
      </div>
      <div class="ob-domain" data-level="avance" onclick="selectLevel(this)" style="flex-direction:row;justify-content:flex-start;gap:14px;padding:14px 18px">
        <div style="font-size:22px">🔥</div>
        <div style="text-align:left"><div class="ob-domain-name">Avancé</div><div class="ob-domain-desc" style="text-align:left">J'ai de l'expérience, je veux exceller</div></div>
      </div>
    </div>
    <button class="ob-btn" onclick="obNext(4)" id="ob-btn-4" disabled>LANCER LIFEXP ⚔</button>
  </div>

</div>

<!-- ══════════ MAIN APP ══════════ -->

<header>
  <div class="logo">LIFEXP</div>
  <div class="header-right">
    <div class="theme-wrap">
      <button class="theme-btn" id="themeBtn">🎨</button>
      <div class="theme-menu" id="themeMenu">
        <div class="theme-lbl">// THÈME</div>
        <div class="theme-opt active" data-theme="cyber"><div class="theme-dot" style="background:#00ff88;box-shadow:0 0 6px #00ff88"></div>🟢 Cyber</div>
        <div class="theme-opt" data-theme="retro"><div class="theme-dot" style="background:#ff6600;box-shadow:0 0 6px #ff6600"></div>🟠 Retro</div>
        <div class="theme-opt" data-theme="neon"><div class="theme-dot" style="background:#ff00ff;box-shadow:0 0 6px #ff00ff"></div>🟣 Neon</div>
        <div class="theme-opt" data-theme="matrix"><div class="theme-dot" style="background:#00ff41;box-shadow:0 0 6px #00ff41"></div>🟩 Matrix</div>
      </div>
    </div>
    <div id="headerXp" style="font-family:var(--fmono);font-size:11px;color:var(--text2)">0 XP</div>
  </div>
</header>

<div class="player-bar">
  <div class="avatar" id="mainAvatar">⚔️</div>
  <div>
    <div class="player-name-lbl" id="playerNameLbl">JOUEUR</div>
    <div class="player-lvl" id="playerLvl">NIVEAU 1 — DÉBUTANT</div>
  </div>
  <div class="xp-bar-wrap">
    <div class="xp-labels"><span>PROGRESSION</span><span id="xpFrac">0 / 100 XP</span></div>
    <div class="xp-track"><div class="xp-fill" id="xpFill" style="width:0%"></div></div>
  </div>
</div>

<div class="stats-banner">
  <div class="stat-cell"><span class="stat-val" id="sLvl">1</span><div class="stat-lbl">Niveau</div></div>
  <div class="stat-cell"><span class="stat-val" id="sQuests">0</span><div class="stat-lbl">Quêtes</div></div>
  <div class="stat-cell"><span class="stat-val" id="sCats">0</span><div class="stat-lbl">Catégories</div></div>
  <div class="stat-cell"><span class="stat-val" id="sXP">0</span><div class="stat-lbl">XP Total</div></div>
</div>

<div class="tabs">
  <button class="tab-btn active" data-tab="skills">⚔ Compétences</button>
  <button class="tab-btn" data-tab="quests">📋 Quêtes</button>
  <button class="tab-btn" data-tab="profile">📊 Profil</button>
  <button class="tab-btn" data-tab="activity">⚡ Activité</button>
</div>

<div class="content">

  <!-- SKILLS -->

  <div class="tab-panel active" id="tab-skills">
    <div class="section-header">
      <div class="section-title">// CATÉGORIES</div>
      <button class="btn" onclick="openCatModal()">＋ NOUVELLE</button>
    </div>
    <div id="catList"></div>
  </div>

  <!-- QUESTS -->

  <div class="tab-panel" id="tab-quests">
    <div class="section-header">
      <div class="section-title">// QUÊTES</div>
      <button class="btn" onclick="openQuestModal()">＋ NOUVELLE</button>
    </div>
    <div class="quest-view-header" id="questViewHeader" style="display:none">
      <div>
        <div class="quest-view-info" id="questViewInfo"></div>
        <div class="quest-progress-bar"><div class="quest-progress-fill" id="questProgressFill"></div></div>
      </div>
    </div>
    <div class="filter-tabs" id="filterTabs"></div>
    <div id="questList"></div>
  </div>

  <!-- PROFILE -->

  <div class="tab-panel" id="tab-profile">
    <div class="profile-hero">
      <div class="profile-avatar" id="profileAvatar">⚔️</div>
      <div class="profile-name" id="profileName">JOUEUR</div>
      <div id="profileClassBadge"></div>
      <div style="font-family:var(--fmono);font-size:12px;color:var(--text2);margin-top:4px" id="profileLvl">NIVEAU 1 — DÉBUTANT</div>
      <button class="profile-edit-btn" onclick="openCharEditModal()">✏️ MODIFIER LE PERSONNAGE</button>
      <div class="profile-stats">
        <div class="profile-stat"><span class="profile-stat-val" id="pXP">0</span><div class="profile-stat-lbl">XP TOTAL</div></div>
        <div class="profile-stat"><span class="profile-stat-val" id="pLvl">1</span><div class="profile-stat-lbl">NIVEAU</div></div>
        <div class="profile-stat"><span class="profile-stat-val" id="pQuests">0</span><div class="profile-stat-lbl">QUÊTES</div></div>
      </div>
    </div>
    <div class="card">
      <div style="font-family:var(--fhead);font-size:8px;color:var(--accent);margin-bottom:14px;letter-spacing:2px">// PROGRESSION PAR CATÉGORIE</div>
      <div id="profileCats"></div>
    </div>
  </div>

  <!-- ACTIVITY -->

  <div class="tab-panel" id="tab-activity">
    <div class="section-header">
      <div class="section-title">// JOURNAL</div>
      <button class="btn btn-danger" onclick="confirmClearActivity()">🗑 EFFACER</button>
    </div>
    <div id="activityLog"></div>
  </div>

</div>

<!-- MODAL CATÉGORIE -->

<div class="modal-overlay" id="catModal">
  <div class="modal">
    <div class="modal-title">// NOUVELLE CATÉGORIE</div>
    <div class="modal-field"><label class="modal-label">Nom</label><input class="modal-input" id="catName" placeholder="Ex: Sport, Lecture…" autocomplete="off"></div>
    <div class="modal-field"><label class="modal-label">Emoji</label><input class="modal-input" id="catEmoji" placeholder="🏃" maxlength="2" autocomplete="off"></div>
    <div class="modal-actions">
      <button class="btn btn-ghost" onclick="closeCatModal()">Annuler</button>
      <button class="btn" onclick="confirmCat()">Créer</button>
    </div>
  </div>
</div>

<!-- MODAL QUÊTE -->

<div class="modal-overlay" id="questModal">
  <div class="modal">
    <div class="modal-title">// NOUVELLE QUÊTE</div>
    <div class="modal-field"><label class="modal-label">Nom</label><input class="modal-input" id="questName" placeholder="Ex: Faire 30 min de vélo" autocomplete="off"></div>
    <div class="modal-field"><label class="modal-label">XP</label><input class="modal-input" id="questXp" type="number" placeholder="50" min="1" autocomplete="off"></div>
    <div class="modal-field"><label class="modal-label">Catégorie</label><select class="modal-input" id="questCat"></select></div>
    <div class="modal-actions">
      <button class="btn btn-ghost" onclick="closeQuestModal()">Annuler</button>
      <button class="btn" onclick="confirmQuest()">Créer</button>
    </div>
  </div>
</div>

<!-- CONFIRM MODAL -->

<div class="modal-overlay" id="confirmModal">
  <div class="confirm-modal">
    <div style="font-size:36px;margin-bottom:12px" id="confirmIcon">⚠️</div>
    <div style="font-family:var(--fhead);font-size:8px;color:var(--danger);margin-bottom:10px;letter-spacing:1px" id="confirmTitle">CONFIRMER</div>
    <div style="font-family:var(--fmono);font-size:11px;color:var(--text2);line-height:1.7;margin-bottom:20px" id="confirmText"></div>
    <div style="display:flex;gap:8px;justify-content:center">
      <button class="btn btn-ghost" onclick="closeConfirm()">Annuler</button>
      <button class="btn btn-danger" id="confirmOkBtn">Confirmer</button>
    </div>
  </div>
</div>

<!-- AVATAR PICKER MODAL -->

<div class="modal-overlay" id="avatarModal">
  <div class="avatar-modal">
    <div class="modal-title">// CHOISIS TON AVATAR</div>
    <div class="avatar-cats" id="avatarCats"></div>
    <div class="avatar-grid" id="avatarGrid"></div>
    <div style="display:flex;gap:8px;justify-content:flex-end">
      <button class="btn btn-ghost" onclick="closeAvatarPicker()">Annuler</button>
      <button class="btn" onclick="confirmAvatarPick()">CHOISIR</button>
    </div>
  </div>
</div>

<!-- CHARACTER EDIT MODAL -->

<div class="modal-overlay" id="charEditModal">
  <div class="modal" style="max-width:460px">
    <div class="modal-title">// MODIFIER LE PERSONNAGE</div>
    <div style="text-align:center;margin-bottom:18px">
      <div class="char-preview" id="edit-char-preview" onclick="openAvatarPicker('edit')" style="width:74px;height:74px;font-size:34px;margin-bottom:28px">⚔️</div>
    </div>
    <div class="modal-field">
      <label class="modal-label">Prénom</label>
      <input class="modal-input" id="edit-name" placeholder="Ton prénom…" maxlength="20" autocomplete="off">
    </div>
    <div class="modal-field">
      <label class="modal-label">Titre / Surnom</label>
      <input class="modal-input" id="edit-title" placeholder="Ex: Le Conquistador, La Phoenix…" maxlength="30" autocomplete="off">
    </div>
    <div class="modal-field">
      <label class="modal-label">Classe</label>
      <div class="char-class-grid" id="edit-class-grid" style="grid-template-columns:repeat(3,1fr)">
        <div class="char-class-card" data-class="Guerrier" onclick="selectClass(this,'edit')"><div class="char-class-icon">⚔️</div><div class="char-class-name">Guerrier</div></div>
        <div class="char-class-card" data-class="Mage" onclick="selectClass(this,'edit')"><div class="char-class-icon">🧙</div><div class="char-class-name">Mage</div></div>
        <div class="char-class-card" data-class="Rôdeur" onclick="selectClass(this,'edit')"><div class="char-class-icon">🏹</div><div class="char-class-name">Rôdeur</div></div>
        <div class="char-class-card" data-class="Alchimiste" onclick="selectClass(this,'edit')"><div class="char-class-icon">⚗️</div><div class="char-class-name">Alchimiste</div></div>
        <div class="char-class-card" data-class="Barde" onclick="selectClass(this,'edit')"><div class="char-class-icon">🎸</div><div class="char-class-name">Barde</div></div>
        <div class="char-class-card" data-class="Moine" onclick="selectClass(this,'edit')"><div class="char-class-icon">🧘</div><div class="char-class-name">Moine</div></div>
      </div>
    </div>
    <div class="modal-actions">
      <button class="btn btn-ghost" onclick="closeCharEditModal()">Annuler</button>
      <button class="btn" onclick="saveCharEdit()">SAUVEGARDER</button>
    </div>
  </div>
</div>

<script>
const INIT_VISIBLE = 2;
let state = {xp:0, categories:[], quests:[], activity:[], onboarded:false, playerName:'', playerLevel:'', playerAvatar:'⚔️', playerClass:'', playerTitle:''};
try{const s=localStorage.getItem('lifexp4');if(s)state=JSON.parse(s);}catch{}
if(!state.playerAvatar)state.playerAvatar='⚔️';
if(!state.playerClass)state.playerClass='';
if(!state.playerTitle)state.playerTitle='';
state.quests.forEach(q=>{if(q.visible===undefined)q.visible=true;});
function save(){try{localStorage.setItem('lifexp4',JSON.stringify(state));}catch(e){showToast('⚠️ Erreur de sauvegarde');}}

const LEVELS=[
  {min:0,label:'DÉBUTANT'},{min:100,label:'NOVICE'},{min:300,label:'AVENTURIER'},
  {min:600,label:'GUERRIER'},{min:1000,label:'EXPERT'},{min:1500,label:'MAÎTRE'},
  {min:2500,label:'ÉLITE'},{min:4000,label:'LÉGENDE'}
];
function getLvl(xp){
  let idx=0;
  for(let i=0;i<LEVELS.length;i++)if(xp>=LEVELS[i].min)idx=i;
  const isMax=idx===LEVELS.length-1;
  const prev=LEVELS[idx].min;
  const next=isMax?null:LEVELS[idx+1].min;
  const pct=isMax?100:Math.min(100,Math.round((xp-prev)/(next-prev)*100));
  return{lvl:idx+1,label:LEVELS[idx].label,next:isMax?'∞':next,pct};
}
let prevLvl=getLvl(state.xp).lvl;

let activeFilter='all';

function render(){
  const {lvl,label,next,pct}=getLvl(state.xp);
  const done=state.quests.filter(q=>q.done).length;
  document.getElementById('headerXp').textContent=state.xp+' XP';
  document.getElementById('playerLvl').textContent=`NIVEAU ${lvl} — ${label}`;
  document.getElementById('xpFrac').textContent=`${state.xp} / ${next} XP`;
  document.getElementById('xpFill').style.width=pct+'%';
  document.getElementById('sLvl').textContent=lvl;
  document.getElementById('sQuests').textContent=done;
  document.getElementById('sCats').textContent=state.categories.length;
  document.getElementById('sXP').textContent=state.xp;
  document.getElementById('profileLvl').textContent=`NIVEAU ${lvl} — ${label}`;
  document.getElementById('pXP').textContent=state.xp;
  document.getElementById('pLvl').textContent=lvl;
  document.getElementById('pQuests').textContent=done;
  // Avatar & character
  const av=state.playerAvatar||'⚔️';
  document.getElementById('mainAvatar').textContent=av;
  document.getElementById('profileAvatar').textContent=av;
  const classBadge=document.getElementById('profileClassBadge');
  if(state.playerClass){
    const classIcons={Guerrier:'⚔️',Mage:'🧙',Rôdeur:'🏹',Alchimiste:'⚗️',Barde:'🎸',Moine:'🧘'};
    const icon=classIcons[state.playerClass]||'⭐';
    classBadge.innerHTML=`<div class="char-title-badge">${icon} ${state.playerClass.toUpperCase()}${state.playerTitle?' · '+state.playerTitle:''}</div>`;
  } else {classBadge.innerHTML='';}
  if(state.playerName){
    document.getElementById('playerNameLbl').textContent=state.playerName.toUpperCase();
    document.getElementById('profileName').textContent=state.playerName.toUpperCase();
  }
  renderCats();renderQuests();renderActivity();renderProfileCats();save();
  const newLvl=getLvl(state.xp).lvl;
  if(newLvl>prevLvl){
    document.getElementById('levelupLabel').textContent=`NIVEAU ${newLvl} — ${getLvl(state.xp).label}`;
    document.getElementById('levelupPopup').classList.add('show');
  }
  prevLvl=newLvl;
}

function renderCats(){
  const el=document.getElementById('catList');
  if(!state.categories.length){el.innerHTML='<div class="empty-state"><div class="empty-icon">⚔️</div>Aucune catégorie.<br>Crée-en une pour commencer !</div>';return;}
  el.innerHTML=state.categories.map((cat,ci)=>{
    const allQ=state.quests.filter(q=>q.catId===ci);
    const catXP=allQ.filter(q=>q.done).reduce((s,q)=>s+q.xp,0);
    const maxXP=allQ.reduce((s,q)=>s+q.xp,0)||100;
    const visibleQ=allQ.filter(q=>q.visible);
    const lockedCount=allQ.filter(q=>!q.visible).length;
    const lockedBadge=lockedCount>0?`<span style="font-family:var(--fmono);font-size:9px;color:var(--text3)">🔒 ${lockedCount}</span>`:'';
    const skills=visibleQ.map(q=>`<div class="skill-tag">${q.name}<span class="skill-xp">+${q.xp}</span></div>`).join('');
    return`<div class="card">
      <div class="cat-header">
        <span class="cat-emoji">${cat.emoji}</span>
        <span class="cat-name">${cat.name}</span>
        <span class="cat-xp">${catXP} XP</span>
        ${lockedBadge}
        <button class="icon-btn" onclick="confirmDeleteCat(${ci})">🗑</button>
      </div>
      <div class="cat-bar"><div class="cat-fill" style="width:${Math.round(catXP/maxXP*100)}%"></div></div>
      <div class="skill-list">${skills||'<span style="font-family:var(--fmono);font-size:10px;color:var(--text3)">Aucune quête visible</span>'}</div>
    </div>`;
  }).join('');
}

function renderQuests(){
  const el=document.getElementById('questList');
  const filterEl=document.getElementById('filterTabs');
  if(state.categories.length>0){
    const cats=[{id:'all',label:'Toutes',emoji:''},...state.categories.map((c,i)=>({id:i,label:c.name,emoji:c.emoji}))];
    filterEl.innerHTML=cats.map(c=>`<button class="filter-tab ${activeFilter===c.id?'active':''}" onclick="setFilter('${c.id}')">${c.emoji?c.emoji+' ':''}${c.label}</button>`).join('');
  }
  let filtered=state.quests.map((q,i)=>({...q,_i:i}));
  if(activeFilter!=='all')filtered=filtered.filter(q=>q.catId===parseInt(activeFilter));
  if(!filtered.length&&!state.quests.length){
    el.innerHTML='<div class="empty-state"><div class="empty-icon">📋</div>Aucune quête.<br>Lance-toi dans ta première mission !</div>';
    document.getElementById('questViewHeader').style.display='none';
    return;
  }
  const visibleQ=state.quests.filter(q=>q.visible);
  const doneVisible=visibleQ.filter(q=>q.done).length;
  const totalVisible=visibleQ.length;
  const hdr=document.getElementById('questViewHeader');
  if(totalVisible>0){
    hdr.style.display='flex';
    document.getElementById('questViewInfo').innerHTML=`<strong>${doneVisible}</strong> / ${totalVisible} quêtes actives complétées`;
    document.getElementById('questProgressFill').style.width=Math.round(doneVisible/totalVisible*100)+'%';
  } else { hdr.style.display='none'; }

  if(activeFilter==='all'){
    const groups={};
    filtered.forEach(q=>{if(!groups[q.catId])groups[q.catId]=[];groups[q.catId].push(q);});
    let html='';
    Object.entries(groups).forEach(([cid,quests])=>{
      const cat=state.categories[cid];if(!cat)return;
      html+=`<div style="margin-bottom:20px">
        <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px">
          <span style="font-size:16px">${cat.emoji}</span>
          <span style="font-family:var(--fhead);font-size:7px;color:var(--text2);letter-spacing:1px">${cat.name.toUpperCase()}</span>
        </div>`;
      html+=renderQuestGroup(quests,parseInt(cid));
      html+='</div>';
    });
    el.innerHTML=html||'<div class="empty-state"><div class="empty-icon">📋</div>Aucune quête.</div>';
  } else {
    el.innerHTML=renderQuestGroup(filtered,parseInt(activeFilter));
  }
}

function renderQuestGroup(quests,catId){
  const visible=quests.filter(q=>q.visible);
  const locked=quests.filter(q=>!q.visible);
  let html=visible.map(q=>`
    <div class="quest-card ${q.done?'done':''}" id="quest-${q._i}">
      <button class="quest-check ${q.done?'checked':''}" onclick="toggleQuest(${q._i})">✓</button>
      <div class="quest-info">
        <div class="quest-name" style="${q.done?'text-decoration:line-through':''}">${q.name}</div>
        <div class="quest-meta">${state.categories[q.catId]?.name??'—'}</div>
      </div>
      <div class="quest-xp">+${q.xp} XP</div>
      <button class="icon-btn" onclick="confirmDeleteQuest(${q._i})">🗑</button>
    </div>`).join('');
  if(locked.length){
    const next=locked[0];
    html+=`<div class="unlock-row">
      <div class="unlock-line"></div>
      <button class="unlock-btn" onclick="unlockNextQuest(${catId})">🔓 Débloquer · <span style="color:var(--accent)">${next.name.substring(0,22)}${next.name.length>22?'…':''}</span></button>
      <div class="unlock-line"></div>
    </div>
    <div class="locked-preview"><span style="font-size:14px">🔒</span><span class="locked-text">${locked.length} quête${locked.length>1?'s':''} verrouillée${locked.length>1?'s':''}</span></div>`;
  }
  return html;
}

function setFilter(id){activeFilter=id==='all'?'all':parseInt(id);renderQuests();}

function renderProfileCats(){
  const el=document.getElementById('profileCats');
  if(!state.categories.length){el.innerHTML='<div style="font-family:var(--fmono);font-size:11px;color:var(--text3);text-align:center;padding:16px">Aucune catégorie</div>';return;}
  el.innerHTML=state.categories.map((cat,ci)=>{
    const allQ=state.quests.filter(q=>q.catId===ci);
    const catXP=allQ.filter(q=>q.done).reduce((s,q)=>s+q.xp,0);
    const maxXP=allQ.reduce((s,q)=>s+q.xp,0)||1;
    const pct=Math.round(catXP/maxXP*100);
    return`<div style="margin-bottom:14px">
      <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:5px">
        <span style="font-size:13px;font-weight:600">${cat.emoji} ${cat.name}</span>
        <span style="font-family:var(--fmono);font-size:10px;color:var(--accent)">${catXP} XP · ${pct}%</span>
      </div>
      <div style="height:5px;background:var(--border);border-radius:3px;overflow:hidden">
        <div style="height:100%;width:${pct}%;background:var(--grad);border-radius:3px;transition:width .5s"></div>
      </div>
    </div>`;
  }).join('');
}

function renderActivity(){
  const el=document.getElementById('activityLog');
  if(!state.activity.length){el.innerHTML='<div class="empty-state"><div class="empty-icon">⚡</div>Aucune activité.</div>';return;}
  el.innerHTML=[...state.activity].reverse().map(a=>`
    <div class="activity-item">
      <div class="activity-icon">${a.icon}</div>
      <div style="flex:1"><div style="font-size:13px;color:var(--text2)">${a.text}</div><div class="activity-time">${a.time}</div></div>
      ${a.xp!==0?`<div class="activity-xp" style="color:${a.xp>0?'var(--accent)':'var(--danger)'}">${a.xp>0?'+':''}${a.xp} XP</div>`:''}
    </div>`).join('');
}

function unlockNextQuest(catId){
  const locked=state.quests.map((q,i)=>({...q,_i:i})).filter(q=>q.catId===catId&&!q.visible);
  if(!locked.length)return;
  const next=locked[0];
  state.quests[next._i].visible=true;
  log(`Quête déverrouillée : ${next.name}`,'🔓',0);
  showToast(`🔓 Nouvelle quête : ${next.name}`);
  render();
  setTimeout(()=>{const card=document.getElementById(`quest-${next._i}`);if(card)card.classList.add('new-unlock');},50);
}

function addCategory(name,emoji){state.categories.push({name,emoji:emoji||'📁'});log(`Catégorie "${name}" créée`,emoji||'📁',0);render();}
function deleteCat(i){
  state.categories.splice(i,1);
  state.quests=state.quests.filter(q=>q.catId!==i).map(q=>({...q,catId:q.catId>i?q.catId-1:q.catId}));
  state.xp=state.quests.filter(q=>q.done).reduce((s,q)=>s+q.xp,0);
  render();
}
function addQuest(name,xp,catId,visible=true){state.quests.push({name,xp:parseInt(xp)||50,catId:parseInt(catId),done:false,visible});log(`Quête "${name}" ajoutée`,'📋',0);render();}
function toggleQuest(i){
  const q=state.quests[i];
  if(!q.visible)return;
  q.done=!q.done;
  state.xp=state.quests.filter(q=>q.done).reduce((s,q)=>s+q.xp,0);
  if(q.done)showToast(`+${q.xp} XP — ${q.name}`);
  log(q.done?`Quête complétée : ${q.name}`:`Quête annulée : ${q.name}`,q.done?'✅':'↩️',q.done?q.xp:-q.xp);
  render();
}
function deleteQuest(i){state.quests.splice(i,1);state.xp=state.quests.filter(q=>q.done).reduce((s,q)=>s+q.xp,0);render();}
function clearActivity(){state.activity=[];render();}
function log(text,icon,xp){
  const time=new Date().toLocaleTimeString('fr-FR',{hour:'2-digit',minute:'2-digit'});
  state.activity.push({text,icon,xp,time});
  if(state.activity.length>200)state.activity.splice(0,state.activity.length-200);
}

function showToast(msg){
  const t=document.getElementById('toast');
  t.textContent=msg;t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),2800);
}

let _confirmCb=null;
function openConfirm(icon,title,text,okLabel,cb){
  document.getElementById('confirmIcon').textContent=icon;
  document.getElementById('confirmTitle').textContent=title;
  document.getElementById('confirmText').textContent=text;
  document.getElementById('confirmOkBtn').textContent=okLabel;
  _confirmCb=cb;
  document.getElementById('confirmModal').classList.add('open');
}
function closeConfirm(){document.getElementById('confirmModal').classList.remove('open');_confirmCb=null;}
document.getElementById('confirmOkBtn').addEventListener('click',()=>{if(_confirmCb)_confirmCb();closeConfirm();});
document.getElementById('confirmModal').addEventListener('click',function(e){if(e.target===this)closeConfirm();});

function confirmDeleteCat(i){
  const cat=state.categories[i];
  const n=state.quests.filter(q=>q.catId===i).length;
  openConfirm('🗑','SUPPRIMER LA CATÉGORIE',`"${cat.emoji} ${cat.name}" et ses ${n} quête(s) seront définitivement supprimées.`,'🗑 Supprimer',()=>deleteCat(i));
}
function confirmDeleteQuest(i){
  const q=state.quests[i];
  openConfirm('🗑','SUPPRIMER LA QUÊTE',`"${q.name}" (${q.xp} XP) sera supprimée définitivement.`,'🗑 Supprimer',()=>deleteQuest(i));
}
function confirmClearActivity(){
  openConfirm('🗑','EFFACER LE JOURNAL','Tout l\'historique sera effacé. Tes XP et quêtes sont conservés.','🗑 Effacer',clearActivity);
}

let currentTheme=localStorage.getItem('lifexp_theme')||'cyber';
document.body.dataset.theme=currentTheme;
document.querySelectorAll('.theme-opt').forEach(o=>o.classList.toggle('active',o.dataset.theme===currentTheme));
document.getElementById('themeBtn').addEventListener('click',e=>{
  e.stopPropagation();
  document.getElementById('themeMenu').classList.toggle('open');
  document.getElementById('themeBtn').classList.toggle('open');
});
document.querySelectorAll('.theme-opt').forEach(opt=>{
  opt.addEventListener('click',()=>{
    currentTheme=opt.dataset.theme;
    document.body.dataset.theme=currentTheme;
    localStorage.setItem('lifexp_theme',currentTheme);
    document.querySelectorAll('.theme-opt').forEach(o=>o.classList.toggle('active',o.dataset.theme===currentTheme));
    document.getElementById('themeMenu').classList.remove('open');
    document.getElementById('themeBtn').classList.remove('open');
    showToast(`Thème "${opt.textContent.trim()}" activé !`);
  });
});
document.addEventListener('click',()=>{
  document.getElementById('themeMenu').classList.remove('open');
  document.getElementById('themeBtn').classList.remove('open');
});

document.querySelectorAll('.tab-btn').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
    document.querySelectorAll('.tab-panel').forEach(p=>p.classList.remove('active'));
    btn.classList.add('active');
    document.getElementById('tab-'+btn.dataset.tab).classList.add('active');
  });
});

function openCatModal(){document.getElementById('catModal').classList.add('open');setTimeout(()=>document.getElementById('catName').focus(),100);}
function closeCatModal(){document.getElementById('catModal').classList.remove('open');}
function confirmCat(){
  const n=document.getElementById('catName').value.trim();
  const e=document.getElementById('catEmoji').value.trim()||'📁';
  if(!n)return;
  addCategory(n,e);
  document.getElementById('catName').value='';document.getElementById('catEmoji').value='';
  closeCatModal();
}
document.getElementById('catModal').addEventListener('click',function(e){if(e.target===this)closeCatModal();});

function openQuestModal(){
  if(!state.categories.length){showToast("Crée d'abord une catégorie !");return;}
  const sel=document.getElementById('questCat');
  sel.innerHTML=state.categories.map((c,i)=>`<option value="${i}">${c.emoji} ${c.name}</option>`).join('');
  document.getElementById('questModal').classList.add('open');
  setTimeout(()=>document.getElementById('questName').focus(),100);
}
function closeQuestModal(){document.getElementById('questModal').classList.remove('open');}
function confirmQuest(){
  const n=document.getElementById('questName').value.trim();
  const x=document.getElementById('questXp').value||50;
  const c=document.getElementById('questCat').value;
  if(!n)return;
  addQuest(n,x,c,true);
  document.getElementById('questName').value='';document.getElementById('questXp').value='';
  closeQuestModal();
}
document.getElementById('questModal').addEventListener('click',function(e){if(e.target===this)closeQuestModal();});

// ONBOARDING
let obSelectedDomains=[];
let obPlayerName='';
let obLevel='';
let obAvatar='⚔️';
let obClass='';

function obNext(step){
  if(step===1){obPlayerName=document.getElementById('ob-name').value.trim()||'JOUEUR';goToStep('char');}
  else if(step==='char'){goToStep(2);}
  else if(step===2){if(!obSelectedDomains.length)return;goToStep(3);}
  else if(step===3){
    document.querySelectorAll('.ob-custom-row').forEach(row=>{
      const emoji=row.querySelector('.ob-custom-emoji').value.trim()||'⭐';
      const name=row.querySelector('.ob-custom-name').value.trim();
      if(name)obSelectedDomains.push({emoji,name,custom:true});
    });
    goToStep(4);
  } else if(step===4){if(!obLevel)return;finishOnboarding();}
}

function goToStep(n){document.querySelectorAll('.ob-step').forEach(s=>s.classList.remove('active'));document.getElementById('ob-step-'+n).classList.add('active');}

function toggleDomain(el){
  const name=el.dataset.name,emoji=el.dataset.emoji;
  el.classList.toggle('selected');
  if(el.classList.contains('selected'))obSelectedDomains.push({name,emoji});
  else obSelectedDomains=obSelectedDomains.filter(d=>d.name!==name);
  const count=obSelectedDomains.length;
  document.getElementById('ob-count').textContent=count+(count<=1?' domaine sélectionné':' domaines sélectionnés');
  document.getElementById('ob-btn-2').disabled=count===0;
}

function selectLevel(el){
  document.querySelectorAll('[data-level]').forEach(e=>e.classList.remove('selected'));
  el.classList.add('selected');
  obLevel=el.dataset.level;
  document.getElementById('ob-btn-4').disabled=false;
}

function selectClass(el,context){
  const grid=document.getElementById(context+'-class-grid');
  grid.querySelectorAll('.char-class-card').forEach(c=>c.classList.remove('selected'));
  el.classList.add('selected');
  const cls=el.dataset.class;
  if(context==='ob'){
    obClass=cls;
    document.getElementById('ob-btn-char').disabled=false;
  } else if(context==='edit'){
    // stored in temp var when saving
  }
}

function addCustomDomain(){
  const container=document.getElementById('ob-custom-domains');
  const row=document.createElement('div');
  row.className='ob-custom-row';
  row.innerHTML=`<input class="ob-custom-emoji" placeholder="⭐" maxlength="2"><input class="ob-custom-name" placeholder="Nom du domaine…" autocomplete="off"><button onclick="this.parentElement.remove()" style="background:none;border:none;color:var(--danger);cursor:pointer;font-size:18px;padding:4px">✕</button>`;
  container.appendChild(row);
  row.querySelector('.ob-custom-name').focus();
}

// ════════════════════════════════════════
// AVATAR PICKER
// ════════════════════════════════════════
const AVATAR_CATS={
  'Guerriers':['⚔️','🗡️','🛡️','🏹','🪃','🔱','⚙️','🦾','💪','🤺'],
  'Mages':['🧙','🧝','🧚','🔮','✨','🌟','💫','⚡','🔥','❄️'],
  'Animaux':['🦊','🐺','🦁','🐉','🦅','🐈','🐻','🦋','🦄','🐸'],
  'Tech':['🤖','👾','🎮','💻','🕹️','📡','🛸','🧬','⚗️','🔬'],
  'Style':['😎','🥷','👑','🎩','🦸','🦹','🧟','🧛','🧜','🧞'],
  'Nature':['🌊','⛰️','🌸','🌿','🍄','☀️','🌙','🌈','🌀','💎'],
  'Autres':['🎭','🎪','🎯','🏆','🚀','💣','🎲','🃏','🎸','🎨'],
};
let _avatarContext='ob';
let _avatarSelected='⚔️';
let _avatarActiveCat='Guerriers';

function openAvatarPicker(context){
  _avatarContext=context;
  _avatarSelected=context==='ob'?obAvatar:(state.playerAvatar||'⚔️');
  document.getElementById('avatarModal').classList.add('open');
  renderAvatarCats();
  renderAvatarGrid(_avatarActiveCat);
}
function closeAvatarPicker(){document.getElementById('avatarModal').classList.remove('open');}
function renderAvatarCats(){
  const el=document.getElementById('avatarCats');
  el.innerHTML=Object.keys(AVATAR_CATS).map(cat=>`<button class="avatar-cat-btn ${cat===_avatarActiveCat?'active':''}" onclick="switchAvatarCat('${cat}')">${cat}</button>`).join('');
}
function switchAvatarCat(cat){_avatarActiveCat=cat;renderAvatarCats();renderAvatarGrid(cat);}
function renderAvatarGrid(cat){
  const el=document.getElementById('avatarGrid');
  el.innerHTML=AVATAR_CATS[cat].map(em=>`<div class="avatar-option ${em===_avatarSelected?'selected':''}" onclick="pickAvatar('${em}')">${em}</div>`).join('');
}
function pickAvatar(em){
  _avatarSelected=em;
  renderAvatarGrid(_avatarActiveCat);
}
function confirmAvatarPick(){
  if(_avatarContext==='ob'){
    obAvatar=_avatarSelected;
    document.getElementById('ob-char-preview').childNodes[0].textContent=obAvatar;
    document.querySelector('#ob-char-preview').firstChild.nodeValue=obAvatar;
    document.getElementById('ob-char-preview').innerHTML=obAvatar+'<div class="char-preview-hint">CLIQUER POUR CHANGER</div>';
  } else if(_avatarContext==='edit'){
    document.getElementById('edit-char-preview').innerHTML=_avatarSelected+'<div class="char-preview-hint">CLIQUER POUR CHANGER</div>';
  }
  closeAvatarPicker();
}

// ════════════════════════════════════════
// CHARACTER EDIT MODAL
// ════════════════════════════════════════
function openCharEditModal(){
  document.getElementById('edit-name').value=state.playerName||'';
  document.getElementById('edit-title').value=state.playerTitle||'';
  const preview=document.getElementById('edit-char-preview');
  preview.innerHTML=(state.playerAvatar||'⚔️')+'<div class="char-preview-hint">CLIQUER POUR CHANGER</div>';
  // highlight current class
  document.getElementById('edit-class-grid').querySelectorAll('.char-class-card').forEach(c=>{
    c.classList.toggle('selected',c.dataset.class===state.playerClass);
  });
  document.getElementById('charEditModal').classList.add('open');
}
function closeCharEditModal(){document.getElementById('charEditModal').classList.remove('open');}
function saveCharEdit(){
  const name=document.getElementById('edit-name').value.trim();
  const title=document.getElementById('edit-title').value.trim();
  const selected=document.getElementById('edit-class-grid').querySelector('.char-class-card.selected');
  const cls=selected?selected.dataset.class:state.playerClass;
  const avatarEl=document.getElementById('edit-char-preview');
  // extract emoji (first text node)
  const newAvatar=avatarEl.textContent.replace('CLIQUER POUR CHANGER','').trim()||state.playerAvatar||'⚔️';
  if(name)state.playerName=name;
  state.playerTitle=title;
  state.playerClass=cls||state.playerClass;
  state.playerAvatar=newAvatar;
  closeCharEditModal();
  render();
  showToast('✅ Personnage mis à jour !');
}

const QUESTS_DB={
  'Tech & IA':{
    debutant:[{name:"Regarder une vidéo d'intro à Python",xp:20},{name:'Faire un exercice sur freeCodeCamp',xp:25},{name:'Lire un article tech en anglais',xp:20},{name:"Tester un outil IA (ChatGPT, Gemini…)",xp:15},{name:'Apprendre 5 commandes terminal',xp:25}],
    intermediaire:[{name:'Coder un mini projet perso (30 min)',xp:50},{name:"Lire la doc officielle d'un framework",xp:35},{name:'Résoudre un exercice algorithmique',xp:40},{name:'Regarder un cours sur YouTube (1h)',xp:30},{name:'Contribuer à un projet open source',xp:60}],
    avance:[{name:'Implémenter une feature complexe',xp:80},{name:'Faire une code review sérieuse',xp:60},{name:'Écrire un article technique',xp:70},{name:"Optimiser les performances d'un projet",xp:75},{name:'Créer et déployer une API',xp:90}],
  },
  'Business':{
    debutant:[{name:"Lire 10 pages d'un livre business",xp:20},{name:'Écouter un podcast entrepreneur',xp:15},{name:"Analyser le business model d'une marque",xp:30},{name:'Rédiger une idée de projet en 1 page',xp:35},{name:'Suivre un compte LinkedIn inspirant',xp:10}],
    intermediaire:[{name:'Créer un business plan simplifié',xp:60},{name:'Faire une veille concurrentielle',xp:40},{name:'Rédiger une proposition commerciale',xp:50},{name:"Analyser des KPIs d'une campagne",xp:45},{name:'Interviewer un entrepreneur',xp:55}],
    avance:[{name:'Pitcher un projet devant un public',xp:80},{name:'Négocier un contrat ou partenariat',xp:75},{name:'Lancer une campagne marketing',xp:90},{name:"Analyser les finances d'un projet",xp:70},{name:'Créer une stratégie de croissance',xp:85}],
  },
  'Santé & Sport':{
    debutant:[{name:'Faire 20 min de marche',xp:20},{name:"Boire 2L d'eau dans la journée",xp:15},{name:"Faire 10 min d'étirements",xp:20},{name:'Dormir 7h minimum',xp:25},{name:'Préparer un repas équilibré',xp:25}],
    intermediaire:[{name:'Séance de sport complète (45 min)',xp:50},{name:'Méditer 15 min',xp:30},{name:'Préparer ses repas pour la semaine',xp:45},{name:'Faire un bilan de sa semaine sportive',xp:30},{name:'Apprendre un nouvel exercice',xp:35}],
    avance:[{name:"Séance d'entraînement intensif (1h)",xp:70},{name:'Suivre un plan nutritionnel strict',xp:60},{name:'Battre un record personnel',xp:80},{name:'Préparer une compétition ou événement',xp:75},{name:"Coacher quelqu'un dans sa pratique",xp:65}],
  },
  'Langues':{
    debutant:[{name:'15 min de Duolingo',xp:20},{name:'Apprendre 10 nouveaux mots',xp:20},{name:'Regarder une série en VO sous-titrée',xp:25},{name:'Écouter une chanson et traduire les paroles',xp:20},{name:'Lire un article simple en langue étrangère',xp:25}],
    intermediaire:[{name:'Écrire un texte court en anglais',xp:40},{name:'Regarder un film en VO sans sous-titres',xp:50},{name:'Avoir une conversation de 10 min',xp:55},{name:'Lire un chapitre de livre en anglais',xp:45},{name:'Écouter un podcast en langue étrangère',xp:35}],
    avance:[{name:'Écrire un article complet en anglais',xp:70},{name:'Présenter un exposé en langue étrangère',xp:80},{name:"Débattre d'un sujet complexe",xp:75},{name:'Lire un livre entier en VO',xp:85},{name:'Passer une certification de langue',xp:90}],
  },
  'Finance':{
    debutant:[{name:'Noter toutes ses dépenses du jour',xp:15},{name:'Créer un budget mensuel simple',xp:35},{name:"Lire un article sur l'épargne",xp:20},{name:'Identifier une dépense inutile',xp:20},{name:"Comprendre ce qu'est un ETF",xp:25}],
    intermediaire:[{name:'Analyser ses dépenses du mois',xp:40},{name:'Comparer des offres de placement',xp:45},{name:'Mettre en place un virement automatique',xp:50},{name:'Lire un rapport financier simplifié',xp:40},{name:"Calculer son taux d'épargne",xp:35}],
    avance:[{name:'Analyser une action en bourse',xp:70},{name:'Rebalancer son portefeuille',xp:75},{name:'Créer un plan financier sur 5 ans',xp:80},{name:'Étudier la fiscalité de ses placements',xp:65},{name:"Optimiser sa déclaration d'impôts",xp:70}],
  },
  'Créativité':{
    debutant:[{name:'Faire un post créatif sur les réseaux',xp:20},{name:'Apprendre les bases de Canva',xp:25},{name:'Écrire un texte court (poème, histoire)',xp:25},{name:'Prendre 5 photos avec intention artistique',xp:20},{name:'Explorer Figma ou Photoshop 30 min',xp:30}],
    intermediaire:[{name:'Créer un visuel complet pour un projet',xp:45},{name:'Écrire et publier un article de blog',xp:50},{name:'Monter une courte vidéo',xp:55},{name:'Créer une identité visuelle simple',xp:60},{name:'Finir un mini projet créatif de A à Z',xp:65}],
    avance:[{name:'Créer un portfolio en ligne',xp:80},{name:'Produire un contenu viral',xp:75},{name:'Designer une interface complète',xp:85},{name:'Diriger une session créative en équipe',xp:70},{name:'Présenter son travail à un client',xp:80}],
  },
  'Études':{
    debutant:[{name:'Réviser 30 min de cours',xp:25},{name:'Faire une fiche de révision',xp:25},{name:'Lire un chapitre en avance',xp:20},{name:'Refaire des exercices ratés',xp:30},{name:'Organiser ses notes de cours',xp:20}],
    intermediaire:[{name:'Préparer un exposé ou projet',xp:55},{name:'Faire un QCM blanc',xp:45},{name:"Expliquer un concept à quelqu'un",xp:40},{name:'Faire 2h de révisions intensives',xp:60},{name:'Rédiger un plan de dissertation',xp:45}],
    avance:[{name:'Rédiger un dossier complet',xp:75},{name:'Préparer et passer un examen blanc',xp:80},{name:'Faire de la veille dans son domaine',xp:55},{name:'Présenter devant un jury fictif',xp:70},{name:'Créer ses propres supports de cours',xp:65}],
  },
  'Social':{
    debutant:[{name:"Engager une conversation avec quelqu'un de nouveau",xp:20},{name:'Envoyer un message de soutien à un ami',xp:15},{name:'Rejoindre une communauté en ligne',xp:20},{name:'Partager un contenu utile sur les réseaux',xp:15},{name:'Assister à un événement local',xp:30}],
    intermediaire:[{name:'Prendre la parole en public',xp:50},{name:'Organiser une rencontre avec des amis',xp:35},{name:"Faire du networking lors d'un événement",xp:55},{name:'Donner un feedback constructif',xp:40},{name:'Résoudre un conflit de façon positive',xp:50}],
    avance:[{name:'Animer un groupe ou atelier',xp:70},{name:'Faire un discours ou présentation',xp:75},{name:"Mentorer quelqu'un pendant 1h",xp:65},{name:'Organiser un événement de A à Z',xp:80},{name:'Développer un partenariat professionnel',xp:75}],
  },
  'Mindset':{
    debutant:[{name:'Méditer 10 min',xp:20},{name:'Écrire 3 choses positives de la journée',xp:15},{name:'Lire 15 min de développement perso',xp:20},{name:'Faire une to-do list la veille',xp:15},{name:'Identifier une peur et la nommer',xp:20}],
    intermediaire:[{name:'Journaling de 20 min',xp:30},{name:'Faire une revue hebdomadaire',xp:40},{name:'Sortir de sa zone de confort',xp:50},{name:'Suivre une routine matinale complète',xp:45},{name:'Écouter un podcast de développement perso',xp:25}],
    avance:[{name:'Définir ses valeurs fondamentales',xp:60},{name:'Créer un plan de vie sur 1 an',xp:75},{name:'Tenir un journal pendant 30 jours',xp:80},{name:'Challenger une croyance limitante',xp:65},{name:"Coacher quelqu'un sur ses objectifs",xp:70}],
  },
  'Musique':{
    debutant:[{name:'Pratiquer son instrument 20 min',xp:25},{name:'Apprendre un nouvel accord ou note',xp:20},{name:'Écouter et analyser une chanson',xp:15},{name:'Regarder un tuto musical',xp:20},{name:'Chanter ou fredonner consciemment',xp:15}],
    intermediaire:[{name:'Apprendre une chanson complète',xp:55},{name:'Enregistrer une courte performance',xp:50},{name:'Improviser 10 min',xp:40},{name:'Travailler un morceau difficile',xp:45},{name:"Jouer avec quelqu'un d'autre",xp:50}],
    avance:[{name:'Composer un morceau original',xp:80},{name:'Produire et mixer un son',xp:75},{name:'Se produire devant un public',xp:85},{name:'Enregistrer en studio',xp:90},{name:"Enseigner une technique à quelqu'un",xp:65}],
  },
};
const DEFAULT_QUESTS={
  debutant:[{name:'Consacrer 20 min à ce domaine',xp:20},{name:'Lire un article ou regarder une vidéo',xp:20},{name:"Prendre des notes sur ce que j'ai appris",xp:15},{name:"Partager une découverte avec quelqu'un",xp:15},{name:'Définir un objectif pour cette semaine',xp:25}],
  intermediaire:[{name:'Pratiquer 45 min dans ce domaine',xp:45},{name:'Créer un mini projet ou exercice',xp:50},{name:"Faire une revue de ce que j'ai appris",xp:35},{name:'Trouver un mentor ou une ressource avancée',xp:40},{name:'Tester une nouvelle approche',xp:45}],
  avance:[{name:'Travailler 1h sur un projet complexe',xp:70},{name:"Enseigner ou expliquer à quelqu'un",xp:65},{name:'Créer une ressource de qualité',xp:75},{name:'Challenger mes propres limites',xp:70},{name:'Contribuer à la communauté de ce domaine',xp:65}],
};

function getQuestsForDomain(domain){
  const lvl=obLevel||'debutant';
  const db=QUESTS_DB[domain.name];
  return(db&&db[lvl])?db[lvl]:DEFAULT_QUESTS[lvl];
}

function finishOnboarding(){
  obSelectedDomains.forEach((d,ci)=>{
    state.categories.push({name:d.name,emoji:d.emoji});
    const quests=getQuestsForDomain(d);
    quests.forEach((q,qi)=>{
      state.quests.push({name:q.name,xp:q.xp,catId:ci,done:false,visible:qi<INIT_VISIBLE});
    });
  });
  state.playerName=obPlayerName;
  state.playerLevel=obLevel;
  state.playerAvatar=obAvatar||'⚔️';
  state.playerClass=obClass||'';
  state.onboarded=true;
  save();
  document.getElementById('onboarding').classList.add('hidden');
  render();
  showToast(`Bienvenue ${obPlayerName} ! ⚔️ C'est parti !`);
}

if(state.onboarded){
  document.getElementById('onboarding').classList.add('hidden');
  if(state.playerName)document.getElementById('playerNameLbl').textContent=state.playerName.toUpperCase();
}
document.getElementById('avatarModal').addEventListener('click',function(e){if(e.target===this)closeAvatarPicker();});
document.getElementById('charEditModal').addEventListener('click',function(e){if(e.target===this)closeCharEditModal();});
render();
</script>

</body>
</html>
