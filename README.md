<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


    { "en": "Apple Inc.?", "id": "Amerika Serikat." },
    { "en": "Microsoft Corporation?", "id": "Amerika Serikat." },
    { "en": "Alphabet Inc. (Google)?", "id": "Amerika Serikat." },
    { "en": "Amazon.com, Inc.?", "id": "Amerika Serikat." },
    { "en": "Meta Platforms, Inc. (Facebook)?", "id": "Amerika Serikat." },
    { "en": "NVIDIA Corporation?", "id": "Amerika Serikat." },
    { "en": "Tesla, Inc.?", "id": "Amerika Serikat." },
    { "en": "Samsung Electronics?", "id": "Korea Selatan." },
    { "en": "Tencent Holdings Ltd.?", "id": "Tiongkok." },
    { "en": "Alibaba Group?", "id": "Tiongkok." },
    { "en": "Taiwan Semiconductor Manufacturing Company (TSMC)?", "id": "Taiwan." },
    { "en": "Sony Group Corporation?", "id": "Jepang." },
    { "en": "Huawei Technologies Co., Ltd.?", "id": "Tiongkok." },
    { "en": "Intel Corporation?", "id": "Amerika Serikat." },
    { "en": "Oracle Corporation?", "id": "Amerika Serikat." },
    { "en": "Adobe Inc.?", "id": "Amerika Serikat." },
    { "en": "SAP SE?", "id": "Jerman." },
    { "en": "ASML Holding?", "id": "Belanda." },
    { "en": "International Business Machines Corporation (IBM)?", "id": "Amerika Serikat." },
    { "en": "Cisco Systems, Inc.?", "id": "Amerika Serikat." },
    { "en": "Qualcomm Incorporated?", "id": "Amerika Serikat." },
    { "en": "Broadcom Inc.?", "id": "Amerika Serikat." },
    { "en": "Netflix, Inc.?", "id": "Amerika Serikat." },
    { "en": "Salesforce, Inc.?", "id": "Amerika Serikat." },
    { "en": "Advanced Micro Devices, Inc. (AMD)?", "id": "Amerika Serikat." },
    { "en": "LG Electronics?", "id": "Korea Selatan." },
    { "en": "Panasonic Corporation?", "id": "Jepang." },
    { "en": "Nintendo Co., Ltd.?", "id": "Jepang." },
    { "en": "ByteDance Ltd. (TikTok)?", "id": "Tiongkok." },
    { "en": "Xiaomi Corporation?", "id": "Tiongkok." },
    { "en": "Baidu, Inc.?", "id": "Tiongkok." },
    { "en": "JD.com (Jingdong)?", "id": "Tiongkok." },
    { "en": "Spotify Technology S.A.?", "id": "Swedia." },
    { "en": "Nokia Corporation?", "id": "Finlandia." },
    { "en": "Telefonaktiebolaget LM Ericsson?", "id": "Swedia." },
    { "en": "Tata Consultancy Services (TCS)?", "id": "India." },
    { "en": "Infosys Limited?", "id": "India." },
    { "en": "Mercado Libre?", "id": "Argentina." },
    { "en": "Dell Technologies?", "id": "Amerika Serikat." },
    { "en": "Hewlett-Packard (HP) Inc.?", "id": "Amerika Serikat." },
    { "en": "SK Hynix?", "id": "Korea Selatan." },
    { "en": "SoftBank Group Corp.?", "id": "Jepang." },
    { "en": "Canon Inc.?", "id": "Jepang." },
    { "en": "Fujitsu Limited?", "id": "Jepang." },
    { "en": "Lenovo Group Limited?", "id": "Tiongkok." },
    { "en": "GoPro, Inc.?", "id": "Amerika Serikat." },
    { "en": "Zoom Video Communications, Inc.?", "id": "Amerika Serikat." },
    { "en": "Shopify Inc.?", "id": "Kanada." },
    { "en": "Atlassian Corporation?", "id": "Australia." },
    { "en": "Siemens AG?", "id": "Jerman." },
    { "en": "Dassault Systèmes?", "id": "Prancis." },
    { "en": "Capgemini SE?", "id": "Prancis." },
    { "en": "Check Point Software Technologies Ltd.?", "id": "Israel." },
    { "en": "Wix.com Ltd.?", "id": "Israel." },
    { "en": "Kaspersky Lab?", "id": "Rusia." },
    { "en": "Yandex N.V.?", "id": "Rusia." },
    { "en": "PayPal Holdings, Inc.?", "id": "Amerika Serikat." },
    { "en": "Accenture plc?", "id": "Irlandia." },
    { "en": "Texas Instruments Inc.?", "id": "Amerika Serikat." },
    { "en": "Micron Technology, Inc.?", "id": "Amerika Serikat." },
    { "en": "Applied Materials, Inc.?", "id": "Amerika Serikat." },
    { "en": "Sony Interactive Entertainment (PlayStation)?", "id": "Jepang." },
    { "en": "Activision Blizzard?", "id": "Amerika Serikat." },
    { "en": "Electronic Arts Inc.?", "id": "Amerika Serikat." },
    { "en": "Take-Two Interactive Software, Inc.?", "id": "Amerika Serikat." },
    { "en": "Sea Ltd (Shopee & Garena)?", "id": "Singapura." },
    { "en": "Pinduoduo Inc.?", "id": "Tiongkok." },
    { "en": "Meituan?", "id": "Tiongkok." },
    { "en": "Rakuten Group, Inc.?", "id": "Jepang." },
    { "en": "eBay Inc.?", "id": "Amerika Serikat." },
    { "en": "Booking Holdings Inc.?", "id": "Amerika Serikat." },
    { "en": "Airbnb, Inc.?", "id": "Amerika Serikat." },
    { "en": "Uber Technologies, Inc.?", "id": "Amerika Serikat." },
    { "en": "ServiceNow, Inc.?", "id": "Amerika Serikat." },
    { "en": "Intuit Inc.?", "id": "Amerika Serikat." },
    { "en": "Block, Inc. (Square)?", "id": "Amerika Serikat." },
    { "en": "Adyen N.V.?", "id": "Belanda." },
    { "en": "Schneider Electric?", "id": "Prancis." },
    { "en": "Arm Holdings plc?", "id": "Inggris." },
    { "en": "Keyence Corporation?", "id": "Jepang." },
    { "en": "MediaTek Inc.?", "id": "Taiwan." },
    { "en": "ZTE Corporation?", "id": "Tiongkok." },
    { "en": "VMware, Inc.?", "id": "Amerika Serikat." },
    { "en": "Palo Alto Networks, Inc.?", "id": "Amerika Serikat." },
    { "en": "Fortinet, Inc.?", "id": "Amerika Serikat." },
    { "en": "Analog Devices, Inc.?", "id": "Amerika Serikat." },
    { "en": "Lam Research Corporation?", "id": "Amerika Serikat." },
    { "en": "Autodesk, Inc.?", "id": "Amerika Serikat." },
    { "en": "Cognizant Technology Solutions Corp.?", "id": "Amerika Serikat." },
    { "en": "Xerox Corporation?", "id": "Amerika Serikat." },
    { "en": "Western Digital Corporation?", "id": "Amerika Serikat." },
    { "en": "Seagate Technology?", "id": "Amerika Serikat." },
    { "en": "Dassault Aviation?", "id": "Prancis." },
    { "en": "Foxconn (Hon Hai Precision Industry)?", "id": "Taiwan." },
    { "en": "Hitachi, Ltd.?", "id": "Jepang." },
    { "en": "NEC Corporation?", "id": "Jepang." },
    { "en": "Infineon Technologies AG?", "id": "Jerman." },
    { "en": "STMicroelectronics N.V.?", "id": "Swiss." },
    { "en": "NXP Semiconductors N.V.?", "id": "Belanda." },
    { "en": "Corning Incorporated?", "id": "Amerika Serikat." },
    { "en": "Naver Corporation?", "id": "Korea Selatan." },
    { "en": "Kakao?", "id": "Korea Selatan." },
    { "en": "Grab Holdings Inc.?", "id": "Singapura." },
    { "en": "GoTo (Gojek & Tokopedia)?", "id": "Indonesia." },
    { "en": "Palantir Technologies Inc.?", "id": "Amerika Serikat." },
    { "en": "Snowflake Inc.?", "id": "Amerika Serikat." },
    { "en": "CrowdStrike Holdings, Inc.?", "id": "Amerika Serikat." },
    { "en": "Workday, Inc.?", "id": "Amerika Serikat." },
    { "en": "Fiserv, Inc.?", "id": "Amerika Serikat." },
    { "en": "Fidelity National Information Services (FIS)?", "id": "Amerika Serikat." },
    { "en": "Global Payments Inc.?", "id": "Amerika Serikat." },
    { "en": "Juniper Networks, Inc.?", "id": "Amerika Serikat." },
    { "en": "Arista Networks?", "id": "Amerika Serikat." },
    { "en": "Akamai Technologies, Inc.?", "id": "Amerika Serikat." },
    { "en": "Synopsys, Inc.?", "id": "Amerika Serikat." },
    { "en": "Cadence Design Systems, Inc.?", "id": "Amerika Serikat." },
    { "en": "Ansys, Inc.?", "id": "Amerika Serikat." },
    { "en": "Dassault Systèmes?", "id": "Prancis." },
    { "en": "Garmin Ltd.?", "id": "Swiss." },
    { "en": "Bose Corporation?", "id": "Amerika Serikat." },
    { "en": "Wipro Limited?", "id": "India." },
    { "en": "HCL Technologies?", "id": "India." },
    { "en": "Amadeus IT Group?", "id": "Spanyol." },
    { "en": "Logitech International S.A.?", "id": "Swiss." },
    { "en": "BlackBerry Limited?", "id": "Kanada." },
    { "en": "Electronic Arts?", "id": "Amerika Serikat." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
