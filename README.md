<!DOCTYPE html>
<html lang="zh-CN" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="description" content="曾与田专属恋爱法庭 - AI爱情判官">
    <title>AI爱情判官 - 曾与田专属恋爱法庭</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        court: {
                            bg: '#0F0913',
                            card: '#1A1024',
                            cardBorder: '#2D1B3F',
                            pink: '#FF2A7A',
                            purple: '#A855F7',
                            gold: '#F59E0B',
                            zeng: '#EC4899',
                            tian: '#8B5CF6'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #0F0913;
            color: #F3F4F6;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;
            background-image: 
                radial-gradient(circle at 10% 20%, rgba(255, 42, 122, 0.08) 0%, transparent 40%),
                radial-gradient(circle at 90% 80%, rgba(168, 85, 247, 0.08) 0%, transparent 40%);
            -webkit-tap-highlight-color: transparent;
        }
        .court-title {
            font-family: "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", serif;
            font-weight: 900;
        }
        .pink-glow {
            box-shadow: 0 0 25px rgba(255, 42, 122, 0.25);
        }
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #0F0913; }
        ::-webkit-scrollbar-thumb { background: #2D1B3F; border-radius: 3px; }
        @keyframes pulseGlow {
            0%, 100% { transform: scale(1); opacity: 0.8; }
            50% { transform: scale(1.03); opacity: 1; }
        }
        .gavel-anim { animation: pulseGlow 3s infinite ease-in-out; }
    </style>
</head>
<body class="min-h-screen pb-12 flex flex-col justify-between">

    <!-- Header -->
    <header class="border-b border-court-cardBorder bg-court-bg/80 backdrop-blur-md sticky top-0 z-50">
        <div class="max-w-6xl mx-auto px-4 py-4 flex flex-wrap items-center justify-between gap-4">
            <div class="flex items-center space-x-3">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-court-pink to-court-purple flex items-center justify-center text-white shadow-lg pink-glow">
                    <i class="fa-solid fa-gavel text-xl"></i>
                </div>
                <div>
                    <h1 class="court-title text-xl md:text-2xl bg-gradient-to-r from-court-pink via-purple-300 to-amber-300 bg-clip-text text-transparent">
                        AI 爱情法庭
                    </h1>
                    <p class="text-xs text-gray-400">《曾田专属恋爱契约与违约处罚法典》</p>
                </div>
            </div>
            <div class="flex items-center gap-3">
                <button onclick="openLawModal()" class="px-3 py-1.5 rounded-lg bg-court-card border border-court-cardBorder text-xs text-pink-300 hover:border-court-pink transition flex items-center gap-2">
                    <i class="fa-solid fa-book-bookmark"></i>
                    <span>查阅法典</span>
                </button>
                <div class="relative group">
                    <button onclick="toggleKeyInput()" class="px-3 py-1.5 rounded-lg bg-court-card border border-court-cardBorder text-xs text-amber-400 hover:border-amber-400/50 transition flex items-center gap-2">
                        <i class="fa-solid fa-key"></i>
                        <span id="keyStatus">Gemini Key</span>
                    </button>
                    <div id="keyDropdown" class="hidden absolute right-0 mt-2 w-72 p-3 bg-court-card border border-court-cardBorder rounded-xl shadow-2xl z-50">
                        <label class="block text-xs font-semibold mb-1 text-gray-300">自定义 Gemini API Key</label>
                        <input type="password" id="apiKeyInput" placeholder="不填则使用本地引擎" class="w-full bg-court-bg border border-court-cardBorder rounded p-2 text-xs text-white focus:border-court-pink focus:outline-none mb-2">
                        <p class="text-[10px] text-gray-400 mb-2">未填时自动使用本地精准匹配引擎</p>
                        <button onclick="saveApiKey()" class="w-full bg-court-pink text-white text-xs py-1.5 rounded font-bold hover:bg-pink-600 transition">保存 Key</button>
                    </div>
                </div>
            </div>
        </div>
    </header>

    <!-- Main -->
    <main class="max-w-6xl mx-auto px-4 py-8 w-full grid grid-cols-1 lg:grid-cols-12 gap-8">
        <!-- Left: Input -->
        <section class="lg:col-span-5 space-y-6">
            <div class="bg-court-card border border-court-cardBorder rounded-2xl p-5 shadow-xl relative overflow-hidden">
                <div class="absolute -top-12 -right-12 w-24 h-24 bg-court-pink/10 rounded-full blur-xl pointer-events-none"></div>
                <h2 class="text-lg font-bold text-gray-100 mb-3 flex items-center gap-2">
                    <i class="fa-solid fa-pen-nib text-court-pink"></i>
                    <span>案情事实陈述</span>
                </h2>
                <p class="text-xs text-gray-400 mb-4">请详细描述曾或田在争议中的言行、语气及事件经过：</p>
                <textarea id="caseDescription" rows="6" 
                    placeholder="例：今天曾跟我说话态度极度不耐烦，我问他晚饭吃什么他敷衍说随便，然后两个人就开始谁也不理谁..." 
                    class="w-full bg-court-bg/90 border border-court-cardBorder rounded-xl p-3.5 text-sm text-gray-200 focus:border-court-pink focus:ring-1 focus:ring-court-pink focus:outline-none transition resize-none"></textarea>
                
                <div class="mt-4">
                    <div class="text-xs font-semibold text-gray-400 mb-2">快捷预设卷宗：</div>
                    <div class="flex flex-wrap gap-2">
                        <button onclick="loadPreset(0)" class="text-[11px] bg-court-bg hover:bg-court-cardBorder border border-court-cardBorder hover:border-court-pink text-gray-300 px-2.5 py-1.5 rounded-lg transition">① 线上聊天态度烦躁</button>
                        <button onclick="loadPreset(1)" class="text-[11px] bg-court-bg hover:bg-court-cardBorder border border-court-cardBorder hover:border-court-pink text-gray-300 px-2.5 py-1.5 rounded-lg transition">② 田翻旧账+冷战</button>
                        <button onclick="loadPreset(2)" class="text-[11px] bg-court-bg hover:bg-court-cardBorder border border-court-cardBorder hover:border-court-pink text-gray-300 px-2.5 py-1.5 rounded-lg transition">③ 曾偷偷跟异性暧昧</button>
                        <button onclick="loadPreset(3)" class="text-[11px] bg-court-bg hover:bg-court-cardBorder border border-court-cardBorder hover:border-court-pink text-gray-300 px-2.5 py-1.5 rounded-lg transition">④ 隐瞒敏感相亲消息</button>
                    </div>
                </div>

                <button id="judgeBtn" onclick="runJudgement()" class="w-full mt-6 bg-gradient-to-r from-court-pink to-court-purple text-white font-bold py-3.5 px-6 rounded-xl shadow-lg pink-glow hover:opacity-95 active:scale-[0.99] transition flex items-center justify-center gap-2">
                    <i class="fa-solid fa-gavel"></i>
                    <span id="btnText">即刻开庭判定</span>
                </button>
            </div>

            <div class="bg-court-card/50 border border-court-cardBorder/60 rounded-xl p-4 text-xs text-gray-400 space-y-2">
                <div class="font-semibold text-gray-300 flex items-center gap-1.5">
                    <i class="fa-solid fa-shield-halved text-court-purple"></i>
                    <span>法庭裁决规则说明</span>
                </div>
                <p>1. 本法庭裁决结果严格背书《曾与田专属恋爱契约》。</p>
                <p>2. 法款判定若涉及“死刑”，即判定终结双方恋爱契约关系。</p>
                <p>3. 罚金违约责任即时生效，双方须严格遵照执行。</p>
            </div>
        </section>

        <!-- Right: Result -->
        <section class="lg:col-span-7 flex flex-col justify-start">
            <div id="emptyState" class="h-full min-h-[420px] bg-court-card border border-dashed border-court-cardBorder rounded-2xl flex flex-col items-center justify-center p-8 text-center space-y-4">
                <div class="w-20 h-20 rounded-full bg-court-bg border border-court-cardBorder flex items-center justify-center text-court-pink text-3xl gavel-anim">
                    <i class="fa-solid fa-scale-balanced"></i>
                </div>
                <div>
                    <h3 class="court-title text-xl font-bold text-gray-200">恋爱法庭准备就绪</h3>
                    <p class="text-xs text-gray-400 max-w-sm mt-1">请在左侧输入争执细节，爱情判官将自动判定责任比例与违约罚款。</p>
                </div>
            </div>

            <div id="loadingState" class="hidden h-full min-h-[420px] bg-court-card border border-court-cardBorder rounded-2xl flex flex-col items-center justify-center p-8 text-center space-y-4">
                <div class="relative w-16 h-16 flex items-center justify-center">
                    <div class="absolute inset-0 rounded-full border-4 border-court-pink border-t-transparent animate-spin"></div>
                    <i class="fa-solid fa-gavel text-court-purple text-xl"></i>
                </div>
                <div>
                    <h3 class="court-title text-lg font-bold text-pink-400 animate-pulse">爱情判官正在审理卷宗...</h3>
                    <p class="text-xs text-gray-400 mt-1">正在检索《恋爱关系特别守则》相关条文</p>
                </div>
            </div>

            <div id="resultCard" class="hidden space-y-6">
                <div class="bg-gradient-to-r from-court-card via-[#261538] to-court-card border-2 border-court-pink/40 rounded-2xl p-6 shadow-2xl relative overflow-hidden">
                    <div class="flex items-center justify-between border-b border-court-cardBorder pb-4">
                        <div class="flex items-center space-x-2">
                            <span class="px-2.5 py-1 bg-court-pink/20 border border-court-pink/50 text-court-pink text-[11px] font-bold rounded-md">号外裁决</span>
                            <span class="text-xs text-gray-400" id="verdictTime">2026-11-07</span>
                        </div>
                        <div class="text-xs text-amber-400 font-semibold flex items-center gap-1">
                            <i class="fa-solid fa-certificate"></i>
                            <span>终审判决 (生效)</span>
                        </div>
                    </div>

                    <div class="mt-6">
                        <div class="flex justify-between items-end mb-2">
                            <div class="text-left">
                                <span class="text-xs font-bold text-court-zeng block">曾 (Zeng) 责任</span>
                                <span class="text-2xl font-black text-court-zeng" id="zengRatioText">0%</span>
                            </div>
                            <div class="text-center pb-1">
                                <span class="text-xs font-bold px-3 py-1 bg-court-bg border border-court-cardBorder rounded-full text-gray-300" id="primaryFaultBadge">曾 错得更多</span>
                            </div>
                            <div class="text-right">
                                <span class="text-xs font-bold text-court-tian block">田 (Tian) 责任</span>
                                <span class="text-2xl font-black text-court-tian" id="tianRatioText">0%</span>
                            </div>
                        </div>
                        <div class="w-full h-4 bg-court-bg rounded-full overflow-hidden flex p-0.5 border border-court-cardBorder">
                            <div id="zengBar" class="h-full bg-gradient-to-r from-pink-600 to-court-zeng rounded-l-full transition-all duration-1000" style="width: 50%"></div>
                            <div id="tianBar" class="h-full bg-gradient-to-r from-court-tian to-purple-600 rounded-r-full transition-all duration-1000" style="width: 50%"></div>
                        </div>
                    </div>

                    <div class="mt-6 bg-court-bg/80 border border-amber-500/30 rounded-xl p-4 flex items-center justify-between">
                        <div class="flex items-center gap-3">
                            <div id="punishmentIcon" class="w-12 h-12 rounded-xl bg-amber-500/20 text-amber-400 flex items-center justify-center text-2xl font-bold">￥</div>
                            <div>
                                <p class="text-xs text-gray-400">判决最终处罚结果</p>
                                <p class="text-lg font-black text-amber-300" id="punishmentText">扣除违约金 300 元</p>
                            </div>
                        </div>
                        <div class="text-right">
                            <span class="text-[10px] text-gray-500 block">执行受限度</span>
                            <span class="text-xs font-bold text-pink-400">强制执行</span>
                        </div>
                    </div>
                </div>

                <div class="bg-court-card border border-court-cardBorder rounded-2xl p-6 shadow-xl space-y-4">
                    <h3 class="text-sm font-bold text-gray-200 flex items-center justify-between">
                        <span class="flex items-center gap-2">
                            <i class="fa-solid fa-scale-unbalanced-flip text-court-purple"></i>
                            <span>最契合触发法条及文本</span>
                        </span>
                        <span class="text-xs px-2.5 py-0.5 rounded-full bg-purple-500/20 text-purple-300 border border-purple-500/30" id="matchDegree">契合度 95%</span>
                    </h3>
                    <div class="bg-court-bg border-l-4 border-court-pink p-4 rounded-r-xl space-y-2">
                        <div class="flex items-center justify-between">
                            <h4 class="font-bold text-sm text-pink-300" id="articleTitle">第九条 (情绪控制与耐烦义务)</h4>
                            <span class="text-[11px] text-gray-400" id="articleChapter">第二章 田之义务</span>
                        </div>
                        <p class="text-xs text-gray-300 leading-relaxed italic" id="articleContent">“田在日常交往中应当保持基本的情绪稳定...”</p>
                        <div class="text-xs text-amber-400 font-semibold pt-1 border-t border-court-cardBorder/50" id="articlePenalty">违约责任：表现出不耐烦的，扣除人民币50元。</div>
                    </div>
                    <div class="space-y-2 pt-2">
                        <h4 class="text-xs font-bold text-gray-300 flex items-center gap-1.5">
                            <i class="fa-solid fa-brain text-court-pink"></i>
                            <span>法庭判定案情理由解析</span>
                        </h4>
                        <div class="bg-court-bg/50 border border-court-cardBorder rounded-xl p-3.5 text-xs text-gray-300 leading-relaxed" id="judgementRationale"></div>
                    </div>
                    <div class="pt-2">
                        <button onclick="copyVerdict()" class="w-full bg-court-cardBorder hover:bg-court-pink/20 border border-court-cardBorder hover:border-court-pink text-xs text-gray-200 hover:text-pink-300 py-2.5 rounded-xl font-bold transition flex items-center justify-center gap-2">
                            <i class="fa-regular fa-copy"></i>
                            <span>复制官方判决书</span>
                        </button>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- Law Modal -->
    <div id="lawModal" class="hidden fixed inset-0 z-50 bg-black/80 backdrop-blur-sm flex items-center justify-center p-4">
        <div class="bg-court-card border border-court-cardBorder w-full max-w-2xl rounded-2xl shadow-2xl max-h-[85vh] flex flex-col">
            <div class="p-4 border-b border-court-cardBorder flex items-center justify-between">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-book text-court-pink text-lg"></i>
                    <h3 class="court-title text-base font-bold text-white">恋爱关系特别守则与违约处罚法典</h3>
                </div>
                <button onclick="closeLawModal()" class="text-gray-400 hover:text-white p-1">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>
            <div class="p-5 overflow-y-auto space-y-6 text-xs text-gray-300">
                <div>
                    <h4 class="text-sm font-bold text-court-zeng border-b border-court-zeng/30 pb-1.5 mb-3">第一章 曾之义务与禁止性规定</h4>
                    <div class="space-y-3" id="zengLawsList"></div>
                </div>
                <div>
                    <h4 class="text-sm font-bold text-court-tian border-b border-court-tian/30 pb-1.5 mb-3">第二章 田之义务与禁止性规定</h4>
                    <div class="space-y-3" id="tianLawsList"></div>
                </div>
            </div>
        </div>
    </div>

    <div id="toast" class="fixed bottom-6 right-6 z-50 transform translate-y-20 opacity-0 transition-all duration-300 bg-court-card border border-court-pink text-white px-4 py-3 rounded-xl shadow-2xl flex items-center gap-2 text-xs">
        <i class="fa-solid fa-circle-check text-court-pink"></i>
        <span id="toastMsg">操作成功</span>
    </div>

    <footer class="border-t border-court-cardBorder mt-auto py-4 bg-court-bg text-center text-xs text-gray-500">
        <p>© 2026 曾与田专属恋爱法庭 · 愿天下有情人遵循法典，恩爱长久</p>
    </footer>

    <script>
        const CODE_DATABASE = [
            {id:"ZENG_1",chapter:"第一章 曾之义务",title:"第一条 (禁止轻率解除恋爱关系义务)",person:"曾",penalty:"扣除人民币1000元作为惩罚性违约金",fineAmount:1000,isDeathPenalty:false,content:"恋爱关系存续期间，曾不得无故、轻率提出分手。",keywords:["分手","提出分手","不谈了","分开","轻率分手","要分手"]},
            {id:"ZENG_2",chapter:"第一章 曾之义务",title:"第二条 (禁止隐瞒沟通与有效表达义务)",person:"曾",penalty:"扣除人民币300元",fineAmount:300,isDeathPenalty:false,content:"曾遭遇不满时，应当直接告知并进行有效沟通，严禁私下向第三方(如朋友)吐槽或采取“憋着不说”的冷处理方式。",keywords:["吐槽","憋着","不说话","第三方","找朋友","闺蜜","兄弟","不直接说","隐瞒沟通","私下"]},
            {id:"ZENG_3",chapter:"第一章 曾之义务",title:"第三条 (禁止严重欺诈与虚构事实规定)",person:"曾",penalty:"直接解除恋爱关系 (死刑)",fineAmount:0,isDeathPenalty:true,content:"曾负有诚实信用告知义务，严禁实施撒谎欺骗、隐瞒恋爱关系核心细节等行为。",keywords:["撒谎","欺骗","虚构","骗我","假装","隐瞒细节","虚报","不诚实","死刑"]},
            {id:"ZENG_4",chapter:"第一章 曾之义务",title:"第四条 (保持社交边界与忠实义务)",person:"曾",penalty:"直接解除恋爱关系 (死刑)",fineAmount:0,isDeathPenalty:true,content:"曾应当恪守恋爱专属边界，保持对伴侣的忠诚，严禁与他人有任何形式的暧昧、超越普通朋友界限的言行。",keywords:["暧昧","出轨","越界","异性","聊骚","超越边界","不忠","背叛","前任"]},
            {id:"ZENG_5",chapter:"第一章 曾之义务",title:"第五条 (禁止工具化与侵害伴侣权益规定)",person:"曾",penalty:"扣除人民币400元",fineAmount:400,isDeathPenalty:false,content:"曾不得将田作为实现个人目的之工具，不得恶意利用田的付出、好意及伴侣资源为自己谋取私利。",keywords:["工具人","利用","好意","付出","谋取私利","占便宜"]},
            {id:"ZENG_6",chapter:"第一章 曾之义务",title:"第六条 (情绪控制与文明沟通义务)",person:"曾",penalty:"扣除人民币50元",fineAmount:50,isDeathPenalty:false,content:"曾通过线上渠道与田进行交流时，应当保持合理的耐心与良好态度，严禁表现出敷衍、烦躁等不良情绪。",keywords:["语气","线上","聊天","敷衍","烦躁","不耐烦","冷淡","回复慢","态度差","哦","嗯"]},
            {id:"TIAN_7",chapter:"第二章 田之义务",title:"第七条 (禁止事后诸葛亮与即时表达义务)",person:"田",penalty:"扣除人民币300元",fineAmount:300,isDeathPenalty:false,content:"田在事件发生时应当即时表达真实想法与诉求，严禁事后诸葛亮、不当场沟通而在事后进行追究或翻旧账。",keywords:["事后诸葛亮","翻旧账","事后","追究","当场不说","过后","之前你","上次你"]},
            {id:"TIAN_8",chapter:"第二章 田之义务",title:"第八条 (禁止冷战与实施冷暴力规定)",person:"田",penalty:"扣除人民币800元",fineAmount:800,isDeathPenalty:false,content:"恋爱双方发生分歧时，田严禁采取拒绝沟通、不予回应的冷战方式，禁止对伴侣实施精神冷暴力。",keywords:["冷战","冷暴力","不理人","拒绝沟通","不回应","不接电话","关机","拉黑","消失"]},
            {id:"TIAN_9",chapter:"第二章 田之义务",title:"第九条 (情绪控制与耐烦义务)",person:"田",penalty:"扣除人民币50元",fineAmount:50,isDeathPenalty:false,content:"田在日常交往中应当保持基本的情绪稳定，对曾保持耐烦态度，严禁表现出不耐烦情绪。",keywords:["情绪不稳","脾气","发火","不耐烦","烦躁","凶","冲"]},
            {id:"TIAN_10",chapter:"第二章 田之义务",title:"第十条 (禁止轻率解除恋爱关系义务)",person:"田",penalty:"扣除人民币1000元作为惩罚性违约金",fineAmount:1000,isDeathPenalty:false,content:"恋爱关系存续期间，田同样不得无故、轻率提出分手。",keywords:["分手","提出分手","不谈了","分开","轻率分手","要分手"]},
            {id:"TIAN_11",chapter:"第二章 田之义务",title:"第十一条 (禁止片面定性与非理性归纳规定)",person:"田",penalty:"扣除人民币400元",fineAmount:400,isDeathPenalty:false,content:"田在评价事物或伴侣行为时，严禁“以点概面”、执着于过去视角直接下定绝对化定义。",keywords:["以点概面","过去","绝对化","你总是","你从来","未来也必然","臆断","定性","主观推断"]},
            {id:"TIAN_12",chapter:"第二章 田之义务",title:"第十二条 (信息及时披露与禁止恶意滞后规定)",person:"田",penalty:"扣除人民币200元",fineAmount:200,isDeathPenalty:false,content:"凡收到任何可能对感情走向产生影响的敏感内容，田负有即时、完全披露的义务，严禁隐瞒或拖延告知。",keywords:["敏感信息","隐瞒","拖延告知","迟到消息","相亲","前任联系","没告诉我","延时"]}
        ];

        const PRESETS = [
            "今天晚上在微信上跟曾沟通周末去哪里玩，曾回消息态度特别敷衍、烦躁，就回了“随便”两个字，感觉很不耐烦。",
            "田对前几天的一件小事一直当场不说，今天突然拉出来翻旧账追究曾，然后直接开始冷战，不回微信不接电话已经一整晚了。",
            "曾今天被田发现私下跟别的异性朋友聊天言语暧昧，甚至还隐瞒了两人一起喝咖啡的事实。",
            "田收到前任发来的叙旧敏感短信，却没有及时告知曾，拖延了好几天被曾无意间看到才承认。"
        ];

        window.onload = function() {
            renderLawModalList();
            checkSavedApiKey();
        };

        function renderLawModalList() {
            const zengList = document.getElementById('zengLawsList');
            const tianList = document.getElementById('tianLawsList');
            CODE_DATABASE.forEach(item => {
                const html = `<div class="bg-court-bg/60 p-3 rounded-xl border border-court-cardBorder"><div class="flex justify-between items-center mb-1"><span class="font-bold text-gray-200">${item.title}</span><span class="text-[10px] px-2 py-0.5 rounded ${item.isDeathPenalty ? 'bg-red-500/20 text-red-400 border border-red-500/30' : 'bg-amber-500/20 text-amber-300 border border-amber-500/30'}">${item.penalty}</span></div><p class="text-gray-400 leading-relaxed">${item.content}</p></div>`;
                if(item.person === '曾') zengList.innerHTML += html;
                else tianList.innerHTML += html;
            });
        }

        function openLawModal() { document.getElementById('lawModal').classList.remove('hidden'); }
        function closeLawModal() { document.getElementById('lawModal').classList.add('hidden'); }
        function loadPreset(idx) { document.getElementById('caseDescription').value = PRESETS[idx]; showToast("案例已一键导入"); }
        function toggleKeyInput() { document.getElementById('keyDropdown').classList.toggle('hidden'); }
        function saveApiKey() {
            const val = document.getElementById('apiKeyInput').value.trim();
            if(val) {
                localStorage.setItem('gemini_api_key', val);
                document.getElementById('keyStatus').innerText = "Key 已设置";
                showToast("API Key 已成功保存！");
            } else {
                localStorage.removeItem('gemini_api_key');
                document.getElementById('keyStatus').innerText = "Gemini Key";
                showToast("使用本地匹配引擎");
            }
            document.getElementById('keyDropdown').classList.add('hidden');
        }
        function checkSavedApiKey() {
            const saved = localStorage.getItem('gemini_api_key');
            if(saved) {
                document.getElementById('keyStatus').innerText = "Key 已设置";
                document.getElementById('apiKeyInput').value = saved;
            }
        }
        function showToast(msg) {
            const toast = document.getElementById('toast');
            document.getElementById('toastMsg').innerText = msg;
            toast.classList.remove('translate-y-20', 'opacity-0');
            setTimeout(() => toast.classList.add('translate-y-20', 'opacity-0'), 3000);
        }

        function runFallbackEngine(text) {
            let zengScores = {}, tianScores = {};
            CODE_DATABASE.forEach(law => {
                let score = 0;
                law.keywords.forEach(kw => { if (text.includes(kw)) score += 25; });
                if (law.person === '曾') zengScores[law.id] = score;
                else tianScores[law.id] = score;
            });
            let bestLaw = CODE_DATABASE[0], maxScore = 0;
            CODE_DATABASE.forEach(law => {
                let s = (law.person === '曾' ? zengScores[law.id] : tianScores[law.id]) || 0;
                if (s > maxScore) { maxScore = s; bestLaw = law; }
            });
            let matchPercentage = Math.min(98, Math.max(70, 75 + maxScore));
            if (maxScore === 0) matchPercentage = 82;
            let zengRatio = 50, tianRatio = 50;
            if (bestLaw.person === '曾') {
                zengRatio = Math.min(95, 60 + Math.floor(Math.random() * 20));
                tianRatio = 100 - zengRatio;
            } else {
                tianRatio = Math.min(95, 60 + Math.floor(Math.random() * 20));
                zengRatio = 100 - tianRatio;
            }
            if(text.includes('冷战') || text.includes('翻旧账')) { tianRatio = 75; zengRatio = 25; }
            if(text.includes('语气') || text.includes('敷衍') || text.includes('暧昧')) { zengRatio = 80; tianRatio = 20; }
            let rationale = `本庭经判定，案情描述中强烈符合【${bestLaw.title}】特征。`;
            if (bestLaw.person === '曾') rationale += `曾在此次事件中主导了违反《法典》的言行（责任占比 ${zengRatio}%）。`;
            else rationale += `田在此次事件中存在明确违反条款行为（责任占比 ${tianRatio}%）。`;
            return {
                matchedLawTitle: bestLaw.title,
                matchedLawChapter: bestLaw.chapter,
                matchedLawContent: bestLaw.content,
                matchedLawPenalty: bestLaw.penalty,
                matchDegree: matchPercentage + "%",
                zengRatio: zengRatio,
                tianRatio: tianRatio,
                punishmentText: bestLaw.penalty,
                isDeathPenalty: bestLaw.isDeathPenalty,
                rationale: rationale
            };
        }

        async function runJudgement() {
            const caseText = document.getElementById('caseDescription').value.trim();
            if(!caseText) { showToast("请先输入争执案情细节！"); return; }
            document.getElementById('emptyState').classList.add('hidden');
            document.getElementById('resultCard').classList.add('hidden');
            document.getElementById('loadingState').classList.remove('hidden');
            const savedKey = localStorage.getItem('gemini_api_key') || "";
            let result = null;
            if(savedKey) {
                try { result = await callGeminiAPI(caseText, savedKey); }
                catch(e) { result = runFallbackEngine(caseText); }
            } else {
                await new Promise(r => setTimeout(r, 600));
                result = runFallbackEngine(caseText);
            }
            renderJudgementResult(result);
        }

        async function callGeminiAPI(caseText, apiKey) {
            const systemPrompt = `你是一个严肃而富有爱意的“AI爱情判官”，背书《恋爱关系特别守则与违约处罚法典》。请严格返回JSON格式。`;
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;
            const response = await fetch(apiUrl, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    contents: [{ parts: [{ text: `案情描述：${caseText}\n\n请按以下JSON格式返回：\n{"matchedLawTitle":"","matchedLawChapter":"","matchedLawContent":"","matchedLawPenalty":"","matchDegree":"95%","zengRatio":50,"tianRatio":50,"punishmentText":"","isDeathPenalty":false,"rationale":""}` }] }],
                    generationConfig: { responseMimeType: "application/json" }
                })
            });
            const data = await response.json();
            const text = data.candidates?.[0]?.content?.parts?.[0]?.text;
            if(!text) throw new Error("Invalid API response");
            return JSON.parse(text);
        }

        let currentJudgementData = null;
        function renderJudgementResult(data) {
            currentJudgementData = data;
            document.getElementById('loadingState').classList.add('hidden');
            document.getElementById('resultCard').classList.remove('hidden');
            const now = new Date();
            document.getElementById('verdictTime').innerText = `${now.getFullYear()}-${(now.getMonth()+1).toString().padStart(2,'0')}-${now.getDate().toString().padStart(2,'0')} ${now.getHours().toString().padStart(2,'0')}:${now.getMinutes().toString().padStart(2,'0')}`;
            const zengR = Math.max(0, Math.min(100, data.zengRatio));
            const tianR = 100 - zengR;
            document.getElementById('zengRatioText').innerText = zengR + "%";
            document.getElementById('tianRatioText').innerText = tianR + "%";
            document.getElementById('zengBar').style.width = zengR + "%";
            document.getElementById('tianBar').style.width = tianR + "%";
            const badge = document.getElementById('primaryFaultBadge');
            if (zengR > tianR) {
                badge.innerText = `曾 责任更大 (${zengR}%)`;
                badge.className = "text-xs font-bold px-3 py-1 bg-court-pink/20 border border-court-pink/40 rounded-full text-pink-300";
            } else if (tianR > zengR) {
                badge.innerText = `田 责任更大 (${tianR}%)`;
                badge.className = "text-xs font-bold px-3 py-1 bg-court-purple/20 border border-court-purple/40 rounded-full text-purple-300";
            } else {
                badge.innerText = `双方同等责任 (50%/50%)`;
                badge.className = "text-xs font-bold px-3 py-1 bg-amber-500/20 border border-amber-500/40 rounded-full text-amber-300";
            }
            const penaltyTextEl = document.getElementById('punishmentText');
            const iconEl = document.getElementById('punishmentIcon');
            if (data.isDeathPenalty) {
                penaltyTextEl.innerText = "终极裁决：直接解除恋爱关系 (死刑)";
                penaltyTextEl.className = "text-lg font-black text-red-500 animate-pulse";
                iconEl.innerHTML = `<i class="fa-solid fa-skull"></i>`;
                iconEl.className = "w-12 h-12 rounded-xl bg-red-500/20 text-red-500 flex items-center justify-center text-2xl font-bold";
            } else {
                penaltyTextEl.innerText = data.punishmentText || data.matchedLawPenalty;
                penaltyTextEl.className = "text-lg font-black text-amber-300";
                iconEl.innerHTML = `￥`;
                iconEl.className = "w-12 h-12 rounded-xl bg-amber-500/20 text-amber-400 flex items-center justify-center text-2xl font-bold";
            }
            document.getElementById('matchDegree').innerText = `契合度 ${data.matchDegree}`;
            document.getElementById('articleTitle').innerText = data.matchedLawTitle;
            document.getElementById('articleChapter').innerText = data.matchedLawChapter;
            document.getElementById('articleContent').innerText = `“${data.matchedLawContent}”`;
            document.getElementById('articlePenalty').innerText = `违约责任：${data.matchedLawPenalty}`;
            document.getElementById('judgementRationale').innerText = data.rationale;
        }

        function copyVerdict() {
            if(!currentJudgementData) return;
            const text = `【AI爱情法庭 - 官方判决书】\n判定时间：${document.getElementById('verdictTime').innerText}\n\n▶ 责任判定比例：\n- 曾：${currentJudgementData.zengRatio}%\n- 田：${currentJudgementData.tianRatio}%\n\n▶ 触犯法条：\n${currentJudgementData.matchedLawTitle}\n“${currentJudgementData.matchedLawContent}”\n\n▶ 最终判决：\n${currentJudgementData.punishmentText || currentJudgementData.matchedLawPenalty}\n\n▶ 法官判词：\n${currentJudgementData.rationale}`;
            const input = document.createElement('textarea');
            input.value = text;
            document.body.appendChild(input);
            input.select();
            document.execCommand('copy');
            document.body.removeChild(input);
            showToast("判决书已复制到剪贴板！");
        }
    </script>
</body>
</html>