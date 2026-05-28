[俄语答辩.html](https://github.com/user-attachments/files/28354772/default.html)
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>俄语答辩练习助手</title>
    <style>
        :root {
            --primary: #4f6ef7;
            --primary-light: #eef1ff;
            --success: #0f9d58;
            --danger: #d93025;
            --bg: #f4f6fd;
            --card: #ffffff;
            --text: #202124;
            --accent: #ff6b9d;
        }
        body {
            font-family: 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
            background: linear-gradient(135deg, #f4f6fd 0%, #e9eefa 100%);
            color: var(--text);
            max-width: 860px;
            margin: 20px auto;
            padding: 0 16px 60px;
        }
        .card {
            background: var(--card);
            border-radius: 20px;
            padding: 24px;
            margin-bottom: 20px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.06);
            border: 1px solid rgba(79,110,247,0.08);
        }
        textarea {
            width: 100%;
            height: 160px;
            border: 2px solid #e0e4f0;
            border-radius: 14px;
            padding: 14px;
            font-size: 16px;
            resize: vertical;
            transition: 0.2s;
        }
        textarea:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(79,110,247,0.15);
        }
        button {
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 30px;
            padding: 10px 24px;
            font-size: 15px;
            font-weight: 600;
            cursor: pointer;
            margin: 5px 4px;
            transition: 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            box-shadow: 0 2px 6px rgba(79,110,247,0.3);
        }
        button:hover { background: #3b5de7; transform: translateY(-1px); }
        button:disabled { background: #c3c8e0; box-shadow: none; cursor: not-allowed; transform: none; }
        .accent {
            font-size: 1.3em;
            line-height: 1.9;
            white-space: pre-wrap;
            background: #fafbff;
            padding: 16px;
            border-radius: 14px;
            margin: 10px 0;
        }
        .sentence-item {
            display: flex;
            flex-direction: column;
            padding: 12px 0;
            border-bottom: 1px solid #f0f2fa;
        }
        .sentence-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex-wrap: wrap;
        }
        .sentence-text { flex: 1; margin-right: 10px; }
        .play-btn, .loop-btn {
            background: none;
            border: none;
            font-size: 22px;
            cursor: pointer;
            padding: 4px 8px;
            border-radius: 20px;
            transition: 0.1s;
        }
        .play-btn:hover, .loop-btn:hover { background: #edf1fe; }
        .loop-active { background: #dce3ff; color: var(--primary); }
        .timer { font-weight: bold; font-size: 1.5em; color: var(--primary); }
        .result { margin-top: 14px; padding: 10px 14px; border-radius: 14px; background: #eef3ff; }
        .fail { background: #ffeaea; }
        .pass { background: #e3f9ee; }
        .word {
            cursor: pointer;
            border-bottom: 2px dotted #aab7f0;
            margin: 0 2px;
            transition: 0.1s;
        }
        .word:hover { background: #e0e5ff; border-radius: 6px; }
        .translation-popup {
            position: fixed;
            background: white;
            border: 1px solid #ccd5f5;
            border-radius: 14px;
            padding: 8px 16px;
            box-shadow: 0 6px 18px rgba(0,0,0,0.15);
            z-index: 1000;
            max-width: 300px;
            font-size: 14px;
        }
        .diff-correct { color: green; }
        .diff-wrong { color: red; text-decoration: line-through; }
        .diff-missing { color: orange; }
        .diff-extra { color: purple; }
        .rate-control {
            display: flex;
            align-items: center;
            gap: 10px;
            margin: 8px 0 16px;
            background: #f0f3ff;
            padding: 8px 16px;
            border-radius: 30px;
            width: fit-content;
        }
        .rate-control label { font-weight: 600; }
        .rate-control input[type=range] { width: 140px; }
        .rate-control span { min-width: 45px; font-weight: 600; color: var(--primary); }
        .footer {
            text-align: center;
            font-size: 0.9em;
            color: #8896b0;
            margin-top: 20px;
            padding-top: 15px;
            border-top: 1px solid #e1e6f5;
        }
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.5); display: flex; align-items: center; justify-content: center;
            z-index: 2000;
        }
        .modal {
            background: white; border-radius: 28px; padding: 32px; text-align: center;
            max-width: 400px; box-shadow: 0 12px 28px rgba(0,0,0,0.3);
            animation: pop 0.4s ease;
        }
        @keyframes pop {
            0% { transform: scale(0.8); opacity: 0; }
            80% { transform: scale(1.04); }
            100% { transform: scale(1); opacity: 1; }
        }
        .modal h2 { color: var(--primary); margin: 0 0 10px; }
        .modal p { font-size: 1.05em; color: #555; margin-bottom: 16px; text-align: left; line-height: 1.6; }
        .help-steps { text-align: left; padding-left: 20px; margin: 10px 0; }
        .help-steps li { margin-bottom: 8px; }
        .modal button { margin-top: 8px; }
        .help-float {
            position: fixed;
            top: 20px;
            right: 20px;
            background: white;
            border-radius: 50px;
            padding: 8px 18px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
            cursor: pointer;
            font-weight: 600;
            color: var(--primary);
            z-index: 1500;
            display: flex;
            align-items: center;
            gap: 6px;
            transition: 0.2s;
        }
        .help-float:hover { background: #f0f3ff; transform: scale(1.05); }
        /* 会员状态条 */
        .membership-bar {
            background: linear-gradient(90deg, #eef2ff, #e0e8ff);
            border-radius: 12px;
            padding: 8px 18px;
            margin-bottom: 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: 500;
            color: #2c3e8c;
            border: 1px solid #ccd9ff;
        }
        .membership-bar.expired {
            background: #ffeaea;
            border-color: #ffc4c4;
            color: #a82020;
        }
        .activation-input {
            width: 100%;
            padding: 12px 16px;
            border: 2px solid #e0e4f0;
            border-radius: 12px;
            font-size: 16px;
            margin: 12px 0;
            text-align: center;
            letter-spacing: 2px;
        }
        .activation-error { color: var(--danger); font-size: 0.9em; margin: 4px 0; }
    </style>
</head>
<body>

    <!-- 会员状态条 -->
    <div class="membership-bar" id="membershipBar">
        ⏳ 会员状态加载中...
    </div>

    <!-- 激活弹窗 -->
    <div id="activationModal" class="modal-overlay" style="display:none;">
        <div class="modal">
            <h2>🔐 需要激活会员</h2>
            <p>输入你的激活码以使用全部功能。<br>请联系管理员获取激活码。</p>
            <input type="text" class="activation-input" id="activationCode" placeholder="请输入激活码">
            <div class="activation-error" id="activationError"></div>
            <button onclick="activate()">✨ 激活</button>
            <p style="font-size:0.8em;color:#999;margin-top:10px;">未激活状态下，仅可预览界面，无法使用朗读和评分功能。</p>
        </div>
    </div>

    <!-- 浮动帮助按钮 -->
    <div class="help-float" onclick="openHelp()">📘 使用指南</div>

    <h1>🎓 俄语答辩朗读练习 <span style="font-size:0.8em;color:#888;">— твой голос, твоя защита</span></h1>
    <div class="card" id="inputArea">
        <h2>📄 输入你的答辩稿</h2>
        <textarea id="textInput" placeholder="Вставьте текст защиты..."></textarea>
        <button onclick="processText()">✨ 分析重音并开始练习</button>
        <p style="font-size:0.9em;color:#6b7a99;">按句号、问号、感叹号自动分句</p>
    </div>

    <div class="card" id="practiceArea" style="display:none;">
        <div style="display:flex; align-items:center; justify-content: space-between; flex-wrap:wrap;">
            <div class="rate-control">
                <label>🐢 语速</label>
                <input type="range" id="rateSlider" min="0.5" max="1.5" step="0.1" value="0.9" oninput="updateRate(this.value)">
                <span id="rateValue">0.9x</span>
            </div>
            <button id="globalPauseBtn" onclick="toggleGlobalPause()" style="background:#f5f5f5; color:#333; box-shadow:none;" disabled>⏸️ 暂停</button>
        </div>

        <h2>📖 带重音的全文（点击单词看中文）</h2>
        <div class="accent" id="fullAccentedText"></div>
        <button onclick="speakFullText()">🔊 听全文示范</button>
        <hr>
        <h3>✍️ 分句练习（点击单词看中文）</h3>
        <div id="sentencesList"></div>
        <hr>
        <h3>🏁 全篇模拟答辩</h3>
        <p>点击开始后，请连续朗读整篇稿子，读完后点停止。</p>
        <button id="startFullTestBtn" onclick="startFullTest()">▶️ 开始全篇测试</button>
        <button id="stopFullTestBtn" onclick="stopFullTest()" disabled>⏹️ 停止并评分</button>
        <div class="timer" id="fullTimer">00:00</div>
        <div id="fullTestResult"></div>
    </div>

    <!-- 祝贺弹窗 -->
    <div id="congratsModal" class="modal-overlay" style="display:none;">
        <div class="modal">
            <h2>🎉 Поздравляю!</h2>
            <p>你的朗读已达标！<br>发音很棒，时间也控制得很好！</p>
            <p style="font-size:1.3em; font-weight:bold; color:var(--accent);">祝你答辩成功！✨</p>
            <button onclick="closeCongrats()">💪 继续练习</button>
        </div>
    </div>

    <!-- 帮助弹窗 -->
    <div id="helpModal" class="modal-overlay" style="display:none;">
        <div class="modal" style="max-width:480px; text-align:left;">
            <h2>📘 使用指南</h2>
            <ol class="help-steps">
                <li><strong>粘贴稿件：</strong>把答辩稿的俄语文本粘贴到输入框，点击“分析重音”。</li>
                <li><strong>查看重音：</strong>系统自动为每个单词标注重音（带小撇的字母）。</li>
                <li><strong>点击单词：</strong>点击任意俄语单词，可弹出中文翻译。</li>
                <li><strong>调整语速：</strong>拖动顶部滑块，可减慢或加快朗读示范。</li>
                <li><strong>分句练习：</strong><br>
                    🔊 点击喇叭听示范；<br>
                    🔁 点击循环按钮可重复播放该句；<br>
                    🎤 点击“练习”录音，系统会对比发音并给出准确率。
                </li>
                <li><strong>全篇模拟：</strong>点击“开始全篇测试”，连续朗读，读完后点击“停止并评分”。</li>
                <li><strong>达标标准：</strong>10 分钟内读完且准确率 ≥ 90%，即视为达标。</li>
            </ol>
            <p style="color:#888;">💡 使用 Chrome 浏览器，允许麦克风权限，效果最佳。</p>
            <button onclick="closeHelp()">我知道了</button>
        </div>
    </div>

    <div class="footer">
        🌟 专为俄语毕业答辩设计 | 祝你答辩顺利！🌟
    </div>
    <div id="translationPopup" class="translation-popup" style="display:none;"></div>

    <script>
        // ========== 会员激活与倒计时系统 ==========
        const ACTIVATION_KEY = 'rus_accent_activation';
        let membershipTimer = null;

        function getActivationData() {
            const raw = localStorage.getItem(ACTIVATION_KEY);
            if (!raw) return null;
            try { return JSON.parse(raw); } catch { return null; }
        }

        function isActivated() {
            const data = getActivationData();
            if (!data) return false;
            return Date.now() < data.expire;
        }

        function getRemainingMs() {
            const data = getActivationData();
            if (!data) return 0;
            return Math.max(0, data.expire - Date.now());
        }

        function formatCountdown(ms) {
            if (ms <= 0) return '已过期';
            const seconds = Math.floor(ms / 1000);
            const days = Math.floor(seconds / 86400);
            const hours = Math.floor((seconds % 86400) / 3600);
            const minutes = Math.floor((seconds % 3600) / 60);
            const secs = seconds % 60;
            let str = '';
            if (days > 0) str += `${days}天 `;
            str += `${hours.toString().padStart(2,'0')}:${minutes.toString().padStart(2,'0')}:${secs.toString().padStart(2,'0')}`;
            return str;
        }

        function updateMembershipBar() {
            const bar = document.getElementById('membershipBar');
            if (!isActivated()) {
                bar.innerHTML = '🔒 未激活会员 | 部分功能不可用';
                bar.className = 'membership-bar expired';
                // 如果已经显示激活弹窗就不再弹出
                if (document.getElementById('activationModal').style.display !== 'flex') {
                    // 不自动弹窗，只在用户点击需要权限功能时弹出
                }
                return;
            }
            const remainingMs = getRemainingMs();
            if (remainingMs <= 0) {
                bar.innerHTML = '⚠️ 会员已过期 | 请重新激活';
                bar.className = 'membership-bar expired';
                // 自动弹出激活窗口
                if (document.getElementById('activationModal').style.display !== 'flex') {
                    showActivationModal();
                }
                return;
            }
            bar.className = 'membership-bar';
            bar.innerHTML = `⭐ 会员剩余：${formatCountdown(remainingMs)}`;
        }

        function startCountdown() {
            if (membershipTimer) clearInterval(membershipTimer);
            updateMembershipBar();
            membershipTimer = setInterval(updateMembershipBar, 1000);
        }

        function saveActivation(expireTimestamp) {
            localStorage.setItem(ACTIVATION_KEY, JSON.stringify({ expire: expireTimestamp }));
            startCountdown();
            // 如果激活弹窗打开着则关闭
            if (document.getElementById('activationModal').style.display === 'flex') {
                hideActivationModal();
            }
        }

        function showActivationModal() {
            document.getElementById('activationModal').style.display = 'flex';
            document.getElementById('activationError').textContent = '';
        }

        function hideActivationModal() {
            document.getElementById('activationModal').style.display = 'none';
        }

        function activate() {
            const code = document.getElementById('activationCode').value.trim();
            if (!code) return;
            try {
                const decoded = atob(code);
                const expire = parseInt(decoded);
                if (isNaN(expire) || expire <= Date.now()) {
                    document.getElementById('activationError').textContent = '激活码无效或已过期。';
                    return;
                }
                saveActivation(expire);
                alert(`✅ 激活成功！剩余有效期：${formatCountdown(expire - Date.now())}`);
            } catch {
                document.getElementById('activationError').textContent = '激活码格式错误。';
            }
        }

        // 功能拦截
        function requireActivation() {
            if (!isActivated()) {
                showActivationModal();
                return false;
            }
            return true;
        }

        // ========== 内置重音词典 + 规则 ==========
        const stressDict = {
            "здравствуйте":"здра́вствуйте","как":"ка́к","дела":"дела́","спасибо":"спаси́бо",
            "пожалуйста":"пожа́луйста","меня":"меня́","зовут":"зову́т","студент":"студе́нт",
            "университет":"университе́т","факультет":"факульте́т","работа":"рабо́та",
            "исследование":"иссле́дование","тема":"те́ма","диплом":"дипло́м",
            "защита":"защи́та","доклад":"докла́д","слово":"сло́во","вопрос":"вопро́с",
            "ответ":"отве́т","хорошо":"хорошо́","отлично":"отли́чно","плохо":"пло́хо",
            "могу":"могу́","хочу":"хочу́","знаю":"зна́ю","понимаю":"понима́ю",
            "говорю":"говорю́","читаю":"чита́ю","пишу":"пишу́","смотрю":"смотрю́",
            "думаю":"ду́маю","считаю":"счита́ю","верю":"ве́рю","люблю":"люблю́"
        };

        function addStress(word) {
            const lower = word.toLowerCase();
            if (stressDict[lower]) return stressDict[lower];
            if (/[аяуюеёиыоэ]$/i.test(word) && word.length > 3) {
                const vowels = 'аеёиоуыэюя';
                let cnt = 0;
                for (let i = word.length-1; i >= 0; i--) {
                    if (vowels.includes(word[i].toLowerCase())) {
                        cnt++;
                        if (cnt === 2) {
                            return word.slice(0, i) + word[i].normalize('NFD') + '\u0301' + word.slice(i+1);
                        }
                    }
                }
            }
            const firstVowel = word.search(/[аеёиоуыэюя]/i);
            if (firstVowel !== -1) {
                const ch = word[firstVowel];
                return word.slice(0, firstVowel) + ch.normalize('NFD') + '\u0301' + word.slice(firstVowel+1);
            }
            return word;
        }

        function stressText(text) { return text.replace(/[А-Яа-яЁё]+/g, addStress); }
        function removeAccents(str) { return str.normalize('NFD').replace(/[\u0301]/g, '').normalize('NFC'); }
        function cleanText(str) { return removeAccents(str).toLowerCase().replace(/[^а-яёa-z0-9\s]/gi, '').trim(); }
        function splitSentences(text) {
            const parts = text.match(/[^.!?]+[.!?]+|[^.!?]+$/g);
            return parts ? parts.map(s => s.trim()).filter(s => s) : [text];
        }

        // ========== 语音控制 ==========
        let speechRate = 0.9;
        let currentUtterance = null;
        let isPaused = false;
        let loopSentenceIdx = -1;

        function updateRate(val) {
            speechRate = parseFloat(val);
            document.getElementById('rateValue').textContent = val + 'x';
        }

        function speak(text, onEndCallback) {
            if (!requireActivation()) return;
            window.speechSynthesis.cancel();
            const utter = new SpeechSynthesisUtterance(text);
            utter.lang = 'ru-RU';
            utter.rate = speechRate;
            currentUtterance = utter;
            isPaused = false;
            document.getElementById('globalPauseBtn').textContent = '⏸️ 暂停';
            document.getElementById('globalPauseBtn').disabled = false;
            utter.onend = () => {
                currentUtterance = null;
                document.getElementById('globalPauseBtn').disabled = true;
                if (onEndCallback) onEndCallback();
            };
            window.speechSynthesis.speak(utter);
        }

        function toggleGlobalPause() {
            if (!requireActivation()) return;
            if (!currentUtterance) return;
            if (isPaused) {
                window.speechSynthesis.resume();
                isPaused = false;
                document.getElementById('globalPauseBtn').textContent = '⏸️ 暂停';
            } else {
                window.speechSynthesis.pause();
                isPaused = true;
                document.getElementById('globalPauseBtn').textContent = '▶️ 继续';
            }
        }

        function playSentence(idx, loop = false) {
            if (!requireActivation()) return;
            const orig = originalSentences[idx];
            if (!orig) return;
            const play = () => {
                if (loop && loopSentenceIdx === idx) {
                    speak(removeAccents(orig), () => {
                        if (loopSentenceIdx === idx) playSentence(idx, true);
                    });
                } else {
                    speak(removeAccents(orig));
                }
            };
            play();
        }

        function toggleLoop(idx) {
            if (loopSentenceIdx === idx) {
                loopSentenceIdx = -1;
                document.querySelectorAll('.loop-btn').forEach(b => b.classList.remove('loop-active'));
            } else {
                loopSentenceIdx = idx;
                document.querySelectorAll('.loop-btn').forEach(b => b.classList.remove('loop-active'));
                const btn = document.getElementById('loopBtn' + idx);
                if (btn) btn.classList.add('loop-active');
            }
        }

        // ========== 翻译 ==========
        async function translateWord(word) {
            const clean = removeAccents(word);
            const url = `https://api.mymemory.translated.net/get?q=${encodeURIComponent(clean)}&langpair=ru|zh`;
            try {
                const r = await fetch(url); const d = await r.json();
                if (d.responseStatus === 200) return d.responseData.translatedText;
            } catch(e) {}
            return '翻译失败';
        }
        function showTranslation(word, event) {
            const popup = document.getElementById('translationPopup');
            popup.style.display = 'block';
            popup.style.left = event.pageX + 'px';
            popup.style.top = (event.pageY - 30) + 'px';
            popup.textContent = '翻译中...';
            translateWord(word).then(t => { popup.textContent = `${word}: ${t}`; });
        }
        function hideTranslation() { document.getElementById('translationPopup').style.display = 'none'; }
        document.addEventListener('click', (e) => { if (!e.target.classList.contains('word')) hideTranslation(); });
        function makeWordsClickable(html) {
            return html.replace(/[А-Яа-яЁё\u0301]+/g, m => `<span class="word" onclick="event.stopPropagation(); showTranslation('${m}', event)">${m}</span>`);
        }

        // ========== 发音对比 ==========
        function wordDiff(ref, hyp) {
            const refW = cleanText(ref).split(/\s+/).filter(w=>w);
            const hypW = cleanText(hyp).split(/\s+/).filter(w=>w);
            const dp = Array(refW.length+1).fill().map(()=>Array(hypW.length+1).fill(0));
            for(let i=0;i<=refW.length;i++) dp[i][0]=i;
            for(let j=0;j<=hypW.length;j++) dp[0][j]=j;
            for(let i=1;i<=refW.length;i++) for(let j=1;j<=hypW.length;j++){
                dp[i][j] = Math.min(dp[i-1][j]+1, dp[i][j-1]+1, dp[i-1][j-1]+(refW[i-1]===hypW[j-1]?0:1));
            }
            let i=refW.length,j=hypW.length; const diff=[];
            while(i>0||j>0){
                if(i>0&&j>0&&refW[i-1]===hypW[j-1]){ diff.unshift({type:'correct',ref:refW[i-1],hyp:hypW[j-1]}); i--;j--; }
                else if(i>0&&j>0&&dp[i][j]===dp[i-1][j-1]+1){ diff.unshift({type:'wrong',ref:refW[i-1],hyp:hypW[j-1]}); i--;j--; }
                else if(j>0&&dp[i][j]===dp[i][j-1]+1){ diff.unshift({type:'extra',ref:null,hyp:hypW[j-1]}); j--; }
                else { diff.unshift({type:'missing',ref:refW[i-1],hyp:null}); i--; }
            }
            return diff;
        }
        function renderDiff(diff) {
            return diff.map(d=>{
                if(d.type==='correct') return `<span class="diff-correct">${d.ref}</span>`;
                if(d.type==='wrong') return `<span class="diff-wrong">${d.ref}</span>→<span>${d.hyp}</span>`;
                if(d.type==='missing') return `<span class="diff-missing">[漏读:${d.ref}]</span>`;
                return `<span class="diff-extra">[多读:${d.hyp}]</span>`;
            }).join(' ');
        }
        function wordAccuracyFromDiff(diff) {
            const corr = diff.filter(d=>d.type==='correct').length;
            const total = diff.filter(d=>d.type!=='extra').length;
            return total?Math.round((corr/total)*100):100;
        }
        function formatTime(s) { const m=Math.floor(s/60), sec=Math.floor(s%60); return `${m.toString().padStart(2,'0')}:${sec.toString().padStart(2,'0')}`; }

        // ========== 全局状态 ==========
        let originalSentences=[], accentedSentences=[], fullOriginalText='', sentenceScores={};
        let fullTestStartTime=null, fullTestTimerInterval=null, fullRecognition=null, fullRecognizedText='', isFullTestRunning=false;

        function processText() {
            const text = document.getElementById('textInput').value.trim();
            if(!text) return alert('Пожалуйста, введите текст.');
            fullOriginalText = text;
            originalSentences = splitSentences(text);
            accentedSentences = originalSentences.map(s=>stressText(s));
            sentenceScores = {};
            document.getElementById('fullAccentedText').innerHTML = makeWordsClickable(stressText(text));
            renderSentenceList();
            document.getElementById('inputArea').style.display='none';
            document.getElementById('practiceArea').style.display='block';
            document.getElementById('fullTestResult').innerHTML='';
            loopSentenceIdx = -1;
        }

        function renderSentenceList() {
            const container = document.getElementById('sentencesList');
            container.innerHTML = '';
            originalSentences.forEach((orig,idx)=>{
                const accented = accentedSentences[idx]||orig;
                const div = document.createElement('div'); div.className='sentence-item';
                div.innerHTML = `
                <div class="sentence-row">
                    <div class="sentence-text">
                        <strong>${idx+1}. ${makeWordsClickable(accented)}</strong>
                        <br><small style="color:gray;">${orig}</small>
                    </div>
                    <div class="progress">
                        <button class="play-btn" onclick="playSentence(${idx}, loopSentenceIdx===${idx})">🔊</button>
                        <button class="loop-btn" id="loopBtn${idx}" onclick="toggleLoop(${idx}); playSentence(${idx}, true)">🔁</button>
                        <button onclick="practiceSentence(${idx})">🎤 练习</button>
                        <span id="score${idx}" style="font-weight:bold;">${sentenceScores[idx]!==undefined?sentenceScores[idx]+'%':'-'}</span>
                    </div>
                </div>
                <div id="diff${idx}" style="font-size:0.9em;margin-top:4px;"></div>`;
                container.appendChild(div);
            });
        }

        function speakFullText() {
            if (!requireActivation()) return;
            window.speechSynthesis.cancel();
            loopSentenceIdx = -1;
            speak(removeAccents(fullOriginalText));
        }

        function practiceSentence(idx) {
            if (!requireActivation()) return;
            const orig = originalSentences[idx];
            const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
            if(!SR) return alert('Нужен Chrome');
            const rec = new SR(); rec.lang='ru-RU'; rec.interimResults=false; rec.maxAlternatives=1;
            speak(removeAccents(orig));
            setTimeout(()=>{ alert(`请朗读第 ${idx+1} 句`); rec.start(); }, 1500);
            rec.onresult = (e)=>{
                const trans = e.results[0][0].transcript;
                const diff = wordDiff(orig,trans);
                const acc = wordAccuracyFromDiff(diff);
                if(sentenceScores[idx]===undefined||acc>sentenceScores[idx]) sentenceScores[idx]=acc;
                document.getElementById('score'+idx).innerText = acc+'%';
                document.getElementById('diff'+idx).innerHTML = '📝 发音对比：'+renderDiff(diff);
                alert(`识别: "${trans}"\n正确率: ${acc}%`);
            };
            rec.onerror = (e)=> alert('Ошибка: '+e.error);
        }

        function startFullTest() {
            if (!requireActivation()) return;
            if(!fullOriginalText) return;
            const SR = window.SpeechRecognition||webkitSpeechRecognition;
            if(!SR) return alert('Нужен Chrome');
            fullRecognition = new SR(); fullRecognition.lang='ru-RU'; fullRecognition.continuous=true; fullRecognition.interimResults=true;
            fullRecognizedText='';
            fullRecognition.onresult = (e)=>{ for(let i=e.resultIndex;i<e.results.length;i++) if(e.results[i].isFinal) fullRecognizedText+=e.results[i][0].transcript+' '; };
            fullRecognition.onerror = (e)=>alert('Ошибка: '+e.error);
            fullRecognition.onend = ()=>{ if(isFullTestRunning) stopFullTest(); };
            fullRecognition.start(); isFullTestRunning=true; fullTestStartTime=Date.now();
            document.getElementById('startFullTestBtn').disabled=true;
            document.getElementById('stopFullTestBtn').disabled=false;
            document.getElementById('fullTestResult').innerHTML='';
            fullTestTimerInterval = setInterval(()=>{ document.getElementById('fullTimer').innerText = formatTime((Date.now()-fullTestStartTime)/1000); },200);
        }

        function stopFullTest() {
            if(!isFullTestRunning) return;
            if(fullRecognition) fullRecognition.stop();
            isFullTestRunning=false; clearInterval(fullTestTimerInterval);
            const elapsed = (Date.now()-fullTestStartTime)/1000;
            document.getElementById('fullTimer').innerText = formatTime(elapsed);
            document.getElementById('startFullTestBtn').disabled=false;
            document.getElementById('stopFullTestBtn').disabled=true;
            const diff = wordDiff(fullOriginalText, fullRecognizedText);
            const acc = wordAccuracyFromDiff(diff);
            const ok = elapsed<=600 && acc>=90;
            const div = document.getElementById('fullTestResult');
            div.className = 'result '+(ok?'pass':'fail');
            div.innerHTML = (ok?`✅ 达标！时间：${formatTime(elapsed)}，准确率：${acc}%`:`❌ 未达标。${elapsed>600?'超时'+formatTime(elapsed):''} ${acc<90?'准确率'+acc+'%':''}`)
                +'<br><small>📝 全文对比：'+renderDiff(diff)+'</small>';
            if(ok) document.getElementById('congratsModal').style.display = 'flex';
        }

        function closeCongrats() { document.getElementById('congratsModal').style.display = 'none'; }

        // ========== 帮助弹窗 ==========
        function openHelp() { document.getElementById('helpModal').style.display = 'flex'; }
        function closeHelp() { document.getElementById('helpModal').style.display = 'none'; }
        document.addEventListener('click', function(e) {
            if (e.target.id === 'helpModal') closeHelp();
            if (e.target.id === 'congratsModal') closeCongrats();
            // 注意：激活弹窗不点击遮罩关闭，只能通过按钮
        });

        // ========== 初始化 ==========
        window.addEventListener('DOMContentLoaded', () => {
            startCountdown();
            // 如果未激活，不自动弹窗（等用户点击需要权限的功能时再弹）
        });
    </script>
</body>
</html>
