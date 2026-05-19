<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ProDigits Pro — Calculator</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-outer: #0f0f11;
            --bg-calc: rgba(255, 255, 255, 0.98);
            --text-primary: #1a1a1a;
            --text-secondary: #8e8e93;
            --btn-num-bg: #f2f2f7;
            --btn-num-text: #1a1a1a;
            --btn-function-bg: #e5e5ea;
            --btn-function-text: #1a1a1a;
            --btn-op-bg: #1c1c1e;
            --btn-op-text: #ffffff;
            --shadow-calc: 0 24px 80px rgba(0, 0, 0, 0.12), 0 0 0 1px rgba(255, 255, 255, 0.1);
            --display-expr: #8e8e93;
            --panel-bg: rgba(255, 255, 255, 0.9);
            --border-subtle: rgba(0, 0, 0, 0.04);
        }

        body.theme-midnight {
            --bg-calc: rgba(28, 28, 30, 0.98);
            --text-primary: #ffffff;
            --text-secondary: #8e8e93;
            --btn-num-bg: #2c2c2e;
            --btn-num-text: #ffffff;
            --btn-function-bg: #3a3a3c;
            --btn-function-text: #ffffff;
            --btn-op-bg: #ffffff;
            --btn-op-text: #1c1c1e;
            --shadow-calc: 0 24px 80px rgba(0, 0, 0, 0.4), 0 0 0 1px rgba(255, 255, 255, 0.05);
            --display-expr: #8e8e93;
            --panel-bg: rgba(28, 28, 30, 0.95);
            --border-subtle: rgba(255, 255, 255, 0.06);
        }

        body.theme-frost {
            --bg-calc: rgba(255, 255, 255, 0.15);
            --text-primary: #ffffff;
            --text-secondary: rgba(255, 255, 255, 0.6);
            --btn-num-bg: rgba(255, 255, 255, 0.1);
            --btn-num-text: #ffffff;
            --btn-function-bg: rgba(255, 255, 255, 0.15);
            --btn-function-text: #ffffff;
            --btn-op-bg: rgba(255, 255, 255, 0.9);
            --btn-op-text: #1a1a1a;
            --shadow-calc: 0 24px 80px rgba(0, 0, 0, 0.2), 0 0 0 1px rgba(255, 255, 255, 0.15);
            --display-expr: rgba(255, 255, 255, 0.5);
            --panel-bg: rgba(255, 255, 255, 0.2);
            --border-subtle: rgba(255, 255, 255, 0.1);
        }

        body.theme-neon {
            --bg-calc: rgba(10, 10, 25, 0.9);
            --text-primary: #00f0ff;
            --text-secondary: #ff00aa;
            --btn-num-bg: rgba(0, 240, 255, 0.08);
            --btn-num-text: #00f0ff;
            --btn-function-bg: rgba(255, 0, 170, 0.1);
            --btn-function-text: #ff00aa;
            --btn-op-bg: linear-gradient(135deg, #ff00aa, #00f0ff);
            --btn-op-text: #ffffff;
            --shadow-calc: 0 0 60px rgba(0, 240, 255, 0.15), 0 0 0 1px rgba(0, 240, 255, 0.2);
            --display-expr: #ff00aa;
            --panel-bg: rgba(10, 10, 25, 0.95);
            --border-subtle: rgba(0, 240, 255, 0.15);
        }

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            margin: 0;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background: var(--bg-outer);
            color: var(--text-primary);
            overflow-x: hidden;
            transition: background 0.5s ease;
        }

        body.theme-frost {
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
        }

        body.theme-neon {
            background: #050508;
        }

        /* App Container */
        .app-container {
            min-height: 100vh;
            min-height: 100dvh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            position: relative;
        }

        .app-container::before {
            content: '';
            position: absolute;
            width: 600px;
            height: 600px;
            background: radial-gradient(circle, rgba(255, 255, 255, 0.04) 0%, transparent 70%);
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            pointer-events: none;
        }

        /* Calculator Card */
        .calculator-card {
            background: var(--bg-calc);
            border-radius: 32px;
            padding: 24px;
            width: 100%;
            max-width: 380px;
            box-shadow: var(--shadow-calc);
            position: relative;
            backdrop-filter: blur(20px);
            animation: enter 0.8s cubic-bezier(0.25, 0.1, 0.25, 1) forwards;
            z-index: 10;
        }

        @keyframes enter {
            from {
                opacity: 0;
                transform: translateY(30px);
            }

            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Header */
        .calc-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
            position: relative;
        }

        .brand {
            font-size: 12px;
            font-weight: 600;
            color: var(--text-secondary);
            letter-spacing: 1px;
            text-transform: uppercase;
        }

        .header-actions {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .premium-badge {
            display: inline-flex;
            align-items: center;
            gap: 4px;
            background: linear-gradient(135deg, #ffd700, #ffaa00);
            color: #1a1a1a;
            font-size: 10px;
            font-weight: 700;
            padding: 5px 10px;
            border-radius: 10px;
            letter-spacing: 0.5px;
            animation: fadeIn 0.4s ease;
        }

        .premium-badge.hidden {
            display: none;
        }

        .icon-btn {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            border: none;
            background: transparent;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--text-secondary);
            transition: all 0.2s;
            padding: 0;
        }

        .icon-btn:hover {
            background: var(--border-subtle);
            color: var(--text-primary);
        }

        .icon-btn svg {
            width: 18px;
            height: 18px;
        }

        /* Menu Dropdown */
        .menu-dropdown {
            position: absolute;
            top: 44px;
            right: 0;
            background: var(--panel-bg);
            backdrop-filter: blur(20px);
            border-radius: 16px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
            padding: 8px;
            z-index: 100;
            min-width: 190px;
            opacity: 0;
            visibility: hidden;
            transform: translateY(-8px);
            transition: all 0.25s cubic-bezier(0.25, 0.1, 0.25, 1);
            border: 1px solid var(--border-subtle);
        }

        .menu-dropdown.active {
            opacity: 1;
            visibility: visible;
            transform: translateY(0);
        }

        .menu-item {
            padding: 10px 14px;
            border-radius: 10px;
            font-size: 14px;
            font-weight: 500;
            color: var(--text-primary);
            cursor: pointer;
            transition: background 0.15s;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .menu-item:hover {
            background: var(--border-subtle);
        }

        .menu-item .lock-hint {
            width: 14px;
            height: 14px;
            color: var(--text-secondary);
            opacity: 0.5;
        }

        .menu-divider {
            height: 1px;
            background: var(--border-subtle);
            margin: 6px 0;
        }

        /* Display */
        .calc-display {
            padding: 20px 8px;
            text-align: right;
            margin-bottom: 16px;
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            position: relative;
        }

        .expression {
            font-size: 18px;
            font-weight: 400;
            color: var(--display-expr);
            min-height: 26px;
            margin-bottom: 8px;
            letter-spacing: 0.5px;
            word-wrap: break-word;
            transition: all 0.3s ease;
        }

        .result {
            font-size: 56px;
            font-weight: 300;
            color: var(--text-primary);
            line-height: 1.1;
            letter-spacing: -2px;
            transition: all 0.3s ease;
            overflow-x: auto;
            scrollbar-width: none;
            word-wrap: break-word;
        }

        .result::-webkit-scrollbar {
            display: none;
        }

        .result.glow {
            animation: resultGlow 1s ease;
        }

        @keyframes resultGlow {

            0%,
            100% {
                filter: brightness(1);
            }

            50% {
                filter: brightness(1.15);
            }
        }

        .result.pop {
            animation: resultPop 0.3s ease;
        }

        @keyframes resultPop {
            0% {
                transform: scale(0.98);
            }

            50% {
                transform: scale(1.02);
            }

            100% {
                transform: scale(1);
            }
        }

        /* Button Grid */
        .button-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            justify-items: center;
        }

        .btn {
            width: 64px;
            height: 64px;
            border-radius: 50%;
            border: 1px solid transparent;
            font-family: 'Inter', sans-serif;
            font-size: 24px;
            font-weight: 400;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            transition: all 0.2s cubic-bezier(0.25, 0.1, 0.25, 1);
            display: flex;
            align-items: center;
            justify-content: center;
            user-select: none;
            -webkit-user-select: none;
        }

        .btn-number {
            background: var(--btn-num-bg);
            color: var(--btn-num-text);
            border-color: var(--border-subtle);
        }

        .btn-number:hover {
            transform: scale(1.06);
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08), 0 0 0 1px var(--border-subtle);
        }

        .btn-number:active {
            transform: scale(0.92);
        }

        .btn-operator {
            background: var(--btn-op-bg);
            color: var(--btn-op-text);
            font-weight: 500;
            border: none;
        }

        body.theme-neon .btn-operator {
            background: linear-gradient(135deg, #ff00aa, #00f0ff);
        }

        .btn-operator:hover {
            transform: scale(1.08);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.25);
        }

        .btn-operator:active {
            transform: scale(0.92);
        }

        .btn-function {
            background: var(--btn-function-bg);
            color: var(--btn-function-text);
            font-size: 18px;
            font-weight: 500;
            border: none;
        }

        .btn-function:hover {
            transform: scale(1.06);
            background: var(--btn-num-bg);
        }

        .btn-function:active {
            transform: scale(0.92);
        }

        .btn svg {
            width: 20px;
            height: 20px;
            pointer-events: none;
        }

        /* Ripple */
        .ripple {
            position: absolute;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.25);
            transform: scale(0);
            animation: ripple 0.6s linear;
            pointer-events: none;
        }

        @keyframes ripple {
            to {
                transform: scale(4);
                opacity: 0;
            }
        }

        /* Side Panels */
        .side-panel {
            position: fixed;
            top: 0;
            right: 0;
            width: 340px;
            height: 100vh;
            height: 100dvh;
            background: var(--panel-bg);
            backdrop-filter: blur(40px);
            box-shadow: -8px 0 40px rgba(0, 0, 0, 0.15);
            z-index: 500;
            transform: translateX(110%);
            transition: transform 0.5s cubic-bezier(0.25, 0.1, 0.25, 1);
            display: flex;
            flex-direction: column;
            border-left: 1px solid var(--border-subtle);
        }

        .side-panel.active {
            transform: translateX(0);
        }

        .panel-header {
            padding: 24px;
            border-bottom: 1px solid var(--border-subtle);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .panel-header h3 {
            font-size: 18px;
            font-weight: 600;
            color: var(--text-primary);
            margin: 0;
        }

        .panel-header button {
            background: none;
            border: none;
            color: var(--text-secondary);
            font-size: 14px;
            cursor: pointer;
            padding: 4px 8px;
            border-radius: 6px;
            font-family: 'Inter', sans-serif;
        }

        .panel-header button:hover {
            color: var(--text-primary);
            background: var(--border-subtle);
        }

        .panel-content {
            flex: 1;
            overflow-y: auto;
            padding: 16px;
            scrollbar-width: thin;
            scrollbar-color: var(--border-subtle) transparent;
        }

        .panel-content::-webkit-scrollbar {
            width: 6px;
        }

        .panel-content::-webkit-scrollbar-thumb {
            background: var(--border-subtle);
            border-radius: 3px;
        }

        .empty-state {
            text-align: center;
            color: var(--text-secondary);
            font-size: 14px;
            padding: 40px 20px;
            opacity: 0.6;
        }

        .history-item {
            padding: 14px;
            border-radius: 14px;
            margin-bottom: 8px;
            background: var(--btn-num-bg);
            cursor: pointer;
            transition: all 0.2s ease;
            border: 1px solid var(--border-subtle);
        }

        .history-item:hover {
            transform: translateX(-4px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
        }

        .history-expr {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 6px;
            letter-spacing: 0.3px;
        }

        .history-result {
            font-size: 22px;
            font-weight: 500;
            color: var(--text-primary);
            letter-spacing: -0.5px;
        }

        .history-time {
            font-size: 11px;
            color: var(--text-secondary);
            margin-top: 6px;
            opacity: 0.7;
        }

        .memory-value {
            font-size: 32px;
            font-weight: 300;
            color: var(--text-primary);
            text-align: center;
            padding: 24px 0;
            letter-spacing: -1px;
        }

        .memory-actions {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
        }

        .memory-actions button {
            padding: 14px;
            border-radius: 14px;
            border: 1px solid var(--border-subtle);
            background: var(--btn-num-bg);
            color: var(--text-primary);
            font-family: 'Inter', sans-serif;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }

        .memory-actions button:hover {
            transform: scale(1.03);
            background: var(--btn-function-bg);
        }

        /* Modals */
        .modal-backdrop {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.45);
            backdrop-filter: blur(16px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.35s ease, visibility 0.35s ease;
            padding: 20px;
        }

        .modal-backdrop.active {
            opacity: 1;
            visibility: visible;
        }

        .modal-panel {
            background: var(--panel-bg);
            backdrop-filter: blur(40px);
            border-radius: 28px;
            padding: 36px;
            width: 100%;
            max-width: 480px;
            box-shadow: 0 24px 80px rgba(0, 0, 0, 0.25);
            transform: scale(0.92) translateY(20px);
            transition: transform 0.45s cubic-bezier(0.25, 0.1, 0.25, 1);
            position: relative;
            border: 1px solid var(--border-subtle);
            max-height: 90vh;
            overflow-y: auto;
        }

        .modal-backdrop.active .modal-panel {
            transform: scale(1) translateY(0);
        }

        .modal-close {
            position: absolute;
            top: 16px;
            right: 16px;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            border: none;
            background: var(--btn-function-bg);
            color: var(--text-secondary);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 0;
            transition: all 0.2s;
        }

        .modal-close:hover {
            background: var(--btn-num-bg);
            color: var(--text-primary);
            transform: rotate(90deg);
        }

        .modal-close svg {
            width: 16px;
            height: 16px;
        }

        /* Premium Modal */
        .premium-icon {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
        }

        .premium-icon svg {
            width: 56px;
            height: 56px;
            color: var(--text-primary);
            stroke-width: 1.2;
            animation: floatIcon 3s ease-in-out infinite;
        }

        @keyframes floatIcon {

            0%,
            100% {
                transform: translateY(0);
            }

            50% {
                transform: translateY(-8px);
            }
        }

        .premium-title {
            font-size: 26px;
            font-weight: 600;
            text-align: center;
            color: var(--text-primary);
            margin: 0 0 10px;
            letter-spacing: -0.5px;
        }

        .premium-subtitle {
            font-size: 15px;
            text-align: center;
            color: var(--text-secondary);
            line-height: 1.5;
            margin: 0 0 28px;
            max-width: 340px;
            margin-left: auto;
            margin-right: auto;
        }

        .pricing-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-bottom: 24px;
        }

        .pricing-card {
            background: var(--btn-num-bg);
            border: 1px solid var(--border-subtle);
            border-radius: 18px;
            padding: 18px 10px;
            text-align: center;
            cursor: pointer;
            transition: all 0.25s cubic-bezier(0.25, 0.1, 0.25, 1);
            position: relative;
        }

        .pricing-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
        }

        .pricing-card.featured {
            background: #1a1a1a;
            border-color: #1a1a1a;
            transform: scale(1.04);
            z-index: 2;
        }

        body.theme-midnight .pricing-card.featured {
            background: #ffffff;
            border-color: #ffffff;
        }

        body.theme-midnight .pricing-card.featured .plan-name,
        body.theme-midnight .pricing-card.featured .plan-desc {
            color: var(--text-secondary);
        }

        body.theme-midnight .pricing-card.featured .plan-price {
            color: var(--text-primary);
        }

        body.theme-midnight .pricing-card.featured .badge {
            background: #1a1a1a;
            color: #fff;
        }

        .pricing-card.featured:hover {
            transform: scale(1.07) translateY(-3px);
        }

        .pricing-card.featured .plan-name {
            color: rgba(255, 255, 255, 0.7);
        }

        .pricing-card.featured .plan-price {
            color: #ffffff;
        }

        .pricing-card.featured .plan-desc {
            color: rgba(255, 255, 255, 0.6);
        }

        .badge {
            position: absolute;
            top: -10px;
            left: 50%;
            transform: translateX(-50%);
            background: #1a1a1a;
            color: #fff;
            font-size: 9px;
            font-weight: 700;
            padding: 4px 10px;
            border-radius: 10px;
            letter-spacing: 0.8px;
            text-transform: uppercase;
            white-space: nowrap;
        }

        .pricing-card.featured .badge {
            background: #fff;
            color: #1a1a1a;
        }

        .plan-name {
            font-size: 13px;
            font-weight: 500;
            color: var(--text-secondary);
            margin-bottom: 6px;
        }

        .plan-price {
            font-size: 22px;
            font-weight: 700;
            color: var(--text-primary);
            margin-bottom: 6px;
            letter-spacing: -0.5px;
        }

        .plan-desc {
            font-size: 11px;
            color: var(--text-secondary);
            line-height: 1.4;
        }

        .btn-unlock {
            width: 100%;
            padding: 16px;
            border-radius: 16px;
            border: none;
            background: #1a1a1a;
            color: #fff;
            font-family: 'Inter', sans-serif;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
            letter-spacing: 0.3px;
        }

        .btn-unlock:hover:not(:disabled) {
            transform: scale(1.02);
            background: #333;
        }

        .btn-unlock:disabled {
            opacity: 0.6;
            cursor: wait;
        }

        .btn-later {
            width: 100%;
            padding: 14px;
            border-radius: 16px;
            border: none;
            background: transparent;
            color: var(--text-secondary);
            font-family: 'Inter', sans-serif;
            font-size: 14px;
            cursor: pointer;
            margin-top: 4px;
            transition: color 0.2s;
        }

        .btn-later:hover {
            color: var(--text-primary);
        }

        /* Login Modal */
        .login-panel h3 {
            font-size: 22px;
            font-weight: 600;
            margin: 0 0 6px;
            text-align: center;
            color: var(--text-primary);
        }

        .login-panel>p {
            text-align: center;
            color: var(--text-secondary);
            font-size: 14px;
            margin: 0 0 24px;
        }

        .login-form {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-bottom: 16px;
        }

        .login-form input {
            padding: 14px 16px;
            border-radius: 14px;
            border: 1px solid var(--border-subtle);
            background: var(--btn-num-bg);
            color: var(--text-primary);
            font-family: 'Inter', sans-serif;
            font-size: 15px;
            outline: none;
            transition: all 0.2s;
        }

        .login-form input:focus {
            border-color: var(--text-secondary);
            box-shadow: 0 0 0 3px rgba(0, 0, 0, 0.04);
        }

        .btn-primary {
            padding: 14px;
            border-radius: 14px;
            border: none;
            background: var(--btn-op-bg);
            color: var(--btn-op-text);
            font-family: 'Inter', sans-serif;
            font-size: 15px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }

        .btn-primary:hover {
            transform: scale(1.02);
            opacity: 0.9;
        }

        .divider {
            display: flex;
            align-items: center;
            gap: 12px;
            margin: 16px 0;
            color: var(--text-secondary);
            font-size: 13px;
        }

        .divider::before,
        .divider::after {
            content: '';
            flex: 1;
            height: 1px;
            background: var(--border-subtle);
        }

        .btn-google,
        .btn-userid {
            width: 100%;
            padding: 12px;
            border-radius: 14px;
            border: 1px solid var(--border-subtle);
            background: var(--btn-num-bg);
            color: var(--text-primary);
            font-family: 'Inter', sans-serif;
            font-size: 14px;
            font-weight: 500;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            transition: all 0.2s;
            margin-bottom: 8px;
        }

        .btn-google:hover {
            background: var(--btn-function-bg);
        }

        .btn-userid {
            border-style: dashed;
        }

        .btn-userid:hover {
            border-style: solid;
            background: var(--btn-function-bg);
        }

        .icon-google {
            width: 18px;
            height: 18px;
        }

        /* Theme Modal */
        .theme-panel h3 {
            font-size: 20px;
            font-weight: 600;
            margin: 0 0 20px;
            text-align: center;
            color: var(--text-primary);
        }

        .theme-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 14px;
        }

        .theme-option {
            position: relative;
            cursor: pointer;
            text-align: center;
            transition: transform 0.2s;
        }

        .theme-option:hover {
            transform: scale(1.03);
        }

        .theme-preview {
            width: 100%;
            height: 80px;
            border-radius: 16px;
            margin-bottom: 8px;
            border: 2px solid transparent;
            transition: all 0.2s;
            position: relative;
            overflow: hidden;
        }

        .theme-option.active .theme-preview {
            border-color: var(--text-primary);
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
        }

        .theme-white {
            background: #f2f2f7;
        }

        .theme-midnight {
            background: #1c1c1e;
        }

        .theme-frost {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.2), rgba(255, 255, 255, 0.1));
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .theme-neon {
            background: linear-gradient(135deg, #ff00aa, #00f0ff);
        }

        .theme-option span {
            font-size: 13px;
            font-weight: 500;
            color: var(--text-secondary);
        }

        .theme-lock {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -70%);
            width: 24px;
            height: 24px;
            color: var(--text-primary);
            background: rgba(255, 255, 255, 0.9);
            border-radius: 50%;
            padding: 4px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        }

        .theme-lock.hidden {
            display: none;
        }

        /* Shortcuts */
        .shortcuts-panel h3 {
            font-size: 20px;
            font-weight: 600;
            margin: 0 0 20px;
            text-align: center;
            color: var(--text-primary);
        }

        .shortcuts-list {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .shortcut-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 14px;
            background: var(--btn-num-bg);
            border-radius: 10px;
            font-size: 14px;
            color: var(--text-primary);
        }

        .key {
            font-family: 'SF Mono', monospace;
            font-size: 12px;
            font-weight: 600;
            background: var(--btn-function-bg);
            padding: 4px 8px;
            border-radius: 6px;
            color: var(--text-secondary);
            border: 1px solid var(--border-subtle);
        }

        /* VIP Overlay */
        .vip-overlay {
            position: fixed;
            inset: 0;
            background: rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(30px);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            opacity: 0;
            visibility: hidden;
            transition: all 0.6s ease;
            flex-direction: column;
        }

        .vip-overlay.active {
            opacity: 1;
            visibility: visible;
        }

        .vip-glow {
            position: absolute;
            width: 400px;
            height: 400px;
            background: radial-gradient(circle, rgba(255, 215, 0, 0.25) 0%, transparent 65%);
            border-radius: 50%;
            animation: vipPulse 2.5s ease infinite;
            pointer-events: none;
        }

        @keyframes vipPulse {
            0% {
                transform: scale(0.9);
                opacity: 0.4;
            }

            50% {
                transform: scale(1.1);
                opacity: 0.8;
            }

            100% {
                transform: scale(0.9);
                opacity: 0.4;
            }
        }

        .vip-text {
            font-size: 24px;
            font-weight: 500;
            color: #ffffff;
            letter-spacing: 0.5px;
            position: relative;
            z-index: 2;
            text-shadow: 0 2px 20px rgba(0, 0, 0, 0.3);
            animation: fadeIn 1s ease 0.3s both;
        }

        .vip-sub {
            font-size: 14px;
            color: rgba(255, 255, 255, 0.7);
            margin-top: 12px;
            position: relative;
            z-index: 2;
            animation: fadeIn 1s ease 0.6s both;
        }

        /* Toast */
        .toast-container {
            position: fixed;
            bottom: 32px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 3000;
            display: flex;
            flex-direction: column;
            gap: 8px;
            pointer-events: none;
        }

        .toast {
            background: rgba(0, 0, 0, 0.8);
            color: #fff;
            padding: 12px 24px;
            border-radius: 24px;
            font-size: 14px;
            font-weight: 500;
            opacity: 0;
            transform: translateY(20px);
            transition: all 0.4s cubic-bezier(0.25, 0.1, 0.25, 1);
            backdrop-filter: blur(10px);
            white-space: nowrap;
        }

        .toast.show {
            opacity: 1;
            transform: translateY(0);
        }

        /* Animations */
        @keyframes fadeIn {
            from {
                opacity: 0;
            }

            to {
                opacity: 1;
            }
        }

        /* Responsive */
        @media (max-width: 480px) {
            .calculator-card {
                max-width: 100%;
                border-radius: 0;
                min-height: 100dvh;
                display: flex;
                flex-direction: column;
                justify-content: center;
                animation: none;
            }

            .btn {
                width: 72px;
                height: 72px;
                font-size: 28px;
            }

            .result {
                font-size: 64px;
            }

            .side-panel {
                width: 100%;
            }

            .pricing-grid {
                grid-template-columns: 1fr;
            }

            .pricing-card.featured {
                transform: scale(1);
                order: -1;
            }

            .pricing-card.featured:hover {
                transform: scale(1.03);
            }
        }

        @media (max-width: 360px) {
            .btn {
                width: 60px;
                height: 60px;
                font-size: 24px;
            }

            .result {
                font-size: 48px;
            }
        }
    </style>
</head>

<body>

    <!-- VIP Welcome -->
    <div class="vip-overlay" id="vipOverlay">
        <div class="vip-glow"></div>
        <div class="vip-text">Welcome back, Pro User ✨</div>
        <div class="vip-sub">Your premium experience is active</div>
    </div>

    <!-- Main App -->
    <div class="app-container">
        <div class="calculator-card" id="calculatorCard">
            <!-- Header -->
            <div class="calc-header">
                <div class="brand">ProDigits</div>
                <div class="header-actions">
                    <span id="premiumBadge" class="premium-badge hidden">
                        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                            stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
                            <polyline points="20 6 9 17 4 12"></polyline>
                        </svg>
                        Premium Active
                    </span>
                    <button class="icon-btn" onclick="toggleMenu(event)" aria-label="Menu">
                        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"
                            stroke-linecap="round" stroke-linejoin="round">
                            <line x1="3" y1="12" x2="21" y2="12"></line>
                            <line x1="3" y1="6" x2="21" y2="6"></line>
                            <line x1="3" y1="18" x2="21" y2="18"></line>
                        </svg>
                    </button>
                </div>
            </div>

            <!-- Menu -->
            <div class="menu-dropdown" id="menuDropdown">
                <div class="menu-item" onclick="toggleHistory()">
                    History
                    <svg class="lock-hint" id="menuLockHistory" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                        stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                        <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                    </svg>
                </div>
                <div class="menu-item" onclick="toggleMemory()">
                    Memory
                    <svg class="lock-hint" id="menuLockMemory" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                        stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                        <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                    </svg>
                </div>
                <div class="menu-item" onclick="showThemes()">
                    Themes
                    <svg class="lock-hint" id="menuLockThemes" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                        stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                        <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                    </svg>
                </div>
                <div class="menu-item" onclick="showShortcuts()">
                    Shortcuts
                    <svg class="lock-hint" id="menuLockShortcuts" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                        stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                        <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                        <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                    </svg>
                </div>
                <div class="menu-divider"></div>
                <div class="menu-item" onclick="handleLoginLogout()" id="loginMenuItem">Sign In</div>
            </div>

            <!-- Display -->
            <div class="calc-display">
                <div id="expressionDisplay" class="expression"></div>
                <div id="resultDisplay" class="result">0</div>
            </div>

            <!-- Buttons -->
            <div class="button-grid">
                <button class="btn btn-function" onclick="clearAll()" data-ripple aria-label="All Clear">AC</button>
                <button class="btn btn-function" onclick="toggleSign()" data-ripple aria-label="Toggle Sign">±</button>
                <button class="btn btn-function" onclick="percentage()" data-ripple aria-label="Percentage">%</button>
                <button class="btn btn-operator" onclick="appendOperator('÷')" data-ripple
                    aria-label="Divide">÷</button>

                <button class="btn btn-number" onclick="appendNumber('7')" data-ripple aria-label="Seven">7</button>
                <button class="btn btn-number" onclick="appendNumber('8')" data-ripple aria-label="Eight">8</button>
                <button class="btn btn-number" onclick="appendNumber('9')" data-ripple aria-label="Nine">9</button>
                <button class="btn btn-operator" onclick="appendOperator('×')" data-ripple
                    aria-label="Multiply">×</button>

                <button class="btn btn-number" onclick="appendNumber('4')" data-ripple aria-label="Four">4</button>
                <button class="btn btn-number" onclick="appendNumber('5')" data-ripple aria-label="Five">5</button>
                <button class="btn btn-number" onclick="appendNumber('6')" data-ripple aria-label="Six">6</button>
                <button class="btn btn-operator" onclick="appendOperator('−')" data-ripple
                    aria-label="Subtract">−</button>

                <button class="btn btn-number" onclick="appendNumber('1')" data-ripple aria-label="One">1</button>
                <button class="btn btn-number" onclick="appendNumber('2')" data-ripple aria-label="Two">2</button>
                <button class="btn btn-number" onclick="appendNumber('3')" data-ripple aria-label="Three">3</button>
                <button class="btn btn-operator" onclick="appendOperator('+')" data-ripple aria-label="Add">+</button>

                <button class="btn btn-function" onclick="toggleHistory()" data-ripple aria-label="History">
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                        stroke-linejoin="round">
                        <polyline points="1 4 1 10 7 10"></polyline>
                        <path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10"></path>
                    </svg>
                </button>
                <button class="btn btn-number" onclick="appendNumber('0')" data-ripple aria-label="Zero">0</button>
                <button class="btn btn-number" onclick="appendNumber('.')" data-ripple aria-label="Decimal">.</button>
                <button class="btn btn-operator btn-equals" onclick="calculate()" data-ripple
                    aria-label="Equals">=</button>
            </div>
        </div>

        <!-- History Panel -->
        <div class="side-panel" id="historyPanel">
            <div class="panel-header">
                <h3>Calculation History</h3>
                <button onclick="toggleHistory()">Close</button>
            </div>
            <div class="panel-content" id="historyList">
                <div class="empty-state">Calculations appear here after you solve them</div>
            </div>
        </div>

        <!-- Memory Panel -->
        <div class="side-panel" id="memoryPanel">
            <div class="panel-header">
                <h3>Memory</h3>
                <button onclick="toggleMemory()">Close</button>
            </div>
            <div class="panel-content">
                <div class="memory-value" id="memoryDisplay">M: 0</div>
                <div class="memory-actions">
                    <button onclick="memoryAdd()">M+</button>
                    <button onclick="memorySubtract()">M−</button>
                    <button onclick="memoryRecall()">MR</button>
                    <button onclick="memoryClear()">MC</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Premium Modal -->
    <div class="modal-backdrop" id="premiumModal" onclick="closeModalOnBackdrop(event, 'premiumModal')">
        <div class="modal-panel premium-panel">
            <button class="modal-close" onclick="closePremiumModal()" aria-label="Close">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                    stroke-linejoin="round">
                    <line x1="18" y1="6" x2="6" y2="18"></line>
                    <line x1="6" y1="6" x2="18" y2="18"></line>
                </svg>
            </button>

            <div class="premium-icon">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"
                    stroke-linejoin="round">
                    <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                    <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                </svg>
            </div>

            <h2 class="premium-title">Unlock ProDigits Pro</h2>
            <p class="premium-subtitle">Experience lightning-fast calculations, elegant design, synced access and
                premium tools.</p>

            <div class="pricing-grid">
                <div class="pricing-card" onclick="selectPlan('1month')">
                    <div class="plan-name">1 Month</div>
                    <div class="plan-price">$2.99</div>
                    <div class="plan-desc">Affordable trial<br>Monthly billing</div>
                </div>
                <div class="pricing-card featured" onclick="selectPlan('6months')">
                    <div class="badge">Best Value</div>
                    <div class="plan-name">6 Months</div>
                    <div class="plan-price">$12.99</div>
                    <div class="plan-desc">Popular option<br>Save 15%</div>
                </div>
                <div class="pricing-card" onclick="selectPlan('1year')">
                    <div class="badge">Save More</div>
                    <div class="plan-name">1 Year</div>
                    <div class="plan-price">$19.99</div>
                    <div class="plan-desc">Biggest savings<br>Premium ownership</div>
                </div>
            </div>

            <button class="btn-unlock" id="unlockBtn" onclick="showLoginModal()">Unlock Pro</button>
            <button class="btn-later" onclick="closePremiumModal()">Maybe Later</button>
        </div>
    </div>

    <!-- Login Modal -->
    <div class="modal-backdrop" id="loginModal" onclick="closeModalOnBackdrop(event, 'loginModal')">
        <div class="modal-panel login-panel">
            <button class="modal-close" onclick="closeLoginModal()" aria-label="Close">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                    stroke-linejoin="round">
                    <line x1="18" y1="6" x2="6" y2="18"></line>
                    <line x1="6" y1="6" x2="18" y2="18"></line>
                </svg>
            </button>

            <h3>Account Access</h3>
            <p>Sign in to unlock your premium experience</p>

            <div class="login-form">
                <input type="email" id="loginEmail" placeholder="Email address">
                <input type="password" id="loginPassword" placeholder="Password">
                <button class="btn-primary" onclick="loginEmail()">Sign In</button>
            </div>

            <div class="divider"><span>or</span></div>

            <button class="btn-google" onclick="loginGoogle()">
                <svg class="icon-google" viewBox="0 0 24 24">
                    <path fill="#4285F4"
                        d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" />
                    <path fill="#34A853"
                        d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" />
                    <path fill="#FBBC05"
                        d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" />
                    <path fill="#EA4335"
                        d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" />
                </svg>
                Continue with Google
            </button>

            <button class="btn-userid" onclick="loginUserId()">Use User ID</button>
        </div>
    </div>

    <!-- Theme Modal -->
    <div class="modal-backdrop" id="themeModal" onclick="closeModalOnBackdrop(event, 'themeModal')">
        <div class="modal-panel theme-panel">
            <button class="modal-close" onclick="closeThemeModal()" aria-label="Close" style="top:12px;right:12px;">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                    stroke-linejoin="round">
                    <line x1="18" y1="6" x2="6" y2="18"></line>
                    <line x1="6" y1="6" x2="18" y2="18"></line>
                </svg>
            </button>
            <h3>Select Theme</h3>
            <div class="theme-grid">
                <div class="theme-option active" id="themeOptWhite" onclick="setTheme('white')">
                    <div class="theme-preview theme-white"></div>
                    <span>Minimal White</span>
                </div>
                <div class="theme-option" id="themeOptMidnight" onclick="setTheme('midnight')">
                    <div class="theme-preview theme-midnight">
                        <svg class="theme-lock" id="lockMidnight" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                            stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                            <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                            <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                        </svg>
                    </div>
                    <span>Midnight Black</span>
                </div>
                <div class="theme-option" id="themeOptFrost" onclick="setTheme('frost')">
                    <div class="theme-preview theme-frost">
                        <svg class="theme-lock" id="lockFrost" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                            stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                            <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                            <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                        </svg>
                    </div>
                    <span>Frost Glass</span>
                </div>
                <div class="theme-option" id="themeOptNeon" onclick="setTheme('neon')">
                    <div class="theme-preview theme-neon">
                        <svg class="theme-lock" id="lockNeon" viewBox="0 0 24 24" fill="none" stroke="currentColor"
                            stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                            <rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect>
                            <path d="M7 11V7a5 5 0 0 1 10 0v4"></path>
                        </svg>
                    </div>
                    <span>Neon Pro</span>
                </div>
            </div>
        </div>
    </div>

    <!-- Shortcuts Modal -->
    <div class="modal-backdrop" id="shortcutsModal" onclick="closeModalOnBackdrop(event, 'shortcutsModal')">
        <div class="modal-panel shortcuts-panel">
            <button class="modal-close" onclick="closeShortcutsModal()" aria-label="Close" style="top:12px;right:12px;">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                    stroke-linejoin="round">
                    <line x1="18" y1="6" x2="6" y2="18"></line>
                    <line x1="6" y1="6" x2="18" y2="18"></line>
                </svg>
            </button>
            <h3>Keyboard Shortcuts</h3>
            <div class="shortcuts-list">
                <div class="shortcut-row"><span class="key">0-9</span><span>Enter numbers</span></div>
                <div class="shortcut-row"><span class="key">. </span><span>Decimal point</span></div>
                <div class="shortcut-row"><span class="key">+ − × /</span><span>Operators</span></div>
                <div class="shortcut-row"><span class="key">Enter</span><span>Calculate</span></div>
                <div class="shortcut-row"><span class="key">Esc</span><span>Clear all</span></div>
                <div class="shortcut-row"><span class="key">Backspace</span><span>Delete digit</span></div>
                <div class="shortcut-row"><span class="key">H</span><span>Toggle History</span></div>
                <div class="shortcut-row"><span class="key">M</span><span>Toggle Memory</span></div>
                <div class="shortcut-row"><span class="key">T</span><span>Open Themes</span></div>
            </div>
        </div>
    </div>

    <!-- Toast -->
    <div class="toast-container" id="toastContainer"></div>

    <script>
        // State
        const state = {
            currentInput: '0',
            expression: '',
            history: [],
            memory: null,
            isPremium: false,
            user: null,
            theme: 'white',
            lastWasResult: false,
            selectedPlan: null
        };

        let audioCtx = null;

        // Init Audio
        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
        }

        // Sound Engine
        function playSound(type) {
            if (!audioCtx) return;
            const now = audioCtx.currentTime;

            const playTone = (freq, dur, vol = 0.03, delay = 0) => {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.type = 'sine';
                osc.frequency.setValueAtTime(freq, now + delay);
                gain.gain.setValueAtTime(vol, now + delay);
                gain.gain.exponentialRampToValueAtTime(0.001, now + delay + dur);
                osc.start(now + delay);
                osc.stop(now + delay + dur);
            };

            switch (type) {
                case 'num': playTone(880, 0.06, 0.025); break;
                case 'op': playTone(600, 0.08, 0.03); break;
                case 'eq': playTone(1200, 0.12, 0.035); break;
                case 'clear': playTone(440, 0.1, 0.02); break;
                case 'paywall': playTone(200, 0.18, 0.02); break;
                case 'unlock':
                    playTone(660, 0.15, 0.03);
                    playTone(880, 0.2, 0.03, 0.15);
                    playTone(1320, 0.3, 0.03, 0.35);
                    break;
                case 'error': playTone(150, 0.2, 0.02); break;
            }
        }

        // Haptics
        function haptic(type) {
            if (!state.isPremium || !navigator.vibrate) return;
            if (type === 'click') navigator.vibrate(3);
            if (type === 'calculate') navigator.vibrate([5, 40, 5]);
            if (type === 'unlock') navigator.vibrate([10, 30, 10, 30, 20]);
        }

        // Ripple
        function createRipple(e, btn) {
            const ripple = document.createElement('span');
            const rect = btn.getBoundingClientRect();
            const size = Math.max(rect.width, rect.height);
            ripple.style.width = ripple.style.height = size + 'px';
            ripple.style.left = (e.clientX - rect.left - size / 2) + 'px';
            ripple.style.top = (e.clientY - rect.top - size / 2) + 'px';
            ripple.className = 'ripple';
            btn.appendChild(ripple);
            setTimeout(() => ripple.remove(), 600);
        }

        // Display
        function updateDisplay() {
            document.getElementById('expressionDisplay').textContent = state.expression;
            document.getElementById('resultDisplay').textContent = state.currentInput;
        }

        // Core Calculator
        function appendNumber(num) {
            initAudio();
            if (state.lastWasResult) {
                state.currentInput = '';
                state.expression = '';
                state.lastWasResult = false;
            }
            if (num === '.' && state.currentInput.includes('.')) return;
            if (state.currentInput === '0' && num !== '.') state.currentInput = '';
            if (state.currentInput.length > 15) return;
            state.currentInput += num;
            updateDisplay();
            playSound('num');
            haptic('click');
        }

        function appendOperator(op) {
            initAudio();
            if (state.lastWasResult) {
                state.expression = state.currentInput + ' ' + op + ' ';
                state.currentInput = '0';
                state.lastWasResult = false;
            } else {
                if (state.currentInput !== '') {
                    state.expression += state.currentInput + ' ' + op + ' ';
                    state.currentInput = '0';
                }
            }
            updateDisplay();
            playSound('op');
            haptic('click');
        }

        function clearAll() {
            initAudio();
            state.currentInput = '0';
            state.expression = '';
            state.lastWasResult = false;
            updateDisplay();
            playSound('clear');
        }

        function backspace() {
            if (state.lastWasResult) {
                clearAll();
                return;
            }
            if (state.currentInput.length > 1) {
                state.currentInput = state.currentInput.slice(0, -1);
            } else {
                state.currentInput = '0';
            }
            updateDisplay();
        }

        function toggleSign() {
            if (state.currentInput === '0') return;
            if (state.currentInput.startsWith('-')) {
                state.currentInput = state.currentInput.slice(1);
            } else {
                state.currentInput = '-' + state.currentInput;
            }
            updateDisplay();
            playSound('op');
        }

        function percentage() {
            const val = parseFloat(state.currentInput);
            if (isNaN(val)) return;
            state.currentInput = String(val / 100);
            updateDisplay();
            playSound('op');
        }

        function evaluate(expr) {
            expr = expr.replace(/×/g, '*').replace(/÷/g, '/').replace(/−/g, '-');
            expr = expr.replace(/\s/g, '');
            if (!/^[0-9+\-*/().]+$/.test(expr)) throw new Error('Invalid');
            const result = Function('"use strict"; return (' + expr + ')')();
            if (!isFinite(result)) throw new Error('Infinity');
            return Math.round(result * 1e12) / 1e12; // Avoid floating point weirdness
        }

        function calculate() {
            initAudio();
            if (!state.isPremium) {
                showPremiumModal();
                return;
            }

            const fullExpr = state.expression + state.currentInput;
            if (!fullExpr.trim()) return;

            try {
                const result = evaluate(fullExpr);
                addToHistory(fullExpr, result);

                document.getElementById('expressionDisplay').textContent = fullExpr + ' =';
                const resEl = document.getElementById('resultDisplay');
                resEl.textContent = result;
                resEl.classList.add('glow', 'pop');
                setTimeout(() => resEl.classList.remove('glow', 'pop'), 1000);

                state.currentInput = String(result);
                state.expression = '';
                state.lastWasResult = true;
                playSound('eq');
                haptic('calculate');
            } catch (e) {
                state.currentInput = 'Error';
                state.expression = '';
                state.lastWasResult = true;
                updateDisplay();
                playSound('error');
            }
        }

        // History
        function addToHistory(expr, result) {
            state.history.unshift({
                expr,
                result,
                time: new Date()
            });
            if (state.history.length > 50) state.history.pop();
            renderHistory();
            if (state.isPremium) {
                saveState();
                showToast('Synced to cloud');
            }
        }

        function renderHistory() {
            const list = document.getElementById('historyList');
            if (state.history.length === 0) {
                list.innerHTML = '<div class="empty-state">Calculations appear here after you solve them</div>';
                return;
            }
            list.innerHTML = state.history.map((item, i) => `
        <div class="history-item" onclick="loadHistoryItem(${i})">
          <div class="history-expr">${item.expr} =</div>
          <div class="history-result">${item.result}</div>
          <div class="history-time">${formatTime(item.time)}</div>
        </div>
      `).join('');
        }

        function loadHistoryItem(index) {
            const item = state.history[index];
            if (!item) return;
            state.currentInput = String(item.result);
            state.expression = '';
            state.lastWasResult = true;
            updateDisplay();
            toggleHistory();
            playSound('num');
        }

        function formatTime(date) {
            return new Intl.DateTimeFormat('en-US', {
                hour: 'numeric',
                minute: 'numeric',
                month: 'short',
                day: 'numeric'
            }).format(date);
        }

        // Memory
        function updateMemoryDisplay() {
            document.getElementById('memoryDisplay').textContent = 'M: ' + (state.memory !== null ? state.memory : 0);
        }

        function memoryAdd() {
            if (!checkPremiumFeature()) return;
            const val = parseFloat(state.currentInput) || 0;
            state.memory = (state.memory || 0) + val;
            updateMemoryDisplay();
            playSound('op');
            showToast('Added to memory');
            saveState();
        }

        function memorySubtract() {
            if (!checkPremiumFeature()) return;
            const val = parseFloat(state.currentInput) || 0;
            state.memory = (state.memory || 0) - val;
            updateMemoryDisplay();
            playSound('op');
            showToast('Subtracted from memory');
            saveState();
        }

        function memoryRecall() {
            if (!checkPremiumFeature()) return;
            state.currentInput = String(state.memory || 0);
            state.lastWasResult = false;
            updateDisplay();
            playSound('num');
            toggleMemory();
        }

        function memoryClear() {
            if (!checkPremiumFeature()) return;
            state.memory = null;
            updateMemoryDisplay();
            playSound('clear');
            showToast('Memory cleared');
            saveState();
        }

        // UI Toggles
        function toggleMenu(e) {
            e.stopPropagation();
            document.getElementById('menuDropdown').classList.toggle('active');
            updateMenuLocks();
        }

        function updateMenuLocks() {
            const locks = ['History', 'Memory', 'Themes', 'Shortcuts'];
            locks.forEach(feature => {
                const el = document.getElementById('menuLock' + feature);
                if (el) el.classList.toggle('hidden', state.isPremium);
            });
        }

        function toggleHistory() {
            document.getElementById('menuDropdown').classList.remove('active');
            if (!state.isPremium) { showPremiumModal(); return; }
            document.getElementById('historyPanel').classList.toggle('active');
        }

        function toggleMemory() {
            document.getElementById('menuDropdown').classList.remove('active');
            if (!state.isPremium) { showPremiumModal(); return; }
            document.getElementById('memoryPanel').classList.toggle('active');
        }

        // Premium & Login
        function checkPremiumFeature() {
            if (!state.isPremium) {
                showPremiumModal();
                return false;
            }
            return true;
        }

        function showPremiumModal() {
            initAudio();
            document.getElementById('premiumModal').classList.add('active');
            playSound('paywall');
            document.getElementById('menuDropdown').classList.remove('active');
        }

        function closePremiumModal() {
            document.getElementById('premiumModal').classList.remove('active');
        }

        function showLoginModal() {
            closePremiumModal();
            document.getElementById('loginModal').classList.add('active');
        }

        function closeLoginModal() {
            document.getElementById('loginModal').classList.remove('active');
        }

        function selectPlan(plan) {
            state.selectedPlan = plan;
            document.querySelectorAll('.pricing-card').forEach(c => c.style.borderColor = '');
            event.currentTarget.style.borderColor = '#ffd700';
        }

        function completePurchase() {
            const btn = document.getElementById('unlockBtn');
            const original = btn.textContent;
            btn.textContent = 'Processing...';
            btn.disabled = true;

            setTimeout(() => {
                state.isPremium = true;
                saveState();
                updateHeader();
                closePremiumModal();
                closeLoginModal();
                showVIPWelcome();
                showToast('Premium Activated ✓');
                playSound('unlock');
                haptic('unlock');
                btn.textContent = original;
                btn.disabled = false;
                updateMenuLocks();
            }, 1500);
        }

        // Auth Methods
        function loginEmail() {
            const email = document.getElementById('loginEmail').value;
            const pass = document.getElementById('loginPassword').value;
            if (email && pass) {
                state.user = { name: email.split('@')[0], email, id: 'email_' + Date.now() };
                completePurchase();
            } else {
                showToast('Please enter email and password');
            }
        }

        function loginGoogle() {
            state.user = { name: 'Alex', email: 'alex@gmail.com', id: 'google_12345' };
            completePurchase();
        }

        function loginUserId() {
            const id = prompt('Enter your User ID:');
            if (id) {
                state.user = { name: id, email: '', id: 'userid_' + id };
                completePurchase();
            }
        }

        function handleLoginLogout() {
            document.getElementById('menuDropdown').classList.remove('active');
            if (state.user) {
                state.user = null;
                state.isPremium = false;
                state.history = [];
                state.memory = null;
                saveState();
                updateHeader();
                updateMenuLocks();
                setTheme('white');
                showToast('Signed out');
            } else {
                showLoginModal();
            }
        }

        // Themes
        function showThemes() {
            document.getElementById('menuDropdown').classList.remove('active');
            document.getElementById('themeModal').classList.add('active');
            document.getElementById('lockMidnight').classList.toggle('hidden', state.isPremium);
            document.getElementById('lockFrost').classList.toggle('hidden', state.isPremium);
            document.getElementById('lockNeon').classList.toggle('hidden', state.isPremium);
        }

        function closeThemeModal() {
            document.getElementById('themeModal').classList.remove('active');
        }

        function setTheme(themeName) {
            if (themeName !== 'white' && !state.isPremium) {
                showPremiumModal();
                return;
            }
            document.body.className = themeName === 'white' ? '' : 'theme-' + themeName;
            state.theme = themeName;
            saveState();

            document.querySelectorAll('.theme-option').forEach(el => el.classList.remove('active'));
            document.getElementById('themeOpt' + themeName.charAt(0).toUpperCase() + themeName.slice(1)).classList.add('active');
            playSound('op');
        }

        // Shortcuts
        function showShortcuts() {
            document.getElementById('menuDropdown').classList.remove('active');
            if (!state.isPremium) { showPremiumModal(); return; }
            document.getElementById('shortcutsModal').classList.add('active');
        }

        function closeShortcutsModal() {
            document.getElementById('shortcutsModal').classList.remove('active');
        }

        // Header
        function updateHeader() {
            const badge = document.getElementById('premiumBadge');
            const loginItem = document.getElementById('loginMenuItem');

            if (state.isPremium) {
                badge.classList.remove('hidden');
            } else {
                badge.classList.add('hidden');
            }

            loginItem.textContent = state.user ? 'Sign Out' : 'Sign In';
        }

        // VIP
        function showVIPWelcome() {
            const overlay = document.getElementById('vipOverlay');
            overlay.classList.add('active');
            setTimeout(() => overlay.classList.remove('active'), 2800);
        }

        // Toast
        function showToast(msg) {
            const container = document.getElementById('toastContainer');
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.textContent = msg;
            container.appendChild(toast);
            requestAnimationFrame(() => toast.classList.add('show'));
            setTimeout(() => {
                toast.classList.remove('show');
                setTimeout(() => toast.remove(), 400);
            }, 2500);
        }

        // Storage
        function saveState() {
            try {
                localStorage.setItem('proDigits_v1', JSON.stringify({
                    isPremium: state.isPremium,
                    user: state.user,
                    theme: state.theme,
                    history: state.history,
                    memory: state.memory
                }));
            } catch (e) { }
        }

        function loadState() {
            try {
                const saved = JSON.parse(localStorage.getItem('proDigits_v1') || '{}');
                if (saved.theme) setTheme(saved.theme);
                if (saved.history) state.history = saved.history;
                if (saved.memory !== undefined) state.memory = saved.memory;
                if (saved.isPremium) {
                    state.isPremium = saved.isPremium;
                    state.user = saved.user || { name: 'Pro User', id: 'local_' + Date.now() };
                }
            } catch (e) { }
        }

        // Modal Backdrop Click
        function closeModalOnBackdrop(e, id) {
            if (e.target.id === id) {
                document.getElementById(id).classList.remove('active');
            }
        }

        // Keyboard
        document.addEventListener('keydown', (e) => {
            initAudio();
            const key = e.key;

            if (key >= '0' && key <= '9') appendNumber(key);
            if (key === '.') appendNumber('.');
            if (key === '+') appendOperator('+');
            if (key === '-') appendOperator('−');
            if (key === '*') appendOperator('×');
            if (key === '/') { e.preventDefault(); appendOperator('÷'); }
            if (key === 'Enter' || key === '=') { e.preventDefault(); calculate(); }
            if (key === 'Escape') clearAll();
            if (key === 'Backspace') { e.preventDefault(); backspace(); }

            if (!state.isPremium) return;
            if (key === 'h' || key === 'H') toggleHistory();
            if (key === 'm' || key === 'M') toggleMemory();
            if (key === 't' || key === 'T') showThemes();
        });

        // Close menu when clicking outside
        document.addEventListener('click', (e) => {
            const menu = document.getElementById('menuDropdown');
            const header = document.querySelector('.calc-header');
            if (!header.contains(e.target)) {
                menu.classList.remove('active');
            }
        });

        // Ripple delegation
        document.querySelector('.button-grid').addEventListener('click', (e) => {
            const btn = e.target.closest('[data-ripple]');
            if (btn) createRipple(e, btn);
        });

        // Init
        function init() {
            loadState();
            updateDisplay();
            updateHeader();
            renderHistory();
            updateMemoryDisplay();
            updateMenuLocks();

            if (state.isPremium && state.user) {
                setTimeout(showVIPWelcome, 600);
            }
        }

        init();
    </script>
</body>

</html>
