<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IGLOO INC. | Created by Rikip 🪐</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollToPlugin.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Syncopate:wght@400;700&family=Rajdhani:wght@300;500;700&family=Orbitron:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        :root {
            --neon-cyan: #00f3ff;
            --neon-pink: #ff00ff;
            --neon-purple: #bf00ff;
            --deep-space: #050508;
            --ice-blue: #e0ffff;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Rajdhani', sans-serif;
            background: var(--deep-space);
            color: white;
            overflow-x: hidden;
            cursor: none;
        }
        
        /* Ultra Premium Cursor */
        .cursor-glow {
            position: fixed;
            width: 400px;
            height: 400px;
            background: radial-gradient(circle, rgba(0,243,255,0.15) 0%, transparent 70%);
            border-radius: 50%;
            pointer-events: none;
            z-index: 1;
            transform: translate(-50%, -50%);
            transition: opacity 0.3s;
            mix-blend-mode: screen;
        }
        
        .cursor-dot {
            position: fixed;
            width: 8px;
            height: 8px;
            background: var(--neon-cyan);
            border-radius: 50%;
            pointer-events: none;
            z-index: 10000;
            transform: translate(-50%, -50%);
            box-shadow: 0 0 20px var(--neon-cyan), 0 0 40px var(--neon-cyan);
            transition: transform 0.1s;
        }
        
        .cursor-ring {
            position: fixed;
            width: 40px;
            height: 40px;
            border: 2px solid rgba(0,243,255,0.5);
            border-radius: 50%;
            pointer-events: none;
            z-index: 9999;
            transform: translate(-50%, -50%);
            transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
        }
        
        .cursor-ring.hover {
            width: 80px;
            height: 80px;
            border-color: var(--neon-pink);
            background: rgba(255,0,255,0.1);
            box-shadow: 0 0 30px rgba(255,0,255,0.3);
        }
        
        /* WebGL Canvas */
        #webgl-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
        }
        
        /* UI Layer */
        #ui-layer {
            position: relative;
            z-index: 10;
        }
        
        /* Navigation */
        .nav-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            padding: 2rem 4rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            z-index: 100;
            backdrop-filter: blur(10px);
            background: linear-gradient(180deg, rgba(5,5,8,0.8) 0%, transparent 100%);
        }
        
        .nav-logo {
            font-family: 'Syncopate', sans-serif;
            font-size: 1.5rem;
            font-weight: 700;
            letter-spacing: 0.3em;
            background: linear-gradient(135deg, #fff 0%, var(--neon-cyan) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            position: relative;
        }
        
        .nav-logo::after {
            content: '🪐';
            position: absolute;
            right: -2rem;
            top: 50%;
            transform: translateY(-50%);
            font-size: 1rem;
            -webkit-text-fill-color: initial;
            filter: drop-shadow(0 0 10px rgba(0,243,255,0.8));
            animation: float 3s ease-in-out infinite;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(-50%) translateY(0); }
            50% { transform: translateY(-50%) translateY(-10px); }
        }
        
        .nav-links {
            display: flex;
            gap: 4rem;
        }
        
        .nav-link {
            font-family: 'Orbitron', sans-serif;
            font-size: 0.75rem;
            letter-spacing: 0.3em;
            text-transform: uppercase;
            color: rgba(255,255,255,0.7);
            text-decoration: none;
            position: relative;
            padding: 0.5rem 0;
            transition: all 0.3s;
        }
        
        .nav-link::before {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 0;
            height: 2px;
            background: linear-gradient(90deg, var(--neon-cyan), var(--neon-pink));
            transition: width 0.4s cubic-bezier(0.23, 1, 0.32, 1);
            box-shadow: 0 0 10px var(--neon-cyan);
        }
        
        .nav-link:hover {
            color: var(--neon-cyan);
            text-shadow: 0 0 20px rgba(0,243,255,0.5);
        }
        
        .nav-link:hover::before {
            width: 100%;
        }
        
        /* Sections */
        .section {
            position: relative;
            width: 100%;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }
        
        /* Enhanced Loader */
        .loader {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--deep-space);
            z-index: 99999;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 2rem;
        }
        
        .loader-text {
            font-family: 'Orbitron', sans-serif;
            font-size: 1rem;
            letter-spacing: 1em;
            color: var(--neon-cyan);
            text-shadow: 0 0 20px rgba(0,243,255,0.5);
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }
        
        .loader-bar-container {
            width: 300px;
            height: 2px;
            background: rgba(255,255,255,0.1);
            position: relative;
            overflow: hidden;
            border-radius: 2px;
        }
        
        .loader-bar {
            position: absolute;
            top: 0;
            left: 0;
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, var(--neon-cyan), var(--neon-pink), var(--neon-purple));
            box-shadow: 0 0 20px var(--neon-cyan);
            transition: width 0.3s;
        }
        
        .loader-creator {
            position: absolute;
            bottom: 3rem;
            font-family: 'Syncopate', sans-serif;
            font-size: 0.8rem;
            letter-spacing: 0.5em;
            color: rgba(255,255,255,0.3);
        }
        
        .loader-creator span {
            color: var(--neon-cyan);
            text-shadow: 0 0 10px rgba(0,243,255,0.5);
        }
        
        /* Content Styling */
        .section-content {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding: 0 10%;
            opacity: 0;
            transform: translateY(100px);
        }
        
        .section-label {
            font-family: 'Orbitron', sans-serif;
            font-size: 0.8rem;
            letter-spacing: 0.5em;
            text-transform: uppercase;
            color: var(--neon-cyan);
            margin-bottom: 2rem;
            display: flex;
            align-items: center;
            gap: 2rem;
            opacity: 0;
            transform: translateX(-50px);
        }
        
        .section-label::before {
            content: '';
            width: 60px;
            height: 1px;
            background: linear-gradient(90deg, var(--neon-cyan), transparent);
            box-shadow: 0 0 10px var(--neon-cyan);
        }
        
        .section-title {
            font-family: 'Syncopate', sans-serif;
            font-size: clamp(2.5rem, 6vw, 5rem);
            font-weight: 700;
            line-height: 1.1;
            margin-bottom: 2rem;
            letter-spacing: -0.02em;
            background: linear-gradient(135deg, #ffffff 0%, var(--ice-blue) 50%, var(--neon-cyan) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: drop-shadow(0 0 30px rgba(0,243,255,0.3));
            opacity: 0;
            transform: translateY(50px);
        }
        
        .section-subtitle {
            font-family: 'Rajdhani', sans-serif;
            font-size: 1.3rem;
            color: rgba(255,255,255,0.6);
            max-width: 600px;
            line-height: 1.8;
            font-weight: 300;
            opacity: 0;
            transform: translateY(30px);
        }
        
        /* Glitch Effect */
        .glitch {
            position: relative;
            display: inline-block;
        }
        
        .glitch::before,
        .glitch::after {
            content: attr(data-text);
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--deep-space);
        }
        
        .glitch::before {
            left: 2px;
            text-shadow: -2px 0 var(--neon-pink);
            clip: rect(24px, 550px, 90px, 0);
            animation: glitch-anim-1 2s infinite linear alternate-reverse;
        }
        
        .glitch::after {
            left: -2px;
            text-shadow: -2px 0 var(--neon-cyan);
            clip: rect(85px, 550px, 140px, 0);
            animation: glitch-anim-2 2s infinite linear alternate-reverse;
        }
        
        @keyframes glitch-anim-1 {
            0% { clip: rect(20px, 9999px, 15px, 0); }
            20% { clip: rect(50px, 9999px, 80px, 0); }
            40% { clip: rect(10px, 9999px, 65px, 0); }
            60% { clip: rect(90px, 9999px, 30px, 0); }
            80% { clip: rect(40px, 9999px, 95px, 0); }
            100% { clip: rect(70px, 9999px, 20px, 0); }
        }
        
        @keyframes glitch-anim-2 {
            0% { clip: rect(65px, 9999px, 99px, 0); }
            20% { clip: rect(20px, 9999px, 45px, 0); }
            40% { clip: rect(75px, 9999px, 10px, 0); }
            60% { clip: rect(30px, 9999px, 60px, 0); }
            80% { clip: rect(95px, 9999px, 25px, 0); }
            100% { clip: rect(15px, 9999px, 80px, 0); }
        }
        
        /* Project Cards */
        .project-info {
            position: absolute;
            right: 10%;
            top: 50%;
            transform: translateY(-50%);
            text-align: right;
            max-width: 500px;
            background: rgba(5,5,8,0.6);
            backdrop-filter: blur(20px);
            padding: 3rem;
            border: 1px solid rgba(0,243,255,0.2);
            border-radius: 20px;
            box-shadow: 0 0 40px rgba(0,243,255,0.1), inset 0 0 40px rgba(0,243,255,0.05);
        }
        
        .project-number {
            font-family: 'Orbitron', sans-serif;
            font-size: 0.9rem;
            letter-spacing: 0.5em;
            color: var(--neon-cyan);
            margin-bottom: 1rem;
            text-shadow: 0 0 10px rgba(0,243,255,0.5);
        }
        
        .project-title {
            font-family: 'Syncopate', sans-serif;
            font-size: clamp(1.8rem, 3vw, 2.5rem);
            font-weight: 700;
            margin-bottom: 1.5rem;
            line-height: 1.2;
            background: linear-gradient(135deg, #fff 0%, var(--neon-cyan) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .project-desc {
            font-size: 1.1rem;
            color: rgba(255,255,255,0.7);
            line-height: 1.8;
            margin-bottom: 2.5rem;
            font-weight: 300;
        }
        
        .project-link {
            display: inline-flex;
            align-items: center;
            gap: 1rem;
            color: var(--neon-cyan);
            text-decoration: none;
            font-family: 'Orbitron', sans-serif;
            font-size: 0.85rem;
            letter-spacing: 0.3em;
            text-transform: uppercase;
            padding: 1rem 2rem;
            border: 1px solid rgba(0,243,255,0.3);
            border-radius: 50px;
            transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
            overflow: hidden;
        }
        
        .project-link::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(0,243,255,0.2), transparent);
            transition: left 0.6s;
        }
        
        .project-link:hover {
            background: rgba(0,243,255,0.1);
            border-color: var(--neon-cyan);
            box-shadow: 0 0 30px rgba(0,243,255,0.3);
            gap: 1.5rem;
            transform: translateX(-5px);
        }
        
        .project-link:hover::before {
            left: 100%;
        }
        
        /* Particle Footer Section */
        .particle-ui {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 10;
        }
        
        .particle-title {
            font-family: 'Syncopate', sans-serif;
            font-size: clamp(2rem, 4vw, 3.5rem);
            font-weight: 700;
            text-align: center;
            margin-bottom: 4rem;
            letter-spacing: 0.2em;
            background: linear-gradient(135deg, #fff 0%, var(--neon-cyan) 50%, var(--neon-pink) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: drop-shadow(0 0 30px rgba(0,243,255,0.3));
        }
        
        .particle-links {
            display: flex;
            gap: 3rem;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        .particle-link {
            color: rgba(255,255,255,0.8);
            text-decoration: none;
            font-family: 'Orbitron', sans-serif;
            font-size: 0.9rem;
            letter-spacing: 0.3em;
            text-transform: uppercase;
            padding: 1.5rem 3rem;
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 50px;
            transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
            overflow: hidden;
            background: rgba(0,0,0,0.3);
            backdrop-filter: blur(10px);
        }
        
        .particle-link::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(0,243,255,0.3), transparent);
            transition: left 0.6s;
        }
        
        .particle-link:hover {
            color: var(--neon-cyan);
            border-color: var(--neon-cyan);
            background: rgba(0,243,255,0.1);
            box-shadow: 0 0 40px rgba(0,243,255,0.2);
            transform: translateY(-5px);
        }
        
        .particle-link:hover::before {
            left: 100%;
        }
        
        /* Instagram Link Special Styling */
        .instagram-link {
            position: fixed;
            bottom: 3rem;
            left: 50%;
            transform: translateX(-50%);
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 1rem 2rem;
            background: linear-gradient(135deg, rgba(225,48,108,0.2), rgba(0,243,255,0.2));
            border: 1px solid rgba(225,48,108,0.5);
            border-radius: 50px;
            color: white;
            text-decoration: none;
            font-family: 'Orbitron', sans-serif;
            font-size: 0.85rem;
            letter-spacing: 0.2em;
            transition: all 0.4s;
            backdrop-filter: blur(10px);
            box-shadow: 0 0 30px rgba(225,48,108,0.2);
        }
        
        .instagram-link:hover {
            transform: translateX(-50%) translateY(-5px);
            box-shadow: 0 10px 40px rgba(225,48,108,0.4), 0 0 60px rgba(0,243,255,0.2);
            border-color: #e1306c;
        }
        
        .instagram-icon {
            width: 24px;
            height: 24px;
            fill: currentColor;
        }
        
        /* Creator Credit */
        .creator-credit {
            position: fixed;
            bottom: 2rem;
            right: 3rem;
            z-index: 1000;
            font-family: 'Syncopate', sans-serif;
            font-size: 0.75rem;
            letter-spacing: 0.3em;
            color: rgba(255,255,255,0.4);
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        
        .creator-credit span {
            color: var(--neon-cyan);
            font-weight: 700;
            text-shadow: 0 0 10px rgba(0,243,255,0.5);
        }
        
        .creator-credit .planet {
            font-size: 1.2rem;
            animation: rotate 10s linear infinite;
            display: inline-block;
        }
        
        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }
        
        /* Progress Indicator */
        .scroll-progress {
            position: fixed;
            right: 3rem;
            top: 50%;
            transform: translateY(-50%);
            width: 3px;
            height: 150px;
            background: rgba(255,255,255,0.1);
            z-index: 100;
            border-radius: 3px;
            overflow: hidden;
        }
        
        .scroll-progress-bar {
            width: 100%;
            height: 0%;
            background: linear-gradient(180deg, var(--neon-cyan), var(--neon-pink));
            box-shadow: 0 0 20px var(--neon-cyan);
            transition: height 0.1s;
        }
        
        /* Section indicators */
        .section-dots {
            position: fixed;
            right: 2.5rem;
            top: 50%;
            transform: translateY(-50%);
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            z-index: 100;
        }
        
        .section-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: rgba(255,255,255,0.2);
            cursor: pointer;
            transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
            border: 2px solid transparent;
        }
        
        .section-dot.active {
            background: var(--neon-cyan);
            transform: scale(1.5);
            box-shadow: 0 0 20px var(--neon-cyan);
            border-color: rgba(255,255,255,0
