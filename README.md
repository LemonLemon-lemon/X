<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>柠檬AI助教 · 数智化成长诊断平台</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;500;600;700;800&display=swap" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root{
    --bg:#eaf0f8;
    --surface:#ffffff;
    --surface-2:#f6f9fd;
    --surface-3:#eef3fa;
    --ink:#15233d;
    --ink-2:#5c6a86;
    --ink-3:#8a96ad;
    --line:#e5ecf5;
    --line-2:#d9e2ef;
    --lemon:#ffd23d;
    --lemon-deep:#f5b400;
    --lemon-soft:#fff6d6;
    --teal:#16b6a4;
    --blue:#3d6ef0;
    --indigo:#4f46e5;
    --weak:#ef5a6a;
    --growth:#f4a52c;
    --strong:#16b884;
    --elite:#3d6ef0;
    --shadow:0 1px 2px rgba(20,40,80,.04), 0 8px 28px rgba(20,40,80,.07);
    --shadow-lg:0 12px 40px rgba(20,40,80,.14);
    --r:18px;
    --r-sm:12px;
    --font-d:"Outfit","PingFang SC","Microsoft YaHei","Source Han Sans CN",system-ui,sans-serif;
    --font-b:"PingFang SC","Microsoft YaHei","Source Han Sans CN","Outfit",system-ui,sans-serif;
  }
  *{box-sizing:border-box;margin:0;padding:0}
  html,body{height:100%}
  body{
    font-family:var(--font-b);color:var(--ink);
    background:
      radial-gradient(1200px 600px at 88% -8%, rgba(255,210,61,.18), transparent 60%),
      radial-gradient(900px 500px at -5% 110%, rgba(22,182,164,.14), transparent 55%),
      var(--bg);
    -webkit-font-smoothing:antialiased;
    line-height:1.5;
  }
  .app{display:flex;min-height:100vh}

  /* ============ TOP BAR ============ */
  .topbar{
    position:fixed;top:0;left:0;right:0;height:64px;z-index:40;
    display:flex;align-items:center;gap:18px;padding:0 24px;
    background:rgba(255,255,255,.82);backdrop-filter:blur(14px);
    border-bottom:1px solid var(--line);
  }
  .brand{display:flex;align-items:center;gap:12px;min-width:230px}
  .logo{
    width:40px;height:40px;border-radius:12px;display:grid;place-items:center;font-size:22px;
    background:linear-gradient(140deg,#ffe071,#f5b400);
    box-shadow:0 6px 16px rgba(245,180,0,.35);
  }
  .brand h1{font-family:var(--font-d);font-size:18px;font-weight:800;letter-spacing:.3px;line-height:1.05}
  .brand small{font-size:11px;color:var(--ink-3);font-weight:600;letter-spacing:.5px}
  .topbar-spacer{flex:1}
  .role-switch{display:flex;gap:4px;background:var(--surface-3);padding:4px;border-radius:12px}
  .role-switch button{
    border:0;background:transparent;font-family:var(--font-b);font-weight:600;font-size:13px;color:var(--ink-2);
    padding:8px 14px;border-radius:9px;cursor:pointer;transition:.18s;white-space:nowrap;
  }
  .role-switch button:hover{color:var(--ink)}
  .role-switch button.active{background:var(--surface);color:var(--ink);box-shadow:0 2px 8px rgba(20,40,80,.12)}
  .role-switch button.active .dot{background:var(--lemon-deep)}
  .who{display:flex;align-items:center;gap:10px;padding-left:8px}
  .who .avatar{width:34px;height:34px;border-radius:50%;background:linear-gradient(140deg,#3d6ef0,#16b6a4);color:#fff;display:grid;place-items:center;font-weight:700;font-size:13px}
  .who .meta{font-size:12px;line-height:1.2}
  .who .meta b{display:block;font-size:13px}
  .who .meta span{color:var(--ink-3)}
  .stu-pick{font-family:var(--font-b);font-size:13px;border:1px solid var(--line-2);background:var(--surface);border-radius:10px;padding:7px 10px;color:var(--ink);font-weight:600;cursor:pointer;max-width:130px}

  /* ============ SIDEBAR ============ */
  .sidebar{
    position:fixed;top:64px;left:0;bottom:0;width:230px;z-index:30;
    background:rgba(255,255,255,.6);backdrop-filter:blur(8px);
    border-right:1px solid var(--line);padding:20px 14px;overflow-y:auto;
  }
  .nav-label{font-size:11px;font-weight:700;color:var(--ink-3);letter-spacing:1px;padding:4px 12px 8px;text-transform:uppercase}
  .nav-item{
    display:flex;align-items:center;gap:11px;padding:11px 13px;border-radius:12px;cursor:pointer;
    font-size:14px;font-weight:600;color:var(--ink-2);transition:.16s;margin-bottom:3px;
  }
  .nav-item .ic{width:20px;text-align:center;font-size:16px}
  .nav-item:hover{background:var(--surface-3);color:var(--ink)}
  .nav-item.active{background:linear-gradient(135deg,#fff6d6,#fff);color:var(--ink);box-shadow:0 2px 10px rgba(245,180,0,.18);border:1px solid #ffe9a8}
  .nav-item.active .ic{filter:saturate(1.2)}
  .side-card{
    margin-top:18px;padding:14px;border-radius:14px;background:linear-gradient(150deg,#16243d,#23365c);color:#fff;
  }
  .side-card h4{font-size:13px;margin-bottom:6px;display:flex;align-items:center;gap:6px}
  .side-card p{font-size:11.5px;color:#b9c6e0;line-height:1.55}

  /* ============ MAIN ============ */
  .main{margin-left:230px;margin-top:64px;padding:26px 30px 60px;width:calc(100% - 230px)}
  .page-head{margin-bottom:22px}
  .page-head h2{font-family:var(--font-d);font-size:24px;font-weight:800;letter-spacing:.2px}
  .page-head p{color:var(--ink-2);font-size:13.5px;margin-top:4px}
  .crumb{font-size:12px;color:var(--ink-3);font-weight:600;margin-bottom:6px;letter-spacing:.4px}

  /* grid + cards */
  .grid{display:grid;gap:18px}
  .card{background:var(--surface);border:1px solid var(--line);border-radius:var(--r);box-shadow:var(--shadow);padding:20px}
  .card.pad-lg{padding:24px}
  .card-h{display:flex;align-items:center;justify-content:space-between;margin-bottom:16px}
  .card-h h3{font-family:var(--font-d);font-size:15.5px;font-weight:700;display:flex;align-items:center;gap:8px}
  .card-h .sub{font-size:12px;color:var(--ink-3)}
  .tag{font-size:11px;font-weight:700;padding:3px 9px;border-radius:20px;letter-spacing:.3px}
  .tag.lemon{background:var(--lemon-soft);color:#9a6b00}
  .tag.teal{background:#dcfaf4;color:#0a7d70}
  .tag.blue{background:#e4ecfe;color:#274dba}
  .tag.demo{background:#16243d;color:#ffd23d}

  /* KPI cards */
  .kpis{display:grid;grid-template-columns:repeat(4,1fr);gap:16px}
  .kpi{background:var(--surface);border:1px solid var(--line);border-radius:var(--r);padding:18px 20px;box-shadow:var(--shadow);position:relative;overflow:hidden}
  .kpi .ribbon{position:absolute;top:0;right:0;width:70px;height:70px;border-radius:0 0 0 70px;opacity:.1}
  .kpi label{font-size:12.5px;color:var(--ink-2);font-weight:600;display:flex;align-items:center;gap:6px}
  .kpi .v{font-family:var(--font-d);font-size:30px;font-weight:800;margin-top:6px;line-height:1}
  .kpi .v small{font-size:14px;color:var(--ink-3);font-weight:600}
  .kpi .trend{font-size:12px;margin-top:7px;font-weight:600}
  .trend.up{color:var(--strong)} .trend.flat{color:var(--ink-3)}

  /* gauge */
  .gauge-wrap{position:relative;width:230px;height:140px;margin:6px auto 2px}
  .gauge-center{position:absolute;left:0;right:0;bottom:6px;text-align:center}
  .gauge-center .score{font-family:var(--font-d);font-size:42px;font-weight:800;line-height:1}
  .gauge-center .score small{font-size:15px;color:var(--ink-3);font-weight:600}
  .gauge-center .lvl{font-size:13px;font-weight:700;margin-top:2px}
  .signal-scale{display:flex;gap:6px;margin-top:14px}
  .signal-scale .seg{flex:1;text-align:center;font-size:10.5px;font-weight:700;color:#fff;padding:5px 2px;border-radius:7px;opacity:.45;transition:.2s}
  .signal-scale .seg.on{opacity:1;transform:translateY(-2px);box-shadow:0 4px 12px rgba(20,40,80,.18)}

  /* dimension cards */
  .dim3{display:grid;grid-template-columns:repeat(3,1fr);gap:14px}
  .dim{border-radius:14px;padding:16px;border:1px solid var(--line);position:relative}
  .dim.k{background:linear-gradient(160deg,#eef3ff,#fff)}
  .dim.a{background:linear-gradient(160deg,#e9faf6,#fff)}
  .dim.l{background:linear-gradient(160deg,#fff7e0,#fff)}
  .dim .dname{font-size:12.5px;font-weight:700;color:var(--ink-2);display:flex;justify-content:space-between}
  .dim .dname .w{font-size:10.5px;background:#fff;border:1px solid var(--line);border-radius:20px;padding:1px 7px;color:var(--ink-3)}
  .dim .dval{font-family:var(--font-d);font-size:28px;font-weight:800;margin:6px 0 8px}
  .dbar{height:7px;border-radius:6px;background:#eef2f8;overflow:hidden}
  .dbar i{display:block;height:100%;border-radius:6px}
  .dim.k .dbar i{background:var(--blue)} .dim.a .dbar i{background:var(--teal)} .dim.l .dbar i{background:var(--lemon-deep)}

  .chart-box{position:relative;height:300px}
  .chart-box.sm{height:250px}
  .chart-box.lg{height:340px}

  /* weakness / resource */
  .weak-item{border:1px solid var(--line);border-radius:14px;padding:14px 16px;margin-bottom:12px;background:var(--surface-2)}
  .weak-item .top{display:flex;align-items:center;justify-content:space-between;gap:10px}
  .weak-item .name{font-weight:700;font-size:14px;display:flex;align-items:center;gap:8px}
  .sev{font-size:10.5px;font-weight:700;padding:2px 9px;border-radius:20px}
  .sev.high{background:#fde6e9;color:#c2384a} .sev.mid{background:#fdf0dc;color:#a96a10}
  .weak-item .score-pill{font-family:var(--font-d);font-weight:800;font-size:15px}
  .res-list{margin-top:10px;display:flex;flex-direction:column;gap:7px}
  .res{display:flex;align-items:center;gap:9px;font-size:13px;color:var(--ink-2);background:#fff;border:1px solid var(--line);border-radius:10px;padding:9px 12px}
  .res .pic{width:22px;height:22px;border-radius:7px;display:grid;place-items:center;font-size:12px;background:var(--lemon-soft)}
  .res .go{margin-left:auto;font-size:11px;font-weight:700;color:var(--blue);background:#eef3ff;padding:3px 9px;border-radius:8px}
  .no-weak{text-align:center;padding:30px 10px;color:var(--ink-3)}
  .no-weak .big{font-size:40px}

  /* data source list */
  .ds-row{display:flex;align-items:center;gap:12px;padding:10px 0;border-bottom:1px dashed var(--line)}
  .ds-row:last-child{border-bottom:0}
  .ds-row .nm{width:118px;font-size:13px;font-weight:600;color:var(--ink-2);flex-shrink:0}
  .ds-row .track{flex:1;height:8px;background:#eef2f8;border-radius:6px;overflow:hidden}
  .ds-row .track i{display:block;height:100%;border-radius:6px;background:linear-gradient(90deg,#16b6a4,#3d6ef0)}
  .ds-row .vv{width:38px;text-align:right;font-family:var(--font-d);font-weight:700;font-size:13px}

  /* table */
  .tbl-wrap{overflow-x:auto}
  table{width:100%;border-collapse:collapse;font-size:13.5px}
  thead th{text-align:left;font-size:11.5px;letter-spacing:.5px;color:var(--ink-3);font-weight:700;text-transform:uppercase;padding:10px 12px;border-bottom:2px solid var(--line)}
  tbody td{padding:12px;border-bottom:1px solid var(--line)}
  tbody tr{transition:.13s}
  tbody tr:hover{background:var(--surface-2)}
  .row-name{display:flex;align-items:center;gap:10px;font-weight:700}
  .mini-av{width:30px;height:30px;border-radius:50%;display:grid;place-items:center;color:#fff;font-size:12px;font-weight:700;flex-shrink:0}
  .lv-pill{font-size:11.5px;font-weight:700;padding:3px 10px;border-radius:20px;display:inline-flex;align-items:center;gap:5px}
  .lv-pill .d{width:7px;height:7px;border-radius:50%}
  .sc-num{font-family:var(--font-d);font-weight:700}
  .wk-tags{display:flex;gap:5px;flex-wrap:wrap}
  .wk-tag{font-size:11px;background:#fde9ec;color:#bf3548;padding:2px 8px;border-radius:7px;font-weight:600}
  .wk-ok{font-size:11px;color:var(--strong);font-weight:700}
  .btn-view{font-size:12px;font-weight:700;color:var(--blue);background:#eef3ff;border:0;padding:6px 12px;border-radius:9px;cursor:pointer}
  .btn-view:hover{background:#e0eaff}

  /* segmented controls / chips */
  .chips{display:flex;gap:8px;flex-wrap:wrap}
  .chip{font-size:12.5px;font-weight:600;padding:7px 13px;border-radius:20px;border:1px solid var(--line-2);background:var(--surface);cursor:pointer;color:var(--ink-2)}
  .chip.active{background:var(--ink);color:#fff;border-color:var(--ink)}

  /* standard library */
  .std-dim{border:1px solid var(--line);border-radius:16px;overflow:hidden;margin-bottom:16px}
  .std-dim .hd{padding:14px 18px;display:flex;align-items:center;justify-content:space-between;color:#fff}
  .std-dim.k .hd{background:linear-gradient(120deg,#3d6ef0,#5b86f5)}
  .std-dim.a .hd{background:linear-gradient(120deg,#0fb3a1,#3fd0bd)}
  .std-dim.l .hd{background:linear-gradient(120deg,#f5b400,#ffce4d)}
  .std-dim .hd h3{font-family:var(--font-d);font-size:16px}
  .std-dim .hd .ww{font-size:13px;font-weight:700;background:rgba(255,255,255,.22);padding:4px 12px;border-radius:20px}
  .std-row{display:grid;grid-template-columns:200px 1fr 200px;gap:14px;padding:13px 18px;border-bottom:1px solid var(--line);font-size:13px;align-items:center}
  .std-row:last-child{border-bottom:0}
  .std-row .it{font-weight:700;color:var(--ink)}
  .std-row .desc{color:var(--ink-2)}
  .std-row .strong-sig{font-size:12px;color:#0a7d70;font-weight:600;background:#dcfaf4;padding:5px 10px;border-radius:9px;display:inline-block}

  /* mentor scoring */
  .score-input{display:flex;align-items:center;gap:14px;margin:14px 0}
  .score-input label{width:120px;font-size:13.5px;font-weight:600}
  .score-input input[type=range]{flex:1;accent-color:var(--lemon-deep)}
  .score-input .num{width:56px;text-align:center;font-family:var(--font-d);font-weight:800;font-size:18px;color:var(--ink)}
  .ta{width:100%;border:1px solid var(--line-2);border-radius:12px;padding:12px;font-family:var(--font-b);font-size:13.5px;resize:vertical;min-height:84px}
  .btn{font-family:var(--font-b);font-weight:700;font-size:14px;border:0;border-radius:12px;padding:12px 22px;cursor:pointer;transition:.15s}
  .btn.primary{background:linear-gradient(135deg,#ffd23d,#f5b400);color:#5a3d00;box-shadow:0 6px 16px rgba(245,180,0,.32)}
  .btn.primary:hover{filter:brightness(1.04);transform:translateY(-1px)}
  .btn.ghost{background:var(--surface-3);color:var(--ink)}
  .grp-card{border:1px solid var(--line);border-radius:14px;padding:16px;cursor:pointer;transition:.15s}
  .grp-card:hover{box-shadow:var(--shadow);transform:translateY(-2px)}
  .grp-card.sel{border-color:var(--lemon-deep);background:var(--lemon-soft)}
  .grp-card .gt{font-weight:700;font-size:14px;display:flex;align-items:center;justify-content:space-between}
  .grp-card .gm{font-size:12px;color:var(--ink-3);margin-top:4px}
  .status-pill{font-size:11px;font-weight:700;padding:2px 9px;border-radius:20px}
  .status-pill.done{background:#dcfaf4;color:#0a7d70}
  .status-pill.pend{background:#fdf0dc;color:#a96a10}
  .ar-badge{font-family:var(--font-d);font-weight:800;font-size:13px;padding:4px 12px;border-radius:9px}
  .ar-A{background:#dcfaf4;color:#0a7d70} .ar-R{background:#fde6e9;color:#c2384a}

  /* AI chat */
  .chat{display:flex;flex-direction:column;height:480px}
  .chat-log{flex:1;overflow-y:auto;padding:6px 2px;display:flex;flex-direction:column;gap:12px}
  .msg{max-width:82%;padding:12px 15px;border-radius:16px;font-size:13.5px;line-height:1.6}
  .msg.bot{background:var(--surface-2);border:1px solid var(--line);border-bottom-left-radius:5px;align-self:flex-start}
  .msg.user{background:linear-gradient(135deg,#3d6ef0,#5b86f5);color:#fff;border-bottom-right-radius:5px;align-self:flex-end}
  .msg.bot b{color:#9a6b00}
  .chat-suggest{display:flex;gap:8px;flex-wrap:wrap;margin:12px 0}
  .sugg{font-size:12px;background:var(--lemon-soft);border:1px solid #ffe9a8;color:#8a5e00;padding:6px 11px;border-radius:18px;cursor:pointer;font-weight:600}
  .sugg:hover{background:#ffefb8}
  .chat-in{display:flex;gap:10px;margin-top:6px}
  .chat-in input{flex:1;border:1px solid var(--line-2);border-radius:12px;padding:12px 14px;font-family:var(--font-b);font-size:14px}
  .chat-in button{background:var(--ink);color:#fff;border:0;border-radius:12px;padding:0 20px;font-weight:700;cursor:pointer}

  /* modal */
  .modal-mask{position:fixed;inset:0;background:rgba(15,25,45,.5);backdrop-filter:blur(3px);z-index:60;display:none;align-items:flex-start;justify-content:center;padding:40px 20px;overflow-y:auto}
  .modal-mask.show{display:flex}
  .modal{background:var(--bg);border-radius:22px;max-width:1000px;width:100%;box-shadow:var(--shadow-lg);padding:26px;position:relative}
  .modal .x{position:absolute;top:18px;right:20px;width:34px;height:34px;border-radius:10px;border:0;background:var(--surface-3);font-size:18px;cursor:pointer;color:var(--ink-2)}

  .helper{font-size:12px;color:var(--ink-3);line-height:1.6}
  .double-helix{display:flex;gap:12px;margin-top:6px}
  .helix{flex:1;border-radius:12px;padding:13px 15px;font-size:12.5px;line-height:1.55}
  .helix.build{background:#e9faf6;border:1px solid #b9ece2}
  .helix.pure{background:#fff7e0;border:1px solid #ffe6a0}
  .helix b{font-size:13px}

  .legend-flat{display:flex;gap:16px;flex-wrap:wrap;font-size:12px;color:var(--ink-2);margin-top:8px}
  .legend-flat span{display:flex;align-items:center;gap:6px}
  .legend-flat i{width:11px;height:11px;border-radius:3px;display:inline-block}

  /* ===== 知识图谱·点亮 ===== */
  .kmap-prog{display:flex;align-items:center;gap:22px;flex-wrap:wrap}
  .ring-wrap{position:relative;width:150px;height:90px;flex-shrink:0}
  .ring-center{position:absolute;left:0;right:0;bottom:2px;text-align:center}
  .ring-center .pct{font-family:var(--font-d);font-size:30px;font-weight:800;line-height:1}
  .ring-center .lbl{font-size:11px;color:var(--ink-3);font-weight:600}
  .cat-stat{display:flex;gap:14px;flex-wrap:wrap}
  .cat-box{border:1px solid var(--line);border-radius:12px;padding:10px 16px;min-width:120px}
  .cat-box .ct{font-size:12px;font-weight:700;display:flex;align-items:center;gap:6px;color:var(--ink-2)}
  .cat-box .cv{font-family:var(--font-d);font-size:20px;font-weight:800;margin-top:3px}
  .cat-box .cv small{font-size:12px;color:var(--ink-3);font-weight:600}
  .kmap-task{border:1px solid var(--line);border-radius:16px;padding:16px 18px;margin-bottom:14px;background:var(--surface)}
  .kmap-task .th{display:flex;align-items:center;justify-content:space-between;margin-bottom:13px}
  .kmap-task .th b{font-family:var(--font-d);font-size:14.5px}
  .kmap-task .th .tp{font-size:12px;font-weight:700;color:var(--lemon-deep)}
  .kmap-task.demo{border-color:#ffe08a;box-shadow:0 4px 16px rgba(245,180,0,.10)}
  .bulb-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(195px,1fr));gap:11px}
  .bulb{border:1px solid var(--line);border-radius:13px;padding:12px 13px;cursor:pointer;background:var(--surface-2);transition:.16s;display:flex;gap:10px;align-items:flex-start;position:relative}
  .bulb:hover{transform:translateY(-2px);box-shadow:var(--shadow)}
  .bulb .ic{font-size:21px;line-height:1.1;filter:grayscale(1);opacity:.38;transition:.2s;flex-shrink:0}
  .bulb.on{background:linear-gradient(160deg,#fff8df,#fff);border-color:#ffdf86}
  .bulb.on .ic{filter:none;opacity:1;text-shadow:0 0 13px rgba(245,180,0,.85),0 0 4px rgba(245,180,0,.6)}
  .bulb.sel{outline:2px solid var(--blue);outline-offset:1px}
  .bulb .nm{font-size:12.5px;font-weight:700;line-height:1.32}
  .bulb .mt{font-size:10.5px;color:var(--ink-3);margin-top:4px;display:flex;align-items:center;gap:6px}
  .cat-dot{width:8px;height:8px;border-radius:50%;display:inline-block;flex-shrink:0}
  .tip-panel{background:linear-gradient(160deg,#16243d,#22335a);color:#fff;border-radius:18px;padding:20px;position:sticky;top:84px}
  .tip-panel .tt{font-family:var(--font-d);font-size:15px;font-weight:700;display:flex;align-items:center;gap:8px;margin-bottom:4px}
  .tip-panel .tcat{font-size:11.5px;color:#9fb0d0;margin-bottom:14px}
  .tip-body{background:rgba(255,255,255,.08);border:1px solid rgba(255,255,255,.12);border-radius:13px;padding:14px;font-size:13px;line-height:1.7}
  .tip-body b{color:#ffd23d}
  .tip-empty{color:#9fb0d0;font-size:13px;line-height:1.7;text-align:center;padding:24px 6px}
  .tip-empty .big{font-size:40px;display:block;margin-bottom:8px}
  .lit-bar{height:7px;border-radius:6px;background:#eef2f8;overflow:hidden;flex:1}
  .lit-bar i{display:block;height:100%;border-radius:6px;background:linear-gradient(90deg,#f5b400,#ffd23d)}
  .kpt-row{display:flex;align-items:center;gap:10px;padding:8px 0;border-bottom:1px dashed var(--line);font-size:12.5px}
  .kpt-row:last-child{border-bottom:0}
  .kpt-row .knm{width:170px;flex-shrink:0;font-weight:600;color:var(--ink-2);display:flex;align-items:center;gap:6px}
  .kpt-row .kv{width:42px;text-align:right;font-family:var(--font-d);font-weight:700;font-size:12.5px}

  @media(max-width:1100px){
    .kpis{grid-template-columns:repeat(2,1fr)}
    .dim3{grid-template-columns:1fr}
    .col-2{grid-template-columns:1fr!important}
  }
  @media(max-width:820px){
    .sidebar{display:none}.main{margin-left:0;width:100%;padding:18px}
    .brand small{display:none}.role-switch button{padding:8px 9px;font-size:12px}
    .std-row{grid-template-columns:1fr}
  }
</style>
</head>
<body>
<div class="app">
  <!-- TOP BAR -->
  <header class="topbar">
    <div class="brand">
      <div class="logo">🍋</div>
      <div>
        <h1>柠檬AI助教</h1>
        <small>《财务大数据分析》· 数智化成长诊断平台</small>
      </div>
    </div>
    <div class="topbar-spacer"></div>
    <div class="role-switch" id="roleSwitch"></div>
    <div class="who" id="whoBox"></div>
  </header>

  <!-- SIDEBAR -->
  <aside class="sidebar" id="sidebar"></aside>

  <!-- MAIN -->
  <main class="main" id="main"></main>
</div>

<!-- MODAL -->
<div class="modal-mask" id="modalMask">
  <div class="modal" id="modalBody"></div>
</div>

<script>
/* ============================================================
   柠檬AI助教 — 数据层（模拟数据）
   理论依据：柠檬市场信号理论
   综合信号分 = 知识×30% + 能力×45% + 素养×25%
   ============================================================ */
const NAMES = ["郑曾非","董思艳","范婷婷","洪小草","黄玲","孔娅婷","赖凤艳","李晗","李敏","李飘","李雯婷","刘艳梅","罗志仙","马丹妮","普蓉","秦嘉蔚","谭秋云","王凤仙","王艳琼","魏欢欢","夏凤艳","徐艾琪","徐清婷","杨春莎","杨艳","杨艺霞","杨云琴","杨祖炼","业鹏圆","叶青青","叶英","尤文兰","张清凤","张蓉","张思南","张永娇"];

const CLASS_NAME = "2024级大数据与审计1班";
const TEACHERS = [
  {name:"胡老师",tasks:[1,2]},
  {name:"凌老师",tasks:[3,4]},
  {name:"符老师",tasks:[5,6]},
  {name:"解老师",tasks:[7,8]},
];

const GROUPS = ["采购风控组","销售回款组","资金链分析组","数据治理组","风险建模组","咨询交付组"];

const TASKS = [
  {id:1,name:"任务一 财务大数据认知与柠檬市场破局",short:"认知破局"},
  {id:2,name:"任务二 多维数据获取与采集",short:"数据采集",demo:true},
  {id:3,name:"任务三 数据清洗与预处理",short:"数据清洗",demo:true},
  {id:4,name:"任务四 风险导向分析建模(一)采购分析",short:"采购分析"},
  {id:5,name:"任务五 风险导向分析建模(二)销售分析",short:"销售分析",demo:true},
  {id:6,name:"任务六 财报与财务指标分析",short:"财报分析"},
  {id:7,name:"任务七 资金分析与综合诊断",short:"资金诊断"},
  {id:8,name:"任务八 咨询报告交付与路演",short:"报告路演",demo:true},
];

// 教案任务模块 → 核心技能 / 聚焦能力项 / 交付物 / 素养主线
const TASK_FOCUS = {
  1:{skills:"柠檬市场认知 · 信息不对称 · AI企业背景检索 · 风险全景图",metrics:["bizFlow","skepticism","compliance"],deliver:"《企业背景调研速览表》《风险全景图》",lit:"职业使命感 · 数据合规意识"},
  2:{skills:"RPA(UiPath) Web抓取 · Excel写入 · 网银流水下载 · 多源整合",metrics:["rpa","compliance","internalControl"],deliver:"《报表收集机器人.xaml》《银行流水机器人.xaml》",lit:"数据安全(SecureString) · 工匠精神"},
  3:{skills:"Power Query 清洗五步法 · 缺失/重复/异常处理 · 多表合并查询",metrics:["powerQuery","internalControl","modelTheory"],deliver:"《银行流水清洗表》《数据质量评估报告》",lit:"高质量输入决定可靠输出 · 严谨规范"},
  4:{skills:"星型模型 · DAX(SUM/CALCULATE/FILTER) · 供应商风险画像",metrics:["modelTheory","dax","purchaseRisk"],deliver:"《供应商风险画像仪表板》《风险识别报告》",lit:"职业怀疑 · 复杂问题拆解"},
  5:{skills:"RFM 客户价值分群 · DAX建模 · 客户集中度风险 · 可视化看板",metrics:["dax","salesRisk","modelTheory"],deliver:"《客户价值矩阵图》《咨询方案》",lit:"识别虚假繁荣 · 数据隐私伦理"},
  6:{skills:"财务报表分析 · 财务指标体系 · 杜邦分析拆解",metrics:["finReport","dupont","bizFlow"],deliver:"《财务指标分析报告》",lit:"用数据说话 · 客观公正"},
  7:{skills:"资金链分析 · 现金流/回款诊断 · 增收不增利综合诊断",metrics:["cashRisk","finReport","skepticism"],deliver:"《资金分析与综合诊断报告》",lit:"风险敏感 · 责任担当"},
  8:{skills:"咨询报告交付 · 90秒路演 · 答辩质询 · 交叉审计",metrics:["report","skepticism","teamwork"],deliver:"《数智咨询报告》《路演PPT》",lit:"诚信执业 · 团队协作 · 质量至上"},
};
// 任务达成分：聚焦能力项 + 该任务节点的成长信号融合
function taskScore(s,taskId){
  const f=TASK_FOCUS[taskId];
  const fm=avg(f.metrics.map(k=>s.met[k]!==undefined?s.met[k]:s[k]));
  const node=s.curve[taskId-1];
  return clamp(fm*0.65+node*0.35);
}

// 信号等级
function levelOf(s){
  if(s>=90) return {key:"elite",name:"准岗位员工",color:"var(--elite)",hex:"#3d6ef0",desc:"可按企业标准交付成果，能说明依据、接受质询、提出改进"};
  if(s>=75) return {key:"strong",name:"强信号学员",color:"var(--strong)",hex:"#16b884",desc:"能独立完成数据处理、风险识别、报告撰写与答辩说明"};
  if(s>=60) return {key:"growth",name:"成长信号学员",color:"var(--growth)",hex:"#f4a52c",desc:"能完成基础任务，复杂问题仍需教师提示"};
  return {key:"weak",name:"弱信号学员",color:"var(--weak)",hex:"#ef5a6a",desc:"会听课但独立操作弱，能套模板但不能解释风险"};
}
const LEVELS=[{k:"weak",n:"弱信号",hex:"#ef5a6a",r:"0-59"},{k:"growth",n:"成长信号",hex:"#f4a52c",r:"60-74"},{k:"strong",n:"强信号",hex:"#16b884",r:"75-89"},{k:"elite",n:"准岗位",hex:"#3d6ef0",r:"90-100"}];

// 短板诊断字典： key -> {label, sev metric, resources}
const WEAK_DEFS = [
  {key:"powerQuery",label:"Power Query 数据清洗能力弱",icon:"🧹",res:["《Power Query 缺失值与重复值处理》微课","ST公司银行流水规范化清洗专项","多表合并查询「拼图法」实训"]},
  {key:"dax",label:"DAX 指标构建能力弱",icon:"🧮",res:["《DAX 函数五十例》精讲","CALCULATE 上下文转换「筛子」专项","应付账款周转天数建模任务"]},
  {key:"purchaseRisk",label:"采购风险识别弱",icon:"📦",res:["供应商集中度与关联交易识别案例","采购单价异常下钻分析任务","《供应商采购异常风险指标定义》研读"]},
  {key:"salesRisk",label:"销售风险识别弱",icon:"📈",res:["RFM 客户价值分群实战","渠道压货虚增/窜货识别案例","「虚假繁荣」销量陷阱找茬训练"]},
  {key:"cashRisk",label:"资金分析弱",icon:"💰",res:["现金流与回款周期分析任务","「增收不增利」资金链诊断案例","资金综合诊断专项练习"]},
  {key:"skepticism",label:"职业怀疑不足",icon:"🔍",res:["「AI 结论找错」异常凭证识别任务","交叉审计与风险听证演练","异常数据找茬训练包"]},
  {key:"compliance",label:"数据合规意识不足",icon:"🛡️",res:["《数据安全法》红线案例研读","SecureString 网银密码安全存储实训","数据隐私与权限控制案例"]},
  {key:"report",label:"咨询报告与路演能力弱",icon:"📝",res:["咨询报告结构模板与修改清单","90秒路演话术训练","答辩质询模拟演练"]},
  {key:"rpa",label:"RPA 自动化采集能力弱",icon:"🤖",res:["Web 抓取 + Excel 写入跟做微课","银行流水机器人搭建任务","Selector 选择器修复专项"]},
];

// 岗位标准库
const STD_LIB = {
  k:{name:"知识维度",weight:30,strong:"能说清业务逻辑",items:[
    {it:"财务报表与财务指标分析",desc:"资产负债/利润/现金流量表解读，盈利、营运、偿债能力指标"},
    {it:"杜邦分析体系",desc:"ROE 拆解，盈利能力—营运效率—财务杠杆的联动分析"},
    {it:"采购/销售/资金业务认知",desc:"采购循环、销售渠道、资金链等业财融合逻辑"},
    {it:"数字化内控流程",desc:"内部控制关键节点与风险点，受托责任与合规边界"},
    {it:"数据模型理论（星型/RFM）",desc:"事实表与维度表关系、RFM 客户价值分层逻辑"},
  ]},
  a:{name:"能力维度",weight:45,strong:"能完成真实任务",items:[
    {it:"RPA 数据采集",desc:"UiPath Web 抓取、Excel 写入、网银流水下载与多源整合"},
    {it:"Power Query 数据清洗",desc:"缺失/重复/异常处理、字段规范、多表合并查询"},
    {it:"DAX 指标建模",desc:"SUM/CALCULATE/FILTER/ALL，构建风险度量值"},
    {it:"风险识别分析",desc:"采购、销售、资金三类风险信号识别与下钻验证"},
    {it:"咨询报告撰写与路演",desc:"咨询报告交付、可视化看板、90秒路演与答辩"},
  ]},
  l:{name:"素养维度",weight:25,strong:"能守底线、敢质疑",items:[
    {it:"数据合规意识",desc:"《数据安全法》红线、隐私保护、密码安全存储"},
    {it:"职业怀疑精神",desc:"不盲从报表与 AI 结论，对异常保持探究与追问"},
    {it:"诚信执业",desc:"数据真实、分析有据、结论可追溯"},
    {it:"团队协作",desc:"分工明确、沟通顺畅、交叉互审"},
    {it:"责任担当",desc:"质量至上、严谨细致、服务实体经济"},
  ]},
};

// ===== 知识图谱：教材知识点 × 审计助理岗位能力点（点亮式）=====
// cat: k=知识储备点  a=能力技术点  l=素养目标点；metric 关联学生能力数据；tip=AI助教点亮秘诀
const KSTD = 75; // 达标点亮线（达到岗位标准即点亮）
const CAT_META = {k:{n:"知识储备点",c:"#3d6ef0"},a:{n:"能力技术点",c:"#16b6a4"},l:{n:"素养目标点",c:"#f5b400"}};
const KMAP = [
 {task:1,points:[
   {id:"t1p1",cat:"k",metric:"bizFlow",name:"柠檬市场与信息不对称",tip:"用「二手车市场」类比：能力强但没证据也会被低估。记牢三个词——柠檬市场、信息不对称、强信号。"},
   {id:"t1p2",cat:"k",metric:"finReport",name:"财务大数据5V特征(价值密度低)",tip:"抓住「价值密度低」这一点：海量数据要先去噪提炼，才有审计价值。能区分ERP内部数据与外部舆情数据即可点亮。"},
   {id:"t1p3",cat:"a",metric:"rpa",name:"AI企业背景检索与提示词优化",tip:"摆脱「百度式」搜索：优化提示词获取结构化信息，并交叉核验至少2条公开来源，不照搬AI答案。"},
   {id:"t1p4",cat:"l",metric:"compliance",name:"数据合规与隐私保护意识",tip:"采集前先想合规红线：不爬取个人隐私，标注《数据安全法》边界，这是审计的底线信号。"},
   {id:"t1p5",cat:"l",metric:"responsibility",name:"准员工角色与职业自信",tip:"把自己当「数智审计助理」：项目成果就是能被企业看见的能力证据，主动承担、严谨求真。"},
 ]},
 {task:2,points:[
   {id:"t2p1",cat:"k",metric:"rpa",name:"Web活动与Excel活动配合",tip:"理解数据流：网页→UiPath抓取→Excel存储。掌握「提取结构化数据」与「写入范围」的配合参数即可点亮。"},
   {id:"t2p2",cat:"a",metric:"rpa",name:"RPA报表收集机器人开发",tip:"按「启动→抓取→写入→关闭」搭框架，DataTable变量是连接Web与Excel的桥梁。先跑通最小流程再优化。"},
   {id:"t2p3",cat:"a",metric:"rpa",name:"银行流水机器人·安全登录",tip:"六步闭环：变量准备→安全登录→日期选择→下载→归档。用「存在元素」判断登录成功。"},
   {id:"t2p4",cat:"l",metric:"compliance",name:"数据安全法·密码不明文存储",tip:"点亮关键：网银密码绝不在Excel或代码里明文写，必须用 SecureString / 获取安全密码。"},
   {id:"t2p5",cat:"a",metric:"rpa",name:"多源数据整合与规范归档",tip:"用「移动文件」活动+字符串拼接做规范命名归档，养成一丝不苟的档案管理习惯。"},
 ]},
 {task:3,points:[
   {id:"t3p1",cat:"k",metric:"powerQuery",name:"数据清洗五步法",tip:"记牢链接→探查→规则→清洗→加载。先建立「脏数据会让结论不可信」的认知，再动手。"},
   {id:"t3p2",cat:"a",metric:"powerQuery",name:"Power Query 核心操作",tip:"练熟替换值、拆分列、填充、删除重复项。先做后讲，在试错中找问题，再对照规范修正。"},
   {id:"t3p3",cat:"a",metric:"modelTheory",name:"多表合并查询(事实表+维度表)",tip:"用「拼图」类比合并查询：关联键要唯一无歧义，否则数据膨胀。合并后核对行数是否符合预期。"},
   {id:"t3p4",cat:"l",metric:"responsibility",name:"高质量输入决定可靠输出·工匠精神",tip:"清洗后量化对比(如缺失率从X%降到Y%),写进《数据质量评估报告》——用数据证明清洗价值。"},
 ]},
 {task:4,points:[
   {id:"t4p1",cat:"k",metric:"modelTheory",name:"星型模型(事实表/维度表)",tip:"在Power BI模型视图里拖拽建立关系：事实表放度量，维度表放分类。能画出星型草图即可点亮。"},
   {id:"t4p2",cat:"a",metric:"dax",name:"DAX基础度量值(SUM/CALCULATE)",tip:"把CALCULATE想成能改筛选条件的「高级筛子」。先掌握SUM，再练CALCULATE，突破上下文转换。"},
   {id:"t4p3",cat:"a",metric:"purchaseRisk",name:"供应商集中度与价格异常识别",tip:"用CALCULATE+ALL算占比，识别过度依赖单一供应商、采购单价异常、关联交易，并下钻到明细核验。"},
   {id:"t4p4",cat:"l",metric:"skepticism",name:"职业怀疑·用数据说话",tip:"别被汇总数字迷惑：对至少1个异常点做根因分析并记录过程，这就是审计的职业怀疑。"},
 ]},
 {task:5,points:[
   {id:"t5p1",cat:"k",metric:"modelTheory",name:"RFM模型R/F/M业务内涵",tip:"R=最近消费、F=频率、M=金额。把业务现象映射成维度：很久没来=R大得分低。能辨析「重要挽留客户」即点亮。"},
   {id:"t5p2",cat:"a",metric:"dax",name:"DAX计算RFM分值与客户分群",tip:"用DAX新建度量值算R/F/M，注意R值计算方向。可用AI生成草稿，但要对照教材三查核验。"},
   {id:"t5p3",cat:"a",metric:"salesRisk",name:"识别客户集中度与虚假繁荣",tip:"警惕「销量涨20%但现金流紧张」：老客户流失、低质量客户占比高都是虚假繁荣信号，下钻验证。"},
   {id:"t5p4",cat:"l",metric:"compliance",name:"数据隐私伦理(避免大数据杀熟)",tip:"方案里加「隐私保护声明/客户退出机制」，平衡商业利益与客户体验——合规也是强信号。"},
 ]},
 {task:6,points:[
   {id:"t6p1",cat:"k",metric:"finReport",name:"三大报表解读",tip:"打通资产负债表、利润表、现金流量表的勾稽关系。能从报表读出经营状况即可点亮。"},
   {id:"t6p2",cat:"k",metric:"dupont",name:"杜邦分析(ROE拆解)",tip:"把ROE拆成盈利能力×营运效率×财务杠杆，找出是哪一环拖累了回报，这是分析的「望远镜」。"},
   {id:"t6p3",cat:"a",metric:"finReport",name:"财务指标可视化分析",tip:"用趋势图+对比图呈现盈利、营运、偿债指标，让数字会「说话」，支撑后续风险判断。"},
   {id:"t6p4",cat:"l",metric:"integrity",name:"客观公正·诚信执业",tip:"结论只跟数据走、可追溯到明细，不迎合、不臆断——诚信执业是审计的立身之本。"},
 ]},
 {task:7,points:[
   {id:"t7p1",cat:"k",metric:"cashRisk",name:"资金链与现金流逻辑",tip:"把销售、采购、资金三端联动起来看。理解「卖得越多越缺钱」背后的资金链断点即可点亮。"},
   {id:"t7p2",cat:"a",metric:"cashRisk",name:"回款周期与增收不增利诊断",tip:"算回款周期、看存货与应付账款变动，定位「增收不增利」的真实原因，给出可执行建议。"},
   {id:"t7p3",cat:"a",metric:"dax",name:"综合风险诊断建模",tip:"把采购、销售、资金多维风险整合成综合评分卡，用动态阈值标记高风险，形成诊断闭环。"},
   {id:"t7p4",cat:"l",metric:"responsibility",name:"风险敏感·责任担当",tip:"主动上报数据造假线索、对异常零容忍。质量至上、勤勉尽责,是准岗位员工的态度信号。"},
 ]},
 {task:8,points:[
   {id:"t8p1",cat:"a",metric:"report",name:"咨询报告撰写",tip:"按「问题—数据—分析—结论—建议」结构写，每个结论都能下钻到明细数据验证。"},
   {id:"t8p2",cat:"a",metric:"report",name:"90秒路演与可视化看板",tip:"90秒讲清「问题—数据—成果—强信号」。看板突出业务价值与合规性，让评委一眼看见你的能力。"},
   {id:"t8p3",cat:"a",metric:"skepticism",name:"答辩质询与交叉审计",tip:"能接受质询、说明依据、提出改进，并在交叉审计中用己方模型验证他组结论——从分析师走向咨询顾问。"},
   {id:"t8p4",cat:"l",metric:"teamwork",name:"团队协作·质量至上",tip:"分工明确、主动补位、交付物格式规范注释完整。A级交付没有「60分万岁」,只有合格与返工。"},
 ]},
];
const KALL = KMAP.flatMap(t=>t.points);
function strHash(str){let h=2166136261;for(let i=0;i<str.length;i++){h^=str.charCodeAt(i);h=Math.imul(h,16777619)>>>0;}return h;}
function pointScore(s,p){const off=(strHash(p.id+"#"+s.id)%21)-10;return clamp(s.met[p.metric]+off);}
function isLit(s,p){return pointScore(s,p)>=KSTD;}
function litCount(s){return KALL.filter(p=>isLit(s,p)).length;}
function litByCat(s,cat){const ps=KALL.filter(p=>p.cat===cat);return {on:ps.filter(p=>isLit(s,p)).length,total:ps.length};}
function classLitRate(p){return Math.round(STUDENTS.filter(s=>isLit(s,p)).length/STUDENTS.length*100);}
function rng(seed){let s=(seed*9301+49297)%233280;return ()=>{s=(s*9301+49297)%233280;return s/233280;};}
const clamp=v=>Math.max(8,Math.min(100,Math.round(v)));
const avg=arr=>arr.reduce((a,b)=>a+b,0)/arr.length;

const TIERS=[2,3,1,0,2, 2,1,2,1,3, 2,0,1,2,2, 1,2,3,1,0, 2,1,1,3,2, 1,2,2,0, 2,1,3,2,1, 0,2];
const TIER_BASE={0:[48,58],1:[62,73],2:[76,88],3:[90,96]};

const STUDENTS = NAMES.map((name,i)=>{
  const r=rng(i*7+11);
  const [lo,hi]=TIER_BASE[TIERS[i]];
  const base=lo+r()*(hi-lo);
  const kT=clamp(base+(r()*10-4)), aT=clamp(base+(r()*10-5)), lT=clamp(base+(r()*10-4));
  const m=(t,bias=0)=>clamp(t+(r()*16-8)-bias);
  const met={
    finReport:m(kT), dupont:m(kT,4), bizFlow:m(kT,1), internalControl:m(kT,2), modelTheory:m(kT,3),
    rpa:m(aT,1), powerQuery:m(aT,3), dax:m(aT,7), report:m(aT,2),
    purchaseRisk:m(aT,2), salesRisk:m(aT,5), cashRisk:m(aT,4),
    compliance:m(lT,1), skepticism:m(lT,6), integrity:m(lT,0), teamwork:m(lT,-3), responsibility:m(lT,-1),
  };
  const riskAnalysis=clamp(avg([met.purchaseRisk,met.salesRisk,met.cashRisk]));
  const knowledge=clamp(avg([met.finReport,met.dupont,met.bizFlow,met.internalControl,met.modelTheory]));
  const ability=clamp(avg([met.rpa,met.powerQuery,met.dax,met.report,riskAnalysis]));
  const literacy=clamp(avg([met.compliance,met.skepticism,met.integrity,met.teamwork,met.responsibility]));
  const composite=Math.round(knowledge*0.3+ability*0.45+literacy*0.25);

  // 成长曲线（8任务）
  const start=clamp(composite-(18+r()*8));
  const curve=[];
  for(let t=0;t<8;t++){
    const lin=start+(composite-start)*(t/7);
    curve.push(clamp(lin+(r()*5-2.5)));
  }
  curve[7]=composite; curve[0]=Math.min(curve[0],start+3);

  // 学习数据采集（8来源）
  const ds=[
    {nm:"课前测试",v:clamp(knowledge-6+(r()*10-5))},
    {nm:"课后测试",v:clamp(knowledge+3+(r()*8-4))},
    {nm:"课堂任务",v:clamp(ability+(r()*8-4))},
    {nm:"作业",v:clamp(avg([ability,knowledge])+(r()*8-4))},
    {nm:"项目报告",v:clamp(avg([ability,met.report])+(r()*8-4))},
    {nm:"平台提问",v:clamp(50+r()*45)},
    {nm:"企业导师评价",v:clamp(composite+(r()*10-6))},
    {nm:"小组互评",v:clamp(literacy+(r()*8-4))},
  ];

  return {id:i,name,group:Math.min(5,Math.floor(i/6)),met,riskAnalysis,knowledge,ability,literacy,composite,curve,ds};
});

// 短板检测
function weaknessesOf(s){
  const out=[];
  WEAK_DEFS.forEach(d=>{
    const v=s.met[d.key];
    if(v<72) out.push({...d,score:v,sev:v<60?"high":"mid"});
  });
  out.sort((a,b)=>a.score-b.score);
  return out;
}

// 班级聚合
function classAvg(field){return Math.round(avg(STUDENTS.map(s=>s[field])));}
function levelDist(){const d={weak:0,growth:0,strong:0,elite:0};STUDENTS.forEach(s=>d[levelOf(s.composite).key]++);return d;}
function weakHeat(){const c={};WEAK_DEFS.forEach(d=>c[d.key]=0);STUDENTS.forEach(s=>weaknessesOf(s).forEach(w=>c[w.key]++));return c;}

// 企业导师 — 小组评价
const GROUP_MEMBERS = GROUPS.map((_,gi)=>STUDENTS.filter(s=>s.group===gi));
const GROUP_EVAL = GROUPS.map((g,gi)=>{
  const mem=GROUP_MEMBERS[gi];
  const gc=Math.round(avg(mem.map(s=>s.composite)));
  const done=gi<3;
  return {id:gi,name:g,gc,size:mem.length,evaluated:done,
    report: done? clamp(gc+(gi-1)*3):80,
    roadshow: done? clamp(gc-2+(gi)*2):80,
    defense: done? clamp(gc+1-(gi)*2):80,
    feedback: done? ["报告结构完整，供应商集中度风险下钻清晰，证据链可追溯，建议补充关联交易金额阈值说明。","RFM 客户分群逻辑准确，路演表达自信，对「重要挽留客户」策略落地性把握到位。","资金链「增收不增利」诊断到位，答辩中对回款周期异常的根因分析有说服力。"][gi]:""};
});

/* ============================================================
   状态 + 角色
   ============================================================ */
const ROLES=[
  {key:"student",label:"学生",icon:"🎓"},
  {key:"teacher",label:"教师",icon:"📊"},
  {key:"mentor",label:"企业导师",icon:"🏢"},
  {key:"admin",label:"管理员",icon:"⚙️"},
];
const NAV={
  student:[
    {key:"overview",ic:"🌱",t:"我的成长画像"},
    {key:"radar",ic:"🎯",t:"能力雷达诊断"},
    {key:"growth",ic:"📈",t:"全链路成长曲线"},
    {key:"weak",ic:"🔍",t:"短板诊断与提升"},
    {key:"kmap",ic:"💡",t:"知识图谱点亮"},
    {key:"data",ic:"🗂️",t:"学习数据采集"},
    {key:"ai",ic:"🍋",t:"柠檬AI助教问答"},
  ],
  teacher:[
    {key:"overview",ic:"📊",t:"班级信号概览"},
    {key:"module",ic:"🧩",t:"教学任务分析"},
    {key:"kmap",ic:"💡",t:"知识图谱"},
    {key:"roster",ic:"👥",t:"学生信号档案"},
    {key:"heat",ic:"🔥",t:"班级短板热力"},
  ],
  mentor:[
    {key:"score",ic:"📝",t:"小组成果评分"},
    {key:"records",ic:"🗃️",t:"已提交评价"},
  ],
  admin:[
    {key:"lib",ic:"📚",t:"岗位信号标准库"},
    {key:"rule",ic:"⚖️",t:"评分规则配置"},
    {key:"data",ic:"🗄️",t:"平台数据概览"},
    {key:"roles",ic:"👤",t:"角色与账号"},
  ],
};
const state={role:"student",nav:"overview",studentId:0,teacherId:0,moduleTask:0,mentorGroup:3,kmapPoint:null,kmapStu:0,
  draft:{report:80,roadshow:80,defense:80,feedback:""}};
let charts={};

function avInit(name){return name.slice(0,1);}
function avColor(i){const c=["#3d6ef0","#16b6a4","#f5b400","#6366f1","#ec5f7a","#0ea5a0","#f08a24"];return c[i%c.length];}

/* ============================================================
   渲染骨架
   ============================================================ */
function renderRoleSwitch(){
  document.getElementById("roleSwitch").innerHTML=ROLES.map(r=>
    `<button class="${state.role===r.key?"active":""}" data-role="${r.key}">${r.icon} ${r.label}</button>`).join("");
}
function renderWho(){
  const box=document.getElementById("whoBox");
  if(state.role==="student"){
    const s=STUDENTS[state.studentId];
    box.innerHTML=`
      <select class="stu-pick" id="stuPick">
        ${STUDENTS.map(x=>`<option value="${x.id}" ${x.id===state.studentId?"selected":""}>${x.name}</option>`).join("")}
      </select>
      <div class="avatar" style="background:${avColor(s.id)}">${avInit(s.name)}</div>
      <div class="meta"><b>${s.name}</b><span>${GROUPS[s.group]} · 学徒班</span></div>`;
    document.getElementById("stuPick").onchange=e=>{state.studentId=+e.target.value;state.nav="overview";render();};
  }else if(state.role==="teacher"){
    const t=TEACHERS[state.teacherId];
    box.innerHTML=`
      <select class="stu-pick" id="tchPick" style="max-width:110px">
        ${TEACHERS.map((x,i)=>`<option value="${i}" ${i===state.teacherId?"selected":""}>${x.name}</option>`).join("")}
      </select>
      <div class="avatar">${t.name.slice(0,1)}</div>
      <div class="meta"><b>${t.name}</b><span>课程主讲 · 主讲任务${t.tasks.join("、")}</span></div>`;
    document.getElementById("tchPick").onchange=e=>{state.teacherId=+e.target.value;render();};
  }else{
    const map={mentor:["王合伙人","中联企业导师"],admin:["平台管理员","系统管理"]};
    const m=map[state.role];
    box.innerHTML=`<div class="avatar">${m[0].slice(0,1)}</div><div class="meta"><b>${m[0]}</b><span>${m[1]}</span></div>`;
  }
}
function renderSidebar(){
  const items=NAV[state.role].map(n=>
    `<div class="nav-item ${state.nav===n.key?"active":""}" data-nav="${n.key}"><span class="ic">${n.ic}</span>${n.t}</div>`).join("");
  document.getElementById("sidebar").innerHTML=`
    <div class="nav-label">${ROLES.find(r=>r.key===state.role).label}工作台</div>
    ${items}
    <div class="side-card">
      <h4>🍋 柠檬信号理论</h4>
      <p>能力不可见 → 弱信号。平台用学习数据、项目成果、评价数据把知识·能力·素养<b style="color:#ffd23d">证据化、可视化</b>，助你从弱信号学员成长为强信号准岗位员工。</p>
    </div>`;
}

function destroyCharts(){Object.values(charts).forEach(c=>{try{c.destroy();}catch(e){}});charts={};}

/* ============================================================
   通用图表
   ============================================================ */
function mkRadar(id,labels,cur,pre){
  const ctx=document.getElementById(id);if(!ctx)return;
  charts[id]=new Chart(ctx,{type:"radar",data:{labels,datasets:[
    {label:"当前信号",data:cur,fill:true,backgroundColor:"rgba(61,110,240,.16)",borderColor:"#3d6ef0",pointBackgroundColor:"#3d6ef0",borderWidth:2,pointRadius:3},
    {label:"课前基线",data:pre,fill:true,backgroundColor:"rgba(245,180,0,.10)",borderColor:"#f5b400",pointBackgroundColor:"#f5b400",borderWidth:2,pointRadius:2,borderDash:[5,4]},
  ]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:"bottom",labels:{font:{family:"Outfit"},usePointStyle:true,boxWidth:8}}},
    scales:{r:{min:0,max:100,ticks:{stepSize:25,backdropColor:"transparent",color:"#8a96ad",font:{size:10}},grid:{color:"#e5ecf5"},angleLines:{color:"#e5ecf5"},pointLabels:{font:{size:11.5,family:"PingFang SC"},color:"#5c6a86"}}}}});
}
function mkBar(id,labels,datasets,opts={}){
  const ctx=document.getElementById(id);if(!ctx)return;
  charts[id]=new Chart(ctx,{type:"bar",data:{labels,datasets},options:{responsive:true,maintainAspectRatio:false,
    plugins:{legend:{display:opts.legend!==false,position:"bottom",labels:{usePointStyle:true,boxWidth:8,font:{family:"Outfit"}}}},
    scales:{y:{beginAtZero:true,max:opts.max||100,grid:{color:"#eef2f8"},ticks:{color:"#8a96ad",font:{size:11}}},x:{grid:{display:false},ticks:{color:"#5c6a86",font:{size:11}}}}}});
}
function mkLine(id,labels,data,demoIdx){
  const ctx=document.getElementById(id);if(!ctx)return;
  const pts=data.map((_,i)=>demoIdx.includes(i)?5:3);
  const pc=data.map((_,i)=>demoIdx.includes(i)?"#f5b400":"#3d6ef0");
  charts[id]=new Chart(ctx,{type:"line",data:{labels,datasets:[{label:"综合信号分",data,borderColor:"#3d6ef0",backgroundColor:"rgba(61,110,240,.12)",fill:true,tension:.35,pointRadius:pts,pointBackgroundColor:pc,pointBorderColor:"#fff",pointBorderWidth:2,borderWidth:3}]},
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{callbacks:{afterBody:(it)=>{const i=it[0].dataIndex;return demoIdx.includes(i)?"★ 课堂展示任务":"";}}}},
    scales:{y:{min:Math.max(0,Math.min(...data)-12),max:100,grid:{color:"#eef2f8"},ticks:{color:"#8a96ad"}},x:{grid:{display:false},ticks:{color:"#5c6a86",font:{size:10.5}}}}}});
}
function mkDoughnut(id,labels,data,colors){
  const ctx=document.getElementById(id);if(!ctx)return;
  charts[id]=new Chart(ctx,{type:"doughnut",data:{labels,datasets:[{data,backgroundColor:colors,borderColor:"#fff",borderWidth:3}]},
    options:{responsive:true,maintainAspectRatio:false,cutout:"62%",plugins:{legend:{position:"right",labels:{usePointStyle:true,boxWidth:9,font:{family:"PingFang SC",size:12},padding:12}}}}});
}
function mkGauge(id,score,hex){
  const ctx=document.getElementById(id);if(!ctx)return;
  charts[id]=new Chart(ctx,{type:"doughnut",data:{datasets:[{data:[score,100-score],backgroundColor:[hex,"#eef2f8"],borderWidth:0,circumference:180,rotation:270}]},
    options:{responsive:true,maintainAspectRatio:false,cutout:"74%",plugins:{legend:{display:false},tooltip:{enabled:false}}}});
}

/* ============================================================
   学生成长画像（公用：学生端 + 教师查看）
   ============================================================ */
function profileHTML(s,p){
  const lv=levelOf(s.composite);
  const segs=LEVELS.map(L=>`<div class="seg ${L.k===lv.key?"on":""}" style="background:${L.hex}">${L.n}<br>${L.r}</div>`).join("");
  return `
  <div class="grid" style="grid-template-columns:340px 1fr;gap:18px" class="col-2">
    <div class="card pad-lg">
      <div class="card-h"><h3>🌱 综合信号分</h3><span class="tag" style="background:${lv.hex}22;color:${lv.hex}">${lv.name}</span></div>
      <div class="gauge-wrap"><canvas id="${p}gauge"></canvas>
        <div class="gauge-center"><div class="score" style="color:${lv.hex}">${s.composite}<small>/100</small></div><div class="lvl" style="color:${lv.hex}">${lv.name}</div></div>
      </div>
      <div class="signal-scale">${segs}</div>
      <p class="helper" style="margin-top:14px">${lv.desc}。</p>
    </div>
    <div class="card pad-lg">
      <div class="card-h"><h3>📐 三维信号构成</h3><span class="sub">知识30% · 能力45% · 素养25%</span></div>
      <div class="dim3">
        <div class="dim k"><div class="dname">知识得分<span class="w">×30%</span></div><div class="dval" style="color:var(--blue)">${s.knowledge}</div><div class="dbar"><i style="width:${s.knowledge}%"></i></div></div>
        <div class="dim a"><div class="dname">能力得分<span class="w">×45%</span></div><div class="dval" style="color:var(--teal)">${s.ability}</div><div class="dbar"><i style="width:${s.ability}%"></i></div></div>
        <div class="dim l"><div class="dname">素养得分<span class="w">×25%</span></div><div class="dval" style="color:var(--lemon-deep)">${s.literacy}</div><div class="dbar"><i style="width:${s.literacy}%"></i></div></div>
      </div>
      <div class="double-helix">
        <div class="helix build"><b>🔧 信号构建（技能）</b><br>RPA采集·Power Query清洗·DAX建模·风险分析——让信号<b>够强</b>。</div>
        <div class="helix pure"><b>🛡️ 信号净化（素养）</b><br>职业怀疑·数据合规·诚信执业——让信号<b>够纯、不掺假</b>。</div>
      </div>
    </div>
  </div>

  <div class="grid" style="grid-template-columns:1fr 1fr;gap:18px;margin-top:18px" class="col-2">
    <div class="card">
      <div class="card-h"><h3>🎯 岗位能力雷达</h3><span class="sub">当前 vs 课前基线</span></div>
      <div class="chart-box"><canvas id="${p}radar"></canvas></div>
    </div>
    <div class="card">
      <div class="card-h"><h3>📈 全链路成长曲线</h3><span class="tag demo">★ 课堂展示任务</span></div>
      <div class="chart-box"><canvas id="${p}line"></canvas></div>
    </div>
  </div>`;
}
function mountProfile(s,p){
  const lv=levelOf(s.composite);
  mkGauge(p+"gauge",s.composite,lv.hex);
  const rl=["财务指标","DAX建模","PQ清洗","RPA采集","采购风险","销售风险","资金分析","职业怀疑","数据合规","报告路演"];
  const cur=[s.met.finReport,s.met.dax,s.met.powerQuery,s.met.rpa,s.met.purchaseRisk,s.met.salesRisk,s.met.cashRisk,s.met.skepticism,s.met.compliance,s.met.report];
  const pre=cur.map(v=>clamp(v-(12+(v%9))));
  mkRadar(p+"radar",rl,cur,pre);
  mkLine(p+"line",TASKS.map(t=>t.short),s.curve,[1,2,4,7]);
}

/* ============================================================
   知识图谱·点亮（学生 + 教师共用）
   ============================================================ */
function kmapGridHTML(s){
  return KMAP.map(t=>{
    const tk=TASKS.find(x=>x.id===t.task);
    const on=t.points.filter(p=>isLit(s,p)).length;
    return `<div class="kmap-task ${tk.demo?"demo":""}">
      <div class="th"><b>${tk.demo?"★ ":""}${tk.name}</b><span class="tp">💡 ${on}/${t.points.length} 已点亮</span></div>
      <div class="bulb-grid">${t.points.map(p=>{
        const sc=pointScore(s,p);const lit=sc>=KSTD;const cm=CAT_META[p.cat];
        return `<div class="bulb ${lit?"on":""} ${state.kmapPoint===p.id?"sel":""}" data-kpt="${p.id}">
          <span class="ic">💡</span>
          <div><div class="nm">${p.name}</div>
          <div class="mt"><span class="cat-dot" style="background:${cm.c}"></span>${cm.n} · ${lit?"已达标 "+sc:"掌握度 "+sc}</div></div>
        </div>`;}).join("")}</div>
    </div>`;
  }).join("");
}
function kmapTipHTML(s){
  if(!state.kmapPoint) return `<div class="tip-panel"><div class="tip-empty"><span class="big">🍋</span>点击任意小灯泡<br>柠檬AI助教会告诉你<br><b style="color:#ffd23d">把它点亮的秘诀</b></div></div>`;
  const p=KALL.find(x=>x.id===state.kmapPoint);
  if(!p) return `<div class="tip-panel"></div>`;
  const sc=pointScore(s,p);const lit=sc>=KSTD;const cm=CAT_META[p.cat];
  const tk=TASKS.find(x=>x.id===KMAP.find(t=>t.points.some(q=>q.id===p.id)).task);
  let body;
  if(lit){
    body=`🎉 <b>已点亮！</b> 掌握度 ${sc}，已达岗位标准线 ${KSTD}。<br><br>${p.tip}<br><br>👉 <b>进阶秘诀：</b>把这个点迁移到新场景，或讲给同学听——能讲清楚，信号才够强。`;
  }else{
    body=`🔅 <b>尚未点亮</b>，掌握度 ${sc}，距达标线 ${KSTD} 还差 <b>${KSTD-sc}</b> 分。<br><br><b>🍋 点亮秘诀：</b><br>${p.tip}<br><br>👉 <b>立即行动：</b>到「短板诊断与提升」完成对应专项练习，再回来点亮它。`;
  }
  return `<div class="tip-panel">
    <div class="tt">💡 ${p.name}</div>
    <div class="tcat">${cm.n} · 来自 ${tk.short} · ${lit?'<span style="color:#5ee0bf">● 已点亮</span>':'<span style="color:#ffb3bd">○ 待点亮</span>'}</div>
    <div class="tip-body">${body}</div>
  </div>`;
}
function kmapHeaderHTML(s,who){
  const on=litCount(s),total=KALL.length,pct=Math.round(on/total*100);
  const k=litByCat(s,"k"),a=litByCat(s,"a"),l=litByCat(s,"l");
  return `<div class="card pad-lg"><div class="card-h"><h3>💡 ${who}知识图谱点亮进度</h3><span class="sub">达标线 ${KSTD} 分即点亮</span></div>
    <div class="kmap-prog">
      <div class="ring-wrap"><canvas id="kRing"></canvas><div class="ring-center"><div class="pct" style="color:var(--lemon-deep)">${pct}%</div><div class="lbl">${on}/${total} 已点亮</div></div></div>
      <div class="cat-stat">
        <div class="cat-box"><div class="ct"><span class="cat-dot" style="background:${CAT_META.k.c}"></span>知识储备点</div><div class="cv" style="color:${CAT_META.k.c}">${k.on}<small>/${k.total}</small></div></div>
        <div class="cat-box"><div class="ct"><span class="cat-dot" style="background:${CAT_META.a.c}"></span>能力技术点</div><div class="cv" style="color:${CAT_META.a.c}">${a.on}<small>/${a.total}</small></div></div>
        <div class="cat-box"><div class="ct"><span class="cat-dot" style="background:${CAT_META.l.c}"></span>素养目标点</div><div class="cv" style="color:${CAT_META.l.c}">${l.on}<small>/${l.total}</small></div></div>
      </div>
    </div></div>`;
}
function renderStudentKmap(){
  destroyCharts();
  const s=STUDENTS[state.studentId];
  document.getElementById("main").innerHTML=`
    <div class="page-head"><div class="crumb">学生工作台 / 知识图谱点亮</div><h2>知识图谱 · 点亮你的能力灯 💡</h2><p>融合教材知识点与审计助理岗位的「知识·能力·素养」要求。达标的点会像小灯泡一样点亮，点击任意灯泡，柠檬AI助教给你点亮秘诀。</p></div>
    ${kmapHeaderHTML(s,"我的")}
    <div class="grid" style="grid-template-columns:1fr 330px;gap:18px;margin-top:18px" class="col-2">
      <div>${kmapGridHTML(s)}</div>
      <div>${kmapTipHTML(s)}</div>
    </div>`;
  mkGauge("kRing",Math.round(litCount(s)/KALL.length*100),"#f5b400");
}
function renderTeacherKmap(){
  destroyCharts();
  const s=STUDENTS[state.kmapStu];
  const classOn=Math.round(avg(STUDENTS.map(x=>litCount(x)/KALL.length*100)));
  const classBlock=KMAP.map(t=>{const tk=TASKS.find(x=>x.id===t.task);
    return `<div style="margin-bottom:13px"><div style="font-weight:700;font-size:13px;margin-bottom:5px">${tk.demo?"★ ":""}${tk.name}</div>
      ${t.points.map(p=>{const r=classLitRate(p);const cm=CAT_META[p.cat];
        return `<div class="kpt-row"><div class="knm"><span class="cat-dot" style="background:${cm.c}"></span>${p.name}</div><div class="lit-bar"><i style="width:${r}%"></i></div><div class="kv" style="color:${r<50?'var(--weak)':r<75?'var(--growth)':'var(--strong)'}">${r}%</div></div>`;}).join("")}
    </div>`;}).join("");
  document.getElementById("main").innerHTML=`
    <div class="page-head"><div class="crumb">教师工作台 / 知识图谱</div><h2>班级知识图谱 💡</h2><p>查看全班每个知识点·能力点·素养点的点亮率（点亮率低=教学攻关重点），也可选择某位学生查看其个人点亮情况与 AI 点亮秘诀。</p></div>
    <div class="kpis" style="margin-bottom:18px">
      <div class="kpi"><div class="ribbon" style="background:var(--lemon-deep)"></div><label>💡 班级平均点亮率</label><div class="v">${classOn}<small>%</small></div><div class="trend flat">共 ${KALL.length} 个能力点</div></div>
      <div class="kpi"><div class="ribbon" style="background:var(--blue)"></div><label>📘 知识储备点</label><div class="v">${KALL.filter(p=>p.cat==="k").length}</div><div class="trend flat">教材知识点</div></div>
      <div class="kpi"><div class="ribbon" style="background:var(--teal)"></div><label>🛠️ 能力技术点</label><div class="v">${KALL.filter(p=>p.cat==="a").length}</div><div class="trend flat">岗位技术能力</div></div>
      <div class="kpi"><div class="ribbon" style="background:var(--strong)"></div><label>🌟 素养目标点</label><div class="v">${KALL.filter(p=>p.cat==="l").length}</div><div class="trend flat">职业素养目标</div></div>
    </div>
    <div class="grid" style="grid-template-columns:1fr 360px;gap:18px" class="col-2">
      <div class="card"><div class="card-h"><h3>🔥 各能力点 · 班级点亮率</h3><span class="sub">越低越需攻关</span></div>${classBlock}</div>
      <div>
        <div class="card" style="margin-bottom:18px"><div class="card-h" style="margin-bottom:12px"><h3>👤 个人图谱</h3>
          <select class="stu-pick" id="kStuPick" style="max-width:130px">${STUDENTS.map(x=>`<option value="${x.id}" ${x.id===state.kmapStu?"selected":""}>${x.name}</option>`).join("")}</select></div>
          ${kmapHeaderHTML(s,s.name+" 的")}
        </div>
        ${kmapTipHTML(s)}
      </div>
    </div>
    <div class="card" style="margin-top:18px"><div class="card-h"><h3>💡 ${s.name} 的知识图谱</h3><span class="sub">点击灯泡看点亮秘诀</span></div>${kmapGridHTML(s)}</div>`;
  mkGauge("kRing",Math.round(litCount(s)/KALL.length*100),"#f5b400");
  document.getElementById("kStuPick").onchange=e=>{state.kmapStu=+e.target.value;state.kmapPoint=null;renderTeacherKmap();};
}

/* ============================================================
   学生端
   ============================================================ */
function renderStudent(){
  const s=STUDENTS[state.studentId];
  const main=document.getElementById("main");
  if(state.nav==="kmap"){renderStudentKmap();return;}
  if(state.nav==="overview"){
    main.innerHTML=`
      <div class="page-head"><div class="crumb">学生工作台 / 成长画像</div>
        <h2>你好，${s.name} 👋</h2><p>这是你在《财务大数据分析》全链路项目中的数智化成长画像。能力会被看见——用数据证明你的「强信号」。</p></div>
      ${profileHTML(s,"self")}`;
    mountProfile(s,"self");
  }
  else if(state.nav==="radar"){
    const rl=["财务指标分析","杜邦分析","业务流程认知","数字化内控","数据模型理论","RPA采集","Power Query清洗","DAX建模","风险识别","报告路演","数据合规","职业怀疑","诚信执业","团队协作"];
    const cur=[s.met.finReport,s.met.dupont,s.met.bizFlow,s.met.internalControl,s.met.modelTheory,s.met.rpa,s.met.powerQuery,s.met.dax,s.riskAnalysis,s.met.report,s.met.compliance,s.met.skepticism,s.met.integrity,s.met.teamwork];
    const pre=cur.map(v=>clamp(v-(13+(v%8))));
    main.innerHTML=`
      <div class="page-head"><div class="crumb">学生工作台 / 能力雷达诊断</div><h2>能力雷达诊断</h2><p>覆盖知识·能力·素养三维 14 项岗位能力，蓝线为当前信号，虚线为课前基线。</p></div>
      <div class="grid" style="grid-template-columns:1.2fr 1fr;gap:18px" class="col-2">
        <div class="card"><div class="card-h"><h3>🎯 14项能力全景雷达</h3></div><div class="chart-box lg"><canvas id="bigRadar"></canvas></div></div>
        <div class="card"><div class="card-h"><h3>📊 三维达成对比</h3><span class="sub">你 vs 班级 vs 岗位标准</span></div><div class="chart-box lg"><canvas id="dimCmp"></canvas></div></div>
      </div>`;
    mkRadar("bigRadar",rl,cur,pre);
    mkBar("dimCmp",["知识","能力","素养"],[
      {label:"我的得分",data:[s.knowledge,s.ability,s.literacy],backgroundColor:"#3d6ef0",borderRadius:6},
      {label:"班级均值",data:[classAvg("knowledge"),classAvg("ability"),classAvg("literacy")],backgroundColor:"#16b6a4",borderRadius:6},
      {label:"岗位标准",data:[80,80,80],backgroundColor:"#ffd23d",borderRadius:6},
    ]);
  }
  else if(state.nav==="growth"){
    main.innerHTML=`
      <div class="page-head"><div class="crumb">学生工作台 / 全链路成长曲线</div><h2>全链路成长曲线</h2><p>「采→治→析→报」八大任务的信号分变化。★ 标记为本学期课堂展示任务。</p></div>
      <div class="card"><div class="card-h"><h3>📈 综合信号分演进（任务一 → 任务八）</h3><span class="tag demo">★ 课堂展示：采集 · 清洗 · 销售分析 · 报告路演</span></div><div class="chart-box lg"><canvas id="growthLine"></canvas></div></div>
      <div class="card" style="margin-top:18px"><div class="card-h"><h3>🧭 各任务信号增幅</h3></div><div class="chart-box"><canvas id="growthBar"></canvas></div></div>`;
    mkLine("growthLine",TASKS.map(t=>t.short),s.curve,[1,2,4,7]);
    const gains=s.curve.map((v,i)=>i===0?0:v-s.curve[i-1]);
    mkBar("growthBar",TASKS.map(t=>t.short),[{label:"较上一任务增幅",data:gains,backgroundColor:gains.map(g=>g>=0?"#16b884":"#ef5a6a"),borderRadius:6}],{legend:false,max:Math.max(...gains)+4});
  }
  else if(state.nav==="weak"){
    const ws=weaknessesOf(s);
    main.innerHTML=`
      <div class="page-head"><div class="crumb">学生工作台 / 短板诊断与提升</div><h2>短板诊断与精准补强</h2><p>平台自动识别低于岗位达标线（72分）的能力项，并推送针对性学习资源与专项练习，形成「诊断→训练→再评价」闭环。</p></div>
      <div class="grid" style="grid-template-columns:1.1fr 1fr;gap:18px" class="col-2">
        <div class="card"><div class="card-h"><h3>🔍 识别到的短板（${ws.length}项）</h3></div>
          ${ws.length? ws.map(w=>`
            <div class="weak-item">
              <div class="top"><span class="name">${w.icon} ${w.label}</span>
                <span><span class="sev ${w.sev}">${w.sev==="high"?"重点突破":"待加强"}</span> <span class="score-pill" style="color:${w.sev==="high"?"var(--weak)":"var(--growth)"}">${w.score}</span></span></div>
              <div class="res-list">${w.res.map(r=>`<div class="res"><span class="pic">▶</span>${r}<span class="go">去练习</span></div>`).join("")}</div>
            </div>`).join("")
          : `<div class="no-weak"><div class="big">🎉</div><b>各项能力均达到岗位达标线</b><br><span class="helper">继续保持，向准岗位员工冲刺！</span></div>`}
        </div>
        <div class="card"><div class="card-h"><h3>📌 提升优先级</h3><span class="sub">能力维度权重最高（45%）</span></div>
          <div class="chart-box lg"><canvas id="weakBar"></canvas></div>
          <p class="helper" style="margin-top:12px">建议优先攻克能力维度短板：每提升能力分对综合信号分贡献最大。完成专项练习后，平台将自动重新评价并刷新你的信号等级。</p>
        </div>
      </div>`;
    const all=WEAK_DEFS.map(d=>({lbl:d.label.replace(/能力弱|不足|弱/g,"").slice(0,6),v:s.met[d.key]}));
    mkBar("weakBar",all.map(a=>a.lbl),[{label:"当前得分",data:all.map(a=>a.v),backgroundColor:all.map(a=>a.v<60?"#ef5a6a":a.v<72?"#f4a52c":"#16b884"),borderRadius:6}],{legend:false});
  }
  else if(state.nav==="data"){
    main.innerHTML=`
      <div class="page-head"><div class="crumb">学生工作台 / 学习数据采集</div><h2>学习数据采集</h2><p>平台持续采集课前测试、课后测试、作业、项目报告、平台提问、课堂任务、企业导师评价、小组互评等过程数据，把你的学习过程「看见」。</p></div>
      <div class="grid" style="grid-template-columns:1fr 1fr;gap:18px" class="col-2">
        <div class="card"><div class="card-h"><h3>🗂️ 八类学习数据来源</h3><span class="sub">过程性评价</span></div>
          ${s.ds.map(d=>`<div class="ds-row"><div class="nm">${d.nm}</div><div class="track"><i style="width:${d.v}%"></i></div><div class="vv">${d.v}</div></div>`).join("")}
        </div>
        <div class="card"><div class="card-h"><h3>📊 过程数据雷达</h3></div><div class="chart-box lg"><canvas id="dsRadar"></canvas></div>
          <p class="helper" style="margin-top:10px">数据来源越均衡、过程表现越扎实，发出的信号越「纯净可信」。企业导师评价与小组互评共同构成你的真实交付证据。</p>
        </div>
      </div>`;
    const ctx=document.getElementById("dsRadar");
    charts["dsRadar"]=new Chart(ctx,{type:"radar",data:{labels:s.ds.map(d=>d.nm),datasets:[{label:"过程数据",data:s.ds.map(d=>d.v),backgroundColor:"rgba(22,182,164,.16)",borderColor:"#16b6a4",pointBackgroundColor:"#16b6a4",borderWidth:2}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{r:{min:0,max:100,ticks:{stepSize:25,backdropColor:"transparent",color:"#8a96ad",font:{size:10}},grid:{color:"#e5ecf5"},angleLines:{color:"#e5ecf5"},pointLabels:{font:{size:11,family:"PingFang SC"},color:"#5c6a86"}}}}});
  }
  else if(state.nav==="ai"){
    main.innerHTML=`
      <div class="page-head"><div class="crumb">学生工作台 / 柠檬AI助教</div><h2>🍋 柠檬AI助教</h2><p>问我关于审计行业、审计知识、法律法规、职业道德、本课程的问题。我也会结合你的学习数据给出个性化建议。</p></div>
      <div class="card"><div class="chat">
        <div class="chat-log" id="chatLog"></div>
        <div class="chat-suggest" id="chatSugg"></div>
        <div class="chat-in"><input id="chatInput" placeholder="例如：我的短板该怎么补？什么是职业怀疑？" /><button id="chatSend">发送</button></div>
      </div></div>`;
    initChat(s);
  }
}

/* ============================================================
   柠檬AI助教（基于规则的演示问答）
   ============================================================ */
function aiReply(q,s){
  const ws=weaknessesOf(s);
  const t=q.toLowerCase();
  const has=arr=>arr.some(k=>q.includes(k)||t.includes(k));
  if(has(["短板","怎么补","怎么提升","该学","提升","薄弱","我该"])){
    if(!ws.length) return `你目前各项能力都达到了岗位达标线，<b>当前综合信号分 ${s.composite}</b>，已是${levelOf(s.composite).name}。建议向准岗位员工冲刺：把咨询报告与路演答辩打磨到可直接交付的水平。`;
    const top=ws[0];
    return `结合你的学习数据，最该优先攻克的是 <b>${top.label}（${top.score}分）</b>。给你三个专项：①${top.res[0]}；②${top.res[1]}；③${top.res[2]}。能力维度在综合信号分里权重最高（45%），补它最划算。`;
  }
  if(has(["柠檬","信号","信息不对称"])) return `「柠檬市场」讲的是信息不对称——企业看不清你到底行不行，能力强但没证据也会被低估。本平台就是帮你把学习过程、项目成果、技能表现<b>数据化、可视化、证据化</b>，让你向岗位发出清晰的「强信号」。`;
  if(has(["职业怀疑"])) return `职业怀疑就是<b>不盲从报表、不盲信 AI 结论</b>。比如销售额涨 20% 但现金流紧张，就要追问是不是压货虚增；对 AI 生成的 DAX 结果要交叉验证、能下钻到明细。建议做「AI 结论找错」和异常凭证识别训练。`;
  if(has(["power query","清洗","pq","脏数据","缺失","重复"])) return `Power Query 清洗的五步：链接→探查→规则→清洗→加载。常见操作：替换值、拆分列、填充、删除重复项、合并查询。多表关联记住「拼图法」——事实表配维度表，关联键要唯一无歧义，否则会数据膨胀。`;
  if(has(["dax","calculate","度量值","建模","上下文"])) return `DAX 核心是 CALCULATE，可以把它想成一个能改筛选条件的「高级筛子」。先掌握 SUM，再练 CALCULATE+FILTER/ALL。难点是<b>上下文转换</b>，建议从「2023年采购总额」「应付账款周转天数」这类有业务含义的度量值练起。`;
  if(has(["rfm","客户","销售分析","分群","价值客户"])) return `RFM 用最近消费(R)、消费频率(F)、消费金额(M)给客户分层。重点辨析「重要价值客户」与「重要挽留客户」(R低F高M高)，并识别客户集中度风险。警惕「虚假繁荣」：销量涨但老客户在流失、低质量客户占比高。`;
  if(has(["数据安全法","合规","隐私","密码"])) return `数据合规是审计的红线：①网银密码绝不能在 Excel 或代码里明文存储，要用 SecureString；②采集数据要守《数据安全法》，不爬取个人隐私；③权限最小化。合规意识是「信号净化」的关键一环。`;
  if(has(["采购","供应商"])) return `采购风险看：采购总额、平均采购单价、付款周期、供应商集中度、关联交易。常见疑点是单一供应商过度依赖、采购单价异常偏高、关联方交易。用 CALCULATE+ALL 算占比，再下钻到明细核验。`;
  if(has(["资金","现金流","回款","增收不增利"])) return `资金分析关注现金流、回款周期、资金链健康度。「增收不增利、卖得越多越缺钱」往往指向回款变慢、存货积压或成本异常。要把销售、采购、资金三端联动起来诊断。`;
  if(has(["报告","路演","答辩","咨询"])) return `咨询报告按「问题—数据—分析—结论—建议」结构写，结论要可追溯到明细。路演 90 秒讲清「问题—数据—成果—强信号」。答辩要能接受质询、说明依据、提出改进——这是从分析师走向咨询顾问的关键。`;
  if(has(["A级","R级","评价","考核"])) return `本课程用 ISO 9001 的 A/R 分级：A级（Audit 审计级，合格交付）、R级（Re-work 返工级）。审计底稿没有「60分万岁」，不合格就要重做——这是底线思维，倒逼你追求高质量交付。`;
  if(has(["你好","在吗","hi","hello"])) return `你好，${s.name}！我是柠檬AI助教 🍋。你可以问我课程知识、审计法规、职业道德，或者直接问「我的短板该怎么补」，我会结合你的数据回答。`;
  return `这是个好问题。我可以帮你解读：①课程技能（Power Query / DAX / RFM / RPA）；②审计风险（采购 / 销售 / 资金）；③职业素养（职业怀疑 / 数据合规 / 诚信执业）；④你的个人短板与提升路径。试着更具体地问我，比如「DAX 的 CALCULATE 怎么用」。`;
}
function pushMsg(role,html){
  const log=document.getElementById("chatLog");
  const d=document.createElement("div");d.className="msg "+role;d.innerHTML=html;log.appendChild(d);log.scrollTop=log.scrollHeight;
}
function initChat(s){
  pushMsg("bot",`你好，<b>${s.name}</b>！我是柠檬AI助教 🍋。当前你的综合信号分是 <b>${s.composite}</b>（${levelOf(s.composite).name}）。问我课程知识、审计法规、职业道德，或让我帮你诊断短板。`);
  const sg=["我的短板该怎么补？","什么是职业怀疑？","CALCULATE 怎么用？","什么是柠檬市场？","数据合规有哪些红线？"];
  document.getElementById("chatSugg").innerHTML=sg.map(x=>`<span class="sugg">${x}</span>`).join("");
  const send=()=>{const v=document.getElementById("chatInput").value.trim();if(!v)return;pushMsg("user",v);document.getElementById("chatInput").value="";setTimeout(()=>pushMsg("bot",aiReply(v,s)),250);};
  document.getElementById("chatSend").onclick=send;
  document.getElementById("chatInput").onkeydown=e=>{if(e.key==="Enter")send();};
  document.querySelectorAll(".sugg").forEach(b=>b.onclick=()=>{pushMsg("user",b.textContent);setTimeout(()=>pushMsg("bot",aiReply(b.textContent,s)),250);});
}

/* ============================================================
   教师端
   ============================================================ */
function renderTeacher(){
  const main=document.getElementById("main");
  const dist=levelDist();
  const above=dist.strong+dist.elite, rework=dist.weak;
  if(state.nav==="overview"){
    main.innerHTML=`
      <div class="page-head"><div class="crumb">教师工作台 / 班级信号概览</div><h2>班级信号概览</h2><p>${CLASS_NAME}（中联学徒班）· ${STUDENTS.length}人 · 《财务大数据分析》全链路项目 · 任课教师：${TEACHERS.map(t=>t.name).join("、")}。</p></div>
      <div class="kpis" style="margin-bottom:18px">
        <div class="kpi"><div class="ribbon" style="background:var(--blue)"></div><label>👥 班级人数</label><div class="v">${STUDENTS.length}<small> 人</small></div><div class="trend flat">学徒制订单班</div></div>
        <div class="kpi"><div class="ribbon" style="background:var(--teal)"></div><label>📊 平均综合信号分</label><div class="v">${classAvg("composite")}<small>/100</small></div><div class="trend up">▲ 较课前提升明显</div></div>
        <div class="kpi"><div class="ribbon" style="background:var(--strong)"></div><label>⭐ 强信号及以上占比</label><div class="v">${Math.round(above/STUDENTS.length*100)}<small>%</small></div><div class="trend up">${above} 人达标</div></div>
        <div class="kpi"><div class="ribbon" style="background:var(--weak)"></div><label>🚩 待整改（弱信号）</label><div class="v">${rework}<small> 人</small></div><div class="trend flat">需靶向辅导</div></div>
      </div>
      <div class="grid" style="grid-template-columns:1fr 1.2fr;gap:18px" class="col-2">
        <div class="card"><div class="card-h"><h3>🔵 全班信号等级分布</h3></div><div class="chart-box"><canvas id="tDist"></canvas></div></div>
        <div class="card"><div class="card-h"><h3>📊 班级知识·能力·素养达成</h3><span class="sub">均值 vs 岗位标准线 80</span></div><div class="chart-box"><canvas id="tDim"></canvas></div></div>
      </div>
      <div class="card" style="margin-top:18px"><div class="card-h"><h3>🎯 班级能力雷达（均值）</h3></div><div class="chart-box"><canvas id="tRadar"></canvas></div></div>`;
    mkDoughnut("tDist",LEVELS.map(L=>`${L.n}（${dist[L.k]}人）`),LEVELS.map(L=>dist[L.k]),LEVELS.map(L=>L.hex));
    mkBar("tDim",["知识","能力","素养"],[
      {label:"班级均值",data:[classAvg("knowledge"),classAvg("ability"),classAvg("literacy")],backgroundColor:"#3d6ef0",borderRadius:6},
      {label:"岗位标准",data:[80,80,80],backgroundColor:"#ffd23d",borderRadius:6},
    ]);
    const rl=["财务指标","DAX建模","PQ清洗","RPA采集","采购风险","销售风险","资金分析","职业怀疑","数据合规","报告路演"];
    const keys=["finReport","dax","powerQuery","rpa","purchaseRisk","salesRisk","cashRisk","skepticism","compliance","report"];
    const cur=keys.map(k=>Math.round(avg(STUDENTS.map(s=>s.met[k]))));
    const pre=cur.map(v=>clamp(v-15));
    mkRadar("tRadar",rl,cur,pre);
  }
  else if(state.nav==="roster"){
    main.innerHTML=`
      <div class="page-head"><div class="crumb">教师工作台 / 学生信号档案</div><h2>学生信号档案</h2><p>点击「成长画像」查看任意学生的完整诊断。表格按综合信号分排序。</p></div>
      <div class="card"><div class="tbl-wrap"><table>
        <thead><tr><th>学生</th><th>项目组</th><th>知识</th><th>能力</th><th>素养</th><th>综合信号分</th><th>信号等级</th><th>主要短板</th><th></th></tr></thead>
        <tbody>${[...STUDENTS].sort((a,b)=>b.composite-a.composite).map(s=>{
          const lv=levelOf(s.composite);const ws=weaknessesOf(s).slice(0,2);
          return `<tr>
            <td><div class="row-name"><span class="mini-av" style="background:${avColor(s.id)}">${avInit(s.name)}</span>${s.name}</div></td>
            <td style="color:var(--ink-2)">${GROUPS[s.group]}</td>
            <td class="sc-num">${s.knowledge}</td><td class="sc-num">${s.ability}</td><td class="sc-num">${s.literacy}</td>
            <td><span class="sc-num" style="font-size:15px;color:${lv.hex}">${s.composite}</span></td>
            <td><span class="lv-pill" style="background:${lv.hex}18;color:${lv.hex}"><span class="d" style="background:${lv.hex}"></span>${lv.name}</span></td>
            <td><div class="wk-tags">${ws.length?ws.map(w=>`<span class="wk-tag">${w.label.replace(/能力弱|不足/g,"").slice(0,6)}</span>`).join(""):`<span class="wk-ok">✓ 全面达标</span>`}</div></td>
            <td><button class="btn-view" data-stu="${s.id}">成长画像</button></td>
          </tr>`;}).join("")}
        </tbody></table></div></div>`;
  }
  else if(state.nav==="heat"){
    const heat=weakHeat();
    const arr=WEAK_DEFS.map(d=>({label:d.label,icon:d.icon,n:heat[d.key]})).sort((a,b)=>b.n-a.n);
    main.innerHTML=`
      <div class="page-head"><div class="crumb">教师工作台 / 班级短板热力</div><h2>班级短板热力分析</h2><p>统计每类短板涉及的学生人数，辅助教师设计分层教学与共性攻关（如「代码诊所」「BI 找茬」）。</p></div>
      <div class="grid" style="grid-template-columns:1.3fr 1fr;gap:18px" class="col-2">
        <div class="card"><div class="card-h"><h3>🔥 各类短板涉及人数</h3></div><div class="chart-box lg"><canvas id="heatBar"></canvas></div></div>
        <div class="card"><div class="card-h"><h3>📋 教学攻关建议</h3></div>
          ${arr.slice(0,5).map(a=>`<div class="weak-item"><div class="top"><span class="name">${a.icon} ${a.label}</span><span class="score-pill" style="color:var(--weak)">${a.n}人</span></div>
            <p class="helper" style="margin-top:6px">建议安排共性攻关 / 推送专项资源包，对 R 级学生启动 48 小时一对一靶向辅导。</p></div>`).join("")}
        </div>
      </div>`;
    mkBar("heatBar",arr.map(a=>a.label.replace(/能力弱|不足/g,"").slice(0,7)),[{label:"涉及人数",data:arr.map(a=>a.n),backgroundColor:"#ef5a6a",borderRadius:6}],{legend:false,max:STUDENTS.length});
  }
  else if(state.nav==="module"){
    renderModule();
  }
  else if(state.nav==="kmap"){
    renderTeacherKmap();
  }
}

/* 教学任务分析：总体 + 按教案任务模块下钻 */
function renderModule(){
  destroyCharts();
  const main=document.getElementById("main");
  const chips=`<div class="chips" style="margin-bottom:18px">
    <span class="chip ${state.moduleTask===0?"active":""}" data-mtask="0">📦 总体分析</span>
    ${TASKS.map(t=>`<span class="chip ${state.moduleTask===t.id?"active":""}" data-mtask="${t.id}">${t.demo?"★ ":""}${t.short}</span>`).join("")}
  </div>`;

  if(state.moduleTask===0){
    // 总体：八大任务对比
    const r=rng(99);
    const completion=TASKS.map(()=>clamp(82+r()*16));
    const quality=TASKS.map(t=>Math.round(avg(STUDENTS.map(s=>taskScore(s,t.id)))));
    main.innerHTML=`
      <div class="page-head"><div class="crumb">教师工作台 / 教学任务分析</div><h2>教学任务分析 · 总体</h2><p>「采→治→析→报」八大教学任务的提交率与平均达成分。点上方任务名可下钻到单个任务的学情分析。★ 为本学期课堂展示任务。</p></div>
      ${chips}
      <div class="card"><div class="card-h"><h3>✅ 各任务提交率 与 平均达成分</h3><span class="tag demo">★ 课堂展示任务</span></div><div class="chart-box lg"><canvas id="taskBar"></canvas></div></div>
      <div class="card" style="margin-top:18px"><div class="card-h"><h3>📋 八大任务一览（点「查看」进入单任务分析）</h3></div>
        <div class="tbl-wrap"><table><thead><tr><th>任务</th><th>核心技能</th><th>主讲教师</th><th>提交率</th><th>平均达成分</th><th>状态</th><th></th></tr></thead><tbody>
        ${TASKS.map((t,i)=>{const tch=TEACHERS.find(x=>x.tasks.includes(t.id));return `<tr><td><b>${t.name}</b></td><td style="color:var(--ink-2);max-width:230px">${TASK_FOCUS[t.id].skills}</td><td>${tch?tch.name:"-"}</td><td class="sc-num">${completion[i]}%</td><td class="sc-num">${quality[i]}</td><td>${t.demo?`<span class="tag demo">★ 课堂展示</span>`:`<span class="status-pill done">已完成</span>`}</td><td><button class="btn-view" data-mtask="${t.id}">查看</button></td></tr>`;}).join("")}
        </tbody></table></div>
      </div>`;
    mkBar("taskBar",TASKS.map(t=>t.short),[
      {label:"提交率%",data:completion,backgroundColor:"#16b6a4",borderRadius:6},
      {label:"平均达成分",data:quality,backgroundColor:"#3d6ef0",borderRadius:6},
    ]);
  }else{
    // 单任务模块分析
    const t=TASKS.find(x=>x.id===state.moduleTask);
    const f=TASK_FOCUS[t.id];
    const tch=TEACHERS.find(x=>x.tasks.includes(t.id));
    const scored=STUDENTS.map(s=>({s,sc:taskScore(s,t.id)})).sort((a,b)=>b.sc-a.sc);
    const aCnt=scored.filter(x=>x.sc>=75).length, rCnt=scored.filter(x=>x.sc<60).length;
    const r=rng(t.id*13+5);
    const submit=clamp(82+r()*16);
    const focusLabels={finReport:"财务指标",dupont:"杜邦分析",bizFlow:"业务认知",internalControl:"内控流程",modelTheory:"模型理论",rpa:"RPA采集",powerQuery:"PQ清洗",dax:"DAX建模",report:"报告路演",purchaseRisk:"采购风险",salesRisk:"销售风险",cashRisk:"资金分析",compliance:"数据合规",skepticism:"职业怀疑",integrity:"诚信执业",teamwork:"团队协作",responsibility:"责任担当"};
    main.innerHTML=`
      <div class="page-head"><div class="crumb">教师工作台 / 教学任务分析 / 单任务</div><h2>${t.name} ${t.demo?'<span class="tag demo" style="vertical-align:middle">★ 课堂展示</span>':''}</h2>
        <p>核心技能：${f.skills}　|　主讲教师：<b>${tch?tch.name:"-"}</b>　|　素养主线：${f.lit}　|　交付物：${f.deliver}</p></div>
      ${chips}
      <div class="kpis" style="margin-bottom:18px">
        <div class="kpi"><div class="ribbon" style="background:var(--teal)"></div><label>📊 任务平均达成分</label><div class="v">${Math.round(avg(scored.map(x=>x.sc)))}<small>/100</small></div><div class="trend flat">聚焦本任务能力项</div></div>
        <div class="kpi"><div class="ribbon" style="background:var(--strong)"></div><label>🅰️ A级（合格交付）</label><div class="v">${aCnt}<small> 人</small></div><div class="trend up">≥75分</div></div>
        <div class="kpi"><div class="ribbon" style="background:var(--weak)"></div><label>🆁 R级（需整改）</label><div class="v">${rCnt}<small> 人</small></div><div class="trend flat">&lt;60分 启动靶向辅导</div></div>
        <div class="kpi"><div class="ribbon" style="background:var(--blue)"></div><label>✅ 提交率</label><div class="v">${submit}<small>%</small></div><div class="trend flat">青蓝云平台记录</div></div>
      </div>
      <div class="grid" style="grid-template-columns:1fr 1fr;gap:18px" class="col-2">
        <div class="card"><div class="card-h"><h3>📈 本任务达成分分布</h3></div><div class="chart-box"><canvas id="mDist"></canvas></div></div>
        <div class="card"><div class="card-h"><h3>🎯 本任务聚焦能力（班级均值）</h3><span class="sub">vs 岗位标准线 80</span></div><div class="chart-box"><canvas id="mFocus"></canvas></div></div>
      </div>
      <div class="card" style="margin-top:18px"><div class="card-h"><h3>👥 学生本任务学情（共${STUDENTS.length}人）</h3><span class="sub">点姓名查看完整成长画像</span></div>
        <div class="tbl-wrap"><table><thead><tr><th>学生</th><th>项目组</th><th>任务达成分</th><th>评级</th><th>本任务薄弱项</th><th>建议</th><th></th></tr></thead><tbody>
        ${scored.map(({s,sc})=>{const ar=sc>=75?"A":sc>=60?"·":"R";
          const weak=f.metrics.map(k=>({k,v:s.met[k]})).sort((a,b)=>a.v-b.v)[0];
          const wlabel=focusLabels[weak.k]||weak.k;
          return `<tr>
            <td><div class="row-name"><span class="mini-av" style="background:${avColor(s.id)}">${avInit(s.name)}</span>${s.name}</div></td>
            <td style="color:var(--ink-2)">${GROUPS[s.group]}</td>
            <td><span class="sc-num" style="font-size:15px;color:${sc>=75?'var(--strong)':sc>=60?'var(--growth)':'var(--weak)'}">${sc}</span></td>
            <td>${ar==="A"?'<span class="ar-badge ar-A">A级</span>':ar==="R"?'<span class="ar-badge ar-R">R级</span>':'<span class="status-pill done">达标</span>'}</td>
            <td>${weak.v<72?`<span class="wk-tag">${wlabel} ${weak.v}</span>`:`<span class="wk-ok">✓ 无明显短板</span>`}</td>
            <td style="font-size:12px;color:var(--ink-2)">${weak.v<72?"推送"+wlabel+"专项练习":"可挑战拓展任务"}</td>
            <td><button class="btn-view" data-stu="${s.id}">画像</button></td>
          </tr>`;}).join("")}
        </tbody></table></div>
      </div>`;
    // 分布直方
    const buckets=[0,0,0,0];
    scored.forEach(x=>{const L=levelOf(x.sc).key;buckets[{weak:0,growth:1,strong:2,elite:3}[L]]++;});
    mkBar("mDist",LEVELS.map(L=>L.n),[{label:"人数",data:buckets,backgroundColor:LEVELS.map(L=>L.hex),borderRadius:6}],{legend:false,max:Math.max(...buckets)+3});
    const fm=f.metrics.map(k=>({lbl:focusLabels[k]||k,v:Math.round(avg(STUDENTS.map(s=>s.met[k])))}));
    mkBar("mFocus",fm.map(x=>x.lbl),[
      {label:"班级均值",data:fm.map(x=>x.v),backgroundColor:"#3d6ef0",borderRadius:6},
      {label:"岗位标准",data:fm.map(()=>80),backgroundColor:"#ffd23d",borderRadius:6},
    ]);
  }
}

/* ============================================================
   企业导师端
   ============================================================ */
function renderMentor(){
  const main=document.getElementById("main");
  if(state.nav==="score"){
    const g=GROUP_EVAL[state.mentorGroup];
    const d=state.draft;
    const comp=Math.round(d.report*0.4+d.roadshow*0.3+d.defense*0.3);
    const ar=comp>=75?"A":"R";
    main.innerHTML=`
      <div class="page-head"><div class="crumb">企业导师工作台 / 小组成果评分</div><h2>小组成果评分</h2><p>对小组的咨询报告、路演表现、答辩表现进行评分与反馈。参照 ISO 9001，评级分 A（审计级·合格交付）与 R（返工级·整改）。</p></div>
      <div class="grid" style="grid-template-columns:300px 1fr;gap:18px" class="col-2">
        <div class="card"><div class="card-h"><h3>🏢 选择项目组</h3></div>
          ${GROUP_EVAL.map(x=>`<div class="grp-card ${x.id===state.mentorGroup?"sel":""}" data-grp="${x.id}" style="margin-bottom:10px">
            <div class="gt">${x.name} <span class="status-pill ${x.evaluated?"done":"pend"}">${x.evaluated?"已评":"待评"}</span></div>
            <div class="gm">${x.size}人 · 组均信号分 ${x.gc}</div></div>`).join("")}
        </div>
        <div class="card pad-lg">
          <div class="card-h"><h3>📝 ${g.name} · 评分</h3><span class="ar-badge ar-${ar}">${ar}级 · ${ar==="A"?"审计级（合格交付）":"返工级（需整改）"}</span></div>
          <div style="display:flex;gap:18px;align-items:center;margin-bottom:8px">
            <div style="font-family:var(--font-d);font-size:13px;color:var(--ink-2)">组员：${GROUP_MEMBERS[state.mentorGroup].map(s=>s.name).join("、")}</div>
          </div>
          <div class="score-input"><label>📄 咨询报告质量</label><input type="range" min="0" max="100" value="${d.report}" id="rg-report"><span class="num" id="num-report">${d.report}</span></div>
          <div class="score-input"><label>🎤 路演表现</label><input type="range" min="0" max="100" value="${d.roadshow}" id="rg-roadshow"><span class="num" id="num-roadshow">${d.roadshow}</span></div>
          <div class="score-input"><label>🗣️ 答辩表现</label><input type="range" min="0" max="100" value="${d.defense}" id="rg-defense"><span class="num" id="num-defense">${d.defense}</span></div>
          <div style="background:var(--surface-2);border:1px solid var(--line);border-radius:12px;padding:14px 16px;margin:10px 0;display:flex;justify-content:space-between;align-items:center">
            <span style="font-weight:600;font-size:13.5px">小组综合评分（报告40%·路演30%·答辩30%）</span>
            <span class="sc-num" style="font-size:26px;color:${comp>=75?'var(--strong)':'var(--weak)'}">${comp}</span>
          </div>
          <label style="font-size:13px;font-weight:600;color:var(--ink-2);display:block;margin-bottom:7px">💬 评价反馈</label>
          <textarea class="ta" id="fb" placeholder="从商业价值、落地可行性、证据链、职业怀疑等角度给出反馈...">${d.feedback}</textarea>
          <div style="margin-top:16px;display:flex;gap:10px"><button class="btn primary" id="submitEval">提交评价</button><button class="btn ghost" id="resetEval">重置</button></div>
        </div>
      </div>`;
    ["report","roadshow","defense"].forEach(k=>{
      const rg=document.getElementById("rg-"+k);
      rg.oninput=e=>{state.draft[k]=+e.target.value;document.getElementById("num-"+k).textContent=e.target.value;renderMentor();};
    });
    document.getElementById("fb").oninput=e=>{state.draft.feedback=e.target.value;};
    document.getElementById("resetEval").onclick=()=>{state.draft={report:80,roadshow:80,defense:80,feedback:""};renderMentor();};
    document.getElementById("submitEval").onclick=()=>{
      const gg=GROUP_EVAL[state.mentorGroup];
      gg.evaluated=true;gg.report=state.draft.report;gg.roadshow=state.draft.roadshow;gg.defense=state.draft.defense;gg.feedback=state.draft.feedback||"（暂无文字反馈）";
      alert("✅ 已提交对「"+gg.name+"」的评价，综合评分 "+comp+"（"+ar+"级）。该成绩将计入学生「企业导师评价」数据，并影响其综合信号分。");
      state.nav="records";render();
    };
  }
  else if(state.nav==="records"){
    main.innerHTML=`
      <div class="page-head"><div class="crumb">企业导师工作台 / 已提交评价</div><h2>已提交评价记录</h2><p>企业导师评价权重占 30%，重点评价咨询成果的商业价值与落地可行性。</p></div>
      <div class="card"><div class="tbl-wrap"><table><thead><tr><th>项目组</th><th>咨询报告</th><th>路演</th><th>答辩</th><th>综合</th><th>ISO评级</th><th>反馈</th></tr></thead><tbody>
        ${GROUP_EVAL.filter(g=>g.evaluated).map(g=>{const c=Math.round(g.report*0.4+g.roadshow*0.3+g.defense*0.3);const ar=c>=75?"A":"R";return `<tr>
          <td><b>${g.name}</b></td><td class="sc-num">${g.report}</td><td class="sc-num">${g.roadshow}</td><td class="sc-num">${g.defense}</td>
          <td><span class="sc-num" style="color:${c>=75?'var(--strong)':'var(--weak)'};font-size:15px">${c}</span></td>
          <td><span class="ar-badge ar-${ar}">${ar}级</span></td>
          <td style="color:var(--ink-2);max-width:320px;font-size:12.5px">${g.feedback}</td></tr>`;}).join("")}
      </tbody></table></div>
      ${GROUP_EVAL.filter(g=>g.evaluated).length===0?`<div class="no-weak"><div class="big">🗃️</div>暂无评价记录</div>`:""}
      </div>`;
  }
}

/* ============================================================
   管理员端
   ============================================================ */
function renderAdmin(){
  const main=document.getElementById("main");
  if(state.nav==="lib"){
    const block=(cls,d)=>`
      <div class="std-dim ${cls}">
        <div class="hd"><h3>${d.name}</h3><span class="ww">权重 ${d.weight}% · 对应强信号：${d.strong}</span></div>
        ${d.items.map(it=>`<div class="std-row"><div class="it">${it.it}</div><div class="desc">${it.desc}</div><div><span class="strong-sig">达标即强信号</span></div></div>`).join("")}
      </div>`;
    main.innerHTML=`
      <div class="page-head"><div class="crumb">管理员工作台 / 岗位信号标准库</div><h2>审计行业岗位信号标准库</h2><p>对接会计师事务所「助理数字化审计咨询师」岗位，把行业人才要求转化为知识·能力·素养三维评价标准——给学生一张「岗位能力地图」。</p></div>
      ${block("k",STD_LIB.k)}${block("a",STD_LIB.a)}${block("l",STD_LIB.l)}`;
  }
  else if(state.nav==="rule"){
    main.innerHTML=`
      <div class="page-head"><div class="crumb">管理员工作台 / 评分规则配置</div><h2>评分规则与信号等级配置</h2><p>综合信号分 = 知识×30% + 能力×45% + 素养×25%。能力维度权重最高，体现「能完成真实任务」的岗位导向。</p></div>
      <div class="grid" style="grid-template-columns:1fr 1fr;gap:18px" class="col-2">
        <div class="card"><div class="card-h"><h3>⚖️ 三维权重</h3></div>
          <div class="chart-box"><canvas id="wPie"></canvas></div>
          <div class="dim3" style="margin-top:14px">
            <div class="dim k"><div class="dname">知识</div><div class="dval" style="color:var(--blue)">30%</div></div>
            <div class="dim a"><div class="dname">能力</div><div class="dval" style="color:var(--teal)">45%</div></div>
            <div class="dim l"><div class="dname">素养</div><div class="dval" style="color:var(--lemon-deep)">25%</div></div>
          </div>
        </div>
        <div class="card"><div class="card-h"><h3>🚦 信号等级阈值</h3></div>
          ${LEVELS.map(L=>`<div class="ds-row"><span class="lv-pill" style="background:${L.hex}18;color:${L.hex};width:auto;margin-right:10px"><span class="d" style="background:${L.hex}"></span>${L.n}学员</span><div class="track"><i style="width:${L.k==='weak'?'30':L.k==='growth'?'67':L.k==='strong'?'82':'95'}%;background:${L.hex}"></i></div><div class="vv" style="width:60px">${L.r}</div></div>`).join("")}
          <div style="margin-top:16px;background:var(--surface-2);border:1px solid var(--line);border-radius:12px;padding:14px">
            <b style="font-size:13.5px">🔄 信号增强闭环</b>
            <p class="helper" style="margin-top:6px">弱信号 → 平台诊断短板 → 推送专项资源 → 完成训练 → 再次评价 → 信号增强。配合 ISO 9001 的 A/R 分级，让学生从「分数考核」走向「能力与品格双重认证」。</p>
          </div>
        </div>
      </div>`;
    mkDoughnut("wPie",["知识 30%","能力 45%","素养 25%"],[30,45,25],["#3d6ef0","#16b6a4","#ffd23d"]);
  }
  else if(state.nav==="data"){
    const dist=levelDist();
    main.innerHTML=`
      <div class="page-head"><div class="crumb">管理员工作台 / 平台数据概览</div><h2>平台数据概览</h2><p>《财务大数据分析》课程 · 全平台运行数据。</p></div>
      <div class="kpis" style="margin-bottom:18px">
        <div class="kpi"><div class="ribbon" style="background:var(--blue)"></div><label>🎓 在册学生</label><div class="v">${STUDENTS.length}</div><div class="trend flat">${CLASS_NAME}</div></div>
        <div class="kpi"><div class="ribbon" style="background:var(--teal)"></div><label>👥 项目组</label><div class="v">${GROUPS.length}</div><div class="trend flat">学徒制项目化</div></div>
        <div class="kpi"><div class="ribbon" style="background:var(--lemon-deep)"></div><label>📚 标准库能力项</label><div class="v">${STD_LIB.k.items.length+STD_LIB.a.items.length+STD_LIB.l.items.length}</div><div class="trend flat">知识+能力+素养</div></div>
        <div class="kpi"><div class="ribbon" style="background:var(--strong)"></div><label>📈 全链路任务</label><div class="v">${TASKS.length}</div><div class="trend flat">采→治→析→报</div></div>
      </div>
      <div class="grid" style="grid-template-columns:1fr 1fr;gap:18px" class="col-2">
        <div class="card"><div class="card-h"><h3>🔵 全平台信号等级分布</h3></div><div class="chart-box"><canvas id="aDist"></canvas></div></div>
        <div class="card"><div class="card-h"><h3>👤 角色账号分布</h3></div><div class="chart-box"><canvas id="aRole"></canvas></div></div>
      </div>`;
    mkDoughnut("aDist",LEVELS.map(L=>`${L.n}（${dist[L.k]}）`),LEVELS.map(L=>dist[L.k]),LEVELS.map(L=>L.hex));
    mkBar("aRole",["学生","教师","企业导师","管理员"],[{label:"账号数",data:[STUDENTS.length,TEACHERS.length,3,1],backgroundColor:["#3d6ef0","#16b6a4","#f5b400","#6366f1"],borderRadius:6}],{legend:false,max:STUDENTS.length});
  }
  else if(state.nav==="roles"){
    const roleDesc=[
      {icon:"🎓",t:"学生",d:"查看个人成长画像、能力雷达、成长曲线、短板诊断与推荐资源，使用柠檬AI助教答疑。",n:STUDENTS.length},
      {icon:"📊",t:"教师",d:"按教案任务模块查看每个任务的学情(任务达成分、A/R评级、薄弱项)与总体分析，查看全班信号分布、短板、知识能力素养达成，下钻每位学生成长画像。任课教师：胡老师、凌老师、符老师、解老师。",n:TEACHERS.length},
      {icon:"🏢",t:"企业导师",d:"对小组咨询报告、路演、答辩进行评分与反馈，评价权重 30%，把关商业价值与落地性。",n:3},
      {icon:"⚙️",t:"管理员",d:"维护岗位信号标准库、配置评分规则与信号等级阈值，掌握平台运行数据。",n:1},
    ];
    main.innerHTML=`
      <div class="page-head"><div class="crumb">管理员工作台 / 角色与账号</div><h2>角色与账号管理</h2><p>平台四类角色协同：标准定信号 · 数据识信号 · 智能强信号。</p></div>
      <div class="grid" style="grid-template-columns:1fr 1fr;gap:18px" class="col-2">
        ${roleDesc.map(r=>`<div class="card"><div class="card-h"><h3>${r.icon} ${r.t}</h3><span class="tag lemon">${r.n} 个账号</span></div><p class="helper" style="font-size:13px;line-height:1.7">${r.d}</p></div>`).join("")}
      </div>`;
  }
}

/* ============================================================
   主渲染
   ============================================================ */
function render(){
  destroyCharts();
  renderRoleSwitch();renderWho();renderSidebar();
  if(state.role==="student")renderStudent();
  else if(state.role==="teacher")renderTeacher();
  else if(state.role==="mentor")renderMentor();
  else renderAdmin();
}

/* 事件委托 */
document.addEventListener("click",e=>{
  const rb=e.target.closest("[data-role]");
  if(rb){state.role=rb.dataset.role;state.nav=NAV[state.role][0].key;render();return;}
  const nb=e.target.closest("[data-nav]");
  if(nb){state.nav=nb.dataset.nav;render();return;}
  const mt=e.target.closest("[data-mtask]");
  if(mt){state.moduleTask=+mt.dataset.mtask;state.nav="module";renderModule();return;}
  const kp=e.target.closest("[data-kpt]");
  if(kp){state.kmapPoint=kp.dataset.kpt;if(state.role==="teacher")renderTeacherKmap();else renderStudentKmap();return;}
  const sv=e.target.closest("[data-stu]");
  if(sv){openStudentModal(+sv.dataset.stu);return;}
  const gp=e.target.closest("[data-grp]");
  if(gp){state.mentorGroup=+gp.dataset.grp;const g=GROUP_EVAL[state.mentorGroup];state.draft={report:g.report,roadshow:g.roadshow,defense:g.defense,feedback:g.feedback};renderMentor();return;}
  if(e.target.id==="modalMask"||e.target.classList.contains("x")){closeModal();}
});

function openStudentModal(id){
  const s=STUDENTS[id];
  document.getElementById("modalBody").innerHTML=`
    <button class="x">✕</button>
    <div class="page-head" style="margin-bottom:16px"><div class="crumb">学生成长画像</div><h2 style="font-size:21px">${s.name} · ${GROUPS[s.group]}</h2></div>
    ${profileHTML(s,"tm")}
    <div class="card" style="margin-top:18px"><div class="card-h"><h3>🔍 短板与推荐</h3></div>
      ${weaknessesOf(s).length? weaknessesOf(s).map(w=>`<div class="weak-item"><div class="top"><span class="name">${w.icon} ${w.label}</span><span class="score-pill" style="color:${w.sev==='high'?'var(--weak)':'var(--growth)'}">${w.score}</span></div><div class="res-list">${w.res.map(r=>`<div class="res"><span class="pic">▶</span>${r}</div>`).join("")}</div></div>`).join("") : `<div class="no-weak"><div class="big">🎉</div>各项能力均达标</div>`}
    </div>`;
  document.getElementById("modalMask").classList.add("show");
  mountProfile(s,"tm");
}
function closeModal(){document.getElementById("modalMask").classList.remove("show");}

render();
</script>
</body>
</html>
