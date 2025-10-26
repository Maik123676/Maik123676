<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>History Learning Module - Pensmith Education</title>
    
    <!-- Include jsPDF library for PDF generation -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    
    <style>
        :root {
            --primary: #8B4513;
            --secondary: #DEB887;
            --accent: #A52A2A;
            --success: #32CD32;
            --warning: #FFD700;
            --coin: #FFD700;
            --light: #FFF8DC;
            --dark: #654321;
            --card-bg: #FFFFFF;
            --shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
            --ancient: #E6E6FA;
            --medieval: #F0FFF0;
            --modern: #FFF0F5;
            --sources: #F5F5DC;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #FFF8DC 0%, #F5F5DC 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            color: var(--dark);
        }
        
        .container {
            max-width: 1200px;
            width: 100%;
            background: var(--card-bg);
            border-radius: 20px;
            box-shadow: var(--shadow);
            overflow: hidden;
            position: relative;
            border: 5px solid var(--primary);
        }
        
        /* Pattern background */
        .pattern {
            position: absolute;
            width: 100%;
            height: 100%;
            opacity: 0.03;
            background-image: 
                linear-gradient(45deg, #8B4513 25%, transparent 25%),
                linear-gradient(-45deg, #8B4513 25%, transparent 25%),
                linear-gradient(45deg, transparent 75%, #8B4513 75%),
                linear-gradient(-45deg, transparent 75%, #8B4513 75%);
            background-size: 40px 40px;
            background-position: 0 0, 0 20px, 20px -20px, -20px 0px;
            z-index: 0;
        }
        
        header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 25px;
            text-align: center;
            position: relative;
            z-index: 1;
            border-bottom: 5px solid var(--accent);
        }
        
        .logo {
            font-size: 3rem;
            margin-bottom: 10px;
            filter: drop-shadow(0 4px 8px rgba(0,0,0,0.3));
        }
        
        h1 {
            font-size: 2.2rem;
            margin-bottom: 5px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        .subtitle {
            font-size: 1.1rem;
            opacity: 0.9;
            max-width: 600px;
            margin: 0 auto;
        }
        
        .academy-name {
            position: absolute;
            top: 25px;
            right: 25px;
            background: rgba(255, 255, 255, 0.2);
            padding: 8px 18px;
            border-radius: 25px;
            font-weight: bold;
            backdrop-filter: blur(10px);
        }
        
        .coin-display {
            position: absolute;
            top: 75px;
            right: 25px;
            background: rgba(255, 255, 255, 0.2);
            padding: 8px 18px;
            border-radius: 25px;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 8px;
            backdrop-filter: blur(10px);
        }
        
        .coin-icon {
            color: var(--coin);
            filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
        }
        
        main {
            display: flex;
            flex-wrap: wrap;
            gap: 25px;
            padding: 25px;
            position: relative;
            z-index: 1;
        }
        
        .content-area {
            flex: 1;
            min-width: 300px;
        }
        
        .sidebar {
            width: 320px;
            background: var(--light);
            border-radius: 15px;
            padding: 25px;
            box-shadow: var(--shadow);
            border: 3px solid var(--primary);
        }
        
        .card {
            background: var(--card-bg);
            border-radius: 15px;
            padding: 25px;
            margin-bottom: 25px;
            box-shadow: var(--shadow);
            border: 2px solid transparent;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 5px;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
        }
        
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 20px rgba(0, 0, 0, 0.2);
        }
        
        .welcome-card {
            text-align: center;
            animation: fadeIn 0.8s ease;
            background: linear-gradient(135deg, #ffffff, #f8f9fa);
            border: 3px solid var(--primary);
        }
        
        .profile-card {
            display: none;
            text-align: center;
        }
        
        .learning-card {
            display: none;
        }
        
        .quiz-card {
            display: none;
        }
        
        .level-indicator {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
            padding-bottom: 20px;
            border-bottom: 2px solid #eee;
        }
        
        .level-tag {
            background: var(--accent);
            color: white;
            padding: 10px 20px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
        }
        
        .progress-container {
            width: 60%;
        }
        
        .progress-bar {
            height: 12px;
            background: #e9ecef;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
        }
        
        .progress {
            height: 100%;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
            width: 0%;
            transition: width 0.5s ease;
            border-radius: 10px;
        }
        
        .concept-header {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-bottom: 20px;
            padding: 15px;
            background: linear-gradient(135deg, #f8f9fa, #e9ecef);
            border-radius: 15px;
        }
        
        .concept-icon {
            font-size: 3rem;
            background: var(--primary);
            color: white;
            width: 80px;
            height: 80px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            box-shadow: 0 6px 12px rgba(0,0,0,0.2);
        }
        
        .concept-name {
            font-size: 2rem;
            font-weight: 700;
            color: var(--primary);
            text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
        }
        
        .definition-box {
            background: linear-gradient(135deg, #e3f2fd, #bbdefb);
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 25px;
            border: 2px dashed var(--primary);
            position: relative;
            overflow: hidden;
        }
        
        .definition-box::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 5px;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
        }
        
        .definition-title {
            font-weight: bold;
            color: var(--primary);
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 1.2rem;
        }
        
        .info-box {
            background: linear-gradient(135deg, #fff8e1, #ffecb3);
            padding: 20px;
            border-radius: 12px;
            margin: 20px 0;
            border-left: 5px solid var(--accent);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        
        .info-title {
            font-size: 1.3rem;
            color: var(--accent);
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .points-list {
            margin: 20px 0;
            padding-left: 20px;
        }
        
        .point-item {
            margin-bottom: 10px;
            padding-left: 10px;
            border-left: 3px solid var(--primary);
        }
        
        .warning-box {
            background: linear-gradient(135deg, #ffebee, #ffcdd2);
            padding: 15px;
            border-radius: 10px;
            margin: 15px 0;
            border-left: 4px solid var(--accent);
            box-shadow: 0 3px 6px rgba(0,0,0,0.1);
        }
        
        .concepts-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }
        
        .concept-item {
            background: white;
            border-radius: 12px;
            padding: 15px;
            text-align: center;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
            border: 2px solid transparent;
            cursor: pointer;
        }
        
        .concept-item:hover {
            transform: translateY(-5px) scale(1.05);
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
        }
        
        .concept-icon-small {
            font-size: 2rem;
            margin-bottom: 10px;
        }
        
        .concept-text {
            font-weight: bold;
            color: var(--dark);
        }
        
        .ancient-concept { border-top: 4px solid var(--ancient); }
        .areas-concept { border-top: 4px solid var(--medieval); }
        .difference-concept { border-top: 4px solid var(--modern); }
        .sources-concept { border-top: 4px solid var(--sources); }
        
        .comparison-table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            border-radius: 10px;
            overflow: hidden;
        }
        
        .comparison-table th {
            background: var(--primary);
            color: white;
            padding: 15px;
            text-align: left;
        }
        
        .comparison-table td {
            padding: 12px 15px;
            border-bottom: 1px solid #ddd;
        }
        
        .comparison-table tr:nth-child(even) {
            background: #f9f9f9;
        }
        
        .comparison-table tr:hover {
            background: #f1f1f1;
        }
        
        .question {
            font-size: 1.3rem;
            font-weight: 600;
            margin-bottom: 20px;
            line-height: 1.5;
            padding: 20px;
            background: linear-gradient(135deg, #f8f9fa, #e9ecef);
            border-radius: 15px;
            border-left: 5px solid var(--primary);
        }
        
        .options {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .option {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 18px;
            background: var(--light);
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid transparent;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        
        .option:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
            border-color: var(--primary);
        }
        
        .option input {
            display: none;
        }
        
        .option-label {
            width: 35px;
            height: 35px;
            border-radius: 50%;
            background: var(--primary);
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            font-weight: bold;
            flex-shrink: 0;
            box-shadow: 0 3px 6px rgba(0,0,0,0.2);
        }
        
        .option-text {
            flex: 1;
            font-weight: 500;
        }
        
        .controls {
            display: flex;
            justify-content: space-between;
            margin-top: 25px;
            flex-wrap: wrap;
            gap: 15px;
        }
        
        button {
            padding: 15px 25px;
            border: none;
            border-radius: 12px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
            font-size: 1rem;
        }
        
        .btn-primary {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
        }
        
        .btn-primary:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 16px rgba(0,0,0,0.3);
        }
        
        .btn-secondary {
            background: var(--light);
            color: var(--dark);
            border: 2px solid #dee2e6;
        }
        
        .btn-secondary:hover {
            background: #e9ecef;
            transform: translateY(-3px);
        }
        
        .btn-accent {
            background: linear-gradient(135deg, var(--accent), #FF4500);
            color: white;
        }
        
        .btn-accent:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 16px rgba(0,0,0,0.3);
        }
        
        .btn-coin {
            background: linear-gradient(135deg, var(--coin), #FFC400);
            color: #333;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .btn-coin:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 16px rgba(0,0,0,0.3);
        }
        
        .btn-coin:disabled {
            background: #cccccc;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .hint-box {
            background: linear-gradient(135deg, #fff3cd, #ffeaa7);
            padding: 20px;
            border-radius: 12px;
            margin-top: 20px;
            border-left: 5px solid #ffc107;
            display: none;
            animation: fadeIn 0.5s ease;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        
        .hint-cost {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9rem;
            margin-top: 10px;
            color: #666;
        }
        
        .results-panel {
            display: none;
            margin-top: 25px;
            animation: slideDown 0.5s ease;
            text-align: center;
        }
        
        .player-info {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-bottom: 25px;
            padding: 20px;
            background: linear-gradient(135deg, #f8f9fa, #e9ecef);
            border-radius: 15px;
        }
        
        .avatar {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 2rem;
            font-weight: bold;
            box-shadow: 0 6px 12px rgba(0,0,0,0.2);
        }
        
        .stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-bottom: 25px;
        }
        
        .stat {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 15px;
            background: white;
            border-radius: 12px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        
        .stat-value {
            font-size: 2rem;
            font-weight: bold;
            color: var(--primary);
        }
        
        .stat-label {
            font-size: 0.9rem;
            color: #666;
            margin-top: 5px;
        }
        
        .badges {
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            margin-top: 15px;
            justify-content: center;
        }
        
        .badge {
            width: 60px;
            height: 60px;
            border-radius: 15px;
            background: #e9ecef;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.8rem;
            position: relative;
            transition: all 0.3s ease;
        }
        
        .badge.earned {
            background: linear-gradient(135deg, var(--accent), #FF4500);
            transform: scale(1.1);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
        }
        
        .badge-tooltip {
            position: absolute;
            bottom: -40px;
            left: 50%;
            transform: translateX(-50%);
            background: #333;
            color: white;
            padding: 8px 12px;
            border-radius: 6px;
            font-size: 0.8rem;
            white-space: nowrap;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s;
        }
        
        .badge:hover .badge-tooltip {
            opacity: 1;
        }
        
        .concept-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 20px;
            margin-top: 20px;
        }
        
        .concept-item-large {
            background: white;
            border-radius: 15px;
            padding: 25px;
            text-align: center;
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
            cursor: pointer;
            border: 3px solid transparent;
        }
        
        .concept-item-large:hover {
            transform: translateY(-8px);
            box-shadow: 0 12px 20px rgba(0, 0, 0, 0.2);
            border-color: var(--primary);
        }
        
        .concept-icon-large {
            font-size: 3rem;
            margin-bottom: 15px;
        }
        
        .concept-name-large {
            font-weight: bold;
            margin-bottom: 10px;
            color: var(--primary);
            font-size: 1.2rem;
        }
        
        .concept-description-large {
            font-size: 0.9rem;
            color: #666;
            line-height: 1.4;
        }
        
        .keyboard-hint {
            font-size: 0.9rem;
            color: #666;
            margin-top: 15px;
            text-align: center;
            padding: 10px;
            background: #f8f9fa;
            border-radius: 8px;
        }
        
        .coin-animation {
            position: fixed;
            font-size: 2.5rem;
            z-index: 1000;
            pointer-events: none;
            animation: coinFly 1s ease-out forwards;
            filter: drop-shadow(0 4px 8px rgba(0,0,0,0.3));
        }
        
        .celebration {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1000;
            display: none;
        }
        
        .confetti {
            position: absolute;
            width: 12px;
            height: 25px;
            opacity: 0;
        }
        
        footer {
            padding: 20px;
            text-align: center;
            background: var(--light);
            border-top: 3px solid var(--primary);
            font-size: 0.9rem;
            color: #666;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes slideDown {
            from { transform: translateY(-30px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        @keyframes coinFly {
            0% { transform: translate(0, 0) scale(1); opacity: 1; }
            100% { transform: translate(var(--tx), var(--ty)) scale(0.5); opacity: 0; }
        }
        
        @media (max-width: 768px) {
            main {
                flex-direction: column;
            }
            
            .sidebar {
                width: 100%;
            }
            
            .concept-grid {
                grid-template-columns: 1fr;
            }
            
            .concepts-grid {
                grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
            }
            
            .options {
                grid-template-columns: 1fr;
            }
            
            .controls {
                flex-direction: column;
            }
            
            .academy-name, .coin-display {
                position: static;
                margin-top: 15px;
                display: inline-block;
            }
            
            .comparison-table {
                font-size: 0.9rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="pattern"></div>
        
        <header>
            <div class="logo">📜</div>
            <h1>History Learning Module</h1>
            <div class="subtitle">Exploring Past Events, People, and Civilizations</div>
            <div class="academy-name">Pensmith Education</div>
            <div class="coin-display">
                <span class="coin-icon">🪙</span>
                <span id="coinCount">10</span> coins
            </div>
        </header>
        
        <main>
            <div class="content-area">
                <div class="card welcome-card" id="welcomeCard">
                    <h2>Welcome to History Learning!</h2>
                    <p>Explore the fascinating world of history - from ancient civilizations to modern events, and understand how the past shapes our present.</p>
                    <div class="concept-grid">
                        <div class="concept-item-large">
                            <div class="concept-icon-large">📚</div>
                            <div class="concept-name-large">Introduction to History</div>
                            <div class="concept-description-large">Understanding what history is and why it matters</div>
                        </div>
                        <div class="concept-item-large">
                            <div class="concept-icon-large">🏛️</div>
                            <div class="concept-name-large">Areas of History</div>
                            <div class="concept-description-large">Ancient, Medieval, Modern, and specialized historical studies</div>
                        </div>
                        <div class="concept-item-large">
                            <div class="concept-icon-large">📖</div>
                            <div class="concept-name-large">History vs Storytelling</div>
                            <div class="concept-description-large">Understanding the difference between factual history and creative storytelling</div>
                        </div>
                        <div class="concept-item-large">
                            <div class="concept-icon-large">🔍</div>
                            <div class="concept-name-large">Sources of History</div>
                            <div class="concept-description-large">Primary, secondary, and tertiary historical sources</div>
                        </div>
                        <div class="concept-item-large">
                            <div class="concept-icon-large">💡</div>
                            <div class="concept-name-large">Importance of History</div>
                            <div class="concept-description-large">Why history matters to individuals, society, and nations</div>
                        </div>
                        <div class="concept-item-large">
                            <div class="concept-icon-large">🇳🇬</div>
                            <div class="concept-name-large">Nok Culture</div>
                            <div class="concept-description-large">Exploring one of Africa's earliest civilizations</div>
                        </div>
                    </div>
                    <button class="btn-primary" id="startBtn" style="margin-top: 25px;">Create Profile & Start Learning</button>
                </div>
                
                <div class="card profile-card" id="profileCard">
                    <h2>Create Your Profile</h2>
                    <p>Enter your name to personalize your learning adventure</p>
                    <div style="margin: 25px 0;">
                        <input type="text" id="userName" placeholder="Enter your name" style="padding: 15px; width: 100%; max-width: 350px; border: 2px solid #ddd; border-radius: 10px; font-size: 1.1rem; text-align: center;">
                    </div>
                    <button class="btn-primary" id="saveProfileBtn">Save Profile & Start Learning</button>
                </div>
                
                <div class="card learning-card" id="learningCard">
                    <div class="level-indicator">
                        <div class="level-tag" id="conceptTag">Introduction to History</div>
                        <div class="progress-container">
                            <div class="progress-bar">
                                <div class="progress" id="progressBar"></div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="concept-header">
                        <div class="concept-icon" id="conceptIcon">📚</div>
                        <div class="concept-name" id="conceptName">Introduction to History</div>
                    </div>
                    
                    <div class="definition-box">
                        <div class="definition-title">
                            <span>📖</span> Definition
                        </div>
                        <div id="conceptDefinition" style="font-size: 1.1rem; line-height: 1.6;">
                            History is the study of past events, people, societies, and cultures. It helps us understand how the present world came to be by examining the successes, struggles, and experiences of those who lived before us.
                        </div>
                    </div>
                    
                    <div id="conceptContent">
                        <!-- Dynamic content will be loaded here -->
                    </div>
                    
                    <div class="controls">
                        <button class="btn-secondary" id="prevConceptBtn">Previous</button>
                        <button class="btn-primary" id="nextConceptBtn">Next Topic</button>
                        <button class="btn-accent" id="startQuizBtn">Take Quiz</button>
                    </div>
                </div>
                
                <div class="card quiz-card" id="quizCard">
                    <div class="level-indicator">
                        <div class="level-tag" id="levelTag">Question 1</div>
                        <div class="progress-container">
                            <div class="progress-bar">
                                <div class="progress" id="quizProgressBar"></div>
                            </div>
                        </div>
                    </div>
                    
                    <div class="question-container">
                        <div class="question" id="questionText"></div>
                        
                        <div class="options" id="optionsContainer">
                            <!-- Options will be inserted here by JavaScript -->
                        </div>
                        
                        <div class="hint-box" id="hintBox">
                            <div id="hintContent"></div>
                            <div class="hint-cost">
                                <span class="coin-icon">🪙</span>
                                <span>Cost: 1 coin</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="controls">
                        <button class="btn-coin" id="hintBtn">
                            <span class="coin-icon">🪙</span>
                            Get Hint (1 coin)
                        </button>
                        <button class="btn-primary" id="submitBtn">Submit Answer</button>
                        <button class="btn-accent" id="nextBtn" style="display: none;">Next Question</button>
                    </div>
                    
                    <div class="keyboard-hint">
                        Keyboard shortcuts: <kbd>1</kbd>-<kbd>4</kbd> to select options • <kbd>H</kbd> for hint • <kbd>Enter</kbd> to submit
                    </div>
                    
                    <div class="results-panel" id="resultsPanel">
                        <h3>Quiz Results</h3>
                        <div id="resultsList"></div>
                        <div class="controls" style="margin-top: 25px;">
                            <button class="btn-primary" id="restartBtn">Try Again</button>
                            <button class="btn-accent" id="backToLearningBtn">Back to Learning</button>
                            <button class="btn-accent" id="downloadBtn">Download Results (PDF)</button>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="sidebar">
                <div class="card">
                    <div class="player-info">
                        <div class="avatar" id="userAvatar">?</div>
                        <div>
                            <div style="font-weight: bold; font-size: 1.3rem;" id="userDisplayName">Student</div>
                            <div class="subtitle">Pensmith Education</div>
                        </div>
                    </div>
                    
                    <div class="stats">
                        <div class="stat">
                            <div class="stat-value" id="scoreValue">0</div>
                            <div class="stat-label">Score</div>
                        </div>
                        <div class="stat">
                            <div class="stat-value" id="conceptCount">0/8</div>
                            <div class="stat-label">Topics</div>
                        </div>
                        <div class="stat">
                            <div class="stat-value" id="correctCount">0</div>
                            <div class="stat-label">Correct</div>
                        </div>
                        <div class="stat">
                            <div class="stat-value" id="coinSidebar">10</div>
                            <div class="stat-label">Coins</div>
                        </div>
                    </div>
                </div>
                
                <div class="card">
                    <h3>History Topics</h3>
                    <div class="concepts-grid">
                        <div class="concept-item ancient-concept">
                            <div class="concept-icon-small">📚</div>
                            <div class="concept-text">Introduction</div>
                        </div>
                        <div class="concept-item areas-concept">
                            <div class="concept-icon-small">🏛️</div>
                            <div class="concept-text">Areas</div>
                        </div>
                        <div class="concept-item difference-concept">
                            <div class="concept-icon-small">📖</div>
                            <div class="concept-text">Difference</div>
                        </div>
                        <div class="concept-item sources-concept">
                            <div class="concept-icon-small">🔍</div>
                            <div class="concept-text">Sources</div>
                        </div>
                        <div class="concept-item ancient-concept">
                            <div class="concept-icon-small">💡</div>
                            <div class="concept-text">Importance</div>
                        </div>
                        <div class="concept-item areas-concept">
                            <div class="concept-icon-small">🏺</div>
                            <div class="concept-text">Sites</div>
                        </div>
                        <div class="concept-item difference-concept">
                            <div class="concept-icon-small">🇳🇬</div>
                            <div class="concept-text">Nok Culture</div>
                        </div>
                        <div class="concept-item sources-concept">
                            <div class="concept-icon-small">⚒️</div>
                            <div class="concept-text">Nok Iron</div>
                        </div>
                    </div>
                </div>
                
                <div class="card">
                    <h3>Achievements</h3>
                    <div class="badges">
                        <div class="badge" id="badge1">
                            📚
                            <div class="badge-tooltip">History Beginner</div>
                        </div>
                        <div class="badge" id="badge2">
                            🏛️
                            <div class="badge-tooltip">Ancient Scholar</div>
                        </div>
                        <div class="badge" id="badge3">
                            🔍
                            <div class="badge-tooltip">Research Expert</div>
                        </div>
                        <div class="badge" id="badge4">
                            🇳🇬
                            <div class="badge-tooltip">Nok Culture Master</div>
                        </div>
                    </div>
                </div>
                
                <div class="card">
                    <h3>Coin Rules</h3>
                    <ul style="padding-left: 20px; margin-top: 15px;">
                        <li>+5 coins for correct answers</li>
                        <li>-1 coin for using hints</li>
                        <li>Start with 10 coins</li>
                        <li>Earn badges for high scores</li>
                    </ul>
                </div>
            </div>
        </main>
        
        <footer>
            <p>Pensmith Education • History Learning Module • Created by Ador Michael Unim</p>
            <p>Created for Educational Purpose</p>
        </footer>
    </div>
    
    <div class="celebration" id="celebration"></div>

    <script>
        // History concepts
        const historyConcepts = [
            {
                name: "Introduction to History",
                icon: "📚",
                definition: "The study of past events, people, societies, and cultures.",
                description: "Understanding how the present world came to be through examination of the past.",
                content: `
                    <div class="info-box">
                        <div class="info-title">
                            <span>💡</span> What is History?
                        </div>
                        <p><strong>History</strong> is the study of past events, people, societies, and cultures. It helps us understand how the present world came to be by examining the successes, struggles, and experiences of those who lived before us.</p>
                        <p style="margin-top: 15px;">History records human achievements, failures, and the lessons learned over time, guiding individuals and nations toward a better future.</p>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>🎯</span> Learning Objectives
                        </div>
                        <p>By the end of this module, students should be able to:</p>
                        <div class="points-list">
                            <div class="point-item">Define history and explain its importance</div>
                            <div class="point-item">Identify different areas of historical study</div>
                            <div class="point-item">Differentiate between history and storytelling</div>
                            <div class="point-item">Identify primary, secondary, and tertiary sources</div>
                            <div class="point-item">Explain the importance of history to individuals, society, and nations</div>
                            <div class="point-item">Describe key historical sites in Nigeria</div>
                            <div class="point-item">Understand the significance of Nok Culture</div>
                        </div>
                    </div>
                    
                    <div style="margin-top: 25px; text-align: center;">
                        <div style="display: flex; justify-content: center; gap: 20px; flex-wrap: wrap;">
                            <div style="text-align: center;">
                                <div style="font-size: 3rem;">⏳</div>
                                <div>Past Events</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-size: 3rem;">👥</div>
                                <div>People & Societies</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-size: 3rem;">🏺</div>
                                <div>Cultural Heritage</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-size: 3rem;">📖</div>
                                <div>Lessons Learned</div>
                            </div>
                        </div>
                    </div>
                `
            },
            {
                name: "Areas of History",
                icon: "🏛️",
                definition: "Different branches of historical study focusing on specific aspects of the past.",
                description: "Ancient, Medieval, Modern, and specialized historical studies.",
                content: `
                    <div class="info-box">
                        <div class="info-title">
                            <span>📋</span> Areas of Historical Study
                        </div>
                        <p>History can be studied in different areas or branches, each focusing on specific aspects of the past:</p>
                    </div>
                    
                    <div class="comparison-table">
                        <thead>
                            <tr>
                                <th>Area of History</th>
                                <th>Focus</th>
                                <th>Examples</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td><strong>Ancient History</strong></td>
                                <td>Study of early civilizations and events before the Middle Ages</td>
                                <td>Egyptian, Greek, and Roman empires</td>
                            </tr>
                            <tr>
                                <td><strong>Medieval History</strong></td>
                                <td>Focuses on events that occurred during the Middle Ages</td>
                                <td>Feudal systems, Crusades, Renaissance beginnings</td>
                            </tr>
                            <tr>
                                <td><strong>Modern History</strong></td>
                                <td>Deals with more recent events from the 16th century to present</td>
                                <td>World Wars, Industrial Revolution, Digital Age</td>
                            </tr>
                            <tr>
                                <td><strong>War History</strong></td>
                                <td>Examines major wars, battles, and their effects</td>
                                <td>World War I & II, Civil wars, Revolutionary wars</td>
                            </tr>
                            <tr>
                                <td><strong>Political History</strong></td>
                                <td>Studies development of governments, laws, and political systems</td>
                                <td>Democracy evolution, Political revolutions</td>
                            </tr>
                            <tr>
                                <td><strong>Economic History</strong></td>
                                <td>Focuses on how trade, industry, and money systems evolved</td>
                                <td>Trade routes, Industrialization, Economic crises</td>
                            </tr>
                            <tr>
                                <td><strong>Social & Cultural History</strong></td>
                                <td>Looks at customs, beliefs, education, and lifestyles</td>
                                <td>Family structures, Religious practices, Cultural movements</td>
                            </tr>
                            <tr>
                                <td><strong>Religious History</strong></td>
                                <td>Studies how religions began and influenced societies</td>
                                <td>Spread of major religions, Religious conflicts</td>
                            </tr>
                        </tbody>
                    </div>
                `
            },
            {
                name: "History vs Storytelling",
                icon: "📖",
                definition: "The distinction between factual historical accounts and creative narratives.",
                description: "Understanding the difference between evidence-based history and imaginative storytelling.",
                content: `
                    <div class="info-box">
                        <div class="info-title">
                            <span>⚖️</span> Key Differences
                        </div>
                        <p>While both history and storytelling narrate events, they differ in purpose and method:</p>
                    </div>
                    
                    <div class="comparison-table">
                        <thead>
                            <tr>
                                <th>Aspect</th>
                                <th>History</th>
                                <th>Storytelling</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td><strong>Basis</strong></td>
                                <td>Factual evidence and real events</td>
                                <td>May include imagination, myths, or exaggerations</td>
                            </tr>
                            <tr>
                                <td><strong>Verification</strong></td>
                                <td>Verified through documents, artefacts, eyewitness accounts</td>
                                <td>Not necessarily verifiable; relies on narrative appeal</td>
                            </tr>
                            <tr>
                                <td><strong>Purpose</strong></td>
                                <td>Record truth and provide accurate understanding</td>
                                <td>Entertain, teach moral lessons, pass down cultural values</td>
                            </tr>
                            <tr>
                                <td><strong>Focus</strong></td>
                                <td>Truth and accuracy</td>
                                <td>Creativity and moral lessons</td>
                            </tr>
                            <tr>
                                <td><strong>Examples</strong></td>
                                <td>Historical documents, archaeological findings</td>
                                <td>Folktales, myths, legends, fictional stories</td>
                            </tr>
                        </tbody>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>💡</span> Important Note
                        </div>
                        <p>Thus, history focuses on truth and accuracy, while storytelling focuses on creativity and moral lessons. Both are valuable but serve different purposes in preserving and transmitting knowledge.</p>
                    </div>
                `
            },
            {
                name: "Sources of History",
                icon: "🔍",
                definition: "The materials and methods used to gather information about the past.",
                description: "Primary, secondary, and tertiary sources of historical information.",
                content: `
                    <div class="info-box">
                        <div class="info-title">
                            <span>📚</span> Three Main Categories of Historical Sources
                        </div>
                        <p>Historians use various sources to gather information about the past:</p>
                    </div>
                    
                    <div class="comparison-table">
                        <thead>
                            <tr>
                                <th>Source Type</th>
                                <th>Definition</th>
                                <th>Examples</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td><strong>Primary Sources</strong></td>
                                <td>Original materials or first-hand evidence from the period being studied</td>
                                <td>Letters, diaries, artefacts, photographs, official documents</td>
                            </tr>
                            <tr>
                                <td><strong>Secondary Sources</strong></td>
                                <td>Interpretations or analyses based on primary sources</td>
                                <td>Textbooks, research papers, biographies, documentaries</td>
                            </tr>
                            <tr>
                                <td><strong>Tertiary Sources</strong></td>
                                <td>Summaries or compilations of secondary sources</td>
                                <td>Encyclopedias, databases, almanacs</td>
                            </tr>
                        </tbody>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>🏺</span> Other Important Historical Sources
                        </div>
                        <div class="points-list">
                            <div class="point-item"><strong>Archaeology</strong> – The scientific study of past human life through excavations and analysis of artefacts, fossils, and ruins.</div>
                            <div class="point-item"><strong>Museums</strong> – Institutions that preserve and display historical objects, artworks, and relics.</div>
                            <div class="point-item"><strong>Oral Traditions</strong> – Information passed down by word of mouth from one generation to another.</div>
                            <div class="point-item"><strong>Written Records</strong> – Books, newspapers, journals, and archives that document past events.</div>
                        </div>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>🌐</span> How to Access Historical Information
                        </div>
                        <div class="points-list">
                            <div class="point-item">Libraries and archives</div>
                            <div class="point-item">Museums and historical monuments</div>
                            <div class="point-item">Online databases and digital libraries</div>
                            <div class="point-item">Interviews with elders or eyewitnesses</div>
                            <div class="point-item">Archaeological sites and findings</div>
                            <div class="point-item">Historical documentaries and journals</div>
                        </div>
                    </div>
                `
            },
            {
                name: "Importance of History",
                icon: "💡",
                definition: "The value and significance of studying history for individuals, society, and nations.",
                description: "Understanding why history matters in shaping our present and future.",
                content: `
                    <div class="info-box">
                        <div class="info-title">
                            <span>👤</span> Importance to the Individual
                        </div>
                        <div class="points-list">
                            <div class="point-item">It helps individuals understand their roots and cultural identity.</div>
                            <div class="point-item">It teaches moral lessons from past mistakes and successes.</div>
                            <div class="point-item">It inspires patriotism and pride in one's heritage.</div>
                            <div class="point-item">It develops critical thinking and analytical skills.</div>
                        </div>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>👥</span> Importance to Society
                        </div>
                        <div class="points-list">
                            <div class="point-item">It preserves the customs and traditions of a people.</div>
                            <div class="point-item">It promotes unity and understanding among diverse groups.</div>
                            <div class="point-item">It guides social progress and helps prevent the repetition of past errors.</div>
                        </div>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>🇺🇳</span> Importance to the Nation
                        </div>
                        <div class="points-list">
                            <div class="point-item">It builds national identity and cohesion.</div>
                            <div class="point-item">It helps leaders make informed decisions by learning from historical experiences.</div>
                            <div class="point-item">It promotes tourism through historical sites and monuments.</div>
                            <div class="point-item">It supports education, culture, and technological development.</div>
                        </div>
                    </div>
                    
                    <div style="margin-top: 25px; text-align: center;">
                        <div style="display: flex; justify-content: center; gap: 20px; flex-wrap: wrap;">
                            <div style="text-align: center;">
                                <div style="font-size: 3rem;">🔍</div>
                                <div>Learn from<br>Past Mistakes</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-size: 3rem;">🌍</div>
                                <div>Understand<br>Cultural Identity</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-size: 3rem;">🏛️</div>
                                <div>Guide<br>Future Decisions</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-size: 3rem;">🤝</div>
                                <div>Promote<br>National Unity</div>
                            </div>
                        </div>
                    </div>
                `
            },
            {
                name: "Historical Sites in Nigeria",
                icon: "🏺",
                definition: "Important historical locations in pre-colonial Nigeria reflecting rich cultural heritage.",
                description: "Exploring Nigeria's archaeological and cultural landmarks.",
                content: `
                    <div class="info-box">
                        <div class="info-title">
                            <span>🇳🇬</span> Historical Sites in Pre-Colonial Nigeria
                        </div>
                        <p>Nigeria has many historical sites that reflect its rich cultural and technological heritage:</p>
                    </div>
                    
                    <div class="comparison-table">
                        <thead>
                            <tr>
                                <th>Historical Site</th>
                                <th>Location</th>
                                <th>Significance</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td><strong>Nok and Benin</strong></td>
                                <td>Central Nigeria & Edo State</td>
                                <td>Centres of early civilisation and artistic achievement</td>
                            </tr>
                            <tr>
                                <td><strong>Kano City Wall</strong></td>
                                <td>Kano State</td>
                                <td>Built for defence and protection during ancient times</td>
                            </tr>
                            <tr>
                                <td><strong>Obudu Hills</strong></td>
                                <td>Cross River State</td>
                                <td>Known for its scenic beauty and ancient settlements</td>
                            </tr>
                            <tr>
                                <td><strong>Zuma Rock</strong></td>
                                <td>Niger State</td>
                                <td>A massive natural monolith with spiritual and cultural importance</td>
                            </tr>
                            <tr>
                                <td><strong>Olumo Rock</strong></td>
                                <td>Ogun State</td>
                                <td>A fortress used by the Egba people during inter-tribal wars</td>
                            </tr>
                        </tbody>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>🌟</span> Advantages of Nigeria's Historical Sites
                        </div>
                        <div class="points-list">
                            <div class="point-item">They attract <strong>tourists</strong>, boosting the nation's economy.</div>
                            <div class="point-item">They help <strong>preserve cultural heritage</strong> and promote education.</div>
                            <div class="point-item">They serve as <strong>centres of research and learning</strong> for students and historians.</div>
                            <div class="point-item">They strengthen <strong>national pride and identity</strong>.</div>
                        </div>
                    </div>
                `
            },
            {
                name: "Nok Culture",
                icon: "🇳🇬",
                definition: "One of West Africa's earliest known civilizations, famous for terracotta sculptures.",
                description: "Exploring the ancient Nok civilization and its significance.",
                content: `
                    <div class="info-box">
                        <div class="info-title">
                            <span>📍</span> Origin of Nok Culture
                        </div>
                        <p>The Nok Culture is one of the earliest known civilisations in West Africa. It existed in what is now <strong>Central Nigeria</strong>, particularly in <strong>Nok village</strong> in <strong>Kaduna State</strong>, between <strong>500 BC and 200 AD</strong> (some evidence suggests as early as 1900 BC).</p>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>⚒️</span> Evidence of Iron Smelting in Nok Culture
                        </div>
                        <p>Archaeological discoveries show that the Nok people were skilled iron workers. They developed furnaces for smelting iron, creating tools, weapons, and farming implements. This early technological advancement proves that ironworking in Africa began long before contact with Europeans.</p>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>🎨</span> Characteristics of Nok Art
                        </div>
                        <p>Nok art is famous for its <strong>terracotta (baked clay) sculptures</strong>, often depicting human and animal figures. The main features include:</p>
                        <div class="points-list">
                            <div class="point-item">Heads that are <strong>cylindrical, conical, or oval</strong> in shape.</div>
                            <div class="point-item"><strong>Wide open mouths</strong> and <strong>thick lips</strong>.</div>
                            <div class="point-item"><strong>Triangular eyes</strong> with detailed facial expressions.</div>
                            <div class="point-item"><strong>Pierced holes</strong> on the cheeks, nostrils, and ears, possibly for decoration or symbolic reasons.</div>
                        </div>
                        <p style="margin-top: 15px;">These artworks reveal the creativity, skill, and spiritual beliefs of the Nok people.</p>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>🏺</span> Occupational Activities of the Nok People
                        </div>
                        <p>The Nok people engaged in several economic activities such as:</p>
                        <div style="display: flex; justify-content: center; gap: 20px; flex-wrap: wrap; margin-top: 15px;">
                            <div style="text-align: center;">
                                <div style="font-size: 2.5rem;">⚒️</div>
                                <div>Iron Smelting</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-size: 2.5rem;">📿</div>
                                <div>Bead Making</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-size: 2.5rem;">🏹</div>
                                <div>Hunting</div>
                            </div>
                            <div style="text-align: center;">
                                <div style="font-size: 2.5rem;">🌾</div>
                                <div>Farming</div>
                            </div>
                        </div>
                    </div>
                `
            },
            {
                name: "Significance of Nok Culture",
                icon: "⚒️",
                definition: "The historical importance and legacy of the Nok civilization.",
                description: "Understanding why Nok Culture matters in African history.",
                content: `
                    <div class="info-box">
                        <div class="info-title">
                            <span>🌟</span> Significance of Nok Culture
                        </div>
                        <div class="points-list">
                            <div class="point-item">It demonstrates that <strong>civilisation began in Nigeria</strong> as early as <strong>1900 BC</strong>, proving that Africans developed advanced societies independently.</div>
                            <div class="point-item">It shows that <strong>Nigeria was technologically advanced</strong>, refuting the claim that Europeans introduced civilisation to Africa.</div>
                            <div class="point-item">The Nok Culture <strong>influenced later civilisations</strong> in Southern Nigeria, including the <strong>Ife</strong> and <strong>Benin Kingdoms</strong>.</div>
                        </div>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>🔍</span> Archaeological Evidence
                        </div>
                        <p>The discovery of Nok artifacts in the 20th century revolutionized our understanding of African history. Key findings include:</p>
                        <div class="points-list">
                            <div class="point-item"><strong>Terracotta sculptures</strong> showing advanced artistic skills</div>
                            <div class="point-item"><strong>Iron smelting furnaces</strong> demonstrating technological advancement</div>
                            <div class="point-item"><strong>Stone tools and weapons</strong> showing practical applications of their technology</div>
                            <div class="point-item"><strong>Evidence of settled agriculture</strong> and community organization</div>
                        </div>
                    </div>
                    
                    <div class="info-box">
                        <div class="info-title">
                            <span>🌍</span> Global Significance
                        </div>
                        <p>The Nok Culture holds importance beyond Nigeria and Africa:</p>
                        <div class="points-list">
                            <div class="point-item">It provides evidence of <strong>independent technological development</strong> in Africa</div>
                            <div class="point-item">It challenges <strong>Eurocentric views</strong> of history and civilization</div>
                            <div class="point-item">It demonstrates the <strong>sophistication of early African societies</strong></div>
                            <div class="point-item">It contributes to our understanding of <strong>human cultural evolution</strong> worldwide</div>
                        </div>
                    </div>
                    
                    <div style="margin-top: 25px; padding: 20px; background: linear-gradient(135deg, #e8f5e9, #c8e6c9); border-radius: 15px; text-align: center;">
                        <h4 style="color: var(--primary); margin-bottom: 15px;">Legacy of Nok Culture</h4>
                        <p>The Nok people left a lasting legacy that continues to influence our understanding of African history and contributes to Nigeria's rich cultural heritage today.</p>
                    </div>
                `
            }
        ];

        // Quiz questions (30 questions)
        const quizData = [
            {
                question: "What is history?",
                options: [
                    "The study of past events, people, societies, and cultures",
                    "The prediction of future events",
                    "The creation of fictional stories about the past",
                    "The study of only political events"
                ],
                correct: 0,
                hint: "It helps us understand how the present world came to be.",
                concept: "Introduction to History"
            },
            {
                question: "Which area of history focuses on events before the Middle Ages?",
                options: [
                    "Ancient History",
                    "Medieval History",
                    "Modern History",
                    "Political History"
                ],
                correct: 0,
                hint: "This includes civilizations like Egyptian, Greek, and Roman empires.",
                concept: "Areas of History"
            },
            {
                question: "What is the main difference between history and storytelling?",
                options: [
                    "History is based on factual evidence, storytelling may include imagination",
                    "History is always boring, storytelling is always exciting",
                    "History is written, storytelling is spoken",
                    "There is no difference"
                ],
                correct: 0,
                hint: "One focuses on truth and accuracy, the other on creativity.",
                concept: "History vs Storytelling"
            },
            {
                question: "Which of these is a primary source of historical information?",
                options: [
                    "A letter written during the time being studied",
                    "A history textbook",
                    "An encyclopedia entry",
                    "A documentary film made recently"
                ],
                correct: 0,
                hint: "Primary sources are original materials from the period.",
                concept: "Sources of History"
            },
            {
                question: "Why is history important to individuals?",
                options: [
                    "It helps understand roots and cultural identity",
                    "It predicts future stock market trends",
                    "It guarantees personal wealth",
                    "It makes people famous"
                ],
                correct: 0,
                hint: "It teaches moral lessons and develops critical thinking.",
                concept: "Importance of History"
            },
            {
                question: "Which historical site in Nigeria was used as a fortress during inter-tribal wars?",
                options: [
                    "Olumo Rock",
                    "Zuma Rock",
                    "Kano City Wall",
                    "Obudu Hills"
                ],
                correct: 0,
                hint: "This site was used by the Egba people.",
                concept: "Historical Sites in Nigeria"
            },
            {
                question: "Where was the Nok Culture located?",
                options: [
                    "Central Nigeria",
                    "Southern Nigeria",
                    "Eastern Nigeria",
                    "Western Nigeria"
                ],
                correct: 0,
                hint: "Specifically in Nok village in Kaduna State.",
                concept: "Nok Culture"
            },
            {
                question: "What material were Nok sculptures primarily made from?",
                options: [
                    "Terracotta (baked clay)",
                    "Bronze",
                    "Marble",
                    "Wood"
                ],
                correct: 0,
                hint: "This material is fired clay.",
                concept: "Nok Culture"
            },
            {
                question: "What technological advancement were the Nok people known for?",
                options: [
                    "Iron smelting",
                    "Paper making",
                    "Gunpowder production",
                    "Ship building"
                ],
                correct: 0,
                hint: "They created tools and weapons from this material.",
                concept: "Nok Culture"
            },
            {
                question: "Which time period did the Nok Culture exist?",
                options: [
                    "500 BC to 200 AD",
                    "1000 AD to 1500 AD",
                    "3000 BC to 2500 BC",
                    "1800 AD to present"
                ],
                correct: 0,
                hint: "Some evidence suggests it began even earlier.",
                concept: "Nok Culture"
            },
            {
                question: "What is NOT a characteristic of Nok art?",
                options: [
                    "Gold plating on sculptures",
                    "Triangular eyes",
                    "Wide open mouths",
                    "Pierced holes on cheeks and nostrils"
                ],
                correct: 0,
                hint: "Nok art is known for terracotta sculptures with specific facial features.",
                concept: "Nok Culture"
            },
            {
                question: "Why is the Nok Culture significant in African history?",
                options: [
                    "It proves Africans developed advanced societies independently",
                    "It was the first culture to use written language",
                    "It conquered most of West Africa",
                    "It invented the wheel"
                ],
                correct: 0,
                hint: "It refutes claims that Europeans introduced civilization to Africa.",
                concept: "Significance of Nok Culture"
            },
            {
                question: "Which of these is a secondary source of history?",
                options: [
                    "A biography written by a modern historian",
                    "An original diary from the 1800s",
                    "A photograph taken during World War II",
                    "A government document from the colonial period"
                ],
                correct: 0,
                hint: "Secondary sources interpret or analyze primary sources.",
                concept: "Sources of History"
            },
            {
                question: "What does archaeology contribute to historical study?",
                options: [
                    "Scientific study of past human life through artefacts",
                    "Study of ancient languages only",
                    "Prediction of future events",
                    "Creation of historical fiction"
                ],
                correct: 0,
                hint: "It involves excavations and analysis of physical remains.",
                concept: "Sources of History"
            },
            {
                question: "How does history benefit society?",
                options: [
                    "It promotes unity and understanding among diverse groups",
                    "It guarantees economic prosperity",
                    "It eliminates all social problems",
                    "It makes everyone agree on everything"
                ],
                correct: 0,
                hint: "It helps prevent repetition of past errors.",
                concept: "Importance of History"
            },
            {
                question: "What was the Kano City Wall used for?",
                options: [
                    "Defense and protection",
                    "Religious ceremonies",
                    "Agricultural storage",
                    "Water collection"
                ],
                correct: 0,
                hint: "It was built during ancient times for this purpose.",
                concept: "Historical Sites in Nigeria"
            },
            {
                question: "Which area of history studies the development of governments and political systems?",
                options: [
                    "Political History",
                    "Economic History",
                    "Social History",
                    "Religious History"
                ],
                correct: 0,
                hint: "This area focuses on laws and governance.",
                concept: "Areas of History"
            },
            {
                question: "What is a tertiary source of historical information?",
                options: [
                    "An encyclopedia that summarizes information",
                    "An original letter from a historical figure",
                    "A research paper analyzing primary sources",
                    "An artefact from an archaeological dig"
                ],
                correct: 0,
                hint: "Tertiary sources compile and summarize secondary sources.",
                concept: "Sources of History"
            },
            {
                question: "How does history help national development?",
                options: [
                    "It helps leaders make informed decisions from past experiences",
                    "It guarantees military victory in all conflicts",
                    "It makes the country the richest in the world",
                    "It eliminates the need for other subjects"
                ],
                correct: 0,
                hint: "Learning from historical experiences guides better decision-making.",
                concept: "Importance of History"
            },
            {
                question: "Which Nigerian historical site is a massive natural monolith?",
                options: [
                    "Zuma Rock",
                    "Olumo Rock",
                    "Kano City Wall",
                    "Nok Village"
                ],
                correct: 0,
                hint: "This rock has spiritual and cultural importance.",
                concept: "Historical Sites in Nigeria"
            },
            {
                question: "What economic activities did the Nok people engage in?",
                options: [
                    "Iron smelting, bead making, hunting, and farming",
                    "Only farming and nothing else",
                    "Trading with Europeans",
                    "Manufacturing textiles for export"
                ],
                correct: 0,
                hint: "They had diverse economic activities including craft production.",
                concept: "Nok Culture"
            },
            {
                question: "Which civilization was influenced by the Nok Culture?",
                options: [
                    "Ife and Benin Kingdoms",
                    "Egyptian Empire",
                    "Roman Empire",
                    "Chinese Dynasties"
                ],
                correct: 0,
                hint: "These are later civilizations in Southern Nigeria.",
                concept: "Significance of Nok Culture"
            },
            {
                question: "What is the main purpose of historical museums?",
                options: [
                    "Preserve and display historical objects and relics",
                    "Sell historical artefacts to tourists",
                    "Create fictional stories about the past",
                    "Predict future historical events"
                ],
                correct: 0,
                hint: "They serve educational and preservation purposes.",
                concept: "Sources of History"
            },
            {
                question: "Which area of history focuses on customs, beliefs, and lifestyles?",
                options: [
                    "Social and Cultural History",
                    "Political History",
                    "Economic History",
                    "War History"
                ],
                correct: 0,
                hint: "This area studies how people lived and what they believed.",
                concept: "Areas of History"
            },
            {
                question: "How does history develop critical thinking skills?",
                options: [
                    "By analyzing evidence and different interpretations",
                    "By memorizing dates and names",
                    "By copying information from textbooks",
                    "By ignoring conflicting information"
                ],
                correct: 0,
                hint: "It involves evaluating sources and constructing arguments.",
                concept: "Importance of History"
            },
            {
                question: "What advantage do historical sites provide for Nigeria?",
                options: [
                    "They attract tourists and boost the economy",
                    "They guarantee good weather for agriculture",
                    "They automatically increase population",
                    "They prevent all natural disasters"
                ],
                correct: 0,
                hint: "Tourism is a significant economic benefit.",
                concept: "Historical Sites in Nigeria"
            },
            {
                question: "What does the existence of Nok Culture prove about African history?",
                options: [
                    "Civilization began in Nigeria independently",
                    "Africans needed European contact to develop",
                    "All African cultures were the same",
                    "Nigeria was always the most powerful African nation"
                ],
                correct: 0,
                hint: "It shows advanced societies developed without external influence.",
                concept: "Significance of Nok Culture"
            },
            {
                question: "Which of these is NOT a way to access historical information?",
                options: [
                    "Predicting the future with crystal balls",
                    "Visiting libraries and archives",
                    "Interviewing elders or eyewitnesses",
                    "Studying archaeological findings"
                ],
                correct: 0,
                hint: "Historical research relies on evidence, not prediction.",
                concept: "Sources of History"
            },
            {
                question: "What period does Modern History cover?",
                options: [
                    "From the 16th century to present",
                    "From prehistoric times to 1000 AD",
                    "Only the 20th century",
                    "Only events from the last 10 years"
                ],
                correct: 0,
                hint: "This includes events from the Renaissance to today.",
                concept: "Areas of History"
            },
            {
                question: "How does history contribute to national identity?",
                options: [
                    "By building shared understanding of the past",
                    "By making everyone rich",
                    "By eliminating cultural differences",
                    "By predicting future political leaders"
                ],
                correct: 0,
                hint: "Shared historical experiences create cohesion.",
                concept: "Importance of History"
            }
        ];

        // Randomize quiz data to shuffle answers
        function randomizeQuizData() {
            return quizData.map(question => {
                // Create a copy of the question
                const randomizedQuestion = {...question};
                
                // Store the correct answer text
                const correctAnswer = question.options[question.correct];
                
                // Shuffle the options array
                const shuffledOptions = [...question.options];
                for (let i = shuffledOptions.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [shuffledOptions[i], shuffledOptions[j]] = [shuffledOptions[j], shuffledOptions[i]];
                }
                
                // Find the new index of the correct answer
                const newCorrectIndex = shuffledOptions.indexOf(correctAnswer);
                
                // Update the question with shuffled options and new correct index
                randomizedQuestion.options = shuffledOptions;
                randomizedQuestion.correct = newCorrectIndex;
                
                return randomizedQuestion;
            });
        }

        // Game state
        const state = {
            currentConcept: 0,
            currentQuestion: 0,
            score: 0,
            selectedOption: null,
            quizCompleted: false,
            coins: 10,
            hintUsed: false,
            userName: "",
            userInitials: "?",
            randomizedQuizData: []
        };

        // DOM elements
        const welcomeCard = document.getElementById('welcomeCard');
        const profileCard = document.getElementById('profileCard');
        const learningCard = document.getElementById('learningCard');
        const quizCard = document.getElementById('quizCard');
        const startBtn = document.getElementById('startBtn');
        const saveProfileBtn = document.getElementById('saveProfileBtn');
        const userNameInput = document.getElementById('userName');
        const userDisplayName = document.getElementById('userDisplayName');
        const userAvatar = document.getElementById('userAvatar');
        const conceptTag = document.getElementById('conceptTag');
        const conceptIcon = document.getElementById('conceptIcon');
        const conceptName = document.getElementById('conceptName');
        const conceptDefinition = document.getElementById('conceptDefinition');
        const conceptContent = document.getElementById('conceptContent');
        const prevConceptBtn = document.getElementById('prevConceptBtn');
        const nextConceptBtn = document.getElementById('nextConceptBtn');
        const startQuizBtn = document.getElementById('startQuizBtn');
        const progressBar = document.getElementById('progressBar');
        const questionText = document.getElementById('questionText');
        const optionsContainer = document.getElementById('optionsContainer');
        const hintBox = document.getElementById('hintBox');
        const hintContent = document.getElementById('hintContent');
        const hintBtn = document.getElementById('hintBtn');
        const submitBtn = document.getElementById('submitBtn');
        const nextBtn = document.getElementById('nextBtn');
        const resultsPanel = document.getElementById('resultsPanel');
        const resultsList = document.getElementById('resultsList');
        const restartBtn = document.getElementById('restartBtn');
        const backToLearningBtn = document.getElementById('backToLearningBtn');
        const downloadBtn = document.getElementById('downloadBtn');
        const quizProgressBar = document.getElementById('quizProgressBar');
        const levelTag = document.getElementById('levelTag');
        const scoreValue = document.getElementById('scoreValue');
        const conceptCount = document.getElementById('conceptCount');
        const correctCount = document.getElementById('correctCount');
        const coinCount = document.getElementById('coinCount');
        const coinSidebar = document.getElementById('coinSidebar');
        const celebration = document.getElementById('celebration');
        const badges = [
            document.getElementById('badge1'),
            document.getElementById('badge2'),
            document.getElementById('badge3'),
            document.getElementById('badge4')
        ];

        // Initialize the game
        function init() {
            startBtn.addEventListener('click', showProfileCard);
            saveProfileBtn.addEventListener('click', saveProfile);
            prevConceptBtn.addEventListener('click', prevConcept);
            nextConceptBtn.addEventListener('click', nextConcept);
            startQuizBtn.addEventListener('click', startQuiz);
            hintBtn.addEventListener('click', showHint);
            submitBtn.addEventListener('click', submitAnswer);
            nextBtn.addEventListener('click', nextQuestion);
            restartBtn.addEventListener('click', restartQuiz);
            backToLearningBtn.addEventListener('click', backToLearning);
            downloadBtn.addEventListener('click', downloadResults);
            
            // Randomize quiz data on initialization
            state.randomizedQuizData = randomizeQuizData();
            
            // Keyboard shortcuts
            document.addEventListener('keydown', handleKeyPress);
            
            updateStats();
        }

        // Show profile creation card
        function showProfileCard() {
            welcomeCard.style.display = 'none';
            profileCard.style.display = 'block';
        }

        // Save user profile
        function saveProfile() {
            const name = userNameInput.value.trim();
            if (name === "") {
                alert("Please enter your name");
                return;
            }
            
            state.userName = name;
            state.userInitials = getInitials(name);
            
            // Update UI with user info
            userDisplayName.textContent = name;
            userAvatar.textContent = state.userInitials;
            
            profileCard.style.display = 'none';
            learningCard.style.display = 'block';
            loadConcept();
        }

        // Get initials from name
        function getInitials(name) {
            return name.split(' ').map(word => word[0]).join('').toUpperCase().substring(0, 2);
        }

        // Load the current concept
        function loadConcept() {
            const concept = historyConcepts[state.currentConcept];
            
            // Update UI
            conceptTag.textContent = concept.name;
            conceptIcon.textContent = concept.icon;
            conceptName.textContent = concept.name;
            conceptDefinition.textContent = concept.definition;
            conceptContent.innerHTML = concept.content;
            progressBar.style.width = `${((state.currentConcept) / historyConcepts.length) * 100}%`;
            
            // Update buttons
            prevConceptBtn.disabled = state.currentConcept === 0;
            nextConceptBtn.disabled = state.currentConcept === historyConcepts.length - 1;
            
            updateStats();
        }

        // Go to previous concept
        function prevConcept() {
            if (state.currentConcept > 0) {
                state.currentConcept--;
                loadConcept();
            }
        }

        // Go to next concept
        function nextConcept() {
            if (state.currentConcept < historyConcepts.length - 1) {
                state.currentConcept++;
                loadConcept();
            }
        }

        // Start the quiz
        function startQuiz() {
            learningCard.style.display = 'none';
            quizCard.style.display = 'block';
            state.currentQuestion = 0;
            state.score = 0;
            state.quizCompleted = false;
            loadQuestion();
        }

        // Load the current question
        function loadQuestion() {
            const question = state.randomizedQuizData[state.currentQuestion];
            
            // Update UI
            questionText.textContent = question.question;
            levelTag.textContent = `Question ${state.currentQuestion + 1}`;
            quizProgressBar.style.width = `${((state.currentQuestion) / state.randomizedQuizData.length) * 100}%`;
            
            // Clear previous options and selection
            optionsContainer.innerHTML = '';
            state.selectedOption = null;
            hintBox.style.display = 'none';
            nextBtn.style.display = 'none';
            submitBtn.style.display = 'block';
            state.hintUsed = false;
            
            // Update hint button state based on coins
            hintBtn.disabled = state.coins < 1;
            
            // Create options
            question.options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.className = 'option';
                optionElement.innerHTML = `
                    <div class="option-label">${String.fromCharCode(65 + index)}</div>
                    <div class="option-text">${option}</div>
                `;
                
                optionElement.addEventListener('click', () => selectOption(index, optionElement));
                optionsContainer.appendChild(optionElement);
            });
            
            updateStats();
        }

        // Select an option
        function selectOption(index, element) {
            // Clear previous selection
            document.querySelectorAll('.option').forEach(opt => {
                opt.style.borderColor = 'transparent';
                opt.style.background = '';
            });
            
            // Highlight selected option
            element.style.borderColor = '#DEB887';
            element.style.background = '#e8f4fd';
            state.selectedOption = index;
        }

        // Show hint for current question
        function showHint() {
            if (state.coins < 1) {
                alert("You don't have enough coins to buy a hint!");
                return;
            }
            
            const question = state.randomizedQuizData[state.currentQuestion];
            hintContent.textContent = question.hint;
            hintBox.style.display = 'block';
            
            // Deduct coin
            state.coins -= 1;
            state.hintUsed = true;
            updateCoinDisplay();
            
            // Disable hint button for this question
            hintBtn.disabled = true;
        }

        // Submit answer
        function submitAnswer() {
            if (state.selectedOption === null) {
                alert('Please select an answer before submitting.');
                return;
            }
            
            const question = state.randomizedQuizData[state.currentQuestion];
            const isCorrect = state.selectedOption === question.correct;
            
            // Update score and coins
            if (isCorrect) {
                state.score += 5;
                state.coins += 5;
                showCoinAnimation();
            }
            
            // Show correct/incorrect feedback
            document.querySelectorAll('.option').forEach((opt, index) => {
                if (index === question.correct) {
                    opt.style.borderColor = '#32CD32';
                    opt.style.background = '#e8f5e9';
                } else if (index === state.selectedOption && !isCorrect) {
                    opt.style.borderColor = '#FF9800';
                    opt.style.background = '#ffebee';
                }
                
                // Disable further selection
                opt.style.pointerEvents = 'none';
            });
            
            // Show explanation
            let explanation = `The correct answer is: "${question.options[question.correct]}".`;
            if (isCorrect) {
                explanation = `Correct! "${question.options[question.correct]}" is the right answer.`;
            }
            
            hintContent.textContent = explanation;
            hintBox.style.display = 'block';
            
            // Update UI
            submitBtn.style.display = 'none';
            nextBtn.style.display = 'block';
            
            updateStats();
            
            // Check if quiz is complete
            if (state.currentQuestion === state.randomizedQuizData.length - 1) {
                nextBtn.textContent = 'See Results';
            }
        }

        // Move to next question or show results
        function nextQuestion() {
            if (state.currentQuestion < state.randomizedQuizData.length - 1) {
                state.currentQuestion++;
                loadQuestion();
            } else {
                showResults();
            }
        }

        // Show quiz results
        function showResults() {
            // Hide question elements
            document.querySelector('.question-container').style.display = 'none';
            document.querySelector('.controls').style.display = 'none';
            document.querySelector('.keyboard-hint').style.display = 'none';
            
            // Show results
            resultsPanel.style.display = 'block';
            
            // Calculate results
            const percentage = Math.round((state.score / (state.randomizedQuizData.length * 5)) * 100);
            let message = '';
            
            if (percentage >= 90) {
                message = `Outstanding ${state.userName}! You're a history expert!`;
                triggerCelebration();
            } else if (percentage >= 80) {
                message = `Excellent ${state.userName}! You have impressive historical knowledge!`;
            } else if (percentage >= 70) {
                message = `Good job ${state.userName}! You have a solid understanding of history.`;
            } else if (percentage >= 60) {
                message = `Not bad ${state.userName}! Keep learning about history.`;
            } else {
                message = `Keep studying ${state.userName}! Review the topics and try again.`;
            }
            
            // Display results
            resultsList.innerHTML = `
                <div style="text-align: center; margin-bottom: 25px;">
                    <h3>${message}</h3>
                    <div style="font-size: 2.5rem; font-weight: bold; color: #8B4513; margin: 15px 0;">${percentage}%</div>
                    <div style="font-size: 1.2rem; margin: 10px 0;">Score: ${state.score} out of ${state.randomizedQuizData.length * 5}</div>
                    <div style="font-size: 1.2rem; margin: 10px 0;">Coins earned: ${state.coins}</div>
                </div>
            `;
            
            // Update badges based on performance
            updateBadges(percentage);
        }

        // Update badges based on performance
        function updateBadges(percentage) {
            if (percentage >= 25) {
                badges[0].classList.add('earned');
            }
            if (percentage >= 50) {
                badges[1].classList.add('earned');
            }
            if (percentage >= 75) {
                badges[2].classList.add('earned');
            }
            if (percentage >= 90) {
                badges[3].classList.add('earned');
            }
        }

        // Restart the quiz
        function restartQuiz() {
            // Reset state
            state.currentQuestion = 0;
            state.score = 0;
            state.selectedOption = null;
            
            // Re-randomize quiz data
            state.randomizedQuizData = randomizeQuizData();
            
            // Reset UI
            document.querySelector('.question-container').style.display = 'block';
            document.querySelector('.controls').style.display = 'flex';
            document.querySelector('.keyboard-hint').style.display = 'block';
            resultsPanel.style.display = 'none';
            nextBtn.textContent = 'Next Question';
            
            // Reload first question
            loadQuestion();
        }

        // Go back to learning
        function backToLearning() {
            quizCard.style.display = 'none';
            learningCard.style.display = 'block';
        }

        // Update stats in sidebar
        function updateStats() {
            scoreValue.textContent = state.score;
            conceptCount.textContent = `${state.currentConcept + 1}/${historyConcepts.length}`;
            correctCount.textContent = Math.floor(state.score / 5);
            updateCoinDisplay();
        }

        // Update coin display
        function updateCoinDisplay() {
            coinCount.textContent = state.coins;
            coinSidebar.textContent = state.coins;
        }

        // Show coin animation when earning coins
        function showCoinAnimation() {
            const coin = document.createElement('div');
            coin.className = 'coin-animation';
            coin.innerHTML = '🪙';
            coin.style.position = 'fixed';
            coin.style.left = '50%';
            coin.style.top = '50%';
            
            // Calculate random end position near coin display
            const endX = Math.random() * 100 + 80;
            const endY = Math.random() * 50 + 10;
            
            coin.style.setProperty('--tx', `${endX}px`);
            coin.style.setProperty('--ty', `-${endY}px`);
            
            document.body.appendChild(coin);
            
            // Remove coin after animation
            setTimeout(() => {
                coin.remove();
            }, 1000);
        }

        // Handle keyboard shortcuts
        function handleKeyPress(e) {
            if (welcomeCard.style.display !== 'none' || profileCard.style.display !== 'none') return;
            
            // Number keys 1-4 to select options
            if (e.key >= '1' && e.key <= '4') {
                const index = parseInt(e.key) - 1;
                const options = document.querySelectorAll('.option');
                if (options[index]) {
                    selectOption(index, options[index]);
                }
            }
            
            // H key for hint
            if (e.key === 'h' || e.key === 'H') {
                showHint();
            }
            
            // Enter key to submit
            if (e.key === 'Enter') {
                if (submitBtn.style.display !== 'none') {
                    submitAnswer();
                } else if (nextBtn.style.display !== 'none') {
                    nextQuestion();
                }
            }
        }

        // Celebration animation for excellent score
        function triggerCelebration() {
            celebration.style.display = 'block';
            
            // Create confetti
            for (let i = 0; i < 100; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * 100 + 'vw';
                confetti.style.background = getRandomColor();
                confetti.style.transform = `rotate(${Math.random() * 360}deg)`;
                celebration.appendChild(confetti);
                
                // Animate confetti
                confetti.animate([
                    { top: '-10px', opacity: 1 },
                    { top: '100vh', opacity: 0 }
                ], {
                    duration: 1000 + Math.random() * 2000,
                    delay: Math.random() * 1000
                });
            }
            
            // Remove celebration after 3 seconds
            setTimeout(() => {
                celebration.style.display = 'none';
                celebration.innerHTML = '';
            }, 3000);
        }

        // Helper function to get random color
        function getRandomColor() {
            const colors = ['#8B4513', '#DEB887', '#A52A2A', '#32CD32', '#FFD700'];
            return colors[Math.floor(Math.random() * colors.length)];
        }

        // Download results as PDF
        function downloadResults() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            
            // Add title
            doc.setFontSize(20);
            doc.setTextColor(139, 69, 19);
            doc.text('Pensmith Education', 20, 20);
            doc.setFontSize(16);
            doc.text('History Learning Module - Results', 20, 30);
            
            // Add student info
            doc.setFontSize(12);
            doc.setTextColor(0, 0, 0);
            doc.text(`Student: ${state.userName}`, 20, 50);
            doc.text(`Date: ${new Date().toLocaleDateString()}`, 20, 60);
            
            // Add score
            const percentage = Math.round((state.score / (state.randomizedQuizData.length * 5)) * 100);
            doc.setFontSize(16);
            doc.setTextColor(139, 69, 19);
            doc.text(`Score: ${state.score}/${state.randomizedQuizData.length * 5} (${percentage}%)`, 20, 80);
            doc.text(`Coins: ${state.coins}`, 20, 95);
            
            // Add performance message
            let message = '';
            if (percentage >= 90) {
                message = 'Outstanding! You\'re a history expert!';
            } else if (percentage >= 80) {
                message = 'Excellent! You have impressive historical knowledge!';
            } else if (percentage >= 70) {
                message = 'Good job! You have a solid understanding of history.';
            } else if (percentage >= 60) {
                message = 'Not bad! Keep learning about history.';
            } else {
                message = 'Keep studying! Review the topics and try again.';
            }
            
            doc.text(message, 20, 115);
            
            // Add topics covered
            doc.setFontSize(14);
            doc.text('Topics Covered:', 20, 140);
            doc.setFontSize(12);
            
            let yPosition = 155;
            historyConcepts.forEach((concept, index) => {
                if (yPosition > 270) {
                    doc.addPage();
                    yPosition = 20;
                }
                doc.text(`✓ ${concept.name}`, 25, yPosition);
                yPosition += 10;
            });
            
            // Add footer
            doc.setFontSize(10);
            doc.setTextColor(100, 100, 100);
            doc.text('Created by Ador Michael Unim', 20, 280);
            
            // Save the PDF
            doc.save(`${state.userName}_Pensmith_History_Results.pdf`);
        }

        // Initialize the game when page loads
        window.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>