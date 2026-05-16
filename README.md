# company-stage-test<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Тест: этап вашей компании</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            background: #eef4ea;
            font-family: 'Segoe UI', Roboto, system-ui, sans-serif;
            padding: 30px 20px;
        }
        .container {
            max-width: 1100px;
            margin: 0 auto;
        }
        h1 {
            font-size: 36px;
            color: #1a3c26;
            text-align: center;
            margin-bottom: 8px;
        }
        .sub {
            text-align: center;
            color: #4a6741;
            margin-bottom: 40px;
        }
        .grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 24px;
        }
        .stage-card {
            background: white;
            border-radius: 28px;
            padding: 20px 24px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.05);
            border: 1px solid #deecd4;
        }
        .stage-title {
            font-size: 24px;
            font-weight: 800;
            color: #2b5e2f;
            margin-bottom: 4px;
        }
        .stage-desc {
            font-size: 13px;
            color: #6b8a5c;
            margin-bottom: 20px;
            border-bottom: 1px solid #e0ecd7;
            padding-bottom: 12px;
        }
        .question {
            margin: 16px 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 12px;
            background: #fafef7;
            padding: 8px 12px;
            border-radius: 60px;
        }
        .question-text {
            font-size: 14px;
            line-height: 1.4;
            flex: 1;
        }
        .btns {
            display: flex;
            gap: 8px;
        }
        .btn-yes, .btn-no {
            border: none;
            padding: 6px 18px;
            border-radius: 40px;
            font-weight: 600;
            cursor: pointer;
            background: #eef2ea;
            color: #2b4b2a;
            transition: 0.1s;
        }
        .btn-yes.active {
            background: #2b7a3e;
            color: white;
        }
        .btn-no.active {
            background: #b0aa7c;
            color: white;
        }
        .result {
            background: #f0f7ed;
            border-radius: 32px;
            padding: 24px 28px;
            margin-top: 32px;
            text-align: center;
            border: 1px solid #c6ddba;
        }
        .result-stage {
            font-size: 28px;
            font-weight: 800;
            margin-bottom: 12px;
        }
        .result-leader {
            font-size: 18px;
            background: white;
            display: inline-block;
            padding: 8px 24px;
            border-radius: 60px;
            margin: 8px 0;
        }
        button.reset-btn {
            background: #4f6b44;
            border: none;
            color: white;
            padding: 10px 28px;
            border-radius: 40px;
            font-weight: 600;
            margin-top: 16px;
            cursor: pointer;
        }
        @media (max-width: 800px) {
            .grid { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>
<div class="container">
    <h1>📊 На каком этапе ваша компания?</h1>
    <div class="sub">Отметьте «Да» или «Нет» в каждом блоке — узнайте, какой лидер вам нужен</div>

    <div class="grid" id="quizGrid"></div>
    <div class="result" id="resultBlock">
        <div style="font-size: 18px;">✅ Отвечайте на вопросы — результат появится автоматически</div>
    </div>
</div>

<script>
    const stages = [
        {
            id: "startup",
            title: "Стартап / Посев",
            desc: "поиск идеи, хаос, быстрые тесты",
            questions: [
                "У нас нет чётко отлаженных процессов, много импровизации.",
                "Ошибки случаются часто, и это считается нормой.",
                "Главный приоритет — быстрее проверить идею, а не стабильная прибыль.",
                "Команда маленькая (до 10–15 человек), все «носят несколько шляп».",
                "Финансовый результат может сильно колебаться от месяца к месяцу."
            ],
            leader: "🟢 Молодой лидер (20–26 лет) — предпринимательский, смелый, скорость решений"
        },
        {
            id: "growth",
            title: "Рост / Масштабирование",
            desc: "высокий темп, найм, первые процессы",
            questions: [
                "Мы быстро нанимаем людей (открыто 3+ позиции в месяц).",
                "Появляются первые стандарты, но их часто нарушают.",
                "Спрос растёт, и мы не всегда успеваем за ним операционно.",
                "Уже есть чёткое разделение ролей, но зоны ответственности могут пересекаться.",
                "Руководители жалуются, что время на обучение отнимает от основной работы."
            ],
            leader: "🟡 Тандем: молодой + взрослый наставник (24–35 лет) — баланс энергии и системы"
        },
        {
            id: "maturity",
            title: "Зрелость / Стабильность",
            desc: "отработанные стандарты, эффективность, контроль",
            questions: [
                "Все ключевые процессы прописаны и выполняются одинаково.",
                "Текучка низкая (менеджеры работают 3+ года).",
                "Главные KPI – эффективность, снижение издержек, стабильное качество.",
                "Новые идеи проходят долгие согласования и пилоты.",
                "Сотрудники чётко знают свои обязанности и редко выходят за их пределы."
            ],
            leader: "🔵 Взрослый лидер (30–45 лет) — системный, предсказуемый, процессный"
        },
        {
            id: "renewal",
            title: "Обновление / Трансформация",
            desc: "стагнация, нужен новый рывок",
            questions: [
                "За последние 1–2 года выручка перестала расти (или падает).",
                "Клиенты стали уходить к конкурентам, которые делают что-то иначе.",
                "Старые продукты/услуги теряют актуальность, нужны новые.",
                "Команда сопротивляется изменениям, говорит «у нас и так всё работает».",
                "Мы уже пробовали внедрять инновации, но безуспешно."
            ],
            leader: "🟢 Снова молодой (22–28 лет) / внешний дисраптор — инноватор, бросающий вызов устоявшемуся"
        }
    ];

    let answers = {};
    stages.forEach(stage => { answers[stage.id] = new Array(5).fill(null); });

    function renderQuiz() {
        const grid = document.getElementById('quizGrid');
        grid.innerHTML = '';
        stages.forEach((stage, sIdx) => {
            const card = document.createElement('div');
            card.className = 'stage-card';
            card.innerHTML = `<div class="stage-title">${stage.title}</div><div class="stage-desc">${stage.desc}</div>`;
            stage.questions.forEach((q, qIdx) => {
                const qDiv = document.createElement('div');
                qDiv.className = 'question';
                qDiv.innerHTML = `
                    <div class="question-text">${q}</div>
                    <div class="btns">
                        <button class="btn-yes" data-stage="${stage.id}" data-q="${qIdx}" data-val="true">Да</button>
                        <button class="btn-no" data-stage="${stage.id}" data-q="${qIdx}" data-val="false">Нет</button>
                    </div>
                `;
                card.appendChild(qDiv);
            });
            grid.appendChild(card);
        });
        attachEvents();
        updateResult();
    }

    function attachEvents() {
        document.querySelectorAll('.btn-yes, .btn-no').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const stageId = btn.dataset.stage;
                const qIdx = parseInt(btn.dataset.q);
                const value = btn.dataset.val === 'true';
                answers[stageId][qIdx] = value;
                highlightButtons(stageId, qIdx);
                updateResult();
            });
        });
    }

    function highlightButtons(stageId, qIdx) {
        const cards = document.querySelectorAll('.stage-card');
        const stageIndex = stages.findIndex(s => s.id === stageId);
        if (stageIndex === -1) return;
        const card = cards[stageIndex];
        const qDivs = card.querySelectorAll('.question');
        const targetQ = qDivs[qIdx];
        const yesBtn = targetQ.querySelector('.btn-yes');
        const noBtn = targetQ.querySelector('.btn-no');
        const isYes = answers[stageId][qIdx] === true;
        if (isYes) { yesBtn.classList.add('active'); noBtn.classList.remove('active'); }
        else if (answers[stageId][qIdx] === false) { noBtn.classList.add('active'); yesBtn.classList.remove('active'); }
        else { yesBtn.classList.remove('active'); noBtn.classList.remove('active'); }
    }

    function computeScores() {
        let scores = {};
        stages.forEach(stage => {
            const countYes = answers[stage.id].filter(v => v === true).length;
            scores[stage.id] = countYes;
        });
        return scores;
    }

    function getWinnerStageId(scores) {
        let maxCount = -1;
        let winners = [];
        for (let [id, count] of Object.entries(scores)) {
            if (count > maxCount) { maxCount = count; winners = [id]; }
            else if (count === maxCount && maxCount !== -1) { winners.push(id); }
        }
        if (winners.length > 1) return 'transition';
        return winners[0];
    }

    function updateResult() {
        const scores = computeScores();
        const winnerId = getWinnerStageId(scores);
        const resultDiv = document.getElementById('resultBlock');
        if (winnerId === 'transition') {
            resultDiv.innerHTML = `<div class="result-stage">🔄 Переходная фаза</div><div class="result-leader">🔁 Рекомендуем тандем или смену стиля управления</div><button class="reset-btn" id="resetBtn">⟳ Пройти заново</button>`;
            const resetBtn = document.getElementById('resetBtn');
            if (resetBtn) resetBtn.onclick = resetAll;
            return;
        }
        const stage = stages.find(s => s.id === winnerId);
        resultDiv.innerHTML = `
            <div class="result-stage">${stage.title}</div>
            <div class="result-leader">🎯 Рекомендуемый лидер: ${stage.leader}</div>
            <div style="margin-top: 12px;">✅ Совпадений: ${scores[winnerId]} из 5</div>
            <button class="reset-btn" id="resetBtn">⟳ Пройти заново</button>
        `;
        const resetBtn = document.getElementById('resetBtn');
        if (resetBtn) resetBtn.onclick = resetAll;
    }

    function resetAll() {
        stages.forEach(stage => { answers[stage.id] = new Array(5).fill(null); });
        renderQuiz();
    }

    renderQuiz();
</script>
</body>
</html>
