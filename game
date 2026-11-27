<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Joint Control Ragdoll Boxing</title>
<style>
  * {
    box-sizing: border-box;
    user-select: none;
    -webkit-user-select: none;
    touch-action: none;
  }
  body {
    margin: 0;
    padding: 0;
    background-color: #11131a;
    color: #fff;
    font-family: 'Segoe UI', Roboto, 'Hiragino Sans', 'Meiryo', sans-serif;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    height: 100vh;
  }
  #hud {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 8px 16px;
    background: linear-gradient(180deg, rgba(16,18,28,0.95) 0%, rgba(16,18,28,0.7) 100%);
    border-bottom: 1px solid rgba(255,255,255,0.1);
    z-index: 10;
    gap: 12px;
  }
  .fighter-hud {
    flex: 1;
    max-width: 340px;
  }
  .fighter-name {
    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.05em;
    margin-bottom: 4px;
    display: flex;
    justify-content: space-between;
  }
  .bar-bg {
    width: 100%;
    height: 12px;
    background: #25293a;
    border-radius: 6px;
    overflow: hidden;
    border: 1px solid rgba(255,255,255,0.15);
    margin-bottom: 4px;
  }
  .hp-fill {
    height: 100%;
    transition: width 0.1s ease-out;
  }
  #p1-hp { background: linear-gradient(90deg, #2563eb, #38bdf8); width: 100%; }
  #p2-hp { background: linear-gradient(90deg, #f43f5e, #fb7185); width: 100%; float: right; }
  .stamina-bg {
    width: 100%;
    height: 6px;
    background: #1e2230;
    border-radius: 3px;
    overflow: hidden;
  }
  #p1-stamina { background: #facc15; height: 100%; width: 100%; }
  #p2-stamina { background: #facc15; height: 100%; width: 100%; float: right; }

  #center-controls {
    display: flex;
    align-items: center;
    gap: 8px;
    flex-wrap: wrap;
    justify-content: center;
  }
  .btn {
    background: #262b3d;
    color: #e2e8f0;
    border: 1px solid rgba(255,255,255,0.2);
    border-radius: 6px;
    padding: 6px 10px;
    font-size: 12px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.15s;
  }
  .btn:hover { background: #333a52; }
  .btn.active {
    background: #0284c7;
    border-color: #38bdf8;
    color: #fff;
    box-shadow: 0 0 10px rgba(56,189,248,0.5);
  }
  select.btn {
    outline: none;
  }

  #canvas-container {
    position: relative;
    flex: 1;
    width: 100%;
    overflow: hidden;
  }
  canvas {
    display: block;
    width: 100%;
    height: 100%;
  }

  #command-panel {
    position: absolute;
    top: 10px;
    left: 12px;
    background: rgba(15, 20, 32, 0.85);
    border: 1px solid rgba(56, 189, 248, 0.35);
    border-radius: 8px;
    padding: 8px 10px;
    font-size: 11px;
    z-index: 5;
    backdrop-filter: blur(4px);
    max-width: 260px;
  }
  #command-panel-header {
    font-weight: 700;
    color: #38bdf8;
    margin-bottom: 6px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .cmd-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 4px 6px;
    margin-bottom: 4px;
    background: rgba(255,255,255,0.06);
    border-radius: 4px;
    cursor: pointer;
    border: 1px solid transparent;
  }
  .cmd-item:hover {
    background: rgba(56, 189, 248, 0.2);
    border-color: rgba(56, 189, 248, 0.5);
  }
  .cmd-keys {
    font-family: monospace;
    background: #1e293b;
    padding: 2px 5px;
    border-radius: 3px;
    color: #facc15;
    font-weight: bold;
  }
  #input-history {
    margin-top: 6px;
    padding-top: 4px;
    border-top: 1px solid rgba(255,255,255,0.12);
    color: #94a3b8;
    font-family: monospace;
    min-height: 18px;
    font-size: 12px;
  }

  #banner {
    position: absolute;
    top: 38%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 42px;
    font-weight: 900;
    letter-spacing: 0.08em;
    text-shadow: 0 4px 20px rgba(0,0,0,0.9);
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.2s, transform 0.2s;
    text-align: center;
    z-index: 20;
  }
  #banner.show {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1.05);
  }

  #controls-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 8px 14px;
    background: #161924;
    border-top: 1px solid rgba(255,255,255,0.1);
    gap: 10px;
    flex-wrap: wrap;
  }
  .pad-group {
    display: flex;
    gap: 6px;
    align-items: center;
  }
  .pad-btn {
    background: #232838;
    border: 1px solid rgba(255,255,255,0.2);
    color: #f8fafc;
    border-radius: 8px;
    min-width: 44px;
    height: 40px;
    padding: 0 10px;
    font-size: 13px;
    font-weight: 700;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .pad-btn:active, .pad-btn.pressed {
    background: #3b82f6;
    border-color: #93c5fd;
    transform: translateY(1px);
  }
  .atk-btn { background: #312338; border-color: rgba(244,63,94,0.4); }
  .atk-btn:active, .atk-btn.pressed { background: #e11d48; }
  .hint-text {
    font-size: 11px;
    color: #94a3b8;
  }
</style>
</head>
<body>

<div id="hud">
  <div class="fighter-hud">
    <div class="fighter-name">
      <span style="color:#38bdf8;">PLAYER (BLUE)</span>
      <span id="p1-hp-text">100%</span>
    </div>
    <div class="bar-bg"><div id="p1-hp" class="hp-fill"></div></div>
    <div class="stamina-bg"><div id="p1-stamina"></div></div>
  </div>

  <div id="center-controls">
    <button id="slowmo-btn" class="btn">スロー予約 (Space)</button>
    <button id="clear-pins-btn" class="btn">ピン解除</button>
    <select id="cpu-level" class="btn">
      <option value="0">CPU: 練習(サンドバッグ)</option>
      <option value="1" selected>CPU: 初級</option>
      <option value="2">CPU: 中級</option>
      <option value="3">CPU: 上級</option>
    </select>
    <button id="reset-btn" class="btn">リセット</button>
  </div>

  <div class="fighter-hud">
    <div class="fighter-name">
      <span id="p2-hp-text">100%</span>
      <span style="color:#fb7185;">CPU (RED)</span>
    </div>
    <div class="bar-bg"><div id="p2-hp" class="hp-fill"></div></div>
    <div class="stamina-bg"><div id="p2-stamina"></div></div>
  </div>
</div>

<div id="canvas-container">
  <canvas id="gameCanvas"></canvas>

  <div id="command-panel">
    <div id="command-panel-header">
      <span>必殺コマンド (上下左右のみ)</span>
      <span style="font-size:10px;color:#94a3b8;">手動で威力1.2x</span>
    </div>
    <div class="cmd-item" data-cmd="mach">
      <span>マッハ連打</span>
      <span class="cmd-keys">↓ → + Z</span>
    </div>
    <div class="cmd-item" data-cmd="upper">
      <span>ライジングアッパー</span>
      <span class="cmd-keys">→ ↓ + X</span>
    </div>
    <div class="cmd-item" data-cmd="straight">
      <span>トルネードストレート</span>
      <span class="cmd-keys">← → + X</span>
    </div>
    <div class="cmd-item" data-cmd="hook">
      <span>ジャンピングフック</span>
      <span class="cmd-keys">↓ ← + X</span>
    </div>
    <div id="input-history">入力: -</div>
  </div>

  <div id="banner">KO!</div>
</div>

<div id="controls-bar">
  <div class="pad-group">
    <button class="pad-btn" data-key="ArrowLeft">← (A)</button>
    <button class="pad-btn" data-key="ArrowDown">↓ 屈み(S)</button>
    <button class="pad-btn" data-key="ArrowUp">↑ 跳(W)</button>
    <button class="pad-btn" data-key="ArrowRight">→ (D)</button>
  </div>

  <div class="hint-text">
    関節を直接ドラッグ操作可能 / スロー中に複数関節をドラッグして予約コンボ
  </div>

  <div class="pad-group">
    <button class="pad-btn atk-btn" data-key="z">ジャブ (Z)</button>
    <button class="pad-btn atk-btn" data-key="x">ストレート (X)</button>
    <button class="pad-btn atk-btn" data-key="c">アッパー (C)</button>
    <button class="pad-btn" data-key="v">ガード (V)</button>
  </div>
</div>

<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
const container = document.getElementById('canvas-container');

let W = 900, H = 500, FLOOR_Y = 430;
function resize() {
  W = container.clientWidth;
  H = container.clientHeight;
  canvas.width = W;
  canvas.height = H;
  FLOOR_Y = H - 55;
}
window.addEventListener('resize', resize);
resize();

// 物理・ゲーム定数
const GRAVITY = 0.52;
const FRICTION = 0.985;
const GROUND_FRICTION = 0.82;
const ITERATIONS = 8;

// 状態変数
let slowMo = false;
let timeScale = 1.0;
let gameOver = false;
let hitStop = 0;
let screenShake = 0;
let particles = [];
let floatingTexts = [];
let keys = {};
let inputBuffer = [];
const BUFFER_MAX_AGE = 1200; // ms

const bannerEl = document.getElementById('banner');
function showBanner(text, color = '#facc15', duration = 1200) {
  bannerEl.textContent = text;
  bannerEl.style.color = color;
  bannerEl.classList.add('show');
  if (duration > 0) {
    setTimeout(() => bannerEl.classList.remove('show'), duration);
  }
}

// 関節ポイントクラス (Verlet Integration)
class Point {
  constructor(x, y, radius = 7, mass = 1, name = '') {
    this.x = x;
    this.y = y;
    this.oldx = x;
    this.oldy = y;
    this.radius = radius;
    this.mass = mass;
    this.name = name;
    this.pin = null; // {x, y} スロー予約目標
  }

  update(dtScale) {
    const vx = (this.x - this.oldx) * FRICTION;
    const vy = (this.y - this.oldy) * FRICTION;
    this.oldx = this.x;
    this.oldy = this.y;

    this.x += vx * dtScale;
    this.y += (vy + GRAVITY * dtScale) * dtScale;

    // 床・壁の衝突判定
    if (this.y + this.radius > FLOOR_Y) {
      this.y = FLOOR_Y - this.radius;
      const groundVx = (this.x - this.oldx) * GROUND_FRICTION;
      this.oldx = this.x - groundVx;
    }
    if (this.y - this.radius < 20) {
      this.y = 20 + this.radius;
    }
    const margin = 45;
    if (this.x - this.radius < margin) {
      this.x = margin + this.radius;
    }
    if (this.x + this.radius > W - margin) {
      this.x = W - margin - this.radius;
    }
  }
}

// ボーン拘束クラス
class Stick {
  constructor(p0, p1, length, stiffness = 0.95, visible = true, thickness = 8, color = null) {
    this.p0 = p0;
    this.p1 = p1;
    this.length = length;
    this.stiffness = stiffness;
    this.visible = visible;
    this.thickness = thickness;
    this.color = color;
  }

  resolve() {
    const dx = this.p1.x - this.p0.x;
    const dy = this.p1.y - this.p0.y;
    const dist = Math.hypot(dx, dy) || 0.001;
    const diff = (this.length - dist) / dist * this.stiffness;
    const totalMass = this.p0.mass + this.p1.mass;
    const m0 = this.p1.mass / totalMass;
    const m1 = this.p0.mass / totalMass;

    const offsetX = dx * diff;
    const offsetY = dy * diff;

    this.p0.x -= offsetX * m0;
    this.p0.y -= offsetY * m0;
    this.p1.x += offsetX * m1;
    this.p1.y += offsetY * m1;
  }
}

// ボクサー生成クラス
class Boxer {
  constructor(x, y, facing, isPlayer) {
    this.facing = facing; // 1: 右向き, -1: 左向き
    this.isPlayer = isPlayer;
    this.hp = 100;
    this.maxHp = 100;
    this.stamina = 100;
    this.colorMain = isPlayer ? '#38bdf8' : '#fb7185';
    this.colorDark = isPlayer ? '#1d4ed8' : '#be123c';
    this.gloveColor = isPlayer ? '#2563eb' : '#e11d48';

    this.crouching = false;
    this.guarding = false;
    this.attackState = null; // { type, timer, duration, hand, damage, extended, special }
    this.stunTimer = 0;
    this.aiTimer = 0;

    this.initBody(x, y);
  }

  initBody(x, y) {
    // 関節定義 (14関節)
    this.head = new Point(x, y - 135, 16, 1.3, 'head');
    this.neck = new Point(x, y - 112, 7, 1.2, 'neck');
    this.hip = new Point(x, y - 62, 10, 1.8, 'hip');

    // 前側（リード）と奥側（リア）の腕・脚
    this.shoulderF = new Point(x + 8 * this.facing, y - 108, 7, 1.1, 'shoulderF');
    this.elbowF = new Point(x + 22 * this.facing, y - 82, 7, 0.9, 'elbowF');
    this.handF = new Point(x + 36 * this.facing, y - 106, 11, 0.85, 'handF');

    this.shoulderB = new Point(x - 8 * this.facing, y - 108, 7, 1.1, 'shoulderB');
    this.elbowB = new Point(x + 6 * this.facing, y - 80, 7, 0.9, 'elbowB');
    this.handB = new Point(x + 22 * this.facing, y - 108, 11, 0.85, 'handB');

    this.kneeF = new Point(x + 16 * this.facing, y - 32, 8, 1.1, 'kneeF');
    this.footF = new Point(x + 22 * this.facing, y, 9, 1.5, 'footF');

    this.kneeB = new Point(x - 14 * this.facing, y - 32, 8, 1.1, 'kneeB');
    this.footB = new Point(x - 20 * this.facing, y, 9, 1.5, 'footB');

    this.points = [
      this.head, this.neck, this.hip,
      this.shoulderF, this.elbowF, this.handF,
      this.shoulderB, this.elbowB, this.handB,
      this.kneeF, this.footF,
      this.kneeB, this.footB
    ];

    // 骨格ボーン（十分な腕のリーチを確保）
    const upperArmLen = 36;
    const foreArmLen = 38;
    const thighLen = 34;
    const shinLen = 34;

    this.sticks = [
      new Stick(this.head, this.neck, 23, 0.98, true, 9),
      new Stick(this.neck, this.hip, 50, 0.98, true, 14),
      // 肩幅構造
      new Stick(this.neck, this.shoulderF, 12, 0.95, false),
      new Stick(this.neck, this.shoulderB, 12, 0.95, false),
      new Stick(this.hip, this.shoulderF, 48, 0.85, false),
      new Stick(this.hip, this.shoulderB, 48, 0.85, false),
      // 前腕・上腕
      new Stick(this.shoulderF, this.elbowF, upperArmLen, 0.98, true, 8),
      new Stick(this.elbowF, this.handF, foreArmLen, 0.98, true, 8),
      new Stick(this.shoulderB, this.elbowB, upperArmLen, 0.98, true, 8, this.colorDark),
      new Stick(this.elbowB, this.handB, foreArmLen, 0.98, true, 8, this.colorDark),
      // 脚
      new Stick(this.hip, this.kneeF, thighLen, 0.98, true, 10),
      new Stick(this.kneeF, this.footF, shinLen, 0.98, true, 9),
      new Stick(this.hip, this.kneeB, thighLen, 0.98, true, 10, this.colorDark),
      new Stick(this.kneeB, this.footB, shinLen, 0.98, true, 9, this.colorDark),
    ];
  }

  // 構えの屈曲とパンチの伸展を切り替える関節キネマティクス制御
  applyLimbKinematics(shoulder, elbow, hand, isPunching, isLead) {
    const dx = hand.x - shoulder.x;
    const dy = hand.y - shoulder.y;
    const dist = Math.hypot(dx, dy) || 1;
    const maxReach = 74; // upperArm(36) + foreArm(38)

    if (isPunching) {
      // 【パンチ発射時】曲げた腕から一気に直線的に伸びきる伸展スプリング
      const targetDist = Math.min(dist, maxReach);
      const midX = shoulder.x + (dx / dist) * (targetDist * 0.48);
      const midY = shoulder.y + (dy / dist) * (targetDist * 0.48) + 3; // わずかに下側へ自然な肘ライン
      elbow.x += (midX - elbow.x) * 0.45;
      elbow.y += (midY - elbow.y) * 0.45;
    } else {
      // 【構え・待機時】肘を曲げて脇を締め、拳を顔〜胸前に構える屈曲スプリング
      const bendOffsetX = -this.facing * (isLead ? 8 : 14);
      const bendOffsetY = 24;
      const targetElbowX = shoulder.x + bendOffsetX;
      const targetElbowY = shoulder.y + bendOffsetY;

      elbow.x += (targetElbowX - elbow.x) * 0.25;
      elbow.y += (targetElbowY - elbow.y) * 0.25;

      // 拳が顔の前で構えの位置をキープ
      const guardX = this.head.x + this.facing * (isLead ? 28 : 16);
      const guardY = this.head.y + (this.guarding ? -4 : 12);
      hand.x += (guardX - hand.x) * 0.22;
      hand.y += (guardY - hand.y) * 0.22;
    }
  }

  // しゃがみ時の前方膝屈曲と垂直重心維持
  applyLegKinematics(stanceCenterX, targetHipY) {
    // 膝が後ろに折れず常に進行方向(前側)へ曲がるようにバイアスを付与
    const kneeBendForward = this.crouching ? 24 : 10;
    const targetKneeFX = (this.hip.x + this.footF.x) * 0.5 + this.facing * kneeBendForward;
    const targetKneeBX = (this.hip.x + this.footB.x) * 0.5 + this.facing * (kneeBendForward * 0.85);

    this.kneeF.x += (targetKneeFX - this.kneeF.x) * 0.28;
    this.kneeB.x += (targetKneeBX - this.kneeB.x) * 0.28;

    // しゃがみ時に後ろへ下がらないよう、スタンス中心の真上に腰と頭を水平固定
    if (this.crouching) {
      const anchorX = stanceCenterX;
      this.hip.x += (anchorX - this.hip.x) * 0.35;
      this.neck.x += (anchorX + this.facing * 4 - this.neck.x) * 0.35;
      this.head.x += (anchorX + this.facing * 8 - this.head.x) * 0.35;
    }
  }

  updatePosture(opponent, dtScale) {
    if (this.hp <= 0) return;

    // 相手の方を向く（ダウン・スタン中以外）
    if (this.stunTimer <= 0 && !this.attackState) {
      this.facing = opponent.hip.x >= this.hip.x ? 1 : -1;
    }

    if (this.stunTimer > 0) {
      this.stunTimer -= dtScale;
      return;
    }

    // スタミナ自然回復
    this.stamina = Math.min(100, this.stamina + 0.25 * dtScale);

    // 足の接地位置とスタンス維持
    const stanceCenterX = (this.footF.x + this.footB.x) * 0.5;
    const footFTargetX = stanceCenterX + this.facing * 22;
    const footBTargetX = stanceCenterX - this.facing * 20;

    if (this.footF.y >= FLOOR_Y - 14) {
      this.footF.x += (footFTargetX - this.footF.x) * 0.15 * dtScale;
    }
    if (this.footB.y >= FLOOR_Y - 14) {
      this.footB.x += (footBTargetX - this.footB.x) * 0.15 * dtScale;
    }

    // 姿勢高さ制御（しゃがみ時は真下に沈み込む）
    const standHeight = this.crouching ? 36 : 64;
    const targetHipY = FLOOR_Y - standHeight;
    const targetNeckY = targetHipY - 48;
    const targetHeadY = targetNeckY - 23;

    this.hip.y += (targetHipY - this.hip.y) * 0.22 * dtScale;
    this.neck.y += (targetNeckY - this.neck.y) * 0.25 * dtScale;
    this.head.y += (targetHeadY - this.head.y) * 0.25 * dtScale;

    if (!this.crouching) {
      this.hip.x += (stanceCenterX - this.hip.x) * 0.16 * dtScale;
      this.neck.x += (stanceCenterX + this.facing * 2 - this.neck.x) * 0.18 * dtScale;
      this.head.x += (stanceCenterX + this.facing * 4 - this.head.x) * 0.18 * dtScale;
    }

    this.applyLegKinematics(stanceCenterX, targetHipY);

    // 攻撃モーション処理
    let punchF = false;
    let punchB = false;

    if (this.attackState) {
      const atk = this.attackState;
      atk.timer += dtScale;

      if (atk.type === 'jab') {
        punchF = true;
        const tx = this.shoulderF.x + this.facing * 74;
        const ty = this.shoulderF.y - 6;
        this.handF.x += (tx - this.handF.x) * 0.45;
        this.handF.y += (ty - this.handF.y) * 0.45;
      } else if (atk.type === 'straight') {
        punchB = true;
        // 腰と肩の踏み込み
        this.hip.x += this.facing * 1.8 * dtScale;
        const tx = this.shoulderB.x + this.facing * 78;
        const ty = this.shoulderB.y - 4;
        this.handB.x += (tx - this.handB.x) * 0.48;
        this.handB.y += (ty - this.handB.y) * 0.48;
      } else if (atk.type === 'upper') {
        punchB = true;
        const progress = atk.timer / atk.duration;
        const tx = this.shoulderB.x + this.facing * (progress < 0.4 ? 30 : 58);
        const ty = this.shoulderB.y + (progress < 0.4 ? 25 : -58);
        this.handB.x += (tx - this.handB.x) * 0.45;
        this.handB.y += (ty - this.handB.y) * 0.45;
        if (progress >= 0.4) this.hip.y -= 2.5 * dtScale;
      } else if (atk.type === 'sp_mach') {
        // マッハ連打
        const phase = Math.floor(atk.timer / 4) % 2;
        punchF = phase === 0;
        punchB = phase === 1;
        this.hip.x += this.facing * 2.2 * dtScale;
        if (punchF) {
          this.handF.x += (this.shoulderF.x + this.facing * 75 - this.handF.x) * 0.55;
          this.handF.y += (this.shoulderF.y - 10 + Math.sin(atk.timer) * 14 - this.handF.y) * 0.55;
        } else {
          this.handB.x += (this.shoulderB.x + this.facing * 75 - this.handB.x) * 0.55;
          this.handB.y += (this.shoulderB.y - 10 - Math.sin(atk.timer) * 14 - this.handB.y) * 0.55;
        }
      } else if (atk.type === 'sp_upper') {
        // ライジングアッパー
        punchB = true;
        this.hip.x += this.facing * 2.5 * dtScale;
        this.hip.y -= 4.5 * dtScale;
        const tx = this.shoulderB.x + this.facing * 52;
        const ty = this.shoulderB.y - 68;
        this.handB.x += (tx - this.handB.x) * 0.52;
        this.handB.y += (ty - this.handB.y) * 0.52;
      } else if (atk.type === 'sp_straight') {
        // トルネードストレート
        punchB = true;
        this.hip.x += this.facing * 4.2 * dtScale;
        const tx = this.shoulderB.x + this.facing * 80;
        const ty = this.shoulderB.y - 2;
        this.handB.x += (tx - this.handB.x) * 0.58;
        this.handB.y += (ty - this.handB.y) * 0.58;
      } else if (atk.type === 'sp_hook') {
        // ジャンピングフック
        punchF = true;
        this.hip.x += this.facing * 4.0 * dtScale;
        if (atk.timer < 10) this.hip.y -= 5.0 * dtScale;
        const tx = this.shoulderF.x + this.facing * 72;
        const ty = this.shoulderF.y - 14;
        this.handF.x += (tx - this.handF.x) * 0.5;
        this.handF.y += (ty - this.handF.y) * 0.5;
      }

      if (atk.timer >= atk.duration) {
        this.attackState = null;
      }
    }

    // 曲げた腕の構え or 伸展パンチのキネマティクス適用
    this.applyLimbKinematics(this.shoulderF, this.elbowF, this.handF, punchF, true);
    this.applyLimbKinematics(this.shoulderB, this.elbowB, this.handB, punchB, false);
  }

  move(dir, dtScale) {
    if (this.hp <= 0 || this.stunTimer > 0) return;
    const speed = this.crouching ? 1.8 : 3.4;
    this.footF.x += dir * speed * dtScale;
    this.footB.x += dir * speed * dtScale;
    this.hip.x += dir * speed * 0.9 * dtScale;
  }

  jump() {
    if (this.hp <= 0 || this.stunTimer > 0) return;
    if (this.footF.y >= FLOOR_Y - 16 || this.footB.y >= FLOOR_Y - 16) {
      for (const p of this.points) {
        p.oldy = p.y + 11.5;
      }
    }
  }

  startAttack(type, isManualCommand = false) {
    if (this.hp <= 0 || this.stunTimer > 0) return;
    if (this.attackState && !type.startsWith('sp_')) return;

    const specs = {
      jab:         { duration: 14, damage: 6,  cost: 8,  knockback: 5,  label: '' },
      straight:    { duration: 20, damage: 11, cost: 14, knockback: 9,  label: '' },
      upper:       { duration: 22, damage: 13, cost: 16, knockback: 11, label: '' },
      sp_mach:     { duration: 28, damage: 5,  cost: 24, knockback: 4,  label: 'マッハ連打！' },
      sp_upper:    { duration: 24, damage: 20, cost: 28, knockback: 16, label: 'ライジングアッパー！' },
      sp_straight: { duration: 24, damage: 22, cost: 28, knockback: 18, label: 'トルネードストレート！' },
      sp_hook:     { duration: 24, damage: 19, cost: 26, knockback: 15, label: 'ジャンピングフック！' }
    };

    const spec = specs[type];
    if (!spec || this.stamina < spec.cost) return;

    this.stamina -= spec.cost;
    const mult = isManualCommand ? 1.2 : 1.0;

    this.attackState = {
      type,
      timer: 0,
      duration: spec.duration,
      damage: Math.round(spec.damage * mult),
      knockback: spec.knockback * mult,
      hitCooldown: 0
    };

    if (spec.label) {
      addFloatingText(this.head.x, this.head.y - 30, spec.label + (isManualCommand ? ' (1.2x)' : ''), '#facc15');
    }
  }

  draw() {
    // 奥側の腕・脚を描画
    this.drawLimb(this.hip, this.kneeB, this.footB, 10, this.colorDark);
    this.drawLimb(this.shoulderB, this.elbowB, this.handB, 8, this.colorDark);

    // 胴体（トランクス＆上半身）
    ctx.strokeStyle = this.colorMain;
    ctx.lineWidth = 16;
    ctx.lineCap = 'round';
    ctx.beginPath();
    ctx.moveTo(this.neck.x, this.neck.y);
    ctx.lineTo(this.hip.x, this.hip.y);
    ctx.stroke();

    // トランクス（腰回り）
    ctx.fillStyle = this.isPlayer ? '#1e3a8a' : '#881337';
    ctx.beginPath();
    ctx.arc(this.hip.x, this.hip.y, 12, 0, Math.PI * 2);
    ctx.fill();

    // 頭部・ヘッドギア風ライン
    ctx.fillStyle = '#fde68a';
    ctx.beginPath();
    ctx.arc(this.head.x, this.head.y, this.head.radius, 0, Math.PI * 2);
    ctx.fill();
    ctx.lineWidth = 3;
    ctx.strokeStyle = this.colorMain;
    ctx.stroke();

    // 目（向いている方向）
    ctx.fillStyle = '#0f172a';
    ctx.beginPath();
    ctx.arc(this.head.x + this.facing * 7, this.head.y - 2, 2.5, 0, Math.PI * 2);
    ctx.fill();

    // 前側の脚・腕を描画
    this.drawLimb(this.hip, this.kneeF, this.footF, 11, this.colorMain);
    this.drawLimb(this.shoulderF, this.elbowF, this.handF, 9, this.colorMain);

    // ボクシンググローブ（両拳）
    this.drawGlove(this.handB, this.colorDark);
    this.drawGlove(this.handF, this.gloveColor);

    // ガードシールド表現
    if (this.guarding && this.hp > 0) {
      ctx.strokeStyle = 'rgba(56, 189, 248, 0.5)';
      ctx.lineWidth = 3;
      ctx.beginPath();
      ctx.arc(this.head.x + this.facing * 20, this.head.y + 8, 26, -0.8, 0.8);
      ctx.stroke();
    }

    // プレイヤー側の操作可能な関節ノードとスロー予約ピンを表示
    if (this.isPlayer) {
      for (const p of this.points) {
        ctx.fillStyle = p === draggedPoint ? '#facc15' : 'rgba(255,255,255,0.45)';
        ctx.beginPath();
        ctx.arc(p.x, p.y, 4, 0, Math.PI * 2);
        ctx.fill();

        if (p.pin) {
          ctx.strokeStyle = '#facc15';
          ctx.lineWidth = 2;
          ctx.setLineDash([4, 3]);
          ctx.beginPath();
          ctx.moveTo(p.x, p.y);
          ctx.lineTo(p.pin.x, p.pin.y);
          ctx.stroke();
          ctx.setLineDash([]);

          ctx.fillStyle = '#facc15';
          ctx.beginPath();
          ctx.arc(p.pin.x, p.pin.y, 6, 0, Math.PI * 2);
          ctx.fill();
        }
      }
    }
  }

  drawLimb(p0, p1, p2, width, color) {
    ctx.strokeStyle = color;
    ctx.lineWidth = width;
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
    ctx.beginPath();
    ctx.moveTo(p0.x, p0.y);
    ctx.lineTo(p1.x, p1.y);
    ctx.lineTo(p2.x, p2.y);
    ctx.stroke();

    // 関節ジョイントの強調
    ctx.fillStyle = '#ffffff';
    ctx.beginPath();
    ctx.arc(p1.x, p1.y, width * 0.32, 0, Math.PI * 2);
    ctx.fill();
  }

  drawGlove(hand, color) {
    ctx.fillStyle = color;
    ctx.beginPath();
    ctx.arc(hand.x, hand.y, hand.radius, 0, Math.PI * 2);
    ctx.fill();
    ctx.lineWidth = 2;
    ctx.strokeStyle = '#ffffff';
    ctx.stroke();
  }
}

let p1, p2;
function resetGame() {
  p1 = new Boxer(W * 0.32, FLOOR_Y, 1, true);
  p2 = new Boxer(W * 0.68, FLOOR_Y, -1, false);
  gameOver = false;
  slowMo = false;
  timeScale = 1.0;
  document.getElementById('slowmo-btn').classList.remove('active');
  bannerEl.classList.remove('show');
  updateHud();
}

function updateHud() {
  document.getElementById('p1-hp').style.width = Math.max(0, p1.hp) + '%';
  document.getElementById('p2-hp').style.width = Math.max(0, p2.hp) + '%';
  document.getElementById('p1-hp-text').textContent = Math.max(0, Math.round(p1.hp)) + '%';
  document.getElementById('p2-hp-text').textContent = Math.max(0, Math.round(p2.hp)) + '%';
  document.getElementById('p1-stamina').style.width = p1.stamina + '%';
  document.getElementById('p2-stamina').style.width = p2.stamina + '%';
}

function addParticles(x, y, color, count = 12) {
  for (let i = 0; i < count; i++) {
    const angle = Math.random() * Math.PI * 2;
    const speed = 2 + Math.random() * 6;
    particles.push({
      x, y,
      vx: Math.cos(angle) * speed,
      vy: Math.sin(angle) * speed - 2,
      life: 1.0,
      color
    });
  }
}

function addFloatingText(x, y, text, color = '#fff') {
  floatingTexts.push({ x, y, text, color, life: 1.0 });
}

// パンチ・物理打撃判定
function checkCombat(attacker, defender) {
  if (attacker.hp <= 0 || defender.hp <= 0) return;

  const hands = [attacker.handF, attacker.handB];
  const targets = [
    { pt: defender.head, mult: 1.35, name: 'CRITICAL!' },
    { pt: defender.neck, mult: 1.0,  name: '' },
    { pt: defender.hip,  mult: 0.85, name: 'BODY!' }
  ];

  for (const hand of hands) {
    const vx = hand.x - hand.oldx;
    const vy = hand.y - hand.oldy;
    const speed = Math.hypot(vx, vy);

    // 攻撃モーション中、またはプレイヤーによる高速ドラッグ打撃
    const isAttacking = attacker.attackState && attacker.attackState.hitCooldown <= 0;
    const isPhysicsPunch = attacker.isPlayer && speed > 11;

    if (!isAttacking && !isPhysicsPunch) continue;

    for (const t of targets) {
      const dist = Math.hypot(hand.x - t.pt.x, hand.y - t.pt.y);
      if (dist < hand.radius + t.pt.radius + 4) {
        let baseDmg = isAttacking ? attacker.attackState.damage : Math.min(18, speed * 0.85);
        let kb = isAttacking ? attacker.attackState.knockback : speed * 0.7;

        if (defender.guarding) {
          baseDmg *= 0.25;
          kb *= 0.35;
          addFloatingText(t.pt.x, t.pt.y - 20, 'GUARD', '#94a3b8');
        } else {
          if (t.name) addFloatingText(t.pt.x, t.pt.y - 24, t.name, '#facc15');
        }

        const finalDmg = Math.max(1, Math.round(baseDmg * t.mult));
        defender.hp = Math.max(0, defender.hp - finalDmg);

        // ノックバック適用
        const dir = attacker.facing;
        t.pt.oldx -= dir * kb;
        t.pt.oldy += 3.5;

        if (isAttacking) {
          attacker.attackState.hitCooldown = attacker.attackState.type === 'sp_mach' ? 5 : 16;
        } else {
          // 物理パンチの連続ヒット防止反発
          hand.oldx = hand.x + dir * 4;
        }

        addParticles(t.pt.x, t.pt.y, defender.guarding ? '#38bdf8' : '#facc15', 14);
        addFloatingText(t.pt.x + 10, t.pt.y - 10, `-${finalDmg}`, '#fb7185');
        hitStop = 3;
        screenShake = Math.min(12, finalDmg * 0.6);
        updateHud();

        if (defender.hp <= 0 && !gameOver) {
          gameOver = true;
          showBanner(defender.isPlayer ? 'YOU LOSE...' : 'K.O.!! YOU WIN!', defender.isPlayer ? '#fb7185' : '#38bdf8', 0);
        }
        return;
      }
    }
  }
}

// CPU AI制御
function updateAI(dtScale) {
  const level = parseInt(document.getElementById('cpu-level').value, 10);
  if (level === 0 || p2.hp <= 0 || p1.hp <= 0) return;

  p2.aiTimer += dtScale;
  const dist = Math.abs(p1.hip.x - p2.hip.x);
  const interval = level === 1 ? 45 : level === 2 ? 26 : 14;

  // 間合い調整
  if (dist > 115) {
    p2.move(p2.facing, dtScale * (0.6 + level * 0.2));
  } else if (dist < 65) {
    p2.move(-p2.facing, dtScale * 0.6);
  }

  if (p2.aiTimer >= interval) {
    p2.aiTimer = 0;
    p2.guarding = false;
    p2.crouching = false;

    const r = Math.random();
    if (p1.attackState && r < 0.25 * level) {
      if (Math.random() < 0.5) p2.guarding = true;
      else p2.crouching = true;
    } else if (dist <= 125) {
      if (r < 0.35) p2.startAttack('jab');
      else if (r < 0.65) p2.startAttack('straight');
      else if (r < 0.85) p2.startAttack('upper');
      else if (level >= 2) {
        const specials = ['sp_mach', 'sp_upper', 'sp_straight', 'sp_hook'];
        p2.startAttack(specials[Math.floor(Math.random() * specials.length)]);
      }
    }
  }
}

// コマンド入力履歴と判定（上下左右のみ）
function recordInput(symbol) {
  const now = performance.now();
  inputBuffer = inputBuffer.filter(item => now - item.time < BUFFER_MAX_AGE);
  inputBuffer.push({ sym: symbol, time: now });

  const seq = inputBuffer.map(i => i.sym).join(' ');
  document.getElementById('input-history').textContent = '入力: ' + seq;

  // 末尾一致判定
  if (seq.endsWith('↓ → Z')) {
    p1.startAttack('sp_mach', true);
    inputBuffer = [];
  } else if (seq.endsWith('→ ↓ X')) {
    p1.startAttack('sp_upper', true);
    inputBuffer = [];
  } else if (seq.endsWith('← → X')) {
    p1.startAttack('sp_straight', true);
    inputBuffer = [];
  } else if (seq.endsWith('↓ ← X')) {
    p1.startAttack('sp_hook', true);
    inputBuffer = [];
  }
}

// キーボードイベント
window.addEventListener('keydown', (e) => {
  if (e.repeat) return;
  const k = e.key.toLowerCase();
  keys[k] = true;
  keys[e.key] = true;

  if (e.code === 'Space') {
    e.preventDefault();
    toggleSlowMo();
    return;
  }

  if (k === 'w' || e.key === 'ArrowUp') {
    recordInput('↑');
    p1.jump();
  } else if (k === 's' || e.key === 'ArrowDown') {
    recordInput('↓');
  } else if (k === 'a' || e.key === 'ArrowLeft') {
    recordInput('←');
  } else if (k === 'd' || e.key === 'ArrowRight') {
    recordInput('→');
  } else if (k === 'z') {
    recordInput('Z');
    p1.startAttack('jab');
  } else if (k === 'x') {
    recordInput('X');
    p1.startAttack('straight');
  } else if (k === 'c') {
    recordInput('C');
    p1.startAttack('upper');
  }
});

window.addEventListener('keyup', (e) => {
  const k = e.key.toLowerCase();
  keys[k] = false;
  keys[e.key] = false;
});

// 画面ボタン操作
document.querySelectorAll('.pad-btn').forEach(btn => {
  const keyName = btn.dataset.key;
  const press = (e) => {
    e.preventDefault();
    btn.classList.add('pressed');
    keys[keyName] = true;
    if (keyName === 'ArrowUp') { recordInput('↑'); p1.jump(); }
    if (keyName === 'ArrowDown') recordInput('↓');
    if (keyName === 'ArrowLeft') recordInput('←');
    if (keyName === 'ArrowRight') recordInput('→');
    if (keyName === 'z') { recordInput('Z'); p1.startAttack('jab'); }
    if (keyName === 'x') { recordInput('X'); p1.startAttack('straight'); }
    if (keyName === 'c') { recordInput('C'); p1.startAttack('upper'); }
  };
  const release = (e) => {
    e.preventDefault();
    btn.classList.remove('pressed');
    keys[keyName] = false;
  };
  btn.addEventListener('mousedown', press);
  btn.addEventListener('mouseup', release);
  btn.addEventListener('mouseleave', release);
  btn.addEventListener('touchstart', press);
  btn.addEventListener('touchend', release);
});

// コマンド表のワンタップ発動
document.querySelectorAll('.cmd-item').forEach(item => {
  item.addEventListener('click', () => {
    const cmd = item.dataset.cmd;
    p1.startAttack('sp_' + cmd, false);
  });
});

// スローモーション＆予約ピン制御
function toggleSlowMo() {
  slowMo = !slowMo;
  const btn = document.getElementById('slowmo-btn');
  if (slowMo) {
    timeScale = 0.15;
    btn.classList.add('active');
    addFloatingText(p1.head.x, p1.head.y - 35, 'SLOW: 関節をドラッグして予約', '#38bdf8');
  } else {
    timeScale = 1.0;
    btn.classList.remove('active');
    // 予約されたピンへ向かって全関節を一斉に射出
    let launched = false;
    for (const p of p1.points) {
      if (p.pin) {
        const dx = p.pin.x - p.x;
        const dy = p.pin.y - p.y;
        p.oldx = p.x - dx * 0.42;
        p.oldy = p.y - dy * 0.42;
        p.pin = null;
        launched = true;
      }
    }
    if (launched) {
      addFloatingText(p1.head.x, p1.head.y - 35, 'COMBO RELEASE!', '#facc15');
    }
  }
}

document.getElementById('slowmo-btn').addEventListener('click', toggleSlowMo);
document.getElementById('clear-pins-btn').addEventListener('click', () => {
  for (const p of p1.points) p.pin = null;
});
document.getElementById('reset-btn').addEventListener('click', resetGame);

// マウス・タッチによるダイレクト関節ドラッグ
let draggedPoint = null;
let pointerPos = { x: 0, y: 0 };

function getCanvasCoords(e) {
  const rect = canvas.getBoundingClientRect();
  const clientX = e.touches ? e.touches[0].clientX : e.clientX;
  const clientY = e.touches ? e.touches[0].clientY : e.clientY;
  return {
    x: clientX - rect.left,
    y: clientY - rect.top
  };
}

function onPointerDown(e) {
  const pos = getCanvasCoords(e);
  pointerPos = pos;
  let closest = null;
  let minDist = 34;
  for (const p of p1.points) {
    const d = Math.hypot(p.x - pos.x, p.y - pos.y);
    if (d < minDist) {
      minDist = d;
      closest = p;
    }
  }
  draggedPoint = closest;
}

function onPointerMove(e) {
  if (!draggedPoint) return;
  e.preventDefault();
  pointerPos = getCanvasCoords(e);
  if (slowMo) {
    draggedPoint.pin = { x: pointerPos.x, y: pointerPos.y };
  }
}

function onPointerUp() {
  draggedPoint = null;
}

canvas.addEventListener('mousedown', onPointerDown);
window.addEventListener('mousemove', onPointerMove);
window.addEventListener('mouseup', onPointerUp);
canvas.addEventListener('touchstart', onPointerDown, { passive: false });
window.addEventListener('touchmove', onPointerMove, { passive: false });
window.addEventListener('touchend', onPointerUp);

// メインループ
function loop() {
  requestAnimationFrame(loop);

  if (hitStop > 0) {
    hitStop--;
    return;
  }

  // プレイヤー入力反映
  p1.crouching = Boolean(keys['s'] || keys['ArrowDown']);
  p1.guarding = Boolean(keys['v']);

  if (keys['a'] || keys['ArrowLeft']) p1.move(-1, timeScale);
  if (keys['d'] || keys['ArrowRight']) p1.move(1, timeScale);

  // ダイレクトドラッグの牽引力
  if (draggedPoint && !slowMo) {
    const dx = pointerPos.x - draggedPoint.x;
    const dy = pointerPos.y - draggedPoint.y;
    draggedPoint.x += dx * 0.45;
    draggedPoint.y += dy * 0.45;
  }

  // 物理ポイント更新
  for (const boxer of [p1, p2]) {
    for (const p of boxer.points) {
      p.update(timeScale);
    }
    boxer.updatePosture(boxer === p1 ? p2 : p1, timeScale);
  }

  // 骨格ボーン拘束の反復解決
  for (let i = 0; i < ITERATIONS; i++) {
    for (const boxer of [p1, p2]) {
      for (const s of boxer.sticks) {
        s.resolve();
      }
    }
  }

  // クールダウン更新＆打撃判定
  for (const boxer of [p1, p2]) {
    if (boxer.attackState && boxer.attackState.hitCooldown > 0) {
      boxer.attackState.hitCooldown -= timeScale;
    }
  }
  checkCombat(p1, p2);
  checkCombat(p2, p1);
  updateAI(timeScale);

  // 描画処理
  ctx.save();
  if (screenShake > 0.2) {
    ctx.translate((Math.random() - 0.5) * screenShake, (Math.random() - 0.5) * screenShake);
    screenShake *= 0.85;
  }

  // 背景とボクシングリング
  ctx.fillStyle = slowMo ? '#0b132b' : '#11131a';
  ctx.fillRect(0, 0, W, H);

  // リングロープ
  ctx.strokeStyle = 'rgba(255, 255, 255, 0.14)';
  ctx.lineWidth = 3;
  for (let r = 1; r <= 3; r++) {
    const ry = FLOOR_Y - r * 42;
    ctx.beginPath();
    ctx.moveTo(40, ry);
    ctx.lineTo(W - 40, ry);
    ctx.stroke();
  }

  // コーナーポスト
  ctx.fillStyle = '#2563eb';
  ctx.fillRect(32, FLOOR_Y - 140, 14, 140);
  ctx.fillStyle = '#e11d48';
  ctx.fillRect(W - 46, FLOOR_Y - 140, 14, 140);

  // リングマット（床）
  ctx.fillStyle = '#1e2538';
  ctx.fillRect(0, FLOOR_Y, W, H - FLOOR_Y);
  ctx.fillStyle = '#38bdf8';
  ctx.fillRect(0, FLOOR_Y, W, 3);

  // ボクサー描画
  p2.draw();
  p1.draw();

  // ドラッグ中のガイドライン
  if (draggedPoint && !slowMo) {
    ctx.strokeStyle = '#facc15';
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.moveTo(draggedPoint.x, draggedPoint.y);
    ctx.lineTo(pointerPos.x, pointerPos.y);
    ctx.stroke();
  }

  // パーティクル更新・描画
  for (let i = particles.length - 1; i >= 0; i--) {
    const pt = particles[i];
    pt.x += pt.vx * timeScale;
    pt.y += pt.vy * timeScale;
    pt.vy += 0.25 * timeScale;
    pt.life -= 0.04 * timeScale;
    if (pt.life <= 0) {
      particles.splice(i, 1);
      continue;
    }
    ctx.globalAlpha = pt.life;
    ctx.fillStyle = pt.color;
    ctx.beginPath();
    ctx.arc(pt.x, pt.y, 3.5, 0, Math.PI * 2);
    ctx.fill();
  }
  ctx.globalAlpha = 1;

  // フローティングテキスト
  ctx.font = 'bold 15px sans-serif';
  ctx.textAlign = 'center';
  for (let i = floatingTexts.length - 1; i >= 0; i--) {
    const ft = floatingTexts[i];
    ft.y -= 0.9 * timeScale;
    ft.life -= 0.025 * timeScale;
    if (ft.life <= 0) {
      floatingTexts.splice(i, 1);
      continue;
    }
    ctx.globalAlpha = ft.life;
    ctx.fillStyle = ft.color;
    ctx.fillText(ft.text, ft.x, ft.y);
  }
  ctx.globalAlpha = 1;
  ctx.restore();
}

resetGame();
loop();
</script>
</body>
</html>
