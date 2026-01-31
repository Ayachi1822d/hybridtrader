<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HybridTrader - Trading Dashboard</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary: #0f1117;
            --secondary: #1a1d29;
            --accent: #2962ff;
            --accent-green: #00e676;
            --accent-red: #ff3d57;
            --accent-yellow: #ffc107;
            --accent-purple: #9c27b0;
            --text-primary: #ffffff;
            --text-secondary: #a0a5b8;
            --border-color: #2d3247;
            --sidebar-width: 240px;
            --header-height: 70px;
            --footer-height: 50px;
            --animation-speed: 0.4s;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', sans-serif;
        }

        body {
            background-color: var(--primary);
            color: var(--text-primary);
            display: flex;
            height: 100vh;
            overflow: hidden;
            opacity: 0;
            animation: fadeIn 0.8s ease-out forwards;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        /* Sidebar Styles */
        .sidebar {
            width: var(--sidebar-width);
            background-color: var(--secondary);
            display: flex;
            flex-direction: column;
            padding: 20px 0;
            border-right: 1px solid var(--border-color);
            transform: translateX(-100%);
            animation: slideInLeft 0.6s cubic-bezier(0.23, 1, 0.32, 1) 0.2s forwards;
        }

        @keyframes slideInLeft {
            from { transform: translateX(-100%); }
            to { transform: translateX(0); }
        }

        .logo {
            padding: 0 20px 30px;
            font-size: 24px;
            font-weight: 700;
            display: flex;
            align-items: center;
            opacity: 0;
            transform: translateY(-10px);
            animation: fadeInUp 0.5s ease-out 0.4s forwards;
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .logo i {
            color: var(--accent);
            margin-right: 10px;
            animation: pulse 2s infinite ease-in-out;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        .menu {
            list-style: none;
            flex-grow: 1;
        }

        .menu li {
            padding: 15px 20px;
            display: flex;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            border-left: 3px solid transparent;
            opacity: 0;
            transform: translateX(-20px);
        }

        .menu li:nth-child(1) { animation: slideInMenuItem 0.5s ease-out 0.5s forwards; }
        .menu li:nth-child(2) { animation: slideInMenuItem 0.5s ease-out 0.6s forwards; }
        .menu li:nth-child(3) { animation: slideInMenuItem 0.5s ease-out 0.7s forwards; }
        .menu li:nth-child(4) { animation: slideInMenuItem 0.5s ease-out 0.8s forwards; }
        .menu li:nth-child(5) { animation: slideInMenuItem 0.5s ease-out 0.9s forwards; }
        .menu li:nth-child(6) { animation: slideInMenuItem 0.5s ease-out 1.0s forwards; }
        .menu li:nth-child(7) { animation: slideInMenuItem 0.5s ease-out 1.1s forwards; }
        .menu li:nth-child(8) { animation: slideInMenuItem 0.5s ease-out 1.2s forwards; }

        @keyframes slideInMenuItem {
            from {
                opacity: 0;
                transform: translateX(-20px);
            }
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        .menu li:hover, .menu li.active {
            background-color: rgba(41, 98, 255, 0.1);
            border-left: 3px solid var(--accent);
            transform: translateX(5px);
        }

        .menu li i {
            width: 24px;
            margin-right: 12px;
            color: var(--text-secondary);
            transition: all 0.3s ease;
        }

        .menu li:hover i, .menu li.active i {
            color: var(--accent);
            transform: scale(1.1);
        }

        .menu li.active {
            color: var(--accent);
            font-weight: 600;
        }

        .user-profile {
            padding: 20px;
            display: flex;
            align-items: center;
            border-top: 1px solid var(--border-color);
            opacity: 0;
            animation: fadeIn 0.8s ease-out 1.4s forwards;
        }

        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--accent), var(--accent-purple));
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 12px;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .user-avatar:hover {
            transform: rotate(15deg) scale(1.1);
        }

        .user-info h4 {
            font-size: 14px;
            font-weight: 600;
        }

        .user-info p {
            font-size: 12px;
            color: var(--text-secondary);
        }

        /* Main Content Styles */
        .main-content {
            flex: 1;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            opacity: 0;
            transform: translateY(20px);
            animation: slideInUp 0.6s cubic-bezier(0.23, 1, 0.32, 1) 0.8s forwards;
        }

        @keyframes slideInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Header Styles */
        .header {
            height: var(--header-height);
            background-color: var(--secondary);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 30px;
            border-bottom: 1px solid var(--border-color);
            position: relative;
            overflow: hidden;
        }

        .header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, 
                rgba(41, 98, 255, 0.05) 0%, 
                rgba(0, 230, 118, 0.05) 50%, 
                rgba(255, 61, 87, 0.05) 100%);
            z-index: 0;
            animation: shimmer 3s infinite linear;
        }

        @keyframes shimmer {
            0% { background-position: -1000px 0; }
            100% { background-position: 1000px 0; }
        }

        .top-tickers {
            display: flex;
            align-items: center;
            gap: 30px;
            z-index: 1;
        }

        .ticker-item {
            display: flex;
            flex-direction: column;
            transition: all 0.3s ease;
        }

        .ticker-item:hover {
            transform: translateY(-3px);
        }

        .ticker-name {
            font-size: 12px;
            color: var(--text-secondary);
            margin-bottom: 3px;
        }

        .ticker-value {
            font-size: 16px;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .ticker-change {
            font-size: 12px;
            padding: 2px 6px;
            border-radius: 4px;
            transition: all 0.3s ease;
        }

        .ticker-change.positive {
            background-color: rgba(0, 230, 118, 0.2);
            color: var(--accent-green);
        }

        .ticker-change.negative {
            background-color: rgba(255, 61, 87, 0.2);
            color: var(--accent-red);
        }

        .ticker-change.updating {
            animation: pulseChange 0.5s ease;
        }

        @keyframes pulseChange {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }

        .confidence-container {
            display: flex;
            align-items: center;
            background: linear-gradient(135deg, rgba(41, 98, 255, 0.2), rgba(0, 230, 118, 0.1));
            padding: 8px 15px;
            border-radius: 20px;
            z-index: 1;
            transition: all 0.3s ease;
            border: 1px solid rgba(41, 98, 255, 0.3);
            animation: glowPulse 2s infinite alternate;
        }

        @keyframes glowPulse {
            from {
                box-shadow: 0 0 10px rgba(41, 98, 255, 0.3);
            }
            to {
                box-shadow: 0 0 20px rgba(41, 98, 255, 0.5);
            }
        }

        .confidence-container:hover {
            transform: scale(1.05);
        }

        .confidence-label {
            font-size: 14px;
            margin-right: 10px;
        }

        .confidence-value {
            font-weight: 700;
            font-size: 18px;
            transition: all 0.5s ease;
        }

        /* Content Area */
        .content-area {
            flex: 1;
            display: flex;
            overflow: hidden;
            padding: 20px;
            gap: 20px;
        }

        /* Left Panel */
        .left-panel {
            flex: 3;
            display: flex;
            flex-direction: column;
            gap: 20px;
            overflow-y: auto;
        }

        /* Right Panel */
        .right-panel {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 20px;
            min-width: 300px;
        }

        /* Card Styles */
        .card {
            background: linear-gradient(135deg, rgba(26, 29, 41, 0.8), rgba(26, 29, 41, 0.6));
            border-radius: 12px;
            padding: 20px;
            border: 1px solid var(--border-color);
            backdrop-filter: blur(10px);
            transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
            overflow: hidden;
            opacity: 0;
            transform: translateY(20px);
        }

        .card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, rgba(41, 98, 255, 0.1), transparent);
            z-index: -1;
            opacity: 0;
            transition: opacity 0.4s ease;
        }

        .card:hover {
            transform: translateY(-5px);
            border-color: rgba(41, 98, 255, 0.5);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        }

        .card:hover::before {
            opacity: 1;
        }

        .card.animate-in {
            animation: cardSlideUp 0.5s cubic-bezier(0.23, 1, 0.32, 1) forwards;
        }

        @keyframes cardSlideUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .card-title {
            font-size: 18px;
            font-weight: 600;
            position: relative;
            display: inline-block;
        }

        .card-title::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 0;
            height: 2px;
            background: linear-gradient(90deg, var(--accent), transparent);
            transition: width 0.6s ease;
        }

        .card:hover .card-title::after {
            width: 100%;
        }

        .card-subtitle {
            font-size: 14px;
            color: var(--text-secondary);
        }

        /* Portfolio Overview */
        .portfolio-overview {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
        }

        .account-card {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0.02));
            border-radius: 10px;
            padding: 20px;
            border-left: 4px solid var(--accent);
            transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .account-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, rgba(41, 98, 255, 0.1), transparent);
            opacity: 0;
            transition: opacity 0.4s ease;
        }

        .account-card:hover {
            transform: translateY(-5px) scale(1.02);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
        }

        .account-card:hover::before {
            opacity: 1;
        }

        .account-card.green {
            border-left-color: var(--accent-green);
        }

        .account-card.red {
            border-left-color: var(--accent-red);
        }

        .account-name {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 5px;
        }

        .account-balance {
            font-size: 22px;
            font-weight: 700;
            margin-bottom: 10px;
            transition: all 0.3s ease;
        }

        .account-card:hover .account-balance {
            color: var(--accent);
        }

        .account-pl {
            font-size: 14px;
            display: flex;
            justify-content: space-between;
            transition: all 0.3s ease;
        }

        .account-pl.positive {
            color: var(--accent-green);
        }

        .account-pl.negative {
            color: var(--accent-red);
        }

        /* Market Overview */
        .market-overview {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
        }

        .market-item {
            display: flex;
            flex-direction: column;
            padding: 15px;
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0.02));
            border-radius: 10px;
            min-width: 120px;
            transition: all 0.3s ease;
            cursor: pointer;
            border: 1px solid transparent;
        }

        .market-item:hover {
            transform: translateY(-5px);
            border-color: rgba(41, 98, 255, 0.3);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .market-symbol {
            font-size: 14px;
            color: var(--text-secondary);
        }

        .market-price {
            font-size: 18px;
            font-weight: 600;
            margin: 5px 0;
            transition: all 0.3s ease;
        }

        .market-item:hover .market-price {
            color: var(--accent);
        }

        /* Chart Container */
        .chart-container {
            height: 300px;
            position: relative;
        }

        /* AI Signal */
        .ai-signal {
            background: linear-gradient(135deg, rgba(41, 98, 255, 0.2), rgba(41, 98, 255, 0.05));
            border-left: 4px solid var(--accent);
            animation: signalPulse 3s infinite;
        }

        @keyframes signalPulse {
            0% { box-shadow: 0 0 0 0 rgba(41, 98, 255, 0.3); }
            70% { box-shadow: 0 0 0 10px rgba(41, 98, 255, 0); }
            100% { box-shadow: 0 0 0 0 rgba(41, 98, 255, 0); }
        }

        .signal-title {
            display: flex;
            align-items: center;
            color: var(--accent);
            margin-bottom: 10px;
        }

        .signal-title i {
            margin-right: 10px;
            animation: bounce 1s infinite alternate;
        }

        @keyframes bounce {
            from { transform: translateY(0); }
            to { transform: translateY(-5px); }
        }

        .signal-content {
            font-size: 16px;
            line-height: 1.5;
        }

        .signal-target {
            margin-top: 10px;
            font-weight: 600;
            color: var(--accent-green);
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .signal-target::before {
            content: '🎯';
            font-size: 14px;
        }

        /* Positions Table */
        .positions-table {
            width: 100%;
            border-collapse: collapse;
        }

        .positions-table th {
            text-align: left;
            padding: 15px 10px;
            font-weight: 600;
            color: var(--text-secondary);
            border-bottom: 1px solid var(--border-color);
            position: sticky;
            top: 0;
            background-color: var(--secondary);
            z-index: 2;
        }

        .positions-table td {
            padding: 18px 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
            transition: all 0.3s ease;
        }

        .positions-table tr {
            transition: all 0.3s ease;
        }

        .positions-table tr:hover {
            background-color: rgba(41, 98, 255, 0.1);
            transform: translateX(5px);
        }

        .positions-table tr:last-child td {
            border-bottom: none;
        }

        .symbol-cell {
            display: flex;
            align-items: center;
        }

        .symbol-icon {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: linear-gradient(135deg, rgba(41, 98, 255, 0.2), rgba(0, 230, 118, 0.2));
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 12px;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .positions-table tr:hover .symbol-icon {
            transform: scale(1.1) rotate(10deg);
        }

        .price-up {
            color: var(--accent-green);
            position: relative;
        }

        .price-up::before {
            content: '↗';
            margin-right: 5px;
            animation: floatUp 1s infinite alternate;
        }

        @keyframes floatUp {
            from { transform: translateY(0); }
            to { transform: translateY(-3px); }
        }

        .price-down {
            color: var(--accent-red);
            position: relative;
        }

        .price-down::before {
            content: '↘';
            margin-right: 5px;
            animation: floatDown 1s infinite alternate;
        }

        @keyframes floatDown {
            from { transform: translateY(0); }
            to { transform: translateY(3px); }
        }

        /* Bottom Action Menu */
        .bottom-action {
            position: fixed;
            bottom: 0;
            left: var(--sidebar-width);
            right: 0;
            height: 80px;
            background: linear-gradient(135deg, rgba(26, 29, 41, 0.9), rgba(26, 29, 41, 0.95));
            border-top: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 30px;
            z-index: 100;
            backdrop-filter: blur(10px);
            transform: translateY(100%);
            animation: slideUpBottom 0.6s cubic-bezier(0.23, 1, 0.32, 1) 1s forwards;
        }

        @keyframes slideUpBottom {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }

        .buy-action {
            background: linear-gradient(135deg, var(--accent-green), #00b248);
            color: white;
            border: none;
            padding: 15px 40px;
            border-radius: 10px;
            font-weight: 600;
            font-size: 16px;
            cursor: pointer;
            display: flex;
            align-items: center;
            transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
            overflow: hidden;
        }

        .buy-action::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: left 0.6s ease;
        }

        .buy-action:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 0 10px 25px rgba(0, 230, 118, 0.3);
        }

        .buy-action:hover::before {
            left: 100%;
        }

        .buy-action:active {
            transform: translateY(-1px) scale(1.02);
        }

        .buy-action i {
            margin-right: 10px;
            transition: all 0.3s ease;
        }

        .buy-action:hover i {
            transform: rotate(15deg) scale(1.2);
        }

        .confidence-indicator {
            display: flex;
            align-items: center;
            font-size: 16px;
            font-weight: 500;
        }

        .confidence-bar {
            width: 200px;
            height: 10px;
            background-color: rgba(255, 255, 255, 0.1);
            border-radius: 5px;
            margin-left: 15px;
            overflow: hidden;
            position: relative;
        }

        .confidence-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--accent-red), var(--accent-yellow), var(--accent-green));
            width: 71%;
            border-radius: 5px;
            transition: width 1s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
            overflow: hidden;
        }

        .confidence-fill::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
            animation: shimmerBar 2s infinite;
        }

        @keyframes shimmerBar {
            from { left: -100%; }
            to { left: 100%; }
        }

        /* Right Panel Widgets */
        .exposure-widget {
            text-align: center;
        }

        .exposure-value {
            font-size: 42px;
            font-weight: 700;
            margin: 15px 0;
            background: linear-gradient(135deg, var(--accent), var(--accent-green));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            transition: all 0.5s ease;
        }

        .exposure-widget:hover .exposure-value {
            transform: scale(1.1);
        }

        .heatmap-container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
        }

        .heatmap-item {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0.02));
            padding: 15px;
            border-radius: 8px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
            position: relative;
            overflow: hidden;
        }

        .heatmap-item::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 3px;
            transition: all 0.3s ease;
        }

        .heatmap-item.up::before {
            background: linear-gradient(90deg, var(--accent-green), transparent);
        }

        .heatmap-item.down::before {
            background: linear-gradient(90deg, var(--accent-red), transparent);
        }

        .heatmap-item.neutral::before {
            background: linear-gradient(90deg, var(--text-secondary), transparent);
        }

        .heatmap-item:hover {
            transform: translateY(-5px) scale(1.05);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
        }

        .heatmap-item.up:hover {
            background: linear-gradient(135deg, rgba(0, 230, 118, 0.1), rgba(0, 230, 118, 0.05));
        }

        .heatmap-item.down:hover {
            background: linear-gradient(135deg, rgba(255, 61, 87, 0.1), rgba(255, 61, 87, 0.05));
        }

        /* Market Sentiment */
        .sentiment-container {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .sentiment-meter {
            width: 140px;
            height: 140px;
            position: relative;
            margin: 20px 0;
        }

        .sentiment-label {
            font-size: 16px;
            font-weight: 600;
            margin-top: 10px;
            padding: 5px 15px;
            border-radius: 20px;
            background: linear-gradient(135deg, rgba(255, 61, 87, 0.2), transparent);
            color: var(--accent-red);
            transition: all 0.3s ease;
        }

        .risk-alert {
            background: linear-gradient(135deg, rgba(255, 61, 87, 0.1), transparent);
            border: 1px solid rgba(255, 61, 87, 0.3);
            border-radius: 12px;
            padding: 20px;
            margin-top: 20px;
            text-align: center;
            transition: all 0.4s ease;
            cursor: pointer;
        }

        .risk-alert:hover {
            transform: translateY(-5px);
            border-color: var(--accent-red);
            box-shadow: 0 10px 25px rgba(255, 61, 87, 0.2);
        }

        .risk-value {
            font-size: 28px;
            font-weight: 700;
            color: var(--accent-red);
            margin: 10px 0;
            transition: all 0.3s ease;
        }

        .risk-alert:hover .risk-value {
            transform: scale(1.1);
        }

        /* Footer */
        .footer {
            position: fixed;
            bottom: 80px;
            left: var(--sidebar-width);
            right: 0;
            height: var(--footer-height);
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding: 0 30px;
            color: var(--text-secondary);
            font-size: 14px;
            border-top: 1px solid var(--border-color);
            background: linear-gradient(135deg, rgba(15, 17, 23, 0.9), rgba(15, 17, 23, 0.95));
            z-index: 90;
            backdrop-filter: blur(10px);
            opacity: 0;
            animation: fadeIn 0.8s ease-out 1.2s forwards;
        }

        /* Responsive adjustments */
        @media (max-width: 1400px) {
            .content-area {
                flex-direction: column;
            }
            
            .right-panel {
                flex-direction: row;
                flex-wrap: wrap;
            }
            
            .right-panel .card {
                min-width: 250px;
                flex: 1;
            }
        }

        /* Scrollbar Styling */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
        }

        ::-webkit-scrollbar-thumb {
            background: linear-gradient(135deg, var(--accent), var(--accent-purple));
            border-radius: 10px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: linear-gradient(135deg, var(--accent-purple), var(--accent));
        }

        /* Floating Animations */
        .floating {
            animation: floating 3s ease-in-out infinite;
        }

        @keyframes floating {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
            100% { transform: translateY(0px); }
        }

        /* Notification Animation */
        @keyframes notification {
            0% { transform: scale(0); opacity: 0; }
            50% { transform: scale(1.1); opacity: 1; }
            100% { transform: scale(1); opacity: 1; }
        }
    </style>
</head>
<body>
    <!-- Sidebar -->
    <div class="sidebar">
        <div class="logo">
            <i class="fas fa-robot"></i>
            HybridTrader
        </div>
        
        <ul class="menu">
            <li class="active">
                <i class="fas fa-tachometer-alt"></i>
                Dashboard
            </li>
            <li>
                <i class="fas fa-chart-bar"></i>
                Reports
            </li>
            <li>
                <i class="fas fa-calendar-alt"></i>
                Calendar
            </li>
            <li>
                <i class="fas fa-bolt"></i>
                AI Signals
            </li>
            <li>
                <i class="fas fa-globe"></i>
                Macro Desk
            </li>
            <li>
                <i class="fas fa-wallet"></i>
                Portfolio
            </li>
            <li>
                <i class="fas fa-shield-alt"></i>
                Risk
            </li>
            <li>
                <i class="fas fa-cog"></i>
                Settings
            </li>
        </ul>
        
        <div class="user-profile">
            <div class="user-avatar">JD</div>
            <div class="user-info">
                <h4>John Doe</h4>
                <p>Senior Trader</p>
            </div>
        </div>
    </div>
    
    <!-- Main Content -->
    <div class="main-content">
        <!-- Header -->
        <div class="header">
            <div class="top-tickers">
                <div class="ticker-item">
                    <div class="ticker-name">Total Balance</div>
                    <div class="ticker-value">$61,528.74 <span class="ticker-change positive">+55.443.78</span></div>
                </div>
                <div class="ticker-item">
                    <div class="ticker-name">BTCUSD</div>
                    <div class="ticker-value">66,129.20</div>
                </div>
                <div class="ticker-item">
                    <div class="ticker-name">ETHUSD</div>
                    <div class="ticker-value">2,978.41</div>
                </div>
                <div class="ticker-item">
                    <div class="ticker-name">EURUSD</div>
                    <div class="ticker-value">1.0852 <span class="ticker-change positive">+0.21%</span></div>
                </div>
                <div class="ticker-item">
                    <div class="ticker-name">US100</div>
                    <div class="ticker-value">16,820 <span class="ticker-change negative">-0.7%</span></div>
                </div>
                <div class="ticker-item">
                    <div class="ticker-name">SPX</div>
                    <div class="ticker-value">5,116.80 <span class="ticker-change positive">+0.24%</span></div>
                </div>
            </div>
            
            <div class="confidence-container">
                <div class="confidence-label">Confidence</div>
                <div class="confidence-value">42%</div>
            </div>
        </div>
        
        <!-- Content Area -->
        <div class="content-area">
            <!-- Left Panel -->
            <div class="left-panel">
                <!-- Portfolio Overview -->
                <div class="card animate-in" style="animation-delay: 0.1s">
                    <div class="card-header">
                        <div>
                            <div class="card-title">Portfolio Overview</div>
                            <div class="card-subtitle">Real-time balances and P&L across all accounts</div>
                        </div>
                    </div>
                    
                    <div class="portfolio-overview">
                        <div class="account-card" style="animation-delay: 0.2s">
                            <div class="account-name">IBKR</div>
                            <div class="account-balance">41,232.84</div>
                            <div class="account-pl positive">
                                <span>P&L: +742.30</span>
                                <span>+782.30</span>
                            </div>
                        </div>
                        
                        <div class="account-card green" style="animation-delay: 0.3s">
                            <div class="account-name">Alpaca</div>
                            <div class="account-balance">12,246.65</div>
                            <div class="account-pl positive">$12246.65</div>
                        </div>
                        
                        <div class="account-card red" style="animation-delay: 0.4s">
                            <div class="account-name">OANDA</div>
                            <div class="account-balance">8,049.25</div>
                            <div class="account-pl negative">
                                <span>P&L: -91.65</span>
                                <span>91.65</span>
                            </div>
                        </div>
                        
                        <div class="account-card" style="animation-delay: 0.5s">
                            <div class="account-name">GDP Growth SA (Q1)</div>
                            <div class="account-balance">121.23</div>
                            <div class="account-pl positive">
                                <span>11.73%</span>
                                <span>10.50%</span>
                            </div>
                        </div>
                        
                        <div class="account-card green" style="animation-delay: 0.6s">
                            <div class="account-name">M. Durable Goods Orders</div>
                            <div class="account-balance">130.90</div>
                            <div class="account-pl positive">
                                <span>AUL Advisors</span>
                                <span>13.40%</span>
                            </div>
                        </div>
                    </div>
                </div>
                
                <!-- Full Market Overview -->
                <div class="card animate-in" style="animation-delay: 0.3s">
                    <div class="card-header">
                        <div class="card-title">Full Market Overview</div>
                    </div>
                    
                    <div class="market-overview">
                        <div class="market-item" style="animation-delay: 0.4s">
                            <div class="market-symbol">US30</div>
                            <div class="market-price">+01.98%</div>
                            <div class="market-change positive">+0.98%</div>
                        </div>
                        
                        <div class="market-item" style="animation-delay: 0.5s">
                            <div class="market-symbol">US100</div>
                            <div class="market-price">6,920</div>
                            <div class="market-change positive">+0.77%</div>
                        </div>
                        
                        <div class="market-item" style="animation-delay: 0.6s">
                            <div class="market-symbol">SPX</div>
                            <div class="market-price">5,116.60</div>
                            <div class="market-change positive">+0.24%</div>
                        </div>
                        
                        <div class="market-item" style="animation-delay: 0.7s">
                            <div class="market-symbol">EURUSD</div>
                            <div class="market-price">1.0852</div>
                            <div class="market-change positive">+0.21%</div>
                        </div>
                        
                        <div class="market-item" style="animation-delay: 0.8s">
                            <div class="market-symbol">S&P500</div>
                            <div class="market-price">3,510</div>
                            <div class="market-change negative">-0.08%</div>
                        </div>
                    </div>
                </div>
                
                <!-- Chart Container -->
                <div class="card animate-in" style="animation-delay: 0.5s">
                    <div class="card-header">
                        <div class="card-title">EURUSD - 1D Chart</div>
                    </div>
                    
                    <div class="chart-container">
                        <canvas id="priceChart"></canvas>
                    </div>
                </div>
                
                <!-- AI Signal -->
                <div class="card ai-signal animate-in" style="animation-delay: 0.7s">
                    <div class="signal-title">
                        <i class="fas fa-bolt"></i>
                        AI Signal
                    </div>
                    <div class="signal-content">
                        Bullish macro expectations, EURUSD retracement target 1.0950
                    </div>
                    <div class="signal-target">Target: 1.0950 (+0.89%)</div>
                </div>
                
                <!-- Positions -->
                <div class="card animate-in" style="animation-delay: 0.9s">
                    <div class="card-header">
                        <div class="card-title">Positions</div>
                    </div>
                    
                    <table class="positions-table">
                        <thead>
                            <tr>
                                <th>Symbols</th>
                                <th>Price</th>
                                <th>CCI</th>
                                <th>EOS</th>
                                <th>Risk Soft P&L</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr style="animation-delay: 1.0s">
                                <td class="symbol-cell">
                                    <div class="symbol-icon">T</div>
                                    <div>TSLA</div>
                                </td>
                                <td class="price-up">↑171.43</td>
                                <td>+64</td>
                                <td class="price-up">+1.35%</td>
                                <td class="price-up">↑+1.25%</td>
                            </tr>
                            <tr style="animation-delay: 1.1s">
                                <td class="symbol-cell">
                                    <div class="symbol-icon">E</div>
                                    <div>EURUSD</div>
                                </td>
                                <td>1.21139</td>
                                <td>4.34</td>
                                <td class="price-up">+1.96%</td>
                                <td class="price-up">↑+1.14%</td>
                            </tr>
                            <tr style="animation-delay: 1.2s">
                                <td class="symbol-cell">
                                    <div class="symbol-icon">A</div>
                                    <div>AAPL</div>
                                </td>
                                <td>176.32</td>
                                <td>+3d</td>
                                <td class="price-up">+0.27%</td>
                                <td class="price-up">↑+0.02%</td>
                            </tr>
                            <tr style="animation-delay: 1.3s">
                                <td class="symbol-cell">
                                    <div class="symbol-icon">G</div>
                                    <div>GBPUSD</div>
                                </td>
                                <td>1.27105</td>
                                <td>425</td>
                                <td class="price-down">-0.30%</td>
                                <td class="price-down">↓-0.30%</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
            
            <!-- Right Panel -->
            <div class="right-panel">
                <!-- Total Exposure -->
                <div class="card exposure-widget animate-in" style="animation-delay: 0.2s">
                    <div class="card-title">Total Exposure</div>
                    <div class="exposure-value">$52,097</div>
                    <div class="card-subtitle">Across all accounts and instruments</div>
                </div>
                
                <!-- Stock Heatmap -->
                <div class="card animate-in" style="animation-delay: 0.4s">
                    <div class="card-header">
                        <div class="card-title">Stock Heatmap</div>
                    </div>
                    
                    <div class="heatmap-container">
                        <div class="heatmap-item up" style="animation-delay: 0.5s">AAPL</div>
                        <div class="heatmap-item up" style="animation-delay: 0.6s">NVDA</div>
                        <div class="heatmap-item down" style="animation-delay: 0.7s">MSFT</div>
                        <div class="heatmap-item neutral" style="animation-delay: 0.8s">AMZN</div>
                        <div class="heatmap-item up" style="animation-delay: 0.9s">S&P500</div>
                        <div class="heatmap-item down" style="animation-delay: 1.0s">XAUUSD</div>
                        <div class="heatmap-item neutral" style="animation-delay: 1.1s">AMA</div>
                        <div class="heatmap-item up" style="animation-delay: 1.2s">PMI</div>
                        <div class="heatmap-item down" style="animation-delay: 1.3s">RSI</div>
                    </div>
                </div>
                
                <!-- Market Sentiment -->
                <div class="card animate-in" style="animation-delay: 0.6s">
                    <div class="card-header">
                        <div class="card-title">Market Sentiment</div>
                    </div>
                    
                    <div class="sentiment-container">
                        <div class="sentiment-meter">
                            <canvas id="sentimentChart"></canvas>
                        </div>
                        <div class="sentiment-label">Stressed - 65%</div>
                        
                        <div class="risk-alert">
                            <div>Liquidation Risk</div>
                            <div class="risk-value">$148.2k</div>
                            <div class="card-subtitle">Monitor positions closely</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Footer -->
        <div class="footer">
            <div>Smarter Retriever: 839</div>
        </div>
        
        <!-- Bottom Action Menu -->
        <div class="bottom-action">
            <div class="confidence-indicator">
                <span>Confidence: 71%</span>
                <div class="confidence-bar">
                    <div class="confidence-fill"></div>
                </div>
            </div>
            
            <button class="buy-action">
                <i class="fas fa-shopping-cart"></i>
                BUY EURUSD
            </button>
        </div>
    </div>

    <script>
        // Initialize charts when the page loads
        document.addEventListener('DOMContentLoaded', function() {
            // Animate cards in sequence
            const cards = document.querySelectorAll('.card');
            cards.forEach((card, index) => {
                card.style.animationDelay = `${0.1 * (index + 1)}s`;
                card.classList.add('animate-in');
            });
            
            // Animate account cards
            const accountCards = document.querySelectorAll('.account-card');
            accountCards.forEach((card, index) => {
                card.style.animation = `cardSlideUp 0.5s cubic-bezier(0.23, 1, 0.32, 1) ${0.2 + (index * 0.1)}s forwards`;
                card.style.opacity = '0';
            });
            
            // Animate market items
            const marketItems = document.querySelectorAll('.market-item');
            marketItems.forEach((item, index) => {
                item.style.animation = `cardSlideUp 0.5s cubic-bezier(0.23, 1, 0.32, 1) ${0.4 + (index * 0.1)}s forwards`;
                item.style.opacity = '0';
            });
            
            // Animate heatmap items
            const heatmapItems = document.querySelectorAll('.heatmap-item');
            heatmapItems.forEach((item, index) => {
                item.style.animation = `cardSlideUp 0.5s cubic-bezier(0.23, 1, 0.32, 1) ${0.5 + (index * 0.1)}s forwards`;
                item.style.opacity = '0';
            });
            
            // Animate table rows
            const tableRows = document.querySelectorAll('.positions-table tbody tr');
            tableRows.forEach((row, index) => {
                row.style.animation = `cardSlideUp 0.5s cubic-bezier(0.23, 1, 0.32, 1) ${1.0 + (index * 0.1)}s forwards`;
                row.style.opacity = '0';
            });

            // Price Chart
            const priceCtx = document.getElementById('priceChart').getContext('2d');
            const priceChart = new Chart(priceCtx, {
                type: 'line',
                data: {
                    labels: ['9:30', '10:00', '10:30', '11:00', '11:30', '12:00', '12:30', '13:00', '13:30', '14:00', '14:30', '15:00', '15:30', '16:00'],
                    datasets: [{
                        label: 'EURUSD',
                        data: [1.0820, 1.0835, 1.0840, 1.0830, 1.0845, 1.0850, 1.0855, 1.0860, 1.0850, 1.0845, 1.0855, 1.0865, 1.0855, 1.0852],
                        borderColor: '#2962ff',
                        backgroundColor: 'rgba(41, 98, 255, 0.1)',
                        borderWidth: 3,
                        fill: true,
                        tension: 0.4,
                        pointBackgroundColor: '#2962ff',
                        pointBorderColor: '#ffffff',
                        pointBorderWidth: 2,
                        pointRadius: 4,
                        pointHoverRadius: 8
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    animation: {
                        duration: 2000,
                        easing: 'easeOutQuart'
                    },
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            mode: 'index',
                            intersect: false,
                            backgroundColor: 'rgba(26, 29, 41, 0.9)',
                            titleColor: '#ffffff',
                            bodyColor: '#a0a5b8',
                            borderColor: '#2962ff',
                            borderWidth: 1,
                            cornerRadius: 8,
                            padding: 12
                        }
                    },
                    scales: {
                        x: {
                            grid: {
                                color: 'rgba(255, 255, 255, 0.05)',
                                drawBorder: false
                            },
                            ticks: {
                                color: '#a0a5b8',
                                font: {
                                    size: 11
                                }
                            }
                        },
                        y: {
                            grid: {
                                color: 'rgba(255, 255, 255, 0.05)',
                                drawBorder: false
                            },
                            ticks: {
                                color: '#a0a5b8',
                                font: {
                                    size: 11
                                },
                                callback: function(value) {
                                    return value.toFixed(4);
                                }
                            }
                        }
                    }
                }
            });
            
            // Sentiment Gauge Chart
            const sentimentCtx = document.getElementById('sentimentChart').getContext('2d');
            const sentimentChart = new Chart(sentimentCtx, {
                type: 'doughnut',
                data: {
                    labels: ['Stressed', 'Neutral', 'Calm'],
                    datasets: [{
                        data: [65, 25, 10],
                        backgroundColor: [
                            '#ff3d57',
                            '#ffc107',
                            '#00e676'
                        ],
                        borderWidth: 0,
                        circumference: 180,
                        rotation: 270,
                        borderRadius: 10,
                        spacing: 5
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    cutout: '70%',
                    animation: {
                        animateScale: true,
                        animateRotate: true,
                        duration: 2000,
                        easing: 'easeOutQuart'
                    },
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return context.label + ': ' + context.parsed + '%';
                                }
                            }
                        }
                    }
                }
            });
            
            // Menu interactivity with smooth transitions
            const menuItems = document.querySelectorAll('.menu li');
            menuItems.forEach(item => {
                item.addEventListener('click', function() {
                    menuItems.forEach(i => {
                        i.classList.remove('active');
                        i.style.transform = 'translateX(0)';
                    });
                    this.classList.add('active');
                    this.style.transform = 'translateX(5px)';
                    
                    // Add a subtle ripple effect
                    const ripple = document.createElement('span');
                    const rect = this.getBoundingClientRect();
                    const size = Math.max(rect.width, rect.height);
                    const x = event.clientX - rect.left - size / 2;
                    const y = event.clientY - rect.top - size / 2;
                    
                    ripple.style.cssText = `
                        position: absolute;
                        border-radius: 50%;
                        background: rgba(41, 98, 255, 0.3);
                        transform: scale(0);
                        animation: ripple 0.6s linear;
                        width: ${size}px;
                        height: ${size}px;
                        left: ${x}px;
                        top: ${y}px;
                    `;
                    
                    this.style.position = 'relative';
                    this.appendChild(ripple);
                    
                    setTimeout(() => {
                        ripple.remove();
                    }, 600);
                });
            });
            
            // Add ripple effect CSS
            const style = document.createElement('style');
            style.textContent = `
                @keyframes ripple {
                    to {
                        transform: scale(4);
                        opacity: 0;
                    }
                }
            `;
            document.head.appendChild(style);
            
            // Buy button action with animation
            const buyButton = document.querySelector('.buy-action');
            buyButton.addEventListener('click', function() {
                // Button press animation
                this.style.transform = 'scale(0.95)';
                setTimeout(() => {
                    this.style.transform = '';
                }, 150);
                
                // Create confirmation notification
                const notification = document.createElement('div');
                notification.textContent = 'BUY EURUSD order placed with 71% confidence';
                notification.style.cssText = `
                    position: fixed;
                    top: 100px;
                    right: 30px;
                    background: linear-gradient(135deg, #00e676, #00b248);
                    color: white;
                    padding: 15px 25px;
                    border-radius: 10px;
                    font-weight: 600;
                    z-index: 1000;
                    box-shadow: 0 10px 30px rgba(0, 230, 118, 0.3);
                    animation: notification 0.5s cubic-bezier(0.23, 1, 0.32, 1);
                    max-width: 300px;
                `;
                
                document.body.appendChild(notification);
                
                // Remove notification after 3 seconds
                setTimeout(() => {
                    notification.style.animation = 'fadeOut 0.5s ease-out forwards';
                    setTimeout(() => notification.remove(), 500);
                }, 3000);
            });
            
            // Add fadeOut animation
            const fadeOutStyle = document.createElement('style');
            fadeOutStyle.textContent = `
                @keyframes fadeOut {
                    from { opacity: 1; transform: translateY(0); }
                    to { opacity: 0; transform: translateY(-20px); }
                }
            `;
            document.head.appendChild(fadeOutStyle);
            
            // Heatmap interactivity with hover effects
            heatmapItems.forEach(item => {
                item.addEventListener('mouseenter', function() {
                    this.style.transform = 'translateY(-5px) scale(1.05)';
                    this.style.zIndex = '10';
                });
                
                item.addEventListener('mouseleave', function() {
                    this.style.transform = '';
                    this.style.zIndex = '';
                });
                
                item.addEventListener('click', function() {
                    const symbol = this.textContent;
                    console.log(`Selected ${symbol} from heatmap`);
                    
                    // Pulse animation on click
                    this.style.animation = 'none';
                    setTimeout(() => {
                        this.style.animation = 'pulseChange 0.5s ease';
                    }, 10);
                });
            });
            
            // Simulate real-time updates with smooth transitions
            function updateTickerValues() {
                // Simulate small price changes
                const tickerValues = document.querySelectorAll('.ticker-value');
                tickerValues.forEach(ticker => {
                    if (ticker.textContent.includes('$')) return; // Skip total balance
                    
                    const currentText = ticker.childNodes[0].textContent.trim();
                    const currentValue = parseFloat(currentText.replace(/[^0-9.-]+/g, ""));
                    if (!isNaN(currentValue)) {
                        const change = (Math.random() - 0.5) * 0.1;
                        const newValue = currentValue * (1 + change/100);
                        const changeElement = ticker.querySelector('.ticker-change');
                        
                        if (changeElement) {
                            // Add updating animation
                            changeElement.classList.add('updating');
                            setTimeout(() => {
                                changeElement.classList.remove('updating');
                            }, 500);
                            
                            const isPositive = change >= 0;
                            const newChangeText = (isPositive ? '+' : '') + change.toFixed(2) + '%';
                            
                            // Smooth transition for change text
                            changeElement.style.opacity = '0';
                            changeElement.style.transform = 'translateY(5px)';
                            setTimeout(() => {
                                changeElement.textContent = newChangeText;
                                changeElement.className = isPositive ? 'ticker-change positive' : 'ticker-change negative';
                                changeElement.style.opacity = '1';
                                changeElement.style.transform = 'translateY(0)';
                            }, 250);
                        }
                        
                        // Smooth transition for main value
                        ticker.childNodes[0].style.opacity = '0';
                        ticker.childNodes[0].style.transform = 'translateY(5px)';
                        setTimeout(() => {
                            ticker.childNodes[0].textContent = newValue.toFixed(2) + ' ';
                            ticker.childNodes[0].style.opacity = '1';
                            ticker.childNodes[0].style.transform = 'translateY(0)';
                        }, 250);
                    }
                });
                
                // Update confidence indicator with smooth animation
                const confidenceValue = document.querySelector('.confidence-value');
                const confidenceFill = document.querySelector('.confidence-fill');
                let currentConfidence = parseInt(confidenceValue.textContent);
                const change = (Math.random() - 0.5) * 2;
                const newConfidence = Math.min(100, Math.max(0, currentConfidence + change));
                
                // Animate confidence value
                confidenceValue.style.opacity = '0.5';
                confidenceValue.style.transform = 'scale(0.9)';
                setTimeout(() => {
                    confidenceValue.textContent = Math.round(newConfidence) + '%';
                    confidenceValue.style.opacity = '1';
                    confidenceValue.style.transform = 'scale(1)';
                }, 300);
                
                // Update confidence bar with smooth animation
                confidenceFill.style.width = newConfidence + '%';
                
                // Update bottom confidence indicator
                const bottomConfidence = document.querySelector('.confidence-indicator span');
                bottomConfidence.style.opacity = '0.5';
                bottomConfidence.style.transform = 'scale(0.9)';
                setTimeout(() => {
                    bottomConfidence.textContent = 'Confidence: ' + Math.round(newConfidence) + '%';
                    bottomConfidence.style.opacity = '1';
                    bottomConfidence.style.transform = 'scale(1)';
                }, 300);
                
                // Randomly update some portfolio values
                const accountBalances = document.querySelectorAll('.account-balance');
                accountBalances.forEach(balance => {
                    if (Math.random() > 0.7) {
                        const currentValue = parseFloat(balance.textContent.replace(/[^0-9.-]+/g, ""));
                        if (!isNaN(currentValue)) {
                            const change = (Math.random() - 0.5) * 0.5;
                            const newValue = currentValue * (1 + change/100);
                            
                            // Add update animation
                            balance.style.color = change >= 0 ? 'var(--accent-green)' : 'var(--accent-red)';
                            balance.style.transform = 'scale(1.05)';
                            
                            setTimeout(() => {
                                balance.textContent = newValue.toFixed(2);
                                balance.style.color = '';
                                balance.style.transform = '';
                            }, 300);
                        }
                    }
                });
                
                // Animate chart data point
                const chartData = priceChart.data.datasets[0].data;
                const lastValue = chartData[chartData.length - 1];
                const newValue = lastValue * (1 + (Math.random() - 0.5) * 0.001);
                chartData.push(newValue);
                chartData.shift();
                priceChart.update('none');
            }
            
            // Update values every 3 seconds
            setInterval(updateTickerValues, 3000);
            
            // Add floating animation to some elements
            setTimeout(() => {
                const floatingElements = document.querySelectorAll('.ai-signal, .exposure-widget, .confidence-container');
                floatingElements.forEach(el => {
                    el.classList.add('floating');
                });
            }, 2000);
            
            // Initialize with first update
            setTimeout(updateTickerValues, 1000);
        });
    </script>
</body>
</html>
