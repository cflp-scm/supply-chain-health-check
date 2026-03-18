<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>供应链健康度60秒自测</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }
        
        .container {
            max-width: 480px;
            margin: 0 auto;
            padding: 20px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }
        
        /* 首页 */
        .welcome-screen {
            text-align: center;
            padding: 40px 20px;
            animation: fadeIn 0.8s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .logo {
            width: 80px;
            height: 80px;
            background: white;
            border-radius: 20px;
            margin: 0 auto 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        
        h1 {
            color: white;
            font-size: 28px;
            margin-bottom: 15px;
            font-weight: 700;
        }
        
        .subtitle {
            color: rgba(255,255,255,0.9);
            font-size: 16px;
            margin-bottom: 40px;
            line-height: 1.6;
        }
        
        .features {
            background: rgba(255,255,255,0.15);
            backdrop-filter: blur(10px);
            border-radius: 16px;
            padding: 25px;
            margin-bottom: 40px;
            text-align: left;
        }
        
        .feature-item {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
            color: white;
            font-size: 15px;
        }
        
        .feature-item:last-child {
            margin-bottom: 0;
        }
        
        .feature-icon {
            width: 24px;
            height: 24px;
            background: rgba(255,255,255,0.3);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 12px;
            font-size: 14px;
        }
        
        .start-btn {
            background: white;
            color: #667eea;
            border: none;
            padding: 18px 60px;
            font-size: 18px;
            font-weight: 700;
            border-radius: 30px;
            cursor: pointer;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            transition: all 0.3s ease;
        }
        
        .start-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 15px 40px rgba(0,0,0,0.3);
        }
        
        .start-btn:active {
            transform: scale(0.98);
        }
        
        /* 答题页 */
        .quiz-screen {
            display: none;
            background: white;
            border-radius: 20px;
            padding: 30px 25px;
            margin: 20px 0;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            animation: slideUp 0.5s ease;
        }
        
        @keyframes slideUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .progress-bar {
            height: 6px;
            background: #f0f0f0;
            border-radius: 3px;
            margin-bottom: 30px;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea, #764ba2);
            border-radius: 3px;
            transition: width 0.4s ease;
        }
        
        .question-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
        }
        
        .question-num {
            color: #667eea;
            font-weight: 700;
            font-size: 14px;
        }
        
        .dimension-tag {
            background: linear-gradient(135deg, #667eea20, #764ba220);
            color: #667eea;
            padding: 6px 14px;
            border-radius: 20px;
            font-size: 13px;
            font-weight: 600;
        }
        
        .question-text {
            font-size: 20px;
            font-weight: 600;
            line-height: 1.6;
            margin-bottom: 30px;
            color: #222;
        }
        
        .options {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        
        .option-btn {
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            padding: 18px 20px;
            border-radius: 12px;
            cursor: pointer;
            text-align: left;
            font-size: 16px;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
        }
        
        .option-btn:hover {
            border-color: #667eea;
            background: #f0f4ff;
        }
        
        .option-btn.selected {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border-color: transparent;
        }
        
        .option-label {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            background: rgba(102,126,234,0.1);
            color: #667eea;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 700;
            margin-right: 12px;
            font-size: 14px;
        }
        
        .option-btn.selected .option-label {
            background: rgba(255,255,255,0.3);
            color: white;
        }
        
        .nav-btns {
            display: flex;
            justify-content: space-between;
            margin-top: 30px;
        }
        
        .nav-btn {
            padding: 12px 24px;
            border-radius: 25px;
            border: none;
            cursor: pointer;
            font-size: 15px;
            transition: all 0.3s ease;
        }
        
        .prev-btn {
            background: #f0f0f0;
            color: #666;
        }
        
        .next-btn, .submit-btn {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            font-weight: 600;
        }
        
        .nav-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        /* 结果页 */
        .result-screen {
            display: none;
            animation: fadeIn 0.6s ease;
        }
        
        .result-card {
            background: white;
            border-radius: 20px;
            padding: 30px 25px;
            margin-bottom: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
        }
        
        .score-section {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .score-circle {
            width: 140px;
            height: 140px;
            border-radius: 50%;
            margin: 0 auto 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
            background: conic-gradient(#667eea var(--score-deg), #f0f0f0 var(--score-deg));
        }
        
        .score-inner {
            width: 120px;
            height: 120px;
            background: white;
            border-radius: 50%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        
        .score-num {
            font-size: 48px;
            font-weight: 800;
            color: #333;
            line-height: 1;
        }
        
        .score-total {
            font-size: 14px;
            color: #999;
            margin-top: 4px;
        }
        
        .score-tag {
            display: inline-block;
            padding: 8px 20px;
            border-radius: 20px;
            font-size: 16px;
            font-weight: 700;
            margin-top: 15px;
        }
        
        .tag-healthy {
            background: #d4edda;
            color: #155724;
        }
        
        .tag-subhealthy {
            background: #fff3cd;
            color: #856404;
        }
        
        .tag-risk {
            background: #f8d7da;
            color: #721c24;
        }
        
        .chart-container {
            margin: 30px 0;
            height: 280px;
            position: relative;
        }
        
        .dimension-scores {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin-top: 20px;
        }
        
        .dim-item {
            background: #f8f9fa;
            padding: 12px;
            border-radius: 10px;
            text-align: center;
        }
        
        .dim-name {
            font-size: 13px;
            color: #666;
            margin-bottom: 5px;
        }
        
        .dim-score {
            font-size: 18px;
            font-weight: 700;
            color: #667eea;
        }
        
        .recommendations {
            margin-top: 30px;
        }
        
        .rec-title {
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 20px;
            color: #333;
        }
        
        .rec-item {
            background: linear-gradient(135deg, #f8f9fa, #e9ecef);
            border-left: 4px solid #667eea;
            padding: 18px;
            border-radius: 0 12px 12px 0;
            margin-bottom: 15px;
        }
        
        .rec-priority {
            display: inline-block;
            background: #667eea;
            color: white;
            padding: 4px 10px;
            border-radius: 4px;
            font-size: 12px;
            font-weight: 600;
            margin-bottom: 8px;
        }
        
        .rec-text {
            font-size: 15px;
            line-height: 1.6;
            color: #333;
        }
        
        .rec-impact {
            font-size: 13px;
            color: #28a745;
            margin-top: 8px;
            font-weight: 600;
        }
        
        .action-btns {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-top: 30px;
        }
        
        .action-btn {
            padding: 16px;
            border-radius: 12px;
            border: none;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        
        .btn-primary {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
        }
        
        .btn-secondary {
            background: white;
            color: #667eea;
            border: 2px solid #667eea;
        }
        
        .action-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(102,126,234,0.3);
        }
        
        /* 海报生成 */
        .poster-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            z-index: 1000;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            padding: 20px;
        }
        
        .poster-content {
            background: white;
            width: 100%;
            max-width: 375px;
            border-radius: 16px;
            overflow: hidden;
            position: relative;
        }
        
        .poster-header {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 30px;
            text-align: center;
        }
        
        .poster-body {
            padding: 30px;
        }
        
        .poster-score {
            font-size: 56px;
            font-weight: 800;
            text-align: center;
            margin: 20px 0;
        }
        
        .poster-chart {
            height: 200px;
            margin: 20px 0;
        }
        
        .poster-footer {
            background: #f8f9fa;
            padding: 20px;
            text-align: center;
        }
        
        .qr-placeholder {
            width: 80px;
            height: 80px;
            background: #ddd;
            margin: 0 auto 10px;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #999;
            font-size: 12px;
        }
        
        .close-poster {
            position: absolute;
            top: 15px;
            right: 15px;
            width: 36px;
            height: 36px;
            background: rgba(255,255,255,0.2);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 20px;
            cursor: pointer;
        }
        
        .hidden-poster {
            position: absolute;
            left: -9999px;
            top: 0;
        }
        
        /* 加载动画 */
        .loading {
            display: none;
            text-align: center;
            padding: 40px;
            color: white;
        }
        
        .spinner {
            width: 50px;
            height: 50px;
            border: 4px solid rgba(255,255,255,0.3);
            border-top-color: white;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 20px;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        /* 响应式 */
        @media (max-width: 480px) {
            .container {
                padding: 15px;
            }
            
            h1 {
                font-size: 24px;
            }
            
            .question-text {
                font-size: 18px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 欢迎页 -->
        <div class="welcome-screen" id="welcomeScreen">
            <div class="logo">📊</div>
            <h1>供应链健康度60秒自测</h1>
            <p class="subtitle">基于Gartner成熟度模型与SCALE框架<br>快速定位你的供应链优先级改进项</p>
            
            <div class="features">
                <div class="feature-item">
                    <div class="feature-icon">⚡</div>
                    <span>10道精选题目，60秒完成评估</span>
                </div>
                <div class="feature-item">
                    <div class="feature-icon">📈</div>
                    <span>6维能力雷达图，可视化短板</span>
                </div>
                <div class="feature-item">
                    <div class="feature-icon">🎯</div>
                    <span>生成专属改进清单，立即行动</span>
                </div>
            </div>
            
            <button class="start-btn" onclick="startQuiz()">开始诊断</button>
        </div>
        
        <!-- 答题页 -->
        <div class="quiz-screen" id="quizScreen">
            <div class="progress-bar">
                <div class="progress-fill" id="progressFill" style="width: 0%"></div>
            </div>
            
            <div class="question-header">
                <span class="question-num" id="questionNum">第 1/10 题</span>
                <span class="dimension-tag" id="dimensionTag">可视化</span>
            </div>
            
            <div class="question-text" id="questionText">加载中...</div>
            
            <div class="options" id="optionsContainer">
                <!-- 动态生成 -->
            </div>
            
            <div class="nav-btns">
                <button class="nav-btn prev-btn" id="prevBtn" onclick="prevQuestion()" disabled>上一题</button>
                <button class="nav-btn next-btn" id="nextBtn" onclick="nextQuestion()" disabled>下一题</button>
            </div>
        </div>
        
        <!-- 结果页 -->
        <div class="result-screen" id="resultScreen">
            <div class="result-card">
                <div class="score-section">
                    <div class="score-circle" id="scoreCircle" style="--score-deg: 0deg">
                        <div class="score-inner">
                            <div class="score-num" id="scoreNum">0</div>
                            <div class="score-total">/ 50</div>
                        </div>
                    </div>
                    <div class="score-tag" id="scoreTag">评估中...</div>
                </div>
                
                <div class="chart-container">
                    <canvas id="radarChart"></canvas>
                </div>
                
                <div class="dimension-scores" id="dimensionScores">
                    <!-- 动态生成 -->
                </div>
            </div>
            
            <div class="result-card">
                <div class="recommendations">
                    <div class="rec-title">🎯 优先改进建议</div>
                    <div id="recommendationsList">
                        <!-- 动态生成 -->
                    </div>
                </div>
            </div>
            
            <div class="action-btns">
                <button class="action-btn btn-primary" onclick="generatePoster()">
                    <span>📸</span> 生成诊断海报
                </button>
                <button class="action-btn btn-secondary" onclick="restartQuiz()">
                    <span>🔄</span> 重新测试
                </button>
            </div>
        </div>
        
        <!-- 海报模态框 -->
        <div class="poster-modal" id="posterModal">
            <div class="poster-content" id="posterContent">
                <div class="poster-header">
                    <h2>供应链健康度诊断报告</h2>
                    <p style="opacity: 0.9; margin-top: 8px;">中国物流与采购联合会采购委</p>
                </div>
                <div class="poster-body">
                    <div style="text-align: center; margin-bottom: 20px;">
                        <div style="font-size: 14px; color: #666;">综合得分</div>
                        <div class="poster-score" id="posterScore">32</div>
                        <div id="posterTag" style="display: inline-block; padding: 6px 16px; border-radius: 20px; font-size: 14px; font-weight: 600;">亚健康</div>
                    </div>
                    <div class="poster-chart">
                        <canvas id="posterChart"></canvas>
                    </div>
                    <div style="background: #f8f9fa; padding: 15px; border-radius: 10px; margin-top: 15px;">
                        <div style="font-size: 13px; color: #666; margin-bottom: 8px;">TOP 2 改进优先级</div>
                        <div id="posterRecs" style="font-size: 14px; line-height: 1.6;"></div>
                    </div>
                </div>
                <div class="poster-footer">
                    <div class="qr-placeholder">二维码</div>
                    <div style="font-size: 13px; color: #666;">扫码测测你的供应链健康度</div>
                </div>
            </div>
            <div class="close-poster" onclick="closePoster()">×</div>
            <button class="action-btn btn-primary" style="margin-top: 20px; max-width: 375px;" onclick="downloadPoster()">
                保存海报到相册
            </button>
        </div>
        
        <!-- 加载中 -->
        <div class="loading" id="loading">
            <div class="spinner"></div>
            <p>正在生成你的专属报告...</p>
        </div>
    </div>

    <script>
        // 题目数据
        const questions = [
            {
                id: 1,
                dimension: '可视化',
                dimensionKey: 'visibility',
                text: '当客户问"货在哪里"，你需要多久能给出准确位置？',
                options: [
                    { label: 'A', text: '实时可见', score: 5 },
                    { label: 'B', text: '2小时内', score: 3 },
                    { label: 'C', text: '半天以上', score: 1 }
                ],
                weight: 0.10
            },
            {
                id: 2,
                dimension: '计划协同',
                dimensionKey: 'planning',
                text: '销售部门做预测时，采购/生产部门是否参与？',
                options: [
                    { label: 'A', text: '每月协同会议', score: 5 },
                    { label: 'B', text: '季度碰头', score: 3 },
                    { label: 'C', text: '基本不沟通', score: 1 }
                ],
                weight: 0.15
            },
            {
                id: 3,
                dimension: '库存健康',
                dimensionKey: 'inventory',
                text: '过去一年，呆滞库存占总库存比例？',
                options: [
                    { label: 'A', text: '<5%', score: 5 },
                    { label: 'B', text: '5-15%', score: 3 },
                    { label: 'C', text: '>15%', score: 1 }
                ],
                weight: 0.15
            },
            {
                id: 4,
                dimension: '供应商韧性',
                dimensionKey: 'resilience',
                text: '核心物料的合格备用供应商有几个？',
                options: [
                    { label: 'A', text: '≥2家', score: 5 },
                    { label: 'B', text: '1家', score: 3 },
                    { label: 'C', text: '没有备用', score: 1 }
                ],
                weight: 0.15
            },
            {
                id: 5,
                dimension: '响应速度',
                dimensionKey: 'responsiveness',
                text: '需求突然增加20%，供应链多久能调整到位？',
                options: [
                    { label: 'A', text: '1周内', score: 5 },
                    { label: 'B', text: '2-4周', score: 3 },
                    { label: 'C', text: '1个月以上', score: 1 }
                ],
                weight: 0.10
            },
            {
                id: 6,
                dimension: '数据质量',
                dimensionKey: 'data',
                text: '系统里的库存数据，与实物盘点差异率？',
                options: [
                    { label: 'A', text: '<2%', score: 5 },
                    { label: 'B', text: '2-5%', score: 3 },
                    { label: 'C', text: '>5%', score: 1 }
                ],
                weight: 0.10
            },
            {
                id: 7,
                dimension: '成本透明',
                dimensionKey: 'cost',
                text: '能否准确说出"端到端供应链总成本"？',
                options: [
                    { label: 'A', text: '精确到SKU', score: 5 },
                    { label: 'B', text: '大致估算', score: 3 },
                    { label: 'C', text: '不清楚', score: 1 }
                ],
                weight: 0.10
            },
            {
                id: 8,
                dimension: '风险预警',
                dimensionKey: 'risk',
                text: '供应商延迟交货，你提前多久知道？',
                options: [
                    { label: 'A', text: '事前预警', score: 5 },
                    { label: 'B', text: '到期当天知道', score: 3 },
                    { label: 'C', text: '客户投诉后才知道', score: 1 }
                ],
                weight: 0.10
            },
            {
                id: 9,
                dimension: '可持续性',
                dimensionKey: 'sustainability',
                text: '是否定期评估供应商ESG风险？',
                options: [
                    { label: 'A', text: '每年评估', score: 5 },
                    { label: 'B', text: '偶尔关注', score: 3 },
                    { label: 'C', text: '从未考虑', score: 1 }
                ],
                weight: 0.05
            },
            {
                id: 10,
                dimension: '数字化',
                dimensionKey: 'digital',
                text: '供应链关键决策，多大程度依赖数据而非经验？',
                options: [
                    { label: 'A', text: '数据驱动', score: 5 },
                    { label: 'B', text: '数据+经验', score: 3 },
                    { label: 'C', text: '主要靠经验', score: 1 }
                ],
                weight: 0.10
            }
        ];

        // 改进建议库
        const recommendations = {
            planning: {
                title: '建立S&OP销售与运营协同机制',
                impact: '预计释放库存资金15-20%',
                priority: 1
            },
            inventory: {
                title: '实施ABC分类+呆滞库存预警',
                impact: '降低库存持有成本25%',
                priority: 1
            },
            resilience: {
                title: '核心物料引入备用供应商',
                impact: '降低断供风险60%',
                priority: 1
            },
            visibility: {
                title: '部署供应链控制塔或可视化平台',
                impact: '缩短异常响应时间70%',
                priority: 2
            },
            data: {
                title: '建立主数据管理（MDM）规范',
                impact: '提升决策准确率30%',
                priority: 2
            },
            responsiveness: {
                title: '建立柔性产能与快速切换机制',
                impact: '提升需求满足率至95%',
                priority: 2
            },
            cost: {
                title: '实施端到端成本核算（TCO）',
                impact: '识别隐性成本10-15%',
                priority: 3
            },
            risk: {
                title: '建立供应商风险预警系统',
                impact: '提前发现80%交付风险',
                priority: 2
            },
            digital: {
                title: '推进需求预测AI模型应用',
                impact: '预测准确率提升至85%',
                priority: 3
            },
            sustainability: {
                title: '建立供应商ESG评估体系',
                impact: '降低合规与品牌风险',
                priority: 3
            }
        };

        // 状态管理
        let currentQuestion = 0;
        let answers = {};
        let radarChart = null;
        let posterChart = null;

        // 开始测试
        function startQuiz() {
            document.getElementById('welcomeScreen').style.display = 'none';
            document.getElementById('quizScreen').style.display = 'block';
            renderQuestion();
        }

        // 渲染题目
        function renderQuestion() {
            const q = questions[currentQuestion];
            const progress = ((currentQuestion + 1) / questions.length) * 100;
            
            document.getElementById('progressFill').style.width = progress + '%';
            document.getElementById('questionNum').textContent = `第 ${currentQuestion + 1}/${questions.length} 题`;
            document.getElementById('dimensionTag').textContent = q.dimension;
            document.getElementById('questionText').textContent = q.text;
            
            const optionsHtml = q.options.map((opt, idx) => `
                <button class="option-btn ${answers[q.id] === opt.score ? 'selected' : ''}" 
                        onclick="selectOption(${q.id}, ${opt.score}, this)">
                    <span class="option-label">${opt.label}</span>
                    <span>${opt.text}</span>
                </button>
            `).join('');
            
            document.getElementById('optionsContainer').innerHTML = optionsHtml;
            
            // 更新按钮状态
            document.getElementById('prevBtn').disabled = currentQuestion === 0;
            const hasAnswer = answers[q.id] !== undefined;
            document.getElementById('nextBtn').disabled = !hasAnswer;
            document.getElementById('nextBtn').textContent = 
                currentQuestion === questions.length - 1 ? '查看结果' : '下一题';
            document.getElementById('nextBtn').className = 
                currentQuestion === questions.length - 1 ? 'nav-btn submit-btn' : 'nav-btn next-btn';
        }

        // 选择选项
        function selectOption(questionId, score, btnElement) {
            answers[questionId] = score;
            
            // 更新UI
            document.querySelectorAll('.option-btn').forEach(btn => btn.classList.remove('selected'));
            btnElement.classList.add('selected');
            
            // 启用下一题按钮
            document.getElementById('nextBtn').disabled = false;
            
            // 自动下一题（可选，延迟800ms）
            if (currentQuestion < questions.length - 1) {
                setTimeout(() => nextQuestion(), 800);
            }
        }

        // 下一题
        function nextQuestion() {
            if (currentQuestion < questions.length - 1) {
                currentQuestion++;
                renderQuestion();
            } else {
                showResult();
            }
        }

        // 上一题
        function prevQuestion() {
            if (currentQuestion > 0) {
                currentQuestion--;
                renderQuestion();
            }
        }

        // 计算分数
        function calculateScores() {
            let totalScore = 0;
            const dimensionScores = {};
            
            // 初始化维度分数
            const dimensions = ['visibility', 'planning', 'inventory', 'resilience', 'responsiveness', 'data', 'cost', 'risk', 'sustainability', 'digital'];
            dimensions.forEach(d => dimensionScores[d] = { sum: 0, count: 0, max: 0 });
            
            // 计算总分和维度分
            questions.forEach(q => {
                const score = answers[q.id] || 0;
                totalScore += score;
                
                if (dimensionScores[q.dimensionKey]) {
                    dimensionScores[q.dimensionKey].sum += score;
                    dimensionScores[q.dimensionKey].count += 1;
                    dimensionScores[q.dimensionKey].max += 5;
                }
            });
            
            // 归一化维度分数（0-100）
            const normalizedDimensions = {};
            for (let key in dimensionScores) {
                const d = dimensionScores[key];
                normalizedDimensions[key] = d.max > 0 ? Math.round((d.sum / d.max) * 100) : 0;
            }
            
            return { totalScore, dimensionScores: normalizedDimensions };
        }

        // 显示结果
        function showResult() {
            document.getElementById('quizScreen').style.display = 'none';
            document.getElementById('loading').style.display = 'block';
            
            setTimeout(() => {
                document.getElementById('loading').style.display = 'none';
                document.getElementById('resultScreen').style.display = 'block';
                
                const { totalScore, dimensionScores } = calculateScores();
                
                // 更新总分
                document.getElementById('scoreNum').textContent = totalScore;
                const deg = (totalScore / 50) * 360;
                document.getElementById('scoreCircle').style.setProperty('--score-deg', deg + 'deg');
                
                // 评级标签
                const tagEl = document.getElementById('scoreTag');
                if (totalScore >= 40) {
                    tagEl.textContent = '健康';
                    tagEl.className = 'score-tag tag-healthy';
                } else if (totalScore >= 25) {
                    tagEl.textContent = '亚健康';
                    tagEl.className = 'score-tag tag-subhealthy';
                } else {
                    tagEl.textContent = '风险';
                    tagEl.className = 'score-tag tag-risk';
                }
                
                // 渲染雷达图
                renderRadarChart(dimensionScores);
                
                // 渲染维度分数
                renderDimensionScores(dimensionScores);
                
                // 生成建议
                generateRecommendations(dimensionScores);
                
            }, 1500);
        }

        // 渲染雷达图
        function renderRadarChart(dimensionScores) {
            const ctx = document.getElementById('radarChart').getContext('2d');
            
            const labels = ['可视化', '计划协同', '库存健康', '供应商韧性', '响应速度', '数据质量'];
            const data = [
                dimensionScores.visibility,
                dimensionScores.planning,
                dimensionScores.inventory,
                dimensionScores.resilience,
                dimensionScores.responsiveness,
                dimensionScores.data
            ];
            
            if (radarChart) {
                radarChart.destroy();
            }
            
            radarChart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: labels,
                    datasets: [{
                        label: '你的供应链健康度',
                        data: data,
                        backgroundColor: 'rgba(102, 126, 234, 0.2)',
                        borderColor: '#667eea',
                        borderWidth: 2,
                        pointBackgroundColor: '#667eea',
                        pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff',
                        pointHoverBorderColor: '#667eea'
                    }, {
                        label: '行业平均',
                        data: [60, 55, 50, 45, 58, 52],
                        backgroundColor: 'rgba(200, 200, 200, 0.1)',
                        borderColor: '#ccc',
                        borderWidth: 1,
                        borderDash: [5, 5],
                        pointRadius: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        r: {
                            beginAtZero: true,
                            max: 100,
                            ticks: {
                                stepSize: 20,
                                display: false
                            },
                            grid: {
                                color: 'rgba(0,0,0,0.1)'
                            },
                            angleLines: {
                                color: 'rgba(0,0,0,0.1)'
                            },
                            pointLabels: {
                                font: {
                                    size: 12
                                },
                                color: '#666'
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                padding: 20,
                                font: {
                                    size: 12
                                }
                            }
                        }
                    }
                }
            });
        }

        // 渲染维度分数
        function renderDimensionScores(dimensionScores) {
            const dimensionNames = {
                visibility: '可视化',
                planning: '计划协同',
                inventory: '库存健康',
                resilience: '供应商韧性',
                responsiveness: '响应速度',
                data: '数据质量'
            };
            
            const html = Object.entries(dimensionNames).map(([key, name]) => `
                <div class="dim-item">
                    <div class="dim-name">${name}</div>
                    <div class="dim-score">${dimensionScores[key]}分</div>
                </div>
            `).join('');
            
            document.getElementById('dimensionScores').innerHTML = html;
        }

        // 生成改进建议
        function generateRecommendations(dimensionScores) {
            // 找出得分最低的3个维度
            const sorted = Object.entries(dimensionScores)
                .filter(([key]) => recommendations[key])
                .sort((a, b) => a[1] - b[1])
                .slice(0, 2);
            
            const html = sorted.map(([key, score], idx) => {
                const rec = recommendations[key];
                return `
                    <div class="rec-item">
                        <span class="rec-priority">优先级 ${idx + 1}</span>
                        <div class="rec-text">${rec.title}</div>
                        <div class="rec-impact">💡 ${rec.impact}</div>
                    </div>
                `;
            }).join('');
            
            document.getElementById('recommendationsList').innerHTML = html;
        }

        // 生成海报
        function generatePoster() {
            const modal = document.getElementById('posterModal');
            modal.style.display = 'flex';
            
            const { totalScore, dimensionScores } = calculateScores();
            
            // 更新海报数据
            document.getElementById('posterScore').textContent = totalScore;
            const tagEl = document.getElementById('posterTag');
            if (totalScore >= 40) {
                tagEl.textContent = '健康';
                tagEl.style.background = '#d4edda';
                tagEl.style.color = '#155724';
            } else if (totalScore >= 25) {
                tagEl.textContent = '亚健康';
                tagEl.style.background = '#fff3cd';
                tagEl.style.color = '#856404';
            } else {
                tagEl.textContent = '风险';
                tagEl.style.background = '#f8d7da';
                tagEl.style.color = '#721c24';
            }
            
            // 找出前2个改进项
            const sorted = Object.entries(dimensionScores)
                .filter(([key]) => recommendations[key])
                .sort((a, b) => a[1] - b[1])
                .slice(0, 2);
            
            const recsText = sorted.map(([key], idx) => 
                `${idx + 1}. ${recommendations[key].title}`
            ).join('<br>');
            document.getElementById('posterRecs').innerHTML = recsText;
            
            // 渲染海报雷达图（简化版）
            setTimeout(() => {
                const ctx = document.getElementById('posterChart').getContext('2d');
                if (posterChart) posterChart.destroy();
                
                posterChart = new Chart(ctx, {
                    type: 'radar',
                    data: {
                        labels: ['可视化', '计划协同', '库存健康', '供应商韧性', '响应速度', '数据质量'],
                        datasets: [{
                            data: [
                                dimensionScores.visibility,
                                dimensionScores.planning,
                                dimensionScores.inventory,
                                dimensionScores.resilience,
                                dimensionScores.responsiveness,
                                dimensionScores.data
                            ],
                            backgroundColor: 'rgba(102, 126, 234, 0.2)',
                            borderColor: '#667eea',
                            borderWidth: 2,
                            pointRadius: 0
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            r: {
                                beginAtZero: true,
                                max: 100,
                                ticks: { display: false },
                                grid: { color: 'rgba(0,0,0,0.05)' },
                                angleLines: { color: 'rgba(0,0,0,0.05)' },
                                pointLabels: { font: { size: 10 } }
                            }
                        },
                        plugins: { legend: { display: false } }
                    }
                });
            }, 100);
        }

        // 关闭海报
        function closePoster() {
            document.getElementById('posterModal').style.display = 'none';
        }

        // 下载海报
        function downloadPoster() {
            const content = document.getElementById('posterContent');
            
            html2canvas(content, {
                scale: 2,
                backgroundColor: '#ffffff',
                useCORS: true
            }).then(canvas => {
                const link = document.createElement('a');
                link.download = '供应链健康度诊断报告.png';
                link.href = canvas.toDataURL();
                link.click();
            });
        }

        // 重新开始
        function restartQuiz() {
            currentQuestion = 0;
            answers = {};
            if (radarChart) radarChart.destroy();
            if (posterChart) posterChart.destroy();
            
            document.getElementById('resultScreen').style.display = 'none';
            document.getElementById('welcomeScreen').style.display = 'block';
        }

        // 键盘支持
        document.addEventListener('keydown', (e) => {
            if (document.getElementById('quizScreen').style.display === 'block') {
                if (e.key === 'ArrowLeft' && currentQuestion > 0) prevQuestion();
                if (e.key === 'ArrowRight' && answers[questions[currentQuestion].id]) nextQuestion();
            }
        });
    </script>
</body>
</html>
