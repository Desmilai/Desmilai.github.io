<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
  <title>ARM</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">

  <style>
    :root {
      --bg-base: #02050e;
      --glass-bg: rgba(15, 23, 42, 0.45);
      --glass-elevated: rgba(255, 255, 255, 0.08);
      --glass-border: rgba(255, 255, 255, 0.16);
      --glass-border-active: rgba(129, 140, 248, 0.7);
      --glass-shadow: inset 0 1px 1px 0 rgba(255, 255, 255, 0.22), 0 16px 36px 0 rgba(0, 0, 0, 0.55);
      --primary: #6366f1;
      --primary-glow: rgba(99, 102, 241, 0.45);
      --accent-neon: #a855f7;
      --accent-gold: #f59e0b;
      --danger: #f43f5e;
      --success: #10b981;
      --idle: #f59e0b;
      --offline: #64748b;
      --text: #f8fafc;
      --text-muted: #94a3b8;
    }

    *, *::before, *::after {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Plus Jakarta Sans', -apple-system, sans-serif !important;
      -webkit-tap-highlight-color: transparent;
      touch-action: manipulation;
      -webkit-user-select: none !important;
      user-select: none !important;
      -webkit-touch-callout: none !important;
    }

    input, textarea {
      -webkit-user-select: text !important;
      user-select: text !important;
    }

    html, body {
      width: 100vw;
      height: 100dvh;
      max-width: 100vw;
      max-height: 100dvh;
      margin: 0;
      padding: 0;
      overflow: hidden;
      overscroll-behavior: none;
      background-color: var(--bg-base);
      color: var(--text);
    }

    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      position: relative;
    }

    .gradient-mesh-bg {
      position: fixed;
      inset: 0;
      background: radial-gradient(at 0% 0%, rgba(99, 102, 241, 0.35) 0px, transparent 50%),
                  radial-gradient(at 100% 0%, rgba(168, 85, 247, 0.3) 0px, transparent 50%),
                  radial-gradient(at 100% 100%, rgba(236, 72, 153, 0.28) 0px, transparent 50%),
                  radial-gradient(at 0% 100%, rgba(6, 182, 212, 0.3) 0px, transparent 50%);
      z-index: -3;
      pointer-events: none;
    }

    .liquid-blob {
      position: fixed;
      border-radius: 50%;
      filter: blur(100px);
      z-index: -2;
      pointer-events: none;
      opacity: 0.65;
      animation: floatFluid 22s infinite alternate ease-in-out;
    }
    .blob-1 { width: 60vw; height: 60vw; max-width: 650px; max-height: 650px; background: radial-gradient(circle, #4f46e5 0%, #a855f7 55%, transparent 75%); top: -15%; left: -10%; }
    .blob-2 { width: 65vw; height: 65vw; max-width: 700px; max-height: 700px; background: radial-gradient(circle, #ec4899 0%, #06b6d4 65%, transparent 80%); bottom: -20%; right: -15%; animation-duration: 26s; }
    .blob-3 { width: 45vw; height: 45vw; max-width: 500px; max-height: 500px; background: radial-gradient(circle, #3b82f6 0%, transparent 75%); top: 30%; left: 30%; animation-duration: 19s; }

    @keyframes floatFluid {
      0% { transform: translate(0, 0) scale(1); }
      50% { transform: translate(45px, 35px) scale(1.12); }
      100% { transform: translate(-35px, 55px) scale(0.92); }
    }

    #ambientCanvas {
      position: fixed;
      top: -15%;
      left: -15%;
      width: 130vw;
      height: 130vh;
      z-index: -1;
      filter: blur(120px) saturate(260%) brightness(0.4);
      opacity: 0;
      pointer-events: none;
      transform: translate3d(0, 0, 0);
      transition: opacity 0.6s ease-in-out;
    }
    #ambientCanvas.active { opacity: 0.85; }

    .app-container {
      width: 100%;
      height: 100dvh;
      max-width: 1180px;
      margin: 0 auto;
      display: flex;
      flex-direction: column;
      gap: 8px;
      padding: env(safe-area-inset-top, 8px) 10px env(safe-area-inset-bottom, 8px) 10px;
      z-index: 1;
      min-height: 0;
    }

    .liquid-card {
      background: var(--glass-bg) !important;
      backdrop-filter: blur(28px) saturate(200%) !important;
      -webkit-backdrop-filter: blur(28px) saturate(200%) !important;
      border: 1px solid var(--glass-border) !important;
      border-radius: 18px !important;
      box-shadow: var(--glass-shadow) !important;
    }

    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 6px 14px;
      flex-shrink: 0;
      height: 50px;
    }

    .header-left, .header-right {
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .header-center {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
    }

    .logo-box {
      display: flex;
      align-items: center;
      gap: 6px;
      font-weight: 800;
      font-size: 1.2rem;
      letter-spacing: -0.03em;
      cursor: pointer;
    }

    .logo-pulse {
      width: 9px;
      height: 9px;
      border-radius: 50%;
      background: var(--primary);
      box-shadow: 0 0 14px var(--primary);
    }

    .btn-icon {
      background: rgba(255, 255, 255, 0.08);
      border: 1px solid var(--glass-border);
      color: var(--text);
      width: 36px;
      height: 36px;
      min-width: 36px;
      border-radius: 10px;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      position: relative;
      transition: all 0.2s;
      box-shadow: inset 0 1px 1px rgba(255,255,255,0.15);
    }
    .btn-icon:hover { background: rgba(255, 255, 255, 0.18); border-color: var(--glass-border-active); }
    .btn-icon:active { transform: scale(0.94); }
    .btn-icon svg { width: 18px; height: 18px; fill: none; stroke: currentColor; stroke-width: 2; stroke-linecap: round; stroke-linejoin: round; }

    .btn-icon.mic-live { background: var(--success); color: #fff; border-color: var(--success); }
    .btn-icon.mic-blocked { background: rgba(244, 63, 94, 0.2); color: var(--danger); border-color: var(--danger); cursor: not-allowed; }

    .badge-dot {
      position: absolute;
      top: 4px;
      right: 4px;
      width: 7px;
      height: 7px;
      background: var(--danger);
      border-radius: 50%;
      display: none;
      box-shadow: 0 0 6px var(--danger);
    }
    .badge-dot.active { display: block; }

    .btn {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 5px;
      padding: 6px 12px;
      font-size: 0.8rem;
      font-weight: 600;
      border-radius: 10px;
      border: 1px solid var(--glass-border);
      cursor: pointer;
      color: #fff;
      background: var(--primary);
      box-shadow: 0 4px 16px var(--primary-glow), inset 0 1px 1px rgba(255,255,255,0.3);
      transition: all 0.2s;
    }
    .btn svg { width: 15px; height: 15px; fill: none; stroke: currentColor; stroke-width: 2; }
    .btn:hover { background: #4f46e5; }
    .btn:active { transform: scale(0.96); }

    .btn-secondary { background: rgba(255, 255, 255, 0.08); box-shadow: inset 0 1px 1px rgba(255,255,255,0.15); }
    .btn-secondary:hover { background: rgba(255, 255, 255, 0.16); }
    .btn-danger { background: var(--danger); box-shadow: 0 4px 16px rgba(244, 63, 94, 0.4); }

    input, select {
      font-size: 16px !important;
      background: rgba(0, 0, 0, 0.35);
      backdrop-filter: blur(10px);
      border: 1px solid var(--glass-border);
      color: var(--text);
      padding: 9px 12px;
      border-radius: 10px;
      outline: none;
      width: 100%;
    }
    input:focus, select:focus { border-color: var(--glass-border-active); }

    .view-section { display: none; flex-direction: column; gap: 6px; flex: 1; min-height: 0; }
    .view-section.active { display: flex; }

    .lobby-grid {
      display: grid;
      grid-template-columns: 1fr 290px;
      gap: 12px;
      align-items: start;
      max-width: 960px;
      margin: 0 auto;
      width: 100%;
      overflow-y: auto;
      padding-bottom: 20px;
    }
    @media (max-width: 860px) {
      .lobby-grid { grid-template-columns: 1fr; }
    }

    .lobby-panel {
      display: flex;
      flex-direction: column;
      padding: 14px;
      height: fit-content;
      max-height: calc(100dvh - 90px);
    }

    .rooms-list, .friends-list {
      display: flex;
      flex-direction: column;
      gap: 8px;
      margin-top: 8px;
      overflow-y: auto;
      max-height: 60vh;
    }

    .room-item-liquid {
      display: flex;
      flex-direction: row;
      background: rgba(255, 255, 255, 0.04);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border: 1px solid var(--glass-border);
      border-radius: 14px;
      padding: 7px;
      gap: 10px;
      align-items: center;
      cursor: pointer;
      transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
      box-shadow: inset 0 1px 1px rgba(255,255,255,0.1);
      max-height: 76px;
    }
    .room-item-liquid:hover {
      background: rgba(255, 255, 255, 0.1);
      border-color: var(--glass-border-active);
      transform: translateY(-1px);
    }
    .room-item-liquid:active { transform: scale(0.98); }

    .room-card-preview {
      position: relative;
      width: 96px;
      height: 60px;
      min-width: 96px;
      border-radius: 10px;
      overflow: hidden;
      background: rgba(0, 0, 0, 0.4);
      display: flex;
      align-items: center;
      justify-content: center;
      border: 1px solid rgba(255,255,255,0.08);
    }
    .room-card-preview img { width: 100%; height: 100%; object-fit: cover; }

    .live-badge {
      position: absolute;
      top: 4px;
      left: 4px;
      background: rgba(244, 63, 94, 0.85);
      backdrop-filter: blur(4px);
      color: #fff;
      font-size: 0.6rem;
      font-weight: 800;
      padding: 1px 4px;
      border-radius: 4px;
      display: flex;
      align-items: center;
      gap: 3px;
    }
    .live-dot-pulse { width: 5px; height: 5px; border-radius: 50%; background: #fff; }

    .room-card-content {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 60px;
      flex: 1;
      min-width: 0;
      padding: 1px 0;
    }
    .room-card-title {
      font-size: 0.88rem;
      font-weight: 700;
      color: #fff;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .status-badge-public {
      background: rgba(16, 185, 129, 0.15);
      color: #34d399;
      border: 1px solid rgba(16, 185, 129, 0.35);
      border-radius: 8px;
      padding: 1px 6px;
      font-weight: 600;
      font-size: 0.65rem;
      display: inline-block;
    }
    .status-badge-unlisted {
      background: rgba(244, 63, 94, 0.15);
      color: #fb7185;
      border: 1px solid rgba(244, 63, 94, 0.35);
      border-radius: 8px;
      padding: 1px 6px;
      font-weight: 600;
      font-size: 0.65rem;
      display: inline-block;
    }
    .status-badge-friends {
      background: rgba(245, 158, 11, 0.15);
      color: #fbbf24;
      border: 1px solid rgba(245, 158, 11, 0.35);
      border-radius: 8px;
      padding: 1px 6px;
      font-weight: 600;
      font-size: 0.65rem;
      display: inline-block;
    }

    .room-members-stack {
      display: flex;
      align-items: center;
      gap: 3px;
    }
    .member-thumb {
      width: 20px;
      height: 20px;
      border-radius: 50%;
      background: #202738;
      border: 1px solid var(--glass-border);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.55rem;
      font-weight: 700;
      overflow: hidden;
    }
    .member-thumb img { width: 100%; height: 100%; object-fit: cover; }
    .member-thumb-more {
      width: 20px;
      height: 20px;
      border-radius: 50%;
      background: var(--primary);
      border: 1px solid #fff;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.52rem;
      font-weight: 800;
      color: #fff;
    }

    .empty-state-card {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 24px 14px;
      gap: 8px;
      text-align: center;
      color: var(--text-muted);
      font-size: 0.88rem;
    }

    .friend-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: rgba(255, 255, 255, 0.04);
      backdrop-filter: blur(12px);
      padding: 7px 10px;
      border-radius: 12px;
      gap: 8px;
      border: 1px solid var(--glass-border);
    }

    .friend-avatar-wrap, .avatar-box {
      position: relative;
      width: 36px;
      height: 36px;
      min-width: 36px;
      aspect-ratio: 1 / 1;
      flex-shrink: 0;
      border-radius: 50%;
      background: #202738;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: 700;
      font-size: 0.85rem;
      border: 1px solid var(--glass-border);
      overflow: visible !important;
      cursor: pointer;
    }
    .friend-avatar-wrap img, .avatar-box img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      object-fit: cover;
      display: block;
    }

    .avatar-crown {
      position: absolute;
      top: -12px;
      left: 50%;
      transform: translateX(-50%);
      width: 16px;
      height: 16px;
      fill: var(--accent-gold);
      filter: drop-shadow(0 0 5px rgba(245, 158, 11, 0.95));
      z-index: 5;
      pointer-events: none;
    }
    
    .status-dot-on-avatar {
      position: absolute;
      bottom: 0px;
      right: 0px;
      width: 9px;
      height: 9px;
      border-radius: 50%;
      border: 2px solid #0f1523;
    }
    .status-dot-on-avatar.online { background: var(--success); box-shadow: 0 0 6px var(--success); }
    .status-dot-on-avatar.offline { background: var(--offline); }

    .room-workspace {
      display: flex;
      flex-direction: column;
      gap: 8px;
      height: 100%;
      min-height: 0;
      overflow: hidden;
      width: 100%;
      margin: 0 auto;
    }

    @media (min-width: 960px) {
      .room-workspace {
        display: grid;
        grid-template-columns: 1fr 420px;
        gap: 12px;
        height: calc(100dvh - 68px);
      }
      .room-left-pane {
        display: flex;
        flex-direction: column;
        gap: 8px;
        height: 100%;
        min-height: 0;
      }
      .video-viewport {
        flex: 1;
        width: 100% !important;
        height: 100% !important;
        aspect-ratio: unset !important;
        min-height: 0;
      }
      .chat-container {
        height: 100% !important;
      }
    }

    .room-left-pane {
      display: flex;
      flex-direction: column;
      gap: 8px;
      flex-shrink: 0;
    }

    .room-sub-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 8px 12px;
      flex-shrink: 0;
    }

    .video-viewport {
      position: relative;
      width: 100%;
      background: rgba(0, 0, 0, 0.45);
      backdrop-filter: blur(24px);
      -webkit-backdrop-filter: blur(24px);
      border-radius: 18px !important;
      overflow: hidden;
      aspect-ratio: 16 / 9;
      display: flex;
      align-items: center;
      justify-content: center;
      border: 1px solid var(--glass-border);
      box-shadow: var(--glass-shadow);
      flex-shrink: 0;
    }
    video, iframe {
      width: 100%;
      height: 100%;
      max-height: 100%;
      object-fit: contain;
      border-radius: 18px;
      border: none;
    }

    .stream-loader-overlay {
      position: absolute;
      inset: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      background-size: cover;
      background-position: center;
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      gap: 10px;
      color: var(--text-muted);
      font-size: 0.8rem;
      z-index: 5;
    }
    .stream-spinner {
      width: 32px;
      height: 32px;
      border: 3px solid rgba(255, 255, 255, 0.12);
      border-top-color: var(--primary);
      border-radius: 50%;
      animation: spinStream 0.8s linear infinite;
    }
    @keyframes spinStream { to { transform: rotate(360deg); } }

    .chat-container {
      flex: 1;
      width: 100%;
      display: flex;
      flex-direction: column;
      padding: 10px;
      gap: 6px;
      min-height: 0;
      overflow: hidden;
    }

    .chat-messages {
      flex: 1;
      min-height: 0;
      overflow-y: auto;
      overflow-x: hidden !important;
      display: flex;
      flex-direction: column;
      gap: 10px;
      padding-right: 4px;
      width: 100%;
    }

    .chat-messages::-webkit-scrollbar { width: 4px; }
    .chat-messages::-webkit-scrollbar-thumb { background: rgba(255, 255, 255, 0.2); border-radius: 4px; }

    .msg-row {
      position: relative;
      display: flex;
      align-items: flex-start;
      gap: 8px;
      width: 100%;
      max-width: 100%;
      user-select: none;
      overflow: visible;
      transition: transform 0.18s cubic-bezier(0.34, 1.56, 0.64, 1);
    }
    .msg-row.self { justify-content: flex-end; }
    .msg-row.other { justify-content: flex-start; }
    .msg-row.pressing { transform: scale(0.92); }

    .msg-body {
      display: flex;
      flex-direction: column;
      gap: 2px;
      max-width: calc(100% - 46px);
      word-break: break-word;
      overflow-wrap: anywhere;
    }
    .msg-row.self .msg-body { align-items: flex-end; text-align: right; }

    .msg-inline-content {
      display: inline-block;
      min-height: 34px;
      line-height: 34px;
      font-size: 0.9rem;
      word-break: break-word;
      overflow-wrap: anywhere;
    }
    .msg-sender-bold {
      font-weight: 700;
      color: #ffffff;
      margin-right: 6px;
    }
    .msg-text-plain {
      font-weight: 400;
      color: #f8fafc;
    }

    .msg-meta-bar {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 0.68rem;
      color: var(--text-muted);
      margin-top: 1px;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.15s;
    }
    .msg-row.self .msg-meta-bar { justify-content: flex-end; }
    .msg-row.other .msg-meta-bar { justify-content: flex-start; }

    @media (min-width: 768px) {
      .msg-row:hover .msg-meta-bar { opacity: 1; pointer-events: auto; }
    }
    @media (max-width: 767px) {
      .msg-meta-bar { display: none !important; }
    }

    .msg-action-btn {
      background: rgba(255, 255, 255, 0.08);
      border: 1px solid var(--glass-border);
      color: var(--text-muted);
      border-radius: 6px;
      padding: 2px 4px;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      justify-content: center;
    }
    .msg-action-btn:hover { color: #fff; background: var(--primary); }
    .msg-action-btn svg { width: 12px; height: 12px; fill: none; stroke: currentColor; stroke-width: 2; }

    .msg-quote-preview {
      background: rgba(0, 0, 0, 0.35);
      backdrop-filter: blur(10px);
      border-radius: 8px;
      padding: 3px 6px;
      margin-bottom: 2px;
      display: flex;
      align-items: center;
      gap: 6px;
      max-width: 100%;
      border: 1px solid var(--glass-border);
    }
    .msg-quote-avatar {
      width: 16px;
      height: 16px;
      min-width: 16px;
      border-radius: 50%;
      background: #334155;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.55rem;
      font-weight: 700;
      overflow: hidden;
    }
    .msg-quote-avatar img { width: 100%; height: 100%; object-fit: cover; }
    .msg-quote-text {
      font-size: 0.7rem;
      color: #cbd5e1;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .emoji-anim-heart { display: inline-block; animation: heartbeat 0.8s infinite ease-in-out; }
    .emoji-anim-fire { display: inline-block; animation: flamePulse 1s infinite alternate ease-in-out; }
    .emoji-anim-party { display: inline-block; animation: confettiBurst 0.6s ease-out; }
    .emoji-anim-skull { display: inline-block; animation: skullShake 0.6s ease-in-out; }

    @keyframes heartbeat {
      0%, 100% { transform: scale(1); }
      25% { transform: scale(1.3); }
      50% { transform: scale(1.1); }
      75% { transform: scale(1.25); }
    }
    @keyframes flamePulse {
      0% { transform: scale(1) translateY(0); filter: drop-shadow(0 0 2px #f97316); }
      100% { transform: scale(1.25) translateY(-2px); filter: drop-shadow(0 0 8px #ef4444); }
    }
    @keyframes confettiBurst {
      0% { transform: scale(0.3) rotate(-20deg); }
      50% { transform: scale(1.4) rotate(15deg); }
      100% { transform: scale(1) rotate(0); }
    }
    @keyframes skullShake {
      0%, 100% { transform: rotate(0deg); }
      25% { transform: rotate(-10deg) scale(1.1); }
      75% { transform: rotate(10deg) scale(1.1); }
    }
    @keyframes popReaction {
      0% { transform: scale(0.3); opacity: 0; }
      100% { transform: scale(1); opacity: 1; }
    }

    .msg-reactions-wrap {
      display: flex;
      flex-wrap: wrap;
      gap: 3px;
      margin-top: 2px;
    }
    .reaction-pill {
      background: rgba(15, 23, 42, 0.7);
      backdrop-filter: blur(10px);
      border: 1px solid var(--glass-border);
      padding: 1px 6px 1px 3px;
      border-radius: 12px;
      display: inline-flex;
      align-items: center;
      gap: 4px;
      font-size: 0.78rem;
      cursor: pointer;
      animation: popReaction 0.28s cubic-bezier(0.34, 1.56, 0.64, 1);
    }

    .reaction-avatar {
      width: 14px;
      height: 14px;
      border-radius: 50%;
      background: #334155;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.5rem;
      font-weight: 700;
      overflow: hidden;
    }
    .reaction-avatar img { width: 100%; height: 100%; object-fit: cover; }

    .system-batch-wrap {
      align-self: center;
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      justify-content: center;
      gap: 4px;
      font-size: 0.75rem;
      color: var(--text-muted);
      margin: 2px 0;
      text-align: center;
    }
    .user-capsule {
      display: inline-flex;
      align-items: center;
      gap: 4px;
      background: rgba(255, 255, 255, 0.08);
      backdrop-filter: blur(10px);
      border: 1px solid var(--glass-border);
      padding: 1px 6px 1px 3px;
      border-radius: 10px;
      cursor: pointer;
    }
    .capsule-avatar {
      width: 14px;
      height: 14px;
      border-radius: 50%;
      background: #334155;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.5rem;
      font-weight: 700;
      overflow: hidden;
    }
    .capsule-avatar img { width: 100%; height: 100%; object-fit: cover; }
    .capsule-name {
      font-weight: 700;
      color: #ffffff;
      font-size: 0.75rem;
    }

    .msg-img { max-width: 180px; max-height: 130px; border-radius: 8px; cursor: pointer; margin-top: 3px; display: block; }

    .reply-preview-bar {
      display: none;
      align-items: center;
      gap: 8px;
      background: rgba(0, 0, 0, 0.35);
      backdrop-filter: blur(12px);
      border: 1px solid var(--glass-border);
      padding: 5px 8px;
      border-radius: 10px;
      font-size: 0.78rem;
    }
    .reply-preview-bar.active { display: flex; }
    .reply-preview-icon { color: var(--primary); display: flex; align-items: center; justify-content: center; }
    .reply-preview-icon svg { width: 14px; height: 14px; stroke: currentColor; fill: none; stroke-width: 2; }
    .reply-preview-content { flex: 1; display: flex; flex-direction: column; gap: 1px; overflow: hidden; }
    .reply-preview-user { font-weight: 700; color: #fff; font-size: 0.74rem; }
    .reply-preview-text { color: var(--text-muted); font-size: 0.7rem; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }

    .chat-input-panel {
      display: flex;
      flex-direction: column;
      gap: 4px;
      background: rgba(255, 255, 255, 0.04);
      backdrop-filter: blur(14px);
      border: 1px solid var(--glass-border);
      padding: 6px 8px;
      border-radius: 12px;
      position: relative;
      flex-shrink: 0;
    }

    .chat-textarea {
      width: 100%;
      background: transparent;
      border: none;
      color: var(--text);
      outline: none;
      resize: none;
      font-size: 16px !important;
      padding: 2px;
      min-height: 32px;
      max-height: 70px;
    }

    .chat-sub-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 4px;
    }

    .chat-tools-left {
      display: flex;
      gap: 4px;
      align-items: center;
    }

    .emoji-picker {
      display: none;
      position: absolute;
      bottom: 70px;
      left: 6px;
      right: 6px;
      background: rgba(15, 23, 42, 0.85);
      backdrop-filter: blur(28px);
      border: 1px solid var(--glass-border);
      border-radius: 14px;
      padding: 8px;
      flex-direction: column;
      gap: 6px;
      z-index: 100;
    }
    .emoji-picker.open { display: flex; animation: popReaction 0.2s cubic-bezier(0.34, 1.56, 0.64, 1); }
    .emoji-grid {
      display: grid;
      grid-template-columns: repeat(6, 1fr);
      gap: 6px;
      max-height: 140px;
      overflow-y: auto;
    }
    .emoji-btn { cursor: pointer; text-align: center; font-size: 1.25rem; }

    .photo-choice-menu {
      display: none;
      position: absolute;
      bottom: 70px;
      left: 35px;
      background: rgba(15, 23, 42, 0.85);
      backdrop-filter: blur(28px);
      border: 1px solid var(--glass-border);
      border-radius: 12px;
      padding: 4px;
      flex-direction: column;
      gap: 4px;
      z-index: 100;
    }
    .photo-choice-menu.open { display: flex; animation: popReaction 0.2s ease-out; }
    .photo-choice-btn {
      background: rgba(255, 255, 255, 0.05);
      border: none;
      color: #fff;
      padding: 6px 10px;
      border-radius: 8px;
      font-size: 0.8rem;
      cursor: pointer;
      text-align: left;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .toast-container {
      position: fixed;
      top: 16px;
      right: 16px;
      z-index: 99999;
      display: flex;
      flex-direction: column;
      gap: 8px;
      pointer-events: none;
    }
    .toast-card {
      background: rgba(18, 24, 38, 0.85);
      backdrop-filter: blur(20px);
      border: 1px solid var(--glass-border-active);
      color: #fff;
      padding: 10px 14px;
      border-radius: 12px;
      font-size: 0.82rem;
      box-shadow: 0 8px 24px rgba(0,0,0,0.5);
      animation: popReaction 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
      pointer-events: auto;
      max-width: 300px;
    }

    .modal-backdrop {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.65);
      backdrop-filter: blur(16px);
      z-index: 250;
      align-items: center;
      justify-content: center;
      padding: 16px;
    }
    .modal-backdrop.open { display: flex; }
    #participantsModal { z-index: 260; }
    #userProfileModal { z-index: 380 !important; }
    #contextMenu { z-index: 450; }

    .modal-card {
      width: 100%;
      max-width: 440px;
      padding: 18px;
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .invite-panel-card {
      width: 100%;
      max-width: 440px;
      height: 560px;
      max-height: 88dvh;
      display: flex;
      flex-direction: column;
      padding: 0;
      overflow: hidden;
    }
    .invite-header {
      padding: 10px 12px;
      display: flex;
      gap: 8px;
      align-items: center;
      border-bottom: 1px solid var(--glass-border);
    }
    .invite-body {
      flex: 1;
      overflow-y: auto;
      padding: 8px 10px;
      display: flex;
      flex-direction: column;
      gap: 6px;
    }
    .invite-friend-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      background: rgba(255, 255, 255, 0.04);
      padding: 6px 10px;
      border-radius: 10px;
      gap: 8px;
    }
    .invite-info-area { flex: 1; display: flex; flex-direction: column; cursor: pointer; overflow: hidden; }
    .invite-name { font-weight: 700; font-size: 0.86rem; }
    .invite-last-msg { font-size: 0.72rem; color: var(--text-muted); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }

    .invite-check-circle {
      width: 22px;
      height: 22px;
      min-width: 22px;
      border-radius: 50%;
      border: 2px solid var(--glass-border);
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.2s;
    }
    .invite-check-circle.selected { background: var(--primary); border-color: var(--primary); }
    .invite-check-circle svg { width: 12px; height: 12px; stroke: #fff; display: none; }
    .invite-check-circle.selected svg { display: block; }

    .invite-bottom-nav {
      border-top: 1px solid var(--glass-border);
      background: rgba(10, 14, 24, 0.7);
      backdrop-filter: blur(12px);
      display: flex;
      justify-content: space-around;
      padding: 6px 4px;
    }
    .invite-nav-btn {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 2px;
      background: none;
      border: none;
      color: var(--text-muted);
      font-size: 0.7rem;
      cursor: pointer;
      padding: 4px 6px;
    }
    .invite-nav-btn.active { color: var(--primary); font-weight: 700; }
    .invite-nav-btn svg { width: 16px; height: 16px; fill: none; stroke: currentColor; stroke-width: 2; }

    .dm-card {
      width: 100%;
      max-width: 440px;
      height: 520px;
      max-height: 88dvh;
      display: flex;
      flex-direction: column;
      padding: 12px;
      gap: 8px;
    }
    .dm-messages { flex: 1; overflow-y: auto; display: flex; flex-direction: column; gap: 6px; }

    .access-selection-row {
      display: flex;
      gap: 8px;
    }
    .access-pill-btn {
      flex: 1;
      padding: 5px 6px;
      border-radius: 10px;
      font-size: 0.72rem;
      font-weight: 600;
      cursor: pointer;
      text-align: center;
      transition: all 0.2s;
      border: 1px solid transparent;
      display: inline-block;
    }
    .access-pill-btn.opt-public { background: rgba(16, 185, 129, 0.15); color: #34d399; border-color: rgba(16, 185, 129, 0.4); }
    .access-pill-btn.opt-unlisted { background: rgba(244, 63, 94, 0.15); color: #fb7185; border-color: rgba(244, 63, 94, 0.4); }
    .access-pill-btn.opt-friends { background: rgba(245, 158, 11, 0.15); color: #fbbf24; border-color: rgba(245, 158, 11, 0.4); }

    .access-pill-btn.opt-public.active { box-shadow: 0 0 8px rgba(16,185,129,0.6); border-color: #10b981; }
    .access-pill-btn.opt-unlisted.active { box-shadow: 0 0 8px rgba(244,63,94,0.6); border-color: #f43f5e; }
    .access-pill-btn.opt-friends.active { box-shadow: 0 0 8px rgba(245,158,11,0.6); border-color: #f59e0b; }

    .user-actions-row {
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .action-sub-btn {
      width: 30px;
      height: 30px;
      min-width: 30px;
      border-radius: 8px;
      background: rgba(255, 255, 255, 0.06);
      border: 1px solid var(--glass-border);
      color: var(--text-muted);
      display: inline-flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.2s;
    }
    .action-sub-btn:hover { background: rgba(255,255,255,0.15); color: #fff; }
    .action-sub-btn.active-muted { color: var(--danger); border-color: var(--danger); background: rgba(244,63,94,0.15); }
    .action-sub-btn svg { width: 16px; height: 16px; }
  </style>

  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>
</head>
<body>

<script>
  (function() {
    const cleanGitHubWrapper = () => {
      document.querySelectorAll('.markdown-body, .container-lg, .my-5, .px-3').forEach(el => {
        el.removeAttribute('class');
        el.style.cssText = 'padding: 0 !important; margin: 0 !important; max-width: none !important; width: 100% !important; background: transparent !important; color: inherit !important;';
      });
    };
    if (document.body) cleanGitHubWrapper();
    window.addEventListener('DOMContentLoaded', cleanGitHubWrapper);
  })();
</script>

<div class="gradient-mesh-bg"></div>
<div class="liquid-blob blob-1"></div>
<div class="liquid-blob blob-2"></div>
<div class="liquid-blob blob-3"></div>
<canvas id="ambientCanvas" width="64" height="36"></canvas>

<div class="toast-container" id="toastContainer"></div>
<div id="remoteAudiosContainer" style="display:none;"></div>

<div class="app-container">
  
  <header class="liquid-card">
    <div class="header-left">
      <button class="btn-icon" onclick="openNotifications()" title="Уведомления">
        <svg viewBox="0 0 24 24"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"></path><path d="M13.73 21a2 2 0 0 1-3.46 0"></path></svg>
        <div class="badge-dot" id="notifBadge"></div>
      </button>

      <button class="btn-icon" onclick="openProfileStatsModal()" title="Мой профиль">
        <span class="status-dot-on-avatar online" id="myStatusDot" style="position: absolute; top: 4px; right: 4px; width:7px; height:7px;"></span>
        <svg viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></svg>
      </button>

      <button class="btn-icon" onclick="openSettingsModal()" title="Настройки">
        <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="3"></circle><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path></svg>
      </button>

      <button class="btn-icon" id="btnRoomSettings" onclick="openRoomSettings()" style="display: none;" title="Настройки комнаты">
        <svg viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect><line x1="9" y1="3" x2="9" y2="21"></line></svg>
      </button>
    </div>

    <div class="header-center">
      <div class="logo-box" onclick="showLobby()">
        <div class="logo-pulse"></div>
        ARM
      </div>
    </div>

    <div class="header-right">
      <button class="btn" id="btnHeaderCreate" onclick="checkAndOpenCreateRoom()" title="Создать комнату">
        <svg viewBox="0 0 24 24"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
        <span>Создать</span>
      </button>

      <button class="btn btn-danger" id="btnHeaderLeave" onclick="handleLeaveRoomClick()" style="display: none;">
        Выйти
      </button>
    </div>
  </header>

  <div id="lobbyView" class="view-section active">
    <div class="lobby-grid">
      <div class="liquid-card lobby-panel">
        <div style="display: flex; justify-content: space-between; align-items: center; flex-shrink: 0;">
          <h3 style="font-size: 1rem;">Комнаты</h3>
          <span style="font-size: 0.76rem; color: var(--text-muted);" id="roomsCount">0 комнат</span>
        </div>
        <div class="rooms-list" id="roomsList"></div>
      </div>

      <div class="liquid-card lobby-panel">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 4px; flex-shrink: 0;">
          <h3 style="font-size: 1rem;">Друзья</h3>
          <button class="btn btn-secondary" style="padding: 3px 8px; font-size: 0.72rem;" onclick="openModal('addFriendModal')">+ Добавить</button>
        </div>
        <div class="friends-list" id="friendsList"></div>
      </div>
    </div>
  </div>

  <div id="roomView" class="view-section">
    <div class="room-workspace">
      
      <div class="room-left-pane">
        <div class="room-sub-bar liquid-card">
          <div style="display: flex; align-items: center; gap: 6px;">
            <span style="font-size: 0.76rem; color: var(--text-muted);">Статус:</span>
            <span id="roomAccessBadge" class="status-badge-public">Публичный</span>
          </div>

          <div style="display: flex; gap: 6px; align-items: center; flex-shrink: 0;">
            <button class="btn btn-secondary" id="btnChooseVk" onclick="openVkVideoModal()" style="display: none; padding: 4px 8px; font-size: 0.75rem;" title="Выбрать ВК Видео">
              📼 ВК Видео
            </button>

            <button class="btn-icon" id="btnStreamToggle" onclick="toggleScreenBroadcast()" style="display: none;" title="Трансляция экрана">
              <svg id="streamIconPlay" viewBox="0 0 24 24"><polygon points="5 3 19 12 5 21 5 3"></polygon></svg>
            </button>

            <button class="btn-icon" id="btnRequestPause" onclick="sendPauseRequest()" title="Попросить о паузе" style="display: none;">
              <svg viewBox="0 0 24 24"><rect x="6" y="4" width="4" height="16"></rect><rect x="14" y="4" width="4" height="16"></rect></svg>
            </button>

            <button class="btn btn-secondary" style="padding: 5px 8px;" onclick="openParticipantsModal()" title="Участники">
              👥 <span id="participantsCounterText" style="font-weight: 700; margin-left: 2px;">0</span>
            </button>
          </div>
        </div>

        <div class="video-viewport" id="videoViewportContainer">
          <div class="stream-loader-overlay" id="streamLoader" style="display: none;">
            <div class="stream-spinner"></div>
            <span>Ожидание трансляции...</span>
          </div>
          <div id="mediaTarget" style="width:100%; height:100%; display:flex; align-items:center; justify-content:center;">
            <video id="roomVideo" autoplay playsinline controls style="width:100%; height:100%; object-fit:contain; border-radius:18px;"></video>
          </div>
        </div>
      </div>

      <div class="chat-container liquid-card">
        <div class="chat-messages" id="chatMessages"></div>

        <div class="reply-preview-bar" id="replyPreview">
          <div class="reply-preview-icon">
            <svg viewBox="0 0 24 24"><polyline points="9 17 4 12 9 7"></polyline><path d="M20 18v-2a4 4 0 0 0-4-4H4"></path></svg>
          </div>
          <div class="reply-preview-content">
            <div class="reply-preview-user" id="replyUserTitle">Ответ на @user</div>
            <div class="reply-preview-text" id="replyTextSnippet">текст сообщения</div>
          </div>
          <button style="background:none; border:none; color:var(--text-muted); cursor:pointer; font-size:1rem; padding:0 4px;" onclick="cancelReply()">✕</button>
        </div>

        <div class="chat-input-panel">
          <textarea id="chatInput" class="chat-textarea" placeholder="Написать сообщение..." rows="1" onkeydown="if(event.key==='Enter' && !event.shiftKey){ event.preventDefault(); sendChatMessage(); }"></textarea>

          <div class="chat-sub-bar">
            <div class="chat-tools-left">
              <button class="btn-icon" id="btnMic" onclick="toggleMicrophone()" style="width:32px; height:32px; min-width:32px;" title="Микрофон">
                <svg viewBox="0 0 24 24"><path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"></path><path d="M19 10v2a7 7 0 0 1-14 0v-2"></path><line x1="12" y1="19" x2="12" y2="23"></line><line x1="8" y1="23" x2="16" y2="23"></line></svg>
              </button>

              <button class="btn-icon" onclick="toggleEmoji()" style="width:32px; height:32px; min-width:32px;" title="Смайлы">
                <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"></circle><path d="M8 14s1.5 2 4 2 4-2 4-2"></path><line x1="9" y1="9" x2="9.01" y2="9"></line><line x1="15" y1="9" x2="15.01" y2="9"></line></svg>
              </button>

              <button class="btn-icon" onclick="togglePhotoMenu()" style="width:32px; height:32px; min-width:32px;" title="Прикрепить фото">
                <svg viewBox="0 0 24 24"><rect x="3" y="3" width="18" height="18" rx="2" ry="2"></rect><circle cx="8.5" cy="8.5" r="1.5"></circle><polyline points="21 15 16 10 5 21"></polyline></svg>
              </button>

              <button class="btn-icon" onclick="openInviteFriendsModal()" style="width:32px; height:32px; min-width:32px;" title="Пригласить друзей">
                <svg viewBox="0 0 24 24"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="8.5" cy="7" r="4"></circle><line x1="20" y1="8" x2="20" y2="14"></line><line x1="23" y1="11" x2="17" y2="11"></line></svg>
              </button>

              <button class="btn-icon" onclick="copyRoomLink()" style="width:32px; height:32px; min-width:32px;" title="Скопировать ссылку">
                <svg viewBox="0 0 24 24"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg>
              </button>
            </div>

            <button class="btn" style="padding: 5px 12px;" onclick="sendChatMessage()">
              <svg viewBox="0 0 24 24"><line x1="22" y1="2" x2="11" y2="13"></line><polygon points="22 2 15 22 11 13 2 9 22 2"></polygon></svg>
            </button>
          </div>

          <div class="emoji-picker" id="emojiPicker">
            <input type="text" id="emojiSearchInput" placeholder="Поиск эмодзи..." oninput="filterEmojis(this.value, '#emojiGrid')" style="padding: 5px 8px; font-size: 14px !important;">
            <div class="emoji-grid" id="emojiGrid">
              <span class="emoji-btn" onclick="addEmoji('❤️')" data-tags="люблю сердце love"><span class="emoji-anim-heart">❤️</span></span>
              <span class="emoji-btn" onclick="addEmoji('💀')" data-tags="скелет череп skull"><span class="emoji-anim-skull">💀</span></span>
              <span class="emoji-btn" onclick="addEmoji('🔥')" data-tags="огонь flame hot"><span class="emoji-anim-fire">🔥</span></span>
              <span class="emoji-btn" onclick="addEmoji('😂')" data-tags="смех ржу lol">😂</span>
              <span class="emoji-btn" onclick="addEmoji('😎')" data-tags="круто cool">😎</span>
              <span class="emoji-btn" onclick="addEmoji('👀')" data-tags="глаза look watch">👀</span>
              <span class="emoji-btn" onclick="addEmoji('🍿')" data-tags="попкорн popcorn">🍿</span>
              <span class="emoji-btn" onclick="addEmoji('🎉')" data-tags="ура праздник party"><span class="emoji-anim-party">🎉</span></span>
              <span class="emoji-btn" onclick="addEmoji('👍')" data-tags="лайк ok">👍</span>
              <span class="emoji-btn" onclick="addEmoji('⚡')" data-tags="молния flash power">⚡</span>
              <span class="emoji-btn" onclick="addEmoji('🚀')" data-tags="ракета space">🚀</span>
              <span class="emoji-btn" onclick="addEmoji('✨')" data-tags="звезды shine">✨</span>
              <span class="emoji-btn" onclick="addEmoji('🥺')" data-tags="милый плач plead">🥺</span>
              <span class="emoji-btn" onclick="addEmoji('😭')" data-tags="плач cry">😭</span>
              <span class="emoji-btn" onclick="addEmoji('😱')" data-tags="шок shock">😱</span>
              <span class="emoji-btn" onclick="addEmoji('🥳')" data-tags="праздник party"><span class="emoji-anim-party">🥳</span></span>
              <span class="emoji-btn" onclick="addEmoji('😈')" data-tags="демон evil">😈</span>
            </div>
          </div>

          <div class="photo-choice-menu" id="photoMenu">
            <label class="photo-choice-btn">
              📷 Сделать фото
              <input type="file" accept="image/*" capture="environment" style="display: none;" onchange="uploadChatPhoto(event)">
            </label>
            <label class="photo-choice-btn">
              🖼️ Из галереи
              <input type="file" accept="image/*" style="display: none;" onchange="uploadChatPhoto(event)">
            </label>
          </div>
        </div>
      </div>

    </div>
  </div>
</div>

<!-- ================= МОДАЛЬНЫЕ ОКНА ================= -->

<div class="modal-backdrop" id="vkVideoModal">
  <div class="modal-card liquid-card">
    <div style="display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid var(--glass-border); padding-bottom: 6px;">
      <h3 style="font-size:1.05rem;">Выбрать ВК Видео</h3>
      <button class="btn-icon" onclick="closeModal('vkVideoModal')">✕</button>
    </div>
    <div style="font-size: 0.8rem; color: var(--text-muted);">Вставьте любую ссылку на видео из VK или iframe-код:</div>
    <input type="text" id="vkVideoInputUrl" placeholder="https://vkvideo.ru/video-203677279_456242617">
    <div style="display: flex; justify-content: flex-end; gap: 6px; margin-top: 6px;">
      <button class="btn btn-secondary" onclick="closeModal('vkVideoModal')">Отмена</button>
      <button class="btn" onclick="launchVkVideoByHost()">Запустить для всех</button>
    </div>
  </div>
</div>

<div class="modal-backdrop" id="hostLeaveModal">
  <div class="modal-card liquid-card" style="text-align: center;">
    <h3 style="font-size:1.05rem;">Выход из комнаты</h3>
    <div style="font-size:0.84rem; color:var(--text-muted); margin:6px 0 10px;">Вы являетесь создателем этой комнаты. Выберите действие:</div>
    <div style="display: flex; flex-direction: column; gap: 8px;">
      <button class="btn btn-danger" onclick="confirmDeleteRoomByHost()">🗑️ Удалить комнату для всех</button>
      <button class="btn btn-secondary" onclick="confirmLeaveAndPassHost()">👑 Просто выйти (передать права)</button>
      <button class="btn btn-secondary" onclick="closeModal('hostLeaveModal')">Отмена</button>
    </div>
  </div>
</div>

<div class="modal-backdrop" id="inviteFriendsDrawer">
  <div class="modal-card liquid-card invite-panel-card">
    <div class="invite-header">
      <input type="text" id="inviteSearchInput" placeholder="Поиск друзей..." oninput="renderInviteFriendsList()" style="flex:1;">
      <button class="btn btn-secondary" style="padding:5px 8px;" onclick="toggleSelectAllFriends()">Все</button>
    </div>
    <div class="invite-body" id="inviteFriendsList"></div>
    <div style="padding: 6px 10px; display:none;" id="sendInviteActionBar">
      <button class="btn btn-success" style="width: 100%;" onclick="sendBatchRoomInvites()">Пригласить (<span id="inviteSelectedCount">0</span>)</button>
    </div>
    <div class="invite-bottom-nav">
      <button class="invite-nav-btn" onclick="closeModal('inviteFriendsDrawer')">
        <svg viewBox="0 0 24 24"><polyline points="15 18 9 12 15 6"></polyline></svg>
        <span>Назад</span>
      </button>
      <button class="invite-nav-btn active" id="tabInvFriends" onclick="switchInviteTab('friends')">
        <svg viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle></svg>
        <span>Друзья</span>
      </button>
      <button class="invite-nav-btn" id="tabInvRecent" onclick="switchInviteTab('recent')">
        <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"></circle><polyline points="12 6 12 12 16 14"></polyline></svg>
        <span>Недавние</span>
      </button>
      <button class="invite-nav-btn" id="tabInvRequests" onclick="switchInviteTab('requests')">
        <svg viewBox="0 0 24 24"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="8.5" cy="7" r="4"></circle><line x1="20" y1="8" x2="20" y2="14"></line><line x1="23" y1="11" x2="17" y2="11"></line></svg>
        <span>Запросы</span>
      </button>
    </div>
  </div>
</div>

<div class="modal-backdrop" id="dmModal">
  <div class="modal-card liquid-card dm-card">
    <div style="display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid var(--glass-border); padding-bottom: 6px;">
      <div style="display: flex; align-items: center; gap: 6px;">
        <div class="avatar-box" id="dmPartnerAvatar">U</div>
        <div>
          <b id="dmPartnerName" style="font-size:0.85rem;">Имя</b>
          <div style="font-size: 0.68rem; color: var(--text-muted);" id="dmPartnerUsername">@username</div>
        </div>
      </div>
      <button class="btn-icon" onclick="closeModal('dmModal')">✕</button>
    </div>
    <div class="dm-messages" id="dmMessages"></div>
    <div style="display: flex; gap: 6px;">
      <input type="text" id="dmInput" placeholder="Личное сообщение..." onkeydown="if(event.key==='Enter') sendDirectMessage()">
      <button class="btn" onclick="sendDirectMessage()">➤</button>
    </div>
  </div>
</div>

<div class="modal-backdrop" id="userProfileModal">
  <div class="modal-card liquid-card" style="text-align: center; align-items: center;">
    <div class="avatar-box" id="upAvatar" style="width: 58px; height: 58px; font-size: 1.4rem; margin-bottom: 4px;">U</div>
    <h3 id="upName" style="font-size:1.05rem;">Имя</h3>
    <div id="upUsername" style="color: var(--text-muted); font-size: 0.8rem; margin-bottom: 12px;">@username</div>
    <div style="display: flex; gap: 6px; width: 100%;">
      <button class="btn btn-secondary" style="flex:1;" onclick="openDmFromProfile()">💬 Написать</button>
      <button class="btn" style="flex:1;" id="btnUpFriendAction" onclick="toggleFriendFromProfile()">+ В друзья</button>
    </div>
    <button class="btn btn-secondary" style="width: 100%; margin-top: 4px;" onclick="closeModal('userProfileModal')">Закрыть</button>
  </div>
</div>

<div class="context-menu-backdrop" id="contextMenu" onclick="closeContextMenu()">
  <div class="context-menu-card" onclick="event.stopPropagation()">
    <div class="msg-quote-preview" style="margin-bottom:0;" id="ctxMsgPreview">Сообщение</div>
    <div style="font-size: 0.7rem; color: var(--text-muted); text-align: center;" id="ctxMsgTime">Время отправки</div>
    <div style="display: flex; gap: 6px;">
      <button class="btn btn-secondary" style="flex:1;" onclick="ctxReply()">💬 Ответить</button>
      <button class="btn btn-secondary" style="flex:1;" onclick="ctxCopy()">📋 Копировать</button>
    </div>
    <div style="font-size: 0.72rem; color: var(--text-muted); margin-top: 2px;">Поставить реакцию:</div>
    <input type="text" id="ctxEmojiSearch" placeholder="Поиск эмодзи..." oninput="filterEmojis(this.value, '#ctxEmojiGrid')" style="padding: 5px 8px; font-size: 14px !important;">
    <div class="emoji-grid" id="ctxEmojiGrid" style="max-height: 100px; overflow-y: auto;">
      <span class="emoji-btn" onclick="ctxReact('❤️')" data-tags="люблю сердце love">❤️</span>
      <span class="emoji-btn" onclick="ctxReact('💀')" data-tags="скелет череп skull">💀</span>
      <span class="emoji-btn" onclick="ctxReact('🔥')" data-tags="огонь flame hot">🔥</span>
      <span class="emoji-btn" onclick="ctxReact('😂')" data-tags="смех ржу lol">😂</span>
      <span class="emoji-btn" onclick="ctxReact('😎')" data-tags="круто cool">😎</span>
      <span class="emoji-btn" onclick="ctxReact('👀')" data-tags="глаза look watch">👀</span>
      <span class="emoji-btn" onclick="ctxReact('👍')" data-tags="лайк ok">👍</span>
      <span class="emoji-btn" onclick="ctxReact('🥺')" data-tags="милый плач plead">🥺</span>
      <span class="emoji-btn" onclick="ctxReact('😭')" data-tags="плач cry">😭</span>
      <span class="emoji-btn" onclick="ctxReact('🎉')" data-tags="праздник party">🎉</span>
    </div>
    <button class="btn btn-secondary" style="margin-top: 2px;" onclick="closeContextMenu()">Отмена</button>
  </div>
</div>

<div class="modal-backdrop" id="profileStatsModal">
  <div class="modal-card liquid-card">
    <div style="display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid var(--glass-border); padding-bottom: 6px;">
      <h3 style="font-size:1.05rem;">Профиль</h3>
      <button class="btn-icon" onclick="closeModal('profileStatsModal')">✕</button>
    </div>
    <div style="display: flex; align-items: center; gap: 10px; margin-top: 4px;">
      <div class="avatar-box" id="myProfileAvatarBig" style="width: 50px; height: 50px; font-size: 1.2rem;">U</div>
      <div style="display: flex; flex-direction: column;">
        <b style="font-size: 1rem; color: #fff;" id="myProfileNameText">Имя</b>
        <span style="font-size: 0.78rem; color: var(--text-muted);" id="myProfileUnameText">@username</span>
      </div>
    </div>
    <div style="background: rgba(0,0,0,0.3); backdrop-filter: blur(10px); border: 1px solid var(--glass-border); border-radius: 12px; padding: 10px; display: flex; flex-direction: column; gap: 8px;">
      <div style="display: flex; justify-content: space-between; align-items: center; font-size: 0.75rem;">
        <span style="font-weight: 700; color: #cbd5e1;">Статистика активности</span>
        <div style="display: flex; align-items: center; gap: 4px;">
          <span class="status-dot-on-avatar online" style="position:static; width:7px; height:7px;"></span>
          <span style="color: var(--success);" id="statsOnlineLabel">В сети</span>
        </div>
      </div>
      <canvas id="activityChartCanvas" width="340" height="70" style="width: 100%; height: 70px;"></canvas>
      <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 6px; font-size: 0.75rem; color: var(--text-muted); border-top: 1px solid rgba(255,255,255,0.06); padding-top: 6px;">
        <div>Зарегистрирован: <b style="color:#fff;" id="statRegDate">20.08.2026</b></div>
        <div>Проведено времени: <b style="color:#fff;" id="statTotalHours">0 ч.</b></div>
        <div>Друзья: <b style="color:#fff;" id="statFriendsCount">0</b></div>
      </div>
    </div>
    <button class="btn btn-secondary" style="width: 100%; margin-top: 4px;" onclick="closeModal('profileStatsModal')">Закрыть</button>
  </div>
</div>

<div class="modal-backdrop" id="settingsModal">
  <div class="modal-card liquid-card">
    <div style="display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid var(--glass-border); padding-bottom: 6px;">
      <h3 style="font-size:1.05rem;">Настройки</h3>
      <button class="btn-icon" onclick="closeModal('settingsModal')">✕</button>
    </div>
    <div style="display: flex; align-items: center; gap: 10px;">
      <div class="avatar-box" id="settingsAvatarPreview" style="width: 44px; height: 44px; font-size: 1.1rem;">U</div>
      <label class="btn btn-secondary" style="font-size: 0.75rem; cursor: pointer;">
        Сменить аватар
        <input type="file" accept="image/*" style="display: none;" onchange="uploadAvatar(event)">
      </label>
    </div>
    <div>
      <label style="font-size: 0.76rem; color: var(--text-muted);">Имя</label>
      <input type="text" id="settingInputName">
    </div>
    <div>
      <label style="font-size: 0.76rem; color: var(--text-muted);">Юзернейм (@username)</label>
      <input type="text" id="settingInputUsername">
    </div>
    <div>
      <label style="font-size: 0.76rem; color: var(--text-muted); margin-bottom: 4px; display: block;">Кто может приглашать в комнаты</label>
      <div class="access-selection-row">
        <div class="access-pill-btn opt-public active" id="privBtn_all" onclick="setInvitePrivacy('all')">Все</div>
        <div class="access-pill-btn opt-friends" id="privBtn_friends" onclick="setInvitePrivacy('friends')">Друзья</div>
        <div class="access-pill-btn opt-unlisted" id="privBtn_none" onclick="setInvitePrivacy('none')">Никто</div>
      </div>
    </div>
    <div>
      <label style="font-size: 0.76rem; color: var(--text-muted); margin-bottom: 4px; display: block;">Быстрая реакция (двойной клик)</label>
      <div style="display: flex; align-items: center; gap: 8px;">
        <button class="btn btn-secondary" style="font-size: 1.2rem; padding: 4px 12px;" id="currentQuickReactionDisplay" onclick="openQuickReactionSelector()">❤️</button>
        <span style="font-size: 0.72rem; color: var(--text-muted);">Нажмите для выбора эмодзи</span>
      </div>
    </div>
    <div style="display: flex; justify-content: flex-end; gap: 6px; margin-top: 4px;">
      <button class="btn btn-secondary" onclick="closeModal('settingsModal')">Отмена</button>
      <button class="btn" onclick="saveSettings()">Сохранить</button>
    </div>
  </div>
</div>

<div class="modal-backdrop" id="quickReactionModal">
  <div class="modal-card liquid-card" style="max-width: 320px;">
    <h3 style="font-size: 1rem;">Выберите быструю реакцию</h3>
    <div class="emoji-grid" style="max-height: 140px; overflow-y: auto;">
      <span class="emoji-btn" onclick="chooseQuickReaction('❤️')">❤️</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('🔥')">🔥</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('💀')">💀</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('😂')">😂</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('😎')">😎</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('👍')">👍</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('🎉')">🎉</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('🥺')">🥺</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('✨')">✨</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('🚀')">🚀</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('😈')">😈</span>
      <span class="emoji-btn" onclick="chooseQuickReaction('🍿')">🍿</span>
    </div>
    <button class="btn btn-secondary" onclick="closeModal('quickReactionModal')">Отмена</button>
  </div>
</div>

<div class="modal-backdrop" id="roomSettingsModal">
  <div class="modal-card liquid-card">
    <h3 style="font-size:1.05rem;">⚙ Настройки комнаты</h3>
    <div>
      <label style="font-size: 0.76rem; color: var(--text-muted); margin-bottom: 4px; display: block;">Название комнаты</label>
      <input type="text" id="editRoomName">
    </div>
    <div>
      <label style="font-size: 0.76rem; color: var(--text-muted); margin-bottom: 4px; display: block;">Доступность</label>
      <div class="access-selection-row">
        <div class="access-pill-btn opt-public active" id="optBtn_public" onclick="setRoomAccessOption('public')">Публичный</div>
        <div class="access-pill-btn opt-unlisted" id="optBtn_unlisted" onclick="setRoomAccessOption('unlisted')">По ссылке</div>
        <div class="access-pill-btn opt-friends" id="optBtn_friends" onclick="setRoomAccessOption('friends')">Только друзьям</div>
      </div>
    </div>
    <div style="display: flex; align-items: center; gap: 6px; margin-top: 2px;">
      <input type="checkbox" id="editMutePresence" style="width: 16px; height: 16px;">
      <label for="editMutePresence" style="font-size: 0.8rem; cursor: pointer;">Отключить уведомления о входе/выходе</label>
    </div>
    <div style="display: flex; justify-content: flex-end; gap: 6px; margin-top: 4px;">
      <button class="btn btn-secondary" onclick="closeModal('roomSettingsModal')">Отмена</button>
      <button class="btn" onclick="saveRoomSettings()">Сохранить</button>
    </div>
  </div>
</div>

<div class="modal-backdrop" id="participantsModal">
  <div class="modal-card liquid-card">
    <div style="display: flex; justify-content: space-between; align-items: center;">
      <h3 style="font-size:1.05rem;">Участники комнаты</h3>
      <button class="btn-icon" onclick="closeModal('participantsModal')">✕</button>
    </div>
    <div id="participantsList" style="display: flex; flex-direction: column; gap: 6px; max-height: 300px; overflow-y: auto;"></div>
  </div>
</div>

<div class="modal-backdrop" id="notifModal">
  <div class="modal-card liquid-card">
    <div style="display: flex; justify-content: space-between; align-items: center;">
      <h3 style="font-size:1.05rem;">Уведомления</h3>
      <button class="btn-icon" onclick="closeModal('notifModal')">✕</button>
    </div>
    <div id="notifList" style="display: flex; flex-direction: column; gap: 6px; max-height: 260px; overflow-y: auto;"></div>
  </div>
</div>

<div class="modal-backdrop" id="addFriendModal">
  <div class="modal-card liquid-card">
    <h3 style="font-size:1.05rem;">Добавить друга</h3>
    <input type="text" id="newFriendUsername" placeholder="Введите @username">
    <div style="display: flex; justify-content: flex-end; gap: 6px;">
      <button class="btn btn-secondary" onclick="closeModal('addFriendModal')">Отмена</button>
      <button class="btn" onclick="sendFriendRequest()">Отправить</button>
    </div>
  </div>
</div>

<div class="modal-backdrop" id="createRoomModal">
  <div class="modal-card liquid-card">
    <h3 style="font-size:1.05rem;">Создать комнату</h3>
    <input type="text" id="newRoomName" placeholder="Название комнаты">
    <div style="margin-top: 4px;">
      <label style="font-size: 0.76rem; color: var(--text-muted); margin-bottom: 4px; display: block;">Доступность</label>
      <div class="access-selection-row">
        <div class="access-pill-btn opt-public active" id="createOptBtn_public" onclick="setCreateAccessOption('public')">Публичный</div>
        <div class="access-pill-btn opt-unlisted" id="createOptBtn_unlisted" onclick="setCreateAccessOption('unlisted')">По ссылке</div>
        <div class="access-pill-btn opt-friends" id="createOptBtn_friends" onclick="setCreateAccessOption('friends')">Только друзьям</div>
      </div>
    </div>
    <div style="display: flex; justify-content: flex-end; gap: 6px; margin-top: 8px;">
      <button class="btn btn-secondary" onclick="closeModal('createRoomModal')">Отмена</button>
      <button class="btn" onclick="createRoom()">Создать</button>
    </div>
  </div>
</div>

<script>
  const CROWN_SVG = `<svg class="avatar-crown" viewBox="0 0 24 24"><path d="M2 4l3 12h14l3-12-6 7-4-7-4 7-6-7zm3 14h14v2H5v-2z"/></svg>`;
  const REPLY_ICON_SVG = `<svg viewBox="0 0 24 24"><polyline points="9 17 4 12 9 7"></polyline><path d="M20 18v-2a4 4 0 0 0-4-4H4"></path></svg>`;
  const REACT_ICON_SVG = `<svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"></circle><path d="M8 14s1.5 2 4 2 4-2 4-2"></path><line x1="9" y1="9" x2="9.01" y2="9"></line><line x1="15" y1="9" x2="15.01" y2="9"></line></svg>`;

  const firebaseConfig = {
    apiKey: "AIzaSyAFMTLchJHYEo0BY7vA7msCkkPu5qivg-E",
    authDomain: "desmi-8631d.firebaseapp.com",
    databaseURL: "https://desmi-8631d-default-rtdb.firebaseio.com",
    projectId: "desmi-8631d",
    storageBucket: "desmi-8631d.firebasestorage.app",
    messagingSenderId: "580647208889",
    appId: "1:580647208889:web:bb632a34651272ee379868",
    measurementId: "G-M35GSPH0QJ"
  };

  firebase.initializeApp(firebaseConfig);
  const db = firebase.database();

  const rtcConfig = {
    iceServers: [
      { urls: 'stun:stun.l.google.com:19302' },
      { urls: 'stun:stun1.l.google.com:19302' },
      { urls: 'stun:stun.cloudflare.com:3478' }
    ],
    iceCandidatePoolSize: 6
  };

  let user = JSON.parse(localStorage.getItem('arm_user')) || {
    id: 'u_' + Math.random().toString(36).substring(2, 8),
    name: 'User_' + Math.floor(Math.random() * 899 + 100),
    username: 'user_' + Math.floor(Math.random() * 899 + 100),
    avatar: null,
    registeredAt: new Date().toLocaleDateString('ru-RU'),
    totalSeconds: 0,
    quickReaction: '❤️',
    invitePrivacy: 'all'
  };

  let friends = {};
  let cachedStatuses = {};
  let cachedRooms = {};
  let cachedAllPresence = {};
  let currentRoomId = null;
  let currentRoomData = null;

  let hostPeerMap = new Map();
  let viewerPeer = null;
  let localStream = null;
  let localAudioStream = null;
  let audioPeerConnections = new Map();
  let candidateBuffer = [];
  let isBroadcasting = false;
  let micActive = false;
  let thumbInterval = null;

  let isMicBlockedByHost = false;
  let isChatBlockedByHost = false;
  let localHiddenChatUsers = new Set();
  let localMutedAudioUsers = new Set();

  let activeMembers = new Map();
  let presenceInitialLoaded = false;
  let heartbeatInterval = null;
  let replyingTo = null;
  let selectedMsgForContext = null;
  let isSendingMessage = false;

  let inviteTab = 'friends';
  let selectedInviteFriends = new Set();
  let dmPartner = null;
  let currentSystemBatch = { elem: null, startTime: 0, events: [] };
  let selectedEditAccess = 'public';
  let selectedCreateAccess = 'public';

  function getVideoElement() {
    return document.getElementById('roomVideo');
  }

  function updateMicButtonUI() {
    const btn = document.getElementById('btnMic');
    if (!btn) return;
    if (isMicBlockedByHost) {
      btn.className = 'btn-icon mic-blocked';
      btn.title = "Создатель отключил вам микрофон";
    } else if (micActive) {
      btn.className = 'btn-icon mic-live';
      btn.title = "Микрофон включен";
    } else {
      btn.className = 'btn-icon';
      btn.title = "Микрофон";
    }
  }

  function playPauseChime() {
    try {
      const ctx = new (window.AudioContext || window.webkitAudioContext)();
      const now = ctx.currentTime;
      const osc = ctx.createOscillator();
      const gain = ctx.createGain();
      osc.type = 'sine';
      osc.frequency.setValueAtTime(587.33, now);
      osc.frequency.exponentialRampToValueAtTime(880, now + 0.15);
      gain.gain.setValueAtTime(0.35, now);
      gain.gain.exponentialRampToValueAtTime(0.001, now + 0.6);
      osc.connect(gain);
      gain.connect(ctx.destination);
      osc.start(now);
      osc.stop(now + 0.6);
    } catch(e) {}
  }

  function showToast(text) {
    const container = document.getElementById('toastContainer');
    if (!container) return;
    const toast = document.createElement('div');
    toast.className = 'toast-card';
    toast.innerText = text;
    container.appendChild(toast);
    setTimeout(() => {
      if (toast && toast.parentNode) toast.parentNode.removeChild(toast);
    }, 3000);
  }

  function saveLocalUser() {
    localStorage.setItem('arm_user', JSON.stringify(user));
  }
  saveLocalUser();

  function initAutoPresence() {
    const userStatusRef = db.ref(`users_status/${user.username}`);
    db.ref('.info/connected').on('value', (snap) => {
      if (snap.val() === true) {
        userStatusRef.onDisconnect().set({ status: 'offline', lastSeen: Date.now() });
        userStatusRef.set({ status: 'online', lastSeen: Date.now() });
      }
    });
  }
  initAutoPresence();

  const ambCanvas = document.getElementById('ambientCanvas');
  const ambCtx = ambCanvas.getContext('2d');

  function renderAmbilight() {
    const v = getVideoElement();
    if (v && !v.paused && !v.ended && v.readyState >= 2) {
      ambCanvas.classList.add('active');
      ambCtx.drawImage(v, 0, 0, ambCanvas.width, ambCanvas.height);
    } else {
      ambCanvas.classList.remove('active');
    }
    requestAnimationFrame(renderAmbilight);
  }
  requestAnimationFrame(renderAmbilight);

  function openSettingsModal() {
    document.getElementById('settingInputName').value = user.name || '';
    document.getElementById('settingInputUsername').value = user.username || '';
    setInvitePrivacy(user.invitePrivacy || 'all');
    document.getElementById('currentQuickReactionDisplay').innerText = user.quickReaction || '❤️';
    
    const setAv = document.getElementById('settingsAvatarPreview');
    if (user.avatar) setAv.innerHTML = `<img src="${user.avatar}">`;
    else setAv.innerText = (user.name || 'U').charAt(0).toUpperCase();

    openModal('settingsModal');
  }

  function setInvitePrivacy(val) {
    user.invitePrivacy = val;
    ['all', 'friends', 'none'].forEach(k => {
      document.getElementById('privBtn_' + k).classList.toggle('active', k === val);
    });
  }

  function openQuickReactionSelector() {
    openModal('quickReactionModal');
  }

  function chooseQuickReaction(emoji) {
    user.quickReaction = emoji;
    document.getElementById('currentQuickReactionDisplay').innerText = emoji;
    closeModal('quickReactionModal');
    showToast(`Быстрая реакция: ${emoji}`);
  }

  async function saveSettings() {
    const name = document.getElementById('settingInputName').value.trim();
    const uname = document.getElementById('settingInputUsername').value.trim().replace('@', '');

    if (!uname) return showToast('Юзернейм не может быть пустым');

    if (uname !== user.username) {
      const snap = await db.ref('registered_usernames/' + uname.toLowerCase()).once('value');
      const owner = snap.val();
      if (owner && owner !== user.id) return showToast('Этот юзернейм уже занят!');
      db.ref('registered_usernames/' + user.username.toLowerCase()).remove();
      db.ref('registered_usernames/' + uname.toLowerCase()).set(user.id);
      db.ref(`users_status/${user.username}`).remove();
    }

    if (name) user.name = name;
    user.username = uname;
    saveLocalUser();
    initAutoPresence();
    closeModal('settingsModal');
    setupNotificationsListener();
    setupFriendsSync();
    showToast('Настройки сохранены!');
  }

  function uploadAvatar(e) {
    const file = e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = (ev) => {
      const img = new Image();
      img.src = ev.target.result;
      img.onload = () => {
        const canvas = document.createElement('canvas');
        canvas.width = 96;
        canvas.height = 96;
        const ctx = canvas.getContext('2d');
        ctx.drawImage(img, 0, 0, 96, 96);
        user.avatar = canvas.toDataURL('image/jpeg', 0.8);
        saveLocalUser();
        openSettingsModal();
        showToast('Аватарка обновлена!');
      };
    };
    reader.readAsDataURL(file);
  }

  function openProfileStatsModal() {
    document.getElementById('myProfileNameText').innerText = user.name;
    document.getElementById('myProfileUnameText').innerText = '@' + user.username;
    document.getElementById('statRegDate').innerText = user.registeredAt || '20.08.2026';
    
    const hours = ((user.totalSeconds || 0) / 3600).toFixed(1);
    document.getElementById('statTotalHours').innerText = `${hours} ч.`;

    const friendKeys = Object.keys(friends || {});
    document.getElementById('statFriendsCount').innerText = friendKeys.length;

    const avBig = document.getElementById('myProfileAvatarBig');
    if (user.avatar) avBig.innerHTML = `<img src="${user.avatar}">`;
    else avBig.innerText = (user.name || 'U').charAt(0).toUpperCase();

    renderActivityChart();
    openModal('profileStatsModal');
  }

  function renderActivityChart() {
    const canvas = document.getElementById('activityChartCanvas');
    if (!canvas) return;
    const ctx = canvas.getContext('2d');
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    const points = [15, 25, 18, 40, 32, 55, 45, 60, 50];
    const step = canvas.width / (points.length - 1);

    ctx.beginPath();
    ctx.moveTo(0, canvas.height - points[0]);
    for (let i = 1; i < points.length; i++) {
      ctx.lineTo(i * step, canvas.height - points[i]);
    }
    ctx.strokeStyle = '#6366f1';
    ctx.lineWidth = 3;
    ctx.stroke();

    ctx.lineTo(canvas.width, canvas.height);
    ctx.lineTo(0, canvas.height);
    ctx.closePath();
    const grad = ctx.createLinearGradient(0, 0, 0, canvas.height);
    grad.addColorStop(0, 'rgba(99, 102, 241, 0.35)');
    grad.addColorStop(1, 'rgba(99, 102, 241, 0)');
    ctx.fillStyle = grad;
    ctx.fill();
  }

  function setupFriendsSync() {
    db.ref('users_status').on('value', (snap) => {
      cachedStatuses = snap.val() || {};
      renderFriends();
    });
    db.ref(`users_friends/${user.username}`).on('value', (snap) => {
      friends = snap.val() || {};
      renderFriends();
    });
  }
  setupFriendsSync();

  function formatTimeTogether(hours) {
    if (!hours || hours < (1 / 60)) return '0 мин. вместе';
    const totalMinutes = Math.floor(hours * 60);
    if (totalMinutes < 60) return `${totalMinutes} мин. вместе`;
    return `${hours.toFixed(1)} ч. вместе`;
  }

  function renderFriends() {
    const list = document.getElementById('friendsList');
    if (!list) return;
    list.innerHTML = '';
    const uniqueKeys = Array.from(new Set(Object.keys(friends || {})));

    if (!uniqueKeys.length) {
      list.innerHTML = '<div class="empty-state-card" style="padding:14px 4px;"><span>У вас нету друзей!</span></div>';
      return;
    }

    uniqueKeys.forEach(k => {
      const f = friends[k] || {};
      const stObj = cachedStatuses[k] || {};
      const isOnline = stObj.status === 'online';
      const timeStr = formatTimeTogether(f.hours || 0);

      const item = document.createElement('div');
      item.className = 'friend-item';
      item.innerHTML = `
        <div style="display:flex; align-items:center; gap:8px;">
          <div class="friend-avatar-wrap">
            ${f.avatar ? `<img src="${f.avatar}">` : (f.name || k).charAt(0).toUpperCase()}
            <span class="status-dot-on-avatar ${isOnline ? 'online' : 'offline'}"></span>
          </div>
          <div style="display:flex; flex-direction:column;">
            <b style="font-size:0.85rem; color:#fff;">${escapeHtml(f.name || k)}</b>
            <span style="font-size:0.7rem; color:var(--text-muted);">@${escapeHtml(k)}</span>
            <span style="font-size:0.68rem; color:#a5b4fc; margin-top:1px;">${timeStr}</span>
          </div>
        </div>
        <div style="display:flex; gap:4px; align-items:center;">
          <button class="btn-icon" style="width:30px; height:30px; min-width:30px;" onclick="openDirectMessage('${k}', '${escapeHtml(f.name || k)}')" title="Написать">
            <svg viewBox="0 0 24 24"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"></path></svg>
          </button>
          <button class="btn-icon" style="width:30px; height:30px; min-width:30px;" onclick="removeFriend('${k}')" title="Удалить">
            <svg viewBox="0 0 24 24"><path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/><line x1="2" y1="22" x2="22" y2="2" stroke="#f43f5e" stroke-width="2.5" stroke-linecap="round"/></svg>
          </button>
        </div>
      `;
      list.appendChild(item);
    });
  }

  function sendFriendRequest(targetUname) {
    const uname = (targetUname || document.getElementById('newFriendUsername').value).trim().replace('@', '');
    if (!uname || uname === user.username) return showToast('Некорректный юзернейм');
    db.ref(`users_notifications/${uname}`).push({
      type: 'friend_request',
      fromUsername: user.username,
      fromName: user.name,
      time: Date.now()
    });
    showToast('Запрос в друзья отправлен!');
    closeModal('addFriendModal');
    if (currentRoomId) openParticipantsModal();
  }

  function acceptFriend(key, fromUsername, fromName) {
    db.ref(`users_friends/${user.username}/${fromUsername}`).set({ name: fromName, username: fromUsername, hours: 0 });
    db.ref(`users_friends/${fromUsername}/${user.username}`).set({ name: user.name, username: user.username, hours: 0 });
    dismissNotif(key);
    showToast(`@${fromUsername} добавлен в друзья!`);
  }

  function removeFriend(uname) {
    db.ref(`users_friends/${user.username}/${uname}`).remove();
    db.ref(`users_friends/${uname}/${user.username}`).remove();
    showToast(`@${uname} удален из друзей`);
    if (currentRoomId) openParticipantsModal();
  }

  function setupNotificationsListener() {
    db.ref(`users_notifications/${user.username}`).on('value', (snap) => {
      const notifs = snap.val() || {};
      const hasN = Object.keys(notifs).length > 0;
      document.getElementById('notifBadge').classList.toggle('active', hasN);
    });
  }
  setupNotificationsListener();

  function openNotifications() {
    const list = document.getElementById('notifList');
    list.innerHTML = '';
    db.ref(`users_notifications/${user.username}`).once('value', (snap) => {
      const notifs = snap.val() || {};
      const keys = Object.keys(notifs);
      if (!keys.length) {
        list.innerHTML = '<div style="font-size:0.82rem; color:var(--text-muted); text-align:center;">Уведомлений нет</div>';
      }
      keys.forEach(k => {
        const n = notifs[k];
        const item = document.createElement('div');
        item.style.cssText = 'background:rgba(255,255,255,0.05); padding:8px; border-radius:10px; display:flex; flex-direction:column; gap:4px; font-size:0.8rem;';
        if (n.type === 'friend_request') {
          item.innerHTML = `
            <div><b>@${escapeHtml(n.fromUsername)}</b> отправил заявку в друзья.</div>
            <div style="display:flex; justify-content:flex-end; gap:4px;">
              <button class="btn btn-secondary" style="padding:3px 6px; font-size:0.72rem;" onclick="dismissNotif('${k}')">Отклонить</button>
              <button class="btn btn-success" style="padding:3px 6px; font-size:0.72rem;" onclick="acceptFriend('${k}', '${n.fromUsername}', '${n.fromName}')">Принять</button>
            </div>
          `;
        } else if (n.type === 'room_invite') {
          item.innerHTML = `
            <div><b>@${escapeHtml(n.fromUsername)}</b> зовет в комнату "<b>${escapeHtml(n.roomName)}</b>"</div>
            <div style="display:flex; justify-content:flex-end; gap:4px;">
              <button class="btn btn-secondary" style="padding:3px 6px; font-size:0.72rem;" onclick="dismissNotif('${k}')">Закрыть</button>
              <button class="btn" style="padding:4px 8px; font-size:0.72rem;" onclick="acceptRoomInvite('${k}', '${n.roomId}')">Войти</button>
            </div>
          `;
        }
        list.appendChild(item);
      });
      openModal('notifModal');
    });
  }

  function dismissNotif(key) {
    db.ref(`users_notifications/${user.username}/${key}`).remove();
    openNotifications();
  }

  function acceptRoomInvite(key, roomId) {
    dismissNotif(key);
    closeModal('notifModal');
    db.ref('rooms_meta/' + roomId).once('value', (snap) => {
      const r = snap.val();
      if (r) enterRoom(roomId, r);
    });
  }

  function showLobby() {
    if (currentRoomId) {
      handleLeaveRoomClick();
      return;
    }
    document.getElementById('lobbyView').classList.add('active');
    document.getElementById('roomView').classList.remove('active');
    document.getElementById('btnHeaderCreate').style.display = 'inline-flex';
    document.getElementById('btnHeaderLeave').style.display = 'none';
    document.getElementById('btnRoomSettings').style.display = 'none';
  }

  function initLobbySync() {
    db.ref('rooms_presence').on('value', (snap) => {
      cachedAllPresence = snap.val() || {};
      renderLobbyRooms();
    });

    db.ref('rooms_meta').on('value', (snap) => {
      cachedRooms = snap.val() || {};
      renderLobbyRooms();
    });
  }

  function renderLobbyRooms() {
    const list = document.getElementById('roomsList');
    if (!list) return;

    const rooms = cachedRooms || {};
    const allPresence = cachedAllPresence || {};
    const rKeys = Object.keys(rooms);
    
    document.getElementById('roomsCount').innerText = `${rKeys.length} комнат`;

    if (!rKeys.length) {
      list.innerHTML = `
        <div class="empty-state-card">
          <span>Пока тут пусто =(</span>
          <button class="btn" style="margin-top:4px;" onclick="checkAndOpenCreateRoom()">Создать</button>
        </div>
      `;
      return;
    }

    const sortedKeys = rKeys.sort((a, b) => {
      const rA = rooms[a], rB = rooms[b];
      const isFriendA = friends[rA.creatorUsername] || (allPresence[a] && Object.values(allPresence[a]).some(u => friends[u.username]));
      const isFriendB = friends[rB.creatorUsername] || (allPresence[b] && Object.values(allPresence[b]).some(u => friends[u.username]));
      return (isFriendB ? 1 : 0) - (isFriendA ? 1 : 0);
    });

    list.innerHTML = '';
    sortedKeys.forEach(rid => {
      const r = rooms[rid];
      if (!r) return;

      if (r.access === 'unlisted') return;
      if (r.access === 'friends' && r.creatorUsername !== user.username && !friends[r.creatorUsername]) return;

      const badgeClass = r.access === 'public' ? 'status-badge-public' : (r.access === 'unlisted' ? 'status-badge-unlisted' : 'status-badge-friends');
      const badgeLabel = r.access === 'public' ? 'Публичный' : (r.access === 'unlisted' ? 'По ссылке' : 'Только для друзей');

      const card = document.createElement('div');
      card.className = 'room-item-liquid';

      let previewHtml = '';
      if ((r.isStreaming || r.vkVideoUrl) && r.thumb) {
        previewHtml = `
          <div class="room-card-preview">
            <div class="live-badge"><div class="live-dot-pulse"></div> LIVE</div>
            <img src="${r.thumb}" />
          </div>
        `;
      } else {
        const av = r.creatorAvatar ? `<img src="${r.creatorAvatar}">` : (r.creatorName || 'U').charAt(0).toUpperCase();
        previewHtml = `
          <div class="room-card-preview">
            <div class="avatar-box" style="width:38px; height:38px; font-size:1rem;">${av}</div>
          </div>
        `;
      }

      const roomMembers = allPresence[rid] ? Object.values(allPresence[rid]) : [];
      let membersStackHtml = '<div class="room-members-stack">';
      const maxDisplay = 4;
      for (let i = 0; i < Math.min(roomMembers.length, maxDisplay); i++) {
        const m = roomMembers[i];
        const mAv = m.avatar ? `<img src="${m.avatar}">` : (m.name || 'U').charAt(0).toUpperCase();
        membersStackHtml += `<div class="member-thumb">${mAv}</div>`;
      }
      if (roomMembers.length > maxDisplay) {
        membersStackHtml += `<div class="member-thumb-more">+${roomMembers.length - maxDisplay}</div>`;
      }
      membersStackHtml += '</div>';

      card.innerHTML = `
        ${previewHtml}
        <div class="room-card-content">
          <div class="room-card-title">${escapeHtml(r.name || 'Комната')}</div>
          <div style="display:flex; justify-content:space-between; align-items:center;">
            <span class="${badgeClass}">${badgeLabel}</span>
            ${membersStackHtml}
          </div>
        </div>
      `;
      card.onclick = () => joinRoom(rid, r.access);
      list.appendChild(card);
    });
  }
  initLobbySync();

  function setCreateAccessOption(val) {
    selectedCreateAccess = val;
    ['public', 'unlisted', 'friends'].forEach(k => {
      document.getElementById('createOptBtn_' + k).classList.toggle('active', k === val);
    });
  }

  function setRoomAccessOption(val) {
    selectedEditAccess = val;
    ['public', 'unlisted', 'friends'].forEach(k => {
      document.getElementById('optBtn_' + k).classList.toggle('active', k === val);
    });
  }

  async function checkAndOpenCreateRoom() {
    const snap = await db.ref('rooms_meta').once('value');
    const rooms = snap.val() || {};
    const existing = Object.values(rooms).find(r => r && r.creatorId === user.id);
    if (existing) {
      return showToast(`У вас уже создана комната: "${existing.name}"`);
    }
    setCreateAccessOption('public');
    openModal('createRoomModal');
  }

  function createRoom() {
    const name = document.getElementById('newRoomName').value.trim() || `Комната ${user.name}`;
    const roomId = 'room_' + Math.random().toString(36).substring(2, 9);

    const roomData = {
      id: roomId,
      name: name,
      access: selectedCreateAccess,
      creatorId: user.id,
      creatorUsername: user.username,
      creatorName: user.name,
      creatorAvatar: user.avatar || null,
      hostId: user.id,
      hostUsername: user.username,
      hostName: user.name,
      isStreaming: false,
      thumb: null,
      vkVideoUrl: null,
      mutePresenceNotifs: false,
      lastHeartbeat: Date.now()
    };

    db.ref('rooms_meta/' + roomId).set(roomData);
    closeModal('createRoomModal');
    enterRoom(roomId, roomData);
    showToast('Комната создана!');
  }

  function joinRoom(roomId, access) {
    db.ref('rooms_meta/' + roomId).once('value', (snap) => {
      const r = snap.val();
      if (r) enterRoom(roomId, r);
    });
  }

  function copyRoomLink() {
    navigator.clipboard.writeText(window.location.origin + window.location.pathname + '#' + currentRoomId).then(() => {
      showToast('Ссылка скопирована!');
    });
  }

  function openRoomSettings() {
    if (!currentRoomData || currentRoomData.creatorId !== user.id) return;
    document.getElementById('editRoomName').value = currentRoomData.name || '';
    setRoomAccessOption(currentRoomData.access || 'public');
    document.getElementById('editMutePresence').checked = !!currentRoomData.mutePresenceNotifs;
    openModal('roomSettingsModal');
  }

  function saveRoomSettings() {
    const name = document.getElementById('editRoomName').value.trim();
    const mutePresence = document.getElementById('editMutePresence').checked;

    db.ref('rooms_meta/' + currentRoomId).update({
      name: name || currentRoomData.name,
      access: selectedEditAccess,
      mutePresenceNotifs: mutePresence
    });
    closeModal('roomSettingsModal');
    showToast('Настройки сохранены!');
  }

  function enterRoom(roomId, roomData) {
    currentRoomId = roomId;
    currentRoomData = roomData;
    activeMembers.clear();
    presenceInitialLoaded = false;
    currentSystemBatch = { elem: null, startTime: 0, events: [] };

    sessionStorage.setItem('arm_active_room', roomId);
    window.location.hash = roomId;

    document.getElementById('lobbyView').classList.remove('active');
    document.getElementById('roomView').classList.add('active');

    document.getElementById('btnHeaderCreate').style.display = 'none';
    document.getElementById('btnHeaderLeave').style.display = 'inline-flex';

    updateRoomAccessBadgeUI(roomData.access || 'public');

    if (roomData.creatorId === user.id && roomData.hostId !== user.id) {
      db.ref(`rooms_meta/${roomId}`).update({
        hostId: user.id,
        hostUsername: user.username,
        hostName: user.name,
        isStreaming: false,
        thumb: null,
        vkVideoUrl: null
      });
      db.ref(`rooms_signal/${roomId}`).remove();
      roomData.hostId = user.id;
      roomData.hostUsername = user.username;
    }

    const presenceRef = db.ref(`rooms_presence/${roomId}/${user.id}`);
    presenceRef.set({
      name: user.name,
      username: user.username,
      avatar: user.avatar || null,
      joinedAt: Date.now(),
      lastSeen: Date.now()
    });
    presenceRef.onDisconnect().remove();

    if (heartbeatInterval) clearInterval(heartbeatInterval);
    heartbeatInterval = setInterval(() => {
      if (currentRoomId) {
        db.ref(`rooms_presence/${currentRoomId}/${user.id}/lastSeen`).set(Date.now());
      }
    }, 12000);

    db.ref(`rooms_kicked/${roomId}/${user.id}`).on('value', (snap) => {
      if (snap.val()) {
        showToast('Вы были исключены из комнаты');
        leaveRoom();
      }
    });

    db.ref(`rooms_meta/${roomId}/muted_mics/${user.id}`).on('value', (snap) => {
      isMicBlockedByHost = snap.val() === true;
      if (isMicBlockedByHost && micActive) toggleMicrophone();
      updateMicButtonUI();
    });

    db.ref(`rooms_meta/${roomId}/muted_chats/${user.id}`).on('value', (snap) => {
      isChatBlockedByHost = snap.val() === true;
      const chatInp = document.getElementById('chatInput');
      if (isChatBlockedByHost) {
        chatInp.disabled = true;
        chatInp.placeholder = "Создатель отключил вам ввод в чат";
      } else {
        chatInp.disabled = false;
        chatInp.placeholder = "Написать сообщение...";
      }
    });

    if (roomData.creatorId === user.id) {
      db.ref(`rooms_meta/${roomId}/pause_requests`).on('child_added', (snap) => {
        const req = snap.val();
        if (req && req.userId !== user.id) {
          playPauseChime();
          showToast(`🔔 @${req.username} попросил поставить видео на паузу!`);
        }
      });
    }

    setupPresenceTracker();
    setupChat();
    setupRoomDataListener();
    setupAudioMeshSignaling();

    handleUserPresenceEvent('joined', { name: user.name, avatar: user.avatar }, user.id);

    if (roomData.hostId === user.id) {
      setupHostMode();
    } else {
      setupViewerMode();
    }
  }

  function updateRoomAccessBadgeUI(acc) {
    const badge = document.getElementById('roomAccessBadge');
    badge.className = acc === 'public' ? 'status-badge-public' : (acc === 'unlisted' ? 'status-badge-unlisted' : 'status-badge-friends');
    badge.innerText = acc === 'public' ? 'Публичный' : (acc === 'unlisted' ? 'По ссылке' : 'Только друзьям');
  }

  function setupHostMode() {
    document.getElementById('btnStreamToggle').style.display = 'inline-flex';
    document.getElementById('btnChooseVk').style.display = 'inline-flex';
    document.getElementById('btnRequestPause').style.display = 'none';
    document.getElementById('streamLoader').style.display = 'none';
  }

  function setupViewerMode() {
    document.getElementById('btnStreamToggle').style.display = 'none';
    document.getElementById('btnChooseVk').style.display = 'none';
    document.getElementById('btnRequestPause').style.display = 'inline-flex';
    
    const loader = document.getElementById('streamLoader');
    if (currentRoomData && currentRoomData.thumb) {
      loader.style.backgroundImage = `url(${currentRoomData.thumb})`;
    } else {
      loader.style.backgroundImage = 'none';
    }

    if (currentRoomData && (currentRoomData.isStreaming || currentRoomData.vkVideoUrl)) {
      renderActiveMedia(currentRoomData);
    } else {
      loader.style.display = 'flex';
    }
  }

  function setupRoomDataListener() {
    db.ref('rooms_meta/' + currentRoomId).on('value', (snap) => {
      const r = snap.val();
      if (!r) return;

      const prevHost = currentRoomData ? currentRoomData.hostUsername : null;
      currentRoomData = r;

      updateRoomAccessBadgeUI(r.access || 'public');

      const isMeHost = r.hostId === user.id;
      const isMeCreator = r.creatorId === user.id;

      if (prevHost && prevHost !== r.hostUsername) {
        const newHostMember = Array.from(activeMembers.values()).find(m => m.username === r.hostUsername);
        const av = newHostMember && newHostMember.avatar ? newHostMember.avatar : null;
        showOwnerNotification(r.hostName || r.hostUsername, av);
      }

      if (!isMeHost && isBroadcasting) {
        stopBroadcasting();
        showToast('Права хоста изменились.');
      }

      document.getElementById('btnRoomSettings').style.display = isMeCreator ? 'inline-flex' : 'none';
      document.getElementById('btnRequestPause').style.display = (isMeCreator || isMeHost) ? 'none' : 'inline-flex';

      const loader = document.getElementById('streamLoader');
      if (r.thumb) {
        loader.style.backgroundImage = `url(${r.thumb})`;
      }

      if (isMeHost) {
        setupHostMode();
      } else {
        renderActiveMedia(r);
      }
    });
  }

  function renderActiveMedia(r) {
    const target = document.getElementById('mediaTarget');
    const loader = document.getElementById('streamLoader');

    if (r.vkVideoUrl) {
      loader.style.display = 'none';
      if (!target.innerHTML.includes('iframe') || !target.innerHTML.includes(r.vkVideoUrl)) {
        target.innerHTML = `<iframe src="${r.vkVideoUrl}" allow="autoplay; encrypted-media; fullscreen; picture-in-picture;" allowfullscreen></iframe>`;
      }
    } else if (r.isStreaming) {
      listenForBroadcast();
    } else {
      loader.style.display = 'flex';
      stopWebRTC();
      target.innerHTML = `<video id="roomVideo" autoplay playsinline controls></video>`;
    }
  }

  function openVkVideoModal() {
    openModal('vkVideoModal');
  }

  function launchVkVideoByHost() {
    let raw = document.getElementById('vkVideoInputUrl').value.trim();
    if (!raw) return showToast('Введите ссылку на ВК Видео');

    let embedUrl = raw;

    if (raw.includes('<iframe') && raw.includes('src="')) {
      const match = raw.match(/src=["']([^"']+)["']/);
      if (match && match[1]) embedUrl = match[1];
    } else if (raw.includes('/video-') || raw.includes('/video')) {
      const match = raw.match(/video(-?\d+)_(\d+)/);
      if (match) {
        embedUrl = `https://vkvideo.ru/video_ext.php?oid=${match[1]}&id=${match[2]}&autoplay=1`;
      }
    }

    if (!embedUrl.includes('autoplay=1')) {
      embedUrl += (embedUrl.includes('?') ? '&' : '?') + 'autoplay=1';
    }

    closeModal('vkVideoModal');
    stopWebRTC();

    db.ref(`rooms_meta/${currentRoomId}`).update({
      vkVideoUrl: embedUrl,
      isStreaming: false,
      thumb: 'https://images.unsplash.com/photo-1574375927938-d5a98e8ffe85?w=500&auto=format&fit=crop&q=60'
    });

    renderActiveMedia({ vkVideoUrl: embedUrl });
    showToast('ВК Видео запущено для всех!');
  }

  function setupPresenceTracker() {
    const presRef = db.ref(`rooms_presence/${currentRoomId}`);

    presRef.once('value', (snap) => {
      const val = snap.val() || {};
      Object.keys(val).forEach(uid => activeMembers.set(uid, val[uid]));
      presenceInitialLoaded = true;
      updateParticipantsCounter();
      evaluateCrownOwnership();
    });

    presRef.on('child_added', (snap) => {
      const u = snap.val();
      const uid = snap.key;
      if (!activeMembers.has(uid)) {
        activeMembers.set(uid, u);
        updateParticipantsCounter();
        if (presenceInitialLoaded && uid !== user.id && (!currentRoomData || !currentRoomData.mutePresenceNotifs)) {
          handleUserPresenceEvent('joined', u, uid);
          evaluateCrownOwnership();
        }
      }
    });

    presRef.on('child_removed', (snap) => {
      const uid = snap.key;
      if (activeMembers.has(uid)) {
        const u = activeMembers.get(uid);
        activeMembers.delete(uid);
        updateParticipantsCounter();
        if (presenceInitialLoaded && (!currentRoomData || !currentRoomData.mutePresenceNotifs)) {
          handleUserPresenceEvent('left', u, uid);
          evaluateCrownOwnership();
        }
      }
      checkEmptyRoomAutoDelete();
    });
  }

  function checkEmptyRoomAutoDelete() {
    if (!currentRoomId) return;
    db.ref(`rooms_presence/${currentRoomId}`).once('value', (snap) => {
      const members = snap.val();
      if (!members || Object.keys(members).length === 0) {
        db.ref(`rooms_meta/${currentRoomId}`).remove();
        db.ref(`rooms_presence/${currentRoomId}`).remove();
        db.ref(`rooms_signal/${currentRoomId}`).remove();
        db.ref(`chats/${currentRoomId}`).remove();
      }
    });
  }

  function updateParticipantsCounter() {
    const countElem = document.getElementById('participantsCounterText');
    if (countElem) countElem.innerText = activeMembers.size;
  }

  function handleUserPresenceEvent(type, userObj, uid) {
    const now = Date.now();
    const isExpired = (now - currentSystemBatch.startTime > 60000);

    if (!currentSystemBatch.elem || isExpired) {
      createNewSystemBatch(type, userObj, uid);
    } else {
      const existingIdx = currentSystemBatch.events.findIndex(e => e.uid === uid);
      if (existingIdx !== -1) {
        const prevAction = currentSystemBatch.events[existingIdx].action;
        if (prevAction === 'joined' && type === 'left') {
          currentSystemBatch.events[existingIdx].action = 'both';
        } else if (prevAction === 'both' && type === 'joined') {
          currentSystemBatch.events[existingIdx].action = 'joined';
        }
      } else {
        if (currentSystemBatch.events.length < 4) {
          currentSystemBatch.events.push({ uid, name: userObj.name, avatar: userObj.avatar, action: type });
        } else {
          createNewSystemBatch(type, userObj, uid);
          return;
        }
      }
      renderSystemBatchHtml(currentSystemBatch);
    }
  }

  function createNewSystemBatch(type, userObj, uid) {
    const container = document.getElementById('chatMessages');
    const wrap = document.createElement('div');
    wrap.className = 'system-batch-wrap';
    container.appendChild(wrap);

    currentSystemBatch = {
      elem: wrap,
      startTime: Date.now(),
      events: [{ uid, name: userObj.name, avatar: userObj.avatar, action: type }]
    };

    renderSystemBatchHtml(currentSystemBatch);
    container.scrollTop = container.scrollHeight;
  }

  function renderSystemBatchHtml(batch) {
    if (!batch.elem) return;

    const joinedList = batch.events.filter(e => e.action === 'joined');
    const leftList = batch.events.filter(e => e.action === 'left');
    const bothList = batch.events.filter(e => e.action === 'both');

    let parts = [];

    const makeCapsule = (e) => {
      const av = e.avatar ? `<img src="${e.avatar}">` : (e.name || 'U').charAt(0).toUpperCase();
      const displayName = (e.uid === user.id) ? `${escapeHtml(e.name)} (Вы)` : escapeHtml(e.name);
      return `
        <span class="user-capsule" onclick="openUserProfile('${e.uid}', '${escapeHtml(e.name)}', '', '${e.avatar || ''}')">
          <span class="capsule-avatar">${av}</span>
          <span class="capsule-name">${displayName}</span>
        </span>
      `;
    };

    if (joinedList.length > 0) {
      const capsules = joinedList.map(makeCapsule).join(' ');
      const verb = joinedList.length > 1 ? 'присоединились' : 'присоединился';
      parts.push(`${capsules} ${verb}`);
    }

    if (leftList.length > 0) {
      const capsules = leftList.map(makeCapsule).join(' ');
      const verb = leftList.length > 1 ? 'вышли' : 'вышел';
      parts.push(`${capsules} ${verb}`);
    }

    if (bothList.length > 0) {
      const capsules = bothList.map(makeCapsule).join(' ');
      const verb = bothList.length > 1 ? 'присоединились и вышли' : 'присоединился и вышел';
      parts.push(`${capsules} ${verb}`);
    }

    batch.elem.innerHTML = parts.join(', ');
  }

  function evaluateCrownOwnership() {
    if (!currentRoomData || !currentRoomId) return;

    const creatorInRoom = activeMembers.has(currentRoomData.creatorId);

    if (creatorInRoom && currentRoomData.hostId !== currentRoomData.creatorId) {
      const crUser = activeMembers.get(currentRoomData.creatorId);
      db.ref('rooms_meta/' + currentRoomId).update({
        hostId: currentRoomData.creatorId,
        hostUsername: crUser.username,
        hostName: crUser.name
      });
      return;
    }

    if (!creatorInRoom && !activeMembers.has(currentRoomData.hostId)) {
      let earliestUser = null;
      let earliestUid = null;
      let earliestTime = Infinity;

      activeMembers.forEach((u, uid) => {
        if ((u.joinedAt || 0) < earliestTime) {
          earliestTime = u.joinedAt || 0;
          earliestUser = u;
          earliestUid = uid;
        }
      });

      if (earliestUid) {
        db.ref(`rooms_meta/${currentRoomId}/muted_mics/${earliestUid}`).remove();
        db.ref(`rooms_meta/${currentRoomId}/muted_chats/${earliestUid}`).remove();

        db.ref('rooms_meta/' + currentRoomId).update({
          hostId: earliestUid,
          hostUsername: earliestUser.username,
          hostName: earliestUser.name
        });
      }
    }
  }

  async function toggleMicrophone() {
    if (isMicBlockedByHost) {
      return showToast('Создатель отключил вам микрофон');
    }

    micActive = !micActive;
    updateMicButtonUI();

    if (micActive) {
      try {
        localAudioStream = await navigator.mediaDevices.getUserMedia({ audio: true });
        showToast('Микрофон включен');
        broadcastAudioToRoom();
      } catch(err) {
        micActive = false;
        updateMicButtonUI();
        showToast('Не удалось получить доступ к микрофону');
      }
    } else {
      if (localAudioStream) {
        localAudioStream.getTracks().forEach(t => t.stop());
        localAudioStream = null;
      }
      showToast('Микрофон выключен');
    }
  }

  function setupAudioMeshSignaling() {
    if (!currentRoomId) return;
    const audRef = db.ref(`rooms_voice/${currentRoomId}/${user.id}`);
    audRef.on('child_added', async (snap) => {
      const fromUid = snap.key;
      const data = snap.val();
      if (!data) return;

      let pc = audioPeerConnections.get(fromUid);
      if (!pc) {
        pc = new RTCPeerConnection(rtcConfig);
        audioPeerConnections.set(fromUid, pc);

        if (localAudioStream) {
          localAudioStream.getTracks().forEach(t => pc.addTrack(t, localAudioStream));
        }

        pc.ontrack = (e) => {
          let audElem = document.getElementById(`remote_audio_${fromUid}`);
          if (!audElem) {
            audElem = document.createElement('audio');
            audElem.id = `remote_audio_${fromUid}`;
            audElem.autoplay = true;
            document.getElementById('remoteAudiosContainer').appendChild(audElem);
          }
          audElem.srcObject = e.streams[0];
          audElem.muted = localMutedAudioUsers.has(fromUid);
        };

        pc.onicecandidate = (ev) => {
          if (ev.candidate) {
            db.ref(`rooms_voice/${currentRoomId}/${fromUid}/${user.id}/candidates`).push(ev.candidate.toJSON());
          }
        };
      }

      if (data.offer && pc.signalingState !== 'stable') {
        await pc.setRemoteDescription(new RTCSessionDescription(data.offer));
        const ans = await pc.createAnswer();
        await pc.setLocalDescription(ans);
        db.ref(`rooms_voice/${currentRoomId}/${fromUid}/${user.id}/answer`).set({ sdp: ans.sdp, type: ans.type });
      } else if (data.answer && pc.signalingState === 'have-local-offer') {
        await pc.setRemoteDescription(new RTCSessionDescription(data.answer));
      }
    });
  }

  async function broadcastAudioToRoom() {
    if (!currentRoomId || !localAudioStream) return;
    activeMembers.forEach(async (u, uid) => {
      if (uid === user.id) return;
      let pc = new RTCPeerConnection(rtcConfig);
      audioPeerConnections.set(uid, pc);

      localAudioStream.getTracks().forEach(t => pc.addTrack(t, localAudioStream));

      pc.ontrack = (e) => {
        let audElem = document.getElementById(`remote_audio_${uid}`);
        if (!audElem) {
          audElem = document.createElement('audio');
          audElem.id = `remote_audio_${uid}`;
          audElem.autoplay = true;
          document.getElementById('remoteAudiosContainer').appendChild(audElem);
        }
        audElem.srcObject = e.streams[0];
        audElem.muted = localMutedAudioUsers.has(uid);
      };

      pc.onicecandidate = (ev) => {
        if (ev.candidate) {
          db.ref(`rooms_voice/${currentRoomId}/${uid}/${user.id}/candidates`).push(ev.candidate.toJSON());
        }
      };

      const off = await pc.createOffer();
      await pc.setLocalDescription(off);
      db.ref(`rooms_voice/${currentRoomId}/${uid}/${user.id}/offer`).set({ sdp: off.sdp, type: off.type });
    });
  }

  async function toggleScreenBroadcast() {
    if (isBroadcasting) {
      stopBroadcasting();
      document.getElementById('streamIconPlay').innerHTML = '<polygon points="5 3 19 12 5 21 5 3"></polygon>';
      return;
    }

    stopWebRTC();
    candidateBuffer = [];
    isBroadcasting = true;

    try {
      localStream = await navigator.mediaDevices.getDisplayMedia({ video: true, audio: true });
    } catch (e) {
      try {
        localStream = await navigator.mediaDevices.getDisplayMedia({ video: true });
      } catch (err) {
        try {
          localStream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
        } catch(finalErr) {
          isBroadcasting = false;
          return showToast('Не удалось получить доступ к экрану');
        }
      }
    }

    const v = getVideoElement();
    if (v) {
      v.srcObject = localStream;
      v.muted = true;
      v.play().catch(() => {});
    }

    document.getElementById('streamIconPlay').innerHTML = '<rect x="6" y="6" width="12" height="12"></rect>';
    showToast('Трансляция запущена!');

    await db.ref(`rooms_signal/${currentRoomId}`).remove();
    await db.ref(`rooms_meta/${currentRoomId}`).update({
      isStreaming: true,
      vkVideoUrl: null
    });

    startThumbnailBroadcast();

    const reqRef = db.ref(`rooms_signal/${currentRoomId}/requests`);
    reqRef.on('child_added', (snap) => {
      const viewerId = snap.key;
      if (viewerId && viewerId !== user.id && isBroadcasting) {
        initHostPeerForViewer(viewerId);
      }
    });
  }

  async function initHostPeerForViewer(viewerId) {
    if (hostPeerMap.has(viewerId)) {
      try { hostPeerMap.get(viewerId).close(); } catch(e) {}
      hostPeerMap.delete(viewerId);
    }

    const pc = new RTCPeerConnection(rtcConfig);
    hostPeerMap.set(viewerId, pc);

    if (localStream) {
      localStream.getTracks().forEach(t => pc.addTrack(t, localStream));
    }

    pc.onicecandidate = (e) => {
      if (e.candidate) {
        db.ref(`rooms_signal/${currentRoomId}/sessions/${viewerId}/hostCandidates`).push(e.candidate.toJSON());
      }
    };

    const offer = await pc.createOffer();
    await pc.setLocalDescription(offer);
    await db.ref(`rooms_signal/${currentRoomId}/sessions/${viewerId}/offer`).set({
      sdp: offer.sdp,
      type: offer.type
    });

    db.ref(`rooms_signal/${currentRoomId}/sessions/${viewerId}/answer`).on('value', async (snap) => {
      const data = snap.val();
      if (data && pc.signalingState === 'have-local-offer') {
        try {
          await pc.setRemoteDescription(new RTCSessionDescription(data));
        } catch(err) {}
      }
    });

    db.ref(`rooms_signal/${currentRoomId}/sessions/${viewerId}/viewerCandidates`).on('child_added', (snap) => {
      const cand = snap.val();
      if (cand && pc.remoteDescription) {
        pc.addIceCandidate(new RTCIceCandidate(cand)).catch(() => {});
      }
    });
  }

  function listenForBroadcast() {
    if (viewerPeer || (currentRoomData && currentRoomData.hostId === user.id)) return;

    candidateBuffer = [];
    viewerPeer = new RTCPeerConnection(rtcConfig);

    viewerPeer.addTransceiver('video', { direction: 'recvonly' });
    viewerPeer.addTransceiver('audio', { direction: 'recvonly' });

    viewerPeer.ontrack = (e) => {
      document.getElementById('streamLoader').style.display = 'none';
      const v = getVideoElement();
      if (v) {
        if (e.streams && e.streams[0]) {
          v.srcObject = e.streams[0];
        } else {
          const ms = new MediaStream();
          ms.addTrack(e.track);
          v.srcObject = ms;
        }
        v.play().catch(() => {});
      }
    };

    viewerPeer.onicecandidate = (e) => {
      if (e.candidate) {
        db.ref(`rooms_signal/${currentRoomId}/sessions/${user.id}/viewerCandidates`).push(e.candidate.toJSON());
      }
    };

    db.ref(`rooms_signal/${currentRoomId}/sessions/${user.id}`).remove();
    db.ref(`rooms_signal/${currentRoomId}/requests/${user.id}`).set({ reqTime: Date.now(), token: Math.random() });

    db.ref(`rooms_signal/${currentRoomId}/sessions/${user.id}/offer`).on('value', async (snap) => {
      const offer = snap.val();
      if (!offer || !viewerPeer) return;

      try {
        await viewerPeer.setRemoteDescription(new RTCSessionDescription(offer));
        const answer = await viewerPeer.createAnswer();
        await viewerPeer.setLocalDescription(answer);

        await db.ref(`rooms_signal/${currentRoomId}/sessions/${user.id}/answer`).set({
          sdp: answer.sdp,
          type: answer.type
        });

        while (candidateBuffer.length) {
          viewerPeer.addIceCandidate(new RTCIceCandidate(candidateBuffer.shift())).catch(() => {});
        }
      } catch(err) {}
    });

    db.ref(`rooms_signal/${currentRoomId}/sessions/${user.id}/hostCandidates`).on('child_added', (snap) => {
      const c = snap.val();
      if (c && viewerPeer && viewerPeer.remoteDescription) {
        viewerPeer.addIceCandidate(new RTCIceCandidate(c)).catch(() => {});
      } else if (c) {
        candidateBuffer.push(c);
      }
    });
  }

  function startThumbnailBroadcast() {
    if (thumbInterval) clearInterval(thumbInterval);
    const canvas = document.createElement('canvas');
    canvas.width = 160;
    canvas.height = 90;
    const ctx = canvas.getContext('2d');

    const updateSnapshot = () => {
      const v = getVideoElement();
      if (!isBroadcasting || !v || v.paused || v.ended) return;
      try {
        ctx.drawImage(v, 0, 0, 160, 90);
        const thumbUrl = canvas.toDataURL('image/jpeg', 0.5);
        if (currentRoomId) {
          db.ref(`rooms_meta/${currentRoomId}/thumb`).set(thumbUrl);
        }
      } catch(e) {}
    };

    setTimeout(updateSnapshot, 2000);
    thumbInterval = setInterval(updateSnapshot, 5 * 60 * 1000);
  }

  function stopBroadcasting() {
    isBroadcasting = false;
    if (thumbInterval) clearInterval(thumbInterval);
    if (currentRoomId) {
      db.ref(`rooms_meta/${currentRoomId}`).update({
        isStreaming: false,
        thumb: null,
        vkVideoUrl: null
      });
    }
    hostPeerMap.forEach(pc => {
      try { pc.close(); } catch(e) {}
    });
    hostPeerMap.clear();
    stopWebRTC();
  }

  function stopWebRTC() {
    if (localStream) {
      localStream.getTracks().forEach(t => {
        try { t.stop(); } catch(e) {}
      });
      localStream = null;
    }
    if (hostPeerMap && hostPeerMap.size > 0) {
      hostPeerMap.forEach(pc => {
        try { pc.close(); } catch(e) {}
      });
      hostPeerMap.clear();
    }
    if (viewerPeer) {
      try { viewerPeer.close(); } catch(e) {}
      viewerPeer = null;
    }
    const v = getVideoElement();
    if (v) v.srcObject = null;
  }

  function sendPauseRequest() {
    if (!currentRoomId) return;
    db.ref(`rooms_meta/${currentRoomId}/pause_requests`).push({
      userId: user.id,
      username: user.username,
      name: user.name,
      time: Date.now()
    });
    showToast('Запрос о паузе отправлен создателю!');
  }

  function openParticipantsModal() {
    const list = document.getElementById('participantsList');
    if (!list) return;
    list.innerHTML = '';
    const isMeHost = currentRoomData && currentRoomData.hostId === user.id;

    const membersArr = Array.from(activeMembers.entries()).map(([uid, u]) => ({ uid, ...u }));
    membersArr.sort((a, b) => {
      if (a.uid === currentRoomData.creatorId) return -1;
      if (b.uid === currentRoomData.creatorId) return 1;
      return (a.joinedAt || 0) - (b.joinedAt || 0);
    });

    membersArr.forEach(u => {
      const uid = u.uid;
      const item = document.createElement('div');
      item.className = 'friend-item';

      const isMe = uid === user.id;
      const isTargetCreator = currentRoomData && currentRoomData.creatorId === uid;
      const isTargetHost = currentRoomData && currentRoomData.hostId === uid;
      const isFriend = !!friends[u.username];

      let actionButtons = '<div class="user-actions-row">';

      if (!isMe) {
        const isMutedAudio = localMutedAudioUsers.has(uid);
        actionButtons += `
          <button class="action-sub-btn ${isMutedAudio ? 'active-muted' : ''}" onclick="toggleAudioMute('${uid}', '${escapeHtml(u.name)}')" title="Отключить микрофон">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M12 1a3 3 0 0 0-3 3v8a3 3 0 0 0 6 0V4a3 3 0 0 0-3-3z"/>
              <path d="M19 10v2a7 7 0 0 1-14 0v-2"/>
              <line x1="2" y1="2" x2="22" y2="22" stroke="#f43f5e" stroke-width="2.5"/>
            </svg>
          </button>
        `;

        const isChatMuted = localHiddenChatUsers.has(uid);
        actionButtons += `
          <button class="action-sub-btn ${isChatMuted ? 'active-muted' : ''}" onclick="toggleChatMute('${uid}', '${escapeHtml(u.name)}')" title="Скрыть чат">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"/>
              ${isChatMuted ? '<line x1="3" y1="3" x2="21" y2="21" stroke="#f43f5e" stroke-width="2.5"/>' : ''}
            </svg>
          </button>
        `;

        if (isFriend) {
          actionButtons += `
            <button class="action-sub-btn" onclick="removeFriend('${u.username}')" title="Удалить из друзей">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/>
                <line x1="2" y1="22" x2="22" y2="2" stroke="#f43f5e" stroke-width="2.5" stroke-linecap="round"/>
              </svg>
            </button>
          `;
        } else {
          actionButtons += `
            <button class="action-sub-btn" onclick="sendFriendRequest('${u.username}')" title="Добавить в друзья">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M16 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="8.5" cy="7" r="4"/>
                <line x1="20" y1="8" x2="20" y2="14" stroke="#10b981" stroke-width="2.5"/>
                <line x1="23" y1="11" x2="17" y2="11" stroke="#10b981" stroke-width="2.5"/>
              </svg>
            </button>
          `;
        }

        if (isMeHost) {
          actionButtons += `
            <button class="action-sub-btn" title="Передать корону" onclick="transferHostTo('${uid}', '${u.username}', '${u.name}')">👑</button>
            <button class="action-sub-btn active-muted" title="Исключить" onclick="kickUser('${uid}')">✕</button>
          `;
        }
      }

      actionButtons += '</div>';

      item.innerHTML = `
        <div style="display:flex; align-items:center; gap:10px; cursor:pointer;" onclick="openUserProfile('${uid}', '${escapeHtml(u.name)}', '@${escapeHtml(u.username)}', '${u.avatar || ''}')">
          <div class="avatar-box" style="width:34px; height:34px; font-size:0.8rem; ${isTargetCreator ? 'border-color:var(--accent-gold);' : ''}">
            ${isTargetHost ? CROWN_SVG : ''}
            ${u.avatar ? `<img src="${u.avatar}">` : (u.name || 'U').charAt(0).toUpperCase()}
          </div>
          <div>
            <b>${escapeHtml(u.name)}</b> <span style="color:var(--text-muted); font-size:0.75rem;">@${escapeHtml(u.username)}</span>
          </div>
        </div>
        ${actionButtons}
      `;
      list.appendChild(item);
    });

    openModal('participantsModal');
  }

  function toggleAudioMute(uid, name) {
    const isMeHost = currentRoomData && currentRoomData.hostId === user.id;
    if (isMeHost) {
      db.ref(`rooms_meta/${currentRoomId}/muted_mics/${uid}`).once('value', (snap) => {
        const nextState = !(snap.val() === true);
        db.ref(`rooms_meta/${currentRoomId}/muted_mics/${uid}`).set(nextState);
        showToast(nextState ? `Микрофон пользователя ${name} отключен` : `Микрофон пользователя ${name} включен`);
        openParticipantsModal();
      });
    } else {
      if (localMutedAudioUsers.has(uid)) localMutedAudioUsers.delete(uid);
      else localMutedAudioUsers.add(uid);
      showToast(localMutedAudioUsers.has(uid) ? `Звук от ${name} заглушен` : `Звук от ${name} включен`);
      openParticipantsModal();
    }
  }

  function toggleChatMute(uid, name) {
    const isMeHost = currentRoomData && currentRoomData.hostId === user.id;
    if (isMeHost) {
      db.ref(`rooms_meta/${currentRoomId}/muted_chats/${uid}`).once('value', (snap) => {
        const nextState = !(snap.val() === true);
        db.ref(`rooms_meta/${currentRoomId}/muted_chats/${uid}`).set(nextState);
        showToast(nextState ? `Чат для ${name} отключен` : `Чат для ${name} включен`);
        openParticipantsModal();
      });
    } else {
      if (localHiddenChatUsers.has(uid)) localHiddenChatUsers.delete(uid);
      else localHiddenChatUsers.add(uid);
      showToast(localHiddenChatUsers.has(uid) ? `Сообщения от ${name} скрыты` : `Сообщения от ${name} отображаются`);
      openParticipantsModal();
    }
  }

  function kickUser(uid) {
    db.ref(`rooms_kicked/${currentRoomId}/${uid}`).set(true);
    openParticipantsModal();
    showToast('Участник исключен');
  }

  function transferHostTo(uid, uname, name) {
    const updates = {
      creatorId: uid,
      creatorUsername: uname,
      creatorName: name,
      hostId: uid,
      hostUsername: uname,
      hostName: name,
      isStreaming: false,
      thumb: null,
      vkVideoUrl: null
    };
    db.ref(`rooms_signal/${currentRoomId}`).remove();
    db.ref(`rooms_meta/${currentRoomId}/muted_mics/${uid}`).remove();
    db.ref(`rooms_meta/${currentRoomId}/muted_chats/${uid}`).remove();
    db.ref('rooms_meta/' + currentRoomId).update(updates);
    showToast(`Права создателя переданы @${uname}`);
    closeModal('participantsModal');
  }

  function handleLeaveRoomClick() {
    if (currentRoomData && currentRoomData.creatorId === user.id) {
      openModal('hostLeaveModal');
    } else {
      leaveRoom();
    }
  }

  function confirmDeleteRoomByHost() {
    closeModal('hostLeaveModal');
    if (currentRoomId) {
      db.ref(`rooms_meta/${currentRoomId}`).remove();
      db.ref(`rooms_presence/${currentRoomId}`).remove();
      db.ref(`rooms_signal/${currentRoomId}`).remove();
      db.ref(`chats/${currentRoomId}`).remove();
    }
    leaveRoom();
    showToast('Комната удалена');
  }

  function confirmLeaveAndPassHost() {
    closeModal('hostLeaveModal');
    leaveRoom();
  }

  function leaveRoom() {
    if (heartbeatInterval) clearInterval(heartbeatInterval);
    stopWebRTC();

    if (localAudioStream) {
      localAudioStream.getTracks().forEach(t => t.stop());
      localAudioStream = null;
    }
    audioPeerConnections.forEach(pc => {
      try { pc.close(); } catch(e) {}
    });
    audioPeerConnections.clear();

    sessionStorage.removeItem('arm_active_room');
    history.replaceState(null, null, ' ');

    if (currentRoomId) {
      db.ref(`rooms_presence/${currentRoomId}/${user.id}`).remove();
      checkEmptyRoomAutoDelete();
    }

    currentRoomId = null;
    currentRoomData = null;

    document.getElementById('lobbyView').classList.add('active');
    document.getElementById('roomView').classList.remove('active');
    document.getElementById('btnHeaderCreate').style.display = 'inline-flex';
    document.getElementById('btnHeaderLeave').style.display = 'none';
    document.getElementById('btnRoomSettings').style.display = 'none';
  }

  function setupChat() {
    const msgContainer = document.getElementById('chatMessages');
    msgContainer.innerHTML = '';

    db.ref('chats/' + currentRoomId).limitToLast(60).off();
    db.ref('chats/' + currentRoomId).limitToLast(60).on('child_added', (snap) => {
      const m = snap.val();
      const msgId = snap.key;
      if (!m) return;

      if (localHiddenChatUsers.has(m.userId)) return;

      currentSystemBatch = { elem: null, startTime: 0, events: [] };

      const isSelf = m.userId === user.id;
      const isHost = currentRoomData && (m.userId === currentRoomData.hostId);
      const row = document.createElement('div');
      row.className = `msg-row ${isSelf ? 'self' : 'other'}`;
      row.id = `msg_${msgId}`;

      let imgHtml = m.image ? `<img src="${m.image}" class="msg-img" onclick="window.open('${m.image}')">` : '';

      let quoteHtml = '';
      if (m.replyQuote) {
        const qAvatar = m.replyQuote.avatar ? `<img src="${m.replyQuote.avatar}">` : (m.replyQuote.sender || 'U').charAt(0).toUpperCase();
        quoteHtml = `
          <div class="msg-quote-preview">
            <div class="msg-quote-avatar">${qAvatar}</div>
            <div class="msg-quote-text"><b>${escapeHtml(m.replyQuote.sender)}:</b> ${escapeHtml(m.replyQuote.text)}</div>
          </div>
        `;
      }

      let reactionsHtml = `<div class="msg-reactions-wrap" id="reactWrap_${msgId}"></div>`;

      const metaBarHtml = `
        <div class="msg-meta-bar">
          <button class="msg-action-btn" title="Ответить" onclick="startReply('${escapeHtml(m.sender)}', '${escapeHtml(m.text || 'Фото')}', '${m.avatar || ''}')">
            ${REPLY_ICON_SVG}
          </button>
          <button class="msg-action-btn" title="Реакция" onclick="openMsgReactionPicker('${msgId}', '${escapeHtml(m.text || '[Фото]')}', '${m.time || ''}')">
            ${REACT_ICON_SVG}
          </button>
          <span>${m.time || ''}</span>
        </div>
      `;

      if (isSelf) {
        row.innerHTML = `
          <div class="msg-body">
            ${quoteHtml}
            <div class="msg-inline-content">
              <span class="msg-text-plain">${escapeHtml(m.text || '')}</span>
            </div>
            ${imgHtml}
            ${reactionsHtml}
            ${metaBarHtml}
          </div>
          <div class="avatar-box" onclick="openUserProfile('${m.userId}', '${escapeHtml(m.sender)}', '', '${m.avatar || ''}')">
            ${isHost ? CROWN_SVG : ''}
            ${m.avatar ? `<img src="${m.avatar}">` : (m.sender || 'U').charAt(0).toUpperCase()}
          </div>
        `;
      } else {
        row.innerHTML = `
          <div class="avatar-box" onclick="openUserProfile('${m.userId}', '${escapeHtml(m.sender)}', '', '${m.avatar || ''}')">
            ${isHost ? CROWN_SVG : ''}
            ${m.avatar ? `<img src="${m.avatar}">` : (m.sender || 'U').charAt(0).toUpperCase()}
          </div>
          <div class="msg-body">
            ${quoteHtml}
            <div class="msg-inline-content">
              <span class="msg-sender-bold">${escapeHtml(m.sender)}:</span>
              <span class="msg-text-plain">${escapeHtml(m.text || '')}</span>
            </div>
            ${imgHtml}
            ${reactionsHtml}
            ${metaBarHtml}
          </div>
        `;
      }

      row.addEventListener('dblclick', () => {
        toggleReaction(msgId, user.quickReaction || '❤️');
      });

      setupTouchGestures(row, msgId, m);
      listenReactions(msgId);

      msgContainer.appendChild(row);
      msgContainer.scrollTop = msgContainer.scrollHeight;
    });
  }

  function setupTouchGestures(elem, msgId, m) {
    let startX = 0, startY = 0, isSwiping = false, isScrolling = false, pressTimer = null, lastTap = 0;

    elem.addEventListener('touchstart', (e) => {
      startX = e.touches[0].clientX;
      startY = e.touches[0].clientY;
      isSwiping = false;
      isScrolling = false;

      const now = Date.now();
      if (now - lastTap < 300) {
        clearTimeout(pressTimer);
        elem.classList.remove('pressing');
        toggleReaction(msgId, user.quickReaction || '❤️');
        lastTap = 0;
        return;
      }
      lastTap = now;

      pressTimer = setTimeout(() => {
        if (!isScrolling && !isSwiping) {
          elem.classList.add('pressing');
          setTimeout(() => {
            elem.classList.remove('pressing');
            selectedMsgForContext = { msgId: msgId, text: m.text, sender: m.sender, avatar: m.avatar };
            document.getElementById('ctxMsgPreview').innerText = m.text || '[Фото]';
            document.getElementById('ctxMsgTime').innerText = m.time ? `Отправлено в ${m.time}` : '';
            document.getElementById('contextMenu').classList.add('open');
          }, 180);
        }
      }, 450);
    }, { passive: true });

    elem.addEventListener('touchmove', (e) => {
      const diffX = startX - e.touches[0].clientX;
      const diffY = Math.abs(e.touches[0].clientY - startY);

      if (diffY > 8) {
        isScrolling = true;
        clearTimeout(pressTimer);
        elem.classList.remove('pressing');
      }

      if (!isScrolling && diffX > 15 && diffX > diffY * 2) {
        isSwiping = true;
        clearTimeout(pressTimer);
        elem.classList.remove('pressing');
      }

      if (isSwiping && diffX > 0 && diffX < 80) {
        elem.style.transform = `translateX(-${diffX}px)`;
      }
    }, { passive: true });

    elem.addEventListener('touchend', () => {
      clearTimeout(pressTimer);
      elem.classList.remove('pressing');
      if (isSwiping && elem.style.transform.includes('-')) {
        startReply(m.sender, m.text || 'Фото', m.avatar || '');
      }
      elem.style.transform = 'translateX(0px)';
      isSwiping = false;
      isScrolling = false;
    });
  }

  function getEmojiAnimationClass(emoji) {
    if (emoji === '❤️') return 'emoji-anim-heart';
    if (emoji === '🔥') return 'emoji-anim-fire';
    if (emoji === '🎉' || emoji === '🥳') return 'emoji-anim-party';
    if (emoji === '💀') return 'emoji-anim-skull';
    return '';
  }

  function listenReactions(msgId) {
    db.ref(`chats/${currentRoomId}/${msgId}/reactions`).on('value', (snap) => {
      const wrap = document.getElementById(`reactWrap_${msgId}`);
      if (!wrap) return;
      wrap.innerHTML = '';
      const reacts = snap.val() || {};
      Object.keys(reacts).forEach(uid => {
        const r = reacts[uid];
        const rAvatar = r.avatar ? `<img src="${r.avatar}">` : (r.sender || 'U').charAt(0).toUpperCase();
        const pill = document.createElement('div');
        pill.className = 'reaction-pill';
        const animClass = getEmojiAnimationClass(r.emoji);
        pill.innerHTML = `
          <div class="reaction-avatar">${rAvatar}</div>
          <span class="${animClass}">${r.emoji}</span>
        `;
        pill.onclick = () => {
          if (uid === user.id) {
            db.ref(`chats/${currentRoomId}/${msgId}/reactions/${user.id}`).remove();
          }
        };
        wrap.appendChild(pill);
      });
    });
  }

  function openMsgReactionPicker(msgId, text, time) {
    selectedMsgForContext = { msgId: msgId };
    document.getElementById('ctxMsgPreview').innerText = text || 'Сообщение';
    document.getElementById('ctxMsgTime').innerText = time ? `Отправлено в ${time}` : '';
    document.getElementById('contextMenu').classList.add('open');
  }

  function toggleReaction(msgId, emoji) {
    const ref = db.ref(`chats/${currentRoomId}/${msgId}/reactions/${user.id}`);
    ref.once('value', (snap) => {
      const val = snap.val();
      if (val && val.emoji === emoji) {
        ref.remove();
      } else {
        ref.set({ emoji: emoji, sender: user.name, avatar: user.avatar || null });
      }
    });
  }

  function closeContextMenu() {
    document.getElementById('contextMenu').classList.remove('open');
    selectedMsgForContext = null;
  }

  function ctxReply() {
    if (selectedMsgForContext) {
      startReply(selectedMsgForContext.sender, selectedMsgForContext.text || 'Фото', selectedMsgForContext.avatar || '');
    }
    closeContextMenu();
  }

  function ctxCopy() {
    if (selectedMsgForContext && selectedMsgForContext.text) {
      navigator.clipboard.writeText(selectedMsgForContext.text);
      showToast('Скопировано!');
    }
    closeContextMenu();
  }

  function ctxReact(emoji) {
    if (selectedMsgForContext) {
      toggleReaction(selectedMsgForContext.msgId, emoji);
    }
    closeContextMenu();
  }

  function startReply(sender, text, avatar) {
    replyingTo = { sender: sender, text: text.substring(0, 50), avatar: avatar || null };
    document.getElementById('replyUserTitle').innerText = `Ответ на @${sender}`;
    document.getElementById('replyTextSnippet').innerText = text.substring(0, 45) + (text.length > 45 ? '...' : '');
    document.getElementById('replyPreview').classList.add('active');
    document.getElementById('chatInput').focus();
  }

  function cancelReply() {
    replyingTo = null;
    document.getElementById('replyPreview').classList.remove('active');
  }

  function sendChatMessage() {
    if (isSendingMessage || isChatBlockedByHost) return;
    const inp = document.getElementById('chatInput');
    const txt = inp.value.trim();
    if (!txt) return;

    isSendingMessage = true;
    inp.value = '';

    postMessage({
      text: txt,
      replyQuote: replyingTo ? { sender: replyingTo.sender, text: replyingTo.text, avatar: replyingTo.avatar } : null
    });

    cancelReply();
    closeAllChatPopups();

    setTimeout(() => { isSendingMessage = false; }, 350);
  }

  function postMessage(payload) {
    const now = new Date();
    db.ref('chats/' + currentRoomId).push({
      userId: user.id,
      sender: payload.sender || user.name,
      avatar: user.avatar || null,
      text: payload.text || null,
      image: payload.image || null,
      replyQuote: payload.replyQuote || null,
      time: now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
    });
  }

  function uploadChatPhoto(e) {
    if (isChatBlockedByHost) return showToast('Чат заблокирован создателем');
    const file = e.target.files[0];
    if (!file) return;
    closeAllChatPopups();
    const reader = new FileReader();
    reader.onload = (ev) => {
      const img = new Image();
      img.src = ev.target.result;
      img.onload = () => {
        const canvas = document.createElement('canvas');
        const MAX_W = 600;
        const scale = MAX_W / img.width;
        canvas.width = MAX_W;
        canvas.height = img.height * scale;
        const ctx = canvas.getContext('2d');
        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
        const compressedBase64 = canvas.toDataURL('image/jpeg', 0.6);
        postMessage({
          image: compressedBase64,
          replyQuote: replyingTo ? { sender: replyingTo.sender, text: replyingTo.text, avatar: replyingTo.avatar } : null
        });
        cancelReply();
      };
    };
    reader.readAsDataURL(file);
  }

  function toggleEmoji() {
    document.getElementById('photoMenu').classList.remove('open');
    document.getElementById('emojiPicker').classList.toggle('open');
  }

  function togglePhotoMenu() {
    document.getElementById('emojiPicker').classList.remove('open');
    document.getElementById('photoMenu').classList.toggle('open');
  }

  function closeAllChatPopups() {
    document.getElementById('emojiPicker').classList.remove('open');
    document.getElementById('photoMenu').classList.remove('open');
  }

  function addEmoji(emoji) {
    document.getElementById('chatInput').value += emoji;
    closeAllChatPopups();
  }

  function filterEmojis(query, containerSelector) {
    const q = query.trim().toLowerCase();
    const items = document.querySelectorAll(`${containerSelector} .emoji-btn`);
    items.forEach(item => {
      const tags = item.getAttribute('data-tags') || '';
      if (!q || tags.includes(q) || item.innerText.includes(q)) {
        item.style.display = 'block';
      } else {
        item.style.display = 'none';
      }
    });
  }

  function openInviteFriendsModal() {
    selectedInviteFriends.clear();
    inviteTab = 'friends';
    document.getElementById('tabInvFriends').classList.add('active');
    document.getElementById('tabInvRecent').classList.remove('active');
    document.getElementById('tabInvRequests').classList.remove('active');
    renderInviteFriendsList();
    openModal('inviteFriendsDrawer');
  }

  function switchInviteTab(tab) {
    inviteTab = tab;
    document.getElementById('tabInvFriends').classList.toggle('active', tab === 'friends');
    document.getElementById('tabInvRecent').classList.toggle('active', tab === 'recent');
    document.getElementById('tabInvRequests').classList.toggle('active', tab === 'requests');
    renderInviteFriendsList();
  }

  async function renderInviteFriendsList() {
    const container = document.getElementById('inviteFriendsList');
    container.innerHTML = '';
    const query = document.getElementById('inviteSearchInput').value.trim().toLowerCase();

    if (inviteTab === 'requests') {
      const snap = await db.ref(`users_notifications/${user.username}`).once('value');
      const notifs = snap.val() || {};
      const reqs = Object.entries(notifs).filter(([_, n]) => n.type === 'friend_request');
      if (!reqs.length) {
        container.innerHTML = '<div style="font-size:0.85rem; color:var(--text-muted); text-align:center; padding:20px;">Нет входящих заявок</div>';
        return;
      }
      reqs.forEach(([k, n]) => {
        const row = document.createElement('div');
        row.className = 'invite-friend-row';
        row.innerHTML = `
          <div style="flex:1;"><b>@${escapeHtml(n.fromUsername)}</b></div>
          <button class="btn btn-success" style="padding:4px 8px; font-size:0.75rem;" onclick="acceptFriend('${k}', '${n.fromUsername}', '${n.fromName}')">Принять</button>
        `;
        container.appendChild(row);
      });
      return;
    }

    const friendKeys = Array.from(new Set(Object.keys(friends || {})));
    if (!friendKeys.length) {
      container.innerHTML = '<div style="font-size:0.85rem; color:var(--text-muted); text-align:center; padding:20px;">Список друзей пуст</div>';
      return;
    }

    const dmMetas = (await db.ref(`users_dm_meta/${user.username}`).once('value')).val() || {};

    friendKeys.forEach(uname => {
      const f = friends[uname] || {};
      if (query && !uname.toLowerCase().includes(query) && !f.name.toLowerCase().includes(query)) return;

      const isOnline = (cachedStatuses[uname] && cachedStatuses[uname].status === 'online');
      const lastMsg = dmMetas[uname] ? dmMetas[uname].lastText : `@${uname}`;

      const row = document.createElement('div');
      row.className = 'invite-friend-row';
      const isChecked = selectedInviteFriends.has(uname);

      row.innerHTML = `
        <div class="avatar-box" style="width:38px; height:38px;" onclick="openUserProfile('', '${escapeHtml(f.name || uname)}', '@${escapeHtml(uname)}', '')">
          ${isOnline ? '<span class="status-dot-on-avatar online"></span>' : ''}
          ${(f.name || uname).charAt(0).toUpperCase()}
        </div>
        <div class="invite-info-area" onclick="openDirectMessage('${uname}', '${escapeHtml(f.name || uname)}')">
          <div class="invite-name">${escapeHtml(f.name || uname)}</div>
          <div class="invite-last-msg">${escapeHtml(lastMsg)}</div>
        </div>
        <div class="invite-check-circle ${isChecked ? 'selected' : ''}" onclick="toggleSelectFriend('${uname}')">
          <svg viewBox="0 0 24 24"><polyline points="20 6 9 17 4 12"></polyline></svg>
        </div>
      `;
      container.appendChild(row);
    });

    updateSelectedCount();
  }

  function toggleSelectFriend(uname) {
    if (selectedInviteFriends.has(uname)) {
      selectedInviteFriends.delete(uname);
    } else {
      selectedInviteFriends.add(uname);
    }
    renderInviteFriendsList();
  }

  function toggleSelectAllFriends() {
    const friendKeys = Object.keys(friends || {});
    if (selectedInviteFriends.size === friendKeys.length) {
      selectedInviteFriends.clear();
    } else {
      friendKeys.forEach(k => selectedInviteFriends.add(k));
    }
    renderInviteFriendsList();
  }

  function updateSelectedCount() {
    const count = selectedInviteFriends.size;
    document.getElementById('inviteSelectedCount').innerText = count;
    document.getElementById('sendInviteActionBar').style.display = count > 0 ? 'block' : 'none';
  }

  function sendBatchRoomInvites() {
    if (!currentRoomId) return showToast('Вы не находитесь в комнате');
    selectedInviteFriends.forEach(friendUname => {
      db.ref(`users_notifications/${friendUname}`).push({
        type: 'room_invite',
        roomId: currentRoomId,
        roomName: currentRoomData ? currentRoomData.name : 'Комната',
        fromUsername: user.username,
        time: Date.now()
      });
    });
    showToast(`Приглашения отправлены (${selectedInviteFriends.size})!`);
    closeModal('inviteFriendsDrawer');
  }

  function openDirectMessage(uname, name) {
    dmPartner = { username: uname, name: name };
    document.getElementById('dmPartnerName').innerText = name;
    document.getElementById('dmPartnerUsername').innerText = '@' + uname;
    document.getElementById('dmPartnerAvatar').innerText = name.charAt(0).toUpperCase();

    closeModal('inviteFriendsDrawer');
    closeModal('userProfileModal');
    openModal('dmModal');

    const dmId = [user.username, uname].sort().join('_');
    const container = document.getElementById('dmMessages');
    container.innerHTML = '';

    db.ref(`users_dms/${dmId}`).limitToLast(50).off();
    db.ref(`users_dms/${dmId}`).limitToLast(50).on('child_added', (snap) => {
      const m = snap.val();
      if (!m) return;
      const isMe = m.sender === user.username;
      const bubble = document.createElement('div');
      bubble.style.cssText = `
        align-self: ${isMe ? 'flex-end' : 'flex-start'};
        background: ${isMe ? 'var(--primary)' : 'rgba(255,255,255,0.08)'};
        backdrop-filter: blur(8px);
        color: #fff;
        padding: 8px 12px;
        border-radius: 14px;
        font-size: 0.85rem;
        max-width: 80%;
        word-break: break-word;
        border: 1px solid var(--glass-border);
      `;
      bubble.innerText = m.text;
      container.appendChild(bubble);
      container.scrollTop = container.scrollHeight;
    });
  }

  function sendDirectMessage() {
    const inp = document.getElementById('dmInput');
    const txt = inp.value.trim();
    if (!txt || !dmPartner) return;

    const dmId = [user.username, dmPartner.username].sort().join('_');
    db.ref(`users_dms/${dmId}`).push({
      sender: user.username,
      text: txt,
      time: Date.now()
    });

    inp.value = '';
  }

  let viewingUser = null;
  function openUserProfile(uid, name, username, avatar) {
    viewingUser = { uid, name, username: username.replace('@', ''), avatar };
    document.getElementById('upName').innerText = name || 'Пользователь';
    document.getElementById('upUsername').innerText = username || '@username';
    const av = document.getElementById('upAvatar');
    if (avatar) {
      av.innerHTML = `<img src="${avatar}">`;
    } else {
      av.innerText = (name || 'U').charAt(0).toUpperCase();
    }

    const isFriend = !!friends[viewingUser.username];
    document.getElementById('btnUpFriendAction').innerText = isFriend ? 'Удалить из друзей' : '+ В друзья';

    openModal('userProfileModal');
  }

  function openDmFromProfile() {
    if (viewingUser) {
      openDirectMessage(viewingUser.username, viewingUser.name);
    }
  }

  function toggleFriendFromProfile() {
    if (!viewingUser) return;
    if (friends[viewingUser.username]) {
      removeFriend(viewingUser.username);
      document.getElementById('btnUpFriendAction').innerText = '+ Добавить в друзья';
    } else {
      sendFriendRequest(viewingUser.username);
    }
  }

  function startWatchTimer() {
    setInterval(() => {
      user.totalSeconds = (user.totalSeconds || 0) + 10;
      saveLocalUser();

      if (currentRoomData && friends[currentRoomData.hostUsername]) {
        const frUname = currentRoomData.hostUsername;
        const newHours = (friends[frUname].hours || 0) + (10 / 3600);
        db.ref(`users_friends/${user.username}/${frUname}/hours`).set(newHours);
        db.ref(`users_friends/${frUname}/${user.username}/hours`).set(newHours);
      }
    }, 10000);
  }

  function openModal(id) {
    const el = document.getElementById(id);
    if (el) el.classList.add('open');
  }

  function closeModal(id) {
    const el = document.getElementById(id);
    if (el) el.classList.remove('open');
  }

  function escapeHtml(str) {
    return (str || '').replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
  }

  window.addEventListener('load', () => {
    const hash = window.location.hash.replace('#', '');
    const savedRoom = sessionStorage.getItem('arm_active_room') || hash;
    if (savedRoom && savedRoom.startsWith('room_')) {
      db.ref('rooms_meta/' + savedRoom).once('value', (snap) => {
        const r = snap.val();
        if (r) enterRoom(savedRoom, r);
      });
    }
  });
</script>

</body>
</html>
