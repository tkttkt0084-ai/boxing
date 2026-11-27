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
    text-align
    
