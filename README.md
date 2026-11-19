# mlodyg67.github.io
<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Nihongo Quiz — słówka, liczby i wyjątki</title>
  <style>
    :root { color-scheme: light; --ink:#202638; --muted:#758095; --border:#e6e9f0; --accent:#6557e8; --green:#159a6a; --red:#e24d66; --bg:#f5f6fb; }
    * { box-sizing: border-box; }
    body { margin:0; min-height:100vh; background: radial-gradient(circle at 10% 0%, #eae6ff 0, transparent 31%),var(--bg); color:var(--ink); font-family:system-ui,-apple-system,"Segoe UI",sans-serif; }
    button,input { font:inherit; }
    button { cursor:pointer; }
    .shell { max-width:980px; margin:auto; padding:28px 22px 72px; }
    header { display:flex; justify-content:space-between; align-items:center; gap:15px; }
    .brand { display:flex; align-items:center; gap:12px; font-size:17px; font-weight:800; }
    .brand-mark { width:42px; height:42px; display:grid; place-items:center; border-radius:14px; background:#6557e8; color:white; font-size:21px; box-shadow:0 9px 18px #6557e82c; }
    .menu-wrap { position:relative; }
    .menu-button { display:flex; align-items:center; gap:8px; padding:11px 15px; border:1px solid var(--border); background:white; border-radius:13px; font-weight:650; color:var(--ink); }
    .dropdown { position:absolute; right:0; top:calc(100% + 10px); z-index:5; min-width:205px; padding:7px; border:1px solid var(--border); border-radius:14px; background:white; box-shadow:0 15px 42px #2629491c; }
    .dropdown[hidden], .overlay[hidden], .feedback[hidden] { display:none; }
    .dropdown button { display:block; width:100%; background:none; border:0; border-radius:9px; padding:11px; text-align:left; font-weight:600; color:var(--ink); }
    .dropdown button:hover { background:#f5f4ff; }
    .main { max-width:635px; margin:88px auto 0; }
    .eyebrow { display:flex; align-items:center; justify-content:center; gap:8px; color:var(--accent); font-size:12px; letter-spacing:1.8px; font-weight:800; text-transform:uppercase; }
    h1 { margin:12px 0 10px; text-align:center; letter-spacing:-1.8px; font-size:clamp(32px,5vw,49px); line-height:1.15; }
    .subtitle { margin:0 0 38px; text-align:center; color:var(--muted); }
    .card { background:white; padding:clamp(24px,5vw,42px); border:1px solid #e8eaf1; border-radius:26px; box-shadow:0 22px 65px #292e5210; }
    .card-top { display:flex; justify-content:space-between; align-items:center; gap:12px; margin-bottom:28px; }
    .tag { font-size:12px; font-weight:750; border-radius:30px; padding:8px 12px; color:#6052da; background:#f0edff; }
    .progress { font-weight:650; font-size:13px; color:var(--muted); }
    .question-label { color:var(--muted); font-size:14px; text-align:center; font-weight:600; }
    .translation { margin:12px 0 10px; font-size:clamp(35px,6vw,51px); text-align:center; letter-spacing:-1.2px; font-weight:800; overflow-wrap:anywhere; }
    .hint { text-align:center; color:#9ca4b4; font-size:13px; margin:0 0 30px; }
    label[for="answer"] { display:block; font-size:13px; font-weight:750; margin-bottom:10px; }
    .answer { width:100%; padding:17px 18px; border:2px solid #e2e5ee; border-radius:14px; background:#fff; outline:none; font-size:24px; transition:border-color .13s,box-shadow .13s; }
    .answer::placeholder { color:#b6bdca; font-size:17px; }
    .answer:focus { border-color:#a8a1ef; box-shadow:0 0 0 4px #6557e813; }
    .answer.wrong { border-color:var(--red); background:#fffafb; box-shadow:0 0 0 4px #e24d6610; }
    .answer.correct { border-color:var(--green); background:#f8fffb; box-shadow:0 0 0 4px #159a6a12; }
    .feedback { margin-top:15px; padding:15px 17px; border-radius:13px; background:#effaf4; color:#16754f; }
    .feedback strong { display:block; font-size:13px; margin-bottom:5px; }
    .reading { font-size:23px; font-weight:800; }
    .foot { display:flex; justify-content:space-between; align-items:center; gap:16px; margin-top:25px; }
    .helper { font-size:12px; color:var(--muted); line-height:1.4; }
    .next { border:0; border-radius:12px; background:var(--accent); padding:13px 20px; color:white; font-weight:750; white-space:nowrap; }
    .next:hover:not(:disabled) { background:#5141d3; }
    .next:disabled { opacity:.45; cursor:not-allowed; }
    .skip { border:1px solid var(--border); border-radius:12px; background:white; padding:12px 18px; color:var(--ink); font-weight:700; }
    .skip:hover:not(:disabled) { background:#f4f2ff; }
    .skip:disabled { opacity:.45; cursor:not-allowed; }
    .foot-actions { display:flex; flex-wrap:wrap; justify-content:flex-end; gap:9px; }
    .feedback.revealed { background:#f1f3ff; color:#38317f; }
    .feedback .reveal-line { margin-top:7px; }
    .jp-revealed { font-size:26px; font-weight:800; }
    .stats { margin-top:23px; display:flex; justify-content:center; flex-wrap:wrap; gap:24px; color:var(--muted); font-size:13px; }
    .stats strong { color:var(--ink); }
    .overlay { position:fixed; inset:0; z-index:10; background:#2024398c; display:grid; place-items:center; padding:18px; }
    .modal { max-height:90vh; overflow-y:auto; width:min(100%,460px); background:white; border-radius:23px; padding:26px; box-shadow:0 25px 90px #1118; }
    .modal-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:7px; }
    .modal h2 { font-size:23px; margin:0; }
    .close { border:0; background:#f2f3f7; border-radius:10px; width:34px; height:34px; font-size:21px; }
    .modal p { font-size:13px; color:var(--muted); line-height:1.5; margin:0 0 22px; }
    .field { margin-bottom:17px; }
    .field label { display:block; margin-bottom:8px; font-size:13px; font-weight:700; }
    .field input { width:100%; padding:12px 14px; border:1.5px solid #dee2ec; border-radius:11px; outline:none; }
    .field input:focus { border-color:var(--accent); }
    .save { width:100%; border:0; border-radius:12px; padding:14px; color:white; background:var(--accent); font-weight:750; }
    .notice { font-size:13px; margin-top:12px; min-height:17px; color:var(--green); }
    .database-modal { width:min(100%,780px); }
    .db-tools { display:flex; gap:10px; margin:15px 0; }
    .db-tools input { width:100%; border:1.5px solid var(--border); border-radius:11px; padding:12px; outline:none; }
    .backup-actions { display:flex; flex-wrap:wrap; gap:9px; margin:0 0 10px; }
    .backup-actions button { border:1px solid var(--border); border-radius:10px; background:#f7f6ff; color:#4035a5; padding:11px 14px; font-weight:750; }
    .backup-actions button:hover { background:#eeebff; }
    .backup-status { min-height:20px; font-size:13px; color:var(--muted); margin:0 0 12px; }
    .backup-help { font-size:12px; line-height:1.55; color:var(--muted); margin:0 0 12px; }
    .backup-actions .save-file { color:#fff; background:var(--accent); border-color:var(--accent); }
    .backup-actions .save-file:hover { background:#5141d3; }
    .db-list { display:grid; gap:9px; max-height:55vh; overflow-y:auto; }
    .db-item { display:flex; justify-content:space-between; align-items:center; gap:12px; padding:12px; border:1px solid var(--border); border-radius:13px; }
    .db-jp { font-size:21px; font-weight:750; overflow-wrap:anywhere; }
    .db-detail { color:var(--muted); font-size:13px; overflow-wrap:anywhere; }
    .db-actions { display:flex; gap:7px; flex-shrink:0; }
    .db-actions button { border:1px solid var(--border); background:#fff; border-radius:9px; padding:8px 10px; font-weight:650; }
    .db-actions .delete { color:var(--red); }
    .empty { text-align:center; color:var(--muted); padding:30px 5px; }
    @media(max-width:600px){ .shell{padding:18px 15px 40px}.main{margin-top:58px}.card{border-radius:20px}.foot{align-items:flex-start}.stats{gap:13px}.helper{max-width:180px} }

    /* Dwa niezależne tryby: słówka i liczby */
    [hidden] { display:none !important; }
    .tabbar { display:flex; gap:8px; max-width:635px; margin:38px auto 0; padding:6px; background:#eaeaf5; border-radius:17px; }
    .tab { flex:1; background:transparent; border:0; border-radius:12px; padding:14px 12px; font-size:14px; font-weight:800; color:#697389; transition:background .15s, box-shadow .15s; }
    .tab[aria-selected="true"] { background:white; color:#5144cb; box-shadow:0 3px 12px #22233a14; }
    .tab:focus-visible,.category:focus-visible,.next:focus-visible,.skip:focus-visible { outline:3px solid #8e80fd; outline-offset:2px; }
    .main { margin-top:42px; }
    .number-categories { display:grid; grid-template-columns:repeat(3,minmax(0,1fr)); gap:11px; margin:28px 0 24px; }
    .category { display:flex; flex-direction:column; gap:7px; text-align:left; padding:17px; background:white; border:2px solid #e7e7f1; border-radius:18px; color:var(--ink); min-height:148px; }
    .category .category-icon { font-size:23px; line-height:1.1; }
    .category strong { font-size:13px; line-height:1.35; }
    .category small { font-size:11px; color:var(--muted); line-height:1.4; }
    .category[aria-pressed="true"] { border-color:var(--accent); box-shadow:0 0 0 3px #6557e815; background:#faf9ff; }
    .category[aria-pressed="true"] strong { color:#5144cb; }
    .number-drills { display:flex; align-items:center; justify-content:space-between; gap:12px; padding:14px 16px; background:#fff; border:1px solid var(--border); border-radius:15px; margin:-7px 0 16px; flex-wrap:wrap; }
    .drill-intro { display:flex; gap:9px; align-items:center; font-size:13px; font-weight:800; color:#50586c; }
    .drill-selector { display:flex; gap:5px; background:#efeff8; border-radius:11px; padding:5px; flex-wrap:wrap; }
    .drill-button { border:0; background:transparent; color:#6e758a; padding:9px 12px; border-radius:8px; font-size:12px; font-weight:800; }
    .drill-button[aria-pressed="true"] { background:#fff; color:#4f3ec0; box-shadow:0 2px 8px #25264b15; }
    .drill-button:focus-visible { outline:3px solid #8e80fd; outline-offset:2px; }
    .drill-button:disabled { opacity:.45; cursor:not-allowed; box-shadow:none; }
    .range-note { width:100%; color:#747c90; font-size:12px; line-height:1.5; margin:0; }
    .number-ranges { margin-top:-8px; }
    .number-ranges .drill-selector { max-width:100%; }
    .number-ranges .drill-button { white-space:nowrap; }
    .exception-rule { margin-top:13px; padding:11px 13px; background:#fffbec; border:1px solid #f5e3a7; color:#66501b; border-radius:10px; font-size:13px; line-height:1.6; }
    .number-info { background:#f1f0ff; color:#494380; border:1px solid #e1ddff; border-radius:14px; padding:13px 15px; margin-bottom:22px; font-size:13px; line-height:1.6; }
    .number-info strong { display:block; font-size:14px; margin-bottom:3px; }
    .num-answer-label { display:block; font-size:13px; font-weight:750; margin-bottom:10px; }
    .number-feedback { margin-top:15px; padding:15px 17px; border-radius:13px; background:#effaf4; color:#16754f; }
    .number-feedback.revealed { background:#f1f3ff; color:#38317f; }
    .number-feedback strong { display:block; font-size:13px; margin-bottom:8px; }
    .num-japanese { font-size:27px; font-weight:850; margin:5px 0 9px; word-break:break-word; }
    .num-detail { font-size:14px; margin-top:6px; line-height:1.5; }
    .num-detail b { font-size:17px; }
    .number-guide { margin-top:20px; padding:14px 18px; background:#fff; border:1px solid var(--border); border-radius:14px; font-size:13px; color:#555e72; line-height:1.65; }
    .number-guide summary { cursor:pointer; font-weight:750; color:#4d45a0; }
    .number-guide p { margin:10px 0 0; }
    .hint-note { font-size:12px; text-align:center; color:var(--muted); margin:12px 0 0; }
    .foot .helper { flex:1; min-width:0; max-width:none; }
    .foot .foot-actions { flex-shrink:0; flex-wrap:nowrap; }
    .category small {font-size:12px; }
    /* Osiem kategorii liczbowych — czytelna siatka na każdym ekranie */
    #numberPanel { max-width:800px; }
    #numberPanel > .card, #numberPanel > .stats, #numberPanel > .number-guide,
    #numberPanel > .number-info, #numberPanel > .number-drills { max-width:635px; margin-left:auto; margin-right:auto; }
    #numberPanel .number-categories { grid-template-columns:repeat(4,minmax(0,1fr)); }
    #numberPanel .category { min-height:148px; }
    #numberPanel .category strong { font-size:13px; }
    #numberPanel .category small { font-size:11.5px; }
    @media(max-width:750px){ #numberPanel .number-categories { grid-template-columns:repeat(2,minmax(0,1fr)); } #numberPanel .category { min-height:135px; } }
    @media(max-width:470px){ #numberPanel .number-categories { grid-template-columns:1fr; } #numberPanel .category { min-height:unset; } }
    @media(max-width:650px) { .number-drills { align-items:stretch; } .drill-intro { width:100%; } .drill-selector { flex:1; } .drill-button { flex:1; } .foot { flex-direction:column; align-items:stretch; gap:14px; } .foot .foot-actions { flex-wrap:wrap; justify-content:stretch; } .foot .foot-actions button { flex:1 1 120px; } .number-categories { grid-template-columns:1fr; } .category { flex-direction:row; min-height:unset; align-items:center; padding:13px; flex-wrap:wrap; } .category .category-icon { flex:0 0 28px; } .category strong { font-size:14px; } .category small { flex-basis:100%; margin-left:35px; } .tabbar { margin-top:25px; } .main { margin-top:35px; } }

    /* Trzeci moduł: ćwiczenia z cząsteczek — przyjazne dla początkujących */
    #grammarPanel { max-width:790px; }
    .grammar-lesson { display:flex; align-items:flex-start; gap:12px; background:#f0efff; border:1px solid #dedafa; border-radius:15px; padding:15px 17px; color:#47417d; font-size:13px; line-height:1.65; margin:0 0 20px; }
    .grammar-lesson strong { display:block; font-size:14px; }
    .grammar-groups { display:grid; grid-template-columns:repeat(4,minmax(0,1fr)); gap:9px; margin:0 0 10px; }
    .grammar-group { text-align:left; display:flex; align-items:center; justify-content:space-between; gap:9px; border:1.5px solid #e5e5f0; background:white; border-radius:13px; padding:12px 13px; color:#50576a; min-height:54px; }
    .grammar-group .grammar-label { display:flex; flex-direction:column; gap:2px; min-width:0; flex:1; }
    .grammar-group .grammar-kana { font-size:19px; font-weight:800; color:#45415f; }
    .grammar-group .grammar-selected { display:inline-grid; place-items:center; border:1.5px solid #d5d7e3; width:21px; height:21px; border-radius:6px; flex:0 0 auto; font-size:13px; font-weight:900; color:transparent; background:#fff; }
    .grammar-group[aria-pressed="true"] .grammar-selected { border-color:var(--accent); color:white; background:var(--accent); }
    .grammar-selection-summary { color:#616b80; font-size:12px; line-height:1.55; margin:0 0 22px; }
    .grammar-group .grammar-roman { font-size:12px; color:#9095a4; }
    .grammar-group[aria-pressed="true"] { border-color:var(--accent); background:#f4f1ff; color:#4f42c9; box-shadow:0 0 0 2px #6658e814; }
    .grammar-group[aria-pressed="true"] .grammar-kana { color:#5243ce; }
    .grammar-main { max-width:635px; margin:auto; }
    .grammar-top-row { display:flex; gap:12px; align-items:center; justify-content:space-between; flex-wrap:wrap; margin-bottom:12px; }
    .grammar-style-options { display:flex; gap:5px; border-radius:11px; padding:5px; background:#efeff8; }
    .grammar-style-options button { border:0; border-radius:8px; padding:8px 12px; font-size:12px; font-weight:750; background:transparent; color:#647087; }
    .grammar-style-options button[aria-pressed="true"] { color:#5542cd; background:white; box-shadow:0 2px 8px #20203015; }
    .grammar-sentence { text-align:center; font-size:clamp(23px,4.3vw,32px); font-weight:750; line-height:1.7; margin:12px 0 6px; overflow-wrap:anywhere; }
    .grammar-romaji { color:#636e83; text-align:center; font-size:16px; line-height:1.5; margin:0 0 16px; overflow-wrap:anywhere; }
    .grammar-polish { background:#f6f6fa; padding:13px 15px; border-radius:11px; text-align:center; color:#525c72; font-size:14px; line-height:1.55; margin:0 0 23px; }
    .grammar-options { display:grid; grid-template-columns:repeat(4,minmax(0,1fr)); gap:10px; }
    .grammar-choice { border:2px solid #e7e8f1; border-radius:13px; background:white; padding:12px 5px; color:var(--ink); min-height:80px; display:flex; flex-direction:column; justify-content:center; align-items:center; gap:4px; }
    .grammar-choice span { font-size:25px; font-weight:800; }
    .grammar-choice small { color:var(--muted); font-size:12px; font-weight:600; }
    .grammar-choice:hover:not(:disabled) { border-color:#aaa1ef; background:#fbfaff; }
    .grammar-choice.wrong { border-color:var(--red); background:#fff3f5; }
    .grammar-choice.correct { border-color:var(--green); background:#edf9f2; }
    .grammar-choice:disabled { cursor:default; }
    .grammar-type-hint { font-size:12px; color:var(--muted); margin:8px 0 0; }
    .grammar-answer-wrap label { display:block; font-size:13px; font-weight:750; margin-bottom:10px; }
    .grammar-feedback { margin-top:16px; padding:15px 17px; border-radius:13px; background:#edf9f2; color:#166b50; font-size:14px; line-height:1.65; }
    .grammar-feedback.revealed { background:#f1f3ff; color:#38317f; }
    .grammar-feedback .grammar-complete { font-size:19px; font-weight:750; margin:6px 0; line-height:1.6; overflow-wrap:anywhere; }
    .grammar-feedback .grammar-why { margin-top:7px; }
    .grammar-hint-btn { border:0; background:none; color:#6153c5; padding:4px 0; font-weight:700; text-decoration:underline; text-underline-offset:3px; font-size:13px; }
    .grammar-hint-text { color:#5b6181; background:#f5f4ff; font-size:13px; border-radius:10px; padding:10px 13px; line-height:1.55; margin-top:10px; }
    .grammar-note { padding:0 4px; color:#788299; font-size:12px; line-height:1.6; margin:14px 0 0; }
    .grammar-guide { max-width:635px; margin:20px auto 0; }
    .grammar-guide p { margin:8px 0 0; }
    .grammar-group:focus-visible,.grammar-choice:focus-visible { outline:3px solid #9586ff; outline-offset:2px; }
    @media(max-width:760px) { .grammar-groups { grid-template-columns:repeat(3,minmax(0,1fr)); } }
    @media(max-width:570px) { .grammar-groups { grid-template-columns:repeat(2,minmax(0,1fr)); } .grammar-options { grid-template-columns:repeat(2,minmax(0,1fr)); } .tab { font-size:12px; padding:12px 6px; } .grammar-sentence { font-size:24px; } }
  </style>
</head>
<body>
  <div class="shell">
    <header>
      <div class="brand"><span class="brand-mark">あ</span><span>Nihongo <span style="color:#6557e8">Quiz</span></span></div>
      <div class="menu-wrap" id="menuWrap">
        <button id="menuToggle" class="menu-button" aria-haspopup="true" aria-expanded="false" aria-controls="dropdown">☰ &nbsp;Menu ▾</button>
        <div class="dropdown" id="dropdown" hidden>
          <button id="addOpen">＋ Dodaj słowo</button>
          <button id="dbOpen">▤ Baza słówek</button>
          <button id="saveAsMenu">📁 Zapisz jako… (wybierz folder)</button>
          <button id="exportMenu">↓ Pobierz bazę (JSON)</button>
          <button id="importMenu">↑ Wczytaj bazę (JSON)</button>
          <button id="resetStats">↻ Wyzeruj wynik</button>
        </div>
      </div>
    </header>
    <nav class="tabbar" role="tablist" aria-label="Wybierz rodzaj quizu">
      <button class="tab" id="tabWords" role="tab" aria-selected="true" aria-controls="wordPanel" type="button">あ &nbsp; Słówka</button>
      <button class="tab" id="tabNumbers" role="tab" aria-selected="false" aria-controls="numberPanel" type="button">123 &nbsp; Liczby i liczniki</button>
      <button class="tab" id="tabGrammar" role="tab" aria-selected="false" aria-controls="grammarPanel" type="button">✎ &nbsp; Gramatyka</button>
    </nav>
    <main class="main" id="wordPanel" role="tabpanel" aria-labelledby="tabWords">
      <div class="eyebrow">✦ &nbsp; Codzienna nauka</div>
      <h1>Ćwicz japońskie słówka</h1>
      <p class="subtitle">Wpisz tłumaczenie po japońsku i od razu sprawdź odpowiedź.</p>
      <section class="card" aria-label="Quiz ze słówek">
        <div class="card-top"><span class="tag">🇯🇵 &nbsp; Polski → Japoński</span><span class="progress" id="progress">Słówko 1 z 8</span></div>
        <div class="question-label">Jak jest po japońsku…</div>
        <div class="translation" id="translation">kot</div>
        <p class="hint">Wpisz odpowiedź w hiraganie, katakanie lub kanji.</p>
        <label for="answer">Twoja odpowiedź</label>
        <input id="answer" class="answer" type="text" placeholder="Wpisz słowo po japońsku…" lang="ja" autocomplete="off" spellcheck="false" aria-describedby="feedback" />
        <div id="feedback" class="feedback" hidden aria-live="polite"><strong id="feedbackTitle">✓ Świetnie! Prawidłowa odpowiedź.</strong><div id="revealedJapanese" class="reveal-line" hidden>Zapis japoński: <span class="jp-revealed" id="japaneseText"></span></div><div class="reveal-line">Wymowa: <span class="reading" id="reading"></span></div><div id="revealedPolish" class="reveal-line" hidden>Po polsku: <strong id="polishText"></strong></div></div>
        <div class="foot"><span class="helper" id="helper">Musisz odpowiedzieć poprawnie, aby przejść dalej.</span><div class="foot-actions"><button class="skip" id="skip" type="button">Pomiń słowo</button><button class="next" id="next" type="button" disabled>Następne słowo →</button></div></div>
      </section>
      <div class="stats"><span>✓ Poprawne: <strong id="correctCount">0</strong></span><span>★ Opanowane: <strong id="masteredCount">0</strong></span><span>▤ Słówka w bazie: <strong id="totalCount">8</strong></span></div>
    </main>
    <main class="main" id="numberPanel" role="tabpanel" aria-labelledby="tabNumbers" hidden>
      <div class="eyebrow">✦ &nbsp; Trening liczb</div>
      <h1>Liczby po japońsku</h1>
      <p class="subtitle">Ćwicz liczby do 9999. Wybierz licznik, zakres i rodzaj quizu, a następnie wpisz japońską odpowiedź.</p>
      <div class="number-categories" aria-label="Kategorie liczb">
        <button class="category" id="catBasic" data-num-mode="basic" aria-pressed="true" type="button">
          <span class="category-icon" aria-hidden="true">🔢</span>
          <strong>Liczby podstawowe</strong>
          <small>Liczenie i matematyka · 一、二、三…</small>
        </button>
        <button class="category" id="catAge" data-num-mode="age" aria-pressed="false" type="button">
          <span class="category-icon" aria-hidden="true">🎂</span>
          <strong>Wiek osób (～歳)</strong>
          <small>Podawanie liczby lat · いっさい、はたち…</small>
        </button>
        <button class="category" id="catBan" data-num-mode="ban" aria-pressed="false" type="button">
          <span class="category-icon" aria-hidden="true">🏷️</span>
          <strong>Numery i kolejność (～番)</strong>
          <small>Numer, miejsce w kolejności · いちばん…</small>
        </button>
        <button class="category" data-num-mode="people" aria-pressed="false" type="button">
          <span class="category-icon" aria-hidden="true">👥</span>
          <strong>Liczenie osób (～人)</strong>
          <small>Ile osób? · ひとり、ふたり、さんにん…</small>
        </button>
        <button class="category" data-num-mode="animals" aria-pressed="false" type="button">
          <span class="category-icon" aria-hidden="true">🐈</span>
          <strong>Małe zwierzęta (～匹)</strong>
          <small>Koty, psy, ryby · いっぴき、さんびき…</small>
        </button>
        <button class="category" data-num-mode="objects" aria-pressed="false" type="button">
          <span class="category-icon" aria-hidden="true">🍎</span>
          <strong>Drobne przedmioty (～個)</strong>
          <small>Jabłka, jajka, rzeczy · いっこ、ろっこ…</small>
        </button>
        <button class="category" data-num-mode="vehicles" aria-pressed="false" type="button">
          <span class="category-icon" aria-hidden="true">🚗</span>
          <strong>Pojazdy i maszyny (～台)</strong>
          <small>Auta, rowery, komputery · いちだい…</small>
        </button>
        <button class="category" data-num-mode="general" aria-pressed="false" type="button">
          <span class="category-icon" aria-hidden="true">📦</span>
          <strong>Ogólne liczenie (～つ)</strong>
          <small>Uniwersalne formy 1–10 · ひとつ、ふたつ…</small>
        </button>
      </div>
      <div class="number-drills number-ranges" aria-label="Zakres liczb">
        <span class="drill-intro">🔢 &nbsp; Zakres liczb</span>
        <div class="drill-selector" role="group" aria-label="Wybierz zakres liczb">
          <button class="drill-button" data-number-range="1-100" type="button" aria-pressed="true">1–100</button>
          <button class="drill-button" data-number-range="101-999" type="button" aria-pressed="false">101–999</button>
          <button class="drill-button" data-number-range="1000-9999" type="button" aria-pressed="false">1000–9999</button>
        </div>
        <p class="range-note" id="rangeNote" aria-live="polite" hidden>Przy liczniku ～つ dostępne są wyłącznie formy od 1 do 10.</p>
      </div>
      <div class="number-drills" aria-label="Rodzaj ćwiczenia">
        <span class="drill-intro">✦ &nbsp; Rodzaj quizu</span>
        <div class="drill-selector" role="group" aria-label="Wszystkie liczby lub tylko trudne odczyty">
          <button class="drill-button" id="drillAll" data-drill="all" type="button" aria-pressed="true">Wszystkie 1–100</button>
          <button class="drill-button" id="drillExceptions" data-drill="exceptions" type="button" aria-pressed="false">Wyjątki i trudne odczyty</button>
        </div>
      </div>
      <div class="number-info" id="numberInfo" aria-live="polite">
        <strong id="numberInfoTitle">Liczby podstawowe — liczenie i matematyka</strong>
        <span id="numberInfoText">Zwykłe liczby używane w liczeniu i działaniach. Np. 23 = 二十三 (にじゅうさん).</span>
      </div>
      <section class="card" aria-label="Quiz liczb japońskich">
        <div class="card-top"><span class="tag" id="numTag">🔢 &nbsp; Liczby podstawowe</span><span class="progress" id="numProgress">Liczba 1 z 100</span></div>
        <div class="question-label" id="numQuestion">Zapisz po japońsku liczbę…</div>
        <div class="translation" id="numDisplay">23</div>
        <p class="hint" id="numHint">Wpisz liczbę hiraganą lub znakami kanji.</p>
        <label class="num-answer-label" for="numAnswer">Twoja odpowiedź</label>
        <input id="numAnswer" class="answer" type="text" lang="ja" placeholder="np. にじゅうさん" autocomplete="off" autocapitalize="off" spellcheck="false" aria-describedby="numFeedback" />
        <div id="numFeedback" class="number-feedback" hidden aria-live="polite">
          <strong id="numFeedbackTitle">✓ Poprawnie!</strong>
          <div class="num-japanese" id="numJapanese"></div>
          <div class="num-detail">Wymowa: <b id="numRomaji"></b></div>
          <div class="num-detail">Po polsku: <span id="numPolish"></span></div>
          <div class="exception-rule" id="numRule" hidden></div>
        </div>
        <div class="foot">
          <span class="helper" id="numHelper">Poprawna odpowiedź odblokuje następną liczbę.</span>
          <div class="foot-actions">
            <button class="skip" id="numSkip" type="button">Pomiń liczbę</button>
            <button class="next" id="numNext" type="button" disabled>Następna liczba →</button>
          </div>
        </div>
      </section>
      <div class="stats">
        <span>✓ Poprawne: <strong id="numCorrectCount">0</strong></span>
        <span>↪ Pominięte: <strong id="numSkippedCount">0</strong></span>
        <span>◎ Przerobione: <strong id="numDoneCount">0</strong>/<strong id="numTotalCount">100</strong></span>
      </div>
      <details class="number-guide">
        <summary>Jak działają kategorie i liczniki? Krótka ściąga</summary>
        <p><b>1. Liczby podstawowe:</b> liczby używane w liczeniu oraz działaniach matematycznych, np. 12 → 十二（じゅうに）.</p>
        <p><b>2. Wiek (～歳 / さい):</b> dodajemy licznik wieku, np. 1 rok → 一歳（いっさい）, 8 lat → 八歳（はっさい）. Wiek 20 lat wyjątkowo czytamy 二十歳（はたち）.</p>
        <p><b>3. Numery i kolejność (～番 / ばん):</b> oznaczanie numeru lub pozycji w szeregu, np. numer 3 → 三番（さんばん）. To nie to samo, co ogólne liczenie przedmiotów.</p>
        <p><b>4. Osoby (～人 / にん):</b> 1 osoba → 一人（ひとり）, 2 osoby → 二人（ふたり）, 4 osoby → 四人（よにん）. Od 11 osób stosuje się zwykłe liczby, np. 11 osób → 十一人（じゅういちにん）.</p>
        <p><b>5. Małe zwierzęta (～匹 / ひき):</b> koty, psy, ryby i owady. 1 → 一匹（いっぴき）, 3 → 三匹（さんびき）, 6 → 六匹（ろっぴき）. Większe zwierzęta (np. konie) liczy się zwykle przez ～頭, ptaki przez ～羽.</p>
        <p><b>6. Drobne przedmioty (～個 / こ):</b> np. jabłka, jajka, małe rzeczy. 1 → 一個（いっこ）, 6 → 六個（ろっこ）, 8 → 八個（はっこ）.</p>
        <p><b>7. Pojazdy i maszyny (～台 / だい):</b> samochody, rowery, komputery czy pralki. 2 → 二台（にだい）, 8 → 八台（はちだい）. Końcówka ～台 jest regularna.</p>
        <p><b>8. Ogólne liczenie (～つ):</b> uniwersalne formy, np. 1 → 一つ（ひとつ）, 3 → 三つ（みっつ）, 10 → 十（とお）. Ten system <b>obejmuje tylko 1–10</b>. Przy większych liczbach wybiera się odpowiedni licznik, np. ～個.</p>
        <p><b>Zakres liczb:</b> możesz ćwiczyć 1–100, 101–999 lub 1000–9999 we wszystkich kategoriach poza ～つ (tylko 1–10). W wyższych zakresach pamiętaj o setkach 300 → さんびゃく, 600 → ろっぴゃく, 800 → はっぴゃく oraz tysiącach 3000 → さんぜん, 8000 → はっせん. Przy ～個 np. 300個 → さんびゃっこ, a przy ～匹 1000匹 → せんびき.</p>
        <p><b>Tryb „Wyjątki i trudne odczyty”:</b> w liczbach podstawowych skupia się na 4, 7 i 9 (oraz liczbach z takimi cyframi na końcu). W wieku ćwiczysz 1, 8 i 10, ich odpowiedniki w większych liczbach oraz szczególną formę 20 lat (はたち). Przy ～番 i ～台 nie ma osobnej nieregularnej odmiany, więc tryb skupia się na trudniejszych odczytach 4, 7 i 9. W ～人 ćwiczysz m.in. ひとり i ふたり, a w ～匹 i ～個 zmiany głosek; w ～つ cały zestaw 1–10.</p>
        <p>W zwykłym quizie możesz wpisywać hiraganą albo kanji. W quizie wyjątków wpisz <b>hiraganą</b>, ponieważ ćwiczymy konkretną wymowę. Błędna odpowiedź zatrzymuje pytanie, a „Pomiń” odsłania rozwiązanie. Pytania nie powtarzają się w jednej rundzie.</p>
      </details>
    </main>
  </div>

    <main class="main" id="grammarPanel" role="tabpanel" aria-labelledby="tabGrammar" hidden>
      <div class="eyebrow">✦ &nbsp; Pierwsze japońskie zdania</div>
      <h1>Ćwicz cząsteczki</h1>
      <p class="subtitle">Uzupełniaj krótkie zdania i ucz się, kiedy używać は, か, の, なに i なん — bez konieczności znajomości kanji.</p>
      <div class="grammar-lesson"><span aria-hidden="true">💡</span><div><strong>Jak się uczyć?</strong> Wybierz jedną lub kilka kategorii, np. は + の + か, albo zaznacz „Wszystkie”. Następnie znajdź brakującą cząsteczkę. Pod każdym zdaniem zobaczysz zapis <b>romaji</b> oraz polskie tłumaczenie. Czerwona odpowiedź oznacza pomyłkę — próbuj dalej albo użyj „Pomiń”.</div></div>
      <div class="grammar-groups" id="grammarGroups" role="group" aria-label="Wybierz jedną lub kilka kategorii gramatyki"></div>
      <p class="grammar-selection-summary" id="grammarSelectionSummary" role="status" aria-live="polite">Wybrane kategorie: Wszystkie.</p>
      <div class="grammar-main">
        <div class="grammar-top-row">
          <span class="tag" id="grammarTopicTag">✦ Wszystkie cząsteczki</span>
          <div class="grammar-style-options" role="group" aria-label="Sposób udzielania odpowiedzi">
            <button id="grammarPickMode" type="button" aria-pressed="true">Wybierz</button>
            <button id="grammarTypeMode" type="button" aria-pressed="false">Wpisz samodzielnie</button>
          </div>
        </div>
        <section class="card" aria-label="Quiz japońskich cząsteczek">
          <div class="card-top"><span class="tag">🇯🇵 &nbsp; Uzupełnij lukę □</span><span class="progress" id="grammarProgress">Pytanie 1</span></div>
          <div class="question-label">Uzupełnij zdanie odpowiednim znakiem</div>
          <div class="grammar-sentence" id="grammarJapanese" lang="ja">これは □ ですか。</div>
          <div class="grammar-romaji" id="grammarRomaji">Kore wa ___ desu ka.</div>
          <div class="grammar-polish"><b>Po polsku:</b> <span id="grammarPolish">Co to jest?</span></div>
          <div class="grammar-options" id="grammarOptions" role="group" aria-label="Możliwe odpowiedzi"></div>
          <div class="grammar-answer-wrap" id="grammarTypeWrap" hidden>
            <label for="grammarAnswer">Wpisz cząsteczkę hiraganą lub romaji</label>
            <input class="answer" id="grammarAnswer" type="text" autocomplete="off" spellcheck="false" autocapitalize="off" placeholder="np. wa, nan, は…" lang="ja" />
            <p class="grammar-type-hint">Możesz pisać zwykłymi literami, np. wa → は, nan → なん. Dla cząsteczki を wpisz o, a dla へ — e.</p>
          </div>
          <div style="margin-top:14px"><button class="grammar-hint-btn" id="grammarHintBtn" type="button" aria-expanded="false">💡 Pokaż podpowiedź</button><div class="grammar-hint-text" id="grammarHint" hidden></div></div>
          <div class="grammar-feedback" id="grammarFeedback" hidden aria-live="polite">
            <strong id="grammarFeedbackTitle">✓ Dobrze!</strong>
            <div class="grammar-complete" id="grammarCompletedJapanese" lang="ja"></div>
            <div id="grammarCompletedRomaji"></div>
            <div class="grammar-why" id="grammarExplanation"></div>
          </div>
          <div class="foot"><span class="helper" id="grammarHelper">Wybierz odpowiedź. Nie musisz znać kanji.</span><div class="foot-actions"><button class="skip" id="grammarSkip" type="button">Pomiń i pokaż</button><button class="next" id="grammarNext" type="button" disabled>Następne zdanie →</button></div></div>
        </section>
        <div class="stats"><span>✓ Poprawne: <strong id="grammarCorrect">0</strong></span><span>↪ Pominięte: <strong id="grammarSkipped">0</strong></span><span>◎ Przerobione: <strong id="grammarDone">0</strong>/<strong id="grammarTotal">0</strong></span></div>
        <p class="grammar-note">は czytamy tutaj „wa”, を — „o”, a へ — „e”. Nie są to literówki! Zdania podają typowe zastosowania; w niektórych kontekstach japoński dopuszcza też inne konstrukcje.</p>
        <details class="number-guide grammar-guide">
          <summary>📘 Krótka ściąga — jak działają te cząsteczki?</summary>
          <p><b>は (wa):</b> mówimy, o kim albo o czym jest zdanie. „Watashi wa…” = „Jeśli chodzi o mnie…”.</p>
          <p><b>か (ka):</b> na końcu zdania grzecznościowego robi z niego pytanie. „Desu ka?” = „Czy…?”.</p>
          <p><b>の (no):</b> łączy rzecz z właścicielem lub określeniem. „Watashi no hon” = „moja książka”.</p>
          <p><b>なに (nani) / なん (nan):</b> oba znaczą „co”, ale mają różną wymowę w różnych połączeniach. Np. „nani o tabemasu ka” = „co jesz?”, a „nan-nin” = „ile osób?”.</p>
          <p><b>を (o):</b> wskazuje rzecz, z którą wykonujesz czynność. „Mizu o nomimasu” = „piję wodę”.</p>
          <p><b>が (ga):</b> często wskazuje, co istnieje, kto coś zrobił, albo co lubisz. „Neko ga imasu” = „jest kot”.</p>
          <p><b>に (ni):</b> może wskazywać konkretny czas, miejsce, w którym coś się znajduje, lub odbiorcę.</p>
          <p><b>で (de):</b> miejsce wykonywania czynności lub sposób/narzędzie. „Basu de” = „autobusem”.</p>
          <p><b>も (mo):</b> znaczy „też”, „również”.</p>
          <p><b>と (to):</b> znaczy „z kimś” albo łączy rzeczy w znaczeniu „i”.</p>
          <p><b>へ (e):</b> wskazuje kierunek ruchu. Często w zdaniach o podróży można też użyć に.</p>
          <p><b>Wskazówka:</b> nie musisz uczyć się wszystkich na raz. Zacznij od は, か, の i なに / なん, a potem dodawaj kolejne tematy.</p>
        </details>
      </div>
    </main>
  <div id="overlay" class="overlay" hidden>
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
      <div class="modal-header"><h2 id="modalTitle">Dodaj nowe słowo</h2><button class="close" id="addClose" aria-label="Zamknij">×</button></div>
      <p>Dodane słowa zapisują się automatycznie w tej przeglądarce i pojawią się w quizie.</p>
      <form id="addForm">
        <div class="field"><label for="jp">Słowo po japońsku</label><input id="jp" required lang="ja" placeholder="np. いぬ" autocomplete="off" /></div>
        <div class="field"><label for="romaji">Wymowa (romaji)</label><input id="romaji" required placeholder="np. inu" autocomplete="off" /></div>
        <div class="field"><label for="pl">Znaczenie po polsku</label><input id="pl" required placeholder="np. pies" autocomplete="off" /></div>
        <button class="save" type="submit">＋ Zapisz słowo</button>
        <div class="notice" id="notice" role="status"></div>
      </form>
    </div>
  </div>
  <div id="dbOverlay" class="overlay" hidden>
    <div class="modal database-modal" role="dialog" aria-modal="true" aria-labelledby="dbTitle">
      <div class="modal-header"><h2 id="dbTitle">Baza słówek</h2><button class="close" id="dbClose" aria-label="Zamknij">×</button></div>
      <p>Przeglądaj, wyszukuj, edytuj lub usuwaj słowa. Zmiany są zapisywane na tym urządzeniu.</p>
      <div class="db-tools"><input type="search" id="dbSearch" placeholder="Szukaj po polsku, japońsku lub romaji…" aria-label="Szukaj słów"></div>
      <div class="backup-actions">
        <button id="saveAsDb" class="save-file" type="button">📁 Zapisz jako… (wybierz folder)</button>
        <button id="exportDb" type="button">↓ Pobierz standardowo</button>
        <button id="importDb" type="button">↑ Wczytaj bazę z pliku</button>
      </div>
      <div class="backup-help">„Zapisz jako…” umożliwia wybranie folderu i nazwy pliku w obsługiwanych przeglądarkach (np. Chrome lub Edge). Jeśli ta funkcja jest niedostępna, użyj zwykłego pobierania — jego folder ustala przeglądarka.</div>
      <div class="backup-status" id="backupStatus" role="status" aria-live="polite"></div>
      <input id="importFile" type="file" accept=".json,application/json" hidden aria-label="Wybierz plik bazy słówek JSON" />
      <div id="dbList" class="db-list"></div>
    </div>
  </div>
  <script>
    (() => {
      'use strict';
      const defaults = [
        {jp:'ねこ', romaji:'neko', pl:'kot'},
        {jp:'いぬ', romaji:'inu', pl:'pies'},
        {jp:'みず', romaji:'mizu', pl:'woda'},
        {jp:'やま', romaji:'yama', pl:'góra'},
        {jp:'ほん', romaji:'hon', pl:'książka'},
        {jp:'さくら', romaji:'sakura', pl:'wiśnia (kwiat)'},
        {jp:'ありがとう', romaji:'arigatō', pl:'dziękuję'},
        {jp:'おはよう', romaji:'ohayō', pl:'dzień dobry (rano)'}
      ];
      const $ = id => document.getElementById(id);
      const storageKey = 'nihongo-quiz-all-words-v2';
      const oldKey = 'nihongo-quiz-words-v1';
      const valid = x => x && ['jp','romaji','pl'].every(k => typeof x[k] === 'string' && x[k].trim());
      const normalize = value => value.normalize('NFKC').replace(/\s+/g,'').trim();
      const accepted = jp => jp.split('|').map(normalize);
      let words = defaults.map(x => ({...x}));
      try {
        const saved = localStorage.getItem(storageKey);
        if (saved !== null) {
          const parsed = JSON.parse(saved);
          if (Array.isArray(parsed)) words = parsed.filter(valid).map(x => ({jp:x.jp,romaji:x.romaji,pl:x.pl}));
        } else {
          const old = JSON.parse(localStorage.getItem(oldKey) || '[]');
          if (Array.isArray(old)) words.push(...old.filter(valid));
        }
      } catch (_) { /* Storage may be disabled */ }
      const persist = () => { try { localStorage.setItem(storageKey, JSON.stringify(words)); } catch (_) {} };
      let remaining = [];
      let current = -1;
      let completed = false;
      let skipped = false;
      let correctCount = 0;
      const mastered = new Set();
      let editingIndex = -1;
      let isComposing = false;
      function updateStats() {
        $('correctCount').textContent = correctCount;
        $('masteredCount').textContent = mastered.size;
        $('totalCount').textContent = words.length;
      }
      function nextWord() {
        if (!words.length) {
          current = -1;
          $('translation').textContent = 'Brak słówek';
          $('progress').textContent = '0 słówek';
          $('answer').value = '';
          $('answer').disabled = true;
          $('next').disabled = true;
          $('skip').disabled = true;
          $('feedback').hidden = true;
          $('helper').textContent = 'Dodaj słowo w menu, aby rozpocząć quiz.';
          return;
        }
        if (!remaining.length) remaining = words.map((_,i) => i);
        const pick = Math.floor(Math.random() * remaining.length);
        current = remaining.splice(pick,1)[0];
        completed = false;
        skipped = false;
        $('translation').textContent = words[current].pl;
        $('progress').textContent = `Słówko ${words.length - remaining.length} z ${words.length}`;
        $('answer').disabled = false;
        $('answer').value = '';
        $('answer').classList.remove('correct','wrong');
        $('answer').removeAttribute('aria-invalid');
        $('feedback').hidden = true;
        $('feedback').classList.remove('revealed');
        $('feedbackTitle').textContent = '✓ Świetnie! Prawidłowa odpowiedź.';
        $('revealedJapanese').hidden = true;
        $('revealedPolish').hidden = true;
        $('reading').textContent = '';
        $('skip').disabled = false;
        $('next').disabled = true;
        $('helper').textContent = 'Wpisz poprawną odpowiedź, aby odblokować kolejne słowo.';
        $('answer').focus();
      }
      function checkAnswer() {
        if (current < 0 || skipped) return;
        const value = normalize($('answer').value);
        const isCorrect = !!value && accepted(words[current].jp).includes(value);
        $('answer').classList.toggle('correct', isCorrect);
        $('answer').classList.toggle('wrong', !!value && !isCorrect);
        if (value) $('answer').setAttribute('aria-invalid', String(!isCorrect));
        else $('answer').removeAttribute('aria-invalid');
        $('feedback').hidden = !isCorrect;
        $('reading').textContent = isCorrect ? words[current].romaji : '';
        // A correctly answered question stays unlocked even if the user changes the input.
        if (isCorrect && !completed) {
          completed = true;
          correctCount++;
          mastered.add(current);
          updateStats();
        }
        $('next').disabled = !completed;
        if (completed) $('skip').disabled = true;
        $('helper').textContent = completed
          ? 'Dobrze! Możesz przejść do następnego słowa.'
          : 'Błędna odpowiedź? Popraw ją, aby przejść dalej.';
      }
      function skipWord() {
        if (current < 0 || completed || skipped) return;
        skipped = true;
        $('answer').disabled = true;
        $('answer').classList.remove('wrong', 'correct');
        $('answer').removeAttribute('aria-invalid');
        $('feedback').hidden = false;
        $('feedback').classList.add('revealed');
        $('feedbackTitle').textContent = 'Pominięto słowo — zapamiętaj odpowiedź:';
        $('revealedJapanese').hidden = false;
        $('revealedPolish').hidden = false;
        $('japaneseText').textContent = words[current].jp.split('|').join(' / ');
        $('reading').textContent = words[current].romaji;
        $('polishText').textContent = words[current].pl;
        $('skip').disabled = true;
        $('next').disabled = false;
        $('helper').textContent = 'Możesz teraz przejść do następnego słowa.';
      }
      function resetQuestionAfterChange() {
        remaining = [];
        mastered.clear();
        correctCount = 0;
        updateStats();
        nextWord();
      }
      $('answer').addEventListener('compositionstart', () => isComposing = true);
      $('answer').addEventListener('compositionend', () => { isComposing = false; checkAnswer(); });
      $('answer').addEventListener('input', () => { if (!isComposing) checkAnswer(); });
      $('answer').addEventListener('keydown', e => {
        if (e.key === 'Enter' && !e.isComposing && !isComposing) {
          e.preventDefault();
          if (completed || skipped) nextWord();
        }
      });
      $('next').addEventListener('click', () => { if (completed || skipped) nextWord(); });
      $('skip').addEventListener('click', skipWord);
      function closeMenu() { $('dropdown').hidden = true; $('menuToggle').setAttribute('aria-expanded','false'); }
      $('menuToggle').addEventListener('click', () => {
        $('dropdown').hidden = !$('dropdown').hidden;
        $('menuToggle').setAttribute('aria-expanded', String(!$('dropdown').hidden));
      });
      document.addEventListener('click', e => { if (!$('menuWrap').contains(e.target)) closeMenu(); });
      function openModal(index = -1) {
        closeMenu();
        editingIndex = index;
        $('addForm').reset();
        $('notice').textContent = '';
        $('modalTitle').textContent = index < 0 ? 'Dodaj nowe słowo' : 'Edytuj słowo';
        $('addForm').querySelector('.save').textContent = index < 0 ? '＋ Zapisz słowo' : 'Zapisz zmiany';
        if (index >= 0) {
          $('jp').value = words[index].jp;
          $('romaji').value = words[index].romaji;
          $('pl').value = words[index].pl;
        }
        $('dbOverlay').hidden = true;
        $('overlay').hidden = false;
        $('jp').focus();
      }
      function closeModal() { $('overlay').hidden = true; $('addForm').reset(); $('notice').textContent = ''; $('menuToggle').focus(); }
      function renderDatabase() {
        const list = $('dbList');
        list.replaceChildren();
        const query = $('dbSearch').value.trim().toLocaleLowerCase('pl');
        const matches = words.map((word,index) => ({word,index})).filter(({word}) =>
          [word.jp,word.romaji,word.pl].some(value => value.toLocaleLowerCase('pl').includes(query)));
        if (!matches.length) {
          const empty = document.createElement('div');
          empty.className = 'empty';
          empty.textContent = words.length ? 'Nie znaleziono takich słówek.' : 'Baza jest pusta. Dodaj pierwsze słówko w menu.';
          list.append(empty);
          return;
        }
        matches.forEach(({word,index}) => {
          const item = document.createElement('div'); item.className = 'db-item';
          const info = document.createElement('div');
          const jp = document.createElement('div'); jp.className = 'db-jp'; jp.textContent = word.jp;
          const detail = document.createElement('div'); detail.className = 'db-detail';
          detail.textContent = `${word.romaji} · ${word.pl}`;
          info.append(jp,detail);
          const actions = document.createElement('div'); actions.className = 'db-actions';
          const edit = document.createElement('button'); edit.type='button'; edit.textContent='Edytuj';
          edit.setAttribute('aria-label',`Edytuj: ${word.pl}`);
          edit.addEventListener('click',() => openModal(index));
          const del = document.createElement('button'); del.type='button'; del.className='delete'; del.textContent='Usuń';
          del.setAttribute('aria-label',`Usuń: ${word.pl}`);
          del.addEventListener('click',() => {
            if (!confirm(`Usunąć słowo „${word.pl}” (${word.jp})?`)) return;
            words.splice(index,1);
            persist();
            resetQuestionAfterChange();
            renderDatabase();
          });
          actions.append(edit,del); item.append(info,actions); list.append(item);
        });
      }
      // Ten sam format pliku jest używany w obu sposobach eksportu.
      function makeBackup() {
        const payload = {format:'nihongo-quiz', version:1, exportedAt:new Date().toISOString(), words};
        return {
          filename: 'nihongo-slowa-' + new Date().toISOString().slice(0,10) + '.json',
          blob: new Blob([JSON.stringify(payload,null,2)], {type:'application/json;charset=utf-8'})
        };
      }
      function exportWords() {
        closeMenu();
        const {filename, blob} = makeBackup();
        const url = URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.href = url;
        link.download = filename;
        document.body.append(link);
        link.click();
        link.remove();
        setTimeout(() => URL.revokeObjectURL(url), 1000);
        $('backupStatus').textContent = `Rozpoczęto pobieranie kopii bazy (${words.length} słówek).`;
      }
      async function saveWordsInFolder() {
        closeMenu();
        if (typeof window.showSaveFilePicker !== 'function') {
          const message = 'Ta przeglądarka nie udostępnia wyboru folderu z poziomu strony. Użyj „Pobierz bazę (JSON)” albo otwórz quiz w aktualnym Chrome lub Edge. Możesz też w ustawieniach pobierania przeglądarki włączyć pytanie o miejsce zapisu każdego pliku.';
          $('backupStatus').textContent = message;
          alert(message);
          return;
        }
        const {filename, blob} = makeBackup();
        try {
          // Otwieramy okno zapisu bezpośrednio po kliknięciu użytkownika.
          const handle = await window.showSaveFilePicker({
            suggestedName: filename,
            types: [{description:'Kopia słówek japońskich (JSON)', accept:{'application/json':['.json']}}]
          });
          const writable = await handle.createWritable();
          try {
            await writable.write(blob);
          } finally {
            await writable.close();
          }
          $('backupStatus').textContent = `Zapisano ${words.length} słówek w pliku „${handle.name || filename}”.`;
        } catch (error) {
          if (error && error.name === 'AbortError') {
            $('backupStatus').textContent = 'Anulowano wybór miejsca zapisu. Baza słówek pozostała bez zmian.';
            return;
          }
          const message = `Nie udało się zapisać pliku: ${error && error.message ? error.message : 'nieznany błąd'}. Możesz użyć zwykłego pobierania.`;
          $('backupStatus').textContent = message;
          alert(message);
        }
      }
      function requestImport() { closeMenu(); $('importFile').click(); }
      $('importFile').addEventListener('change', async e => {
        const file = e.target.files[0];
        e.target.value = '';
        if (!file) return;
        try {
          if (file.size > 5 * 1024 * 1024) throw Error('Plik jest za duży (maksymalnie 5 MB).');
          const parsed = JSON.parse((await file.text()).replace(/^\uFEFF/,''));
          const data = Array.isArray(parsed) ? parsed : (parsed && parsed.words);
          if (!Array.isArray(data) || data.length > 20000 || !data.every(valid)) {
            throw Error('Nieprawidłowy format pliku. Wymagana jest lista słówek z polami jp, romaji i pl.');
          }
          const imported = data.map(x => ({jp:x.jp.trim(),romaji:x.romaji.trim(),pl:x.pl.trim()}));
          if (!confirm(`Wczytać ${imported.length} słówek z pliku „${file.name}”?\n\nZastąpi to całą obecną bazę (${words.length} słówek). Jeśli chcesz zachować obecną bazę, najpierw ją pobierz.`)) return;
          words = imported;
          persist();
          resetQuestionAfterChange();
          if (!$('dbOverlay').hidden) renderDatabase();
          $('backupStatus').textContent = `Wczytano ${words.length} słówek z pliku „${file.name}”.`;
          alert(`Wczytano ${words.length} słówek. Baza została zapisana w przeglądarce.`);
        } catch (error) {
          $('backupStatus').textContent = error.message;
          alert('Nie udało się wczytać bazy: ' + error.message);
        }
      });
      $('saveAsMenu').addEventListener('click', saveWordsInFolder);
      $('exportMenu').addEventListener('click', exportWords);
      $('importMenu').addEventListener('click', requestImport);
      $('saveAsDb').addEventListener('click', saveWordsInFolder);
      $('exportDb').addEventListener('click', exportWords);
      $('importDb').addEventListener('click', requestImport);
      function openDatabase() { closeMenu(); $('dbSearch').value=''; renderDatabase(); $('dbOverlay').hidden = false; $('dbSearch').focus(); }
      function closeDatabase() { $('dbOverlay').hidden = true; $('menuToggle').focus(); }
      $('addOpen').addEventListener('click',() => openModal());
      $('dbOpen').addEventListener('click',openDatabase);
      $('dbClose').addEventListener('click',closeDatabase);
      $('dbSearch').addEventListener('input',renderDatabase);
      $('addClose').addEventListener('click',closeModal);
      $('overlay').addEventListener('click', e => { if (e.target === $('overlay')) closeModal(); });
      $('dbOverlay').addEventListener('click', e => { if (e.target === $('dbOverlay')) closeDatabase(); });
      document.addEventListener('keydown', e => {
        if (e.key === 'Escape') {
          if (!$('overlay').hidden) closeModal();
          else if (!$('dbOverlay').hidden) closeDatabase();
          else closeMenu();
        }
      });
      $('addForm').addEventListener('submit',e => {
        e.preventDefault();
        const entry = {jp:$('jp').value.trim(),romaji:$('romaji').value.trim(),pl:$('pl').value.trim()};
        if (!valid(entry)) return;
        const duplicated = words.some((word,index) => index !== editingIndex &&
          normalize(word.jp) === normalize(entry.jp) && word.pl.toLocaleLowerCase('pl') === entry.pl.toLocaleLowerCase('pl'));
        if (duplicated) {
          $('notice').style.color = '#e24d66';
          $('notice').textContent = 'Takie słowo jest już w bazie.';
          return;
        }
        if (editingIndex < 0) words.push(entry);
        else words[editingIndex] = entry;
        persist();
        resetQuestionAfterChange();
        $('notice').style.color = '#159a6a';
        $('notice').textContent = editingIndex < 0 ? 'Dodano słowo do bazy!' : 'Zapisano zmiany!';
        if (editingIndex < 0) { $('addForm').reset(); $('jp').focus(); }
      });
      $('resetStats').addEventListener('click',() => { closeMenu(); resetQuestionAfterChange(); });
      updateStats();
      nextWord();
    })();
  </script>

  <script>
    (() => {
      'use strict';
      const $ = id => document.getElementById(id);
      const tabs = [
        {name:'words',tab:$('tabWords'),panel:$('wordPanel'),focus:$('answer')},
        {name:'numbers',tab:$('tabNumbers'),panel:$('numberPanel'),focus:$('numAnswer')},
        {name:'grammar',tab:$('tabGrammar'),panel:$('grammarPanel'),focus:$('grammarHintBtn')}
      ];
      const showTab = which => {
        tabs.forEach(({name,tab,panel})=>{
          panel.hidden = name !== which;
          tab.setAttribute('aria-selected',String(name === which));
          tab.tabIndex = name === which ? 0 : -1;
        });
        const picked=tabs.find(item=>item.name===which);
        if(picked) picked.focus.focus();
      };
      tabs.forEach(({name,tab},index)=>{
        tab.addEventListener('click',()=>showTab(name));
        tab.addEventListener('keydown',event=>{
          if(event.key!=='ArrowLeft' && event.key!=='ArrowRight') return;
          event.preventDefault();
          const next=(index+(event.key==='ArrowRight'?1:-1)+tabs.length)%tabs.length;
          showTab(tabs[next].name);
          tabs[next].tab.focus();
        });
      });

      const digits = [
        {kana:'', kanji:'', romaji:''},
        {kana:'いち',kanji:'一',romaji:'ichi'},
        {kana:'に',kanji:'二',romaji:'ni'},
        {kana:'さん',kanji:'三',romaji:'san'},
        {kana:'よん',kanji:'四',romaji:'yon'},
        {kana:'ご',kanji:'五',romaji:'go'},
        {kana:'ろく',kanji:'六',romaji:'roku'},
        {kana:'なな',kanji:'七',romaji:'nana'},
        {kana:'はち',kanji:'八',romaji:'hachi'},
        {kana:'きゅう',kanji:'九',romaji:'kyū'}
      ];
      // Odczyty setek i tysięcy, włącznie z nieregularnymi 300, 600, 800, 3000 i 8000.
      // Wszystkie liczby quizu mieszczą się w przedziale 1–9999.
      const hundreds = [null,
        ['ひゃく','百','hyaku'], ['にひゃく','二百','nihyaku'],
        ['さんびゃく','三百','sanbyaku'], ['よんひゃく','四百','yonhyaku'],
        ['ごひゃく','五百','gohyaku'], ['ろっぴゃく','六百','roppyaku'],
        ['ななひゃく','七百','nanahyaku'], ['はっぴゃく','八百','happyaku'],
        ['きゅうひゃく','九百','kyūhyaku']
      ];
      const thousands = [null,
        ['せん','千','sen'], ['にせん','二千','nisen'],
        ['さんぜん','三千','sanzen'], ['よんせん','四千','yonsen'],
        ['ごせん','五千','gosen'], ['ろくせん','六千','rokusen'],
        ['ななせん','七千','nanasen'], ['はっせん','八千','hassen'],
        ['きゅうせん','九千','kyūsen']
      ];
      function numeric(n) {
        if (!Number.isInteger(n) || n < 1 || n > 9999) throw new RangeError('Liczba poza zakresem 1–9999');
        if (n < 10) return {...digits[n]};
        const groups = [];
        const th = Math.floor(n / 1000);
        const h = Math.floor(n % 1000 / 100);
        const t = Math.floor(n % 100 / 10);
        const u = n % 10;
        if (th) groups.push(thousands[th]);
        if (h) groups.push(hundreds[h]);
        if (t) groups.push([
          (t === 1 ? '' : digits[t].kana) + 'じゅう',
          (t === 1 ? '' : digits[t].kanji) + '十',
          (t === 1 ? '' : digits[t].romaji) + 'jū'
        ]);
        if (u) groups.push([digits[u].kana,digits[u].kanji,digits[u].romaji]);
        return {
          kana:groups.map(g=>g[0]).join(''),
          kanji:groups.map(g=>g[1]).join(''),
          romaji:groups.map(g=>g[2]).join('')
        };
      }
      function age(n) {
        const base = numeric(n);
        if (n === 20) {
          return {kana:'はたち',kanji:'二十歳',romaji:'hatachi',alternatives:['二十才']};
        }
        const unit = n % 10;
        let kana, romaji, alternatives=[];
        if (unit === 0 && n % 100 !== 0) {
          const prefix = {kana:base.kana.slice(0,-3),romaji:base.romaji.replace(/jū$/,'')};
          kana=prefix.kana+'じゅっさい';
          romaji=prefix.romaji+'jussai';
          alternatives=[prefix.kana+'じっさい'];
        } else if (unit === 1) {
          const prefix = n === 1 ? {kana:'',romaji:''} : numeric(n-1);
          kana=prefix.kana+'いっさい';
          romaji=prefix.romaji+'issai';
        } else if (unit === 8) {
          const prefix = n === 8 ? {kana:'',romaji:''} : numeric(n-8);
          kana=prefix.kana+'はっさい';
          romaji=prefix.romaji+'hassai';
        } else {
          kana=base.kana+'さい';
          romaji=base.romaji+'sai';
        }
        return {kana,kanji:base.kanji+'歳',romaji,alternatives};
      }
      // Liczniki: zapis, odczyt hiraganą, romaji i dopuszczalne warianty.
      // Przy większych liczbach odmienia się ostatni człon (np. 21匹 → にじゅういっぴき).
      const generalForms = [null,
        ['ひとつ','一つ','hitotsu'],['ふたつ','二つ','futatsu'],['みっつ','三つ','mittsu'],
        ['よっつ','四つ','yottsu'],['いつつ','五つ','itsutsu'],['むっつ','六つ','muttsu'],
        ['ななつ','七つ','nanatsu'],['やっつ','八つ','yattsu'],
        ['ここのつ','九つ','kokonotsu'],['とお','十','tō']
      ];
      function people(n) {
        const base = numeric(n);
        const endings = {
          1:['ひとり','hitori'], 2:['ふたり','futari'], 4:['よにん','yonin']
        };
        if (n <= 4 && endings[n] && n !== 3) return {kana:endings[n][0],kanji:base.kanji+'人',romaji:endings[n][1],alternatives:[]};
        const unit = n % 10;
        const prefix = n >= 10 && unit ? numeric(n-unit) : {kana:'',romaji:''};
        if (unit === 4) return {kana:prefix.kana+'よにん',kanji:base.kanji+'人',romaji:prefix.romaji+'yonin',alternatives:[]};
        const alts = [];
        if (unit === 7) alts.push(prefix.kana+'しちにん');
        if (unit === 9) alts.push(prefix.kana+'くにん');
        return {kana:base.kana+'にん',kanji:base.kanji+'人',romaji:base.romaji+'nin',alternatives:alts};
      }
      function counterWithChanges(n,kind) {
        const base = numeric(n);
        const animal = kind === 'animals';
        const suffix = animal ? '匹' : '個';
        const forms = animal ? [null,
          ['いっぴき','ippiki'], ['にひき','nihiki'], ['さんびき','sanbiki'],
          ['よんひき','yonhiki'], ['ごひき','gohiki'], ['ろっぴき','roppiki'],
          ['ななひき','nanahiki'], ['はっぴき','happiki'], ['きゅうひき','kyūhiki']
        ] : [null,
          ['いっこ','ikko'], ['にこ','niko'], ['さんこ','sanko'],
          ['よんこ','yonko'], ['ごこ','goko'], ['ろっこ','rokko'],
          ['ななこ','nanako'], ['はっこ','hakko'], ['きゅうこ','kyūko']
        ];
        const alternatives = [];
        // W równych tysiącach: 千匹 = せんびき, 千個 = せんこ.
        if (n % 1000 === 0) {
          return {kana:base.kana+(animal?'びき':'こ'),kanji:base.kanji+suffix,
            romaji:base.romaji+(animal?'biki':'ko'),alternatives};
        }
        // Pełne setki: 百匹 = ひゃっぴき, 三百個 = さんびゃっこ.
        if (n % 100 === 0) {
          return {kana:base.kana.slice(0,-1)+(animal?'っぴき':'っこ'),kanji:base.kanji+suffix,
            romaji:base.romaji.replace(/ku$/,'')+(animal?'ppiki':'kko'),alternatives};
        }
        // Pełne dziesiątki (10, 20, ... 990, 9990): ～じゅっぴき / ～じゅっこ.
        if (n % 10 === 0) {
          const prefix={kana:base.kana.slice(0,-3),romaji:base.romaji.replace(/jū$/,'')};
          const endKana=animal?'じゅっぴき':'じゅっこ';
          const endRomaji=animal?'juppiki':'jukko';
          alternatives.push(prefix.kana+(animal?'じっぴき':'じっこ'));
          return {kana:prefix.kana+endKana,kanji:base.kanji+suffix,romaji:prefix.romaji+endRomaji,alternatives};
        }
        // Liczby z końcówką 1–9: odmienia się końcowa cyfra, nie setki ani tysiące.
        const unit=n%10;
        const prefix=n < 10 ? {kana:'',romaji:''} : numeric(n-unit);
        if (unit === 8) alternatives.push(prefix.kana+(animal?'はちひき':'はちこ'));
        if (unit === 7) alternatives.push(prefix.kana+(animal?'しちひき':'しちこ'));
        return {kana:prefix.kana+forms[unit][0],kanji:base.kanji+suffix,
          romaji:prefix.romaji+forms[unit][1],alternatives};
      }
      function counterAnswer(mode,n) {
        if (mode === 'people') return {...people(n),polish:`${n} ${n === 1 ? 'osoba' : (n%10>=2 && n%10<=4 && (n%100<12||n%100>14)) ? 'osoby' : 'osób'}`};
        if (mode === 'animals') return {...counterWithChanges(n,mode),polish:`${n} małych zwierząt`};
        if (mode === 'objects') return {...counterWithChanges(n,mode),polish:`${n} drobnych przedmiotów`};
        if (mode === 'vehicles') {
          const base = numeric(n);
          return {kana:base.kana+'だい',kanji:base.kanji+'台',romaji:base.romaji+'dai',alternatives:[],polish:`${n} pojazdów / maszyn`};
        }
        const form = generalForms[n];
        return {kana:form[0],kanji:form[1],romaji:form[2],alternatives:[],polish:`${n} ${n===1?'rzecz':n>=2 && n<=4?'rzeczy':'rzeczy'} (licznik ogólny)`};
      }
      function answerFor(mode,n) {
        if (['people','animals','objects','vehicles','general'].includes(mode)) return counterAnswer(mode,n);
        const base = numeric(n);
        if (mode === 'basic') {
          // W liczbach złożonych alternatywny odczyt dotyczy także ostatniej cyfry.
          const finalDigit = n % 10;
          const alternateDigits = {4:'し',7:'しち',9:'く'};
          const suffix = alternateDigits[finalDigit];
          const alternatives = suffix ? [base.kana.slice(0,-digits[finalDigit].kana.length)+suffix] : [];
          return {...base, alternatives, polish:`liczba ${n}`};
        }
        if (mode === 'age') {
          const result = age(n);
          return {...result, alternatives:[...(result.alternatives||[]),result.kanji.replace(/歳$/,'才')],polish:`${n} ${n===1?'rok':(n%10>=2&&n%10<=4 && (n%100<12 || n%100>14))?'lata':'lat'} (wiek)`};
        }
        return {kana:base.kana+'ばん',kanji:base.kanji+'番',romaji:base.romaji+'ban',alternatives:[],polish:`numer ${n} / pozycja ${n} w kolejności`};
      }
      const normalize = value => value.normalize('NFKC').replace(/\s+/g,'').trim();
      const categoryDescriptions = {
        basic: { title:'Liczby podstawowe — liczenie i matematyka', text:'Zwykłe liczby używane w liczeniu i działaniach. Przykład: 23 → 二十三 (にじゅうさん).', label:'🔢  Liczby podstawowe', question:'Zapisz po japońsku liczbę…', hint:'Wpisz liczbę hiraganą lub znakami kanji.' },
        age: { title:'Podawanie wieku — licznik ～歳 (さい)', text:'Używane, gdy mówimy, ile ktoś ma lat. Końcówka zmienia niektóre odczyty: 1 rok → 一歳 (いっさい); 20 lat → 二十歳 (はたち).', label:'🎂  Wiek osoby (～歳)', question:'Jak powiedzieć po japońsku, że ktoś ma…', hint:'Podaj wiek po japońsku, łącznie z ～歳 (さい).' },
        ban: { title:'Numery i miejsca w kolejności — ～番 (ばん)', text:'Oznacza numer lub pozycję w uporządkowanym szeregu, np. numer 3 → 三番 (さんばん).', label:'🏷️  Numer / pozycja (～番)', question:'Jak zapisać po japońsku…', hint:'Podaj numer lub pozycję z końcówką ～番 (ばん).' },
        people: { title:'Liczenie osób — ～人 (にん)', text:'Liczymy ludzi. Zwróć uwagę na 1 osobę → 一人 (ひとり), 2 osoby → 二人 (ふたり) i 4 osoby → 四人 (よにん).', label:'👥  Osoby (～人)', question:'Jak policzysz po japońsku…', hint:'Wpisz liczbę osób, razem z licznikiem ～人.' },
        animals: { title:'Liczenie małych zwierząt — ～匹 (ひき)', text:'Koty, psy, ryby i owady liczymy przez ～匹. Przykłady: 1 → 一匹 (いっぴき), 3 → 三匹 (さんびき), 6 → 六匹 (ろっぴき). Większe zwierzęta mają inne liczniki.', label:'🐈  Małe zwierzęta (～匹)', question:'Jak policzysz po japońsku…', hint:'Użyj ～匹 i poprawnej wymowy: ひき, びき lub ぴき.' },
        objects: { title:'Liczenie drobnych przedmiotów — ～個 (こ)', text:'Licznik małych, konkretnych rzeczy, np. jabłek, jajek czy pudełek. 1 → 一個 (いっこ), 6 → 六個 (ろっこ).', label:'🍎  Przedmioty (～個)', question:'Jak policzysz po japońsku…', hint:'Użyj licznika ～個 (こ).' },
        vehicles: { title:'Pojazdy i maszyny — ～台 (だい)', text:'Samochody, rowery, komputery i maszyny. 2 → 二台 (にだい), 8 → 八台 (はちだい). Końcówka nie zmienia wymowy.', label:'🚗  Pojazdy / maszyny (～台)', question:'Jak policzysz po japońsku…', hint:'Dopisz ～台 (だい) do liczby.' },
        general: { title:'Ogólne liczenie przedmiotów — ～つ (1–10)', text:'Uniwersalne formy do 10: 1 → 一つ (ひとつ), 3 → 三つ (みっつ), 10 → 十 (とお). Powyżej 10 wybierz inny licznik (np. ～個).', label:'📦  Licznik ogólny (～つ)', question:'Jak policzysz w ogólny sposób…', hint:'Użyj tradycyjnej formy liczenia rzeczy (～つ) od 1 do 10.' }
      };
      let mode = 'basic';
      let drill = 'all';
      let chosenRange = '1-100';
      const ranges = {
        '1-100':[1,100],
        '101-999':[101,999],
        '1000-9999':[1000,9999]
      };
      const activeRange = () => mode === 'general' ? '1-100' : chosenRange;
      const trickyEnding = n => [4,7,9].includes(n % 10);
      const irregularHigher = n =>
        (n >= 300 && [3,6,8].includes(Math.floor(n/100)%10)) ||
        (n >= 3000 && [3,8].includes(Math.floor(n/1000)));
      const cache = new Map();
      function availableNumbers(category,kind) {
        const selected = category === 'general' ? '1-100' : chosenRange;
        const key = category + '|' + kind + '|' + selected;
        if (cache.has(key)) return cache.get(key);
        const [start,end] = category === 'general' ? [1,10] : ranges[selected];
        let all = Array.from({length:end-start+1},(_,i)=>start+i);
        if (kind !== 'all' && category !== 'general') all = all.filter(n => {
          if (irregularHigher(n)) return true;
          if (category === 'age') return n === 20 || n%10 === 1 || n%10 === 8 || (n%10 === 0 && n%100 !== 0);
          if (category === 'people') return n <= 2 || [4,7,9].includes(n%10);
          if (category === 'animals') return n%100 === 0 || [0,1,3,6,8].includes(n%10);
          if (category === 'objects') return n%100 === 0 || [0,1,6,8].includes(n%10);
          return trickyEnding(n); // 番 i 台 są regularne.
        });
        cache.set(key,all);
        return all;
      }
      const numberCount = () => availableNumbers(mode,drill).length;
      function highNumberRule(n) {
        const parts=[];
        const th=Math.floor(n/1000);
        const h=Math.floor(n%1000/100);
        if (th === 3) parts.push('3000 czytamy さんぜん, a nie さんせん.');
        if (th === 8) parts.push('8000 czytamy はっせん, a nie はちせん.');
        if (h === 3) parts.push('300 czytamy さんびゃく, a nie さんひゃく.');
        if (h === 6) parts.push('600 czytamy ろっぴゃく, a nie ろくひゃく.');
        if (h === 8) parts.push('800 czytamy はっぴゃく, a nie はちひゃく.');
        return parts.join(' ');
      }
      function ruleFor(category,n) {
        const baseNote=highNumberRule(n);
        if (category === 'people') {
          if (n === 1) return 'Jedna osoba to 一人 = ひとり, a nie いちにん.';
          if (n === 2) return 'Dwie osoby to 二人 = ふたり, ale 12 osób to じゅうににん.';
          if (n%10 === 4) return 'W wyrazach zakończonych na 四人 używamy よにん (np. 24 osoby: にじゅうよにん). ' + baseNote;
          if (n%10 === 7) return 'Końcówka 七人 ma odczyty ななにん i しちにん. ' + baseNote;
          if (n%10 === 9) return 'Końcówka 九人 ma odczyty きゅうにん i くにん. ' + baseNote;
          return baseNote;
        }
        if (category === 'animals') {
          if (n%1000 === 0) return `Pełne tysiące z 匹: ${numeric(n).kana}びき (np. 1000匹 → せんびき). ` + baseNote;
          if (n%100 === 0) return `Pełne setki z 匹 wymawiamy ～ひゃっぴき / ～びゃっぴき / ～ぴゃっぴき (np. 100匹 → ひゃっぴき). ` + baseNote;
          if (n%10 === 0) return 'Końcówka ～十匹 to じゅっぴき (także じっぴき). ' + baseNote;
          if (n%10 === 1) return 'Końcówka ～一匹 to いっぴき. ' + baseNote;
          if (n%10 === 3) return 'Końcówka ～三匹 to さんびき. ' + baseNote;
          if (n%10 === 6) return 'Końcówka ～六匹 to ろっぴき. ' + baseNote;
          if (n%10 === 8) return 'Końcówka ～八匹 to はっぴき (spotyka się też はちひき). ' + baseNote;
          return baseNote;
        }
        if (category === 'objects') {
          if (n%1000 === 0) return 'Pełne tysiące z 個: 1000個 → せんこ; nie dodajemy małego っ. ' + baseNote;
          if (n%100 === 0) return 'Pełne setki z 個 wymawiamy ～ひゃっこ / ～びゃっこ / ～ぴゃっこ (np. 100個 → ひゃっこ). ' + baseNote;
          if (n%10 === 0) return 'Końcówka ～十個 to じゅっこ (także じっこ). ' + baseNote;
          if (n%10 === 1) return 'Końcówka ～一個 to いっこ. ' + baseNote;
          if (n%10 === 6) return 'Końcówka ～六個 to ろっこ. ' + baseNote;
          if (n%10 === 8) return 'Końcówka ～八個 to はっこ (spotyka się też はちこ). ' + baseNote;
          return baseNote;
        }
        if (category === 'general') return '～つ ma tradycyjne nieregularne formy 1–10. Powyżej 10 użyj innego licznika, np. ～個.';
        if (category === 'age') {
          if (n === 20) return '20 lat: 二十歳 = はたち, a nie にじゅっさい.';
          if (n%10 === 1) return 'Końcówka ～一歳 to いっさい. ' + baseNote;
          if (n%10 === 8) return 'Końcówka ～八歳 to はっさい. ' + baseNote;
          if (n%10 === 0 && n%100 !== 0) return 'Końcówka ～十歳 to じゅっさい (także じっさい). ' + baseNote;
          return baseNote;
        }
        if (category === 'ban' || category === 'vehicles') {
          const note=trickyEnding(n) ? 'W tym liczniku preferujemy よん (4), なな (7) i きゅう (9); sama końcówka nie zmienia wymowy.' : '';
          return [baseNote,note].filter(Boolean).join(' ');
        }
        const notes={4:'4: よん (alternatywnie し).',7:'7: なな (alternatywnie しち).',9:'9: きゅう (alternatywnie く).'};
        return [baseNote,notes[n%10]].filter(Boolean).join(' ');
      }
      function refreshDrillUI() {
        document.querySelectorAll('[data-drill]').forEach(button => button.setAttribute('aria-pressed',String(button.dataset.drill===drill)));
        const desc = categoryDescriptions[mode];
        const extra = {
          basic:'Ćwiczysz liczby z 4, 7, 9 na końcu oraz nieregularne setki (300, 600, 800) i tysiące (3000, 8000) w wyższych zakresach.',
          age:'Ćwiczysz końcówki 1, 8 i pełne dziesiątki, szczególne 20 lat (はたち) oraz nieregularne setki i tysiące.',
          ban:'Końcówka ～番 nie tworzy własnych wyjątków wymowy. Ćwiczymy tutaj prawidłowy odczyt numerów kończących się na 4, 7 i 9.',
          people:'Ćwiczysz ひとり, ふたり, よにん, a także odczyty 7 i 9 osób.',
          animals:'Ćwiczysz zmiany ～ひき / ～びき / ～ぴき po 1, 3, 6, 8, dziesiątkach, setkach i tysiącach.',
          objects:'Ćwiczysz zmiany ～こ / ～っこ po 1, 6, 8, dziesiątkach, setkach i tysiącach.',
          vehicles:'～台 nie tworzy nieregularności. Przećwiczysz odczyt liczb kończących się na 4, 7 i 9.',
          general:'Wszystkie formy 1–10 mają własne tradycyjne odczyty, dlatego tryb wyjątków obejmuje cały zestaw.'
        };
        $('numberInfoText').textContent = desc.text + (drill === 'exceptions' ? ' ' + extra[mode] : '');
        $('numHint').textContent = drill === 'exceptions' ? 'Wpisz wymowę hiraganą — sprawdzamy odczyt, więc kanji nie wystarczy.' : desc.hint;
        $('numTag').textContent = desc.label + (drill === 'exceptions' ? ' · Wyjątki' : '');
        $('drillAll').textContent = mode === 'general' ? 'Wszystkie 1–10' : 'Wszystkie ' + activeRange().replace('-', '–');
        $('numTotalCount').textContent = numberCount();
        refreshRangeUI();
      }
      function refreshRangeUI() {
        document.querySelectorAll('[data-number-range]').forEach(button => {
          button.disabled = mode === 'general' && button.dataset.numberRange !== '1-100';
          button.setAttribute('aria-pressed',String(button.dataset.numberRange === activeRange()));
        });
        $('rangeNote').hidden = mode !== 'general';
        const firstRangeButton=document.querySelector('[data-number-range="1-100"]');
        if (firstRangeButton) firstRangeButton.textContent = mode === 'general' ? '1–10' : '1–100';
      }
      let remaining = [];
      let value = 1;
      let roundCount = 0;
      let gotIt = false;
      let skipped = false;
      let correctCount = 0;
      let skipCount = 0;
      let isComposing = false;
      let answer = answerFor(mode,value);
      function stats() {
        $('numCorrectCount').textContent = correctCount;
        $('numSkippedCount').textContent = skipCount;
        $('numDoneCount').textContent = correctCount+skipCount;
        $('numTotalCount').textContent = numberCount();
      }
      function showSolution(revealed) {
        $('numFeedback').hidden = false;
        $('numFeedback').classList.toggle('revealed',revealed);
        $('numFeedbackTitle').textContent = revealed ? 'Pominięto — zapamiętaj odpowiedź:' : '✓ Dobrze! Prawidłowa odpowiedź.';
        $('numJapanese').textContent = `${answer.kanji} · ${answer.kana}`;
        $('numRomaji').textContent = answer.romaji;
        $('numPolish').textContent = answer.polish;
        $('numRule').hidden = drill !== 'exceptions';
        $('numRule').textContent = drill === 'exceptions' ? 'Dlaczego to ważne? ' + ruleFor(mode,value) : '';
      }
      function nextNumber() {
        if (!remaining.length) {
          // Nowa runda dopiero po przejściu puli danej kategorii i rodzaju ćwiczenia.
          remaining = [...availableNumbers(mode,drill)];
          roundCount = 0;
          correctCount = 0;
          skipCount = 0;
          stats();
        }
        const chosen = Math.floor(Math.random()*remaining.length);
        value = remaining.splice(chosen,1)[0];
        roundCount++;
        answer = answerFor(mode,value);
        gotIt = false;
        skipped = false;
        $('numProgress').textContent = `Pytanie ${roundCount} z ${numberCount()}`;
        const prompts = {
          people:`${value} ${value===1?'osoba': (value%10>=2&&value%10<=4&&(value%100<12||value%100>14))?'osoby':'osób'}`,
          animals:`${value} ${value===1?'kot':(value%10>=2&&value%10<=4&&(value%100<12||value%100>14))?'koty':'kotów'}`,
          objects:`${value} ${value===1?'jabłko':(value%10>=2&&value%10<=4&&(value%100<12||value%100>14))?'jabłka':'jabłek'}`,
          vehicles:`${value} ${value===1?'samochód':(value%10>=2&&value%10<=4&&(value%100<12||value%100>14))?'samochody':'samochodów'}`,
          general:`${value} ${value===1?'rzecz':(value%10>=2&&value%10<=4&&(value%100<12||value%100>14))?'rzeczy':'rzeczy'}`
        };
        $('numDisplay').textContent = mode === 'age' ? `${value} ${value===1?'rok':(value%10>=2 && value%10<=4 && (value%100<12||value%100>14))?'lata':'lat'}` : mode === 'ban' ? `numer ${value}` : (prompts[mode] || String(value));
        $('numAnswer').disabled = false;
        $('numAnswer').value = '';
        $('numAnswer').classList.remove('wrong','correct');
        $('numAnswer').removeAttribute('aria-invalid');
        $('numFeedback').hidden = true;
        $('numRule').hidden = true;
        $('numNext').disabled = true;
        $('numSkip').disabled = false;
        $('numHelper').textContent = 'Wpisz poprawnie lub pomiń, aby przejść dalej.';
        if (!$('numberPanel').hidden) $('numAnswer').focus();
      }
      function check() {
        if (skipped || gotIt || isComposing) return;
        const typed = normalize($('numAnswer').value);
        const valid = (drill === 'exceptions'
          ? [answer.kana,...answer.alternatives.filter(item => /^[ぁ-ゖー]+$/.test(item))]
          : [answer.kana,answer.kanji,...answer.alternatives]).map(normalize);
        const correct = !!typed && valid.includes(typed);
        $('numAnswer').classList.toggle('correct',correct);
        $('numAnswer').classList.toggle('wrong',!!typed && !correct);
        if (typed) $('numAnswer').setAttribute('aria-invalid',String(!correct));
        else $('numAnswer').removeAttribute('aria-invalid');
        if (!correct) { $('numFeedback').hidden=true; return; }
        gotIt=true;
        correctCount++;
        stats();
        showSolution(false);
        $('numNext').disabled = false;
        $('numSkip').disabled = true;
        $('numAnswer').disabled = true;
        $('numHelper').textContent = 'Super! Możesz przejść do następnej liczby.';
      }
      function skip() {
        if (gotIt || skipped) return;
        skipped=true;
        skipCount++;
        stats();
        $('numAnswer').disabled = true;
        $('numAnswer').classList.remove('wrong','correct');
        $('numAnswer').removeAttribute('aria-invalid');
        showSolution(true);
        $('numNext').disabled = false;
        $('numSkip').disabled = true;
        $('numHelper').textContent = 'Przeczytaj odpowiedź i przejdź do kolejnej liczby.';
      }
      function changeMode(nextMode) {
        mode = nextMode;
        const desc = categoryDescriptions[mode];
        document.querySelectorAll('[data-num-mode]').forEach(button => button.setAttribute('aria-pressed',String(button.dataset.numMode===mode)));
        $('numberInfoTitle').textContent = desc.title;
        $('numQuestion').textContent = desc.question;
        refreshDrillUI();
        remaining=[];
        nextNumber();
      }
      function changeDrill(nextDrill) {
        if (drill === nextDrill) return;
        drill = nextDrill;
        refreshDrillUI();
        remaining=[];
        nextNumber();
      }
      function changeRange(nextRange) {
        if (mode === 'general' || chosenRange === nextRange || !ranges[nextRange]) return;
        chosenRange=nextRange;
        refreshDrillUI();
        remaining=[];
        nextNumber();
      }
      document.querySelectorAll('[data-number-range]').forEach(button => button.addEventListener('click',()=>changeRange(button.dataset.numberRange)));
      document.querySelectorAll('[data-num-mode]').forEach(button => button.addEventListener('click', () => changeMode(button.dataset.numMode)));
      document.querySelectorAll('[data-drill]').forEach(button => button.addEventListener('click', () => changeDrill(button.dataset.drill)));
      $('numAnswer').addEventListener('compositionstart',()=>isComposing=true);
      $('numAnswer').addEventListener('compositionend',()=>{isComposing=false; check();});
      $('numAnswer').addEventListener('input',check);
      $('numAnswer').addEventListener('keydown',event => {
        if (event.key==='Enter' && !event.isComposing && !isComposing && (gotIt || skipped)) {
          event.preventDefault();
          nextNumber();
        }
      });
      $('numSkip').addEventListener('click',skip);
      $('numNext').addEventListener('click',()=>{if(gotIt||skipped) nextNumber();});
      changeMode('basic');
    })();
  </script>

  <script>
    (() => {
      'use strict';
      const $=id=>document.getElementById(id);
      const questions=[{"kind":"wa","jp":"わたし□がくせいです。","ro":"watashi __ gakusei desu.","pl":"Jestem uczniem."},{"kind":"wa","jp":"わたし□ポーランドじんです。","ro":"watashi __ poorandojin desu.","pl":"Jestem Polakiem/Polką."},{"kind":"wa","jp":"これ□ほんです。","ro":"kore __ hon desu.","pl":"To jest książka."},{"kind":"wa","jp":"あれ□くるまです。","ro":"are __ kuruma desu.","pl":"Tamto jest samochód."},{"kind":"wa","jp":"きょう□げつようびです。","ro":"kyou __ getsuyoubi desu.","pl":"Dzisiaj jest poniedziałek."},{"kind":"wa","jp":"わたしのなまえ□アンナです。","ro":"watashi no namae __ Anna desu.","pl":"Mam na imię Anna."},{"kind":"wa","jp":"とうきょう□おおきいです。","ro":"toukyou __ ookii desu.","pl":"Tokio jest duże."},{"kind":"wa","jp":"このねこ□かわいいです。","ro":"kono neko __ kawaii desu.","pl":"Ten kot jest uroczy."},{"kind":"wa","jp":"そのいぬ□しろいです。","ro":"sono inu __ shiroi desu.","pl":"Ten pies jest biały."},{"kind":"wa","jp":"あした□やすみです。","ro":"ashita __ yasumi desu.","pl":"Jutro jest dzień wolny."},{"kind":"wa","jp":"にほんご□おもしろいです。","ro":"nihongo __ omoshiroi desu.","pl":"Japoński jest ciekawy."},{"kind":"wa","jp":"わたしのいえ□ちいさいです。","ro":"watashi no ie __ chiisai desu.","pl":"Mój dom jest mały."},{"kind":"ka","jp":"これはねこです□。","ro":"kore wa neko desu __.","pl":"Czy to jest kot?"},{"kind":"ka","jp":"あなたはがくせいです□。","ro":"anata wa gakusei desu __.","pl":"Czy jesteś uczniem?"},{"kind":"ka","jp":"にほんごをはなします□。","ro":"nihongo o hanashimasu __.","pl":"Czy mówisz po japońsku?"},{"kind":"ka","jp":"みずをのみます□。","ro":"mizu o nomimasu __.","pl":"Czy pijesz wodę?"},{"kind":"ka","jp":"いまなんじです□。","ro":"ima nanji desu __.","pl":"Która jest teraz godzina?"},{"kind":"ka","jp":"それはあなたのほんです□。","ro":"sore wa anata no hon desu __.","pl":"Czy to twoja książka?"},{"kind":"ka","jp":"りんごがすきです□。","ro":"ringo ga suki desu __.","pl":"Czy lubisz jabłka?"},{"kind":"ka","jp":"あしたいきます□。","ro":"ashita ikimasu __.","pl":"Czy pójdziesz jutro?"},{"kind":"ka","jp":"おなまえはなんです□。","ro":"onamae wa nan desu __.","pl":"Jak masz na imię?"},{"kind":"ka","jp":"コーヒーをのみます□。","ro":"koohii o nomimasu __.","pl":"Czy pijesz kawę?"},{"kind":"ka","jp":"これもおいしいです□。","ro":"kore mo oishii desu __.","pl":"Czy to też jest smaczne?"},{"kind":"ka","jp":"そのいえはふるいです□。","ro":"sono ie wa furui desu __.","pl":"Czy ten dom jest stary?"},{"kind":"no","jp":"これはわたし□ほんです。","ro":"kore wa watashi __ hon desu.","pl":"To moja książka."},{"kind":"no","jp":"あれはせんせい□くるまです。","ro":"are wa sensei __ kuruma desu.","pl":"Tamto jest samochód nauczyciela."},{"kind":"no","jp":"これはにほん□おちゃです。","ro":"kore wa nihon __ ocha desu.","pl":"To japońska herbata."},{"kind":"no","jp":"ともだち□ねこはくろいです。","ro":"tomodachi __ neko wa kuroi desu.","pl":"Kot mojego przyjaciela jest czarny."},{"kind":"no","jp":"わたし□なまえはマリアです。","ro":"watashi __ namae wa Maria desu.","pl":"Mam na imię Maria."},{"kind":"no","jp":"これはともだち□ペンです。","ro":"kore wa tomodachi __ pen desu.","pl":"To jest długopis przyjaciela."},{"kind":"no","jp":"がっこう□せんせいです。","ro":"gakkou __ sensei desu.","pl":"To nauczyciel ze szkoły."},{"kind":"no","jp":"きょう□てんきはいいです。","ro":"kyou __ tenki wa ii desu.","pl":"Dzisiejsza pogoda jest dobra."},{"kind":"no","jp":"あれはわたし□かばんです。","ro":"are wa watashi __ kaban desu.","pl":"Tamto jest moja torba."},{"kind":"no","jp":"このほん□なまえはなんですか。","ro":"kono hon __ namae wa nan desu ka.","pl":"Jaki jest tytuł tej książki?"},{"kind":"no","jp":"おかあさん□りょうりはおいしいです。","ro":"okaasan __ ryouri wa oishii desu.","pl":"Gotowanie mamy jest pyszne."},{"kind":"no","jp":"とうきょう□えきはおおきいです。","ro":"toukyou __ eki wa ookii desu.","pl":"Dworzec w Tokio jest duży."},{"kind":"nani","jp":"□をたべますか。","ro":"__ o tabemasu ka.","pl":"Co będziesz jeść?"},{"kind":"nani","jp":"□がすきですか。","ro":"__ ga suki desu ka.","pl":"Co lubisz?"},{"kind":"nani","jp":"□をのみますか。","ro":"__ o nomimasu ka.","pl":"Co będziesz pić?"},{"kind":"nani","jp":"□をかいますか。","ro":"__ o kaimasu ka.","pl":"Co kupisz?"},{"kind":"nani","jp":"□をみますか。","ro":"__ o mimasu ka.","pl":"Co oglądasz?"},{"kind":"nani","jp":"□をしますか。","ro":"__ o shimasu ka.","pl":"Co będziesz robić?"},{"kind":"nani","jp":"□がほしいですか。","ro":"__ ga hoshii desu ka.","pl":"Co chcesz dostać?"},{"kind":"nani","jp":"□をよみますか。","ro":"__ o yomimasu ka.","pl":"Co czytasz?"},{"kind":"nan","jp":"これは□ですか。","ro":"kore wa __ desu ka.","pl":"Co to jest?"},{"kind":"nan","jp":"いま□じですか。","ro":"ima __ ji desu ka.","pl":"Która godzina?"},{"kind":"nan","jp":"□にんいますか。","ro":"__ nin imasu ka.","pl":"Ile jest osób?"},{"kind":"nan","jp":"□だいありますか。","ro":"__ dai arimasu ka.","pl":"Ile jest pojazdów lub urządzeń?"},{"kind":"nan","jp":"りんごは□こありますか。","ro":"ringo wa __ ko arimasu ka.","pl":"Ile jest jabłek?"},{"kind":"nan","jp":"□ばんですか。","ro":"__ ban desu ka.","pl":"Który numer?"},{"kind":"nan","jp":"これは□のほんですか。","ro":"kore wa __ no hon desu ka.","pl":"O czym jest ta książka? / Jaka to książka?"},{"kind":"nan","jp":"おなまえは□ですか。","ro":"onamae wa __ desu ka.","pl":"Jak masz na imię?"},{"kind":"nan","jp":"□さいですか。","ro":"__ sai desu ka.","pl":"Ile masz lat?"},{"kind":"nan","jp":"□いろがすきですか。","ro":"__ iro ga suki desu ka.","pl":"Jaki kolor lubisz?"},{"kind":"o","jp":"みず□のみます。","ro":"mizu __ nomimasu.","pl":"Piję wodę."},{"kind":"o","jp":"りんご□たべます。","ro":"ringo __ tabemasu.","pl":"Jem jabłko."},{"kind":"o","jp":"ほん□よみます。","ro":"hon __ yomimasu.","pl":"Czytam książkę."},{"kind":"o","jp":"えいが□みます。","ro":"eiga __ mimasu.","pl":"Oglądam film."},{"kind":"o","jp":"おんがく□ききます。","ro":"ongaku __ kikimasu.","pl":"Słucham muzyki."},{"kind":"o","jp":"てがみ□かきます。","ro":"tegami __ kakimasu.","pl":"Piszę list."},{"kind":"o","jp":"にほんご□べんきょうします。","ro":"nihongo __ benkyou shimasu.","pl":"Uczę się japońskiego."},{"kind":"o","jp":"パン□かいます。","ro":"pan __ kaimasu.","pl":"Kupuję chleb."},{"kind":"o","jp":"まど□あけます。","ro":"mado __ akemasu.","pl":"Otwieram okno."},{"kind":"o","jp":"しゃしん□とります。","ro":"shashin __ torimasu.","pl":"Robię zdjęcie."},{"kind":"o","jp":"なまえ□かきます。","ro":"namae __ kakimasu.","pl":"Piszę imię."},{"kind":"o","jp":"テレビ□みます。","ro":"terebi __ mimasu.","pl":"Oglądam telewizję."},{"kind":"ga","jp":"ねこ□います。","ro":"neko __ imasu.","pl":"Jest kot."},{"kind":"ga","jp":"いぬ□います。","ro":"inu __ imasu.","pl":"Jest pies."},{"kind":"ga","jp":"ほん□あります。","ro":"hon __ arimasu.","pl":"Jest książka."},{"kind":"ga","jp":"あめ□ふっています。","ro":"ame __ futte imasu.","pl":"Pada deszcz."},{"kind":"ga","jp":"おなか□すきました。","ro":"onaka __ sukimashita.","pl":"Jestem głodny/głodna."},{"kind":"ga","jp":"にほんご□わかります。","ro":"nihongo __ wakarimasu.","pl":"Rozumiem japoński."},{"kind":"ga","jp":"コーヒー□すきです。","ro":"koohii __ suki desu.","pl":"Lubię kawę."},{"kind":"ga","jp":"じかん□ありません。","ro":"jikan __ arimasen.","pl":"Nie mam czasu."},{"kind":"ga","jp":"こども□います。","ro":"kodomo __ imasu.","pl":"Jest dziecko."},{"kind":"ga","jp":"みず□あります。","ro":"mizu __ arimasu.","pl":"Jest woda."},{"kind":"ga","jp":"このケーキ□おいしいです。","ro":"kono keeki __ oishii desu.","pl":"To właśnie to ciasto jest smaczne."},{"kind":"ga","jp":"だれ□きましたか。","ro":"dare __ kimashita ka.","pl":"Kto przyszedł?"},{"kind":"ni","jp":"ろくじ□おきます。","ro":"rokuji __ okimasu.","pl":"Wstaję o szóstej."},{"kind":"ni","jp":"しちじ□ねます。","ro":"shichiji __ nemasu.","pl":"Kładę się spać o siódmej."},{"kind":"ni","jp":"つくえのうえ□ほんがあります。","ro":"tsukue no ue __ hon ga arimasu.","pl":"Na biurku znajduje się książka."},{"kind":"ni","jp":"いえ□ねこがいます。","ro":"ie __ neko ga imasu.","pl":"W domu jest kot."},{"kind":"ni","jp":"はちじ□がっこうがはじまります。","ro":"hachiji __ gakkou ga hajimarimasu.","pl":"Szkoła zaczyna się o ósmej."},{"kind":"ni","jp":"いす□すわります。","ro":"isu __ suwarimasu.","pl":"Siadam na krześle."},{"kind":"ni","jp":"ともだち□プレゼントをあげます。","ro":"tomodachi __ purezento o agemasu.","pl":"Daję prezent przyjacielowi."},{"kind":"ni","jp":"せんせい□しつもんします。","ro":"sensei __ shitsumon shimasu.","pl":"Zadaję pytanie nauczycielowi."},{"kind":"ni","jp":"へや□テレビがあります。","ro":"heya __ terebi ga arimasu.","pl":"W pokoju znajduje się telewizor."},{"kind":"ni","jp":"はは□でんわします。","ro":"haha __ denwa shimasu.","pl":"Dzwonię do mamy."},{"kind":"ni","jp":"くじ□しごとがはじまります。","ro":"kuji __ shigoto ga hajimarimasu.","pl":"Praca zaczyna się o dziewiątej."},{"kind":"ni","jp":"かばんのなか□かぎがあります。","ro":"kaban no naka __ kagi ga arimasu.","pl":"W torbie jest klucz."},{"kind":"de","jp":"バス□いきます。","ro":"basu __ ikimasu.","pl":"Jadę autobusem."},{"kind":"de","jp":"でんしゃ□いきます。","ro":"densha __ ikimasu.","pl":"Jadę pociągiem."},{"kind":"de","jp":"がっこう□べんきょうします。","ro":"gakkou __ benkyou shimasu.","pl":"Uczę się w szkole."},{"kind":"de","jp":"うち□ごはんをたべます。","ro":"uchi __ gohan o tabemasu.","pl":"Jem posiłek w domu."},{"kind":"de","jp":"としょかん□ほんをよみます。","ro":"toshokan __ hon o yomimasu.","pl":"Czytam książkę w bibliotece."},{"kind":"de","jp":"ペン□なまえをかきます。","ro":"pen __ namae o kakimasu.","pl":"Piszę imię długopisem."},{"kind":"de","jp":"はし□たべます。","ro":"hashi __ tabemasu.","pl":"Jem pałeczkami."},{"kind":"de","jp":"レストラン□たべます。","ro":"resutoran __ tabemasu.","pl":"Jem w restauracji."},{"kind":"de","jp":"こうえん□あそびます。","ro":"kouen __ asobimasu.","pl":"Bawię się w parku."},{"kind":"de","jp":"にほんご□はなします。","ro":"nihongo __ hanashimasu.","pl":"Mówię po japońsku."},{"kind":"de","jp":"じてんしゃ□いきます。","ro":"jitensha __ ikimasu.","pl":"Jadę rowerem."},{"kind":"de","jp":"へや□えいがをみます。","ro":"heya __ eiga o mimasu.","pl":"Oglądam film w pokoju."},{"kind":"mo","jp":"わたし□いきます。","ro":"watashi __ ikimasu.","pl":"Ja też idę."},{"kind":"mo","jp":"これ□おいしいです。","ro":"kore __ oishii desu.","pl":"To też jest smaczne."},{"kind":"mo","jp":"ねこ□います。","ro":"neko __ imasu.","pl":"Jest też kot."},{"kind":"mo","jp":"おちゃ□のみます。","ro":"ocha __ nomimasu.","pl":"Piję też herbatę."},{"kind":"mo","jp":"わたしのともだち□がくせいです。","ro":"watashi no tomodachi __ gakusei desu.","pl":"Mój przyjaciel też jest uczniem."},{"kind":"mo","jp":"きょう□あついです。","ro":"kyou __ atsui desu.","pl":"Dzisiaj też jest gorąco."},{"kind":"mo","jp":"そのほん□おもしろいです。","ro":"sono hon __ omoshiroi desu.","pl":"Ta książka też jest ciekawa."},{"kind":"mo","jp":"いもうと□いきます。","ro":"imouto __ ikimasu.","pl":"Młodsza siostra też idzie."},{"kind":"mo","jp":"バナナ□あります。","ro":"banana __ arimasu.","pl":"Są też banany."},{"kind":"mo","jp":"ここ□しずかです。","ro":"koko __ shizuka desu.","pl":"Tutaj też jest cicho."},{"kind":"to","jp":"ともだち□はなします。","ro":"tomodachi __ hanashimasu.","pl":"Rozmawiam z przyjacielem."},{"kind":"to","jp":"はは□かいものします。","ro":"haha __ kaimono shimasu.","pl":"Robię zakupy z mamą."},{"kind":"to","jp":"ねこ□いぬがいます。","ro":"neko __ inu ga imasu.","pl":"Jest kot i pies."},{"kind":"to","jp":"りんご□バナナをかいます。","ro":"ringo __ banana o kaimasu.","pl":"Kupuję jabłka i banany."},{"kind":"to","jp":"ともだち□えいがをみます。","ro":"tomodachi __ eiga o mimasu.","pl":"Oglądam film z przyjacielem."},{"kind":"to","jp":"おとうさん□こうえんにいきます。","ro":"otousan __ kouen ni ikimasu.","pl":"Idę do parku z tatą."},{"kind":"to","jp":"パン□たまごをたべます。","ro":"pan __ tamago o tabemasu.","pl":"Jem chleb i jajka."},{"kind":"to","jp":"いもうと□あそびます。","ro":"imouto __ asobimasu.","pl":"Bawię się z młodszą siostrą."},{"kind":"to","jp":"にほんご□えいごをべんきょうします。","ro":"nihongo __ eigo o benkyou shimasu.","pl":"Uczę się japońskiego i angielskiego."},{"kind":"to","jp":"せんせい□はなしました。","ro":"sensei __ hanashimashita.","pl":"Rozmawiałem z nauczycielem."},{"kind":"e","jp":"にほん□いきます。","ro":"nihon __ ikimasu.","pl":"Jadę do Japonii (w kierunku Japonii)."},{"kind":"e","jp":"とうきょう□いきます。","ro":"toukyou __ ikimasu.","pl":"Jadę do Tokio."},{"kind":"e","jp":"がっこう□いきます。","ro":"gakkou __ ikimasu.","pl":"Idę do szkoły."},{"kind":"e","jp":"えき□あるきます。","ro":"eki __ arukimasu.","pl":"Idę pieszo w stronę dworca."},{"kind":"e","jp":"うみ□いきます。","ro":"umi __ ikimasu.","pl":"Idę nad morze."},{"kind":"e","jp":"こうえん□いきます。","ro":"kouen __ ikimasu.","pl":"Idę do parku."},{"kind":"e","jp":"うち□かえります。","ro":"uchi __ kaerimasu.","pl":"Wracam do domu."},{"kind":"e","jp":"にほん□ようこそ。","ro":"nihon __ youkoso.","pl":"Witamy w Japonii!"}];
      const symbols={
        wa:{kana:'は',ro:'wa',accept:['は','wa'],choices:['wa','ga','o','ni']},
        ka:{kana:'か',ro:'ka',accept:['か','ka'],choices:['ka','no','mo','ga']},
        no:{kana:'の',ro:'no',accept:['の','no'],choices:['no','wa','ni','to']},
        nani:{kana:'なに',ro:'nani',accept:['なに','nani'],choices:['nani','nan']},
        nan:{kana:'なん',ro:'nan',accept:['なん','nan'],choices:['nani','nan']},
        o:{kana:'を',ro:'o',accept:['を','o','wo'],choices:['o','wa','ga','ni']},
        ga:{kana:'が',ro:'ga',accept:['が','ga'],choices:['ga','wa','o','ni']},
        ni:{kana:'に',ro:'ni',accept:['に','ni'],choices:['ni','de','wa','ga']},
        de:{kana:'で',ro:'de',accept:['で','de'],choices:['de','ni','wa','o']},
        mo:{kana:'も',ro:'mo',accept:['も','mo'],choices:['mo','wa','no','ga']},
        to:{kana:'と',ro:'to',accept:['と','to'],choices:['to','no','de','wa']},
        e:{kana:'へ',ro:'e',accept:['へ','e'],choices:['e','de','wa','o']}
      };
      const groups=[
        ['all','Wszystkie','mix','Wybieraj spośród wszystkich tematów.'],
        ['wa','は','wa','O kim albo o czym mówimy?'],
        ['ka','か','ka','Czy to pytanie?'],
        ['no','の','no','Czyja rzecz? Z czym związana?'],
        ['nani','なに / なん','nani / nan','Co? Ile? Przyjrzyj się słowu po luce.'],
        ['o','を','o','Co ktoś robi z daną rzeczą?'],
        ['ga','が','ga','Co jest, kto przyszedł lub co lubimy?'],
        ['ni','に','ni','O której? Gdzie coś się znajduje? Komu?'],
        ['de','で','de','Gdzie odbywa się czynność? Czym?'],
        ['mo','も','mo','Czy po polsku jest „też”?'],
        ['to','と','to','Z kim? Albo co i co?'],
        ['e','へ','e','W jakim kierunku ktoś się rusza?']
      ];
      const explanations={
        wa:'は (wymawiane „wa”) pokazuje, o kim lub o czym mówimy. Wyobraź sobie „jeśli chodzi o…”.',
        ka:'か (ka) na końcu zdania grzecznościowego zmienia wypowiedź w pytanie.',
        no:'の (no) łączy wyrazy: pokazuje, do kogo coś należy albo jaką rzecz mamy na myśli.',
        nani:'なに (nani) to „co?”. Tu występuje przed wyrażeniem, w którym naturalnie czytamy pełne „nani”.',
        nan:'なん (nan) to również „co?”. Używamy go m.in. przed です i licznikami (nan-nin, nan-ko, nan-dai).',
        o:'を zapisujemy „wo”, ale wymawiamy „o”. Pokazuje, na czym wykonujemy czynność: np. wodę pijemy, książkę czytamy.',
        ga:'が (ga) często pokazuje, co istnieje, co lubimy albo kto wykonuje czynność. W szczególnym kontekście zdanie może mieć inne podkreślenie.',
        ni:'に (ni) może wskazywać dokładną godzinę, miejsce, w którym coś się znajduje, albo osobę, która coś otrzymuje.',
        de:'で (de) opisuje miejsce działania („w szkole”) lub środek/narzędzie („autobusem”, „długopisem”).',
        mo:'も (mo) mówi, że coś dotyczy też kogoś lub czegoś innego. To japońskie „też”.',
        to:'と (to) może oznaczać „z kimś” lub łączyć rzeczowniki jak polskie „i”.',
        e:'へ (wymawiane „e”) pokazuje kierunek ruchu. Przy podróży często da się też użyć に.'
      };
      const introHints={
        wa:'Pomyśl, o czym jest całe zdanie — to japońskie „jeśli chodzi o…”.',
        ka:'Sprawdź, czy całe zdanie po polsku jest pytaniem.',
        no:'Sprawdź, czy ktoś jest właścicielem rzeczy lub czy jeden wyraz opisuje drugi.',
        nani:'Zwróć uwagę na słowo stojące po luce; to ono decyduje o sposobie wymowy.',
        nan:'Przy pytaniach o wiek, liczbę osób, czas lub przed です często mówimy „nan”.',
        o:'Znajdź rzecz, którą ktoś pije, je, czyta, ogląda lub kupuje.',
        ga:'Pomyśl, co znajduje się na miejscu, kto przyszedł albo co ktoś lubi.',
        ni:'Wskazówka: godzina, miejsce istnienia lub odbiorca.',
        de:'Sprawdź, czy chodzi o miejsce czynności lub narzędzie/pojazd.',
        mo:'Poszukaj w polskim tłumaczeniu słowa „też”.',
        to:'Czy chodzi o „razem z” albo „i” między rzeczami?',
        e:'Sprawdź, czy ktoś zmierza w stronę jakiegoś miejsca.'
      };
      const selectableGroups=groups.filter(g=>g[0]!=='all');
      let selectedGroups=new Set(selectableGroups.map(g=>g[0]));
      let mode='pick',deck=[],index=0,question=null,answered=false,skipped=false,correctCount=0,skippedCount=0;
      let composing=false,wrongChoice=null;
      const normalize=value=>String(value||'').normalize('NFKC').toLocaleLowerCase('pl').replace(/[\s\u200b]/g,'');
      const groupOf=q=>q.kind==='nan'?'nani':q.kind;
      function shuffle(arr){
        for(let i=arr.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[arr[i],arr[j]]=[arr[j],arr[i]];}
        return arr;
      }
      const labelOf=key=>{
        const s=symbols[key]; return s.kana+' ('+s.ro+')';
      };
      function renderGroups(){
        const container=$('grammarGroups'); container.replaceChildren();
        const allSelected=selectedGroups.size===selectableGroups.length;
        groups.forEach(([key,name,roman])=>{
          const active=key==='all'?allSelected:selectedGroups.has(key);
          const button=document.createElement('button');
          button.type='button';button.className='grammar-group';
          button.setAttribute('aria-pressed',String(active));
          button.setAttribute('aria-label',key==='all'?'Wybierz wszystkie kategorie':name+' ('+roman+') — '+(active?'zaznaczona':'niezaznaczona'));
          const label=document.createElement('span');label.className='grammar-label';
          const title=document.createElement('span');title.className='grammar-kana';title.textContent=name;
          const reading=document.createElement('span');reading.className='grammar-roman';reading.textContent=roman;
          const tick=document.createElement('span');tick.className='grammar-selected';tick.textContent='✓';tick.setAttribute('aria-hidden','true');
          label.append(title,reading);button.append(label,tick);
          button.addEventListener('click',()=>setGroup(key));
          container.append(button);
        });
        const count=selectedGroups.size;
        const names=selectableGroups.filter(g=>selectedGroups.has(g[0])).map(g=>g[1]);
        $('grammarTopicTag').textContent=allSelected?'✦ Wszystkie cząsteczki':
          count===1?'✦ '+names[0]:`✦ Wybrane kategorie: ${count}`;
        $('grammarSelectionSummary').textContent=allSelected
          ? `Zaznaczono wszystkie ${count} kategorii. Kliknij jedną, aby ćwiczyć tylko ją, a potem dodawaj kolejne.`
          : `Zaznaczono ${count} z ${selectableGroups.length}: ${names.join(' · ')}. Quiz losuje przykłady tylko z tych kategorii.`;
      }
      function resetDeck(){
        deck=shuffle(questions.filter(q=>selectedGroups.has(groupOf(q))).slice());
        index=0;correctCount=0;skippedCount=0;
        nextQuestion();
      }
      function setGroup(next){
        if(next==='all'){
          if(selectedGroups.size===selectableGroups.length && deck.length) return;
          selectedGroups=new Set(selectableGroups.map(g=>g[0]));
        } else if(selectedGroups.size===selectableGroups.length){
          // Pierwsze kliknięcie po "Wszystkie" rozpoczyna własny wybór.
          selectedGroups=new Set([next]);
        } else if(selectedGroups.has(next)){
          // Zawsze pozostawiamy co najmniej jedną wybraną kategorię.
          if(selectedGroups.size===1) return;
          selectedGroups.delete(next);
        } else {
          selectedGroups.add(next);
        }
        renderGroups();resetDeck();
      }
      function updateStats(){
        $('grammarCorrect').textContent=correctCount;
        $('grammarSkipped').textContent=skippedCount;
        $('grammarDone').textContent=index;
        $('grammarTotal').textContent=deck.length;
      }
      function renderOptions(){
        const container=$('grammarOptions'); container.replaceChildren();
        let choices=[...symbols[question.kind].choices];
        shuffle(choices).forEach(key=>{
          const s=symbols[key],button=document.createElement('button');
          button.type='button';button.className='grammar-choice';button.dataset.option=key;
          const jp=document.createElement('span');jp.textContent=s.kana;
          const ro=document.createElement('small');ro.textContent=s.ro;
          button.append(jp,ro);
          button.setAttribute('aria-label',labelOf(key));
          button.addEventListener('click',()=>choose(key));
          container.append(button);
        });
      }
      function nextQuestion(){
        if(index>=deck.length){
          deck=shuffle(deck.slice());index=0;correctCount=0;skippedCount=0;
        }
        question=deck[index++]; answered=false;skipped=false;wrongChoice=null;
        $('grammarProgress').textContent=`Pytanie ${index} z ${deck.length}`;
        $('grammarJapanese').textContent=question.jp;
        $('grammarRomaji').textContent=question.ro;
        $('grammarPolish').textContent=question.pl;
        $('grammarAnswer').value='';$('grammarAnswer').disabled=false;
        $('grammarAnswer').classList.remove('correct','wrong');$('grammarAnswer').removeAttribute('aria-invalid');
        $('grammarFeedback').hidden=true;$('grammarFeedback').classList.remove('revealed');
        $('grammarNext').disabled=true;$('grammarSkip').disabled=false;
        $('grammarHintBtn').textContent='💡 Pokaż podpowiedź';
        $('grammarHintBtn').setAttribute('aria-expanded','false');$('grammarHint').hidden=true;
        $('grammarHint').textContent=introHints[question.kind];
        $('grammarHelper').textContent='Spróbuj poprawnie lub pomiń, aby przejść dalej.';
        renderOptions();updateStats();
        if(!$('grammarPanel').hidden && mode==='type') $('grammarAnswer').focus();
      }
      function showSolution(isSkipped){
        $('grammarFeedback').hidden=false;
        $('grammarFeedback').classList.toggle('revealed',isSkipped);
        $('grammarFeedbackTitle').textContent=isSkipped?'↪ Pominięto — oto poprawna odpowiedź:':'✓ Świetnie! Poprawna odpowiedź.';
        $('grammarCompletedJapanese').textContent=question.jp.replace('□',symbols[question.kind].kana);
        $('grammarCompletedRomaji').textContent=question.ro.replace('__',symbols[question.kind].ro);
        $('grammarExplanation').textContent=explanations[question.kind];
        $('grammarNext').disabled=false;$('grammarSkip').disabled=true;
        $('grammarAnswer').disabled=true;
        document.querySelectorAll('.grammar-choice').forEach(b=>{
          b.disabled=true;
          b.classList.toggle('correct',b.dataset.option===question.kind);
          b.classList.remove('wrong');
        });
      }
      function correct(){
        if(answered||skipped)return;
        answered=true;correctCount++;updateStats();
        $('grammarAnswer').classList.remove('wrong');$('grammarAnswer').classList.add('correct');
        $('grammarAnswer').setAttribute('aria-invalid','false');
        $('grammarHelper').textContent='Dobrze! Przeczytaj wyjaśnienie i przejdź dalej.';
        showSolution(false);
      }
      function choose(selected){
        if(mode!=='pick'||answered||skipped)return;
        if(selected===question.kind){correct();return;}
        wrongChoice=selected;
        document.querySelectorAll('.grammar-choice').forEach(b=>b.classList.toggle('wrong',b.dataset.option===selected));
        $('grammarHelper').textContent='To nie ta cząsteczka — spróbuj jeszcze raz albo skorzystaj z podpowiedzi.';
      }
      function checkTyped(){
        if(mode!=='type'||answered||skipped||composing) return;
        const v=normalize($('grammarAnswer').value),good=v&&symbols[question.kind].accept.some(a=>normalize(a)===v);
        $('grammarAnswer').classList.toggle('correct',!!good);
        $('grammarAnswer').classList.toggle('wrong',!!v&&!good);
        if(v) $('grammarAnswer').setAttribute('aria-invalid',String(!good));
        else $('grammarAnswer').removeAttribute('aria-invalid');
        if(good)correct();
      }
      function skip(){
        if(answered||skipped)return;
        skipped=true;skippedCount++;updateStats();
        $('grammarAnswer').classList.remove('correct','wrong');$('grammarAnswer').removeAttribute('aria-invalid');
        showSolution(true);
        $('grammarHelper').textContent='Zobacz odpowiedź i jej wyjaśnienie; możesz przejść dalej.';
      }
      function setMode(next){
        if(mode===next)return;
        mode=next;
        $('grammarPickMode').setAttribute('aria-pressed',String(mode==='pick'));
        $('grammarTypeMode').setAttribute('aria-pressed',String(mode==='type'));
        $('grammarOptions').hidden=mode!=='pick';$('grammarTypeWrap').hidden=mode!=='type';
        if(!answered&&!skipped){
          $('grammarAnswer').value='';$('grammarAnswer').classList.remove('wrong','correct');
          document.querySelectorAll('.grammar-choice').forEach(b=>b.classList.remove('wrong'));
        }
        if(mode==='type' && !answered && !skipped) $('grammarAnswer').focus();
      }
      $('grammarPickMode').addEventListener('click',()=>setMode('pick'));
      $('grammarTypeMode').addEventListener('click',()=>setMode('type'));
      $('grammarAnswer').addEventListener('compositionstart',()=>composing=true);
      $('grammarAnswer').addEventListener('compositionend',()=>{composing=false;checkTyped();});
      $('grammarAnswer').addEventListener('input',checkTyped);
      $('grammarAnswer').addEventListener('keydown',e=>{
        if(e.key==='Enter'&&!e.isComposing&&!composing&&(answered||skipped)){e.preventDefault();nextQuestion();}
      });
      $('grammarNext').addEventListener('click',()=>{if(answered||skipped)nextQuestion();});
      $('grammarSkip').addEventListener('click',skip);
      $('grammarHintBtn').addEventListener('click',()=>{
        const show=$('grammarHint').hidden;
        $('grammarHint').hidden=!show;
        $('grammarHintBtn').setAttribute('aria-expanded',String(show));
        $('grammarHintBtn').textContent=show?'Ukryj podpowiedź':'💡 Pokaż podpowiedź';
      });
      renderGroups();
      resetDeck();
    })();
  </script>
</body>
</html>
