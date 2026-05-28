<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Toro Quest: CSUDH Student Companion</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        toro: {
                            crimson: '#8A002C',
                            gold: '#FFC72C',
                            goldDark: '#D4A316',
                            green: '#008751',
                            dark: '#1A1A1A',
                            light: '#FDFBF7'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap');
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        .progress-bar {
            transition: width 0.4s ease-in-out;
        }
        @keyframes customPulse {
            0%, 100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(138, 0, 44, 0.4); }
            50% { transform: scale(1.08); box-shadow: 0 0 0 8px rgba(138, 0, 44, 0); }
        }
        .active-pulse {
            animation: customPulse 2s infinite;
        }
        @keyframes popIn {
            0% { transform: scale(0.8); opacity: 0; }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); opacity: 1; }
        }
        .animate-pop {
            animation: popIn 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }
        @keyframes floatBull {
            0%, 100% { transform: translateY(0) rotate(-10deg); }
            50% { transform: translateY(-8px) rotate(10deg); }
        }
        .bull-float {
            animation: floatBull 3.5s ease-in-out infinite;
        }
    </style>
</head>
<body class="bg-toro-light text-gray-800 min-h-screen flex flex-col antialiased pb-24 relative overflow-x-hidden">

    <!-- Top Status Bar & Profile Hub -->
    <header class="bg-toro-crimson text-white shadow-md sticky top-0 z-40 border-b-4 border-toro-gold px-4 py-3">
        <div class="max-w-md mx-auto flex items-center justify-between">
            <div class="flex items-center space-x-2.5">
                <div class="bg-toro-gold p-1.5 rounded-xl border border-white shadow-inner flex items-center justify-center animate-bounce">
                    <span class="text-xl">🐂</span>
                </div>
                <div>
                    <h1 class="text-base font-extrabold tracking-tight">Toro Quest!</h1>
                    <div class="flex items-center space-x-1.5 mt-0.5">
                        <span class="text-xxs uppercase font-black text-toro-gold" id="studentRank">Recruit</span>
                        <div class="w-16 bg-red-950 rounded-full h-1.5 overflow-hidden">
                            <div id="xpBar" class="bg-toro-gold h-full progress-bar" style="width: 20%;"></div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="flex items-center space-x-2">
                <button onclick="toggleLanguage()" id="langBtn" class="bg-white/10 hover:bg-white/20 border border-white/20 text-white font-extrabold text-xs px-2.5 py-1.5 rounded-xl transition flex items-center gap-1">
                    <span>🇲🇽</span> <span id="langLabel">Español</span>
                </button>
                <button onclick="resetApp()" class="p-1.5 text-red-200 hover:text-white transition" title="Reset Quest Progress">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M16.023 9.348h4.992v-.001M2.985 19.644v-4.992m0 0h4.992m-4.993 0l3.181 3.183a8.25 8.25 0 0013.803-3.7M4.031 9.865a8.25 8.25 0 0113.803-3.7l3.181 3.182m0-4.991v4.99"></path></svg>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-grow max-w-md w-full mx-auto px-4 py-4 flex flex-col space-y-4 relative z-10">

        <!-- FIXED DYNAMIC HIDDEN BULL PER STOP (Opacity 40%) -->
        <div id="stopHiddenBull" onclick="discoverSecretBull()" class="absolute text-5xl opacity-40 cursor-pointer z-50 select-none hover:opacity-100 bull-float" style="display: none; text-shadow: 0 0 10px rgba(255,255,255,0.8);">
            🐂
        </div>

        <!-- SCREEN 1: QUESTS & PRESENTATION HUB -->
        <div id="questsScreen" class="space-y-4 relative z-10">
            
            <div class="bg-white p-4 rounded-3xl shadow-sm border border-gray-100 relative z-10">
                <h3 class="text-xs font-bold text-gray-400 uppercase tracking-widest mb-3 flex justify-between items-center">
                    <span id="companionPathLabel">Your Live Track</span>
                    <span id="companionStopName" class="text-toro-crimson font-black text-xs">Welch Courtyard</span>
                </h3>
                <div class="flex justify-between items-center relative">
                    <div class="absolute left-0 right-0 top-1/2 h-1.5 bg-gray-200 -translate-y-1/2 z-0 rounded-full"></div>
                    <div id="pathProgressHighlight" class="absolute left-0 top-1/2 h-1.5 bg-toro-crimson -translate-y-1/2 z-0 rounded-full transition-all duration-300" style="width: 0%;"></div>
                    <button onclick="changeStop(1)" id="stopNode-1" class="z-10 w-8 h-8 rounded-full bg-toro-crimson text-white text-xs font-black shadow-md flex items-center justify-center transition-all focus:outline-none border-2 border-toro-gold active-pulse">1</button>
                    <button onclick="changeStop(2)" id="stopNode-2" class="z-10 w-8 h-8 rounded-full bg-gray-200 text-gray-600 text-xs font-black flex items-center justify-center transition-all focus:outline-none">2</button>
                    <button onclick="changeStop(3)" id="stopNode-3" class="z-10 w-8 h-8 rounded-full bg-gray-200 text-gray-600 text-xs font-black flex items-center justify-center transition-all focus:outline-none">3</button>
                    <button onclick="changeStop(4)" id="stopNode-4" class="z-10 w-8 h-8 rounded-full bg-gray-200 text-gray-600 text-xs font-black flex items-center justify-center transition-all focus:outline-none">4</button>
                    <button onclick="changeStop(5)" id="stopNode-5" class="z-10 w-8 h-8 rounded-full bg-gray-200 text-gray-600 text-xs font-black flex items-center justify-center transition-all focus:outline-none">5</button>
                </div>
            </div>

            <!-- Active Quest Card / Main Quiz -->
            <div id="questCard" class="bg-white rounded-3xl shadow-md border border-gray-100 overflow-hidden relative z-10">
                <div class="bg-gradient-to-r from-toro-crimson to-red-900 px-4 py-3 text-white flex justify-between items-center">
                    <span class="text-xs font-black tracking-widest uppercase text-toro-gold animate-pulse" id="questStopNum">Stop 1 Quest</span>
                    <span class="bg-white/10 text-white text-xxs font-bold px-2 py-0.5 rounded-full border border-white/10 flex items-center gap-1">
                        <span id="questStatusLabel">Active Quest</span>
                    </span>
                </div>

                <div class="p-5 space-y-4" id="quizContentWrapper">
                    <div class="flex justify-between items-center border-b border-gray-100 pb-3">
                        <div>
                            <h2 id="questTitle" class="text-lg font-extrabold text-toro-crimson">Unleash Your Inner Toro!</h2>
                            <p id="questSubtitle" class="text-xs text-gray-400 font-bold mt-0.5">📍 Welch Hall Courtyard</p>
                        </div>
                        <div class="text-right">
                            <span class="text-xxs font-bold text-toro-crimson uppercase block" id="quizStepCount">Question 1 of 5</span>
                            <span class="text-xxs font-extrabold text-gray-400" id="quizReqLabel">Score 3/5 to Pass!</span>
                        </div>
                    </div>

                    <div id="questClueBox" class="bg-amber-50/50 rounded-2xl p-3.5 border border-amber-100 flex items-start gap-3">
                        <span class="text-2xl animate-bounce" id="questVisualIcon">🐂</span>
                        <div>
                            <p id="questGoalText" class="text-xs text-gray-700 leading-relaxed font-semibold">Goal: Listen carefully to your guide, then verify your understanding of Welch Hall!</p>
                        </div>
                    </div>

                    <div id="quizQuestionPanel" class="space-y-4">
                        <!-- Questions injected here -->
                    </div>
                </div>
            </div>

            <!-- Visual Identification Photo Quiz -->
            <div class="bg-white rounded-3xl shadow-md border border-gray-100 p-5 space-y-4 relative overflow-hidden z-10">
                <div class="absolute right-3 top-3 text-3xl opacity-10 font-black">📸</div>
                <div class="flex items-center justify-between">
                    <span class="bg-toro-gold/20 text-toro-crimson text-xs font-extrabold px-2.5 py-1 rounded-full uppercase" id="photoQuizBadgeLabel">Visual Check</span>
                    <span class="text-xs font-extrabold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-lg" id="visualTriesLabel">Tries: 3</span>
                </div>
                
                <div class="rounded-xl overflow-hidden shadow-sm border border-gray-200">
                    <img id="photoQuizImg" src="" alt="Campus Feature" class="w-full h-44 object-cover">
                </div>
                
                <div id="visualCheckContent">
                    <h3 class="font-black text-sm text-toro-crimson" id="photoQuizQuestion">What building is shown in this picture?</h3>
                    <div id="photoQuizOptions" class="space-y-2 mt-2">
                        <!-- Photo quiz buttons injected programmatically -->
                    </div>
                </div>
                <div id="visualCheckResult" class="hidden text-center py-4 bg-gray-50 rounded-xl border border-gray-200">
                    <!-- Results injected here -->
                </div>
            </div>

            <!-- Resource Spotlight Comprehension Quiz -->
            <div class="bg-white rounded-3xl shadow-md border border-gray-100 p-5 space-y-4 relative overflow-hidden z-10">
                <div class="absolute right-3 top-3 text-3xl opacity-10 font-black">🏫</div>
                <div class="flex items-center justify-between">
                    <span class="bg-toro-green/10 text-toro-green text-xs font-extrabold px-2.5 py-1 rounded-full uppercase" id="resourceSpotlightBadgeLabel">Resource Spotlight</span>
                    <span class="text-xs font-extrabold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-lg" id="resourceTriesLabel">Tries: 3</span>
                </div>
                
                <div id="resourceQuizContent">
                    <h3 class="font-black text-base text-toro-crimson" id="resourceQuizTitle">Spotlight Title</h3>
                    <p class="text-xs text-gray-600 mt-1 mb-3" id="resourceQuizDesc">Description</p>
                    <div class="bg-slate-50 p-3 rounded-xl border border-slate-200 mb-3">
                        <p class="text-sm font-bold text-slate-800" id="resourceQuizQuestion">Question?</p>
                    </div>
                    <div id="resourceQuizOptions" class="space-y-2">
                        <!-- Resource quiz options injected programmatically -->
                    </div>
                </div>
                <div id="resourceCheckResult" class="hidden text-center py-4 bg-gray-50 rounded-xl border border-gray-200">
                    <!-- Results injected here -->
                </div>
            </div>

            <!-- Toro Voice Coach -->
            <div class="bg-white p-5 rounded-3xl shadow-sm border border-gray-100 flex flex-col relative z-10">
                <div class="flex justify-between items-start mb-3">
                    <div>
                        <h3 class="font-extrabold text-sm text-toro-crimson flex items-center gap-1.5">
                            🗣️ Toro Voice Coach
                        </h3>
                        <p id="toroTalkerSubtitle" class="text-xxs text-gray-400">Tap to hear academic terms spoken clearly with helpful pacing</p>
                    </div>
                    <span class="bg-toro-crimson/5 text-toro-crimson text-xxs font-bold px-2 py-0.5 rounded-full" id="eldGuideLabel">ELD Guide</span>
                </div>

                <div id="voiceCoachContainer" class="space-y-3">
                    <!-- Term items dynamically injected -->
                </div>
            </div>

        </div>

        <!-- SCREEN 2: BINGO CARD INTERACTIVE -->
        <div id="bingoScreen" class="hidden space-y-4 relative z-10">
            <div class="text-center py-2">
                <span class="text-4xl block">🎰</span>
                <h2 id="bingoScreenTitle" class="text-xl font-black text-toro-crimson mt-1">Toro Bingo Challenge</h2>
                <p id="bingoScreenDesc" class="text-xs text-gray-500 max-w-[280px] mx-auto mt-1">Find these items during the tour and tap them. Get a blackout (all squares) to win the badge!</p>
            </div>

            <div class="bg-white p-4 rounded-3xl shadow-md border border-gray-100 relative">
                <div id="bingoCelebrationOverlay" class="hidden absolute inset-0 bg-toro-crimson/95 backdrop-blur-sm z-20 rounded-3xl flex flex-col justify-center items-center text-center p-4 text-white">
                    <span class="text-5xl animate-bounce">🎉</span>
                    <h3 class="text-3xl font-black text-toro-gold animate-pulse mt-2">BINGO!</h3>
                    <p id="bingoWinDesc" class="text-sm text-red-100 mt-2 max-w-xs font-semibold">You found every single item on the campus tour! The Bingo Champion badge is yours.</p>
                    <button onclick="claimBingoBadge()" id="bingoClaimBtn" class="mt-6 bg-toro-gold text-toro-crimson font-black text-sm px-6 py-3 rounded-2xl hover:scale-105 transition shadow-lg">
                        Claim Bingo Badge!
                    </button>
                </div>

                <div class="grid grid-cols-3 gap-3" id="bingoGridContainer">
                    <!-- Bingo generated -->
                </div>
            </div>
            
            <div class="text-center">
                 <button onclick="resetBingo()" id="resetBingoBtn" class="text-xs font-bold text-gray-400 underline hover:text-toro-crimson">Reset Bingo Board</button>
            </div>
        </div>

        <!-- SCREEN 3: STUDENT LIFE RESOURCE DIRECTORY -->
        <div id="resourcesScreen" class="hidden space-y-4 relative z-10">
            <div class="text-center py-2">
                <span class="text-3xl">🏫</span>
                <h2 id="resScreenTitle" class="text-xl font-black text-toro-crimson mt-1">Student Life Directory</h2>
                <p id="resScreenDesc" class="text-xs text-gray-500">Essential support resources on campus—all completely free!</p>
            </div>

            <div class="relative">
                <input type="text" id="resourceSearch" oninput="searchResources()" placeholder="Search services..." class="w-full pl-10 pr-4 py-2.5 rounded-2xl border border-gray-200 bg-white text-sm focus:outline-none focus:ring-2 focus:ring-toro-crimson focus:border-transparent shadow-xs">
                <span class="absolute left-3.5 top-3.5 text-gray-400 text-xs">🔍</span>
            </div>

            <div id="studentResourcesList" class="space-y-3">
                <!-- Resources -->
            </div>
        </div>

        <!-- SCREEN 4: FUN STICKERS & GOLD PASS -->
        <div id="bagScreen" class="hidden space-y-4 relative z-10">
            <div class="text-center py-2">
                <span class="text-4xl">🎒</span>
                <h2 id="bagTitle" class="text-2xl font-black text-toro-crimson mt-1">My Toro Sticker Book</h2>
                <p id="bagDesc" class="text-xs text-gray-500 mt-1">Pass checks and find secrets to earn stickers!</p>
            </div>

            <div class="bg-toro-crimson/5 border border-toro-crimson/10 p-4 rounded-3xl mb-2">
                <h3 id="specialBadgesTitle" class="text-xs font-black text-toro-crimson uppercase tracking-wider text-center mb-3">Special Badges</h3>
                <div class="grid grid-cols-2 gap-4">
                    <div id="sticker-bingo" class="bg-white border border-gray-200/60 p-3 rounded-2xl flex flex-col items-center justify-center text-center opacity-40 transition-all shadow-xs">
                        <div class="w-12 h-12 bg-amber-50 rounded-full flex items-center justify-center text-2xl shadow-inner border border-amber-100">🎰</div>
                        <h4 id="stkTitle-bingo" class="font-extrabold text-xs mt-1.5 text-gray-800">Bingo Champ</h4>
                        <span class="text-[10px] font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1" id="stkStatus-bingo">Locked</span>
                    </div>
                    <div id="sticker-bull" class="bg-white border border-gray-200/60 p-3 rounded-2xl flex flex-col items-center justify-center text-center opacity-40 transition-all shadow-xs">
                        <div class="w-12 h-12 bg-gray-100 rounded-full flex items-center justify-center text-2xl shadow-inner border border-gray-200">🕵️</div>
                        <h4 id="stkTitle-bull" class="font-extrabold text-xs mt-1.5 text-gray-800">Eagle Eye</h4>
                        <p class="text-[9px] text-gray-400 mt-0.5" id="bullProgressLabel">0/5 Bulls</p>
                        <span class="text-[10px] font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1" id="stkStatus-bull">Locked</span>
                    </div>
                    <div id="sticker-visual" class="bg-white border border-gray-200/60 p-3 rounded-2xl flex flex-col items-center justify-center text-center opacity-40 transition-all shadow-xs">
                        <div class="w-12 h-12 bg-indigo-50 rounded-full flex items-center justify-center text-2xl shadow-inner border border-indigo-100">📸</div>
                        <h4 id="stkTitle-visual" class="font-extrabold text-xs mt-1.5 text-gray-800">Visual Explorer</h4>
                        <p class="text-[9px] text-gray-400 mt-0.5" id="visualProgressLabel">0/5 Checks</p>
                        <span class="text-[10px] font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1" id="stkStatus-visual">Locked</span>
                    </div>
                    <div id="sticker-resource" class="bg-white border border-gray-200/60 p-3 rounded-2xl flex flex-col items-center justify-center text-center opacity-40 transition-all shadow-xs">
                        <div class="w-12 h-12 bg-teal-50 rounded-full flex items-center justify-center text-2xl shadow-inner border border-teal-100">🧠</div>
                        <h4 id="stkTitle-resource" class="font-extrabold text-xs mt-1.5 text-gray-800">Resourceful</h4>
                        <p class="text-[9px] text-gray-400 mt-0.5" id="resourceProgressLabel">0/5 Spotlights</p>
                        <span class="text-[10px] font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1" id="stkStatus-resource">Locked</span>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-2 gap-4">
                <div id="sticker-1" class="bg-white border border-gray-200/60 p-4 rounded-3xl flex flex-col items-center justify-center text-center opacity-40 transition-all duration-300 shadow-xs">
                    <div class="w-16 h-16 bg-amber-50 rounded-full flex items-center justify-center text-3xl shadow-inner border border-amber-100">🐂</div>
                    <h4 id="stkTitle-1" class="font-extrabold text-sm mt-2 text-gray-800">Toro Pride</h4>
                    <span class="text-xxs font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1" id="stkStatus-1">Locked</span>
                </div>
                <div id="sticker-2" class="bg-white border border-gray-200/60 p-4 rounded-3xl flex flex-col items-center justify-center text-center opacity-40 transition-all duration-300 shadow-xs">
                    <div class="w-16 h-16 bg-blue-50 rounded-full flex items-center justify-center text-3xl shadow-inner border border-blue-100">🛡️</div>
                    <h4 id="stkTitle-2" class="font-extrabold text-sm mt-2 text-gray-800">Cyber Guard</h4>
                    <span class="text-xxs font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1" id="stkStatus-2">Locked</span>
                </div>
                <div id="sticker-3" class="bg-white border border-gray-200/60 p-4 rounded-3xl flex flex-col items-center justify-center text-center opacity-40 transition-all duration-300 shadow-xs">
                    <div class="w-16 h-16 bg-emerald-50 rounded-full flex items-center justify-center text-3xl shadow-inner border border-emerald-100">🧪</div>
                    <h4 id="stkTitle-3" class="font-extrabold text-sm mt-2 text-gray-800">STEM Pioneer</h4>
                    <span class="text-xxs font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1" id="stkStatus-3">Locked</span>
                </div>
                <div id="sticker-4" class="bg-white border border-gray-200/60 p-4 rounded-3xl flex flex-col items-center justify-center text-center opacity-40 transition-all duration-300 shadow-xs">
                    <div class="w-16 h-16 bg-purple-50 rounded-full flex items-center justify-center text-3xl shadow-inner border border-purple-100">🏺</div>
                    <h4 id="stkTitle-4" class="font-extrabold text-sm mt-2 text-gray-800">Time Traveler</h4>
                    <span class="text-xxs font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1" id="stkStatus-4">Locked</span>
                </div>
                <div id="sticker-5" class="col-span-2 bg-white border border-gray-200/60 p-4 rounded-3xl flex flex-col items-center justify-center text-center opacity-40 transition-all duration-300 shadow-xs">
                    <div class="w-16 h-16 bg-rose-50 rounded-full flex items-center justify-center text-3xl shadow-inner border border-rose-100">👥</div>
                    <h4 id="stkTitle-5" class="font-extrabold text-sm mt-2 text-gray-800">Community Leader</h4>
                    <span class="text-xxs font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1" id="stkStatus-5">Locked</span>
                </div>
            </div>

            <div id="goldTicket" class="bg-gradient-to-r from-toro-gold via-yellow-300 to-toro-gold border border-toro-goldDark/30 p-5 rounded-3xl flex items-center justify-between shadow-md relative overflow-hidden group opacity-30 transition-all duration-300 mt-2">
                <div class="absolute -right-6 -bottom-6 w-16 h-16 bg-white/20 rounded-full blur-md"></div>
                <div>
                    <h4 id="ticketTitle" class="font-extrabold text-base text-toro-crimson flex items-center gap-1">🎟️ College Access Card</h4>
                    <p id="ticketDesc" class="text-xxs text-amber-950 font-medium max-w-[200px] mt-0.5">Unlock at least 3 out of 5 stop badges to prove you are ready for Toro College Life!</p>
                </div>
                <span id="ticketStatus" class="font-black text-toro-crimson border-2 border-toro-crimson/50 rounded-lg px-2.5 py-1 text-xs uppercase rotate-12">LOCKED</span>
            </div>
        </div>

        <!-- SCREEN 5: TORO FUN HUBS (Jokes, Trivia, & Fortunes) -->
        <div id="wisdomScreen" class="hidden space-y-4 relative z-10">
            <div class="text-center py-2">
                <span class="text-4xl animate-bounce block">🎭</span>
                <h2 id="wisdomHeader" class="text-2xl font-black text-toro-crimson mt-1">Toro Fun Hub</h2>
                <p id="wisdomSub" class="text-xs text-gray-500">Tap to reveal interactive campus fun, bilingual jokes, and fortunes!</p>
            </div>

            <div class="flex bg-gray-100 p-1.5 rounded-2xl border border-gray-200/50">
                <button onclick="setFunHubTab('joke')" id="funTab-joke" class="flex-1 text-center py-2 text-xs font-bold rounded-xl transition bg-white text-toro-crimson shadow-xs focus:outline-none">
                    😂 <span id="funTabName-joke">Joke Generator</span>
                </button>
                <button onclick="setFunHubTab('fact')" id="funTab-fact" class="flex-1 text-center py-2 text-xs font-bold rounded-xl transition text-gray-500 focus:outline-none">
                    💡 <span id="funTabName-fact">Fun Facts</span>
                </button>
                <button onclick="setFunHubTab('fortune')" id="funTab-fortune" class="flex-1 text-center py-2 text-xs font-bold rounded-xl transition text-gray-500 focus:outline-none">
                    🔮 <span id="funTabName-fortune">Guidance</span>
                </button>
            </div>

            <div id="funHubCentralCard" class="bg-white border-2 border-dashed border-toro-gold hover:border-solid hover:shadow-lg p-6 rounded-3xl text-center transition-all w-full py-8 flex flex-col items-center justify-center relative min-h-[240px]">
                <div id="funHubCardContent" class="space-y-3 z-10 animate-pop">
                    <!-- Injected dynamically based on sub-tab -->
                </div>
            </div>

            <button id="funHubActionBtn" onclick="triggerFunHubGeneration()" class="bg-toro-crimson hover:bg-red-950 text-white font-extrabold text-xs px-6 py-4 rounded-2xl shadow-md transition flex items-center justify-center gap-1.5 transform active:scale-95 w-full">
                ✨ <span id="funHubActionLabel">Generate Toro Joke!</span>
            </button>
        </div>

    </main>

    <!-- Bottom Navigation Bar (5 Tabs) -->
    <nav class="bg-white border-t border-gray-100 fixed bottom-0 left-0 right-0 z-50 py-2 shadow-[0_-4px_6px_-1px_rgba(0,0,0,0.05)]">
        <div class="max-w-md mx-auto flex justify-between items-center px-2">
            <button onclick="switchTab('quests')" id="tab-quests" class="flex-1 flex flex-col items-center justify-center text-toro-crimson transition transform active:scale-90 focus:outline-none">
                <span class="text-xl">🎯</span>
                <span class="text-[10px] font-black mt-1" id="navQuestLabel">Quests</span>
            </button>
            <button onclick="switchTab('bingo')" id="tab-bingo" class="flex-1 flex flex-col items-center justify-center text-gray-400 hover:text-toro-crimson transition transform active:scale-90 focus:outline-none relative">
                <span class="text-xl">🎰</span>
                <span class="text-[10px] font-black mt-1" id="navBingoLabel">Bingo</span>
                <span id="bingoAlertDot" class="hidden absolute top-0 right-3 w-2.5 h-2.5 bg-toro-gold rounded-full border border-white animate-ping"></span>
            </button>
            <button onclick="switchTab('resources')" id="tab-resources" class="flex-1 flex flex-col items-center justify-center text-gray-400 hover:text-toro-crimson transition transform active:scale-90 focus:outline-none">
                <span class="text-xl">🏫</span>
                <span class="text-[10px] font-black mt-1" id="navResourceLabel">Resources</span>
            </button>
            <button onclick="switchTab('wisdom')" id="tab-wisdom" class="flex-1 flex flex-col items-center justify-center text-gray-400 hover:text-toro-crimson transition transform active:scale-90 focus:outline-none">
                <span class="text-xl">🎭</span>
                <span class="text-[10px] font-black mt-1" id="navFortuneLabel">Fun Hub</span>
            </button>
            <button onclick="switchTab('bag')" id="tab-bag" class="flex-1 flex flex-col items-center justify-center text-gray-400 hover:text-toro-crimson transition transform active:scale-90 focus:outline-none relative">
                <span class="text-xl">🎒</span>
                <span class="text-[10px] font-black mt-1" id="navBagLabel">Badges</span>
                <span id="badgeCounter" class="hidden absolute top-0.5 right-2 bg-toro-crimson text-white text-[9px] font-extrabold w-4 h-4 rounded-full flex items-center justify-center border border-white">0</span>
            </button>
        </div>
    </nav>

    <!-- Celebration Alert Modal -->
    <div id="questSuccessModal" class="hidden fixed inset-0 bg-black/60 backdrop-blur-xs z-[60] flex justify-center items-center p-4">
        <div class="bg-white rounded-3xl p-6 max-w-sm w-full shadow-2xl border-t-8 border-toro-crimson text-center space-y-4 transform transition-all scale-100">
            <span id="successIcon" class="text-6xl block animate-bounce">🌟</span>
            <h3 id="successTitle" class="text-xl font-extrabold text-toro-crimson">Quest Cleared!</h3>
            <p id="successDesc" class="text-sm text-gray-500 font-medium">Correct! You successfully located Welch Courtyard and completed your challenge.</p>
            <div class="bg-amber-50 p-2.5 rounded-xl text-toro-goldDark font-black text-xs inline-block animate-pulse" id="xpAwardLabel">
                +100 Quest XP Earned!
            </div>
            <button onclick="dismissSuccessAlert()" class="bg-toro-crimson hover:bg-red-950 text-white font-extrabold text-sm py-3 px-6 rounded-2xl transition shadow-md w-full focus:outline-none" id="successClaimBtn">
                Continue
            </button>
        </div>
    </div>

    <script>
        // Adjust these to ensure the bull is floating visibly inside the container for mobile views
        const bullPositions = [
            { top: '180px', right: '15px' },    // Stop 1
            { top: '450px', left: '10px' },     // Stop 2
            { top: '300px', right: '10px' },    // Stop 3
            { top: '550px', left: '15px' },     // Stop 4
            { top: '220px', left: '40%' }       // Stop 5
        ];

        // Data & Translations
        const dataLang = {
            en: {
                langBtn: "🇲🇽 Español",
                studentRank: "Recruit",
                navQuestLabel: "Quests",
                navBingoLabel: "Bingo",
                navResourceLabel: "Resources",
                navFortuneLabel: "Fun Hub",
                navBagLabel: "Badges",
                bagTitle: "My Toro Sticker Book",
                bagDesc: "Pass stop comprehension checks to earn stickers! Score at least 3 out of 5 stop badges to unlock the overall Golden Ticket!",
                ticketTitle: "🎟️ College Access Card",
                ticketDesc: "Unlock at least 3 out of 5 stop badges to prove you are ready for Toro College Life!",
                wisdomHeader: "Toro Fun Hub",
                wisdomSub: "Tap to reveal interactive campus fun, bilingual jokes, and fortunes!",
                generateWisdomBtnLabel: "Generate Toro Joke!",
                pronounceSub: "Tap to hear academic terms spoken clearly with helpful pacing",
                listenSlowly: "Listen Now",
                questStatusActive: "Active Quest",
                questStatusDone: "Completed!",
                badgeCounter: "Badges",
                pathLabel: "Your Live Track",
                congratsText: "Comprehension Mastered! 🎉",
                claimText: "Claim Badge & Continue",
                ticketLocked: "LOCKED",
                ticketUnlocked: "UNLOCKED",
                galleryLabel: "Visual Check",
                quizStepCount: "Question {X} of 5",
                quizReqLabel: "Score 3/5 to Pass!",
                retryBtnText: "Retry Quiz 🔄",
                passBtnText: "Collect Badge 🏆",
                specialBadgesTitle: "Special Badges",
                stkTitleBingo: "Bingo Champ",
                stkTitleBull: "Eagle Eye",
                stkTitleVisual: "Visual Explorer",
                stkTitleResource: "Resourceful",
                stkLocked: "Locked",
                stkUnlocked: "Unlocked!",
                eldGuideLabel: "ELD Guide",
                searchPlaceholder: "Search services...",
                bingoScreenTitle: "Toro Bingo Challenge",
                bingoScreenDesc: "Find these items during the tour and tap them. Get a blackout (all squares) to win the badge!",
                bingoClaimBtn: "Claim Bingo Badge!",
                bingoWinDesc: "You found every single item on the campus tour! The Bingo Champion badge is yours.",
                resetBingoBtn: "Reset Bingo Board",
                photoQuizHeader: "What is shown in this picture?",
                photoCorrect: "Correct! Well done.",
                photoWrong: "Not quite. Look closer at the details!",
                triesLabel: "Tries:",
                resourceSpotlightLabel: "Resource Spotlight",
                resourceCorrect: "Correct! Resource check passed.",
                resourceWrong: "Incorrect. Read the description and try again!",
                visualBadgeQuote: "You have a great eye for detail! Navigating a new campus is the first step to owning your educational journey. 📸",
                resourceBadgeQuote: "Knowledge is the key that unlocks all doors, and resourcefulness is knowing which door to open first! 🌟",
                stops: {
                    1: {
                        name: "Welch Courtyard",
                        title: "Welch Hall Challenge",
                        subtitle: "📍 Welch Hall Courtyard",
                        goal: "Listen carefully to the guide, then answer the questions below. Score at least 3/5 to pass!",
                        terms: [
                            { word: "Mascot", trans: "Mascota (School animal/symbol)" },
                            { word: "Courtyard", trans: "Patio (Outdoor gathering area)" }
                        ],
                        photoQuiz: {
                            url: "https://images.unsplash.com/photo-1541339907198-e08756dedf3f?auto=format&fit=crop&w=600&q=80",
                            options: ["Welch Hall (Administration)", "The Science Lab", "The Student Union"],
                            correct: 0
                        },
                        resourceQuiz: {
                            title: "Office of Financial Aid",
                            desc: "Located on the 1st floor of Welch Hall, this office helps students apply for free government grants and scholarships to pay for college.",
                            q: "What free service in Welch Hall helps you pay for college?",
                            opts: ["The Cafeteria", "Office of Financial Aid", "The Computer Lab"],
                            correct: 1
                        },
                        questions: [
                            { q: "What historical land grant is CSUDH located on?", opts: ["Rancho San Pedro 🏡", "Rancho Los Cerritos 🌳", "Rancho Palos Verdes 🌊"], correct: 0 },
                            { q: "What famous historic milestone took place on these grounds in 1910?", opts: ["The first international air show in America ✈️", "The founding of Los Angeles ☀️", "The discovery of California gold 🪙"], correct: 0 },
                            { q: "Why was the 'Toro' (bull) chosen as the university mascot?", opts: ["It was decided by a drawing contest 🎨", "It was selected by students in the late 1960s to represent power and determination 🐂", "It was gifted by a local ranch 🐄"], correct: 1 },
                            { q: "Which key student services are located on the first floor of Welch Hall?", opts: ["The dining hall & buffet 🍕", "Office of Financial Aid and Admissions Hub 🎓", "The science testing laboratories 🧪"], correct: 1 },
                            { q: "What administrative office is located in Room 310 of Welch Hall?", opts: ["Educational Opportunity Program (EOP) counseling 🤝", "The library helpdesk 📖", "Toro Zone arcade lounge 🎮"], correct: 0 }
                        ]
                    },
                    2: {
                        name: "I&I Building",
                        title: "Business & Cybersecurity",
                        subtitle: "📍 Innovation & Instruction (Building #4)",
                        goal: "Check out the high-tech classrooms, then answer these 5 questions about I&I!",
                        terms: [
                            { word: "Cybersecurity", trans: "Ciberseguridad (Digital safety)" },
                            { word: "Innovation", trans: "Innovación (New smart ideas)" }
                        ],
                        photoQuiz: {
                            url: "https://images.unsplash.com/photo-1531403009284-440f080d1e12?auto=format&fit=crop&w=600&q=80",
                            options: ["A cafeteria kitchen", "A modern tech/business lab", "A library reading room"],
                            correct: 1
                        },
                        resourceQuiz: {
                            title: "Career Center",
                            desc: "The Career Center in Welch 200 assists students with building professional resumes, practicing mock interviews, and finding jobs.",
                            q: "Where can you get help building a professional resume and finding jobs?",
                            opts: ["The Testing Center", "The Gym", "The Career Center"],
                            correct: 2
                        },
                        questions: [
                            { q: "Which college is primarily housed inside the I&I building?", opts: ["College of Business Administration and Public Policy 📈", "College of Arts and Humanities 🎭", "College of Natural Sciences 🧬"], correct: 0 },
                            { q: "What specific practical training laboratory is featured inside the business center?", opts: ["A real-world financial trading stock room 📊", "A chemistry test lab 🧪", "An indoor botanical greenhouse 🌿"], correct: 0 },
                            { q: "What modern security concept is studied in the tech wings of I&I?", opts: ["Biometric fingerprint door locks 🚪", "Cybersecurity and protecting digital bank networks 🛡️", "Physical security fencing 🧱"], correct: 1 },
                            { q: "How are classrooms in the I&I building structured to feel professional?", opts: ["They resemble forest campsites 🌳", "They look like glass-walled corporate offices and conference rooms 💼", "They resemble standard testing rooms 📝"], correct: 1 },
                            { q: "What key skill is practiced by students in the cybersecurity programs here?", opts: ["Building physical desktop computers 🛠️", "Defending computer networks from virtual security hacks 🛡️", "Graphic design software 🎨"], correct: 1 }
                        ]
                    },
                    3: {
                        name: "Science Complex",
                        title: "Science & Innovation",
                        subtitle: "📍 Science & Innovation (Building #5)",
                        goal: "Verify what you learned at the discovery laboratories! Get at least 3 correct.",
                        terms: [
                            { word: "STEM", trans: "STEM (Science and Math fields)" },
                            { word: "Laboratory", trans: "Laboratorio (Testing room)" }
                        ],
                        photoQuiz: {
                            url: "https://images.unsplash.com/photo-1581091226825-a6a2a5aee158?auto=format&fit=crop&w=600&q=80",
                            options: ["A telescope", "An optical microscope", "A time capsule"],
                            correct: 1
                        },
                        resourceQuiz: {
                            title: "Teddy's Pantry",
                            desc: "Located in COE #14, Teddy's Pantry provides free emergency groceries and personal toiletries to students experiencing food insecurity.",
                            q: "If a student needs emergency groceries for free, where should they go?",
                            opts: ["Teddy's Pantry", "The Science Lab", "The Library"],
                            correct: 0
                        },
                        questions: [
                            { q: "What does the abbreviation 'SCI' represent on the campus map?", opts: ["Social Community Institute 👥", "Science & Innovation Building 🧬", "Student Counseling Center 🛠️"], correct: 1 },
                            { q: "Which scientific branches are focused on in the advanced laboratories here?", opts: ["Astronomy and telescope engineering 🪐", "Biology, Chemistry, and Physics experiments 🔬", "Music theory and composition 🎵"], correct: 1 },
                            { q: "What is unique about the research opportunities for students in the SCI building?", opts: ["They can shadow professors in high-tech research labs 🥼", "They only read from traditional textbooks 📚", "They test physical sports gear 👟"], correct: 0 },
                            { q: "What type of special high-powered tools do biology students use in these labs?", opts: ["Magnifying glasses 🔎", "Scientific optical microscopes 🔬", "Astronomical telescopes 🔭"], correct: 1 },
                            { q: "What is a 'cleanroom' used for in the Science Complex?", opts: ["Storing dining equipment 🍽️", "A dust-free room to perform delicate nano-technology or biological tests 🧪", "Washing hands and tools 🧼"], correct: 1 }
                        ]
                    },
                    4: {
                        name: "Cain Library",
                        title: "Heart of Knowledge",
                        subtitle: "📍 Leo F. Cain Library (Building #1)",
                        goal: "Test your library comprehension to earn your Time Traveler badge!",
                        terms: [
                            { word: "Time Capsule", trans: "Cápsula del Tiempo (History box)" },
                            { word: "Resources", trans: "Recursos (Helpful tools for success)" }
                        ],
                        photoQuiz: {
                            url: "https://images.unsplash.com/photo-1521587760476-6c12a4b040da?auto=format&fit=crop&w=600&q=80",
                            options: ["Cain Library study area", "The Toro Zone Arcade", "The main parking lot"],
                            correct: 0
                        },
                        resourceQuiz: {
                            title: "Toro Learning & Testing Center (TLTC)",
                            desc: "Found in Library North C-121, the TLTC provides completely free subject tutoring, writing review, and study skills development.",
                            q: "What resource in the library offers completely free tutoring?",
                            opts: ["The Admissions Hub", "Toro Learning & Testing Center (TLTC)", "The Career Center"],
                            correct: 1
                        },
                        questions: [
                            { q: "Approximately how many books and documents are housed inside the Cain Library?", opts: ["Around 5,000 books 📚", "Over half a million research documents and books 📘", "Only digital files are available 💻"], correct: 1 },
                            { q: "What unique local history collection is preserved in the library archives?", opts: ["The South Bay and Rancho San Pedro historical photography collection 📸", "The history of Spanish literature 📖", "Ancient Roman artifacts 🏺"], correct: 0 },
                            { q: "What year was the famous CSUDH campus time capsule buried?", opts: ["In 1910 during the air show ✈️", "In 1974 during the early campus years 🏺", "In 2000 during the millennium change 📅"], correct: 1 },
                            { q: "In what year is the buried time capsule scheduled to be opened?", opts: ["In the year 2074! ⌛", "Next year 📅", "In the year 3000 🚀"], correct: 0 },
                            { q: "Which key academic service is located in Library North Room C-121?", opts: ["The Toro Computer Lab 💻", "Toro Learning & Testing Center (TLTC) for free tutoring 📖", "The campus store 👕"], correct: 1 }
                        ]
                    },
                    5: {
                        name: "Loker LSU",
                        title: "Student Union",
                        subtitle: "📍 Loker Student Union (Building #17)",
                        goal: "Final comprehension check! Learn about Toro community to finish your quest.",
                        terms: [
                            { word: "Community", trans: "Comunidad (People supporting each other)" },
                            { word: "Student Union", trans: "Unión Estudiantil (Campus hangout building)" }
                        ],
                        photoQuiz: {
                            url: "https://images.unsplash.com/photo-1511512578047-dfb367046420?auto=format&fit=crop&w=600&q=80",
                            options: ["Taking formal exams", "Playing games and socializing", "Meeting with financial aid"],
                            correct: 1
                        },
                        resourceQuiz: {
                            title: "Immigrant Justice Center",
                            desc: "Located in LSU Room 111, this center offers free, confidential legal help for immigrant students and their families.",
                            q: "Which center inside the LSU offers free, confidential legal help for families?",
                            opts: ["Immigrant Justice Center", "The Game Lounge", "Teddy's Pantry"],
                            correct: 0
                        },
                        questions: [
                            { q: "What popular entertainment and arcade area is located inside Loker LSU?", opts: ["The Toro Zone Game Lounge 🎮", "A movie theater 🎬", "An indoor basketball court 🏀"], correct: 0 },
                            { q: "What type of student organization hubs are located inside Loker LSU?", opts: ["The Latinx Cultural Center (La Casita) and Black Resource Center 🏡", "State administrative headquarters 💼", "Only commercial fast food counters 🍕"], correct: 0 },
                            { q: "What does the Loker Student Union offer to help students connect?", opts: ["Over 100 active clubs, student government, and cultural events 👥", "A database of homework answers 💻", "Quiet individual exam rooms 🤫"], correct: 0 },
                            { q: "What supportive success centers are placed side-by-side in Loker rooms 110 & 110A?", opts: ["The computer repair labs 🔧", "La Casita Latinx Center and the Toro Dreamers Success Center 🦋", "Teddy's Pantry food shelves 🍎"], correct: 1 },
                            { q: "What legal resource is housed in Loker LSU Room 111?", opts: ["Free confidential help at the Immigrant Justice Center ⚖️", "The campus security patrol office 👮", "The financial aid scholarship office 🎓"], correct: 0 }
                        ]
                    }
                },
                bingoCells: [
                    { icon: "🐂", label: "Mascot" },
                    { icon: "💻", label: "Computer" },
                    { icon: "🔬", label: "Microscope" },
                    { icon: "📖", label: "Book" },
                    { icon: "🛡️", label: "Free Space", isFree: true },
                    { icon: "💧", label: "Water Bottle" },
                    { icon: "🏺", label: "Time Capsule" },
                    { icon: "🍕", label: "Food Area" },
                    { icon: "👥", label: "Friends" }
                ],
                jokes: [
                    { q: "Why did the Toro bring a ladder to the college tour?", a: "Because they wanted to reach higher education! 🪜" },
                    { q: "What is a Toro's favorite subject in school?", a: "Bull-ogy! 🧬" },
                    { q: "Why are Toro degrees so sharp?", a: "Because they are earned with hard work and determination! 🎓" },
                    { q: "Where do Toros go to sleep on campus?", a: "In the calf-eteria! 🍕" },
                    { q: "Why did the student eat their homework?", a: "Because the professor said it was a piece of cake! 🍰" },
                    { q: "What is a computer's favorite snack?", a: "Micro-chips! 💻" },
                    { q: "How does a scientist freshen their breath?", a: "With experi-mints! 🧪" },
                    { q: "What building has the most stories?", a: "The Leo Cain Library! 📚" },
                    { q: "Why did the Toro sit on the clock?", a: "To be on time for graduation! ⏰" }
                ],
                funFacts: [
                    "CSUDH is situated on the historic Rancho San Pedro, the oldest Spanish land grant in all of California.",
                    "In 1910, the very site where our campus stands hosted the historical First International Aviation Meet in America!",
                    "The campus mascot, the Toro, was chosen directly by students in the late 1960s to represent power and forward drive.",
                    "The Leo F. Cain Library holds over half a million books, digital maps, and historical research documents.",
                    "CSUDH has one of the most diverse student populations in the entire western United States!",
                    "The Innovation & Instruction building was designed to mirror real Silicon Valley tech companies.",
                    "Our campus is built on over 346 acres of land, making it very spacious for walking and sports.",
                    "CSUDH is a proud Hispanic-Serving Institution (HSI) and Minority-Serving Institution (MSI).",
                    "The CSUDH Science Complex features earthquake-safe architecture designed to move with the ground!"
                ],
                fortunes: [
                    "You belong on this campus. Your voice, background, and cultural perspective are incredible strengths!",
                    "First-generation means you are a trailblazer. You are carving an inspiring path for your entire family!",
                    "Never be afraid to ask for directions—every student resource room exists solely to help you grow!",
                    "College is an adventure. Try new topics, build strong friendships, and run toward your ambitions!",
                    "Your potential is endless. Every great scholar started exactly where you are standing today.",
                    "Do not fear failure. In college, mistakes are simply research data for your ultimate success!",
                    "A Toro never gives up. Your resilience will carry you to the graduation stage.",
                    "The library is your sanctuary. Knowledge is the most powerful tool you will ever own.",
                    "You are not alone. The Toro community is a family ready to catch you if you fall."
                ],
                hiddenQualities: [
                    { title: "Curiosity Unlocked! 🧠", desc: "You found the hidden Toro because you are curious! Curiosity is the spark that drives college research and new discoveries." },
                    { title: "Determination Found! 💪", desc: "You didn't give up looking! Determination is the exact quality that helps first-generation students overcome obstacles and graduate." },
                    { title: "Observation Mastered! 👁️", desc: "You notice the small details! Paying close attention to your surroundings will make you an excellent university scholar." },
                    { title: "Courage Discovered! 🦁", desc: "It takes courage to explore the unknown. That same courage will help you ask questions and succeed in your future classes." },
                    { title: "Ambition Achieved! 🚀", desc: "You found all 5! Your drive to complete this challenge shows you have the ambition needed to earn a college degree! Eagle Eye badge unlocked!" }
                ]
            },
            es: {
                langBtn: "🇺🇸 English",
                studentRank: "Recluta",
                navQuestLabel: "Misiones",
                navBingoLabel: "Bingo",
                navResourceLabel: "Servicios",
                navFortuneLabel: "Diversión",
                navBagLabel: "Insignias",
                bagTitle: "Mi Colección de Insignias",
                bagDesc: "¡Pasa las pruebas de comprensión para ganar estampas! ¡Consigue al menos 3 insignias para desbloquear el Pase Universitario Dorado!",
                ticketTitle: "🎟️ Pase Universitario Dorado",
                ticketDesc: "¡Desbloquea al menos 3 de las 5 insignias de parada para demostrar que estás listo para la universidad!",
                wisdomHeader: "Centro de Diversión",
                wisdomSub: "¡Presiona abajo para revelar datos interesantes, chistes de Toros y fortunas!",
                generateWisdomBtnLabel: "¡Generar Chiste de Toro!",
                pronounceSub: "Presiona para escuchar los términos académicos pronunciados claramente",
                listenSlowly: "Escuchar ahora",
                questStatusActive: "Búsqueda Activa",
                questStatusDone: "¡Completado!",
                badgeCounter: "Logros",
                pathLabel: "Tu Camino",
                congratsText: "¡Comprensión Dominada! 🎉",
                claimText: "Reclamar insignia y continuar",
                ticketLocked: "BLOQUEADO",
                ticketUnlocked: "DESBLOQUEADO",
                galleryLabel: "Revisión Visual",
                photoQuizHeader: "¿Qué se muestra en esta imagen?",
                photoCorrect: "¡Correcto! Muy bien hecho.",
                photoWrong: "Casi. ¡Observa mejor los detalles!",
                triesLabel: "Intentos:",
                resourceSpotlightLabel: "Foco de Recursos",
                resourceCorrect: "¡Correcto! Prueba de recurso aprobada.",
                resourceWrong: "Incorrecto. ¡Lee la descripción e intenta de nuevo!",
                visualBadgeQuote: "¡Tienes un gran ojo para los detalles! Navegar un campus nuevo es el primer paso para adueñarte de tu camino educativo. 📸",
                resourceBadgeQuote: "¡El conocimiento es la llave que abre todas las puertas, y ser recursivo es saber qué puerta abrir primero! 🌟",
                quizStepCount: "Pregunta {X} de 5",
                quizReqLabel: "¡Saca 3/5 para pasar!",
                retryBtnText: "Reintentar Prueba 🔄",
                passBtnText: "Colectar Insignia 🏆",
                specialBadgesTitle: "Insignias Especiales",
                stkTitleBingo: "Campeón Bingo",
                stkTitleBull: "Ojo de Águila",
                stkTitleVisual: "Explorador Visual",
                stkTitleResource: "Recursivo",
                stkLocked: "Bloqueado",
                stkUnlocked: "¡Desbloqueado!",
                eldGuideLabel: "Guía ELD",
                searchPlaceholder: "Buscar servicios...",
                bingoScreenTitle: "Reto Bingo Toro",
                bingoScreenDesc: "Encuentra estos elementos durante el recorrido y tócalos. ¡Llena todo el cartón para ganar la insignia!",
                bingoClaimBtn: "¡Reclamar Insignia Bingo!",
                bingoWinDesc: "¡Encontraste todos los elementos del recorrido! La insignia de Campeón Bingo es tuya.",
                resetBingoBtn: "Reiniciar Tablero de Bingo",
                stops: {
                    1: {
                        name: "Patio Welch",
                        title: "Desafío Welch Hall",
                        subtitle: "📍 Patio de Welch Hall",
                        goal: "Escucha con atención al guía, luego responde las preguntas. ¡Necesitas 3/5 correctas para pasar!",
                        terms: [
                            { word: "Mascot", trans: "Mascota (Animal de la escuela)" },
                            { word: "Courtyard", trans: "Patio (Área al aire libre)" }
                        ],
                        photoQuiz: {
                            url: "https://images.unsplash.com/photo-1541339907198-e08756dedf3f?auto=format&fit=crop&w=600&q=80",
                            options: ["Welch Hall (Administración)", "El Laboratorio de Ciencias", "La Unión Estudiantil"],
                            correct: 0
                        },
                        resourceQuiz: {
                            title: "Oficina de Ayuda Financiera",
                            desc: "Ubicada en el primer piso de Welch Hall, esta oficina ayuda a los estudiantes a solicitar becas y ayudas gratuitas del gobierno.",
                            q: "¿Qué servicio gratuito en Welch Hall te ayuda a pagar la universidad?",
                            opts: ["La Cafetería", "Oficina de Ayuda Financiera", "El Laboratorio de Cómputo"],
                            correct: 1
                        },
                        questions: [
                            { q: "¿En qué histórica concesión de tierras se encuentra ubicada CSUDH?", opts: ["Rancho San Pedro 🏡", "Rancho Los Cerritos 🌳", "Rancho Palos Verdes 🌊"], correct: 0 },
                            { q: "¿Qué famoso hito histórico tuvo lugar en estos terrenos en 1910?", opts: ["El primer espectáculo aéreo internacional en América ✈️", "La fundación de Los Ángeles ☀️", "El descubrimiento del oro de California 🪙"], correct: 0 },
                            { q: "¿Por qué se eligió al 'Toro' como la mascota de la universidad?", opts: ["Se decidió por un concurso de dibujo 🎨", "Fue seleccionado por los estudiantes a finales de los años 60 para representar poder y determinación 🐂", "Fue un regalo de un rancho local 🐄"], correct: 1 },
                            { q: "¿Qué servicios estudiantiles clave se encuentran en el primer piso de Welch Hall?", opts: ["El comedor universitario 🍕", "La Oficina de Ayuda Financiera y de Admisiones 🎓", "Los laboratorios de pruebas científicas 🧪"], correct: 1 },
                            { q: "¿Qué oficina administrativa se encuentra en el salón 310 de Welch Hall?", opts: ["La consejería del Programa de Oportunidad Educativa (EOP) 🤝", "El mostrador de ayuda de la biblioteca 📖", "La sala de juegos Toro Zone 🎮"], correct: 0 }
                        ]
                    },
                    2: {
                        name: "Edificio I&I",
                        title: "Negocios y Ciberseguridad",
                        subtitle: "📍 Innovación e Instrucción (I&I)",
                        goal: "¡Observa las aulas de alta tecnología, luego responde estas 5 preguntas sobre I&I!",
                        terms: [
                            { word: "Cybersecurity", trans: "Ciberseguridad (Seguridad digital)" },
                            { word: "Innovation", trans: "Innovación (Nuevas ideas)" }
                        ],
                        photoQuiz: {
                            url: "https://images.unsplash.com/photo-1531403009284-440f080d1e12?auto=format&fit=crop&w=600&q=80",
                            options: ["Una cocina de cafetería", "Un laboratorio moderno tecnológico", "Una sala de lectura"],
                            correct: 1
                        },
                        resourceQuiz: {
                            title: "Centro de Carreras Profesionales",
                            desc: "El Centro de Carreras en Welch 200 asiste a los estudiantes a crear currículums profesionales, practicar entrevistas y encontrar empleos.",
                            q: "¿Dónde puedes obtener ayuda para crear tu currículum y encontrar empleo?",
                            opts: ["Centro de Pruebas", "El Gimnasio", "Centro de Carreras Profesionales"],
                            correct: 2
                        },
                        questions: [
                            { q: "¿Qué facultad reside principalmente dentro del edificio I&I?", opts: ["La Facultad de Administración de Negocios y Políticas Públicas 📈", "La Facultad de Artes y Humanidades 🎭", "La Facultad de Ciencias Naturales 🧬"], correct: 0 },
                            { q: "¿Qué laboratorio de entrenamiento práctico se encuentra en el centro de negocios?", opts: ["Una sala real de simulación de transacciones de bolsa 📊", "Un laboratorio de pruebas químicas 🧪", "Un invernadero botánico interior 🌿"], correct: 0 },
                            { q: "¿Qué concepto moderno de seguridad se estudia en las áreas tecnológicas de I&I?", opts: ["Cerraduras biométricas de puertas 🚪", "Ciberseguridad y protección de redes bancarias digitales 🛡️", "Vallas de seguridad física 🧱"], correct: 1 },
                            { q: "¿Cómo están estructuradas las aulas de I&I para sentirse profesionales?", opts: ["Parecen campamentos forestales 🌳", "Tienen paredes de cristal que simulan oficinas corporativas reales 💼", "Parecen aulas de examen tradicionales 📝"], correct: 1 },
                            { q: "¿Qué habilidad clave practican los estudiantes en el programa de ciberseguridad?", opts: ["Ensamblar computadoras de escritorio 🛠️", "Defender redes informáticas contra ataques virtuales 🛡️", "Software de diseño gráfico 🎨"], correct: 1 }
                        ]
                    },
                    3: {
                        name: "Complejo Científico",
                        title: "Ciencias e Innovación",
                        subtitle: "📍 Ciencias e Innovación (SCI)",
                        goal: "¡Demuestra lo aprendido en el laboratorio de descubrimientos! Saca 3 correctas.",
                        terms: [
                            { word: "STEM", trans: "STEM (Ciencia y Matemáticas)" },
                            { word: "Laboratory", trans: "Laboratorio (Sala de ciencias)" }
                        ],
                        photoQuiz: {
                            url: "https://images.unsplash.com/photo-1581091226825-a6a2a5aee158?auto=format&fit=crop&w=600&q=80",
                            options: ["Un telescopio", "Un microscopio óptico", "Una cápsula del tiempo"],
                            correct: 1
                        },
                        resourceQuiz: {
                            title: "La Alacena de Teddy (Teddy's Pantry)",
                            desc: "Ubicada en COE #14, proporciona despensas y artículos de emergencia a los estudiantes que enfrentan inseguridad alimentaria.",
                            q: "Si un estudiante necesita comestibles de emergencia gratis, ¿a dónde debe ir?",
                            opts: ["Teddy's Pantry", "El Laboratorio de Ciencias", "La Biblioteca"],
                            correct: 0
                        },
                        questions: [
                            { q: "¿Qué representa la abreviatura 'SCI' en el mapa del campus?", opts: ["Instituto de Comunidad Social 👥", "Edificio de Ciencias e Innovación 🧬", "Centro de Consejería Estudiantil 🛠️"], correct: 1 },
                            { q: "¿En qué disciplinas científicas se enfocan los laboratorios avanzados de aquí?", opts: ["Astronomía e ingeniería de telescopios 🪐", "Experimentos de Biología, Química y Física 🔬", "Teoría y composición musical 🎵"], correct: 1 },
                            { q: "¿Qué tiene de especial las oportunidades de investigación para alumnos en el edificio SCI?", opts: ["Pueden colaborar con profesores en laboratorios reales 🥼", "Solo leen libros de texto tradicionales 📚", "Prueban equipo deportivo físico 👟"], correct: 0 },
                            { q: "¿Qué tipo de herramientas de alta potencia usan los alumnos de biología en estos laboratorios?", opts: ["Lupas comunes 🔎", "Microscopios ópticos científicos 🔬", "Telescopios astronómicos 🔭"], correct: 1 },
                            { q: "¿Para qué sirve una 'sala limpia' en el Complejo de Ciencias?", opts: ["Guardar utensilios de cocina 🍽️", "Un cuarto libre de polvo para hacer pruebas delicadas de nanotecnología o biología 🧪", "Lavar manos y herramientas 🧼"], correct: 1 }
                        ]
                    },
                    4: {
                        name: "Biblioteca Cain",
                        title: "Corazón del Saber",
                        subtitle: "📍 Biblioteca Leo F. Cain",
                        goal: "¡Prueba tu comprensión de la biblioteca para ganar tu insignia de Viajero!",
                        terms: [
                            { word: "Time Capsule", trans: "Cápsula del Tiempo (Caja de historia)" },
                            { word: "Resources", trans: "Recursos (Herramientas útiles)" }
                        ],
                        photoQuiz: {
                            url: "https://images.unsplash.com/photo-1521587760476-6c12a4b040da?auto=format&fit=crop&w=600&q=80",
                            options: ["Área de estudio Biblioteca Cain", "El arcade Toro Zone", "El estacionamiento principal"],
                            correct: 0
                        },
                        resourceQuiz: {
                            title: "Centro de Aprendizaje y Pruebas (TLTC)",
                            desc: "Ubicado en Biblioteca Norte C-121, el TLTC ofrece tutorías completamente gratuitas, revisión de escritura y habilidades de estudio.",
                            q: "¿Qué recurso en la biblioteca ofrece tutorías completamente gratuitas?",
                            opts: ["Centro de Admisiones", "Centro de Aprendizaje y Pruebas (TLTC)", "El Centro de Carreras"],
                            correct: 1
                        },
                        questions: [
                            { q: "¿La biblioteca es solo un lugar para pedir libros prestados?", opts: ["Sí, los libros son el único recurso 📚", "No, ofrece computadoras, áreas de estudio y tutorías gratis 💻", "No, es principalmente una cafetería de comida 🍕"], correct: 1 },
                            { q: "¿Qué objeto histórico de 1974 está enterrado en el campus?", opts: ["Una cápsula del tiempo 🏺", "Un cofre del tesoro 🪙", "Un auto clásico 🚗"], correct: 0 },
                            { q: "¿Qué es una 'cápsula del tiempo'?", opts: ["Un cohete rápido 🚀", "Un contenedor sellado con objetos históricos para abrir en el futuro ⌛", "Un reloj futurista ⌚"], correct: 1 },
                            { q: "¿Cuándo se abrirá la cápsula de tiempo de 1974?", opts: ["El próximo sábado 📅", "¡En el año 2074! ⌛", "En el año 3000 🚀"], correct: 1 },
                            { q: "¿Qué ayuda gratuita ofrece la biblioteca para mejorar tus calificaciones?", opts: ["Tutoría gratuita de materias y recursos de estudio en la sala TLTC 🛠️", "Ropa gratis 👕", "Almuerzos de pizza gratis 🍕"], correct: 0 }
                        ]
                    },
                    5: {
                        name: "Unión Loker LSU",
                        title: "Unión Estudiantil",
                        subtitle: "📍 Unión Estudiantil Loker",
                        goal: "¡Prueba final! Aprende sobre la comunidad Toro para terminar la búsqueda.",
                        terms: [
                            { word: "Community", trans: "Comunidad (Personas juntas)" },
                            { word: "Student Union", trans: "Unión Estudiantil (Edificio de descanso)" }
                        ],
                        photoQuiz: {
                            url: "https://images.unsplash.com/photo-1511512578047-dfb367046420?auto=format&fit=crop&w=600&q=80",
                            options: ["Tomar exámenes formales", "Jugar y socializar", "Reunirse con ayuda financiera"],
                            correct: 1
                        },
                        resourceQuiz: {
                            title: "Centro de Justicia para Inmigrantes",
                            desc: "Ubicado en LSU Sala 111, este centro ofrece ayuda legal confidencial y gratuita para estudiantes inmigrantes y sus familias.",
                            q: "¿Qué centro dentro de LSU ofrece ayuda legal confidencial para las familias?",
                            opts: ["Centro de Justicia para Inmigrantes", "La Sala de Juegos", "Teddy's Pantry"],
                            correct: 0
                        },
                        questions: [
                            { q: "¿Qué área de juegos populares se encuentra dentro de Loker LSU?", opts: ["La sala de juegos recreativa Toro Zone 🎮", "Un cine completo 🎬", "Una cancha de baloncesto interior 🏀"], correct: 0 },
                            { q: "¿Qué tipo de centros culturales estudiantiles están ubicados en Loker LSU?", opts: ["La Casita (Centro Latinx) y el Black Resource Center 🏡", "Oficinas gubernamentales del estado 💼", "Solo mostradores de comida rápida comercial 🍕"], correct: 0 },
                            { q: "¿Qué ofrece la Unión Estudiantil Loker para ayudar a los estudiantes a conectarse?", opts: ["Más de 100 clubes activos, gobierno estudiantil y eventos culturales 👥", "Una base de datos de respuestas de tareas 💻", "Salas de examen individuales silenciosas 🤫"], correct: 0 },
                            { q: "¿Qué servicios de apoyo están ubicados en las salas 110 y 110A de Loker?", opts: ["Los talleres de reparación de computadoras 🔧", "La Casita y el Toro Dreamers Success Center 🦋", "Los estantes de comida Teddy's Pantry 🍎"], correct: 1 },
                            { q: "¿Qué ayuda legal se ofrece de forma gratuita en el salón 111 de Loker LSU?", opts: ["Asesoría legal confidencial en el Centro de Justicia para Inmigrantes ⚖️", "La oficina de patrulla de seguridad del campus 👮", "La oficina de becas de ayuda financiera 🎓"], correct: 0 }
                        ]
                    }
                },
                bingoCells: [
                    { icon: "🐂", label: "Mascota" },
                    { icon: "💻", label: "Compu" },
                    { icon: "🔬", label: "Microscopio" },
                    { icon: "📖", label: "Libro" },
                    { icon: "🛡️", label: "Centro (Gratis)", isFree: true },
                    { icon: "💧", label: "Agua" },
                    { icon: "🏺", label: "Cápsula" },
                    { icon: "🍕", label: "Comida" },
                    { icon: "👥", label: "Amigos" }
                ],
                jokes: [
                    { q: "¿Por qué el Toro llevó una escalera al recorrido universitario?", a: "¡Porque quería alcanzar la educación superior! 🪜" },
                    { q: "¿Cuál es la materia favorita de un Toro?", a: "¡La Toro-logía! 🧬" },
                    { q: "¿Por qué los títulos de los Toros son tan valiosos?", a: "¡Porque se ganan con esfuerzo, sudor y determinación! 🎓" },
                    { q: "¿Dónde duermen la siesta los Toros en el campus?", a: "¡En la becerro-tería! 🍕" },
                    { q: "¿Por qué el estudiante se comió su tarea?", a: "¡Porque el profesor dijo que era pan comido! 🍰" },
                    { q: "¿Cuál es la merienda favorita de una computadora?", a: "¡Los micro-chips! 💻" },
                    { q: "¿Cómo se refresca el aliento un científico?", a: "¡Con experi-mentas! 🧪" },
                    { q: "¿Qué edificio tiene más historias?", a: "¡La biblioteca Leo Cain! 📚" },
                    { q: "¿Por qué el Toro se sentó en el reloj?", a: "¡Para llegar a tiempo a su graduación! ⏰" }
                ],
                funFacts: [
                    "CSUDH se encuentra situado en el histórico Rancho San Pedro, la concesión de tierras españolas más antigua de toda California.",
                    "¡En 1910, el terreno de nuestro campus albergó el Primer Encuentro Internacional de Aviación en la historia de América!",
                    "La mascota oficial, el Toro, fue elegida por los propios estudiantes a finales de la década de 1960 para representar poder, unión y empuje social.",
                    "La biblioteca Leo F. Cain alberga más de medio millón de libros físicos, mapas y documentos históricos.",
                    "¡CSUDH tiene una de las poblaciones estudiantiles más diversas de todo el oeste de los Estados Unidos!",
                    "El edificio de Innovación e Instrucción fue diseñado para imitar a las empresas tecnológicas de Silicon Valley.",
                    "Nuestro campus está construido sobre más de 346 acres de terreno, lo que lo hace muy espacioso.",
                    "CSUDH es una orgullosa Institución al Servicio de los Hispanos (HSI) y Minorías (MSI).",
                    "El Complejo de Ciencias cuenta con arquitectura antisísmica diseñada para moverse con la tierra."
                ],
                fortunes: [
                    "Tú perteneces a esta universidad. ¡Tu historia, idioma y herencia familiar son superpoderes de éxito!",
                    "Ser de primera generación significa que abres nuevos caminos. ¡Estás forjando un futuro maravilloso!",
                    "Nunca tengas miedo de pedir ayuda—cada salón de servicio existe únicamente para impulsarte.",
                    "La universidad es una aventura de descubrimiento. ¡Explora nuevas ideas, haz buenos amigos y triunfa!",
                    "Tu potencial es infinito. Cada gran académico comenzó exactamente en el lugar donde estás hoy.",
                    "No temas equivocarte. ¡En la universidad los errores son datos de investigación para tu éxito!",
                    "Un Toro nunca se rinde. Tu resistencia te llevará hasta el escenario de graduación.",
                    "La biblioteca es tu refugio. El conocimiento es la herramienta más poderosa que tendrás.",
                    "No estás solo. La comunidad Toro es una familia dispuesta a apoyarte si te caes."
                ],
                hiddenQualities: [
                    { title: "¡Curiosidad Desbloqueada! 🧠", desc: "¡Encontraste el Toro porque eres curioso! La curiosidad es la chispa que impulsa la investigación universitaria." },
                    { title: "¡Determinación Encontrada! 💪", desc: "¡No te rendiste! La determinación es lo que ayuda a los estudiantes a superar obstáculos y llegar a su graduación." },
                    { title: "¡Observación Dominada! 👁️", desc: "¡Notas los pequeños detalles! Prestar atención te convertirá en un excelente estudiante universitario." },
                    { title: "¡Valentía Descubierta! 🦁", desc: "Se necesita valor para explorar lo nuevo. Esa misma valentía te ayudará a triunfar en tus futuras clases." },
                    { title: "¡Ambición Lograda! 🚀", desc: "¡Encontraste los 5! Tu deseo de completar este reto demuestra que tienes la ambición para obtener un título universitario. ¡Insignia Ojo de Águila desbloqueada!" }
                ]
            }
        };

        const supportResources = [
            { nameEn: "Educational Opportunity Program (EOP)", nameEs: "Programa de Oportunidad Educativa (EOP)", loc: "Welch Hall Room 310", hours: "M-F 8AM - 5PM", descEn: "Brings you personal academic advising, admissions assistance, and direct counselor guidance.", descEs: "Le brinda asesoría académica personal, ayuda con la admisión y orientación directa de consejeros.", icon: "🤝" },
            { nameEn: "Office of Financial Aid", nameEs: "Oficina de Ayuda Financiera", loc: "Welch Hall 1st Floor Lobby", hours: "M-F 8AM - 5PM", descEn: "Helps you secure free government grants, scholarships, work-study opportunities.", descEs: "Le ayuda a asegurar subvenciones gubernamentales gratuitas, becas, oportunidades de estudio.", icon: "🎓" },
            { nameEn: "Career Center", nameEs: "Centro de Carreras Profesionales", loc: "Welch Hall Room 200", hours: "M-F 8AM - 5PM", descEn: "Assists with professional resume building, mock interviews, and finding student jobs.", descEs: "Le ayuda a redactar su currículum profesional, practicar entrevistas y encontrar empleos.", icon: "💼" },
            { nameEn: "Toro Learning & Testing Center (TLTC)", nameEs: "Centro de Aprendizaje y Pruebas (TLTC)", loc: "Library North C-121", hours: "M-Th 8AM-6PM, F 8AM-5PM", descEn: "Provides completely free subject tutoring, writing review, and study skills development.", descEs: "Ofrece tutorías de materias completamente gratuitas y desarrollo de habilidades de estudio.", icon: "📖" },
            { nameEn: "Toro Computer Lab", nameEs: "Laboratorio de Computadoras Toro", loc: "Library G-146", hours: "Aligned with Library", descEn: "Open access to high-speed desktop computers, software, and completely free printing.", descEs: "Acceso a computadoras de escritorio, software y créditos de impresión gratis.", icon: "💻" },
            { nameEn: "La Casita (Latinx Center)", nameEs: "La Casita (Centro Latinx)", loc: "LSU Room 110", hours: "M-F 9AM - 5PM", descEn: "A welcoming, family-oriented cultural safe space providing Latinx community mentoring.", descEs: "Un espacio cultural acogedor y familiar que ofrece mentoría para la comunidad Latinx.", icon: "🏡" },
            { nameEn: "Toro Dreamers Success Center", nameEs: "Centro de Éxito Dreamers", loc: "LSU Room 110A", hours: "M-F 8AM - 5PM", descEn: "Specialized academic counseling and CA Dream Act support for undocumented students.", descEs: "Ayuda académica especializada y apoyo del CA Dream Act para estudiantes indocumentados.", icon: "🦋" },
            { nameEn: "Immigrant Justice Center", nameEs: "Centro de Justicia para Inmigrantes", loc: "LSU Room 111", hours: "M-F 9AM - 5PM", descEn: "Confidential and free legal consultations, document support, and immigration pathway assistance.", descEs: "Consultas legales gratuitas, apoyo con documentos y asistencia en procesos de inmigración.", icon: "⚖️" },
            { nameEn: "Rose Black Resource Center", nameEs: "Centro de Recursos Rose Black", loc: "LSU Room 132", hours: "See door", descEn: "Fosters academic excellence, strong peer mentoring, and black student community representation.", descEs: "Fomenta la excelencia académica, mentoría y representación de la comunidad afrodescendiente.", icon: "✊🏿" },
            { nameEn: "Teddy's Pantry", nameEs: "La Alacena de Teddy", loc: "COE Building #14", hours: "M-F 9AM-5PM", descEn: "Supplies completely free emergency food boxes, groceries, and personal toiletries.", descEs: "Proporciona cajas de alimentos de emergencia gratuitas, víveres y artículos de tocador.", icon: "🍎" }
        ];

        // Global States
        let currentLang = 'en';
        let currentStop = 1;
        let currentActiveTab = 'quests';
        let activeFunHubTab = 'joke'; 
        let unlockedStickers = [false, false, false, false, false]; 
        let bingoState = [false, false, false, false, true, false, false, false, false]; 
        let hiddenBullsFound = [false, false, false, false, false]; 
        let xpPoints = 0;
        let currentQuizIndex = 0; 
        let stopAnswersCorrect = 0;
        let isAdvancingFromQuiz = false; 

        // New Visual and Resource Tracker Arrays & Tries
        let visualPassedState = [false, false, false, false, false];
        let resourcePassedState = [false, false, false, false, false];
        let visualTriesState = [3, 3, 3, 3, 3];
        let resourceTriesState = [3, 3, 3, 3, 3];

        window.onload = function() {
            setLanguage('en');
            changeStop(1);
            setFunHubTab('joke');
            renderResourcesList();
            renderBingoGrid();
        };

        function toggleLanguage() {
            setLanguage(currentLang === 'en' ? 'es' : 'en');
        }

        function setLanguage(lang) {
            currentLang = lang;
            const dict = dataLang[lang];

            document.getElementById('langBtn').innerHTML = lang === 'en' ? '🇲🇽 <span>Español</span>' : '🇺🇸 <span>English</span>';
            document.getElementById('studentRank').textContent = xpPoints >= 400 ? (lang === 'en' ? "Gold Champion" : "Campeón Dorado") : (xpPoints >= 200 ? (lang === 'en' ? "Toro Leader" : "Líder Toro") : dict.studentRank);
            
            document.getElementById('navQuestLabel').textContent = dict.navQuestLabel;
            document.getElementById('navBingoLabel').textContent = dict.navBingoLabel;
            document.getElementById('navResourceLabel').textContent = dict.navResourceLabel;
            document.getElementById('navFortuneLabel').textContent = dict.navFortuneLabel;
            document.getElementById('navBagLabel').textContent = dict.navBagLabel;

            document.getElementById('resScreenTitle').textContent = dict.resScreenTitle;
            document.getElementById('resScreenDesc').textContent = dict.resScreenDesc;
            document.getElementById('bagTitle').textContent = dict.bagTitle;
            document.getElementById('bagDesc').textContent = dict.bagDesc;
            document.getElementById('ticketTitle').textContent = dict.ticketTitle;
            document.getElementById('ticketDesc').textContent = dict.ticketDesc;
            document.getElementById('photoQuizBadgeLabel').textContent = dict.galleryLabel;
            document.getElementById('eldGuideLabel').textContent = dict.eldGuideLabel;
            
            document.getElementById('specialBadgesTitle').textContent = dict.specialBadgesTitle;
            document.getElementById('stkTitle-bingo').textContent = dict.stkTitleBingo;
            document.getElementById('stkTitle-bull').textContent = dict.stkTitleBull;
            document.getElementById('stkTitle-visual').textContent = dict.stkTitleVisual;
            document.getElementById('stkTitle-resource').textContent = dict.stkTitleResource;

            document.getElementById('bingoScreenTitle').textContent = dict.bingoScreenTitle;
            document.getElementById('bingoScreenDesc').textContent = dict.bingoScreenDesc;
            document.getElementById('bingoClaimBtn').textContent = dict.bingoClaimBtn;
            document.getElementById('bingoWinDesc').textContent = dict.bingoWinDesc;
            document.getElementById('resetBingoBtn').textContent = dict.resetBingoBtn;
            
            document.getElementById('resourceSearch').placeholder = dict.searchPlaceholder;

            for (let i = 1; i <= 5; i++) {
                updateStickerUI(i);
            }
            updateSpecialStickersUI();

            document.getElementById('funTabName-joke').textContent = dict.funTab_joke;
            document.getElementById('funTabName-fact').textContent = dict.funTab_fact;
            document.getElementById('funTabName-fortune').textContent = dict.funTab_fortune;
            document.getElementById('wisdomHeader').textContent = dict.wisdomHeader;
            document.getElementById('wisdomSub').textContent = dict.wisdomSub;

            document.getElementById('companionPathLabel').textContent = dict.pathLabel;
            document.getElementById('toroTalkerSubtitle').textContent = dict.pronounceSub;
            document.getElementById('successClaimBtn').textContent = dict.claimText;
            
            document.getElementById('photoQuizQuestion').textContent = dict.photoQuizHeader;

            changeStop(currentStop);
            setFunHubTab(activeFunHubTab);
            renderResourcesList();
            renderBingoGrid();
        }

        // Navigate / Slide stop numbers
        function changeStop(stopNum) {
            currentStop = stopNum;
            const dict = dataLang[currentLang];
            const stopInfo = dict.stops[stopNum];

            const percent = ((stopNum - 1) / 4) * 100;
            document.getElementById('pathProgressHighlight').style.width = `${percent}%`;

            currentQuizIndex = 0;
            stopAnswersCorrect = 0;

            for (let i = 1; i <= 5; i++) {
                const node = document.getElementById(`stopNode-${i}`);
                if (i === stopNum) {
                    node.className = "z-10 w-9 h-9 rounded-full bg-toro-crimson text-white text-xs font-black shadow-md flex items-center justify-center transition-all focus:outline-none border-2 border-toro-gold active-pulse";
                } else if (i < stopNum || unlockedStickers[i-1]) {
                    node.className = "z-10 w-8 h-8 rounded-full bg-toro-green text-white text-xs font-black flex items-center justify-center transition-all focus:outline-none";
                } else {
                    node.className = "z-10 w-8 h-8 rounded-full bg-gray-200 text-gray-600 text-xs font-black flex items-center justify-center transition-all focus:outline-none";
                }
            }

            document.getElementById('companionStopName').textContent = stopInfo.name;
            document.getElementById('questStopNum').textContent = `${dict.questStatusActive}: Stop ${stopNum}`;
            document.getElementById('questTitle').textContent = stopInfo.title;
            document.getElementById('questSubtitle').textContent = stopInfo.subtitle;
            document.getElementById('questGoalText').textContent = stopInfo.goal;

            const icons = ["🐂", "💻", "🧪", "🏺", "🍕"];
            document.getElementById('questVisualIcon').textContent = icons[stopNum - 1];

            // Render Visual Check
            renderVisualCheck();

            // Render Resource Spotlight Check
            renderResourceCheck();

            // MULTI-WORD VOICE COACH
            const voiceContainer = document.getElementById('voiceCoachContainer');
            voiceContainer.innerHTML = '';
            stopInfo.terms.forEach(termObj => {
                const termBlock = document.createElement('div');
                termBlock.className = "bg-slate-50 border border-slate-200/50 rounded-2xl p-3 flex items-center justify-between gap-3 shadow-xs";
                termBlock.innerHTML = `
                    <div class="min-w-0">
                        <span class="font-black text-base text-toro-crimson truncate block">${termObj.word}</span>
                        <span class="block text-[10px] text-gray-500 leading-tight truncate">${termObj.trans}</span>
                    </div>
                    <button onclick="speakSpecificWord('${termObj.word}')" class="bg-toro-crimson hover:bg-red-900 text-white rounded-xl p-2.5 shadow-md transition transform active:scale-95 focus:outline-none flex items-center justify-center gap-1 font-bold text-xs shrink-0">
                        <svg class="w-4 h-4 animate-pulse" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" d="M19.114 5.636a9 9 0 010 12.728M16.463 8.288a5.25 5.25 0 010 7.424M6.75 8.25l4.72-4.72a.75.75 0 011.28.53v15.88a.75.75 0 01-1.28.53l-4.72-4.72H4.51c-.88 0-1.704-.507-1.938-1.354A9.01 9.01 0 012.25 12c0-.83.112-1.633.322-2.396C2.806 8.756 3.63 8.25 4.51 8.25H6.75z"></path></svg>
                        ${dict.listenSlowly}
                    </button>
                `;
                voiceContainer.appendChild(termBlock);
            });

            // Handle the Hidden Bull for this specific stop
            const bullEl = document.getElementById('stopHiddenBull');
            if (hiddenBullsFound[stopNum - 1]) {
                bullEl.style.display = 'none';
            } else {
                bullEl.style.display = 'block';
                bullEl.style.top = bullPositions[stopNum - 1].top || 'auto';
                bullEl.style.bottom = bullPositions[stopNum - 1].bottom || 'auto';
                bullEl.style.left = bullPositions[stopNum - 1].left || 'auto';
                bullEl.style.right = bullPositions[stopNum - 1].right || 'auto';
            }

            renderActiveQuestionStep();
        }

        // Dedicated renderer for Visual Photo Check
        function renderVisualCheck() {
            const dict = dataLang[currentLang];
            const stopInfo = dict.stops[currentStop];
            const isPassed = visualPassedState[currentStop - 1];
            const triesLeft = visualTriesState[currentStop - 1];

            document.getElementById('photoQuizImg').src = stopInfo.photoQuiz.url;
            document.getElementById('visualTriesLabel').textContent = `${dict.triesLabel} ${triesLeft}`;
            
            const optsContainer = document.getElementById('photoQuizOptions');
            const resultBox = document.getElementById('visualCheckResult');
            const contentBox = document.getElementById('visualCheckContent');

            optsContainer.innerHTML = '';
            
            if (isPassed) {
                contentBox.classList.add('hidden');
                resultBox.classList.remove('hidden');
                resultBox.innerHTML = `
                    <span class="text-3xl block text-toro-green mb-1">✓</span>
                    <p class="font-black text-sm text-toro-green">${dict.photoCorrect}</p>
                `;
            } else if (triesLeft <= 0) {
                contentBox.classList.add('hidden');
                resultBox.classList.remove('hidden');
                resultBox.innerHTML = `
                    <span class="text-3xl block text-toro-crimson mb-1">🔒</span>
                    <p class="font-bold text-xs text-toro-crimson mb-2">${currentLang === 'en' ? "Out of tries! Review the guide and try again." : "¡Sin intentos! Revisa la guía e intenta de nuevo."}</p>
                    <button onclick="resetVisualTries()" class="bg-gray-200 text-gray-700 px-4 py-2 rounded-xl text-xs font-bold shadow-sm">${dict.retryBtnText}</button>
                `;
            } else {
                contentBox.classList.remove('hidden');
                resultBox.classList.add('hidden');
                stopInfo.photoQuiz.options.forEach((opt, idx) => {
                    const btn = document.createElement('button');
                    btn.className = "w-full text-left bg-gray-50 hover:bg-amber-50 border border-gray-200 rounded-xl p-2.5 text-xs font-bold text-gray-700 transition active:scale-95 focus:outline-none flex items-center justify-between shadow-xs";
                    btn.innerHTML = `<span>${opt}</span> <span class="text-gray-300 font-bold">➔</span>`;
                    btn.onclick = () => evaluatePhotoQuiz(idx, stopInfo.photoQuiz.correct);
                    optsContainer.appendChild(btn);
                });
            }
        }

        function resetVisualTries() {
            visualTriesState[currentStop - 1] = 3;
            renderVisualCheck();
        }

        function evaluatePhotoQuiz(selectedIdx, correctIdx) {
            if (visualPassedState[currentStop - 1] || visualTriesState[currentStop - 1] <= 0) return;

            if (selectedIdx === correctIdx) {
                visualPassedState[currentStop - 1] = true;
                renderVisualCheck();
                checkVisualBadge();
            } else {
                visualTriesState[currentStop - 1]--;
                renderVisualCheck();
                
                // Show brief wrong notice if still has tries
                if (visualTriesState[currentStop - 1] > 0) {
                    const dict = dataLang[currentLang];
                    const resultBox = document.getElementById('visualCheckResult');
                    const contentBox = document.getElementById('visualCheckContent');
                    contentBox.classList.add('hidden');
                    resultBox.classList.remove('hidden');
                    resultBox.innerHTML = `
                        <span class="text-3xl block text-toro-crimson mb-1">✗</span>
                        <p class="font-bold text-xs text-toro-crimson mb-2">${dict.photoWrong}</p>
                        <button onclick="renderVisualCheck()" class="bg-gray-200 text-gray-700 px-4 py-2 rounded-xl text-xs font-bold shadow-sm">${currentLang === 'en' ? "Try Again" : "Intentar de nuevo"}</button>
                    `;
                }
            }
        }

        function checkVisualBadge() {
            const allPassed = visualPassedState.every(Boolean);
            if (allPassed) {
                updateSpecialStickersUI();
                updateBadgeCounter();
                isAdvancingFromQuiz = false;
                showVisualNotification(
                    currentLang === 'en' ? "Visual Explorer Badge Unlocked! 📸" : "¡Insignia Explorador Visual Desbloqueada! 📸",
                    dataLang[currentLang].visualBadgeQuote,
                    "👁️‍🗨️"
                );
            }
        }

        // Dedicated renderer for Resource Spotlight Check
        function renderResourceCheck() {
            const dict = dataLang[currentLang];
            const stopInfo = dict.stops[currentStop];
            const isPassed = resourcePassedState[currentStop - 1];
            const triesLeft = resourceTriesState[currentStop - 1];

            document.getElementById('resourceSpotlightBadgeLabel').textContent = dict.resourceSpotlightLabel;
            document.getElementById('resourceTriesLabel').textContent = `${dict.triesLabel} ${triesLeft}`;
            
            document.getElementById('resourceQuizTitle').textContent = stopInfo.resourceQuiz.title;
            document.getElementById('resourceQuizDesc').textContent = stopInfo.resourceQuiz.desc;
            document.getElementById('resourceQuizQuestion').textContent = stopInfo.resourceQuiz.q;

            const optsContainer = document.getElementById('resourceQuizOptions');
            const resultBox = document.getElementById('resourceCheckResult');
            const contentBox = document.getElementById('resourceQuizContent');

            optsContainer.innerHTML = '';
            
            if (isPassed) {
                contentBox.classList.add('hidden');
                resultBox.classList.remove('hidden');
                resultBox.innerHTML = `
                    <span class="text-3xl block text-toro-green mb-1">✓</span>
                    <p class="font-black text-sm text-toro-green">${dict.resourceCorrect}</p>
                `;
            } else if (triesLeft <= 0) {
                contentBox.classList.add('hidden');
                resultBox.classList.remove('hidden');
                resultBox.innerHTML = `
                    <span class="text-3xl block text-toro-crimson mb-1">🔒</span>
                    <p class="font-bold text-xs text-toro-crimson mb-2">${currentLang === 'en' ? "Out of tries! Review the guide and try again." : "¡Sin intentos! Revisa la guía e intenta de nuevo."}</p>
                    <button onclick="resetResourceTries()" class="bg-gray-200 text-gray-700 px-4 py-2 rounded-xl text-xs font-bold shadow-sm">${dict.retryBtnText}</button>
                `;
            } else {
                contentBox.classList.remove('hidden');
                resultBox.classList.add('hidden');
                stopInfo.resourceQuiz.opts.forEach((opt, idx) => {
                    const btn = document.createElement('button');
                    btn.className = "w-full text-left bg-gray-50 hover:bg-teal-50 border border-gray-200 rounded-xl p-2.5 text-xs font-bold text-gray-700 transition active:scale-95 focus:outline-none flex items-center justify-between shadow-xs";
                    btn.innerHTML = `<span>${opt}</span> <span class="text-gray-300 font-bold">➔</span>`;
                    btn.onclick = () => evaluateResourceQuiz(idx, stopInfo.resourceQuiz.correct);
                    optsContainer.appendChild(btn);
                });
            }
        }

        function resetResourceTries() {
            resourceTriesState[currentStop - 1] = 3;
            renderResourceCheck();
        }

        function evaluateResourceQuiz(selectedIdx, correctIdx) {
            if (resourcePassedState[currentStop - 1] || resourceTriesState[currentStop - 1] <= 0) return;

            if (selectedIdx === correctIdx) {
                resourcePassedState[currentStop - 1] = true;
                renderResourceCheck();
                checkResourceBadge();
            } else {
                resourceTriesState[currentStop - 1]--;
                renderResourceCheck();
                
                if (resourceTriesState[currentStop - 1] > 0) {
                    const dict = dataLang[currentLang];
                    const resultBox = document.getElementById('resourceCheckResult');
                    const contentBox = document.getElementById('resourceQuizContent');
                    contentBox.classList.add('hidden');
                    resultBox.classList.remove('hidden');
                    resultBox.innerHTML = `
                        <span class="text-3xl block text-toro-crimson mb-1">✗</span>
                        <p class="font-bold text-xs text-toro-crimson mb-2">${dict.resourceWrong}</p>
                        <button onclick="renderResourceCheck()" class="bg-gray-200 text-gray-700 px-4 py-2 rounded-xl text-xs font-bold shadow-sm">${currentLang === 'en' ? "Try Again" : "Intentar de nuevo"}</button>
                    `;
                }
            }
        }

        function checkResourceBadge() {
            const allPassed = resourcePassedState.every(Boolean);
            if (allPassed) {
                updateSpecialStickersUI();
                updateBadgeCounter();
                isAdvancingFromQuiz = false;
                showVisualNotification(
                    currentLang === 'en' ? "Resourceful Scholar Badge Unlocked! 🧠" : "¡Insignia Erudito Recursivo Desbloqueada! 🧠",
                    dataLang[currentLang].resourceBadgeQuote,
                    "🎓"
                );
            }
        }


        // Render current quiz step inside quest card
        function renderActiveQuestionStep() {
            const dict = dataLang[currentLang];
            const stopInfo = dict.stops[currentStop];
            const qCountLabel = document.getElementById('quizStepCount');
            const questionPanel = document.getElementById('quizQuestionPanel');

            qCountLabel.textContent = dict.quizStepCount.replace("{X}", currentQuizIndex + 1);

            if (unlockedStickers[currentStop - 1]) {
                questionPanel.innerHTML = `
                    <div class="text-center py-6 space-y-4">
                        <span class="text-5xl">🏆</span>
                        <h4 class="font-extrabold text-toro-crimson text-sm">${dict.questStatusDone}</h4>
                        <p class="text-xs text-gray-500 leading-relaxed max-w-xs mx-auto">
                            ${currentLang === 'en' 
                                ? "You already completed this stop's check-in! Feel free to practice on other locations." 
                                : "¡Ya completaste el registro de esta parada! Siéntete libre de practicar en otros lugares."
                            }
                        </p>
                        <button onclick="advanceToNextStop()" class="bg-toro-crimson hover:bg-red-950 text-white font-extrabold text-xs px-5 py-2.5 rounded-xl transition shadow-md">
                            ${currentLang === 'en' ? "Continue Tour ➔" : "Continuar Recorrido ➔"}
                        </button>
                    </div>
                `;
                return;
            }

            const currentQ = stopInfo.questions[currentQuizIndex];

            questionPanel.innerHTML = `
                <div class="bg-slate-50 border border-slate-100 rounded-2xl p-4 shadow-inner space-y-3">
                    <p class="text-sm font-black text-slate-800 leading-snug">${currentQ.q}</p>
                    <div class="space-y-2" id="quizAnswersBlock"></div>
                </div>
            `;

            const ansBlock = document.getElementById('quizAnswersBlock');
            currentQ.opts.forEach((opt, idx) => {
                const btn = document.createElement('button');
                btn.className = "w-full text-left bg-white hover:bg-amber-50 border border-gray-200 rounded-xl p-3 text-xs font-bold text-gray-700 transition active:scale-98 focus:outline-none flex items-center justify-between shadow-xs";
                btn.innerHTML = `<span>${opt}</span> <span class="text-gray-300 font-bold">➔</span>`;
                btn.onclick = () => evaluateStepSelection(idx, currentQ.correct, btn);
                ansBlock.appendChild(btn);
            });
        }

        function evaluateStepSelection(selectedIdx, correctIdx, btnElement) {
            const ansBlock = document.getElementById('quizAnswersBlock');
            const btns = ansBlock.querySelectorAll('button');

            btns.forEach(b => b.disabled = true);

            const isCorrect = selectedIdx === correctIdx;
            if (isCorrect) {
                stopAnswersCorrect++;
                btnElement.className = "w-full text-left bg-emerald-50 border-2 border-toro-green text-toro-green rounded-xl p-3 text-xs font-black transition flex items-center justify-between shadow-sm";
                btnElement.innerHTML = `<span>${btnElement.innerText.replace('➔', '')}</span> <span class="text-toro-green text-lg">✓</span>`;
            } else {
                btnElement.className = "w-full text-left bg-red-50 border-2 border-toro-crimson text-toro-crimson rounded-xl p-3 text-xs font-black transition flex items-center justify-between shadow-sm";
                btnElement.innerHTML = `<span>${btnElement.innerText.replace('➔', '')}</span> <span class="text-toro-crimson text-lg">✗</span>`;

                const correctBtn = btns[correctIdx];
                correctBtn.className = "w-full text-left bg-emerald-50 border border-toro-green/50 text-toro-green rounded-xl p-3 text-xs font-bold transition flex items-center justify-between shadow-xs";
                correctBtn.innerHTML = `<span>${correctBtn.innerText.replace('➔', '')}</span> <span class="text-toro-green">✓</span>`;
            }

            const actionDiv = document.createElement('div');
            actionDiv.className = "pt-3 flex justify-end";
            
            const isLast = currentQuizIndex === 4;
            const nextBtnText = currentLang === 'en' 
                ? (isLast ? "See My Results! 📊" : "Next Question ➔") 
                : (isLast ? "¡Ver mis resultados! 📊" : "Siguiente pregunta ➔");

            actionDiv.innerHTML = `
                <button onclick="advanceQuizStep()" class="bg-toro-crimson hover:bg-red-950 text-white font-extrabold text-xs px-5 py-2.5 rounded-xl transition shadow-md active:scale-95">
                    ${nextBtnText}
                </button>
            `;
            document.getElementById('quizQuestionPanel').appendChild(actionDiv);
        }

        function advanceQuizStep() {
            if (currentQuizIndex < 4) {
                currentQuizIndex++;
                renderActiveQuestionStep();
            } else {
                renderStopQuizResults();
            }
        }

        function renderStopQuizResults() {
            const dict = dataLang[currentLang];
            const questionPanel = document.getElementById('quizQuestionPanel');
            const hasPassed = stopAnswersCorrect >= 3;

            document.getElementById('quizStepCount').textContent = currentLang === 'en' ? "Stop Quiz Done" : "Prueba finalizada";

            if (hasPassed) {
                questionPanel.innerHTML = `
                    <div class="text-center py-4 space-y-4">
                        <span class="text-5xl animate-bounce block">🎉</span>
                        <h4 class="font-extrabold text-toro-green text-base">${dict.congratsText}</h4>
                        <p class="text-xs text-gray-500 leading-relaxed max-w-xs mx-auto">
                            ${currentLang === 'en' 
                                ? `Outstanding! You answered <strong>${stopAnswersCorrect} of 5</strong> questions correctly.`
                                : `¡Increíble! Respondiste correctamente <strong>${stopAnswersCorrect} de 5</strong> preguntas.`
                            }
                        </p>
                        <div class="bg-amber-50 border border-amber-200/50 p-2.5 rounded-xl text-toro-goldDark font-black text-xs inline-block animate-pulse">
                            +100 Quest XP Earned!
                        </div>
                        <button onclick="claimStopBadgeReward()" class="w-full bg-toro-green hover:bg-emerald-850 text-white font-extrabold text-xs py-3 rounded-2xl transition shadow-md">
                            ${dict.passBtnText}
                        </button>
                    </div>
                `;
            } else {
                questionPanel.innerHTML = `
                    <div class="text-center py-4 space-y-4">
                        <span class="text-5xl animate-pulse block">💡</span>
                        <h4 class="font-extrabold text-toro-crimson text-sm">
                            ${currentLang === 'en' ? "Toro Guidance Check-in" : "Insuficiente comprensión"}
                        </h4>
                        <p class="text-xs text-gray-500 leading-relaxed max-w-xs mx-auto">
                            ${currentLang === 'en' 
                                ? `You scored <strong>${stopAnswersCorrect} of 5</strong>. You need at least <strong>3/5</strong> to earn your sticker. Listen to the guide and try again!`
                                : `Respondiste <strong>${stopAnswersCorrect} de 5</strong>. Necesitas al menos <strong>3/5</strong> para conseguir tu estampa. ¡Escucha al guía e intenta de nuevo!`
                            }
                        </p>
                        <button onclick="changeStop(currentStop)" class="w-full bg-toro-crimson hover:bg-red-950 text-white font-extrabold text-xs py-3 rounded-2xl transition shadow-md">
                            ${dict.retryBtnText}
                        </button>
                    </div>
                `;
            }
        }

        function claimStopBadgeReward() {
            const dict = dataLang[currentLang];
            xpPoints += 100;
            document.getElementById('xpBar').style.width = `${Math.min(xpPoints / 5, 100)}%`;
            document.getElementById('studentRank').textContent = xpPoints >= 400 ? (currentLang === 'en' ? "Gold Champion" : "Campeón Dorado") : (xpPoints >= 200 ? (currentLang === 'en' ? "Toro Leader" : "Líder Toro") : dict.studentRank);

            unlockedStickers[currentStop - 1] = true;
            updateStickerUI(currentStop);
            updateBadgeCounter();

            isAdvancingFromQuiz = true; 
            
            document.getElementById('successTitle').textContent = dict.congratsText;
            document.getElementById('successDesc').textContent = currentLang === 'en' 
                ? `You passed Stop ${currentStop} check-in quiz successfully and earned a new sticker badge!`
                : `¡Pasaste la prueba de la Parada ${currentStop} con éxito y ganaste una estampa!`;
            
            document.getElementById('xpAwardLabel').classList.remove('hidden');
            document.getElementById('questSuccessModal').classList.remove('hidden');
        }

        function dismissSuccessAlert() {
            document.getElementById('questSuccessModal').classList.add('hidden');
            
            if (isAdvancingFromQuiz) {
                isAdvancingFromQuiz = false;
                advanceToNextStop();
            }
        }

        function advanceToNextStop() {
            if (currentStop < 5) {
                changeStop(currentStop + 1);
            } else {
                const allEarned = unlockedStickers.filter(Boolean).length;
                if (allEarned >= 3) {
                    showVisualNotification(
                        currentLang === 'en' ? "👑 Tour Complete!" : "👑 ¡Recorrido Completo!", 
                        currentLang === 'en' 
                            ? "Splendid work! You unlocked your Golden College Access Card! Show this screen to the guides for special prizes." 
                            : "¡Excelente trabajo! ¡Desbloqueaste tu Pase Universitario Dorado! Muestra esta pantalla al guía para conseguir un premio.", 
                        "🏆"
                    );
                } else {
                    showVisualNotification(
                        currentLang === 'en' ? "Keep Practicing! 🎯" : "¡Sigue Practicando! 🎯", 
                        currentLang === 'en' 
                            ? "You completed the walking tour stops! Go back to failed stops to earn at least 3 out of 5 badges to unlock the overall Golden Pass!" 
                            : "¡Completaste las paradas del recorrido! Regresa a las pruebas que no pasaste para conseguir al menos 3 de las 5 insignias.", 
                        "🐂"
                    );
                }
            }
        }

        // Bingo Mechanics
        function renderBingoGrid() {
            const dict = dataLang[currentLang];
            const container = document.getElementById('bingoGridContainer');
            container.innerHTML = '';
            
            dict.bingoCells.forEach((cell, index) => {
                const isMarked = bingoState[index];
                const btn = document.createElement('button');
                
                if (isMarked) {
                    btn.className = "aspect-square bg-toro-crimson/10 border-2 border-toro-crimson rounded-2xl p-2 flex flex-col items-center justify-center text-center transition-all shadow-md transform scale-102";
                    btn.innerHTML = `
                        <span class="text-3xl mb-1">${cell.icon}</span>
                        <p class="text-[10px] font-black tracking-tight text-toro-crimson leading-tight">${cell.label}</p>
                        <div class="w-3 h-3 mt-1.5 rounded-full bg-toro-crimson border-2 border-white shadow-sm"></div>
                    `;
                } else {
                    btn.className = "aspect-square bg-white hover:bg-amber-50 border border-gray-200 rounded-2xl p-2 flex flex-col items-center justify-center text-center transition-all shadow-xs group";
                    btn.innerHTML = `
                        <span class="text-3xl mb-1 group-hover:scale-110 transition">${cell.icon}</span>
                        <p class="text-[10px] font-extrabold tracking-tight text-gray-500 leading-tight">${cell.label}</p>
                        <div class="w-3 h-3 mt-1.5 rounded-full border border-gray-300 bg-gray-50 shadow-inner"></div>
                    `;
                }
                
                if (index !== 4) {
                    btn.onclick = () => toggleBingoCell(index);
                }
                
                container.appendChild(btn);
            });
        }

        function toggleBingoCell(index) {
            if (index === 4) return; 
            bingoState[index] = !bingoState[index];
            renderBingoGrid();
            checkBingoWin();
        }

        function checkBingoWin() {
            const hasWon = bingoState.every(cell => cell === true);
            if (hasWon) {
                document.getElementById('bingoCelebrationOverlay').classList.remove('hidden');
                document.getElementById('bingoAlertDot').classList.remove('hidden');
            } else {
                document.getElementById('bingoAlertDot').classList.add('hidden');
            }
        }

        function claimBingoBadge() {
            document.getElementById('bingoCelebrationOverlay').classList.add('hidden');
            document.getElementById('bingoAlertDot').classList.add('hidden');
            updateSpecialStickersUI();
            updateBadgeCounter();
            
            isAdvancingFromQuiz = false; 
            showVisualNotification(
                currentLang === 'en' ? "Bingo Champion! 🎰" : "¡Campeón de Bingo! 🎰",
                currentLang === 'en' ? "You found every item and claimed the special Bingo Badge! Check your bag." : "¡Encontraste todos los artículos y reclamaste la Insignia Especial de Bingo! Revisa tu mochila.",
                "🎉"
            );
        }

        function resetBingo() {
            bingoState = [false, false, false, false, true, false, false, false, false];
            document.getElementById('bingoCelebrationOverlay').classList.add('hidden');
            document.getElementById('bingoAlertDot').classList.add('hidden');
            renderBingoGrid();
            updateSpecialStickersUI();
        }

        // Secret Bull Discovery (5 total, 1 per stop)
        function discoverSecretBull() {
            if (hiddenBullsFound[currentStop - 1]) return; 
            
            hiddenBullsFound[currentStop - 1] = true;
            document.getElementById('stopHiddenBull').style.display = 'none';
            
            const totalFound = hiddenBullsFound.filter(Boolean).length;
            document.getElementById('bullProgressLabel').textContent = `${totalFound}/5 Bulls`;
            
            const dict = dataLang[currentLang];
            const qualityData = dict.hiddenQualities[totalFound - 1]; 

            isAdvancingFromQuiz = false; 

            if (totalFound >= 5) {
                updateSpecialStickersUI();
                updateBadgeCounter();
                showVisualNotification(
                    qualityData.title,
                    qualityData.desc,
                    "🎓"
                );
            } else {
                showVisualNotification(
                    qualityData.title,
                    qualityData.desc,
                    "🌟"
                );
            }
        }

        // Voice Coach Speech Engine
        function speakSpecificWord(wordText) {
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
                const utterance = new SpeechSynthesisUtterance(wordText);
                utterance.lang = 'en-US';
                utterance.rate = 0.7; 
                utterance.pitch = 1.0;
                window.speechSynthesis.speak(utterance);
            }
        }

        // Lightbox
        function openLightbox(url, caption) {
            document.getElementById('lightboxImage').src = url;
            document.getElementById('lightboxCaption').textContent = caption;
            document.getElementById('lightboxModal').classList.remove('hidden');
        }

        function closeLightbox() {
            document.getElementById('lightboxModal').classList.add('hidden');
        }

        function updateStickerUI(num) {
            const dict = dataLang[currentLang];
            const stickerCard = document.getElementById(`sticker-${num}`);
            if (!stickerCard) return;
            const label = document.getElementById(`stkStatus-${num}`);
            const unlocked = unlockedStickers[num - 1];

            if (unlocked) {
                stickerCard.classList.remove('opacity-40');
                stickerCard.classList.add('border-toro-crimson', 'bg-red-50/20');
                label.textContent = dict.stkUnlocked;
                label.className = "text-xxs font-black text-toro-crimson bg-toro-gold px-2.5 py-0.5 rounded-full mt-2 animate-bounce";
            } else {
                stickerCard.classList.add('opacity-40');
                stickerCard.classList.remove('border-toro-crimson', 'bg-red-50/20');
                label.textContent = dict.stkLocked;
                label.className = "text-xxs font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-2";
            }

            const badgesEarnedCount = unlockedStickers.filter(Boolean).length;
            const hasQualified = badgesEarnedCount >= 3;
            
            const ticket = document.getElementById('goldTicket');
            const passStatus = document.getElementById('ticketStatus');
            
            if (hasQualified) {
                ticket.classList.remove('opacity-30');
                ticket.classList.add('border-toro-goldDark', 'shadow-lg');
                passStatus.textContent = dict.ticketUnlocked;
                passStatus.className = "font-black text-toro-crimson border-2 border-toro-crimson bg-toro-gold rounded-lg px-2.5 py-1 text-xs uppercase rotate-12 animate-pulse";
            } else {
                ticket.classList.add('opacity-30');
                passStatus.textContent = dict.ticketLocked;
                passStatus.className = "font-black text-gray-400 border-2 border-gray-300 rounded-lg px-2.5 py-1 text-xs uppercase rotate-12";
            }
        }

        function updateSpecialStickersUI() {
            const dict = dataLang[currentLang];
            
            // Bingo Badge
            const bingoWon = bingoState.every(Boolean);
            const stkBingo = document.getElementById('sticker-bingo');
            const lblBingo = document.getElementById('stkStatus-bingo');
            if (bingoWon) {
                stkBingo.classList.remove('opacity-40');
                stkBingo.classList.add('border-toro-gold', 'bg-amber-50/30');
                lblBingo.textContent = dict.stkUnlocked;
                lblBingo.className = "text-[10px] font-black text-toro-crimson bg-toro-gold px-2 py-0.5 rounded-full mt-1 animate-bounce";
            } else {
                stkBingo.classList.add('opacity-40');
                stkBingo.classList.remove('border-toro-gold', 'bg-amber-50/30');
                lblBingo.textContent = dict.stkLocked;
                lblBingo.className = "text-[10px] font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1";
            }

            // Hidden Bull Badge
            const allBullsFound = hiddenBullsFound.every(Boolean);
            const stkBull = document.getElementById('sticker-bull');
            const lblBull = document.getElementById('stkStatus-bull');
            document.getElementById('bullProgressLabel').textContent = `${hiddenBullsFound.filter(Boolean).length}/5 Bulls`;
            
            if (allBullsFound) {
                stkBull.classList.remove('opacity-40');
                stkBull.classList.add('border-slate-800', 'bg-slate-100');
                lblBull.textContent = dict.stkUnlocked;
                lblBull.className = "text-[10px] font-black text-white bg-slate-800 px-2 py-0.5 rounded-full mt-1 animate-bounce";
            } else {
                stkBull.classList.add('opacity-40');
                stkBull.classList.remove('border-slate-800', 'bg-slate-100');
                lblBull.textContent = dict.stkLocked;
                lblBull.className = "text-[10px] font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1";
            }

            // Visual Explorer Badge
            const allVisualPassed = visualPassedState.every(Boolean);
            const stkVisual = document.getElementById('sticker-visual');
            const lblVisual = document.getElementById('stkStatus-visual');
            document.getElementById('visualProgressLabel').textContent = `${visualPassedState.filter(Boolean).length}/5 Checks`;

            if (allVisualPassed) {
                stkVisual.classList.remove('opacity-40');
                stkVisual.classList.add('border-indigo-800', 'bg-indigo-50');
                lblVisual.textContent = dict.stkUnlocked;
                lblVisual.className = "text-[10px] font-black text-white bg-indigo-600 px-2 py-0.5 rounded-full mt-1 animate-bounce";
            } else {
                stkVisual.classList.add('opacity-40');
                stkVisual.classList.remove('border-indigo-800', 'bg-indigo-50');
                lblVisual.textContent = dict.stkLocked;
                lblVisual.className = "text-[10px] font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1";
            }

            // Resourceful Badge
            const allResourcePassed = resourcePassedState.every(Boolean);
            const stkResource = document.getElementById('sticker-resource');
            const lblResource = document.getElementById('stkStatus-resource');
            document.getElementById('resourceProgressLabel').textContent = `${resourcePassedState.filter(Boolean).length}/5 Spotlights`;

            if (allResourcePassed) {
                stkResource.classList.remove('opacity-40');
                stkResource.classList.add('border-teal-800', 'bg-teal-50');
                lblResource.textContent = dict.stkUnlocked;
                lblResource.className = "text-[10px] font-black text-white bg-teal-600 px-2 py-0.5 rounded-full mt-1 animate-bounce";
            } else {
                stkResource.classList.add('opacity-40');
                stkResource.classList.remove('border-teal-800', 'bg-teal-50');
                lblResource.textContent = dict.stkLocked;
                lblResource.className = "text-[10px] font-bold text-gray-400 bg-gray-100 px-2 py-0.5 rounded-full mt-1";
            }
        }

        function updateBadgeCounter() {
            let count = unlockedStickers.filter(Boolean).length;
            if (bingoState.every(Boolean)) count++;
            if (hiddenBullsFound.every(Boolean)) count++;
            if (visualPassedState.every(Boolean)) count++;
            if (resourcePassedState.every(Boolean)) count++;
            
            const counter = document.getElementById('badgeCounter');
            if (count > 0) {
                counter.textContent = count;
                counter.classList.remove('hidden');
            } else {
                counter.classList.add('hidden');
            }
        }

        function showVisualNotification(title, desc, icon = "🌟") {
            document.getElementById('successIcon').textContent = icon;
            document.getElementById('successTitle').textContent = title;
            document.getElementById('successDesc').textContent = desc;
            document.getElementById('xpAwardLabel').classList.add('hidden');
            document.getElementById('questSuccessModal').classList.remove('hidden');
        }

        // App Navigation Switcher (5 Tabs)
        function switchTab(tabId) {
            currentActiveTab = tabId;
            const tabIds = ['quests', 'bingo', 'resources', 'bag', 'wisdom'];
            
            tabIds.forEach(id => {
                const button = document.getElementById(`tab-${id}`);
                if (id === tabId) {
                    button.classList.add('text-toro-crimson');
                    button.classList.remove('text-gray-400');
                } else {
                    button.classList.remove('text-toro-crimson');
                    button.classList.add('text-gray-400');
                }
            });

            document.getElementById('questsScreen').style.display = tabId === 'quests' ? 'block' : 'none';
            document.getElementById('bingoScreen').style.display = tabId === 'bingo' ? 'block' : 'none';
            document.getElementById('resourcesScreen').style.display = tabId === 'resources' ? 'block' : 'none';
            document.getElementById('bagScreen').style.display = tabId === 'bag' ? 'block' : 'none';
            document.getElementById('wisdomScreen').style.display = tabId === 'wisdom' ? 'block' : 'none';
        }

        // Fun Hub
        function setFunHubTab(tabName) {
            activeFunHubTab = tabName;
            const subTabs = ['joke', 'fact', 'fortune'];
            
            subTabs.forEach(name => {
                const btn = document.getElementById(`funTab-${name}`);
                if (name === tabName) {
                    btn.className = "flex-1 text-center py-2 text-xs font-bold rounded-xl transition bg-white text-toro-crimson shadow-xs focus:outline-none";
                } else {
                    btn.className = "flex-1 text-center py-2 text-xs font-bold rounded-xl transition text-gray-500 focus:outline-none hover:bg-white/50";
                }
            });

            const dict = dataLang[currentLang];
            const actionBtnLabel = document.getElementById('funHubActionLabel');
            if (tabName === 'joke') {
                actionBtnLabel.textContent = currentLang === 'en' ? "Generate Toro Joke!" : "¡Generar Chiste de Toro!";
            } else if (tabName === 'fact') {
                actionBtnLabel.textContent = currentLang === 'en' ? "Next Trivia Fact!" : "¡Siguiente Dato Curioso!";
            } else {
                actionBtnLabel.textContent = currentLang === 'en' ? "Reveal My Destiny!" : "¡Revelar mi destino!";
            }

            triggerFunHubGeneration();
        }

        function triggerFunHubGeneration() {
            const container = document.getElementById('funHubCardContent');
            const dict = dataLang[currentLang];
            
            container.classList.remove('animate-pop');
            void container.offsetWidth; // trigger reflow
            container.classList.add('animate-pop');

            if (activeFunHubTab === 'joke') {
                const index = Math.floor(Math.random() * dict.jokes.length);
                const joke = dict.jokes[index];
                container.innerHTML = `
                    <span class="text-5xl block mb-2">😂</span>
                    <h4 class="font-black text-toro-crimson text-sm uppercase tracking-wide">${currentLang === 'en' ? "Campus Laughs" : "Chistes Universitarios"}</h4>
                    <p class="text-sm text-slate-800 font-extrabold max-w-[250px] mx-auto leading-snug mt-1">${joke.q}</p>
                    <div class="mt-3 bg-gradient-to-r from-emerald-50 to-green-50 p-3 rounded-2xl border border-emerald-100 shadow-inner">
                        <p class="text-xs text-toro-green font-black max-w-[250px] mx-auto leading-relaxed">${joke.a}</p>
                    </div>
                `;
            } else if (activeFunHubTab === 'fact') {
                const index = Math.floor(Math.random() * dict.funFacts.length);
                const fact = dict.funFacts[index];
                container.innerHTML = `
                    <span class="text-5xl block mb-2">💡</span>
                    <h4 class="font-black text-toro-crimson text-sm uppercase tracking-wide">${currentLang === 'en' ? "Did You Know?" : "¿Sabías Qué?"}</h4>
                    <div class="mt-2 bg-slate-50 p-4 rounded-2xl border border-slate-200">
                        <p class="text-xs text-slate-700 font-bold max-w-[250px] mx-auto leading-relaxed">${fact}</p>
                    </div>
                `;
            } else {
                const index = Math.floor(Math.random() * dict.fortunes.length);
                const fortune = dict.fortunes[index];
                container.innerHTML = `
                    <span class="text-5xl block mb-2">✨</span>
                    <h4 class="font-black text-toro-crimson text-sm uppercase tracking-wide">${currentLang === 'en' ? "Toro Guidance" : "Sabiduría Toro"}</h4>
                    <div class="mt-2 bg-amber-50 p-4 rounded-2xl border border-amber-200 border-dashed">
                        <p class="text-xs text-amber-900 italic font-bold max-w-[250px] mx-auto leading-relaxed">"${fortune}"</p>
                    </div>
                `;
            }
        }

        function renderResourcesList(filterQuery = "") {
            const listContainer = document.getElementById('studentResourcesList');
            listContainer.innerHTML = '';

            const lowercaseQuery = filterQuery.toLowerCase();

            supportResources.forEach(res => {
                const name = currentLang === 'en' ? res.nameEn : res.nameEs;
                const desc = currentLang === 'en' ? res.descEn : res.descEs;

                if (filterQuery !== "") {
                    const matchesName = name.toLowerCase().includes(lowercaseQuery);
                    const matchesDesc = desc.toLowerCase().includes(lowercaseQuery);
                    const matchesLoc = res.loc.toLowerCase().includes(lowercaseQuery);
                    if (!matchesName && !matchesDesc && !matchesLoc) return;
                }

                const card = document.createElement('div');
                card.className = "bg-white p-4 rounded-2xl border border-gray-100 shadow-xs flex items-start gap-3.5 hover:border-toro-crimson/30 transition-all";
                card.innerHTML = `
                    <span class="text-3xl p-1 bg-slate-50 rounded-xl border border-slate-100">${res.icon}</span>
                    <div class="flex-1 min-w-0">
                        <h4 class="font-extrabold text-sm text-toro-crimson truncate leading-snug">${name}</h4>
                        <div class="flex flex-wrap items-center gap-x-2 text-xxs font-bold text-gray-400 mt-0.5 uppercase tracking-wide">
                            <span>📍 ${res.loc}</span>
                            <span class="text-gray-300">•</span>
                            <span>⏱️ ${res.hours}</span>
                        </div>
                        <p class="text-xs text-gray-500 mt-2 leading-relaxed">${desc}</p>
                    </div>
                `;
                listContainer.appendChild(card);
            });

            if (listContainer.children.length === 0) {
                listContainer.innerHTML = `
                    <div class="text-center py-8 text-gray-400">
                        <p class="text-2xl">🔍</p>
                        <p class="text-xs mt-1 font-semibold">${currentLang === 'en' ? "No resources found. Try another term!" : "No se encontraron servicios. ¡Prueba otro término!"}</p>
                    </div>
                `;
            }
        }

        function searchResources() {
            const val = document.getElementById('resourceSearch').value;
            renderResourcesList(val);
        }

        function resetApp() {
            xpPoints = 0;
            unlockedStickers = [false, false, false, false, false];
            bingoState = [false, false, false, false, true, false, false, false, false];
            hiddenBullsFound = [false, false, false, false, false];
            visualPassedState = [false, false, false, false, false];
            resourcePassedState = [false, false, false, false, false];
            visualTriesState = [3, 3, 3, 3, 3];
            resourceTriesState = [3, 3, 3, 3, 3];
            
            document.getElementById('xpBar').style.width = '20%';
            document.getElementById('resourceSearch').value = '';
            document.getElementById('xpAwardLabel').classList.remove('hidden');
            document.getElementById('bingoCelebrationOverlay').classList.add('hidden');
            document.getElementById('bingoAlertDot').classList.add('hidden');
            document.getElementById('photoQuizResult').classList.add('hidden');
            document.getElementById('resourceCheckResult').classList.add('hidden');
            
            updateBadgeCounter();
            updateSpecialStickersUI();
            for (let i = 1; i <= 5; i++) {
                updateStickerUI(i);
            }
            
            setLanguage(currentLang);
            changeStop(1);
            switchTab('quests');
        }
    </script>
</body>
</html>
