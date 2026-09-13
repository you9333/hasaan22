<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ultra HD Animated Stream Overlay</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@1,900&family=Orbitron:wght@800;900&display=swap');

    body {
      background-color: transparent;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 0;
      font-family: 'Montserrat', sans-serif;
      -webkit-font-smoothing: antialiased;
    }

    /* Scaled Up Container */
    .card-wrapper {
      position: relative;
      margin-top: 25px;
      transform: scale(1.25); /* Makes the overall card bigger for streams */
      filter: drop-shadow(0 12px 30px rgba(0, 0, 0, 0.9));
    }

    /* Outer Metallic Frame with Cut Corners */
    .banner-frame {
      background: linear-gradient(135deg, #3a4b6e 0%, #0a0f18 50%, #202d45 100%);
      padding: 2px;
      clip-path: polygon(
        18px 0, 100% 0, 
        100% calc(100% - 18px), calc(100% - 18px) 100%, 
        0 100%, 0 18px
      );
    }

    /* Main Inner Dark Card */
    .banner-card {
      display: flex;
      align-items: center;
      gap: 16px;
      background: linear-gradient(180deg, #090c15 0%, #020407 100%);
      padding: 12px 24px 12px 20px;
      clip-path: polygon(
        17px 0, 100% 0, 
        100% calc(100% - 17px), calc(100% - 17px) 100%, 
        0 100%, 0 17px
      );
    }

    /* Top-Left White LIVE Badge */
    .live-badge {
      position: absolute;
      top: -12px;
      left: 24px;
      background: linear-gradient(180deg, #ffffff 0%, #ececec 100%);
      color: #e61238;
      font-size: 12px;
      font-weight: 900;
      padding: 3px 12px 3px 10px;
      border-radius: 4px;
      display: flex;
      align-items: center;
      gap: 6px;
      letter-spacing: 1px;
      z-index: 10;
      box-shadow: 0 4px 14px rgba(230, 18, 56, 0.5);
      border: 1px solid rgba(255, 255, 255, 0.9);
    }

    .live-dot {
      width: 7px;
      height: 7px;
      background-color: #e61238;
      border-radius: 50%;
      box-shadow: 0 0 8px #e61238;
      animation: pulse-dot 1.2s infinite ease-in-out;
    }

    /* Equalizer Signal Icon */
    .signal-icon {
      display: flex;
      align-items: flex-end;
      gap: 3px;
      height: 20px;
    }

    .signal-bar {
      width: 4px;
      background: linear-gradient(180deg, #fff275 0%, #f7bc08 100%);
      border-radius: 1px;
      box-shadow: 0 0 6px rgba(247, 188, 8, 0.6);
    }
    .bar-1 { height: 35%; animation: eq 1.4s infinite 0.1s ease-in-out; }
    .bar-2 { height: 65%; animation: eq 1.4s infinite 0.3s ease-in-out; }
    .bar-3 { height: 85%; animation: eq 1.4s infinite 0.2s ease-in-out; }
    .bar-4 { height: 100%; animation: eq 1.4s infinite 0.4s ease-in-out; }

    /* Crisp Esports Title Typography */
    .title {
      font-size: 22px;
      font-weight: 900;
      font-style: italic;
      color: #ffffff;
      letter-spacing: 1px;
      white-space: nowrap;
      text-transform: uppercase;
      text-shadow: 0 2px 10px rgba(0, 0, 0, 0.9);
    }

    .title .yellow {
      color: #ffe600;
      text-shadow: 0 0 14px rgba(255, 230, 0, 0.5);
    }

    /* Neon Ping Tag */
    .ping-tag {
      display: flex;
      align-items: center;
      gap: 6px;
      background: rgba(0, 255, 102, 0.08);
      border: 1px solid #00ff66;
      color: #00ff66;
      font-family: 'Orbitron', sans-serif;
      font-weight: 800;
      font-size: 13px;
      padding: 5px 12px;
      border-radius: 5px;
      letter-spacing: 0.8px;
      box-shadow: 0 0 12px rgba(0, 255, 102, 0.3);
      text-shadow: 0 0 6px rgba(0, 255, 102, 0.8);
    }

    .ping-dot {
      width: 6px;
      height: 6px;
      background-color: #00ff66;
      border-radius: 50%;
      box-shadow: 0 0 8px #00ff66;
    }

    /* Blue FINALS Tag */
    .finals-tag {
      background: linear-gradient(180deg, #092047 0%, #040e21 100%);
      border: 1px solid #1f62cb;
      color: #3b93ff;
      font-family: 'Orbitron', sans-serif;
      font-weight: 800;
      font-size: 13px;
      padding: 5px 14px;
      border-radius: 5px;
      letter-spacing: 1px;
      text-transform: uppercase;
      box-shadow: 0 0 12px rgba(31, 98, 203, 0.4);
      text-shadow: 0 0 6px rgba(59, 147, 255, 0.8);
    }

    /* Flag Section with 3D Wave Animation */
    .flag-group {
      display: flex;
      align-items: center;
      perspective: 400px;
    }

    .flag-pole {
      width: 4px;
      height: 34px;
      background: linear-gradient(180deg, #ffee75 0%, #d4a017 50%, #8a6500 100%);
      border-radius: 2px 0 0 2px;
      position: relative;
      z-index: 2;
      box-shadow: 0 0 6px rgba(212, 160, 23, 0.6);
    }

    .flag-pole::before {
      content: '';
      position: absolute;
      top: -4px;
      left: -2px;
      width: 8px;
      height: 8px;
      background: #ffee75;
      border-radius: 50%;
      box-shadow: 0 0 8px #ffee75;
    }

    /* Animated Flag Canvas */
    .flag-wrapper {
      transform-origin: left center;
      animation: wave-flag 2.2s infinite ease-in-out;
    }

    .flag-icon {
      width: 44px;
      height: 28px;
      border-radius: 0 3px 3px 0;
      display: block;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.7);
    }

    /* Keyframe Animations */
    @keyframes pulse-dot {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.3; transform: scale(0.85); }
    }

    @keyframes eq {
      0%, 100% { transform: scaleY(1); }
      50% { transform: scaleY(0.6); }
    }

    /* Dynamic Flag Waving Animation */
    @keyframes wave-flag {
      0% {
        transform: rotateY(0deg) skewY(0deg) scaleX(1);
      }
      25% {
        transform: rotateY(-18deg) skewY(-2deg) scaleX(0.96);
      }
      50% {
        transform: rotateY(0deg) skewY(1deg) scaleX(1);
      }
      75% {
        transform: rotateY(18deg) skewY(2deg) scaleX(0.96);
      }
      100% {
        transform: rotateY(0deg) skewY(0deg) scaleX(1);
      }
    }
  </style>
</head>
<body>

  <div class="card-wrapper">
    <!-- Top-Left White LIVE Badge -->
    <div class="live-badge">
      <span class="live-dot"></span> LIVE
    </div>

    <!-- Outer Frame with Metallic Border -->
    <div class="banner-frame">
      <div class="banner-card">
        
        <!-- Animated Signal Bars -->
        <div class="signal-icon">
          <div class="signal-bar bar-1"></div>
          <div class="signal-bar bar-2"></div>
          <div class="signal-bar bar-3"></div>
          <div class="signal-bar bar-4"></div>
        </div>

        <!-- Esports Title -->
        <div class="title">
          USA <span class="yellow">GTA 5</span> TOURNAMENT
        </div>

        <!-- Ping Badge -->
        <div class="ping-tag">
          <span class="ping-dot"></span> NA • 24MS
        </div>

        <!-- Stage Tag -->
        <div class="finals-tag">
          FINALS
        </div>

        <!-- US Flag with 3D Wave Animation -->
        <div class="flag-group">
          <div class="flag-pole"></div>
          <div class="flag-wrapper">
            <svg class="flag-icon" viewBox="0 0 640 480">
              <g fill-rule="evenodd">
                <path fill="#bd3d44" d="M0 0h640v480H0z"/>
                <path fill="#fff" d="M0 36.9h640v36.9H0zm0 73.8h640v36.9H0zm0 73.9h640v36.9H0zm0 73.8h640v36.9H0zm0 73.9h640v36.9H0zm0 73.8h640v36.9H0z"/>
                <path fill="#192f5d" d="M0 0h280v258.5H0z"/>
                <g fill="#fff">
                  <path d="M24.7 13l2.8 8.6h9l-7.3 5.3 2.8 8.6-7.3-5.3-7.3 5.3 2.8-8.6-7.3-5.3h9z"/>
                  <path d="M72.2 13l2.8 8.6h9l-7.3 5.3 2.8 8.6-7.3-5.3-7.3 5.3 2.8-8.6-7.3-5.3h9z"/>
                  <path d="M119.8 13l2.8 8.6h9l-7.3 5.3 2.8 8.6-7.3-5.3-7.3 5.3 2.8-8.6-7.3-5.3h9z"/>
                  <path d="M167.3 13l2.8 8.6h9l-7.3 5.3 2.8 8.6-7.3-5.3-7.3 5.3 2.8-8.6-7.3-5.3h9z"/>
                  <path d="M214.8 13l2.8 8.6h9l-7.3 5.3 2.8 8.6-7.3-5.3-7.3 5.3 2.8-8.6-7.3-5.3h9z"/>
                </g>
              </g>
            </svg>
          </div>
        </div>

      </div>
    </div>
  </div>

</body>
</html>
