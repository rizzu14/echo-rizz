
<div class="page">

  <!-- Header -->
  <header class="site-header">
    <div class="badge">ECHO OS CENTER </div>
    <h1>ECHO <span>Command</span> Center</h1>
    <p class="tagline">Detects cost leakage, proposes optimizations, simulates them, and executes them — with full human oversight and governance controls.</p>
  </header>

  <!-- TOC -->
  <nav class="toc">
    <div class="toc-title">// Table of Contents</div>
    <ol>
      <li><a href="#what" data-num="01">What is ECHO?</a></li>
      <li><a href="#arch" data-num="02">Architecture Overview</a></li>
      <li><a href="#prereqs" data-num="03">Prerequisites</a></li>
      <li><a href="#step1" data-num="04">Clone the Repository</a></li>
      <li><a href="#step2" data-num="05">Start Infrastructure</a></li>
      <li><a href="#step3" data-num="06">Install Dependencies</a></li>
      <li><a href="#step4" data-num="07">Environment Variables</a></li>
      <li><a href="#step5" data-num="08">Run the Backend</a></li>
      <li><a href="#step6" data-num="09">Run the Frontend</a></li>
      <li><a href="#step7" data-num="10">Run Tests</a></li>
      <li><a href="#pages" data-num="11">Dashboard Pages</a></li>
      <li><a href="#api" data-num="12">REST API Reference</a></li>
      <li><a href="#deploy" data-num="13">Deployment</a></li>
      <li><a href="#stack" data-num="14">Tech Stack</a></li>
      <li><a href="#structure" data-num="15">Project Structure</a></li>
    </ol>
  </nav>

  <!-- What is ECHO -->
  <section class="section" id="what">
    <h2>What is ECHO?</h2>
    <p>ECHO is a multi-agent SaaS platform built for enterprises that want to stop bleeding money on cloud infrastructure, vendor contracts, and operational inefficiencies. It runs a network of specialized AI agents that continuously monitor your operations, surface anomalies, and take autonomous action — within the boundaries you define.</p>
    <div class="caps-grid">
      <div class="cap-card"><div class="icon">🔍</div><strong>Cost Leakage Detection</strong><p>Real-time detection across cloud resources</p></div>
      <div class="cap-card"><div class="icon">⚙️</div><strong>Autonomous Optimization</strong><p>Simulation before execution for every action</p></div>
      <div class="cap-card"><div class="icon">🧑‍💼</div><strong>Human-in-the-Loop</strong><p>Approval workflow for high-risk actions</p></div>
      <div class="cap-card"><div class="icon">🔒</div><strong>Full Governance Controls</strong><p>Spend limits, delegation rules, and kill switch</p></div>
      <div class="cap-card"><div class="icon">🤝</div><strong>A2A Coordination</strong><p>Agent-to-agent messaging with full transparency</p></div>
      <div class="cap-card"><div class="icon">📊</div><strong>ROI & Carbon Tracking</strong><p>Savings monitoring and compliance reporting</p></div>
    </div>
  </section>

  <hr/>

  <!-- Architecture -->
  <section class="section" id="arch">
    <h2>Architecture Overview</h2>
    <div class="arch-block">
      <pre><code>┌─────────────────────────────────────────────────────────┐
│                  ECHO Command Center                     │
│              React + TypeScript + Vite                   │
│         (Dashboard, Pipeline, Governance, Network)       │
└────────────────────┬────────────────────────────────────┘
                     │ WebSocket + REST
┌────────────────────▼────────────────────────────────────┐
│               Backend Server (Express + WS)              │
│         Node.js · TypeScript · Port 8080                 │
└──────┬──────────────┬──────────────┬────────────────────┘
       │              │              │
  ┌────▼────┐   ┌─────▼─────┐  ┌────▼────────┐
  │  Agent  │   │ Ingestion  │  │Infrastructure│
  │   OS    │   │  Pipeline  │  │Kafka · Redis │
  │Auditor  │   │Connectors  │  │Cassandra     │
  │Governor │   │Normalizer  │  └─────────────┘
  │Finance  │   └───────────┘
  │Green    │
  │SLA      │
  └─────────┘</code></pre>
    </div>
    <h3>Packages</h3>
    <div class="table-wrap"><table>
      <thead><tr><th>Package</th><th>Path</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td><code>echo-command-center</code></td><td><code>src/web/command-center</code></td><td>React frontend dashboard</td></tr>
        <tr><td><code>echo-backend</code></td><td><code>src/backend</code></td><td>Express REST API + WebSocket server</td></tr>
        <tr><td><code>echo-agents</code></td><td><code>src/agents</code></td><td>AI agent OS (Orchestrator, Auditor, Governor, Finance, Green, SLA)</td></tr>
        <tr><td><code>@echo/ingestion</code></td><td><code>src/ingestion</code></td><td>Data ingestion layer — cloud connectors, normalization pipeline</td></tr>
      </tbody>
    </table></div>
  </section>

  <hr/>

  <!-- Prerequisites -->
  <section class="section" id="prereqs">
    <h2>Prerequisites</h2>
    <p>Make sure you have the following installed before you begin:</p>
    <div class="table-wrap"><table>
      <thead><tr><th>Tool</th><th>Version</th><th>Purpose</th></tr></thead>
      <tbody>
        <tr><td><code>Node.js</code></td><td>v20+</td><td>JavaScript runtime</td></tr>
        <tr><td><code>npm</code></td><td>v9+</td><td>Package manager</td></tr>
        <tr><td><code>Docker</code></td><td>Latest</td><td>Kafka and Redis containers</td></tr>
        <tr><td><code>Git</code></td><td>Latest</td><td>Clone the repository</td></tr>
      </tbody>
    </table></div>
    <p>Verify your versions:</p>
    <pre><code>node --version   # should be v20+
npm --version    # should be v9+
docker --version</code></pre>
  </section>

  <hr/>

  <!-- Step 1 -->
  <section class="section" id="step1">
    <h2><span class="step-num">1</span> Clone the Repository</h2>
    <pre><code>git clone https://github.com/your-org/echo-command-center.git
cd echo-command-center</code></pre>
  </section>

  <hr/>

  <!-- Step 2 -->
  <section class="section" id="step2">
    <h2><span class="step-num">2</span> Start Infrastructure (Kafka + Redis)</h2>
    <p>ECHO uses a 3-broker Kafka cluster and a 6-node Redis cluster for event streaming and caching.</p>

    <h3>Start Kafka</h3>
    <pre><code>docker compose -f docker-compose.kafka.yml up -d</code></pre>
    <p>Wait for all brokers to be healthy (~30 seconds), then verify:</p>
    <pre><code>docker ps --filter "name=echo-broker"</code></pre>
    <p>All three brokers (<code>echo-broker-1</code>, <code>echo-broker-2</code>, <code>echo-broker-3</code>) should show <code>healthy</code>.</p>

    <h3>Start Redis</h3>
    <pre><code>docker compose -f docker-compose.redis.yml up -d</code></pre>
    <p>Then initialize the Redis cluster (<strong>run this once only</strong>):</p>
    <pre><code>docker exec -it redis-node-1 redis-cli --cluster create \
  127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 \
  127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 \
  --cluster-replicas 1 --cluster-yes</code></pre>
  </section>

  <hr/>

  <!-- Step 3 -->
  <section class="section" id="step3">
    <h2><span class="step-num">3</span> Install Dependencies</h2>
    <p>Each package has its own <code>node_modules</code>. Install them all from the project root:</p>
    <pre><code># Backend
cd src/backend && npm install && cd ../..

# Agents
cd src/agents && npm install && cd ../..

# Ingestion
cd src/ingestion && npm install && cd ../..

# Frontend
cd src/web/command-center && npm install && cd ../..</code></pre>
  </section>

  <hr/>

  <!-- Step 4 -->
  <section class="section" id="step4">
    <h2><span class="step-num">4</span> Environment Variables</h2>
    <h3>Backend — <code>src/backend/.env</code></h3>
    <pre><code>PORT=8080
NODE_ENV=development</code></pre>
    <h3>Frontend — <code>src/web/command-center/.env.local</code></h3>
    <pre><code>VITE_WS_URL=ws://localhost:8080/ws
VITE_API_URL=http://localhost:8080</code></pre>
    <div class="callout"><strong>Note:</strong> The frontend <code>.env.production.local</code> is already configured for the deployed environment. Only create <code>.env.local</code> for local development.</div>
  </section>

  <hr/>

  <!-- Step 5 -->
  <section class="section" id="step5">
    <h2><span class="step-num">5</span> Run the Backend</h2>
    <pre><code>cd src/backend
npm run dev</code></pre>
    <p>Expected output:</p>
    <pre><code>ECHO Backend running on http://localhost:8080
WebSocket: ws://localhost:8080/ws
Frontend:  http://localhost:3000</code></pre>
    <div class="callout"><strong>Note:</strong> The backend starts in <strong>mock-only mode</strong> if the full engine isn't loaded — this is fine for local development. The dashboard will still receive live simulated data.</div>
  </section>

  <hr/>

  <!-- Step 6 -->
  <section class="section" id="step6">
    <h2><span class="step-num">6</span> Run the Frontend</h2>
    <p>Open a <strong>new terminal</strong> and run:</p>
    <pre><code>cd src/web/command-center
npm run dev</code></pre>
    <p>Then open your browser at:</p>
    <pre><code>http://localhost:5173</code></pre>
    <p>The dashboard will connect to the backend via WebSocket and start receiving live data immediately.</p>
  </section>

  <hr/>

  <!-- Step 7 -->
  <section class="section" id="step7">
    <h2><span class="step-num">7</span> Run Tests</h2>
    <h3>Agent Tests</h3>
    <pre><code>cd src/agents
npm run test:all</code></pre>
    <h3>Ingestion Tests</h3>
    <pre><code>cd src/ingestion
npm run test:run</code></pre>
  </section>

  <hr/>

  <!-- Dashboard Pages -->
  <section class="section" id="pages">
    <h2>Dashboard Pages</h2>
    <div class="table-wrap"><table>
      <thead><tr><th>Page</th><th>Route</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td><strong>Mission Control</strong></td><td><code>/</code></td><td>Live KPIs, ROI summary, cost leakage stream, agent health</td></tr>
        <tr><td><strong>Action Pipeline</strong></td><td><code>/pipeline</code></td><td>All optimization actions — proposed, simulated, pending, executed</td></tr>
        <tr><td><strong>Governance</strong></td><td><code>/governance</code></td><td>Kill switch, spend limits, policy editor, compliance report</td></tr>
        <tr><td><strong>Agent Network</strong></td><td><code>/network</code></td><td>Live A2A communication graph and message inspector</td></tr>
        <tr><td><strong>Agent Intelligence</strong></td><td><code>/intelligence</code></td><td>Per-agent diagnostics, reasoning accuracy, model performance</td></tr>
      </tbody>
    </table></div>
  </section>

  <hr/>

  <!-- REST API -->
  <section class="section" id="api">
    <h2>REST API Reference</h2>
    <p>The backend exposes the following endpoints on <code>http://localhost:8080</code>:</p>
    <div class="table-wrap"><table>
      <thead><tr><th>Method</th><th>Endpoint</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td><code>GET</code></td><td><code>/api/health</code></td><td>Server health check</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/agents</code></td><td>List all agents</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/actions</code></td><td>List all pipeline actions</td></tr>
        <tr><td><code>PATCH</code></td><td><code>/api/actions/:id/approve</code></td><td>Approve an action</td></tr>
        <tr><td><code>PATCH</code></td><td><code>/api/actions/:id/reject</code></td><td>Reject an action</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/roi</code></td><td>Current ROI summary</td></tr>
        <tr><td><code>POST</code></td><td><code>/api/roi/calculate</code></td><td>Calculate ROI from inputs</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/cost-leakage</code></td><td>Cost leakage events</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/carbon</code></td><td>Carbon savings summary</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/governance/report</code></td><td>Compliance report</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/governance/dow</code></td><td>Delegation of Work config</td></tr>
        <tr><td><code>PATCH</code></td><td><code>/api/governance/dow</code></td><td>Update DoW config</td></tr>
        <tr><td><code>POST</code></td><td><code>/api/governance/kill-switch/activate</code></td><td>Activate kill switch</td></tr>
        <tr><td><code>POST</code></td><td><code>/api/governance/kill-switch/deactivate</code></td><td>Deactivate kill switch</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/a2a-messages</code></td><td>Agent-to-agent messages</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/ledger</code></td><td>Audit ledger entries</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/ledger/integrity</code></td><td>Verify ledger integrity</td></tr>
        <tr><td><code>POST</code></td><td><code>/api/anomaly/detect</code></td><td>Run anomaly detection</td></tr>
        <tr><td><code>POST</code></td><td><code>/api/reasoning/route</code></td><td>Route a reasoning request</td></tr>
        <tr><td><code>GET</code></td><td><code>/api/engine/status</code></td><td>Engine status</td></tr>
      </tbody>
    </table></div>

    <h3>WebSocket</h3>
    <p>Connects at <code>ws://localhost:8080/ws?tenant_id=tenant-acme</code></p>
    <p>On connection, the server sends an <code>INITIAL_SNAPSHOT</code> with all current state. Live events are then pushed as they occur:</p>
    <div class="table-wrap"><table>
      <thead><tr><th>Event</th><th>Frequency</th><th>Description</th></tr></thead>
      <tbody>
        <tr><td><code>COST_LEAKAGE_EVENT</code></td><td>~every 8s</td><td>New anomaly detected</td></tr>
        <tr><td><code>A2A_MESSAGE</code></td><td>~every 5s</td><td>Agent-to-agent message</td></tr>
        <tr><td><code>AGENT_HEALTH_UPDATE</code></td><td>~every 10s</td><td>Agent health change</td></tr>
        <tr><td><code>ROI_UPDATE</code></td><td>~every 30s</td><td>ROI recalculation</td></tr>
        <tr><td><code>ACTION_UPDATE</code></td><td>On change</td><td>Action state changed</td></tr>
        <tr><td><code>KILL_SWITCH_EVENT</code></td><td>On trigger</td><td>Kill switch activated/deactivated</td></tr>
      </tbody>
    </table></div>
  </section>

  <hr/>

  <!-- Deployment -->
  <section class="section" id="deploy">
    <h2>Deployment</h2>
    <p>The backend is configured for <strong>Render</strong> via <code>render.yaml</code>, and the frontend for <strong>Vercel</strong> via <code>vercel.json</code> in <code>src/web/command-center</code>.</p>

    <h3>Backend (Render)</h3>
    <p>Render will automatically run:</p>
    <pre><code>npm install && npm run build
npm start</code></pre>
    <p>Set these environment variables in your Render dashboard:</p>
    <pre><code>NODE_ENV=production
PORT=10000</code></pre>

    <h3>Frontend (Vercel)</h3>
    <p>Set these environment variables in your Vercel dashboard:</p>
    <pre><code>VITE_WS_URL=wss://your-backend.onrender.com/ws
VITE_API_URL=https://your-backend.onrender.com</code></pre>
  </section>

  <hr/>

  <!-- Tech Stack -->
  <section class="section" id="stack">
    <h2>Tech Stack</h2>
    <div class="table-wrap"><table>
      <thead><tr><th>Layer</th><th>Technology</th></tr></thead>
      <tbody>
        <tr><td><strong>Frontend</strong></td><td>React 18, TypeScript, Vite, Zustand, Recharts, D3, React Router</td></tr>
        <tr><td><strong>Backend</strong></td><td>Node.js, Express, WebSocket (ws), TypeScript</td></tr>
        <tr><td><strong>Agents</strong></td><td>TypeScript, custom Agent OS with orchestrator</td></tr>
        <tr><td><strong>Ingestion</strong></td><td>KafkaJS, TypeScript</td></tr>
        <tr><td><strong>Streaming</strong></td><td>Apache Kafka (3-broker cluster, Confluent 7.5)</td></tr>
        <tr><td><strong>Caching</strong></td><td>Redis 7.2 (6-node cluster)</td></tr>
        <tr><td><strong>Styling</strong></td><td>Pure CSS, CSS custom properties (no Tailwind)</td></tr>
      </tbody>
    </table></div>
  </section>

  <hr/>

  <!-- Project Structure -->
  <section class="section" id="structure">
    <h2>Project Structure</h2>
    <pre><code>.
├── docker-compose.kafka.yml       # 3-broker Kafka cluster
├── docker-compose.redis.yml       # 6-node Redis cluster
├── render.yaml                    # Backend deployment config
├── vercel.json                    # Frontend deployment config
└── src/
    ├── agents/                    # AI agent OS
    │   ├── orchestrator/          # Event routing + A2A coordination
    │   ├── auditor/               # Cost leakage detection
    │   ├── governor/              # Governance + policy enforcement
    │   ├── finance/               # Financial modeling
    │   ├── green/                 # Carbon optimization
    │   └── sla/                   # SLA contract management
    ├── backend/                   # Express + WebSocket server
    │   └── src/
    │       ├── server.ts          # Main server, REST API, WS
    │       ├── engine.ts          # Core ECHO engine
    │       └── mockData.ts        # Simulated data for dev
    ├── execution/                 # Plan cache, approval router, rollback
    ├── ingestion/                 # Cloud connectors, normalization pipeline
    ├── infrastructure/            # Kafka, Redis, Cassandra, Kubernetes configs
    ├── reasoning/                 # Reasoning engine
    ├── security/                  # API key management
    └── web/
        └── command-center/        # React dashboard
            └── src/
                ├── views/         # Dashboard, Pipeline, Governance, Network, Intelligence
                ├── components/    # Shared UI components
                ├── store/         # Zustand state management
                ├── hooks/         # Custom React hooks
                └── styles/        # CSS tokens and global styles</code></pre>
  </section>

  <hr/>

  <footer style="text-align:center; color: var(--text-dim); font-family: 'JetBrains Mono', monospace; font-size: 12px; padding: 20px 0;">
    MIT License · ECHO Command Center
  </footer>

</div>
</body>
</html>

<img width="246" height="67" alt="Screenshot 2026-03-29 194954" src="https://github.com/user-attachments/assets/62a3ff2b-9805-4336-9b99-504de58f127d" />

                                             ECHO OS CENTER LINK :     
           
           
  https://command-center-ten-ashen.vercel.app/                                      
                                       
                                          
                                            LOGIN PAGE OF OUR SITE
<img width="1689" height="917" alt="Screenshot 2026-03-29 195717" src="https://github.com/user-attachments/assets/e1117d8c-1557-4063-8648-d0143b17a7bc" />
                                                    
                                                    HOME PAGE 
<img width="1911" height="974" alt="Screenshot 2026-03-29 195828" src="https://github.com/user-attachments/assets/66202a82-164c-4da9-9af3-ab24603590cc" />
                                                    
                                                      DASHBOARD
  <img width="1918" height="977" alt="Screenshot 2026-03-29 195912" src="https://github.com/user-attachments/assets/cd984f61-25fe-45ee-b5ab-7ea5a1ef82dc" />
                                                       
                                                       AGENTS
  <img width="1915" height="891" alt="Screenshot 2026-03-29 200049" src="https://github.com/user-attachments/assets/69536d26-163f-4ad8-a1a7-ad013c492e00" />

                                                    
