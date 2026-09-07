<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>九羽 于 · 全栈工程师 / AI 应用开发</title>
<style>
  :root{
    --bg:#05070f;
    --bg2:#0a0f1e;
    --panel:rgba(16,24,45,.55);
    --panel-line:rgba(0,240,255,.16);
    --cyan:#00f0ff;
    --magenta:#ff2d95;
    --purple:#8b5cff;
    --text:#e6f1ff;
    --muted:#8b96b8;
    --glow:0 0 24px rgba(0,240,255,.35);
    --font:'Segoe UI','PingFang SC','Microsoft YaHei','Noto Sans SC',system-ui,sans-serif;
    --mono:'Consolas','SF Mono','JetBrains Mono',monospace;
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    font-family:var(--font);
    background:radial-gradient(1200px 700px at 15% -10%, #12204a 0%, transparent 55%),
               radial-gradient(1000px 700px at 110% 20%, #2a0f42 0%, transparent 50%),
               var(--bg);
    color:var(--text);
    min-height:100vh;
    overflow-x:hidden;
    line-height:1.65;
  }
  /* 粒子背景 */
  #bg{position:fixed;inset:0;z-index:0;pointer-events:none;opacity:.9;}
  /* CRT 扫描线 */
  .scanlines{
    position:fixed;inset:0;z-index:1;pointer-events:none;
    background:repeating-linear-gradient(0deg, rgba(255,255,255,.025) 0 1px, transparent 1px 3px);
    mix-blend-mode:overlay;
  }
  /* 网格 */
  .grid-overlay{
    position:fixed;inset:0;z-index:0;pointer-events:none;
    background-image:linear-gradient(rgba(0,240,255,.05) 1px,transparent 1px),
                     linear-gradient(90deg,rgba(0,240,255,.05) 1px,transparent 1px);
    background-size:44px 44px;
    mask-image:radial-gradient(ellipse at 50% 40%, #000 30%, transparent 75%);
  }
  .container{
    position:relative;z-index:2;
    max-width:1060px;margin:0 auto;padding:40px 24px 80px;
    display:grid;grid-template-columns:320px 1fr;gap:28px;
    align-items:start;
  }

  /* ---------- 通用卡片 ---------- */
  .card{
    background:var(--panel);
    border:1px solid var(--panel-line);
    border-radius:18px;
    padding:26px;
    backdrop-filter:blur(14px);
    -webkit-backdrop-filter:blur(14px);
    box-shadow:0 10px 40px rgba(0,0,0,.45), inset 0 0 0 1px rgba(255,255,255,.02);
    position:relative;
    overflow:hidden;
  }
  .card::before{
    content:"";position:absolute;inset:0;border-radius:18px;padding:1px;
    background:linear-gradient(135deg, rgba(0,240,255,.4), transparent 30%, transparent 70%, rgba(255,45,149,.4));
    -webkit-mask:linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
    -webkit-mask-composite:xor;mask-composite:exclude;
    opacity:.5;pointer-events:none;
  }
  .card h2{
    font-size:15px;letter-spacing:.14em;text-transform:uppercase;
    color:var(--cyan);margin-bottom:18px;display:flex;align-items:center;gap:10px;
  }
  .card h2::after{content:"";flex:1;height:1px;background:linear-gradient(90deg,var(--panel-line),transparent);}
  .card h2 .icon{width:22px;height:22px;flex:none;}

  /* ---------- 侧边栏 ---------- */
  .sidebar{position:sticky;top:32px;display:flex;flex-direction:column;gap:24px;}
  .hero{text-align:center;padding:34px 22px 26px;}
  .avatar-wrap{position:relative;width:150px;height:150px;margin:0 auto 22px;}
  .avatar-ring{
    position:absolute;inset:-8px;border-radius:50%;
    background:conic-gradient(from 0deg,var(--cyan),var(--purple),var(--magenta),var(--cyan));
    animation:spin 6s linear infinite;filter:drop-shadow(0 0 14px rgba(0,240,255,.5));
  }
  .avatar-ring::after{content:"";position:absolute;inset:6px;border-radius:50%;background:var(--bg2);}
  @keyframes spin{to{transform:rotate(360deg);}}
  .avatar{
    position:absolute;inset:0;border-radius:50%;overflow:hidden;
    background:linear-gradient(160deg,#1a2340,#0b0f1e);
    z-index:2;box-shadow:0 0 30px rgba(139,92,255,.4);
  }
  .avatar svg{width:100%;height:100%;display:block;}
  .status{
    position:absolute;right:4px;bottom:6px;z-index:3;
    background:#0c1424;border:1px solid var(--cyan);border-radius:99px;
    padding:2px 10px;font-size:10px;letter-spacing:.1em;color:var(--cyan);
    display:flex;align-items:center;gap:5px;box-shadow:var(--glow);
  }
  .status::before{content:"";width:6px;height:6px;border-radius:50%;background:#00ffa3;box-shadow:0 0 8px #00ffa3;animation:blink 1.6s infinite;}
  @keyframes blink{50%{opacity:.3;}}
  .hero .name{
    font-size:28px;font-weight:800;letter-spacing:.02em;
    background:linear-gradient(90deg,#fff,#9be8ff,#b48bff);
    -webkit-background-clip:text;background-clip:text;color:transparent;
  }
  .hero .jp{font-size:12px;color:var(--muted);letter-spacing:.3em;margin-top:2px;}
  .hero .role{
    margin-top:14px;display:flex;flex-wrap:wrap;gap:8px;justify-content:center;
  }
  .tag{
    font-family:var(--mono);font-size:11px;letter-spacing:.06em;
    padding:4px 11px;border-radius:6px;
    border:1px solid var(--panel-line);color:var(--cyan);background:rgba(0,240,255,.06);
  }
  .tag.hot{border-color:rgba(255,45,149,.4);color:#ff7ac1;background:rgba(255,45,149,.08);}

  .line{display:flex;align-items:center;gap:10px;padding:7px 0;font-size:13px;color:var(--text);}
  .line .ic{width:26px;height:26px;flex:none;display:grid;place-items:center;border-radius:7px;
    background:rgba(0,240,255,.08);border:1px solid var(--panel-line);color:var(--cyan);}
  .line .ic svg{width:14px;height:14px;}
  .line .muted{color:var(--muted);font-size:12px;}
  .line a{color:inherit;text-decoration:none;border-bottom:1px dashed rgba(0,240,255,.4);}
  .line a:hover{color:var(--cyan);}

  /* 技能条 */
  .skill{margin-bottom:15px;}
  .skill .top{display:flex;justify-content:space-between;font-size:12px;margin-bottom:6px;}
  .skill .top b{font-weight:600;letter-spacing:.04em;}
  .skill .top span{color:var(--muted);font-family:var(--mono);}
  .bar{height:6px;border-radius:99px;background:rgba(255,255,255,.06);overflow:hidden;}
  .bar i{
    display:block;height:100%;border-radius:99px;width:0;
    background:linear-gradient(90deg,var(--cyan),var(--purple),var(--magenta));
    box-shadow:var(--glow);transition:width 1.2s cubic-bezier(.2,.7,.2,1);
  }

  /* 角色卡 */
  .charcard{font-size:12.5px;}
  .attr{display:grid;grid-template-columns:64px 1fr;gap:6px 12px;padding:6px 0;border-bottom:1px dashed rgba(255,255,255,.06);}
  .attr:last-child{border-bottom:none;}
  .attr .k{color:var(--muted);}
  .attr .v{color:var(--text);}
  .attr .v.cyan{color:var(--cyan);}

  /* ---------- 主区域 ---------- */
  .main{display:flex;flex-direction:column;gap:24px;}

  .about p{color:#c6d2ee;font-size:14px;}
  .about p + p{margin-top:12px;}
  .hl{color:var(--cyan);font-weight:600;}

  /* 时间线 */
  .timeline{position:relative;padding-left:20px;}
  .timeline::before{content:"";position:absolute;left:5px;top:6px;bottom:6px;width:1px;
    background:linear-gradient(var(--cyan),var(--purple),var(--magenta));opacity:.5;}
  .tl-item{position:relative;padding:0 0 24px 18px;}
  .tl-item::before{content:"";position:absolute;left:-19px;top:6px;width:9px;height:9px;border-radius:50%;
    background:var(--bg);border:2px solid var(--cyan);box-shadow:0 0 10px var(--cyan);}
  .tl-item:last-child{padding-bottom:4px;}
  .tl-head{display:flex;flex-wrap:wrap;align-items:baseline;gap:8px;margin-bottom:4px;}
  .tl-head h3{font-size:15px;font-weight:700;}
  .tl-head .org{color:var(--cyan);font-family:var(--mono);font-size:12.5px;}
  .tl-time{font-family:var(--mono);font-size:11px;color:var(--muted);letter-spacing:.05em;}
  .tl-item p{color:#c6d2ee;font-size:13px;margin-top:4px;}
  .tl-item ul{margin:8px 0 0 18px;color:#b9c5e2;font-size:13px;}
  .tl-item li{margin-bottom:5px;}
  .tl-item li::marker{color:var(--cyan);}

  /* 项目 */
  .proj{margin-bottom:22px;}
  .proj:last-child{margin-bottom:0;}
  .proj .p-head{display:flex;flex-wrap:wrap;align-items:center;gap:10px;margin-bottom:6px;}
  .proj h3{font-size:15px;font-weight:700;}
  .proj .p-tags{display:flex;flex-wrap:wrap;gap:6px;margin:8px 0;}
  .proj .p-tags span{
    font-family:var(--mono);font-size:10.5px;padding:3px 8px;border-radius:5px;
    background:rgba(139,92,255,.12);color:#c9b3ff;border:1px solid rgba(139,92,255,.28);
  }
  .proj p{color:#b9c5e2;font-size:13px;}
  .proj .metric{color:var(--cyan);font-family:var(--mono);font-size:12px;}

  /* 荣誉 */
  .awards{display:grid;grid-template-columns:1fr 1fr;gap:12px;}
  .award{border:1px solid var(--panel-line);border-radius:12px;padding:14px;background:rgba(255,255,255,.02);}
  .award .a-name{font-size:13.5px;font-weight:700;margin-bottom:3px;}
  .award .a-meta{font-size:11.5px;color:var(--muted);font-family:var(--mono);}
  .award .a-level{font-size:11px;color:#ffd166;}

  .reveal{opacity:0;transform:translateY(18px);transition:opacity .7s ease, transform .7s ease;}
  .reveal.visible{opacity:1;transform:none;}

  /* 页脚 */
  footer{position:relative;z-index:2;text-align:center;color:var(--muted);font-size:12px;
    padding:30px 0 50px;font-family:var(--mono);letter-spacing:.05em;}

  @media (max-width:840px){
    .container{grid-template-columns:1fr;}
    .sidebar{position:static;}
    .awards{grid-template-columns:1fr;}
  }
  @media print{
    #bg,.scanlines,.grid-overlay{display:none;}
    body{background:#0a0f1e;}
    .card{break-inside:avoid;}
    .reveal{opacity:1;transform:none;}
  }
</style>
</head>
<body>
<canvas id="bg"></canvas>
<div class="grid-overlay"></div>
<div class="scanlines"></div>

<div class="container">

  <!-- ============ 侧边栏 ============ -->
  <aside class="sidebar">
    <div class="card hero">
      <div class="avatar-wrap">
        <div class="avatar-ring"></div>
        <div class="avatar">
          <!-- 二次元虚拟形象（自绘 SVG，可替换成自己的头像图） -->
          <svg viewBox="0 0 220 240" xmlns="http://www.w3.org/2000/svg">
            <defs>
              <linearGradient id="hairG" x1="0" y1="0" x2="1" y2="1">
                <stop offset="0" stop-color="#8ecbff"/><stop offset="1" stop-color="#b48bff"/>
              </linearGradient>
              <radialGradient id="eyeG" cx="0.5" cy="0.5" r="0.5">
                <stop offset="0" stop-color="#7df9ff"/><stop offset=".6" stop-color="#3b82f6"/><stop offset="1" stop-color="#7b2fff"/>
              </radialGradient>
            </defs>
            <!-- 后发 + 双马尾 -->
            <ellipse cx="110" cy="100" rx="70" ry="74" fill="url(#hairG)"/>
            <path d="M46 132 C28 152 22 192 38 214 C54 226 72 208 74 180 C76 160 70 140 58 128 Z" fill="url(#hairG)"/>
            <path d="M174 132 C192 152 198 192 182 214 C166 226 148 208 146 180 C144 160 150 140 162 128 Z" fill="url(#hairG)"/>
            <!-- 身体 -->
            <path d="M72 242 C72 204 90 192 110 192 C130 192 148 204 148 242 Z" fill="#1b2440"/>
            <path d="M110 192 L110 210" stroke="#00f0ff" stroke-width="3" stroke-linecap="round"/>
            <!-- 脖子 -->
            <rect x="99" y="176" width="22" height="18" fill="#ffd9c9"/>
            <!-- 脸 -->
            <ellipse cx="110" cy="122" rx="52" ry="56" fill="#ffe0d2"/>
            <!-- 腮红 -->
            <ellipse cx="70" cy="147" rx="11" ry="6" fill="#ff9bb0" opacity=".55"/>
            <ellipse cx="150" cy="147" rx="11" ry="6" fill="#ff9bb0" opacity=".55"/>
            <!-- 眉毛 -->
            <path d="M72 106 Q88 98 104 106" stroke="#b48bff" stroke-width="2.5" fill="none" stroke-linecap="round"/>
            <path d="M116 106 Q132 98 148 106" stroke="#b48bff" stroke-width="2.5" fill="none" stroke-linecap="round"/>
            <!-- 眼睛 -->
            <ellipse cx="88" cy="126" rx="17" ry="20" fill="#fff"/>
            <ellipse cx="132" cy="126" rx="17" ry="20" fill="#fff"/>
            <circle cx="89" cy="129" r="13" fill="url(#eyeG)"/>
            <circle cx="131" cy="129" r="13" fill="url(#eyeG)"/>
            <circle cx="89" cy="130" r="5" fill="#1a1a2e"/>
            <circle cx="131" cy="130" r="5" fill="#1a1a2e"/>
            <circle cx="83" cy="122" r="5" fill="#fff"/>
            <circle cx="125" cy="122" r="5" fill="#fff"/>
            <circle cx="95" cy="136" r="2.6" fill="#fff"/>
            <circle cx="137" cy="136" r="2.6" fill="#fff"/>
            <path d="M71 122 Q88 106 105 122" stroke="#3a2f55" stroke-width="3" fill="none" stroke-linecap="round"/>
            <path d="M115 122 Q132 106 149 122" stroke="#3a2f55" stroke-width="3" fill="none" stroke-linecap="round"/>
            <!-- 嘴巴 -->
            <path d="M104 153 Q110 159 116 153" stroke="#c05b6b" stroke-width="2.2" fill="none" stroke-linecap="round"/>
            <!-- 前发 / 刘海 -->
            <path d="M58 118 C60 70 90 54 110 54 C130 54 160 70 162 118 C152 94 141 102 131 90 C121 102 99 102 89 90 C79 102 68 94 58 118 Z" fill="url(#hairG)"/>
            <!-- 呆毛 -->
            <path d="M110 54 C110 40 120 34 129 37 C125 44 116 47 110 54 Z" fill="url(#hairG)"/>
          </svg>
        </div>
        <div class="status">ONLINE</div>
      </div>

      <div class="name">九羽 于</div> <!-- 【替换为你的真实姓名】 -->
      <div class="jp">HOSHINO RIN · スターフィールド</div>
      <div class="role">
        <span class="tag">全栈工程师</span>
        <span class="tag hot">AI 应用开发</span>
        <span class="tag">数据工程</span>
      </div>
    </div>

    <div class="card">
      <h2><span class="icon">◈</span> 联系方式</h2>
      <div class="line"><span class="ic">✉</span>【你的邮箱】</div>
      <div class="line"><span class="ic">☎</span>【你的电话】</div>
      <div class="line"><span class="ic">⌘</span><a href="#">github.com/【你的ID】</a></div>
      <div class="line"><span class="ic">◉</span>【你的城市】 · 应届可全职</div>
    </div>

    <div class="card">
      <h2><span class="icon">⚙</span> 技术栈</h2>
      <div class="skill"><div class="top"><b>前端</b><span>React / Vue / TS</span></div><div class="bar"><i data-w="85"></i></div></div>
      <div class="skill"><div class="top"><b>后端</b><span>Node / Python / Go</span></div><div class="bar"><i data-w="80"></i></div></div>
      <div class="skill"><div class="top"><b>AI / LLM</b><span>Prompt · RAG · Agent</span></div><div class="bar"><i data-w="88"></i></div></div>
      <div class="skill"><div class="top"><b>数据</b><span>SQL / Pandas / 可视化</span></div><div class="bar"><i data-w="75"></i></div></div>
      <div class="skill"><div class="top"><b>工程化</b><span>Docker / Git / CI</span></div><div class="bar"><i data-w="72"></i></div></div>
    </div>

    <div class="card charcard">
      <h2><span class="icon">✦</span> 角色设定</h2>
      <div class="attr"><span class="k">代号</span><span class="v cyan">Rin / 凛</span></div>
      <div class="attr"><span class="k">阵营</span><span class="v">秩序 · 守序善良</span></div>
      <div class="attr"><span class="k">装备</span><span class="v">MacBook · VSCode · 机械键盘</span></div>
      <div class="attr"><span class="k">必杀技</span><span class="v">让 Bug 自己害怕</span></div>
      <div class="attr"><span class="k">属性</span><span class="v">夜猫子 / 咖啡驱动</span></div>
    </div>
  </aside>

  <!-- ============ 主区域 ============ -->
  <main class="main">

    <div class="card about reveal">
      <h2><span class="icon">◈</span> 关于我</h2>
      <p>热爱构建兼具<span class="hl">工程美感</span>与<span class="hl">实用价值</span>的产品，从需求拆解到上线部署都能独立闭环。持续在 <span class="hl">AI / 大模型应用</span> 方向深耕，擅长把前沿技术快速落地为可用的工具。</p>
      <p>坚信「<span class="hl">可维护的代码是写给下一个人看的情书</span>」，追求清晰、克制、可复用的实现。正在寻找一份能让我同时<span class="hl">写代码、做产品、碰 AI</span> 的工作。</p>
    </div>

    <div class="card reveal">
      <h2><span class="icon">◈</span> 项目经历</h2>

      <div class="proj">
        <div class="p-head"><h3>【项目名称一】</h3><span class="metric">▲ 影响力 / 用户数</span></div>
        <div class="p-tags"><span>LLM</span><span>RAG</span><span>TypeScript</span><span>向量数据库</span></div>
        <p>基于大语言模型与检索增强（RAG）构建的【智能问答 / 助手 / 工具】。负责【架构设计 / 数据链路 / 前端实现】，优化了【关键环节】。</p>
      </div>

      <div class="proj">
        <div class="p-head"><h3>【项目名称二】</h3><span class="metric">▲ 开源 · ★ 2k+</span></div>
        <div class="p-tags"><span>Python</span><span>FastAPI</span><span>Docker</span><span>CI/CD</span></div>
        <p>从 0 到 1 设计并实现的后端服务，抽象出可复用的【模块 / 中间件】，通过自动化测试与流水线保证交付质量。</p>
      </div>

      <div class="proj">
        <div class="p-head"><h3>【项目名称三】</h3><span class="metric">▲ 数据驱动</span></div>
        <div class="p-tags"><span>数据分析</span><span>可视化</span><span>Pandas</span></div>
        <p>对【某业务 / 数据集】进行分析与建模，产出可交互看板与洞察报告，推动【某决策 / 优化】落地。</p>
      </div>
    </div>

    <div class="card reveal">
      <h2><span class="icon">◈</span> 实习经历</h2>
      <div class="timeline">
        <div class="tl-item">
          <div class="tl-head"><h3>【岗位】</h3><span class="org">【公司名】</span></div>
          <div class="tl-time">【2025.06 — 2025.09】</div>
          <ul>
            <li>负责【核心模块】，落地【具体成果】。</li>
            <li>参与【某专项 / 重构 / 优化】，产出可量化的结果。</li>
          </ul>
        </div>
        <div class="tl-item">
          <div class="tl-head"><h3>【岗位】</h3><span class="org">【公司名】</span></div>
          <div class="tl-time">【2024.07 — 2024.09】</div>
          <ul>
            <li>独立完成【某功能】，覆盖【场景】。</li>
            <li>与团队协作优化【某流程】，沉淀为可复用文档。</li>
          </ul>
        </div>
      </div>
    </div>

    <div class="card reveal">
      <h2><span class="icon">◈</span> 教育背景</h2>
      <div class="tl-item" style="padding:0 0 0 18px;">
        <div class="tl-head"><h3>【专业名称】</h3><span class="org">【学校名】</span></div>
        <div class="tl-time">【20XX.09 — 20XX.06】 · 本科 · GPA【X.X/4.0】</div>
        <p>主修课程：数据结构、操作系统、数据库、机器学习、计算机网络。</p>
      </div>
    </div>

    <div class="card reveal">
      <h2><span class="icon">◈</span> 荣誉 &amp; 证书</h2>
      <div class="awards">
        <div class="award"><div class="a-name">【奖项名称】</div><div class="a-meta">【20XX】</div><div class="a-level">★【级别】</div></div>
        <div class="award"><div class="a-name">【奖项名称】</div><div class="a-meta">【20XX】</div><div class="a-level">★【级别】</div></div>
        <div class="award"><div class="a-name">【证书：如 CET-6 / 软考 / 云厂商认证】</div><div class="a-meta">【20XX】</div></div>
        <div class="award"><div class="a-name">【竞赛 / 活动经历】</div><div class="a-meta">【20XX】</div></div>
      </div>
    </div>

  </main>
</div>

<footer>HOSHINO RIN · RESUME v2.0 · POWERED BY COFFEE &amp; CODE</footer>

<script>
  // ---------- 粒子背景 ----------
  const cv = document.getElementById('bg');
  const ctx = cv.getContext('2d');
  let W, H, pts = [];
  function resize(){
    W = cv.width = innerWidth; H = cv.height = innerHeight;
  }
  resize(); addEventListener('resize', resize);
  const N = Math.min(120, Math.floor(innerWidth/12));
  for(let i=0;i<N;i++){
    pts.push({x:Math.random()*W, y:Math.random()*H,
      vx:(Math.random()-.5)*.35, vy:(Math.random()-.5)*.35,
      r:Math.random()*1.6+.4, c:['0,240,255','139,92,255','255,45,149'][i%3]});
  }
  function tick(){
    ctx.clearRect(0,0,W,H);
    for(let i=0;i<pts.length;i++){
      const p = pts[i];
      p.x += p.vx; p.y += p.vy;
      if(p.x<0||p.x>W)p.vx*=-1;
      if(p.y<0||p.y>H)p.vy*=-1;
      ctx.beginPath();
      ctx.fillStyle = 'rgba('+p.c+','+(Math.random()*.5+.2)+')';
      ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
      ctx.fill();
    }
    // 连线
    for(let i=0;i<pts.length;i++){
      for(let j=i+1;j<pts.length;j++){
        const a=pts[i], b=pts[j], dx=a.x-b.x, dy=a.y-b.y, d=dx*dx+dy*dy;
        if(d<10000){
          ctx.strokeStyle='rgba(0,240,255,'+(0.08*(1-d/10000))+')';
          ctx.lineWidth=.5;
          ctx.beginPath(); ctx.moveTo(a.x,a.y); ctx.lineTo(b.x,b.y); ctx.stroke();
        }
      }
    }
    requestAnimationFrame(tick);
  }
  tick();

  // ---------- 技能条动画 ----------
  const bars = document.querySelectorAll('.bar i');
  const barIO = new IntersectionObserver(es=>{
    es.forEach(e=>{ if(e.isIntersecting){ e.target.style.width = e.target.dataset.w+'%'; barIO.unobserve(e.target); }});
  },{threshold:.3});
  bars.forEach(b=>barIO.observe(b));

  // ---------- 滚动渐入 ----------
  const rev = new IntersectionObserver(es=>{
    es.forEach(e=>{ if(e.isIntersecting){ e.target.classList.add('visible'); rev.unobserve(e.target); }});
  },{threshold:.12});
  document.querySelectorAll('.reveal').forEach(el=>rev.observe(el));

  // ---------- 标题打字机 ----------
  const role = ['全栈工程师','AI 应用开发','数据工程','终身学习者'];
  const roleEl = document.querySelector('.role .tag:first-child');
  if(roleEl){
    let ri=0, ci=0, del=false;
    (function type(){
      const cur = role[ri%role.length];
      roleEl.textContent = cur.slice(0, ci) + '▌';
      if(!del && ci<cur.length){ ci++; setTimeout(type,110); }
      else if(!del){ del=true; setTimeout(type,1400); }
      else if(ci>0){ ci--; setTimeout(type,45); }
      else{ del=false; ri++; setTimeout(type,250); }
    })();
  }
</script>
</body>
</html>

