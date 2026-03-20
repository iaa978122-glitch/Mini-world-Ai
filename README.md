<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Космический чат</title>
    <style>
        :root {
            --bg-color: #0a0e27;
            --panel-bg: #0f1425;
            --panel-light: #1a2040;
            --text-color: #e4e6eb;
            --user-msg-bg: #0066ff;
            --ai-msg-bg: #1a2332;
            --button-bg: #0066ff;
            --button-hover-bg: #0052cc;
            --border-color: #2a3450;
            --copy-bg: #2a3450;
            --copy-hover: #3a4460;
            --danger: #ff4757;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: #000;
            color: var(--text-color);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
        }

        #chat-container {
            display: flex;
            flex-direction: column;
            width: 95%;
            max-width: 900px;
            height: 95vh;
            background-color: var(--panel-bg);
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 20px 60px rgba(0, 102, 255, 0.3);
            border: 1px solid var(--border-color);
            position: relative;
        }

        #cosmic-bg {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: radial-gradient(ellipse at 20% 50%, rgba(0, 102, 255, 0.05) 0%, transparent 50%),
                        radial-gradient(ellipse at 80% 80%, rgba(147, 51, 234, 0.05) 0%, transparent 50%);
            pointer-events: none;
            z-index: 0;
        }

        #stars-container {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            pointer-events: none;
            overflow: hidden;
            z-index: 1;
        }

        .star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: white;
            border-radius: 50%;
            box-shadow: 0 0 3px rgba(255, 255, 255, 0.8);
            animation: twinkle 3s infinite;
        }

        .star.large {
            width: 3px;
            height: 3px;
            box-shadow: 0 0 5px rgba(255, 255, 255, 1);
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        .falling-star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: white;
            border-radius: 50%;
            box-shadow: 0 0 8px rgba(0, 102, 255, 0.8), 0 0 15px rgba(0, 102, 255, 0.4);
            animation: fall-star 4s linear forwards;
            opacity: 0.8;
        }

        @keyframes fall-star {
            0% {
                transform: translateY(-100px) translateX(0);
                opacity: 1;
            }
            100% {
                transform: translateY(calc(100vh + 100px)) translateX(30px);
                opacity: 0;
            }
        }

        .planet {
            position: absolute;
            border-radius: 50%;
            animation: float-planet 20s infinite ease-in-out;
        }

        .planet-1 {
            width: 80px;
            height: 80px;
            background: radial-gradient(circle at 30% 30%, #ff9500, #ff6b00);
            top: 10%;
            right: 5%;
            box-shadow: 0 0 30px rgba(255, 107, 0, 0.5), inset -2px -2px 10px rgba(0, 0, 0, 0.5);
            animation-delay: 0s;
        }

        .planet-2 {
            width: 60px;
            height: 60px;
            background: radial-gradient(circle at 40% 40%, #4a9eff, #1a5fb5);
            bottom: 15%;
            left: 3%;
            box-shadow: 0 0 25px rgba(74, 158, 255, 0.5), inset -2px -2px 8px rgba(0, 0, 0, 0.5);
            animation-delay: 2s;
        }

        .planet-3 {
            width: 50px;
            height: 50px;
            background: radial-gradient(circle at 35% 35%, #ff1744, #c41c3b);
            top: 50%;
            right: 2%;
            box-shadow: 0 0 20px rgba(255, 23, 68, 0.5), inset -1px -1px 6px rgba(0, 0, 0, 0.5);
            animation-delay: 1s;
        }

        .planet-4 {
            width: 70px;
            height: 70px;
            background: radial-gradient(circle at 30% 30%, #9c27b0, #6a1b9a);
            bottom: 5%;
            right: 10%;
            box-shadow: 0 0 28px rgba(156, 39, 176, 0.5), inset -2px -2px 10px rgba(0, 0, 0, 0.5);
            animation-delay: 3s;
        }

        @keyframes float-planet {
            0%, 100% {
                transform: translateY(0px) translateX(0px);
            }
            50% {
                transform: translateY(-30px) translateX(-20px);
            }
        }

        #top-panel {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 16px 24px;
            background: linear-gradient(135deg, rgba(15, 20, 37, 0.95) 0%, rgba(26, 32, 64, 0.95) 100%);
            border-bottom: 1px solid var(--border-color);
            position: relative;
            z-index: 10;
            backdrop-filter: blur(10px);
        }

        .top-left, .top-right {
            display: flex;
            gap: 12px;
            align-items: center;
        }

        .icon-btn {
            background-color: rgba(26, 32, 64, 0.8);
            color: var(--text-color);
            border: 1px solid var(--border-color);
            border-radius: 10px;
            padding: 10px 14px;
            cursor: pointer;
            font-size: 18px;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 44px;
            height: 44px;
            backdrop-filter: blur(5px);
        }

        .icon-btn:hover {
            background-color: var(--button-bg);
            border-color: var(--button-bg);
            transform: scale(1.05);
            box-shadow: 0 0 15px rgba(0, 102, 255, 0.5);
        }

        .current-model {
            font-size: 13px;
            color: var(--button-bg);
            font-weight: 600;
            padding: 8px 16px;
            background-color: rgba(26, 32, 64, 0.8);
            border-radius: 8px;
            border: 1px solid var(--border-color);
            backdrop-filter: blur(5px);
        }

        #history-panel {
            position: fixed;
            left: 0;
            top: 0;
            width: 320px;
            height: 100vh;
            background: linear-gradient(180deg, rgba(15, 20, 37, 0.95) 0%, rgba(10, 14, 39, 0.95) 100%);
            display: none;
            flex-direction: column;
            z-index: 300;
            box-shadow: 4px 0 15px rgba(0, 102, 255, 0.2);
            border-right: 1px solid var(--border-color);
            backdrop-filter: blur(10px);
        }

        #history-panel.active {
            display: flex;
        }

        .history-header {
            padding: 20px;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .history-close {
            background: none;
            border: none;
            color: var(--text-color);
            font-size: 24px;
            cursor: pointer;
            padding: 0;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 8px;
            transition: all 0.3s;
        }

        .history-close:hover {
            background-color: var(--panel-light);
            color: var(--button-bg);
        }

        .history-list {
            flex: 1;
            overflow-y: auto;
            padding: 12px;
        }

        .history-item {
            padding: 12px;
            margin-bottom: 8px;
            background-color: rgba(26, 32, 64, 0.6);
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            border: 2px solid transparent;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 8px;
            position: relative;
        }

        .history-item:hover {
            background-color: rgba(26, 32, 64, 0.9);
        }

        .history-item.active {
            background-color: var(--button-bg);
            border-color: var(--button-bg);
            box-shadow: 0 0 15px rgba(0, 102, 255, 0.5);
        }

        .history-item-name {
            flex: 1;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            font-size: 13px;
        }

        .history-item-delete {
            background: none;
            border: none;
            color: var(--danger);
            cursor: pointer;
            font-size: 18px;
            opacity: 0;
            transition: all 0.3s;
            padding: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 32px;
            height: 32px;
            border-radius: 6px;
        }

        .history-item:hover .history-item-delete {
            opacity: 1;
            background-color: rgba(255, 71, 87, 0.1);
        }

        .history-item-delete:hover {
            background-color: rgba(255, 71, 87, 0.2);
        }

        .history-footer {
            padding: 12px;
            border-top: 1px solid var(--border-color);
        }

        #new-chat-btn {
            width: 100%;
            background-color: var(--button-bg);
            color: white;
            border: none;
            border-radius: 10px;
            padding: 12px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
            font-size: 14px;
        }

        #new-chat-btn:hover {
            background-color: var(--button-hover-bg);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 102, 255, 0.3);
        }

        .history-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 299;
        }

        .history-overlay.active {
            display: block;
        }

        #chat {
            flex: 1;
            overflow-y: auto;
            padding: 24px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            position: relative;
            z-index: 2;
        }

        .message {
            display: flex;
            margin: 0;
            word-wrap: break-word;
        }

        .message-content {
            padding: 12px 16px;
            border-radius: 14px;
            max-width: 65%;
            word-wrap: break-word;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
            font-size: 14px;
            line-height: 1.4;
            backdrop-filter: blur(5px);
        }

        .message.user {
            justify-content: flex-end;
        }

        .message.user .message-content {
            background: linear-gradient(135deg, #0066ff 0%, #0052cc 100%);
            color: white;
            box-shadow: 0 4px 12px rgba(0, 102, 255, 0.3);
        }

        .message.ai {
            justify-content: flex-start;
        }

        .message.ai .message-content {
            background-color: rgba(26, 35, 50, 0.8);
            color: var(--text-color);
            border: 1px solid var(--border-color);
        }

        .thinking-indicator {
            display: flex;
            gap: 4px;
            align-items: center;
            padding: 12px 16px;
            background-color: rgba(26, 35, 50, 0.8);
            border-radius: 14px;
            border: 1px solid var(--border-color);
            max-width: 65%;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
        }

        .thinking-dot {
            width: 8px;
            height: 8px;
            background-color: var(--button-bg);
            border-radius: 50%;
            animation: thinking 1.4s infinite;
        }

        .thinking-dot:nth-child(2) {
            animation-delay: 0.2s;
        }

        .thinking-dot:nth-child(3) {
            animation-delay: 0.4s;
        }

        @keyframes thinking {
            0%, 60%, 100% {
                opacity: 0.3;
                transform: translateY(0);
            }
            30% {
                opacity: 1;
                transform: translateY(-8px);
            }
        }

        .script-box {
            background-color: rgba(26, 35, 50, 0.8);
            padding: 16px;
            border-radius: 10px;
            border: 1px solid var(--border-color);
            font-family: 'Courier New', monospace;
            color: var(--text-color);
            white-space: pre-wrap;
            overflow-x: auto;
            overflow-y: auto;
            font-size: 12px;
            position: relative;
            width: 100%;
            max-height: 400px;
            margin-left: 0;
            backdrop-filter: blur(5px);
            word-wrap: break-word;
            line-height: 1.5;
        }

        .script-box::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }

        .script-box::-webkit-scrollbar-track {
            background: rgba(26, 32, 64, 0.5);
            border-radius: 10px;
        }

        .script-box::-webkit-scrollbar-thumb {
            background: var(--button-bg);
            border-radius: 10px;
        }

        .script-box::-webkit-scrollbar-thumb:hover {
            background: var(--button-hover-bg);
        }

        #input-container {
            display: flex;
            padding: 16px 24px 24px;
            background: linear-gradient(135deg, rgba(15, 20, 37, 0.95) 0%, rgba(26, 32, 64, 0.95) 100%);
            gap: 12px;
            position: relative;
            z-index: 10;
            border-top: 1px solid var(--border-color);
            backdrop-filter: blur(10px);
        }

        input {
            flex: 1;
            padding: 12px 16px;
            border-radius: 12px;
            border: 1px solid var(--border-color);
            background-color: rgba(26, 32, 64, 0.8);
            color: var(--text-color);
            outline: none;
            font-size: 14px;
            transition: all 0.3s;
            backdrop-filter: blur(5px);
        }

        input::placeholder {
            color: #6a7590;
        }

        input:focus {
            border-color: var(--button-bg);
            background-color: rgba(26, 32, 64, 0.95);
            box-shadow: 0 0 0 3px rgba(0, 102, 255, 0.1);
        }

        #send-btn {
            background: linear-gradient(135deg, #0066ff 0%, #0052cc 100%);
            color: white;
            font-weight: 600;
            border: none;
            border-radius: 12px;
            padding: 12px 28px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 14px;
        }

        #send-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 102, 255, 0.4);
        }

        .copy-btn {
            background-color: rgba(42, 52, 80, 0.8);
            color: var(--text-color);
            border: none;
            border-radius: 6px;
            padding: 6px 10px;
            font-size: 11px;
            position: absolute;
            top: 8px;
            right: 8px;
            cursor: pointer;
            transition: all 0.3s;
            z-index: 5;
        }

        .copy-btn:hover {
            background-color: rgba(58, 68, 96, 0.9);
        }

        #suggestions {
            position: absolute;
            bottom: 90px;
            left: 24px;
            width: calc(100% - 48px);
            max-width: 600px;
            background-color: rgba(26, 32, 64, 0.95);
            border-radius: 12px;
            border: 1px solid var(--border-color);
            overflow: hidden;
            display: none;
            z-index: 100;
            max-height: 220px;
            overflow-y: auto;
            box-shadow: 0 10px 30px rgba(0, 102, 255, 0.2);
            backdrop-filter: blur(10px);
        }

        .suggestion-item {
            padding: 12px 16px;
            cursor: pointer;
            transition: background-color 0.3s;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            flex-direction: column;
            gap: 4px;
        }

        .suggestion-item:last-child {
            border-bottom: none;
        }

        .suggestion-item:hover {
            background-color: rgba(0, 102, 255, 0.2);
        }

        .suggestion-keyword {
            font-weight: 600;
            font-size: 13px;
        }

        .suggestion-hint {
            font-size: 12px;
            opacity: 0.7;
        }

        .empty-state {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100%;
            color: #6a7590;
            text-align: center;
            gap: 12px;
        }

        .empty-state-icon {
            font-size: 56px;
        }

        .empty-state-text {
            font-size: 14px;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.8);
            z-index: 500;
            justify-content: center;
            align-items: center;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: linear-gradient(135deg, rgba(15, 20, 37, 0.98) 0%, rgba(26, 32, 64, 0.98) 100%);
            padding: 28px;
            border-radius: 16px;
            width: 90%;
            max-width: 650px;
            max-height: 80vh;
            overflow-y: auto;
            border: 1px solid var(--border-color);
            position: relative;
            box-shadow: 0 20px 60px rgba(0, 102, 255, 0.2);
            backdrop-filter: blur(10px);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
        }

        .modal-header h2 {
            color: var(--button-bg);
            margin: 0;
            font-size: 20px;
        }

        .close-btn {
            background: none;
            border: none;
            color: var(--text-color);
            font-size: 28px;
            cursor: pointer;
            padding: 0;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 8px;
            transition: all 0.3s;
        }

        .close-btn:hover {
            background-color: var(--panel-light);
            color: var(--button-bg);
        }

        .tab-buttons {
            display: flex;
            gap: 8px;
            margin-bottom: 24px;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 12px;
        }

        .tab-btn {
            background-color: transparent;
            color: var(--text-color);
            border: none;
            padding: 10px 16px;
            cursor: pointer;
            border-bottom: 3px solid transparent;
            transition: all 0.3s;
            font-size: 13px;
            font-weight: 600;
        }

        .tab-btn.active {
            color: var(--button-bg);
            border-bottom-color: var(--button-bg);
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        textarea {
            width: 100%;
            height: 280px;
            border: 1px solid var(--border-color);
            background-color: rgba(26, 32, 64, 0.8);
            color: var(--text-color);
            padding: 12px;
            border-radius: 10px;
            font-family: 'Courier New', monospace;
            margin-bottom: 16px;
            resize: vertical;
            outline: none;
            font-size: 12px;
            transition: all 0.3s;
        }

        textarea:focus {
            border-color: var(--button-bg);
            box-shadow: 0 0 0 3px rgba(0, 102, 255, 0.1);
        }

        .input-group {
            margin-bottom: 16px;
        }

        .input-group label {
            display: block;
            margin-bottom: 6px;
            color: var(--text-color);
            font-weight: 600;
            font-size: 13px;
        }

        .input-group input[type="text"] {
            width: 100%;
            padding: 10px;
            border: 1px solid var(--border-color);
            background-color: rgba(26, 32, 64, 0.8);
            color: var(--text-color);
            border-radius: 8px;
            font-size: 13px;
            margin: 0;
            transition: all 0.3s;
        }

        .input-group input[type="text"]:focus {
            border-color: var(--button-bg);
            box-shadow: 0 0 0 3px rgba(0, 102, 255, 0.1);
        }

        #file-json {
            display: none;
        }

        #file-txt {
            display: none;
        }

        .file-upload-section {
            margin-bottom: 20px;
            padding: 16px;
            background-color: rgba(26, 32, 64, 0.6);
            border-radius: 10px;
            border: 1px solid var(--border-color);
        }

        .file-upload-section h3 {
            font-size: 13px;
            color: var(--button-bg);
            margin-bottom: 12px;
            font-weight: 600;
        }

        .file-upload-btn {
            background-color: rgba(0, 102, 255, 0.2);
            color: var(--button-bg);
            border: 1px dashed var(--button-bg);
            border-radius: 8px;
            padding: 12px 16px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
            font-size: 13px;
            text-align: center;
            margin-bottom: 8px;
            display: block;
            width: 100%;
        }

        .file-upload-btn:hover {
            background-color: rgba(0, 102, 255, 0.3);
        }

        .file-name-display {
            font-size: 12px;
            color: #6a7590;
            padding: 8px 0;
        }

        .button-group {
            display: flex;
            gap: 10px;
            justify-content: flex-end;
            margin-top: 20px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #0066ff 0%, #0052cc 100%);
            color: white;
            border: none;
            border-radius: 8px;
            padding: 10px 20px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
            font-size: 13px;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 102, 255, 0.3);
        }

        .btn-secondary {
            background-color: rgba(26, 32, 64, 0.8);
            color: var(--text-color);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 10px 20px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
            font-size: 13px;
        }

        .btn-secondary:hover {
            background-color: rgba(26, 32, 64, 0.95);
        }

        .success-message {
            background-color: #10b981;
            color: white;
            padding: 12px 16px;
            border-radius: 8px;
            margin-bottom: 16px;
            display: none;
            font-size: 13px;
        }

        .success-message.show {
            display: block;
        }

        .error-message {
            background-color: var(--danger);
            color: white;
            padding: 12px 16px;
            border-radius: 8px;
            margin-bottom: 16px;
            display: none;
            font-size: 13px;
        }

        .error-message.show {
            display: block;
        }

        .model-section {
            background-color: rgba(26, 32, 64, 0.6);
            padding: 16px;
            border-radius: 10px;
            margin-bottom: 12px;
            border: 1px solid var(--border-color);
        }

        .model-item-settings {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
        }

        .model-info {
            flex: 1;
        }

        .model-info strong {
            display: block;
            margin-bottom: 2px;
            font-size: 13px;
        }

        .model-info small {
            color: #6a7590;
            font-size: 12px;
        }

        .model-actions {
            display: flex;
            gap: 8px;
        }

        .model-action-btn {
            padding: 6px 12px;
            font-size: 12px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s;
        }

        .model-select-btn {
            background: linear-gradient(135deg, #0066ff 0%, #0052cc 100%);
            color: white;
        }

        .model-select-btn:hover {
            box-shadow: 0 3px 10px rgba(0, 102, 255, 0.3);
        }

        .model-delete-btn {
            background-color: rgba(255, 71, 87, 0.1);
            color: var(--danger);
        }

        .model-delete-btn:hover {
            background-color: rgba(255, 71, 87, 0.2);
        }

        .scrollbar-hide::-webkit-scrollbar {
            display: none;
        }

        @media (max-width: 768px) {
            #chat-container {
                max-width: 100%;
                border-radius: 0;
            }

            .message-content {
                max-width: 85%;
            }

            .script-box {
                max-width: 100%;
            }
        }
    </style>
</head>
<body>
    <div id="chat-container">
        <div id="cosmic-bg"></div>
        <div id="stars-container"></div>

        <div id="top-panel">
            <div class="top-left">
                <button id="history-btn" class="icon-btn" title="История">📋</button>
            </div>
            <div class="current-model" id="current-model">🤖 MiniWorld-G1.1</div>
            <div class="top-right">
                <button id="settings-btn" class="icon-btn" title="Настройки">⚙️</button>
            </div>
        </div>

        <div id="chat"></div>
        <div id="suggestions"></div>

        <div id="input-container">
            <input id="user-input" type="text" placeholder="Сообщение...">
            <button id="send-btn">Отправить</button>
        </div>
    </div>

    <div class="history-overlay" id="history-overlay"></div>
    <div id="history-panel">
        <div class="history-header">
            <div style="font-weight: 600;">💬</div>
            <button class="history-close">✕</button>
        </div>
        <div class="history-list scrollbar-hide" id="history-list"></div>
        <div class="history-footer">
            <button id="new-chat-btn">➕ Новый чат</button>
        </div>
    </div>

    <div id="settings-modal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>⚙️ Настройки</h2>
                <button class="close-btn">✕</button>
            </div>

            <div class="tab-buttons">
                <button class="tab-btn active" data-tab="models-tab">🤖 Модели</button>
                <button class="tab-btn" data-tab="create-tab">➕ Создать</button>
            </div>

            <div id="models-tab" class="tab-content active">
                <div id="models-list"></div>
            </div>

            <div id="create-tab" class="tab-content">
                <div class="success-message" id="create-success">✅ Создано</div>
                <div class="error-message" id="create-error"></div>
                
                <div class="input-group">
                    <label>Имя</label>
                    <input type="text" id="bot-name" placeholder="MyBot-1.0">
                </div>

                <div class="input-group">
                    <label>Описание</label>
                    <input type="text" id="bot-description" placeholder="Функции">
                </div>

                <div class="file-upload-section">
                    <h3>📁 Загрузить файл (JSON или TXT с JSON)</h3>
                    <input type="file" id="file-input" accept=".json,.txt">
                    <button class="file-upload-btn" id="file-upload-label">📁 Выберите файл</button>
                    <div class="file-name-display" id="file-name"></div>
                </div>

                <label style="display: block; margin-bottom: 8px; margin-top: 16px; color: var(--text-color); font-weight: 600; font-size: 13px;">или введите JSON вручную</label>
                <textarea id="bot-json" placeholder='[{"keywords":["привет"],"hint":"Привет","response":"Привет!","script":"","showScript":false}]'></textarea>

                <div class="button-group">
                    <button class="btn-secondary" id="clear-bot-btn">Очистить</button>
                    <button class="btn-primary" id="create-bot-btn">Создать</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        function createStars() {
            const container = document.getElementById('stars-container');
            
            for (let i = 0; i < 100; i++) {
                const star = document.createElement('div');
                star.className = `star ${Math.random() > 0.7 ? 'large' : ''}`;
                star.style.left = Math.random() * 100 + '%';
                star.style.top = Math.random() * 100 + '%';
                star.style.animationDelay = Math.random() * 3 + 's';
                container.appendChild(star);
            }

            setInterval(() => {
                const fallingStar = document.createElement('div');
                fallingStar.className = 'falling-star';
                fallingStar.style.left = Math.random() * 100 + '%';
                fallingStar.style.top = '-50px';
                container.appendChild(fallingStar);

                setTimeout(() => {
                    fallingStar.remove();
                }, 4000);
            }, 800);
        }

        const chat = document.getElementById("chat");
        const userInput = document.getElementById("user-input");
        const sendBtn = document.getElementById("send-btn");
        const suggestions = document.getElementById("suggestions");
        const settingsModal = document.getElementById("settings-modal");
        const historyPanel = document.getElementById("history-panel");
        const historyOverlay = document.getElementById("history-overlay");
        const historyBtn = document.getElementById("history-btn");
        const settingsBtn = document.getElementById("settings-btn");
        const historyList = document.getElementById("history-list");
        const newChatBtn = document.getElementById("new-chat-btn");
        const closeSettingsBtn = document.querySelector(".close-btn");
        const closeHistoryBtn = document.querySelector(".history-close");
        const tabBtns = document.querySelectorAll(".tab-btn");
        const tabContents = document.querySelectorAll(".tab-content");
        const currentModelDisplay = document.getElementById("current-model");
        const fileInput = document.getElementById("file-input");
        const fileUploadLabel = document.getElementById("file-upload-label");
        const fileName = document.getElementById("file-name");
        const botJson = document.getElementById("bot-json");

        const miniWorldCommands = [
            { "keywords": ["привет", "здравствуй", "хай"], "hint": "Поздравить боту", "response": "Привет! Я бот-помощник. Как я могу тебе помочь?", "script": "Chat:sendSystemMsg('Приветствую в Mini World!')", "showScript": true },
            { "keywords": ["время", "тайм"], "hint": "Узнать игровое время", "response": "Текущее время в мире:", "script": "local time = World:getTime()\nChat:sendSystemMsg('Игровое время: ' .. time)", "showScript": true },
            { "keywords": ["позиция", "координаты", "где я"], "hint": "Узнать текущие координаты", "response": "Твои координаты:", "script": "local ret, x, y, z = Player:getPosition(Player:getMainPlayerUin())\nChat:sendSystemMsg('X: '..x..' Y: '..y..' Z: '..z)", "showScript": true },
            { "keywords": ["здоровье", "хп", "hp"], "hint": "Проверить уровень здоровья", "response": "Текущий уровень здоровья:", "script": "local res, hp = Player:getAttr(Player:getMainPlayerUin(), PLAYERATTR.CUR_HP)\nChat:sendSystemMsg('HP: '..hp..'/100')", "showScript": true },
            { "keywords": ["голод", "еда"], "hint": "Проверить уровень голода", "response": "Уровень голода:", "script": "local res, hunger = Player:getAttr(Player:getMainPlayerUin(), PLAYERATTR.CUR_HUNGER)\nChat:sendSystemMsg('Сытость: '..hunger)", "showScript": true },
            { "keywords": ["опыт", "exp", "уровень"], "hint": "Узнать количество опыта", "response": "Твой опыт:", "script": "local res, exp = Player:getAttr(Player:getMainPlayerUin(), PLAYERATTR.EXP)\nChat:sendSystemMsg('Опыт: '..exp)", "showScript": true },
            { "keywords": ["помощь", "help", "команды"], "hint": "Справка по всем доступным командам", "response": "Доступные команды: привет, время, позиция, здоровье, голод, опыт, лечи, накорми, скорость", "script": "", "showScript": false },
            { "keywords": ["лечи", "хил", "heal"], "hint": "Восстановить здоровье полностью", "response": "Здоровье восстановлено!", "script": "local playerId = Player:getMainPlayerUin()\nPlayer:setAttr(playerId, PLAYERATTR.CUR_HP, 100)\nChat:sendSystemMsg('❤ Ты исцелён!')", "showScript": true },
            { "keywords": ["накорми", "есть"], "hint": "Восполнить голод до максимума", "response": "Сытость восстановлена!", "script": "local playerId = Player:getMainPlayerUin()\nPlayer:setAttr(playerId, PLAYERATTR.CUR_HUNGER, 100)\nChat:sendSystemMsg('🍖 Ты сыт!')", "showScript": true },
            { "keywords": ["скорость", "бег", "спид"], "hint": "Увеличить скорость передвижения", "response": "Скорость увеличена!", "script": "local playerId = Player:getMainPlayerUin()\nActor:setAttr(playerId, ACTORATTR.WALK_SPEED, 1.5)\nChat:sendSystemMsg('⚡ Скорость: 1.5')", "showScript": true },
        ];

        const aiModels = [
            {
                id: "miniworld-g1.1",
                name: "MiniWorld-G1.1",
                description: "Mini World API",
                commands: miniWorldCommands,
                isDefault: true
            }
        ];

        let chats = [];
        let currentChat = null;
        let currentModel = aiModels[0];
        let customModels = [];

        createStars();
        loadChats();
        loadModels();
        updateModelsList();

        function generateChatId() {
            return `chat-${Date.now()}-${Math.random().toString(36).substr(2, 9)}`;
        }

        function createNewChat() {
            const chatId = generateChatId();
            const newChat = {
                id: chatId,
                name: `${new Date().toLocaleTimeString('ru-RU', { hour: '2-digit', minute: '2-digit' })}`,
                messages: [],
                createdAt: new Date().toISOString(),
                modelId: currentModel.id
            };

            chats.unshift(newChat);
            saveChats();
            switchChat(chatId);
        }

        function switchChat(chatId) {
            currentChat = chats.find(c => c.id === chatId);
            chat.innerHTML = "";
            suggestions.style.display = "none";
            userInput.value = "";
            updateHistoryList();
            loadChatMessages();
        }

        function loadChatMessages() {
            chat.innerHTML = "";
            if (!currentChat || currentChat.messages.length === 0) {
                chat.innerHTML = `<div class="empty-state"><div class="empty-state-icon">💬</div><div class="empty-state-text">Начните разговор</div></div>`;
            } else {
                currentChat.messages.forEach(msg => {
                    if (msg.type === "message") {
                        addMessageToChat(msg.content, msg.sender, false);
                    } else if (msg.type === "script") {
                        addScriptToChat(msg.content, false);
                    }
                });
            }
        }

        function deleteChat(chatId, event) {
            event.stopPropagation();
            if (confirm("Удалить чат?")) {
                chats = chats.filter(c => c.id !== chatId);
                saveChats();
                if (currentChat && currentChat.id === chatId) {
                    if (chats.length > 0) {
                        switchChat(chats[0].id);
                    } else {
                        currentChat = null;
                        chat.innerHTML = `<div class="empty-state"><div class="empty-state-icon">📭</div><div class="empty-state-text">Создайте новый чат</div></div>`;
                    }
                }
                updateHistoryList();
            }
        }

        function updateHistoryList() {
            historyList.innerHTML = "";
            chats.forEach(c => {
                const item = document.createElement("div");
                item.className = `history-item ${c.id === currentChat?.id ? "active" : ""}`;
                
                const nameDiv = document.createElement("div");
                nameDiv.className = "history-item-name";
                nameDiv.textContent = c.name;
                nameDiv.addEventListener("click", () => {
                    switchChat(c.id);
                    closeHistoryPanel();
                });
                
                const deleteBtn = document.createElement("button");
                deleteBtn.className = "history-item-delete";
                deleteBtn.innerHTML = "🗑";
                deleteBtn.addEventListener("click", (e) => {
                    e.stopPropagation();
                    deleteChat(c.id, {stopPropagation: () => {}});
                });
                
                item.appendChild(nameDiv);
                item.appendChild(deleteBtn);
                historyList.appendChild(item);
            });
        }

        function saveChats() {
            localStorage.setItem("chats", JSON.stringify(chats));
        }

        function loadChats() {
            const saved = localStorage.getItem("chats");
            if (saved) {
                chats = JSON.parse(saved);
                if (chats.length > 0) {
                    currentChat = chats[0];
                    loadChatMessages();
                }
            } else {
                createNewChat();
            }
            updateHistoryList();
        }

        function loadModels() {
            const saved = localStorage.getItem("customModels");
            if (saved) {
                customModels = JSON.parse(saved);
            }
        }

        function saveModels() {
            localStorage.setItem("customModels", JSON.stringify(customModels));
            updateModelsList();
        }

        function updateModelsList() {
            const modelsList = document.getElementById("models-list");
            const allModels = [...aiModels, ...customModels];

            modelsList.innerHTML = "";
            allModels.forEach(model => {
                const section = document.createElement("div");
                section.className = "model-section";
                
                const item = document.createElement("div");
                item.className = "model-item-settings";
                
                const info = document.createElement("div");
                info.className = "model-info";
                info.innerHTML = `<strong>${model.name}</strong><small>${model.description}</small>`;
                
                const actions = document.createElement("div");
                actions.className = "model-actions";
                
                const selectBtn = document.createElement("button");
                selectBtn.className = `model-action-btn ${model === currentModel ? "model-select-btn" : "model-select-btn"}`;
                selectBtn.textContent = model === currentModel ? "✓" : "Выбрать";
                selectBtn.disabled = model === currentModel;
                selectBtn.addEventListener("click", () => {
                    currentModel = model;
                    currentModelDisplay.textContent = `🤖 ${model.name}`;
                    updateModelsList();
                });
                
                actions.appendChild(selectBtn);
                
                if (!model.isDefault) {
                    const deleteBtn = document.createElement("button");
                    deleteBtn.className = "model-action-btn model-delete-btn";
                    deleteBtn.textContent = "🗑";
                    deleteBtn.addEventListener("click", () => {
                        if (confirm(`Удалить "${model.name}"?`)) {
                            customModels = customModels.filter(m => m.id !== model.id);
                            saveModels();
                            if (currentModel === model) {
                                currentModel = aiModels[0];
                                currentModelDisplay.textContent = `🤖 ${currentModel.name}`;
                            }
                        }
                    });
                    actions.appendChild(deleteBtn);
                }
                
                item.appendChild(info);
                item.appendChild(actions);
                section.appendChild(item);
                modelsList.appendChild(section);
            });
        }

        function showThinkingIndicator() {
            const messageWrapper = document.createElement("div");
            messageWrapper.className = "message ai";
            messageWrapper.id = "thinking-message";

            const thinkingDiv = document.createElement("div");
            thinkingDiv.className = "thinking-indicator";

            for (let i = 0; i < 3; i++) {
                const dot = document.createElement("div");
                dot.className = "thinking-dot";
                thinkingDiv.appendChild(dot);
            }

            messageWrapper.appendChild(thinkingDiv);
            chat.appendChild(messageWrapper);
            chat.scrollTop = chat.scrollHeight;
        }

        function removeThinkingIndicator() {
            const thinkingMessage = document.getElementById("thinking-message");
            if (thinkingMessage) {
                thinkingMessage.remove();
            }
        }

        function sendMessage() {
            if (!currentChat) return;
            
            const query = userInput.value.trim();
            if (!query) return;

            addMessageToChat(query, "user");
            currentChat.messages.push({
                type: "message",
                content: query,
                sender: "user"
            });

            const match = currentModel.commands.find((cmd) =>
                cmd.keywords.some((keyword) => query.toLowerCase().includes(keyword.toLowerCase()))
            );

            showThinkingIndicator();

            setTimeout(() => {
                removeThinkingIndicator();

                if (match) {
                    addMessageToChat(match.response, "ai");
                    currentChat.messages.push({
                        type: "message",
                        content: match.response,
                        sender: "ai"
                    });

                    if (match.showScript && match.script) {
                        addScriptToChat(match.script);
                        currentChat.messages.push({
                            type: "script",
                            content: match.script
                        });
                    }
                } else {
                    const errorMsg = "😕 Не найдено";
                    addMessageToChat(errorMsg, "ai");
                    currentChat.messages.push({
                        type: "message",
                        content: errorMsg,
                        sender: "ai"
                    });
                }

                saveChats();
                userInput.value = "";
                suggestions.style.display = "none";
            }, 1500);
        }

        function addMessageToChat(content, sender, save = true) {
            const messageWrapper = document.createElement("div");
            messageWrapper.className = `message ${sender}`;

            const messageContent = document.createElement("div");
            messageContent.className = "message-content";
            messageContent.textContent = content;

            messageWrapper.appendChild(messageContent);
            chat.appendChild(messageWrapper);
            chat.scrollTop = chat.scrollHeight;
        }

        function addScriptToChat(script, save = true) {
            const scriptBox = document.createElement("div");
            scriptBox.className = "script-box";
            scriptBox.textContent = script;

            const copyBtn = document.createElement("button");
            copyBtn.className = "copy-btn";
            copyBtn.textContent = "📋 Скопировать";

            copyBtn.addEventListener("click", (e) => {
                e.stopPropagation();
                navigator.clipboard.writeText(script).then(() => {
                    const originalText = copyBtn.textContent;
                    copyBtn.textContent = "✅";
                    setTimeout(() => {
                        copyBtn.textContent = originalText;
                    }, 2000);
                });
            });

            scriptBox.appendChild(copyBtn);
            chat.appendChild(scriptBox);
            chat.scrollTop = chat.scrollHeight;
        }

        function closeHistoryPanel() {
            historyPanel.classList.remove("active");
            historyOverlay.classList.remove("active");
        }

        function showSuccessMessage(elementId) {
            const element = document.getElementById(elementId);
            element.classList.add("show");
            setTimeout(() => {
                element.classList.remove("show");
            }, 2000);
        }

        function showErrorMessage(elementId, message) {
            const element = document.getElementById(elementId);
            element.textContent = message;
            element.classList.add("show");
            setTimeout(() => {
                element.classList.remove("show");
            }, 3000);
        }

        historyBtn.addEventListener("click", () => {
            historyPanel.classList.add("active");
            historyOverlay.classList.add("active");
        });

        closeHistoryBtn.addEventListener("click", closeHistoryPanel);
        historyOverlay.addEventListener("click", closeHistoryPanel);

        settingsBtn.addEventListener("click", () => {
            settingsModal.classList.add("active");
        });

        closeSettingsBtn.addEventListener("click", () => {
            settingsModal.classList.remove("active");
        });

        settingsModal.addEventListener("click", (e) => {
            if (e.target === settingsModal) {
                settingsModal.classList.remove("active");
            }
        });

        newChatBtn.addEventListener("click", () => {
            createNewChat();
            closeHistoryPanel();
        });

        tabBtns.forEach(btn => {
            btn.addEventListener("click", () => {
                const tabName = btn.dataset.tab;
                tabBtns.forEach(b => b.classList.remove("active"));
                tabContents.forEach(c => c.classList.remove("active"));
                btn.classList.add("active");
                document.getElementById(tabName).classList.add("active");
            });
        });

        fileUploadLabel.addEventListener("click", () => {
            fileInput.click();
        });

        fileInput.addEventListener("change", (e) => {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (event) => {
                    try {
                        const content = event.target.result;
                        let commands;

                        try {
                            commands = JSON.parse(content);
                        } catch (jsonError) {
                            const lines = content.split('\n').filter(line => line.trim());
                            commands = lines.map((line) => {
                                const parts = line.split('|').map(p => p.trim());
                                
                                if (parts.length < 2) {
                                    return null;
                                }

                                const keywords = parts[0].split(',').map(k => k.trim());
                                const response = parts[1];
                                const hint = parts[2] || response;
                                const script = parts[3] || '';
                                const showScript = parts[4] === 'true' || parts[4] === '1';

                                return {
                                    keywords: keywords,
                                    hint: hint,
                                    response: response,
                                    script: script,
                                    showScript: showScript
                                };
                            }).filter(cmd => cmd !== null);
                        }

                        if (!Array.isArray(commands) || commands.length === 0) {
                            throw new Error("Не удалось разобрать команды");
                        }

                        botJson.value = JSON.stringify(commands, null, 2);
                        fileName.textContent = "✅ " + file.name;
                    } catch (error) {
                        showErrorMessage("create-error", "❌ Ошибка при чтении файла");
                        console.error(error);
                    }
                };
                reader.readAsText(file);
            }
        });

        document.getElementById("create-bot-btn").addEventListener("click", () => {
            const botName = document.getElementById("bot-name").value.trim();
            const botDescription = document.getElementById("bot-description").value.trim();
            const botJsonText = botJson.value.trim();

            if (!botName) {
                showErrorMessage("create-error", "❌ Введите имя");
                return;
            }

            if (!botJsonText) {
                showErrorMessage("create-error", "❌ Загрузите файл или введите JSON");
                return;
            }

            try {
                const commands = JSON.parse(botJsonText);
                if (!Array.isArray(commands)) {
                    throw new Error("Должен быть массив");
                }

                const newModel = {
                    id: `custom-${Date.now()}`,
                    name: botName,
                    description: botDescription || "Бот",
                    commands: commands,
                    isDefault: false
                };

                customModels.push(newModel);
                saveModels();

                showSuccessMessage("create-success");
                document.getElementById("bot-name").value = "";
                document.getElementById("bot-description").value = "";
                botJson.value = "";
                fileName.textContent = "";
                fileInput.value = "";

                setTimeout(() => {
                    currentModel = newModel;
                    currentModelDisplay.textContent = `🤖 ${newModel.name}`;
                    updateModelsList();
                    document.querySelector("[data-tab='models-tab']").click();
                }, 1000);
            } catch (error) {
                showErrorMessage("create-error", "❌ Ошибка JSON");
            }
        });

        document.getElementById("clear-bot-btn").addEventListener("click", () => {
            document.getElementById("bot-name").value = "";
            document.getElementById("bot-description").value = "";
            botJson.value = "";
            fileName.textContent = "";
            fileInput.value = "";
        });

        sendBtn.addEventListener("click", sendMessage);
        userInput.addEventListener("keypress", (e) => {
            if (e.key === "Enter") sendMessage();
        });

        userInput.addEventListener("input", () => {
            if (!currentChat) return;
            
            const input = userInput.value.trim();
            suggestions.innerHTML = "";

            if (!input) {
                suggestions.style.display = "none";
                return;
            }

            const matched = currentModel.commands.filter((cmd) =>
                cmd.keywords.some((keyword) => keyword.toLowerCase().includes(input.toLowerCase()))
            );

            matched.forEach((cmd) => {
                const suggestionItem = document.createElement("div");
                suggestionItem.className = "suggestion-item";
                
                const matchedKeyword = cmd.keywords.find(kw => kw.toLowerCase().includes(input.toLowerCase()));
                
                const keyword = document.createElement("div");
                keyword.className = "suggestion-keyword";
                keyword.textContent = matchedKeyword || cmd.keywords[0];
                
                const hint = document.createElement("div");
                hint.className = "suggestion-hint";
                hint.textContent = cmd.hint;
                
                suggestionItem.appendChild(keyword);
                suggestionItem.appendChild(hint);
                
                suggestionItem.addEventListener("click", () => {
                    userInput.value = `${matchedKeyword || cmd.keywords[0]} `;
                    suggestions.style.display = "none";
                    userInput.focus();
                });
                
                suggestions.appendChild(suggestionItem);
            });

            suggestions.style.display = matched.length > 0 ? "block" : "none";
        });

        userInput.focus();
    </script>
</body>
</html>
