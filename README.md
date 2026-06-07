# melomelo-studio
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MeloMelo Studio</title>
<link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>🎀</text></svg>">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600;700&family=Syne:wght@400;600;700;800&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<link rel="stylesheet" href="style.css">
</head>
<body>

<!-- ═══ LOGIN SCREEN ═══ -->
<div id="loginScreen" class="screen-overlay">
  <div class="login-card">
    <div class="login-logo">🎀 MeloMelo<span>Studio</span></div>
    <div class="login-sub">Professional Pastel DAW</div>
    <div class="login-form">
      <div class="login-field">
        <label class="lf-label" id="ll_password">비밀번호</label>
        <div class="lf-input-wrap">
          <input type="password" id="loginPwd" class="lf-input" placeholder="••••••" onkeydown="if(event.key==='Enter')doLogin()">
          <button class="lf-eye" onclick="togglePwdVis('loginPwd',this)">👁</button>
        </div>
      </div>
      <div class="login-error" id="loginError"></div>
      <button class="login-btn" onclick="doLogin()" id="ll_enter">입장하기</button>
    </div>
    <div class="login-lang">
      <button class="ll-btn active" onclick="setLoginLang('ko',this)">한국어</button>
      <button class="ll-btn" onclick="setLoginLang('en',this)">English</button>
    </div>
    <div class="login-hint" id="ll_hint">기본 비밀번호: 1234</div>
  </div>
  <div class="login-bg-circles">
    <div class="lbc lbc1"></div><div class="lbc lbc2"></div><div class="lbc lbc3"></div>
  </div>
</div>

<!-- ═══ MAIN APP ═══ -->
<div id="appRoot" class="hidden">

<!-- NAV -->
<nav class="topnav">
  <div class="tn-logo">🎀 MeloMelo</div>
  
  <div class="tn-transport">
    <button class="tb" id="tb_rw" onclick="transportRewind()" title="Rewind">⏮</button>
    <button class="tb tb-play" id="tb_play" onclick="transportPlay()">▶</button>
    <button class="tb" id="tb_stop" onclick="transportStop()" title="Stop">⏹</button>
    <button class="tb tb-rec" id="tb_rec" onclick="transportRec()" title="Record">⏺</button>
    <div class="tb-sep"></div>
    <div class="bpm-wrap">
      <span class="bpm-lbl">BPM</span>
      <input type="number" id="bpmInp" value="120" min="40" max="240" class="bpm-inp" onchange="setBpm(+this.value)">
      <button class="tap-btn" id="tapBtn" onclick="tapTempo()">TAP</button>
    </div>
    <select id="timeSigSel" class="tn-sel" onchange="setTimeSig(+this.value)">
      <option value="4" selected>4/4</option>
      <option value="3">3/4</option>
      <option value="6">6/8</option>
    </select>
    <div class="pos-display" id="posDisplay">1 : 1 : 000</div>
  </div>

  <div class="tn-right">
    <div class="master-vol-wrap">
      <span class="mv-lbl">Master</span>
      <input type="range" id="masterVolSl" min="0" max="100" value="80" class="mv-sl" oninput="setMasterVol(+this.value)">
      <span class="mv-val" id="masterVolVal">80</span>
    </div>
    <button class="tn-icon-btn" onclick="openSaveModal()" title="Save / Load">💾</button>
    <button class="tn-icon-btn" onclick="openSettings()" title="Settings">⚙️</button>
    <button class="tn-icon-btn" onclick="toggleTheme()" id="themeBtn" title="Theme">🌙</button>
    <button class="tn-icon-btn" onclick="toggleLang()" id="langBtn">EN</button>
    <button class="tn-icon-btn" onclick="doLogout()" title="Logout">🔒</button>
  </div>
</nav>

<!-- BODY -->
<div class="app-body">

<!-- SIDEBAR -->
<aside class="sidebar" id="sidebar">
  <div class="sb-group">
    <div class="sb-group-title" id="sg_compose">작곡</div>
    <button class="sb-item active" id="sbi-arrange" onclick="switchTab('arrange')">
      <span class="sbi-ico">⬛</span><span id="sbn_arrange">어레인지</span>
    </button>
    <button class="sb-item" id="sbi-piano" onclick="switchTab('piano')">
      <span class="sbi-ico">🎹</span><span id="sbn_piano">피아노 롤</span>
    </button>
    <button class="sb-item" id="sbi-drum" onclick="switchTab('drum')">
      <span class="sbi-ico">🥁</span><span id="sbn_drum">드럼 머신</span>
    </button>
    <button class="sb-item" id="sbi-chord" onclick="switchTab('chord')">
      <span class="sbi-ico">🎸</span><span id="sbn_chord">코드 빌더</span>
    </button>
    <button class="sb-item" id="sbi-melody" onclick="switchTab('melody')">
      <span class="sbi-ico">✏️</span><span id="sbn_melody">멜로디 스케치</span>
    </button>
    <button class="sb-item" id="sbi-mixer" onclick="switchTab('mixer')">
      <span class="sbi-ico">🎚</span><span id="sbn_mixer">믹서</span>
    </button>
  </div>
  <div class="sb-group">
    <div class="sb-group-title" id="sg_tools">도구</div>
    <button class="sb-item" id="sbi-arp" onclick="switchTab('arp')">
      <span class="sbi-ico">🔄</span><span id="sbn_arp">아르페지에이터</span>
    </button>
    <button class="sb-item" id="sbi-scale" onclick="switchTab('scale')">
      <span class="sbi-ico">📐</span><span id="sbn_scale">스케일 헬퍼</span>
    </button>
    <button class="sb-item" id="sbi-eq" onclick="switchTab('eq')">
      <span class="sbi-ico">〰️</span><span id="sbn_eq">이퀄라이저</span>
    </button>
  </div>
  <div class="sb-group">
    <div class="sb-group-title" id="sg_files">파일</div>
    <button class="sb-item" id="sbi-projects" onclick="switchTab('projects')">
      <span class="sbi-ico">📁</span><span id="sbn_projects">내 프로젝트</span>
    </button>
    <button class="sb-item" id="sbi-loops" onclick="switchTab('loops')">
      <span class="sbi-ico">🔁</span><span id="sbn_loops">루프 라이브러리</span>
    </button>
  </div>
  <div class="sb-group">
    <div class="sb-group-title" id="sg_learn">학습</div>
    <button class="sb-item" id="sbi-tutorial" onclick="switchTab('tutorial')">
      <span class="sbi-ico">📖</span><span id="sbn_tutorial">튜토리얼</span>
    </button>
    <button class="sb-item" id="sbi-glossary" onclick="switchTab('glossary')">
      <span class="sbi-ico">📚</span><span id="sbn_glossary">용어사전</span>
    </button>
  </div>
</aside>

<!-- MAIN -->
<main class="main-area" id="mainArea">

  <!-- ── ARRANGE ── -->
  <div class="tab-view active" id="tab-arrange">
    <div class="tab-header">
      <h2 class="tab-title" id="th_arrange">어레인지</h2>
      <div class="tab-actions">
        <button class="ta-btn" onclick="addTrack('synth')" id="ta_addsynth">+ 신스</button>
        <button class="ta-btn" onclick="addTrack('drum')" id="ta_adddrum">+ 드럼</button>
        <button class="ta-btn" onclick="addTrack('bass')" id="ta_addbass">+ 베이스</button>
        <button class="ta-btn" onclick="addTrack('pad')" id="ta_addpad">+ 패드</button>
        <button class="ta-btn" onclick="addTrack('sample')" id="ta_addsample">+ 샘플</button>
        <button class="ta-btn ta-btn-outline" onclick="clearArrange()" id="ta_clearall">전체 초기화</button>
      </div>
    </div>
    <div class="ruler-strip" id="rulerStrip"></div>
    <div class="track-area" id="trackArea">
      <div class="empty-state" id="arrangeEmpty">
        <div class="es-icon">🎼</div>
        <div class="es-text" id="es_arrange">트랙을 추가해서 작곡을 시작하세요</div>
      </div>
    </div>
  </div>

  <!-- ── PIANO ROLL ── -->
  <div class="tab-view" id="tab-piano">
    <div class="tab-header">
      <h2 class="tab-title" id="th_piano">피아노 롤</h2>
      <div class="tab-actions">
        <select class="ta-sel" id="rollOctSel" onchange="rebuildRoll()">
          <option value="6">Oct 6</option><option value="5">Oct 5</option>
          <option value="4" selected>Oct 4</option><option value="3">Oct 3</option><option value="2">Oct 2</option>
        </select>
        <select class="ta-sel" id="rollBarsSel" onchange="rebuildRoll()">
          <option value="1">1 Bar</option><option value="2" selected>2 Bars</option>
          <option value="4">4 Bars</option><option value="8">8 Bars</option>
        </select>
        <select class="ta-sel" id="rollSnapSel">
          <option value="4">1/4</option><option value="8" selected>1/8</option>
          <option value="16">1/16</option><option value="32">1/32</option>
        </select>
        <select class="ta-sel" id="rollInstSel">
          <option value="triangle" selected>Triangle</option>
          <option value="sine">Sine</option>
          <option value="sawtooth">Sawtooth</option>
          <option value="square">Square</option>
        </select>
        <button class="ta-btn" onclick="quantizeNotes()" id="ta_quantize">Quantize</button>
        <button class="ta-btn ta-btn-play" onclick="playRoll()" id="ta_rplay">▶ Play</button>
        <button class="ta-btn ta-btn-danger" onclick="clearRoll()" id="ta_rclear">Clear</button>
        <button class="ta-btn ta-btn-save" onclick="exportMidi()" id="ta_rmidi">⬇ MIDI</button>
      </div>
    </div>
    <div class="roll-toolbar">
      <button class="rt-btn active" id="rt-pencil" onclick="setRollTool('pencil',this)">✏️</button>
      <button class="rt-btn" id="rt-select" onclick="setRollTool('select',this)">↖</button>
      <button class="rt-btn" id="rt-erase" onclick="setRollTool('erase',this)">🧹</button>
      <div class="rt-divider"></div>
      <label class="rt-lbl" id="rl_vel">Velocity</label>
      <input type="range" id="velSl" min="1" max="127" value="100" class="rt-slider" oninput="document.getElementById('velVal').textContent=this.value">
      <span class="rt-val" id="velVal">100</span>
      <div class="rt-divider"></div>
      <label class="rt-lbl" id="rl_notecolor">Color</label>
      <div class="note-colors" id="noteColors"></div>
    </div>
    <div class="roll-wrap">
      <div class="keys-col" id="pianoKeysCol"></div>
      <div class="roll-scroll" id="rollScroll">
        <div class="roll-beat-header" id="rollBeatHeader"></div>
        <div class="roll-grid-container" id="rollGridContainer"></div>
      </div>
    </div>
    <div class="vel-lane">
      <div class="vel-lane-label" id="rl_vellane">Vel</div>
      <div class="vel-lane-bars" id="velLaneBars"></div>
    </div>
  </div>

  <!-- ── DRUM MACHINE ── -->
  <div class="tab-view" id="tab-drum">
    <div class="tab-header">
      <h2 class="tab-title" id="th_drum">드럼 머신</h2>
      <div class="tab-actions">
        <select class="ta-sel" id="drumPatternSel">
          <option>Pattern A</option><option>Pattern B</option>
          <option>Pattern C</option><option>Pattern D</option>
        </select>
        <select class="ta-sel" id="drumStepsSel" onchange="rebuildDrum()">
          <option value="16" selected>16 Steps</option>
          <option value="32">32 Steps</option>
        </select>
        <button class="ta-btn" onclick="loadDrumPreset('basic')" id="ta_dp1">Basic</button>
        <button class="ta-btn" onclick="loadDrumPreset('hiphop')" id="ta_dp2">Hip-Hop</button>
        <button class="ta-btn" onclick="loadDrumPreset('bossa')" id="ta_dp3">Bossa Nova</button>
        <button class="ta-btn" onclick="loadDrumPreset('dnb')" id="ta_dp4">D&amp;B</button>
        <div class="ta-divider"></div>
        <!-- 비트 미리듣기 토글 -->
        <div class="preview-toggle-wrap">
          <span class="pt-label" id="ta_previewlbl">비트 미리듣기</span>
          <label class="toggle-switch">
            <input type="checkbox" id="drumPreviewToggle" checked onchange="drumPreviewEnabled=this.checked">
            <span class="toggle-track"><span class="toggle-thumb"></span></span>
          </label>
        </div>
        <div class="ta-divider"></div>
        <button class="ta-btn ta-btn-play" onclick="playDrum()" id="ta_dplay">▶ Play</button>
        <button class="ta-btn ta-btn-danger" onclick="clearDrum()" id="ta_dclear">Clear</button>
        <button class="ta-btn ta-btn-save" onclick="exportDrumMidi()" id="ta_dmidi">⬇ MIDI</button>
      </div>
    </div>
    <div class="drum-main">
      <div class="drum-beat-nums" id="drumBeatNums"></div>
      <div class="drum-rows" id="drumRows"></div>
    </div>
    <div class="drum-footer">
      <div class="df-ctrl">
        <label class="df-lbl" id="dl_swing">Swing</label>
        <input type="range" min="0" max="80" value="0" id="swingSl" class="rt-slider" oninput="swingAmt=+this.value;document.getElementById('swingVal').textContent=this.value+'%'">
        <span class="rt-val" id="swingVal">0%</span>
      </div>
      <div class="df-ctrl">
        <label class="df-lbl" id="dl_humanize">Humanize</label>
        <input type="range" min="0" max="30" value="0" id="humanizeSl" class="rt-slider" oninput="humanizeAmt=+this.value;document.getElementById('humanizeVal').textContent=this.value+'ms'">
        <span class="rt-val" id="humanizeVal">0ms</span>
      </div>
      <div class="df-ctrl">
        <label class="df-lbl" id="dl_vol">Drum Vol</label>
        <input type="range" min="0" max="100" value="80" id="drumVolSl" class="rt-slider" oninput="drumMasterVol=+this.value/100;document.getElementById('drumVolVal').textContent=this.value">
        <span class="rt-val" id="drumVolVal">80</span>
      </div>
    </div>
  </div>

  <!-- ── CHORD BUILDER ── -->
  <div class="tab-view" id="tab-chord">
    <div class="tab-header">
      <h2 class="tab-title" id="th_chord">코드 빌더</h2>
      <div class="tab-actions">
        <button class="ta-btn ta-btn-play" onclick="playChords()" id="ta_cplay">▶ Play</button>
        <button class="ta-btn ta-btn-save" onclick="exportChordsTxt()" id="ta_cexport">⬇ Export</button>
        <button class="ta-btn ta-btn-danger" onclick="clearChords()" id="ta_cclear">Clear</button>
      </div>
    </div>
    <div class="chord-layout">
      <div class="chord-left-panel">
        <div class="cp-section">
          <div class="cp-label" id="cl_key">KEY</div>
          <div class="key-btn-grid" id="keyBtnGrid"></div>
        </div>
        <div class="cp-section">
          <div class="cp-label" id="cl_mode">MODE / SCALE</div>
          <div class="mode-btn-grid" id="modeBtnGrid"></div>
        </div>
        <div class="cp-section">
          <div class="cp-label" id="cl_ext">EXTENSION</div>
          <div class="ext-btn-row" id="extBtnRow"></div>
        </div>
        <div class="cp-section">
          <div class="cp-label" id="cl_inv">INVERSION</div>
          <div class="ext-btn-row">
            <button class="ext-btn active" id="inv0" onclick="setInversion(0,this)">Root</button>
            <button class="ext-btn" id="inv1" onclick="setInversion(1,this)">1st</button>
            <button class="ext-btn" id="inv2" onclick="setInversion(2,this)">2nd</button>
          </div>
        </div>
        <div class="cp-section">
          <div class="cp-label" id="cl_presets">PRESETS</div>
          <div class="preset-btn-grid" id="presetBtnGrid"></div>
        </div>
      </div>
      <div class="chord-right-panel">
        <div class="cp-section">
          <div class="cp-label" id="cl_diatonic">DIATONIC CHORDS</div>
          <div class="diatonic-grid" id="diatonicGrid"></div>
        </div>
        <div class="cp-section">
          <div class="cp-label" id="cl_prog">PROGRESSION</div>
          <div class="progression-row" id="progressionRow">
            <span class="prog-empty-hint" id="prog_hint">코드를 클릭해서 추가하세요</span>
          </div>
          <div class="prog-info" id="progInfo"></div>
          <div class="prog-actions">
            <button class="ta-btn" onclick="shuffleProg()" id="ta_shuffle">🔀 Shuffle</button>
            <button class="ta-btn" onclick="reverseProg()" id="ta_reverse">↩ Reverse</button>
          </div>
        </div>
        <div class="cp-section">
          <div class="cp-label" id="cl_keyboard">KEYBOARD PREVIEW</div>
          <div class="mini-kb" id="miniKb"></div>
          <div class="mini-kb-info" id="miniKbInfo"></div>
        </div>
      </div>
    </div>
  </div>

  <!-- ── MELODY SKETCH ── -->
  <div class="tab-view" id="tab-melody">
    <div class="tab-header">
      <h2 class="tab-title" id="th_melody">멜로디 스케치</h2>
      <div class="tab-actions">
        <button class="mel-tool-btn active" id="mt-draw" onclick="setMelTool('draw',this)">✏️ <span id="mt_draw">그리기</span></button>
        <button class="mel-tool-btn" id="mt-erase" onclick="setMelTool('erase',this)">🧹 <span id="mt_erase">지우기</span></button>
        <button class="mel-tool-btn" id="mt-sel" onclick="setMelTool('select',this)">↖ <span id="mt_select">선택</span></button>
        <div class="ta-divider"></div>
        <select class="ta-sel" id="melScaleSel" onchange="updateMelCanvas()">
          <option value="major">Major</option><option value="minor">Minor</option>
          <option value="pentatonic">Pentatonic</option><option value="blues">Blues</option>
          <option value="chromatic">Chromatic</option>
        </select>
        <select class="ta-sel" id="melKeySel" onchange="updateMelCanvas()">
          <option>C</option><option>C#</option><option>D</option><option>D#</option>
          <option>E</option><option>F</option><option>F#</option><option>G</option>
          <option>G#</option><option>A</option><option>A#</option><option>B</option>
        </select>
        <div class="mel-color-dots" id="melColorDots"></div>
        <div class="ta-divider"></div>
        <button class="ta-btn ta-btn-play" onclick="playMelody()" id="ta_mplay">▶ Play</button>
        <button class="ta-btn ta-btn-save" onclick="exportMelodyMidi()" id="ta_mmidi">⬇ MIDI</button>
        <button class="ta-btn ta-btn-danger" onclick="clearMelody()" id="ta_mclear">Clear</button>
      </div>
    </div>
    <div class="melody-area">
      <div class="mel-pitch-col" id="melPitchCol"></div>
      <div class="mel-canvas-col">
        <div class="mel-ruler-row" id="melRulerRow"></div>
        <canvas id="melCanvas"></canvas>
      </div>
    </div>
    <div class="mel-statusbar">
      <span id="melNoteInfo" class="msb-info"></span>
      <span class="msb-hint" id="msb_hint">✏️ Click · Drag to extend · Right-click remove</span>
      <span id="melNoteCount" class="msb-count"></span>
    </div>
  </div>

  <!-- ── MIXER ── -->
  <div class="tab-view" id="tab-mixer">
    <div class="tab-header">
      <h2 class="tab-title" id="th_mixer">믹서</h2>
      <div class="tab-actions">
        <button class="ta-btn" onclick="resetAllMixer()" id="ta_mixreset">Reset All</button>
        <button class="ta-btn ta-btn-save" onclick="exportMixdown()" id="ta_mixdown">⬇ Mixdown</button>
      </div>
    </div>
    <div class="mixer-area" id="mixerArea">
      <div class="empty-state">
        <div class="es-icon">🎚</div>
        <div class="es-text" id="es_mixer">Arrange에서 트랙을 추가하세요</div>
      </div>
    </div>
  </div>

  <!-- ── ARPEGGIATOR ── -->
  <div class="tab-view" id="tab-arp">
    <div class="tab-header">
      <h2 class="tab-title" id="th_arp">아르페지에이터</h2>
      <div class="tab-actions">
        <button class="ta-btn ta-btn-play" id="ta_arpplay" onclick="toggleArp()">▶ Start</button>
      </div>
    </div>
    <div class="arp-body" id="arpBody"></div>
  </div>

  <!-- ── SCALE HELPER ── -->
  <div class="tab-view" id="tab-scale">
    <div class="tab-header">
      <h2 class="tab-title" id="th_scale">스케일 헬퍼</h2>
    </div>
    <div class="scale-body" id="scaleBody"></div>
  </div>

  <!-- ── EQ ── -->
  <div class="tab-view" id="tab-eq">
    <div class="tab-header">
      <h2 class="tab-title" id="th_eq">이퀄라이저</h2>
      <div class="tab-actions">
        <button class="ta-btn" onclick="resetEq()" id="ta_eqreset">Reset</button>
        <button class="ta-btn ta-btn-play" onclick="previewEq()" id="ta_eqpreview">Preview</button>
      </div>
    </div>
    <div class="eq-body" id="eqBody"></div>
  </div>

  <!-- ── PROJECTS ── -->
  <div class="tab-view" id="tab-projects">
    <div class="tab-header">
      <h2 class="tab-title" id="th_projects">내 프로젝트</h2>
      <div class="tab-actions">
        <button class="ta-btn ta-btn-play" onclick="saveProject()" id="ta_saveproj">💾 저장</button>
        <button class="ta-btn" onclick="newProject()" id="ta_newproj">+ 새 프로젝트</button>
        <button class="ta-btn ta-btn-save" onclick="exportProject()" id="ta_exportproj">⬇ JSON 내보내기</button>
        <button class="ta-btn" onclick="document.getElementById('importFileInp').click()" id="ta_importproj">⬆ 가져오기</button>
        <input type="file" id="importFileInp" accept=".json" style="display:none" onchange="importProject(this)">
      </div>
    </div>
    <div class="projects-body" id="projectsBody"></div>
  </div>

  <!-- ── LOOPS ── -->
  <div class="tab-view" id="tab-loops">
    <div class="tab-header">
      <h2 class="tab-title" id="th_loops">루프 라이브러리</h2>
    </div>
    <div class="loops-body" id="loopsBody"></div>
  </div>

  <!-- ── TUTORIAL ── -->
  <div class="tab-view" id="tab-tutorial">
    <div class="tab-header"><h2 class="tab-title" id="th_tutorial">튜토리얼</h2></div>
    <div class="tutorial-body" id="tutorialBody"></div>
  </div>

  <!-- ── GLOSSARY ── -->
  <div class="tab-view" id="tab-glossary">
    <div class="tab-header"><h2 class="tab-title" id="th_glossary">용어사전</h2></div>
    <div class="glossary-body">
      <input class="glossary-search" id="glossarySearch" placeholder="검색..." oninput="renderGlossary(this.value)">
      <div id="glossaryList"></div>
    </div>
  </div>

</main>

<!-- INSPECTOR -->
<aside class="inspector" id="inspector">
  <div class="ins-title" id="ins_title">인스펙터</div>
  <div id="insBody"><p class="ins-empty" id="ins_empty">항목을 선택하세요</p></div>
</aside>

</div><!-- /app-body -->

<!-- STATUS BAR -->
<footer class="statusbar">
  <div class="sb-l"><div class="sb-dot" id="sbDot"></div><span id="sbTxt">Ready</span></div>
  <div class="sb-c" id="sbProject">새 프로젝트</div>
  <div class="sb-r"><span id="sbClock"></span><span>MeloMelo Studio v3.0</span></div>
</footer>

</div><!-- /appRoot -->

<!-- ═══ MODALS ═══ -->

<!-- Save/Load Modal -->
<div class="modal-overlay hidden" id="saveModal">
  <div class="modal-card">
    <div class="modal-title" id="sm_title">💾 저장 / 불러오기</div>
    <div class="modal-tabs">
      <button class="mtab active" onclick="switchModalTab('save',this)" id="mt_save">저장</button>
      <button class="mtab" onclick="switchModalTab('load',this)" id="mt_load">불러오기</button>
    </div>
    <div id="saveTab" class="modal-body">
      <div class="modal-field">
        <label class="mf-lbl" id="mf_name">프로젝트 이름</label>
        <input type="text" id="saveProjName" class="mf-inp" placeholder="My Project">
      </div>
      <div class="modal-actions">
        <button class="ma-btn ma-btn-primary" onclick="doSaveNew()" id="ma_savenew">새로 저장</button>
        <button class="ma-btn" onclick="doOverwrite()" id="ma_overwrite">현재 덮어쓰기</button>
        <button class="ma-btn ta-btn-save" onclick="exportProject()" id="ma_export">JSON 내보내기</button>
      </div>
    </div>
    <div id="loadTab" class="modal-body hidden">
      <div class="saved-project-list" id="savedProjectList"></div>
      <div class="modal-actions" style="margin-top:12px">
        <button class="ma-btn" onclick="document.getElementById('importFileInp').click()" id="ma_import">⬆ JSON 가져오기</button>
      </div>
    </div>
    <button class="modal-close" onclick="closeModal('saveModal')">✕</button>
  </div>
</div>

<!-- Settings Modal -->
<div class="modal-overlay hidden" id="settingsModal">
  <div class="modal-card">
    <div class="modal-title">⚙️ <span id="sets_title">설정</span></div>
    <div class="modal-body">
      <div class="sets-section">
        <div class="sets-label" id="sets_security">🔒 보안</div>
        <div class="sets-row">
          <span class="sets-key" id="sets_changepwd">비밀번호 변경</span>
        </div>
        <div class="modal-field">
          <label class="mf-lbl" id="mf_curpwd">현재 비밀번호</label>
          <div class="lf-input-wrap">
            <input type="password" id="curPwd" class="mf-inp" placeholder="현재 비밀번호">
            <button class="lf-eye" onclick="togglePwdVis('curPwd',this)">👁</button>
          </div>
        </div>
        <div class="modal-field">
          <label class="mf-lbl" id="mf_newpwd">새 비밀번호</label>
          <div class="lf-input-wrap">
            <input type="password" id="newPwd" class="mf-inp" placeholder="새 비밀번호 (최소 4자)">
            <button class="lf-eye" onclick="togglePwdVis('newPwd',this)">👁</button>
          </div>
        </div>
        <div class="modal-field">
          <label class="mf-lbl" id="mf_cfmpwd">비밀번호 확인</label>
          <div class="lf-input-wrap">
            <input type="password" id="cfmPwd" class="mf-inp" placeholder="비밀번호 재입력">
            <button class="lf-eye" onclick="togglePwdVis('cfmPwd',this)">👁</button>
          </div>
        </div>
        <div id="pwdChangeMsg" class="sets-msg"></div>
        <button class="ma-btn ma-btn-primary" onclick="changePassword()" id="sets_applypwd">비밀번호 변경</button>
      </div>

      <div class="sets-section">
        <div class="sets-label" id="sets_audio">🔊 오디오</div>
        <div class="sets-row">
          <span class="sets-key" id="sets_latency">레이턴시 모드</span>
          <select class="ta-sel" id="latencyMode"><option>Low</option><option selected>Normal</option><option>High Quality</option></select>
        </div>
        <div class="sets-row">
          <span class="sets-key" id="sets_tuning">기준음 (Hz)</span>
          <input type="number" id="tuningHz" value="440" min="430" max="450" class="mf-inp" style="width:80px">
        </div>
      </div>

      <div class="sets-section">
        <div class="sets-label" id="sets_display">🎨 디스플레이</div>
        <div class="sets-row">
          <span class="sets-key" id="sets_theme">테마</span>
          <select class="ta-sel" id="themeSelect" onchange="applyTheme(this.value)">
            <option value="light">라이트</option>
            <option value="dark">다크</option>
            <option value="pink">핑크</option>
            <option value="mint">민트</option>
          </select>
        </div>
        <div class="sets-row">
          <span class="sets-key" id="sets_lang">언어</span>
          <select class="ta-sel" id="langSelect" onchange="setLangFromSelect(this.value)">
            <option value="ko">한국어</option>
            <option value="en">English</option>
          </select>
        </div>
      </div>

      <div class="sets-section">
        <div class="sets-label" id="sets_data">📦 데이터</div>
        <div class="sets-row">
          <button class="ma-btn" onclick="clearAllData()" id="sets_cleardata">모든 데이터 초기화</button>
        </div>
      </div>
    </div>
    <button class="modal-close" onclick="closeModal('settingsModal')">✕</button>
  </div>
</div>

<!-- Toast notification -->
<div class="toast hidden" id="toast"></div>

<script src="app.js"></script>
</body>
</html>
/* ══════════════════════════════════════════
   MeloMelo Studio v3 — Pastel Pro DAW
   ══════════════════════════════════════════ */
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600;700&family=Syne:wght@400;600;700;800&family=DM+Mono:wght@400;500&display=swap');

:root {
  --c-bg:   #fdf8f5; --c-bg1:  #fff9f7; --c-bg2:  #f5eff4; --c-bg3:  #ede4ec;
  --c-pk:   #f7c5d5; --c-pk2:  #f0a0be; --c-pk3:  #d4749a;
  --c-lv:   #ddd0f8; --c-lv2:  #baaff0; --c-lv3:  #8b6dd4;
  --c-mn:   #c0e4f8; --c-mn2:  #85c8f0; --c-mn3:  #4a9ed8;
  --c-yw:   #fdeea0; --c-yw2:  #f8d560; --c-yw3:  #c8a018;
  --c-gn:   #b8f0d0; --c-gn2:  #7cd8a0; --c-gn3:  #38a868;
  --c-co:   #ffd8a8; --c-co2:  #ffb060; --c-co3:  #c86818;
  --c-txt:  #2e2440; --c-txt2: #6e6080; --c-txt3: #a898b8;
  --c-brd:  #e4d8f0; --c-brd2: #d0c0e4;
  --c-wh:   #ffffff;
  --nav-h:  54px; --sb-h: 26px; --sidebar-w: 196px; --ins-w: 210px;
  --r:  8px; --r2: 14px;
  --sh: 0 2px 12px rgba(100,80,150,.09);
  --sh2:0 8px 30px rgba(100,80,150,.14);
  --f-ui: 'DM Sans',sans-serif;
  --f-hd: 'Syne',sans-serif;
  --f-mo: 'DM Mono',monospace;
}

/* DARK */
.dark { --c-bg:#18131e; --c-bg1:#1e182a; --c-bg2:#261e36; --c-bg3:#30264a;
  --c-txt:#ede8f8; --c-txt2:#a898c8; --c-txt3:#6858a0;
  --c-brd:#332650; --c-brd2:#4a3870; --c-wh:#201830; }

/* PINK */
.pink { --c-bg:#fff0f4; --c-bg1:#fff5f8; --c-bg2:#fde8ee; --c-bg3:#f8d0dc;
  --c-lv3:#d0507a; --c-lv2:#f0a0be; --c-lv:#fdd0df; }

/* MINT */
.mint { --c-bg:#f0faf8; --c-bg1:#f5fdfb; --c-bg2:#e0f5f0; --c-bg3:#c8ece4;
  --c-lv3:#2a9870; --c-lv2:#6acca8; --c-lv:#b0e8d8; }

*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%;overflow:hidden}
body{font-family:var(--f-ui);background:var(--c-bg);color:var(--c-txt);font-size:13px;transition:background .3s,color .3s}

/* ── LOGIN ── */
.screen-overlay{position:fixed;inset:0;z-index:9000;display:flex;align-items:center;justify-content:center;overflow:hidden}
.login-bg-circles{position:absolute;inset:0;pointer-events:none}
.lbc{position:absolute;border-radius:50%;filter:blur(60px);opacity:.5}
.lbc1{width:500px;height:500px;background:radial-gradient(circle,#d8ccf5,transparent);top:-100px;left:-100px}
.lbc2{width:400px;height:400px;background:radial-gradient(circle,#f7c5d5,transparent);bottom:-80px;right:-80px}
.lbc3{width:350px;height:350px;background:radial-gradient(circle,#b8dff7,transparent);top:50%;left:50%;transform:translate(-50%,-50%)}
.login-card{position:relative;z-index:1;background:rgba(255,255,255,.72);backdrop-filter:blur(24px);border:1.5px solid rgba(255,255,255,.9);border-radius:24px;padding:48px 52px;text-align:center;box-shadow:0 24px 80px rgba(120,90,200,.14);min-width:360px}
.login-logo{font-family:var(--f-hd);font-size:42px;font-weight:800;color:var(--c-lv3);line-height:1}
.login-logo span{display:block;font-size:18px;font-weight:400;color:var(--c-pk3);letter-spacing:5px;margin-top:4px}
.login-sub{font-family:var(--f-mo);font-size:11px;color:var(--c-txt2);letter-spacing:3px;margin:12px 0 28px;text-transform:uppercase}
.login-form{display:flex;flex-direction:column;gap:10px}
.login-field{text-align:left}
.lf-label{font-size:11px;font-weight:600;color:var(--c-txt2);display:block;margin-bottom:5px;font-family:var(--f-mo);letter-spacing:1px}
.lf-input-wrap{position:relative;display:flex}
.lf-input{width:100%;padding:10px 40px 10px 14px;border:1.5px solid var(--c-brd2);border-radius:var(--r);font-family:var(--f-ui);font-size:15px;background:rgba(255,255,255,.7);color:var(--c-txt);transition:.2s}
.lf-input:focus{outline:none;border-color:var(--c-lv3);box-shadow:0 0 0 3px rgba(139,109,212,.15)}
.lf-eye{position:absolute;right:10px;top:50%;transform:translateY(-50%);background:none;border:none;cursor:pointer;font-size:14px;color:var(--c-txt3)}
.login-error{color:var(--c-pk3);font-size:12px;min-height:16px;font-weight:600}
.login-btn{margin-top:4px;padding:13px;border-radius:var(--r);background:linear-gradient(135deg,var(--c-lv3),var(--c-pk3));color:#fff;font-family:var(--f-hd);font-size:15px;font-weight:700;border:none;cursor:pointer;transition:.2s;letter-spacing:.5px;box-shadow:0 4px 18px rgba(139,109,212,.35)}
.login-btn:hover{transform:translateY(-1px);box-shadow:0 8px 26px rgba(139,109,212,.45)}
.login-lang{display:flex;gap:8px;justify-content:center;margin-top:18px}
.ll-btn{padding:5px 16px;border-radius:16px;border:1.5px solid var(--c-brd2);background:transparent;font-size:12px;font-weight:600;color:var(--c-txt2);cursor:pointer;transition:.15s;font-family:var(--f-ui)}
.ll-btn.active,.ll-btn:hover{background:var(--c-lv);border-color:var(--c-lv3);color:var(--c-lv3)}
.login-hint{margin-top:12px;font-size:11px;color:var(--c-txt3);font-family:var(--f-mo)}

.fade-out{animation:fadeOut .4s forwards}
@keyframes fadeOut{to{opacity:0;transform:scale(1.04);pointer-events:none}}
.hidden{display:none!important}
.screen-overlay.hidden{display:none!important}

/* ── LAYOUT ── */
#appRoot{display:flex;flex-direction:column;height:100vh}
.topnav{height:var(--nav-h);background:var(--c-bg1);border-bottom:1px solid var(--c-brd);display:flex;align-items:center;gap:12px;padding:0 14px;flex-shrink:0;box-shadow:var(--sh)}
.tn-logo{font-family:var(--f-hd);font-size:17px;font-weight:800;color:var(--c-lv3);white-space:nowrap;letter-spacing:-.5px}
.tn-transport{display:flex;align-items:center;gap:8px;flex:1;justify-content:center}
.tn-right{display:flex;align-items:center;gap:8px}
.app-body{display:flex;flex:1;overflow:hidden;height:calc(100vh - var(--nav-h) - var(--sb-h))}

/* ── TRANSPORT ── */
.tb{width:30px;height:30px;border-radius:50%;border:1.5px solid var(--c-brd);background:var(--c-bg2);color:var(--c-txt2);display:flex;align-items:center;justify-content:center;cursor:pointer;font-size:11px;transition:.14s}
.tb:hover{background:var(--c-lv);border-color:var(--c-lv3);color:var(--c-lv3)}
.tb-play{background:var(--c-gn);border-color:var(--c-gn3);color:var(--c-gn3)}
.tb-play.active{background:var(--c-gn3);color:#fff;animation:pulse-gn .7s infinite}
.tb-rec{background:var(--c-pk);border-color:var(--c-pk3)}
.tb-rec.active{background:var(--c-pk3);color:#fff;animation:pulse-pk .7s infinite}
.tb-sep{width:1px;height:20px;background:var(--c-brd);margin:0 2px}
@keyframes pulse-gn{0%,100%{box-shadow:0 0 0 0 rgba(56,168,104,.5)}50%{box-shadow:0 0 0 5px rgba(56,168,104,0)}}
@keyframes pulse-pk{0%,100%{box-shadow:0 0 0 0 rgba(212,116,154,.5)}50%{box-shadow:0 0 0 5px rgba(212,116,154,0)}}

.bpm-wrap{display:flex;align-items:center;gap:4px}
.bpm-lbl{font-family:var(--f-mo);font-size:9px;color:var(--c-txt3);letter-spacing:1px}
.bpm-inp{width:54px;text-align:center;border:1.5px solid var(--c-brd);border-radius:6px;padding:4px;font-family:var(--f-mo);font-size:14px;font-weight:500;background:var(--c-bg2);color:var(--c-txt)}
.bpm-inp:focus{outline:none;border-color:var(--c-lv3)}
.tap-btn{padding:4px 8px;border-radius:6px;background:var(--c-yw);border:1.5px solid var(--c-yw3);color:var(--c-yw3);font-size:10px;font-weight:700;cursor:pointer;transition:.12s;letter-spacing:1px;font-family:var(--f-ui)}
.tap-btn:active,.tap-btn.flash{background:var(--c-yw3);color:#fff}
.tn-sel,.ta-sel,.mf-inp{border:1.5px solid var(--c-brd);border-radius:6px;padding:4px 8px;font-family:var(--f-ui);font-size:12px;background:var(--c-bg2);color:var(--c-txt);cursor:pointer}
.tn-sel:focus,.ta-sel:focus,.mf-inp:focus{outline:none;border-color:var(--c-lv3)}
.pos-display{font-family:var(--f-mo);font-size:12px;color:var(--c-lv3);background:var(--c-bg2);border:1.5px solid var(--c-brd);border-radius:6px;padding:4px 10px;min-width:96px;text-align:center}
.master-vol-wrap{display:flex;align-items:center;gap:4px}
.mv-lbl{font-size:9px;color:var(--c-txt3);font-family:var(--f-mo);letter-spacing:1px}
.mv-sl{width:64px;accent-color:var(--c-lv3)}
.mv-val{font-size:10px;font-family:var(--f-mo);color:var(--c-lv3);min-width:22px}
.tn-icon-btn{width:30px;height:30px;border-radius:7px;border:1.5px solid var(--c-brd);background:var(--c-bg2);cursor:pointer;font-size:14px;display:flex;align-items:center;justify-content:center;transition:.14s;color:var(--c-txt2)}
.tn-icon-btn:hover{background:var(--c-lv);border-color:var(--c-lv3)}

/* ── SIDEBAR ── */
.sidebar{width:var(--sidebar-w);flex-shrink:0;background:var(--c-bg1);border-right:1px solid var(--c-brd);overflow-y:auto;padding:8px 0}
.sidebar::-webkit-scrollbar{width:4px}
.sidebar::-webkit-scrollbar-thumb{background:var(--c-brd2);border-radius:2px}
.sb-group{margin-bottom:12px}
.sb-group-title{font-family:var(--f-mo);font-size:8.5px;color:var(--c-txt3);letter-spacing:2.5px;text-transform:uppercase;padding:6px 14px 4px}
.sb-item{width:100%;display:flex;align-items:center;gap:9px;padding:8px 14px;background:none;border:none;border-left:2.5px solid transparent;font-family:var(--f-ui);font-size:12.5px;font-weight:500;color:var(--c-txt2);cursor:pointer;transition:.14s;text-align:left}
.sb-item:hover{background:var(--c-bg2);color:var(--c-txt)}
.sb-item.active{background:var(--c-lv);color:var(--c-lv3);border-left-color:var(--c-lv3)}
.sbi-ico{width:18px;text-align:center;font-size:14px}

/* ── MAIN / TABS ── */
.main-area{flex:1;overflow:hidden;position:relative;background:var(--c-bg)}
.tab-view{display:none;flex-direction:column;height:100%;overflow-y:auto}
.tab-view.active{display:flex}
.tab-view::-webkit-scrollbar{width:6px}
.tab-view::-webkit-scrollbar-thumb{background:var(--c-brd2);border-radius:3px}
.tab-header{display:flex;align-items:center;gap:8px;padding:10px 16px;border-bottom:1px solid var(--c-brd);background:var(--c-bg1);position:sticky;top:0;z-index:20;flex-shrink:0}
.tab-title{font-family:var(--f-hd);font-size:17px;font-weight:700;color:var(--c-txt);margin-right:auto}
.tab-actions{display:flex;align-items:center;gap:5px;flex-wrap:wrap}
.ta-btn{padding:5px 11px;border-radius:7px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-family:var(--f-ui);font-size:11.5px;font-weight:600;color:var(--c-txt2);cursor:pointer;transition:.14s;white-space:nowrap}
.ta-btn:hover{background:var(--c-lv);border-color:var(--c-lv3);color:var(--c-lv3)}
.ta-btn-play{background:var(--c-gn);border-color:var(--c-gn3);color:var(--c-gn3)}
.ta-btn-play:hover{background:var(--c-gn3);color:#fff;border-color:var(--c-gn3)}
.ta-btn-danger{background:var(--c-pk);border-color:var(--c-pk3);color:var(--c-pk3)}
.ta-btn-danger:hover{background:var(--c-pk3);color:#fff}
.ta-btn-save{background:var(--c-mn);border-color:var(--c-mn3);color:var(--c-mn3)}
.ta-btn-save:hover{background:var(--c-mn3);color:#fff}
.ta-btn-outline{background:transparent;border-color:var(--c-brd2)}
.ta-divider{width:1px;height:20px;background:var(--c-brd);margin:0 2px}
.empty-state{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:10px;color:var(--c-txt3);padding:60px}
.es-icon{font-size:44px;opacity:.4}
.es-text{font-size:14px}

/* ── PREVIEW TOGGLE ── */
.preview-toggle-wrap{display:flex;align-items:center;gap:6px}
.pt-label{font-size:11.5px;font-weight:600;color:var(--c-txt2)}
.toggle-switch{position:relative;display:inline-flex;align-items:center;cursor:pointer}
.toggle-switch input{opacity:0;width:0;height:0;position:absolute}
.toggle-track{width:38px;height:20px;background:var(--c-brd2);border-radius:20px;transition:.2s;position:relative;display:block}
.toggle-switch input:checked + .toggle-track{background:var(--c-gn3)}
.toggle-thumb{position:absolute;top:2px;left:2px;width:16px;height:16px;border-radius:50%;background:#fff;transition:.2s;box-shadow:0 1px 4px rgba(0,0,0,.2)}
.toggle-switch input:checked + .toggle-track .toggle-thumb{transform:translateX(18px)}

/* ── ARRANGE ── */
.ruler-strip{height:26px;background:var(--c-bg2);border-bottom:1px solid var(--c-brd);display:flex;align-items:center;padding-left:172px;overflow:hidden;flex-shrink:0}
.ruler-mark{display:inline-flex;align-items:center;min-width:58px;height:100%;font-family:var(--f-mo);font-size:9px;color:var(--c-txt3);border-right:.5px solid var(--c-brd)}
.ruler-mark.bar{color:var(--c-lv3);font-weight:700;border-right-color:var(--c-lv2)}
.track-area{flex:1;overflow-y:auto}
.track-row{display:flex;min-height:50px;border-bottom:1px solid var(--c-brd);transition:background .12s}
.track-row:hover{background:var(--c-bg2)}
.track-hd{width:170px;flex-shrink:0;padding:6px 10px;border-right:1px solid var(--c-brd);display:flex;flex-direction:column;gap:4px;justify-content:center}
.track-colorbar{width:4px;flex-shrink:0;border-right:1px solid var(--c-brd)}
.track-name{font-size:12px;font-weight:600;color:var(--c-txt);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.track-ctls{display:flex;gap:3px;align-items:center}
.tc{width:20px;height:20px;border-radius:4px;border:1px solid var(--c-brd);background:var(--c-bg2);font-size:9px;cursor:pointer;display:flex;align-items:center;justify-content:center;transition:.12s;color:var(--c-txt2)}
.tc:hover{background:var(--c-lv);color:var(--c-lv3)}
.tc.muted{background:var(--c-yw2);color:var(--c-yw3);border-color:var(--c-yw3)}
.tc.soloed{background:var(--c-gn2);color:var(--c-gn3);border-color:var(--c-gn3)}
.track-clips{flex:1;display:flex;align-items:center;padding:4px 6px;gap:4px;overflow-x:auto}
.clip{height:38px;min-width:56px;border-radius:6px;border:1.5px solid transparent;display:flex;align-items:center;padding:4px 8px;cursor:pointer;transition:.14s;font-size:10px;font-weight:600;white-space:nowrap}
.clip:hover{transform:translateY(-1px);box-shadow:var(--sh)}
.clip-add{background:transparent;border:1.5px dashed var(--c-brd2);color:var(--c-txt3);cursor:pointer}

/* ── PIANO ROLL ── */
.roll-toolbar{display:flex;align-items:center;gap:6px;padding:6px 12px;background:var(--c-bg1);border-bottom:1px solid var(--c-brd);flex-shrink:0;flex-wrap:wrap}
.rt-btn{width:28px;height:28px;border-radius:6px;border:1.5px solid var(--c-brd);background:var(--c-bg2);display:flex;align-items:center;justify-content:center;font-size:13px;cursor:pointer;transition:.12s}
.rt-btn:hover{background:var(--c-lv);border-color:var(--c-lv3)}
.rt-btn.active{background:var(--c-lv2);border-color:var(--c-lv3)}
.rt-divider{width:1px;height:20px;background:var(--c-brd);margin:0 3px}
.rt-lbl{font-size:10.5px;color:var(--c-txt2);font-family:var(--f-mo)}
.rt-slider{width:80px;accent-color:var(--c-lv3)}
.rt-val{font-size:10.5px;font-family:var(--f-mo);color:var(--c-lv3);min-width:28px}
.note-colors{display:flex;gap:4px}
.nc-dot{width:18px;height:18px;border-radius:50%;cursor:pointer;border:2px solid transparent;transition:.12s}
.nc-dot.active,.nc-dot:hover{border-color:var(--c-txt);transform:scale(1.15)}

.roll-wrap{display:flex;flex:1;overflow:hidden;min-height:0}
.keys-col{width:54px;flex-shrink:0;background:var(--c-bg1);border-right:1px solid var(--c-brd);overflow:hidden;display:flex;flex-direction:column}
.pk{height:20px;flex-shrink:0;display:flex;align-items:center;justify-content:flex-end;padding-right:5px;font-size:8.5px;font-family:var(--f-mo);border-bottom:.5px solid rgba(200,180,230,.3);cursor:pointer;transition:.1s}
.pk.white{background:var(--c-wh);color:var(--c-txt3)}
.pk.black{background:#2e2340;color:#a098b8}
.pk.cn{color:var(--c-lv3);font-weight:600}
.pk.white:hover{background:var(--c-lv)}
.pk.black:hover{background:var(--c-lv3)}
.roll-scroll{flex:1;overflow:auto;position:relative}
.roll-beat-header{height:20px;background:var(--c-bg2);border-bottom:1px solid var(--c-brd);display:flex;position:sticky;top:0;z-index:5}
.rbh-cell{border-right:.5px solid rgba(200,180,230,.4);display:flex;align-items:center;justify-content:center;font-size:8.5px;font-family:var(--f-mo);color:var(--c-txt3);flex-shrink:0}
.rbh-cell.bar{border-right:1.5px solid var(--c-lv2);color:var(--c-lv3);font-weight:600}
.roll-grid-container{position:relative}
.rg-row{display:flex;height:20px;position:absolute;width:100%}
.rg-row.blk{background:rgba(180,150,230,.05)}
.rg-cell{flex-shrink:0;height:20px;border-right:.5px solid rgba(200,180,230,.2);border-bottom:.5px solid rgba(200,180,230,.15);cursor:pointer;transition:.06s}
.rg-cell:hover{background:rgba(180,150,230,.18)}
.rg-cell.bs{border-right:1px solid rgba(180,150,230,.45)}
.rg-cell.brs{border-right:1.5px solid rgba(139,109,212,.5)}
.roll-note{position:absolute;border-radius:3px;cursor:pointer;display:flex;align-items:center;padding:0 3px;font-size:8px;font-weight:600;overflow:hidden;white-space:nowrap;transition:filter .1s;border:1px solid transparent}
.roll-note:hover{filter:brightness(1.12)}
.roll-note.selected{outline:2px solid var(--c-pk3)}

.vel-lane{height:68px;flex-shrink:0;display:flex;border-top:1.5px solid var(--c-brd);background:var(--c-bg1);overflow:hidden}
.vel-lane-label{width:54px;flex-shrink:0;font-size:8px;font-family:var(--f-mo);color:var(--c-txt3);display:flex;align-items:center;justify-content:center;border-right:1px solid var(--c-brd);writing-mode:vertical-rl;transform:rotate(180deg);letter-spacing:1px}
.vel-lane-bars{flex:1;display:flex;align-items:flex-end;padding:4px 4px 0;gap:2px;overflow-x:auto}
.vel-bar{flex-shrink:0;min-width:8px;background:var(--c-lv2);border-radius:2px 2px 0 0;cursor:pointer;transition:.1s}
.vel-bar:hover{background:var(--c-lv3)}

/* ── DRUM ── */
.drum-main{flex:1;overflow-y:auto;padding:14px 16px}
.drum-beat-nums{display:flex;margin-left:112px;gap:3px;margin-bottom:4px}
.dbn{width:30px;text-align:center;font-size:8.5px;font-family:var(--f-mo);color:var(--c-txt3);flex-shrink:0}
.dbn.strong{color:var(--c-lv3);font-weight:700}
.drum-rows{display:flex;flex-direction:column;gap:6px}
.drum-row-wrap{display:flex;align-items:center;gap:8px}
.drum-row-label{width:104px;text-align:right;font-size:11.5px;font-weight:600;color:var(--c-txt2);flex-shrink:0}
.drum-row-pads{display:flex;gap:3px}
.dp{width:30px;height:30px;border-radius:7px;border:1.5px solid var(--c-brd);background:var(--c-bg2);cursor:pointer;transition:.1s;flex-shrink:0;position:relative}
.dp:hover{border-color:var(--c-lv2);background:var(--c-bg3)}
.dp.sep{margin-left:5px}
.dp.on-kick  {background:var(--c-pk2);border-color:var(--c-pk3);box-shadow:0 2px 8px rgba(212,116,154,.3)}
.dp.on-snare {background:var(--c-lv2);border-color:var(--c-lv3);box-shadow:0 2px 8px rgba(139,109,212,.3)}
.dp.on-hihat {background:var(--c-mn2);border-color:var(--c-mn3);box-shadow:0 2px 8px rgba(74,150,220,.3)}
.dp.on-clap  {background:var(--c-yw2);border-color:var(--c-yw3);box-shadow:0 2px 8px rgba(200,160,24,.3)}
.dp.on-crash {background:var(--c-gn2);border-color:var(--c-gn3)}
.dp.on-ride  {background:var(--c-co2);border-color:var(--c-co3)}
.dp.on-perc  {background:var(--c-pk); border-color:var(--c-pk2)}
.dp.on-fx    {background:var(--c-mn); border-color:var(--c-mn2)}
.dp.playing  {transform:scale(.88);transition:transform .05s}
.drum-row-vol{display:flex;align-items:center;gap:4px}
.drv{width:56px;accent-color:var(--c-lv3)}
.drum-footer{display:flex;gap:20px;align-items:center;padding:9px 16px;border-top:1px solid var(--c-brd);background:var(--c-bg1);flex-shrink:0;flex-wrap:wrap}
.df-ctrl{display:flex;align-items:center;gap:6px}
.df-lbl{font-size:10.5px;font-family:var(--f-mo);color:var(--c-txt2);letter-spacing:.5px}

/* ── CHORD ── */
.chord-layout{display:flex;flex:1;overflow:hidden}
.chord-left-panel{width:300px;flex-shrink:0;border-right:1px solid var(--c-brd);overflow-y:auto;padding:14px;display:flex;flex-direction:column;gap:16px}
.chord-right-panel{flex:1;overflow-y:auto;padding:14px;display:flex;flex-direction:column;gap:16px}
.cp-section{}
.cp-label{font-family:var(--f-mo);font-size:8.5px;letter-spacing:2.5px;color:var(--c-txt3);text-transform:uppercase;margin-bottom:8px}
.key-btn-grid{display:flex;flex-wrap:wrap;gap:4px}
.kb-btn{padding:5px 9px;border-radius:7px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-family:var(--f-mo);font-size:11.5px;font-weight:500;color:var(--c-txt);cursor:pointer;transition:.14s}
.kb-btn:hover{background:var(--c-lv);border-color:var(--c-lv3);color:var(--c-lv3)}
.kb-btn.active{background:var(--c-lv2);border-color:var(--c-lv3);color:var(--c-lv3);font-weight:700}
.mode-btn-grid{display:flex;flex-wrap:wrap;gap:4px}
.mode-btn{padding:5px 10px;border-radius:7px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-size:11.5px;font-weight:500;color:var(--c-txt2);cursor:pointer;transition:.14s;font-family:var(--f-ui)}
.mode-btn:hover{background:var(--c-mn);border-color:var(--c-mn3);color:var(--c-mn3)}
.mode-btn.active{background:var(--c-mn2);border-color:var(--c-mn3);color:var(--c-mn3);font-weight:700}
.ext-btn-row{display:flex;flex-wrap:wrap;gap:4px}
.ext-btn{padding:4px 9px;border-radius:14px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-size:11px;font-weight:600;font-family:var(--f-mo);color:var(--c-txt2);cursor:pointer;transition:.14s}
.ext-btn:hover{background:var(--c-co);border-color:var(--c-co3);color:var(--c-co3)}
.ext-btn.active{background:var(--c-co2);border-color:var(--c-co3);color:var(--c-co3)}
.preset-btn-grid{display:flex;flex-wrap:wrap;gap:4px}
.prs-btn{padding:5px 10px;border-radius:7px;background:var(--c-mn);border:1.5px solid var(--c-mn3);color:var(--c-mn3);font-size:11px;font-weight:600;cursor:pointer;transition:.14s;font-family:var(--f-ui)}
.prs-btn:hover{background:var(--c-mn2)}
.diatonic-grid{display:flex;flex-wrap:wrap;gap:6px}
.dchip{padding:10px 13px;border-radius:10px;font-size:13px;font-weight:800;font-family:var(--f-hd);cursor:pointer;transition:all .18s;border:1.5px solid transparent;display:flex;flex-direction:column;align-items:center;gap:2px;min-width:60px}
.dchip:hover{transform:translateY(-2px);box-shadow:var(--sh)}
.dchip-name{font-size:14px}
.dchip-roman{font-size:8.5px;font-family:var(--f-mo);opacity:.65}
.dchip.I  {background:var(--c-pk);border-color:var(--c-pk2);color:var(--c-pk3)}
.dchip.ii {background:var(--c-yw);border-color:var(--c-yw2);color:var(--c-yw3)}
.dchip.iii{background:var(--c-gn);border-color:var(--c-gn2);color:var(--c-gn3)}
.dchip.IV {background:var(--c-lv);border-color:var(--c-lv2);color:var(--c-lv3)}
.dchip.V  {background:var(--c-mn);border-color:var(--c-mn2);color:var(--c-mn3)}
.dchip.vi {background:var(--c-co);border-color:var(--c-co2);color:var(--c-co3)}
.dchip.vii{background:var(--c-bg3);border-color:var(--c-brd2);color:var(--c-txt2)}
.progression-row{display:flex;gap:6px;flex-wrap:wrap;min-height:48px;padding:8px;background:var(--c-bg2);border-radius:10px;border:1.5px dashed var(--c-brd2);margin-bottom:8px;align-items:center}
.prog-slot{padding:8px 13px;border-radius:8px;font-size:13px;font-weight:700;cursor:pointer;font-family:var(--f-hd);position:relative;transition:.14s}
.prog-slot:hover{opacity:.7}
.prog-empty-hint{font-size:11px;color:var(--c-txt3)}
.prog-info{font-size:10.5px;color:var(--c-txt2);font-family:var(--f-mo);line-height:1.7}
.prog-actions{display:flex;gap:6px;margin-top:8px}
.mini-kb{display:flex;position:relative;height:58px;background:var(--c-bg2);border-radius:8px;border:1px solid var(--c-brd);overflow:hidden;padding:4px}
.mk-white{flex:1;min-width:16px;max-width:26px;height:48px;background:var(--c-wh);border:1px solid var(--c-brd);border-radius:0 0 4px 4px;cursor:pointer;transition:.1s;position:relative;z-index:1}
.mk-black{position:absolute;background:#2e2340;z-index:2;height:30px;width:13px;border-radius:0 0 3px 3px;cursor:pointer;top:4px}
.mk-white:hover,.mk-black:hover{background:var(--c-lv)!important}
.mk-active{background:var(--c-lv2)!important}
.mini-kb-info{font-size:10px;color:var(--c-txt2);font-family:var(--f-mo);margin-top:6px;line-height:1.6}

/* ── MELODY ── */
.melody-area{display:flex;flex:1;overflow:hidden;min-height:0}
.mel-pitch-col{width:46px;flex-shrink:0;border-right:1px solid var(--c-brd);background:var(--c-bg1);overflow:hidden;display:flex;flex-direction:column;padding-top:22px}
.mel-pitch-lbl{height:14px;flex-shrink:0;font-size:8px;font-family:var(--f-mo);color:var(--c-txt3);display:flex;align-items:center;justify-content:flex-end;padding-right:5px}
.mel-pitch-lbl.cn{color:var(--c-lv3);font-weight:600}
.mel-pitch-lbl.inscale{color:var(--c-txt2)}
.mel-canvas-col{flex:1;display:flex;flex-direction:column;overflow:auto}
.mel-ruler-row{height:22px;flex-shrink:0;background:var(--c-bg2);border-bottom:1px solid var(--c-brd);display:flex;align-items:center;position:sticky;top:0;z-index:5}
.mel-ruler-tick{flex-shrink:0;font-size:8.5px;font-family:var(--f-mo);color:var(--c-txt3);border-right:1px solid var(--c-brd);display:flex;align-items:center;padding-left:3px;height:100%}
.mel-ruler-tick.bar{color:var(--c-lv3);font-weight:600;border-right-color:var(--c-lv2)}
#melCanvas{display:block;cursor:crosshair}
.mel-statusbar{display:flex;align-items:center;gap:14px;padding:5px 14px;border-top:1px solid var(--c-brd);background:var(--c-bg1);flex-shrink:0;font-size:11px}
.msb-info{font-family:var(--f-mo);color:var(--c-lv3);font-weight:500}
.msb-hint{color:var(--c-txt3)}
.msb-count{margin-left:auto;font-family:var(--f-mo);color:var(--c-txt2)}
.mel-tool-btn{padding:5px 9px;border-radius:7px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-size:11.5px;font-weight:600;color:var(--c-txt2);cursor:pointer;transition:.14s;font-family:var(--f-ui)}
.mel-tool-btn:hover{background:var(--c-yw);border-color:var(--c-yw3);color:var(--c-yw3)}
.mel-tool-btn.active{background:var(--c-yw2);border-color:var(--c-yw3);color:var(--c-yw3)}
.mel-color-dots{display:flex;gap:4px}
.mcd{width:18px;height:18px;border-radius:50%;cursor:pointer;border:2px solid transparent;transition:.12s}
.mcd.active,.mcd:hover{border-color:var(--c-txt);transform:scale(1.15)}

/* ── MIXER ── */
.mixer-area{display:flex;flex:1;overflow-x:auto;padding:16px;gap:2px;background:var(--c-bg2);align-items:flex-start}
.mix-ch{width:80px;flex-shrink:0;background:var(--c-bg1);border:1px solid var(--c-brd);border-radius:10px;display:flex;flex-direction:column;align-items:center;padding:10px 6px 8px;gap:5px}
.mix-ch-name{font-size:9.5px;font-weight:600;color:var(--c-txt2);text-align:center;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;width:100%}
.mix-fader-wrap{height:90px;display:flex;align-items:center;justify-content:center}
.mix-fader{-webkit-appearance:slider-vertical;appearance:slider-vertical;width:4px;height:80px;accent-color:var(--c-lv3)}
.mix-val{font-size:8.5px;font-family:var(--f-mo);color:var(--c-lv3)}
.mix-pan{width:58px;accent-color:var(--c-pk3)}
.mix-ms{display:flex;gap:3px;width:100%}
.mix-ms-btn{flex:1;padding:2px 0;border-radius:5px;border:1px solid var(--c-brd);background:var(--c-bg2);font-size:9px;font-weight:700;cursor:pointer;transition:.1s;text-align:center}
.mix-ms-btn.M:hover,.mix-ms-btn.M.active{background:var(--c-yw2);border-color:var(--c-yw3);color:var(--c-yw3)}
.mix-ms-btn.S:hover,.mix-ms-btn.S.active{background:var(--c-gn2);border-color:var(--c-gn3);color:var(--c-gn3)}
.mix-tiny-btn{width:100%;padding:2px 0;border-radius:5px;border:1px solid var(--c-brd);background:var(--c-bg2);font-size:9px;cursor:pointer;transition:.1s;text-align:center;color:var(--c-txt2)}
.mix-tiny-btn:hover{background:var(--c-lv);color:var(--c-lv3)}

/* ── ARP ── */
.arp-body{padding:16px;flex:1;overflow-y:auto}
.arp-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.arp-card{background:var(--c-bg1);border:1px solid var(--c-brd);border-radius:12px;padding:14px}
.arp-card-title{font-family:var(--f-mo);font-size:9px;letter-spacing:2px;color:var(--c-txt3);text-transform:uppercase;margin-bottom:10px}
.arp-ctrl{display:flex;align-items:center;gap:8px;margin-bottom:8px}
.arp-lbl{font-size:12px;color:var(--c-txt2);min-width:80px}
.arp-pat-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:5px}
.arp-pat-btn{padding:6px 0;border-radius:7px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-size:11px;font-weight:600;cursor:pointer;text-align:center;color:var(--c-txt2);transition:.14s}
.arp-pat-btn:hover{background:var(--c-lv);border-color:var(--c-lv3);color:var(--c-lv3)}
.arp-pat-btn.active{background:var(--c-lv2);border-color:var(--c-lv3);color:var(--c-lv3)}
.arp-keys{display:flex;flex-wrap:wrap;gap:4px}
.arp-key{padding:5px 9px;border-radius:7px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-family:var(--f-mo);font-size:11px;cursor:pointer;transition:.14s;color:var(--c-txt2)}
.arp-key:hover{background:var(--c-pk);border-color:var(--c-pk3);color:var(--c-pk3)}
.arp-key.active{background:var(--c-pk2);border-color:var(--c-pk3);color:var(--c-pk3)}

/* ── SCALE ── */
.scale-body{padding:16px;flex:1;overflow-y:auto}
.scale-key-row{display:flex;gap:5px;flex-wrap:wrap;margin-bottom:14px}
.scale-cards-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(270px,1fr));gap:10px}
.scale-card{background:var(--c-bg1);border:1px solid var(--c-brd);border-radius:12px;padding:13px;cursor:pointer;transition:.14s}
.scale-card:hover{border-color:var(--c-lv2);box-shadow:var(--sh);transform:translateY(-1px)}
.sc-title{font-family:var(--f-hd);font-size:14px;font-weight:700;margin-bottom:3px}
.sc-intervals{font-size:10px;color:var(--c-txt2);font-family:var(--f-mo);margin-bottom:8px}
.sc-notes{display:flex;gap:3px;flex-wrap:wrap}
.scn{padding:3px 7px;border-radius:5px;font-family:var(--f-mo);font-size:10.5px;font-weight:600}
.scn.root{background:var(--c-pk2);color:var(--c-pk3)}
.scn.in{background:var(--c-lv);color:var(--c-lv3)}
.scn.out{background:var(--c-bg3);color:var(--c-txt3)}

/* ── EQ ── */
.eq-body{padding:16px;flex:1;overflow-y:auto}
.eq-canvas-wrap{background:var(--c-bg1);border:1px solid var(--c-brd);border-radius:12px;overflow:hidden;margin-bottom:14px}
#eqCanvas{display:block;width:100%}
.eq-bands{display:flex;gap:12px;overflow-x:auto;padding-bottom:4px}
.eq-band{display:flex;flex-direction:column;align-items:center;gap:6px;background:var(--c-bg1);border:1px solid var(--c-brd);border-radius:10px;padding:10px 8px;min-width:70px}
.eq-band-freq{font-size:9px;font-family:var(--f-mo);color:var(--c-txt2)}
.eq-band-fader{-webkit-appearance:slider-vertical;appearance:slider-vertical;width:4px;height:80px;accent-color:var(--c-lv3)}
.eq-band-gain{font-size:9px;font-family:var(--f-mo);color:var(--c-lv3)}
.eq-band-type{font-size:8px;color:var(--c-txt3)}

/* ── PROJECTS ── */
.projects-body{padding:16px;flex:1;overflow-y:auto}
.proj-empty{text-align:center;color:var(--c-txt3);padding:40px;font-size:13px}
.proj-cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:10px}
.proj-card{background:var(--c-bg1);border:1px solid var(--c-brd);border-radius:12px;padding:14px;transition:.14s;cursor:pointer}
.proj-card:hover{border-color:var(--c-lv2);box-shadow:var(--sh);transform:translateY(-1px)}
.proj-card-name{font-family:var(--f-hd);font-size:14px;font-weight:700;margin-bottom:4px}
.proj-card-meta{font-size:10.5px;color:var(--c-txt2);font-family:var(--f-mo);margin-bottom:8px}
.proj-card-tags{display:flex;gap:4px;flex-wrap:wrap;margin-bottom:10px}
.proj-tag{padding:2px 7px;border-radius:5px;font-size:9.5px;font-weight:600}
.proj-card-actions{display:flex;gap:5px}
.pca-btn{padding:4px 10px;border-radius:6px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-size:10.5px;font-weight:600;cursor:pointer;transition:.12s;color:var(--c-txt2)}
.pca-btn:hover{background:var(--c-lv);color:var(--c-lv3);border-color:var(--c-lv3)}
.pca-btn.del:hover{background:var(--c-pk3);color:#fff;border-color:var(--c-pk3)}

/* ── LOOPS ── */
.loops-body{padding:16px;flex:1;overflow-y:auto}
.loop-filter-row{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:14px}
.lf-tag{padding:5px 12px;border-radius:14px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-size:11px;font-weight:600;color:var(--c-txt2);cursor:pointer;transition:.14s}
.lf-tag:hover{background:var(--c-mn);border-color:var(--c-mn3);color:var(--c-mn3)}
.lf-tag.active{background:var(--c-mn2);border-color:var(--c-mn3);color:var(--c-mn3)}
.loop-cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(210px,1fr));gap:8px}
.loop-card{background:var(--c-bg1);border:1px solid var(--c-brd);border-radius:10px;padding:10px 12px;display:flex;align-items:center;gap:9px;cursor:pointer;transition:.14s}
.loop-card:hover{border-color:var(--c-lv2);background:var(--c-bg2);transform:translateY(-1px);box-shadow:var(--sh)}
.lc-play{width:32px;height:32px;border-radius:50%;border:none;cursor:pointer;font-size:12px;transition:.14s;flex-shrink:0;display:flex;align-items:center;justify-content:center}
.lc-info{flex:1;min-width:0}
.lc-name{font-size:12px;font-weight:600;color:var(--c-txt);margin-bottom:2px}
.lc-meta{font-size:9.5px;color:var(--c-txt3);font-family:var(--f-mo)}
.lc-tags{display:flex;gap:3px;margin-top:3px}
.lc-tag{padding:1px 5px;border-radius:4px;font-size:8.5px;font-weight:600}

/* ── TUTORIAL ── */
.tutorial-body{padding:16px;flex:1;overflow-y:auto}
.tut-cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(290px,1fr));gap:12px}
.tut-card{background:var(--c-bg1);border:1px solid var(--c-brd);border-radius:14px;overflow:hidden;cursor:pointer;transition:.18s}
.tut-card:hover{border-color:var(--c-lv2);box-shadow:var(--sh);transform:translateY(-2px)}
.tut-card-top{height:72px;display:flex;align-items:center;justify-content:center;font-size:32px}
.tut-card-body{padding:12px 14px}
.tut-card-title{font-family:var(--f-hd);font-size:13px;font-weight:700;margin-bottom:4px}
.tut-card-desc{font-size:11px;color:var(--c-txt2);line-height:1.5;margin-bottom:8px}
.tut-steps{list-style:none;display:flex;flex-direction:column;gap:5px}
.tut-step{display:flex;gap:6px;font-size:10.5px;color:var(--c-txt2);line-height:1.5}
.tut-snum{width:18px;height:18px;border-radius:50%;flex-shrink:0;display:flex;align-items:center;justify-content:center;font-size:8.5px;font-weight:700}

/* ── GLOSSARY ── */
.glossary-body{padding:16px;flex:1;overflow-y:auto}
.glossary-search{width:100%;max-width:500px;padding:8px 14px;border-radius:8px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-family:var(--f-ui);font-size:13px;color:var(--c-txt);margin-bottom:12px;display:block}
.glossary-search:focus{outline:none;border-color:var(--c-lv3)}
.g-item{background:var(--c-bg1);border:1px solid var(--c-brd);border-radius:10px;padding:10px 14px;margin-bottom:6px;cursor:pointer;transition:.14s}
.g-item:hover{border-color:var(--c-lv2);background:var(--c-bg2)}
.g-term{font-weight:700;font-size:13px;margin-bottom:3px}
.g-def{font-size:11.5px;color:var(--c-txt2);line-height:1.6}
.g-tags{display:flex;gap:4px;margin-top:5px}
.g-tag{padding:1px 6px;border-radius:4px;font-size:9px;font-weight:700}

/* ── INSPECTOR ── */
.inspector{width:var(--ins-w);flex-shrink:0;background:var(--c-bg1);border-left:1px solid var(--c-brd);overflow-y:auto;padding:12px}
.inspector::-webkit-scrollbar{width:4px}
.inspector::-webkit-scrollbar-thumb{background:var(--c-brd2);border-radius:2px}
.ins-title{font-family:var(--f-hd);font-size:14px;font-weight:700;margin-bottom:12px;padding-bottom:8px;border-bottom:1px solid var(--c-brd)}
.ins-empty{font-size:12px;color:var(--c-txt3);line-height:1.6}
.ins-sec{margin-bottom:12px}
.ins-sec-lbl{font-family:var(--f-mo);font-size:8.5px;letter-spacing:2px;color:var(--c-txt3);text-transform:uppercase;margin-bottom:6px}
.ins-row{display:flex;justify-content:space-between;align-items:center;margin-bottom:5px}
.ins-k{font-size:11px;color:var(--c-txt2)}
.ins-v{font-size:11px;font-family:var(--f-mo);color:var(--c-lv3);font-weight:500}

/* ── STATUS BAR ── */
.statusbar{height:var(--sb-h);background:var(--c-bg1);border-top:1px solid var(--c-brd);display:flex;align-items:center;padding:0 12px;gap:14px;font-family:var(--f-mo);font-size:9.5px;color:var(--c-txt3);flex-shrink:0}
.sb-l{display:flex;align-items:center;gap:6px}
.sb-c{flex:1;text-align:center;font-weight:600;color:var(--c-txt2)}
.sb-r{display:flex;align-items:center;gap:12px}
.sb-dot{width:7px;height:7px;border-radius:50%;background:var(--c-brd2)}
.sb-dot.ready{background:var(--c-gn3)}
.sb-dot.playing{background:var(--c-gn3);animation:pulse-gn .7s infinite}
.sb-dot.rec{background:var(--c-pk3);animation:pulse-pk .7s infinite}

/* ── MODALS ── */
.modal-overlay{position:fixed;inset:0;z-index:8000;background:rgba(30,20,50,.45);backdrop-filter:blur(4px);display:flex;align-items:center;justify-content:center}
.modal-card{background:var(--c-bg1);border:1px solid var(--c-brd2);border-radius:18px;padding:28px;min-width:420px;max-width:540px;width:90%;position:relative;box-shadow:var(--sh2);max-height:90vh;overflow-y:auto}
.modal-title{font-family:var(--f-hd);font-size:18px;font-weight:700;margin-bottom:16px}
.modal-tabs{display:flex;gap:0;border-bottom:1px solid var(--c-brd);margin-bottom:16px}
.mtab{padding:7px 18px;border:none;background:none;font-family:var(--f-ui);font-size:13px;font-weight:600;color:var(--c-txt2);cursor:pointer;border-bottom:2.5px solid transparent;transition:.14s}
.mtab.active{color:var(--c-lv3);border-bottom-color:var(--c-lv3)}
.modal-body{display:flex;flex-direction:column;gap:12px}
.modal-field{display:flex;flex-direction:column;gap:4px}
.mf-lbl{font-size:11px;font-weight:600;color:var(--c-txt2);font-family:var(--f-mo);letter-spacing:1px}
.mf-inp{padding:8px 12px;border:1.5px solid var(--c-brd);border-radius:7px;font-family:var(--f-ui);font-size:13px;background:var(--c-bg2);color:var(--c-txt)}
.mf-inp:focus{outline:none;border-color:var(--c-lv3)}
.modal-actions{display:flex;gap:8px;flex-wrap:wrap}
.ma-btn{padding:8px 16px;border-radius:8px;border:1.5px solid var(--c-brd);background:var(--c-bg2);font-family:var(--f-ui);font-size:12px;font-weight:600;color:var(--c-txt2);cursor:pointer;transition:.14s}
.ma-btn:hover{background:var(--c-lv);border-color:var(--c-lv3);color:var(--c-lv3)}
.ma-btn-primary{background:var(--c-lv2);border-color:var(--c-lv3);color:var(--c-lv3)}
.ta-btn-save.ma-btn{background:var(--c-mn);border-color:var(--c-mn3);color:var(--c-mn3)}
.modal-close{position:absolute;top:14px;right:14px;background:var(--c-bg2);border:1px solid var(--c-brd);border-radius:50%;width:28px;height:28px;font-size:13px;cursor:pointer;display:flex;align-items:center;justify-content:center;color:var(--c-txt2);transition:.14s}
.modal-close:hover{background:var(--c-pk);color:var(--c-pk3)}
.saved-project-list{display:flex;flex-direction:column;gap:6px;max-height:300px;overflow-y:auto}
.sp-item{display:flex;align-items:center;gap:8px;padding:9px 12px;border:1px solid var(--c-brd);border-radius:9px;background:var(--c-bg2);cursor:pointer;transition:.14s}
.sp-item:hover{border-color:var(--c-lv2)}
.sp-name{flex:1;font-size:13px;font-weight:600}
.sp-meta{font-size:10px;color:var(--c-txt3);font-family:var(--f-mo)}
.sp-load-btn{padding:4px 10px;border-radius:6px;border:1.5px solid var(--c-lv3);background:var(--c-lv);color:var(--c-lv3);font-size:11px;font-weight:700;cursor:pointer;transition:.12s}
.sp-load-btn:hover{background:var(--c-lv2)}
.sp-del-btn{padding:4px 8px;border-radius:6px;border:1.5px solid var(--c-pk3);background:var(--c-pk);color:var(--c-pk3);font-size:11px;cursor:pointer;transition:.12s}
.sp-del-btn:hover{background:var(--c-pk3);color:#fff}
.sets-section{border:1px solid var(--c-brd);border-radius:10px;padding:12px;display:flex;flex-direction:column;gap:8px}
.sets-label{font-family:var(--f-mo);font-size:9px;letter-spacing:2px;color:var(--c-txt3);text-transform:uppercase;margin-bottom:2px}
.sets-row{display:flex;align-items:center;justify-content:space-between;gap:8px}
.sets-key{font-size:12px;font-weight:500;color:var(--c-txt2)}
.sets-msg{font-size:12px;min-height:16px;font-weight:600}
.sets-msg.ok{color:var(--c-gn3)}
.sets-msg.err{color:var(--c-pk3)}

/* ── TOAST ── */
.toast{position:fixed;bottom:36px;left:50%;transform:translateX(-50%);background:var(--c-txt);color:var(--c-bg);padding:10px 22px;border-radius:20px;font-size:13px;font-weight:600;z-index:9999;box-shadow:var(--sh2);animation:toast-in .25s ease}
@keyframes toast-in{from{opacity:0;transform:translateX(-50%) translateY(10px)}to{opacity:1;transform:translateX(-50%) translateY(0)}}

/* ── SCROLLBAR ── */
::-webkit-scrollbar{width:6px;height:6px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:var(--c-brd2);border-radius:3px}
::-webkit-scrollbar-thumb:hover{background:var(--c-lv2)}

/* ── ANIM ── */
.tab-view.active{animation:fadeSlide .18s ease}
@keyframes fadeSlide{from{opacity:0;transform:translateY(5px)}to{opacity:1;transform:none}}
@media(max-width:900px){.inspector{display:none}}
@media(max-width:700px){.sidebar{display:none}}
/* ══════════════════════════════════════════════════
   MeloMelo Studio v3.0 — Full Feature JavaScript
   ══════════════════════════════════════════════════ */
'use strict';

// ── CONSTANTS ──────────────────────────────────────────────────────────
const NOTES_S = ['C','C#','D','D#','E','F','F#','G','G#','A','A#','B'];
const NOTES_F = ['C','Db','D','Eb','E','F','F#','G','Ab','A','Bb','B'];
const IS_BLACK = [0,1,0,1,0,0,1,0,1,0,1,0];

const SCALES = {
  major:     {iv:[0,2,4,5,7,9,11], ko:'장조 (Major)', en:'Major'},
  minor:     {iv:[0,2,3,5,7,8,10], ko:'단조 (Minor)', en:'Minor'},
  dorian:    {iv:[0,2,3,5,7,9,10], ko:'도리안 (Dorian)', en:'Dorian'},
  phrygian:  {iv:[0,1,3,5,7,8,10], ko:'프리지안 (Phrygian)', en:'Phrygian'},
  lydian:    {iv:[0,2,4,6,7,9,11], ko:'리디안 (Lydian)', en:'Lydian'},
  mixolydian:{iv:[0,2,4,5,7,9,10], ko:'믹솔리디안 (Mixolydian)', en:'Mixolydian'},
  pentatonic:{iv:[0,2,4,7,9],       ko:'펜타토닉', en:'Pentatonic Major'},
  blues:     {iv:[0,3,5,6,7,10],    ko:'블루스', en:'Blues'},
  chromatic: {iv:[0,1,2,3,4,5,6,7,8,9,10,11], ko:'반음계', en:'Chromatic'},
};
const CHORD_DEGS = {
  major:['I','ii','iii','IV','V','vi','vii°'],
  minor:['i','ii°','III','iv','v','VI','VII'],
  dorian:['i','ii','III','IV','v','vi°','VII'],
  phrygian:['i','II','III','iv','v°','VI','vii'],
  lydian:['I','II','iii','iv°','V','vi','vii'],
  mixolydian:['I','ii','iii°','IV','v','vi','VII'],
};
const DRUM_INST = [
  {id:'kick',  ko:'킥 드럼',  en:'Kick Drum',  freq:55,  type:'sine',     dur:.28},
  {id:'snare', ko:'스네어',   en:'Snare',      freq:200, type:'sawtooth', dur:.18},
  {id:'hihat', ko:'하이햇',   en:'Hi-Hat',     freq:900, type:'square',   dur:.07},
  {id:'clap',  ko:'클랩',     en:'Clap',       freq:400, type:'sawtooth', dur:.12},
  {id:'crash', ko:'크래시',   en:'Crash',      freq:1100,type:'square',   dur:.55},
  {id:'ride',  ko:'라이드',   en:'Ride',       freq:700, type:'square',   dur:.3},
  {id:'perc',  ko:'퍼커션',   en:'Perc',       freq:300, type:'triangle', dur:.16},
  {id:'fx',    ko:'FX',       en:'FX',         freq:500, type:'sine',     dur:.22},
];
const DRUM_PRESETS = {
  basic: {kick:[1,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0],snare:[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,0],hihat:[1,0,1,0,1,0,1,0,1,0,1,0,1,0,1,0],clap:[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,0],crash:[1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],ride:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],perc:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],fx:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]},
  hiphop:{kick:[1,0,0,1,0,0,1,0,1,0,0,0,1,0,0,0],snare:[0,0,0,0,1,0,0,0,0,0,0,0,1,0,1,0],hihat:[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],clap:[0,0,1,0,0,0,0,1,0,0,1,0,0,1,0,0],crash:[1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],ride:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],perc:[0,1,0,0,0,1,0,0,0,0,0,1,0,0,0,0],fx:[0,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0]},
  bossa: {kick:[1,0,0,1,0,0,1,0,0,1,0,0,1,0,0,0],snare:[0,0,1,0,0,1,0,0,0,0,1,0,0,1,0,0],hihat:[1,0,1,0,1,0,1,0,1,0,1,0,1,0,1,0],clap:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],crash:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],ride:[1,0,0,1,0,1,0,0,1,0,0,1,0,1,0,0],perc:[0,1,0,0,1,0,0,1,0,1,0,0,1,0,0,1],fx:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]},
  dnb:   {kick:[1,0,0,0,0,0,0,0,0,0,1,0,0,0,0,0],snare:[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,0],hihat:[1,1,0,1,1,1,0,1,1,1,0,1,1,1,0,1],clap:[0,0,0,0,1,0,0,0,0,0,0,0,1,0,0,0],crash:[1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],ride:[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],perc:[0,0,1,0,0,0,1,0,0,1,0,0,0,0,1,0],fx:[0,0,0,0,0,0,0,0,1,0,0,0,0,0,0,0]},
};

const NOTE_COLORS = ['#baaff0','#f0a0be','#85c8f0','#f8d560','#7cd8a0','#ffb060','#d0a0e8','#a0d8c0'];
const MEL_COLORS =  ['#baaff0','#f0a0be','#85c8f0','#f8d560','#7cd8a0','#ffb060'];
const TRACK_COLORS = ['#f7c5d5','#ddd0f8','#c0e4f8','#fdeea0','#b8f0d0','#ffd8a8','#e8c0f0','#b8e8f8'];

// ── STATE ──────────────────────────────────────────────────────────────
let lang = 'ko';
let theme = 'light';
let bpm = 120;
let timeSig = 4;
let masterVol = 0.8;
let isPlaying = false;
let isRecording = false;
let playheadBeat = 0;
let phTimer = null;
let tapTimes = [];
let actx = null;

// Tracks
let tracks = [];
let trackId = 0;

// Piano Roll
let rollNotes = [];
let rollTool = 'pencil';
let rollNoteColor = NOTE_COLORS[0];
let rollCellW = 22, rollCellH = 20;
let rollBars = 2, rollSnap = 8, rollOct = 4;
let rollDrawing = false, rollDrawNote = null;

// Drum
let drumSeq = {};
let drumSteps = 16;
let drumPreviewEnabled = true;
let swingAmt = 0, humanizeAmt = 0, drumMasterVol = 0.8;

// Chord
let chordKey = 0;
let chordMode = 'major';
let chordExt = 'triad';
let chordInv = 0;
let chordProg = [];

// Melody
let melNotes = [];
let melTool = 'draw';
let melColor = MEL_COLORS[0];
let melCellW = 24, melCellH = 14;
let melCols = 64, melRows = 36;
let melDrawing = false, melDragNote = null;
let melCanvasEl = null, melCtx2 = null;

// Arp
let arpKeys = [];
let arpPattern = 'up';
let arpRate = 8;
let arpOct = 1;
let arpGate = 80;
let arpInst = 'triangle';
let arpTimer = null;
let arpStep = 0;
let arpPlaying = false;

// Projects
let currentProjectName = '새 프로젝트';
let currentProjectId = null;

// Settings / Auth
let PASSWORD = '1234'; // default

// EQ
const EQ_BANDS = [31,62,125,250,500,1000,2000,4000,8000,16000];
let eqGains = new Array(EQ_BANDS.length).fill(0);
let eqNodes = [];

// ── AUTH ───────────────────────────────────────────────────────────────
function doLogin() {
  const v = document.getElementById('loginPwd').value;
  const stored = localStorage.getItem('mm_pwd') || PASSWORD;
  if (v === stored) {
    document.getElementById('loginScreen').classList.add('fade-out');
    setTimeout(() => {
      document.getElementById('loginScreen').classList.add('hidden');
      document.getElementById('appRoot').classList.remove('hidden');
      initApp();
    }, 400);
  } else {
    document.getElementById('loginError').textContent =
      lang === 'ko' ? '비밀번호가 올바르지 않습니다.' : 'Incorrect password.';
    document.getElementById('loginPwd').value = '';
    setTimeout(() => document.getElementById('loginError').textContent = '', 2000);
  }
}
function doLogout() {
  if (!confirm(lang === 'ko' ? '로그아웃 하시겠습니까?' : 'Logout?')) return;
  document.getElementById('appRoot').classList.add('hidden');
  document.getElementById('loginScreen').classList.remove('hidden', 'fade-out');
  document.getElementById('loginPwd').value = '';
}
function setLoginLang(l, el) {
  lang = l;
  document.querySelectorAll('.ll-btn').forEach(b => b.classList.remove('active'));
  el.classList.add('active');
  applyLoginLang();
}
function applyLoginLang() {
  document.getElementById('ll_password').textContent = lang === 'ko' ? '비밀번호' : 'Password';
  document.getElementById('ll_enter').textContent = lang === 'ko' ? '입장하기' : 'Enter Studio';
  document.getElementById('ll_hint').textContent = lang === 'ko' ? '기본 비밀번호: 1234' : 'Default password: 1234';
}
function togglePwdVis(id, btn) {
  const inp = document.getElementById(id);
  inp.type = inp.type === 'password' ? 'text' : 'password';
  btn.textContent = inp.type === 'password' ? '👁' : '🙈';
}
function changePassword() {
  const cur = document.getElementById('curPwd').value;
  const nw = document.getElementById('newPwd').value;
  const cf = document.getElementById('cfmPwd').value;
  const msg = document.getElementById('pwdChangeMsg');
  const stored = localStorage.getItem('mm_pwd') || '1234';
  if (cur !== stored) { msg.textContent = lang==='ko'?'현재 비밀번호가 틀렸습니다.':'Wrong current password.'; msg.className='sets-msg err'; return; }
  if (nw.length < 4) { msg.textContent = lang==='ko'?'4자 이상 입력하세요.':'Min 4 characters.'; msg.className='sets-msg err'; return; }
  if (nw !== cf) { msg.textContent = lang==='ko'?'비밀번호가 일치하지 않습니다.':'Passwords do not match.'; msg.className='sets-msg err'; return; }
  localStorage.setItem('mm_pwd', nw);
  msg.textContent = lang==='ko'?'비밀번호가 변경되었습니다!':'Password changed!';
  msg.className = 'sets-msg ok';
  document.getElementById('curPwd').value = '';
  document.getElementById('newPwd').value = '';
  document.getElementById('cfmPwd').value = '';
  setTimeout(() => msg.textContent = '', 3000);
}

// ── AUDIO ───────────────────────────────────────────────────────────────
function ctx() { if (!actx) actx = new (window.AudioContext || window.webkitAudioContext)(); return actx; }
function playTone(freq, when, dur, vol=0.3, type='triangle', att=0.01, rel=0.08) {
  const c = ctx();
  const osc = c.createOscillator();
  const g = c.createGain();
  osc.connect(g); g.connect(c.destination);
  osc.type = type; osc.frequency.value = freq;
  g.gain.setValueAtTime(0, when);
  g.gain.linearRampToValueAtTime(Math.min(1, vol * masterVol * drumMasterVol), when + att);
  g.gain.exponentialRampToValueAtTime(0.001, when + dur - rel + 0.01);
  osc.start(when); osc.stop(when + dur + 0.05);
}
function playDrum(id, when) {
  const d = DRUM_INST.find(x => x.id === id);
  if (!d) return;
  const c = ctx();
  const osc = c.createOscillator();
  const g = c.createGain();
  osc.connect(g); g.connect(c.destination);
  osc.type = d.type; osc.frequency.value = d.freq;
  if (id === 'kick') { osc.frequency.setValueAtTime(d.freq*2.5, when); osc.frequency.exponentialRampToValueAtTime(d.freq, when+0.07); }
  g.gain.setValueAtTime(Math.min(1, 0.55 * masterVol * drumMasterVol), when);
  g.gain.exponentialRampToValueAtTime(0.001, when + d.dur);
  osc.start(when); osc.stop(when + d.dur + 0.06);
}
function midiToFreq(midi) { return 440 * Math.pow(2, (midi - 69) / 12); }
function freqToMidi(freq) { return Math.round(69 + 12 * Math.log2(freq / 440)); }

// ── INIT ────────────────────────────────────────────────────────────────
function initApp() {
  loadSettings();
  applyLang();
  buildRuler();
  buildNoteColors();
  buildPianoRoll();
  buildDrumMachine();
  buildChordUI();
  buildMelodyCanvas();
  buildMelColorDots();
  buildArp();
  buildScaleHelper();
  buildEQ();
  buildLoops();
  buildTutorial();
  buildGlossary();
  startClock();
  setStatus('ready');
  renderProjects();
  updateProjectName();
  document.getElementById('sbProject').textContent = currentProjectName;
}

function loadSettings() {
  const s = localStorage.getItem('mm_settings');
  if (s) {
    try {
      const o = JSON.parse(s);
      if (o.lang) { lang = o.lang; document.getElementById('langSelect').value = lang; }
      if (o.theme) { applyTheme(o.theme); document.getElementById('themeSelect').value = o.theme; }
    } catch(e) {}
  }
}
function saveSettings() {
  localStorage.setItem('mm_settings', JSON.stringify({ lang, theme }));
}

// ── LANGUAGE ────────────────────────────────────────────────────────────
const TT = {
  // sidebar
  sg_compose:{ko:'작곡',en:'COMPOSE'}, sg_tools:{ko:'도구',en:'TOOLS'}, sg_files:{ko:'파일',en:'FILES'}, sg_learn:{ko:'학습',en:'LEARN'},
  sbn_arrange:{ko:'어레인지',en:'Arrange'}, sbn_piano:{ko:'피아노 롤',en:'Piano Roll'}, sbn_drum:{ko:'드럼 머신',en:'Drum Machine'},
  sbn_chord:{ko:'코드 빌더',en:'Chord Builder'}, sbn_melody:{ko:'멜로디 스케치',en:'Melody Sketch'}, sbn_mixer:{ko:'믹서',en:'Mixer'},
  sbn_arp:{ko:'아르페지에이터',en:'Arpeggiator'}, sbn_scale:{ko:'스케일 헬퍼',en:'Scale Helper'}, sbn_eq:{ko:'이퀄라이저',en:'EQ'},
  sbn_projects:{ko:'내 프로젝트',en:'My Projects'}, sbn_loops:{ko:'루프 라이브러리',en:'Loop Library'},
  sbn_tutorial:{ko:'튜토리얼',en:'Tutorial'}, sbn_glossary:{ko:'용어사전',en:'Glossary'},
  // headers
  th_arrange:{ko:'어레인지',en:'Arrange'}, th_piano:{ko:'피아노 롤',en:'Piano Roll'}, th_drum:{ko:'드럼 머신',en:'Drum Machine'},
  th_chord:{ko:'코드 빌더',en:'Chord Builder'}, th_melody:{ko:'멜로디 스케치',en:'Melody Sketch'}, th_mixer:{ko:'믹서',en:'Mixer'},
  th_arp:{ko:'아르페지에이터',en:'Arpeggiator'}, th_scale:{ko:'스케일 헬퍼',en:'Scale Helper'}, th_eq:{ko:'이퀄라이저',en:'EQ'},
  th_projects:{ko:'내 프로젝트',en:'My Projects'}, th_loops:{ko:'루프 라이브러리',en:'Loop Library'},
  th_tutorial:{ko:'튜토리얼',en:'Tutorial'}, th_glossary:{ko:'용어사전',en:'Glossary'},
  // actions
  ta_addsynth:{ko:'+ 신스',en:'+ Synth'}, ta_adddrum:{ko:'+ 드럼',en:'+ Drum'}, ta_addbass:{ko:'+ 베이스',en:'+ Bass'},
  ta_addpad:{ko:'+ 패드',en:'+ Pad'}, ta_addsample:{ko:'+ 샘플',en:'+ Sample'}, ta_clearall:{ko:'전체 초기화',en:'Clear All'},
  ta_quantize:{ko:'Quantize',en:'Quantize'}, ta_rplay:{ko:'▶ Play',en:'▶ Play'}, ta_rclear:{ko:'Clear',en:'Clear'}, ta_rmidi:{ko:'⬇ MIDI',en:'⬇ MIDI'},
  ta_dp1:{ko:'기본',en:'Basic'}, ta_dp2:{ko:'힙합',en:'Hip-Hop'}, ta_dp3:{ko:'보사노바',en:'Bossa Nova'}, ta_dp4:{ko:'D&B',en:'D&B'},
  ta_previewlbl:{ko:'비트 미리듣기',en:'Beat Preview'}, ta_dplay:{ko:'▶ Play',en:'▶ Play'}, ta_dclear:{ko:'Clear',en:'Clear'}, ta_dmidi:{ko:'⬇ MIDI',en:'⬇ MIDI'},
  ta_cplay:{ko:'▶ Play',en:'▶ Play'}, ta_cexport:{ko:'⬇ Export',en:'⬇ Export'}, ta_cclear:{ko:'Clear',en:'Clear'},
  ta_mplay:{ko:'▶ Play',en:'▶ Play'}, ta_mmidi:{ko:'⬇ MIDI',en:'⬇ MIDI'}, ta_mclear:{ko:'Clear',en:'Clear'},
  ta_mixreset:{ko:'Reset All',en:'Reset All'}, ta_mixdown:{ko:'⬇ Mixdown',en:'⬇ Mixdown'},
  ta_arpplay:{ko:'▶ Start',en:'▶ Start'}, ta_saveproj:{ko:'💾 저장',en:'💾 Save'}, ta_newproj:{ko:'+ 새 프로젝트',en:'+ New Project'},
  ta_exportproj:{ko:'⬇ JSON 내보내기',en:'⬇ Export JSON'}, ta_importproj:{ko:'⬆ 가져오기',en:'⬆ Import'},
  ta_eqreset:{ko:'Reset',en:'Reset'}, ta_eqpreview:{ko:'Preview',en:'Preview'},
  ta_shuffle:{ko:'🔀 Shuffle',en:'🔀 Shuffle'}, ta_reverse:{ko:'↩ Reverse',en:'↩ Reverse'},
  // labels
  rl_vel:{ko:'Velocity',en:'Velocity'}, rl_notecolor:{ko:'Color',en:'Color'}, rl_vellane:{ko:'Vel',en:'Vel'},
  dl_swing:{ko:'Swing',en:'Swing'}, dl_humanize:{ko:'Humanize',en:'Humanize'}, dl_vol:{ko:'Drum Vol',en:'Drum Vol'},
  cl_key:{ko:'KEY',en:'KEY'}, cl_mode:{ko:'MODE / SCALE',en:'MODE / SCALE'}, cl_ext:{ko:'EXTENSION',en:'EXTENSION'},
  cl_inv:{ko:'INVERSION',en:'INVERSION'}, cl_presets:{ko:'PRESETS',en:'PRESETS'},
  cl_diatonic:{ko:'DIATONIC CHORDS',en:'DIATONIC CHORDS'}, cl_prog:{ko:'PROGRESSION',en:'PROGRESSION'},
  cl_keyboard:{ko:'KEYBOARD PREVIEW',en:'KEYBOARD PREVIEW'},
  mt_draw:{ko:'그리기',en:'Draw'}, mt_erase:{ko:'지우기',en:'Erase'}, mt_select:{ko:'선택',en:'Select'},
  msb_hint:{ko:'✏️ 클릭·드래그로 늘리기·우클릭 삭제',en:'✏️ Click · Drag to extend · Right-click remove'},
  prog_hint:{ko:'코드를 클릭해서 추가하세요',en:'Click chords to add'},
  es_arrange:{ko:'트랙을 추가해서 작곡을 시작하세요',en:'Add a track to start composing'},
  es_mixer:{ko:'Arrange에서 트랙을 추가하세요',en:'Add tracks in Arrange'},
  ins_title:{ko:'인스펙터',en:'Inspector'}, ins_empty:{ko:'항목을 선택하세요',en:'Select an item'},
  // modals
  sm_title:{ko:'💾 저장 / 불러오기',en:'💾 Save / Load'},
  mt_save:{ko:'저장',en:'Save'}, mt_load:{ko:'불러오기',en:'Load'},
  mf_name:{ko:'프로젝트 이름',en:'Project Name'}, ma_savenew:{ko:'새로 저장',en:'Save New'},
  ma_overwrite:{ko:'현재 덮어쓰기',en:'Overwrite Current'}, ma_export:{ko:'JSON 내보내기',en:'Export JSON'},
  ma_import:{ko:'⬆ JSON 가져오기',en:'⬆ Import JSON'},
  sets_title:{ko:'설정',en:'Settings'}, sets_security:{ko:'🔒 보안',en:'🔒 Security'},
  sets_changepwd:{ko:'비밀번호 변경',en:'Change Password'}, mf_curpwd:{ko:'현재 비밀번호',en:'Current Password'},
  mf_newpwd:{ko:'새 비밀번호',en:'New Password'}, mf_cfmpwd:{ko:'비밀번호 확인',en:'Confirm Password'},
  sets_applypwd:{ko:'비밀번호 변경',en:'Change Password'}, sets_audio:{ko:'🔊 오디오',en:'🔊 Audio'},
  sets_latency:{ko:'레이턴시 모드',en:'Latency Mode'}, sets_tuning:{ko:'기준음 (Hz)',en:'Tuning (Hz)'},
  sets_display:{ko:'🎨 디스플레이',en:'🎨 Display'}, sets_theme:{ko:'테마',en:'Theme'}, sets_lang:{ko:'언어',en:'Language'},
  sets_data:{ko:'📦 데이터',en:'📦 Data'}, sets_cleardata:{ko:'모든 데이터 초기화',en:'Clear All Data'},
};
function applyLang() {
  for (const [id, val] of Object.entries(TT)) {
    const el = document.getElementById(id);
    if (el) el.textContent = val[lang] || val.ko;
  }
  document.getElementById('langBtn').textContent = lang === 'ko' ? 'EN' : '한';
  document.getElementById('glossarySearch').placeholder = lang === 'ko' ? '검색...' : 'Search...';
  saveSettings();
}
function toggleLang() { lang = lang === 'ko' ? 'en' : 'ko'; document.getElementById('langSelect').value = lang; applyLang(); buildChordUI(); buildDrumMachine(); buildArp(); buildScaleHelper(); buildTutorial(); buildGlossary(); renderProjects(); }
function setLangFromSelect(v) { lang = v; document.getElementById('langBtn').textContent = lang === 'ko' ? 'EN' : '한'; applyLang(); buildChordUI(); buildDrumMachine(); buildArp(); buildScaleHelper(); buildTutorial(); buildGlossary(); renderProjects(); }

// ── THEME ────────────────────────────────────────────────────────────────
function applyTheme(t) {
  theme = t;
  document.body.classList.remove('dark','pink','mint');
  if (t !== 'light') document.body.classList.add(t);
  document.getElementById('themeBtn').textContent = t === 'dark' ? '☀️' : '🌙';
  saveSettings();
}
function toggleTheme() { applyTheme(theme === 'dark' ? 'light' : 'dark'); document.getElementById('themeSelect').value = theme; }

// ── TRANSPORT ────────────────────────────────────────────────────────────
function transportPlay() {
  isPlaying = !isPlaying;
  const b = document.getElementById('tb_play');
  b.textContent = isPlaying ? '⏸' : '▶';
  b.classList.toggle('active', isPlaying);
  setStatus(isPlaying ? 'playing' : 'ready');
  if (isPlaying) { startPhTimer(); playDrumLoop(); } else stopPhTimer();
}
function transportStop() { isPlaying = false; playheadBeat = 0; document.getElementById('tb_play').textContent='▶'; document.getElementById('tb_play').classList.remove('active'); stopPhTimer(); updatePosDisplay(); setStatus('ready'); }
function transportRewind() { playheadBeat = 0; updatePosDisplay(); }
function transportRec() { isRecording = !isRecording; document.getElementById('tb_rec').classList.toggle('active', isRecording); setStatus(isRecording ? 'rec' : (isPlaying?'playing':'ready')); }
function setBpm(v) { bpm = Math.max(40, Math.min(240, v)); document.getElementById('bpmInp').value = bpm; }
function setTimeSig(v) { timeSig = v; }
function setMasterVol(v) { masterVol = v/100; document.getElementById('masterVolVal').textContent = v; }

let drumLoopTimer = null;
function playDrumLoop() {
  if (drumLoopTimer) clearTimeout(drumLoopTimer);
  let step = 0;
  const total = drumSteps;
  function tick() {
    if (!isPlaying) return;
    const spb = 60/bpm/4;
    const now = ctx().currentTime;
    DRUM_INST.forEach(d => {
      if (drumSeq[d.id] && drumSeq[d.id][step]) {
        const jitter = humanizeAmt > 0 ? (Math.random()-0.5)*humanizeAmt/1000 : 0;
        let t = now + jitter;
        if (swingAmt > 0 && step%2===1) t += (spb * swingAmt/200);
        playDrum(d.id, Math.max(now, t));
        highlightDrumPad(d.id, step);
      }
    });
    step = (step+1) % total;
    drumLoopTimer = setTimeout(tick, (60/bpm/4)*1000);
  }
  tick();
}
function highlightDrumPad(id, step) {
  const pad = document.querySelector(`.dp[data-id="${id}"][data-step="${step}"]`);
  if (pad) { pad.classList.add('playing'); setTimeout(() => pad.classList.remove('playing'), 80); }
}

function tapTempo() {
  const now = Date.now();
  tapTimes.push(now);
  if (tapTimes.length > 6) tapTimes.shift();
  if (tapTimes.length >= 2) {
    const avgs = [];
    for (let i=1;i<tapTimes.length;i++) avgs.push(tapTimes[i]-tapTimes[i-1]);
    setBpm(Math.round(60000/(avgs.reduce((a,b)=>a+b,0)/avgs.length)));
  }
  const b = document.getElementById('tapBtn');
  b.classList.add('flash'); setTimeout(()=>b.classList.remove('flash'),120);
}
function startPhTimer() { if(phTimer)clearInterval(phTimer); phTimer=setInterval(()=>{ playheadBeat+=(bpm/60/10); updatePosDisplay(); },100); }
function stopPhTimer() { clearInterval(phTimer); }
function updatePosDisplay() {
  const bar=Math.floor(playheadBeat/timeSig)+1;
  const beat=Math.floor(playheadBeat%timeSig)+1;
  const ms=Math.floor((playheadBeat*1000)%1000);
  document.getElementById('posDisplay').textContent=`${bar} : ${beat} : ${String(ms).padStart(3,'0')}`;
}
function setStatus(s) {
  document.getElementById('sbDot').className = 'sb-dot ' + s;
  document.getElementById('sbTxt').textContent = ({ready:lang==='ko'?'준비':'Ready', playing:lang==='ko'?'재생 중':'Playing', rec:lang==='ko'?'녹음 중':'Recording'})[s]||s;
}
function startClock() { setInterval(()=>{ document.getElementById('sbClock').textContent=new Date().toLocaleTimeString(); },1000); }

// ── TABS ─────────────────────────────────────────────────────────────────
function switchTab(name) {
  document.querySelectorAll('.tab-view').forEach(v=>v.classList.remove('active'));
  document.querySelectorAll('.sb-item').forEach(s=>s.classList.remove('active'));
  const v = document.getElementById('tab-'+name);
  if(v) v.classList.add('active');
  const s = document.getElementById('sbi-'+name);
  if(s) s.classList.add('active');
  if(name==='melody') setTimeout(()=>{ if(!melCanvasEl) buildMelodyCanvas(); }, 30);
}

// ── ARRANGE / TRACKS ─────────────────────────────────────────────────────
function addTrack(type) {
  const id = ++trackId;
  const color = TRACK_COLORS[(trackId-1)%TRACK_COLORS.length];
  const names = {synth:{ko:'신스',en:'Synth'}, drum:{ko:'드럼',en:'Drum'}, bass:{ko:'베이스',en:'Bass'}, pad:{ko:'패드',en:'Pad'}, sample:{ko:'샘플',en:'Sample'}};
  tracks.push({id, type, name:(names[type][lang]||type)+' '+id, color, muted:false, soloed:false, vol:80, pan:0, clips:[]});
  document.getElementById('arrangeEmpty').style.display='none';
  renderTracks();
  addMixerChannel(tracks[tracks.length-1]);
}
function renderTracks() {
  const area = document.getElementById('trackArea');
  const rows = area.querySelectorAll('.track-row');
  rows.forEach(r=>r.remove());
  tracks.forEach(tr=>{
    const row = document.createElement('div');
    row.className = 'track-row';
    row.innerHTML = `
      <div class="track-colorbar" style="background:${tr.color}"></div>
      <div class="track-hd">
        <div class="track-name">${tr.name}</div>
        <div class="track-ctls">
          <button class="tc ${tr.muted?'muted':''}" onclick="toggleMute(${tr.id})" title="Mute">M</button>
          <button class="tc ${tr.soloed?'soloed':''}" onclick="toggleSolo(${tr.id})" title="Solo">S</button>
          <input type="range" min="0" max="100" value="${tr.vol}" style="width:48px;accent-color:var(--c-lv3)" oninput="setTrackVol(${tr.id},+this.value)">
          <button class="tc" onclick="removeTrack(${tr.id})" title="Remove">✕</button>
        </div>
      </div>
      <div class="track-clips" id="clips-${tr.id}">
        ${tr.clips.map(c=>`<div class="clip" style="background:${tr.color}55;border-color:${tr.color}">${c.name}</div>`).join('')}
        <div class="clip clip-add" onclick="addClip(${tr.id})">+</div>
      </div>`;
    area.appendChild(row);
  });
  if(tracks.length===0) document.getElementById('arrangeEmpty').style.display='';
}
function toggleMute(id){const t=tracks.find(x=>x.id===id);if(t){t.muted=!t.muted;renderTracks();}}
function toggleSolo(id){const t=tracks.find(x=>x.id===id);if(t){t.soloed=!t.soloed;renderTracks();}}
function setTrackVol(id,v){const t=tracks.find(x=>x.id===id);if(t)t.vol=v;}
function removeTrack(id){tracks=tracks.filter(t=>t.id!==id);renderTracks();removeMixerChannel(id);}
function addClip(id){const t=tracks.find(x=>x.id===id);if(t){t.clips.push({name:(lang==='ko'?'클립':'Clip')+' '+(t.clips.length+1)});renderTracks();}}
function clearArrange(){tracks=[];trackId=0;renderTracks();document.getElementById('mixerArea').innerHTML=`<div class="empty-state"><div class="es-icon">🎚</div><div class="es-text" id="es_mixer">${lang==='ko'?'Arrange에서 트랙을 추가하세요':'Add tracks in Arrange'}</div></div>`;}
function buildRuler(){const r=document.getElementById('rulerStrip');r.innerHTML='';for(let i=1;i<=16;i++){const m=document.createElement('div');m.className='ruler-mark'+(i%4===1?' bar':'');m.textContent='Bar '+i;r.appendChild(m);}}

// ── MIXER ─────────────────────────────────────────────────────────────────
function addMixerChannel(tr) {
  const area = document.getElementById('mixerArea');
  const empty = area.querySelector('.empty-state');
  if(empty) empty.remove();
  const ch = document.createElement('div');
  ch.className = 'mix-ch'; ch.id = 'mix-'+tr.id;
  ch.innerHTML = `
    <div class="mix-ch-name" title="${tr.name}">${tr.name}</div>
    <div style="width:8px;height:8px;border-radius:50%;background:${tr.color}"></div>
    <div class="mix-fader-wrap">
      <input type="range" class="mix-fader" min="0" max="100" value="${tr.vol}" orient="vertical"
        oninput="setTrackVol(${tr.id},+this.value);this.nextElementSibling.textContent=this.value">
      <div class="mix-val">${tr.vol}</div>
    </div>
    <div style="display:flex;align-items:center;gap:2px">
      <span style="font-size:8px;color:var(--c-txt3);font-family:var(--f-mo)">PAN</span>
      <input type="range" class="mix-pan" min="-100" max="100" value="0" style="width:54px">
    </div>
    <div class="mix-ms">
      <div class="mix-ms-btn M" onclick="toggleMute(${tr.id});this.classList.toggle('active')">M</div>
      <div class="mix-ms-btn S" onclick="toggleSolo(${tr.id});this.classList.toggle('active')">S</div>
    </div>
    <div class="mix-tiny-btn">FX Chain</div>
    <div class="mix-tiny-btn">Send A</div>`;
  area.appendChild(ch);
}
function removeMixerChannel(id){const el=document.getElementById('mix-'+id);if(el)el.remove();if(!document.querySelector('.mix-ch'))document.getElementById('mixerArea').innerHTML=`<div class="empty-state"><div class="es-icon">🎚</div><div class="es-text">${lang==='ko'?'Arrange에서 트랙을 추가하세요':'Add tracks'}</div></div>`;}
function resetAllMixer(){tracks.forEach(t=>{t.vol=80;t.muted=false;t.soloed=false;});renderTracks();}
function exportMixdown(){showToast(lang==='ko'?'Mixdown 기능은 준비 중입니다.':'Mixdown coming soon!');}

// ── PIANO ROLL ────────────────────────────────────────────────────────────
function buildNoteColors() {
  const wrap = document.getElementById('noteColors');
  wrap.innerHTML = '';
  NOTE_COLORS.forEach((c,i) => {
    const d = document.createElement('div');
    d.className = 'nc-dot'+(i===0?' active':'');
    d.style.background = c;
    d.onclick = () => { rollNoteColor=c; document.querySelectorAll('.nc-dot').forEach(x=>x.classList.remove('active')); d.classList.add('active'); };
    wrap.appendChild(d);
  });
}
function buildPianoRoll() {
  rollOct = +document.getElementById('rollOctSel').value;
  rollBars = +document.getElementById('rollBarsSel').value;
  rollSnap = +document.getElementById('rollSnapSel').value;
  const startMidi = (rollOct+2)*12-1;
  const endMidi = (rollOct-1)*12;
  const totalRows = startMidi - endMidi + 1;
  const beatsTotal = rollBars * timeSig;
  const totalCells = beatsTotal * (rollSnap/4);
  rollCellW = Math.max(14, Math.floor(780/totalCells));
  rollCellH = 20;
  buildPianoKeys(startMidi, endMidi);
  buildBeatHeader(totalCells, beatsTotal);
  buildRollGrid(startMidi, totalRows, totalCells);
  renderRollNotes(startMidi, totalCells);
  buildVelLane(totalCells);
}
function buildPianoKeys(start, end) {
  const col = document.getElementById('pianoKeysCol'); col.innerHTML='';
  for (let m=start; m>=end; m--) {
    const pc=m%12, oct=Math.floor(m/12)-1, isB=IS_BLACK[pc];
    const k=document.createElement('div');
    k.className='pk '+(isB?'black':'white')+(pc===0?' cn':'');
    k.style.height=rollCellH+'px';
    k.textContent=pc===0?NOTES_S[pc]+oct:(isB?'':NOTES_S[pc]);
    k.onclick=()=>playTone(midiToFreq(m),ctx().currentTime,.3,.25,'triangle');
    col.appendChild(k);
  }
}
function buildBeatHeader(cells, beats) {
  const h=document.getElementById('rollBeatHeader'); h.innerHTML='';
  const barCells=cells/rollBars;
  for(let c=0;c<cells;c++){
    const bar=Math.floor(c/barCells), beat=Math.floor((c%barCells)/(rollSnap/4));
    const sub=c%(rollSnap/4);
    const d=document.createElement('div');
    d.className='rbh-cell'+(sub===0&&beat===0?' bar':'');
    d.style.cssText=`width:${rollCellW}px;min-width:${rollCellW}px;flex-shrink:0`;
    if(sub===0) d.textContent=beat===0?`${bar+1}:1`:`${beat+1}`;
    h.appendChild(d);
  }
}
function buildRollGrid(startMidi, rows, cells) {
  const cont=document.getElementById('rollGridContainer');
  cont.innerHTML='';
  cont.style.cssText=`position:relative;width:${cells*rollCellW}px;height:${rows*rollCellH}px`;
  const barCells=cells/rollBars;
  for(let r=0;r<rows;r++){
    const midi=startMidi-r, pc=midi%12, isB=IS_BLACK[pc];
    const row=document.createElement('div');
    row.className='rg-row'+(isB?' blk':'');
    row.style.cssText=`top:${r*rollCellH}px;width:${cells*rollCellW}px`;
    for(let c=0;c<cells;c++){
      const cell=document.createElement('div');
      cell.className='rg-cell'+(c%(rollSnap/4)===0?(c%barCells===0?' brs':' bs'):'');
      cell.style.cssText=`width:${rollCellW}px;min-width:${rollCellW}px;height:${rollCellH}px;display:inline-block`;
      cell.dataset.midi=midi; cell.dataset.cell=c;
      cell.onmousedown=e=>rollMouseDown(e,midi,c);
      cell.onmousemove=e=>rollMouseMove(e,midi,c);
      row.appendChild(cell);
    }
    cont.appendChild(row);
  }
  document.onmouseup=()=>{rollDrawing=false;rollDrawNote=null;};
}
function rollMouseDown(e,midi,cell){
  e.preventDefault();
  if(rollTool==='pencil'){
    const ex=rollNotes.find(n=>n.midi===midi&&cell>=n.cell&&cell<n.cell+n.len);
    if(ex){rollNotes=rollNotes.filter(n=>n!==ex);}
    else{const note={midi,cell,len:1,vel:+document.getElementById('velSl').value,color:rollNoteColor};rollNotes.push(note);rollDrawNote=note;rollDrawing=true;}
    const startMidi=(rollOct+2)*12-1;renderRollNotes(startMidi,rollBars*timeSig*(rollSnap/4));buildVelLane(rollBars*timeSig*(rollSnap/4));
  } else if(rollTool==='erase'){
    rollNotes=rollNotes.filter(n=>!(n.midi===midi&&cell>=n.cell&&cell<n.cell+n.len));
    const startMidi=(rollOct+2)*12-1;renderRollNotes(startMidi,rollBars*timeSig*(rollSnap/4));buildVelLane(rollBars*timeSig*(rollSnap/4));
  }
}
function rollMouseMove(e,midi,cell){
  if(!rollDrawing||!rollDrawNote||rollDrawNote.midi!==midi)return;
  if(cell>=rollDrawNote.cell){
    rollDrawNote.len=cell-rollDrawNote.cell+1;
    const startMidi=(rollOct+2)*12-1;renderRollNotes(startMidi,rollBars*timeSig*(rollSnap/4));
  }
}
function renderRollNotes(startMidi, cells) {
  const cont=document.getElementById('rollGridContainer');
  cont.querySelectorAll('.roll-note').forEach(n=>n.remove());
  rollNotes.forEach(n=>{
    const top=(startMidi-n.midi)*rollCellH;
    const left=n.cell*rollCellW;
    const w=n.len*rollCellW-1;
    const el=document.createElement('div');
    el.className='roll-note';
    el.style.cssText=`top:${top}px;left:${left}px;width:${w}px;height:${rollCellH-1}px;background:${n.color}cc;border-color:${n.color};color:${n.color}`;
    el.textContent=NOTES_S[n.midi%12];
    el.onmousedown=ev=>{ev.stopPropagation();if(rollTool==='erase'){rollNotes=rollNotes.filter(x=>x!==n);renderRollNotes(startMidi,cells);buildVelLane(cells);}};
    cont.appendChild(el);
  });
}
function buildVelLane(cells) {
  const bars=document.getElementById('velLaneBars'); bars.innerHTML='';
  for(let c=0;c<cells;c++){
    const note=rollNotes.find(n=>n.cell===c);
    const bar=document.createElement('div');
    bar.className='vel-bar';
    bar.style.cssText=`width:${Math.max(6,rollCellW-2)}px;height:${note?note.vel/127*54+4:4}px`;
    bars.appendChild(bar);
  }
}
function setRollTool(t,el){rollTool=t;document.querySelectorAll('.rt-btn').forEach(b=>b.classList.remove('active'));el.classList.add('active');}
function quantizeNotes(){const sub=rollSnap/4;rollNotes.forEach(n=>n.cell=Math.round(n.cell/sub)*sub);const s=(rollOct+2)*12-1;renderRollNotes(s,rollBars*timeSig*(rollSnap/4));showToast('Quantized!');}
function clearRoll(){rollNotes=[];buildPianoRoll();}
function rebuildRoll(){buildPianoRoll();}
function playRoll(){
  const c=ctx(); const now=c.currentTime;
  const sub=rollSnap/4;
  const spb=60/bpm/sub;
  const inst=document.getElementById('rollInstSel').value;
  rollNotes.forEach(n=>{
    const freq=midiToFreq(n.midi);
    playTone(freq,now+n.cell*spb,n.len*spb*.88,n.vel/127*.4,inst);
  });
  setStatus('playing');
  const maxCell=rollNotes.reduce((m,n)=>Math.max(m,n.cell+n.len),0);
  setTimeout(()=>setStatus('ready'),(maxCell*spb+.5)*1000);
}
function exportMidi() {
  // Generate simple MIDI-like text representation
  const lines = ['MeloMelo MIDI Export', `BPM: ${bpm}`, `Time Signature: ${timeSig}/4`, '---Notes---'];
  rollNotes.forEach(n=>{
    lines.push(`MIDI:${n.midi} Cell:${n.cell} Len:${n.len} Vel:${n.vel}`);
  });
  downloadText(lines.join('\n'), 'melomelo_piano.txt');
  showToast(lang==='ko'?'MIDI 데이터를 내보냈습니다':'MIDI exported!');
}

// ── DRUM MACHINE ──────────────────────────────────────────────────────────
function rebuildDrum() { drumSteps=+document.getElementById('drumStepsSel').value; buildDrumMachine(); }
function buildDrumMachine() {
  drumSteps = +document.getElementById('drumStepsSel').value;
  DRUM_INST.forEach(d=>{ if(!drumSeq[d.id]) drumSeq[d.id]=new Array(drumSteps).fill(0); });
  // Beat header
  const header = document.getElementById('drumBeatNums'); header.innerHTML='';
  for(let i=0;i<drumSteps;i++){
    const d=document.createElement('div');
    d.className='dbn'+(i%4===0?' strong':'');
    d.textContent=i+1; header.appendChild(d);
  }
  // Rows
  const rows = document.getElementById('drumRows'); rows.innerHTML='';
  DRUM_INST.forEach(d=>{
    const row=document.createElement('div'); row.className='drum-row-wrap';
    const lbl=document.createElement('div'); lbl.className='drum-row-label';
    lbl.textContent=lang==='ko'?d.ko:d.en; row.appendChild(lbl);
    const pads=document.createElement('div'); pads.className='drum-row-pads';
    for(let i=0;i<drumSteps;i++){
      const pad=document.createElement('div');
      pad.className='dp'+(i%4===0&&i>0?' sep':'')+(drumSeq[d.id][i]?' on-'+d.id:'');
      pad.dataset.id=d.id; pad.dataset.step=i;
      pad.onclick=()=>{
        drumSeq[d.id][i]=drumSeq[d.id][i]?0:1;
        pad.className='dp'+(i%4===0&&i>0?' sep':'')+(drumSeq[d.id][i]?' on-'+d.id:'');
        if(drumSeq[d.id][i] && drumPreviewEnabled) playDrum(d.id, ctx().currentTime);
      };
      pads.appendChild(pad);
    }
    row.appendChild(pads);
    // Per-track volume
    const vwrap=document.createElement('div'); vwrap.className='drum-row-vol';
    vwrap.innerHTML=`<input type="range" min="0" max="100" value="80" class="drv" title="${lang==='ko'?'볼륨':'Volume'}">`;
    row.appendChild(vwrap);
    rows.appendChild(row);
  });
}
function clearDrum(){DRUM_INST.forEach(d=>drumSeq[d.id]=new Array(drumSteps).fill(0));buildDrumMachine();}
function loadDrumPreset(name){const p=DRUM_PRESETS[name];DRUM_INST.forEach(d=>drumSeq[d.id]=[...(p[d.id]||new Array(drumSteps).fill(0))]);buildDrumMachine();showToast((lang==='ko'?'프리셋 로드: ':'Preset loaded: ')+name);}
function playDrum_all(){
  const c=ctx(); const now=c.currentTime;
  const spb=60/bpm/4;
  DRUM_INST.forEach(d=>{
    for(let i=0;i<drumSteps;i++){
      if(drumSeq[d.id][i]){
        const jitter=humanizeAmt>0?(Math.random()-.5)*humanizeAmt/1000:0;
        let t=now+i*spb+jitter;
        if(swingAmt>0&&i%2===1)t+=spb*swingAmt/200;
        playDrum(d.id,Math.max(now,t));
      }
    }
  });
  setStatus('playing');
  setTimeout(()=>setStatus('ready'),(drumSteps*spb+.3)*1000);
}
// Alias
const playDrumOneShot = playDrum_all;
document.addEventListener('DOMContentLoaded',()=>{
  const btn=document.getElementById('ta_dplay');
  if(btn) btn.onclick=playDrum_all;
});
function exportDrumMidi(){
  const lines=['MeloMelo Drum Export',`BPM: ${bpm}`,'---Pattern---'];
  DRUM_INST.forEach(d=>{
    const steps=drumSeq[d.id]||[];
    lines.push(`${d.en}: ${steps.map(v=>v?'X':'.').join('')}`);
  });
  downloadText(lines.join('\n'),'melomelo_drums.txt');
  showToast(lang==='ko'?'드럼 패턴을 내보냈습니다':'Drum pattern exported!');
}

// ── CHORD BUILDER ─────────────────────────────────────────────────────────
function buildChordUI(){buildKeyBtns();buildModeBtns();buildExtBtns();buildDiatonicGrid();buildChordPresets();renderProgression();}
function buildKeyBtns(){const g=document.getElementById('keyBtnGrid');g.innerHTML='';NOTES_S.forEach((n,i)=>{const b=document.createElement('button');b.className='kb-btn'+(i===chordKey?' active':'');b.textContent=n;b.onclick=()=>{chordKey=i;buildChordUI();};g.appendChild(b);});}
function buildModeBtns(){const g=document.getElementById('modeBtnGrid');g.innerHTML='';Object.entries(SCALES).filter(([k])=>k!=='chromatic'&&k!=='pentatonic'&&k!=='blues').forEach(([key,sc])=>{const b=document.createElement('button');b.className='mode-btn'+(key===chordMode?' active':'');b.textContent=lang==='ko'?sc.ko:sc.en;b.onclick=()=>{chordMode=key;buildChordUI();};g.appendChild(b);});}
function buildExtBtns(){const g=document.getElementById('extBtnRow');g.innerHTML='';['triad','7th','9th','sus2','sus4','add9'].forEach(e=>{const b=document.createElement('button');b.className='ext-btn'+(e===chordExt?' active':'');b.textContent=e;b.onclick=()=>{chordExt=e;b.parentNode.querySelectorAll('.ext-btn').forEach(x=>x.classList.remove('active'));b.classList.add('active');chordExt=e;};g.appendChild(b);});}
function setInversion(inv,el){chordInv=inv;document.querySelectorAll('#tab-chord .ext-btn[id^=inv]').forEach(b=>b.classList.remove('active'));el.classList.add('active');}
function buildDiatonicGrid(){
  const g=document.getElementById('diatonicGrid');g.innerHTML='';
  const sc=SCALES[chordMode]||SCALES.major;
  const degs=CHORD_DEGS[chordMode]||CHORD_DEGS.major;
  const ivs=sc.iv;
  degs.forEach((deg,i)=>{
    if(i>=ivs.length)return;
    const root=(chordKey+ivs[i])%12;
    const chip=document.createElement('div');
    chip.className='dchip '+getRomanClass(deg);
    const {third,fifth}=getChordNotes(i,ivs,root);
    chip.innerHTML=`<div class="dchip-name">${NOTES_S[root]}</div><div class="dchip-roman">${deg}</div>`;
    chip.title=`${NOTES_S[root]}-${NOTES_S[third]}-${NOTES_S[fifth]}`;
    chip.onclick=()=>{
      chordProg.push({deg:i,root,third,fifth,label:deg});
      renderProgression();
      highlightMiniKb([root,third,fifth]);
      playChordNow(root,third,fifth);
    };
    g.appendChild(chip);
  });
}
function getChordNotes(deg,ivs,root){
  const third=(chordKey+ivs[(deg+2)%ivs.length])%12;
  const fifth=(chordKey+ivs[(deg+4)%ivs.length])%12;
  return{third,fifth};
}
function getRomanClass(l){const m={'I':'I','ii':'ii','iii':'iii','IV':'IV','V':'V','vi':'vi','vii°':'vii','i':'I','ii°':'ii','III':'iii','iv':'IV','v':'V','VI':'vi','VII':'vii'};return m[l]||'I';}
function playChordNow(root,third,fifth){const c=ctx();const now=c.currentTime;[root,third,fifth].forEach((pc,i)=>{const midi=60+pc+(pc<root?12:0);const freq=midiToFreq(midi);playTone(freq,now,.7,.12,'sine');});}
function buildChordPresets(){
  const g=document.getElementById('presetBtnGrid');g.innerHTML='';
  const presets=[
    {ko:'팝 I-V-vi-IV',en:'Pop I-V-vi-IV',prog:[0,4,5,3]},
    {ko:'재즈 ii-V-I',en:'Jazz ii-V-I',prog:[1,4,0]},
    {ko:'클래식 I-IV-V',en:'Classic I-IV-V',prog:[0,3,4]},
    {ko:'슬픔 vi-IV-I-V',en:'Sad vi-IV-I-V',prog:[5,3,0,4]},
    {ko:'I-vi-IV-V',en:'I-vi-IV-V',prog:[0,5,3,4]},
    {ko:'재즈 I-VI-ii-V',en:'Jazz I-VI-ii-V',prog:[0,5,1,4]},
    {ko:'왈츠 I-IV-V',en:'Waltz I-IV-V',prog:[0,3,4]},
    {ko:'블루스 I-IV-I-V',en:'Blues I-IV-I-V',prog:[0,3,0,4]},
  ];
  presets.forEach(p=>{
    const b=document.createElement('button');b.className='prs-btn';
    b.textContent=lang==='ko'?p.ko:p.en;
    b.onclick=()=>{
      const sc=SCALES[chordMode]||SCALES.major;const ivs=sc.iv;const degs=CHORD_DEGS[chordMode]||CHORD_DEGS.major;
      chordProg=p.prog.filter(i=>i<ivs.length).map(i=>{const root=(chordKey+ivs[i])%12;const{third,fifth}=getChordNotes(i,ivs,root);return{deg:i,root,third,fifth,label:degs[i]||'?'};});
      renderProgression();
    };
    g.appendChild(b);
  });
}
function renderProgression(){
  const row=document.getElementById('progressionRow');row.innerHTML='';
  const info=document.getElementById('progInfo');
  if(!chordProg.length){row.innerHTML=`<span class="prog-empty-hint" id="prog_hint">${lang==='ko'?'코드를 클릭해서 추가하세요':'Click chords to add'}</span>`;info.textContent='';return;}
  chordProg.forEach((ch,idx)=>{
    const slot=document.createElement('div');slot.className='prog-slot '+getRomanClass(ch.label);
    let bg='var(--c-lv)';let col='var(--c-lv3)';
    const rc=getRomanClass(ch.label);
    if(rc==='I')bg='var(--c-pk)',col='var(--c-pk3)';
    else if(rc==='IV')bg='var(--c-lv)',col='var(--c-lv3)';
    else if(rc==='V')bg='var(--c-mn)',col='var(--c-mn3)';
    else if(rc==='vi')bg='var(--c-co)',col='var(--c-co3)';
    else if(rc==='ii')bg='var(--c-yw)',col='var(--c-yw3)';
    slot.style.cssText=`background:${bg};color:${col};border:1.5px solid ${col}`;
    slot.textContent=NOTES_S[ch.root]+' '+ch.label;
    slot.onclick=()=>{chordProg.splice(idx,1);renderProgression();};
    row.appendChild(slot);
  });
  info.innerHTML=chordProg.map(ch=>`<b>${NOTES_S[ch.root]}${ch.label}</b>: ${NOTES_S[ch.root]}-${NOTES_S[ch.third]}-${NOTES_S[ch.fifth]}`).join(' → ');
  buildMiniKb();
}
function buildMiniKb(){
  const kb=document.getElementById('miniKb');kb.innerHTML='';kb.style.position='relative';
  const whites=[];let pos=0;
  for(let i=0;i<24;i++){const pc=i%12;if(!IS_BLACK[pc]){whites.push({pc,idx:i,pos:pos});pos++;}}
  kb.style.height='58px';
  whites.forEach((w,wi)=>{
    const k=document.createElement('div');k.className='mk-white';
    k.style.cssText=`position:absolute;left:${wi*18}px;top:4px;width:16px;height:48px;background:var(--c-wh);border:1px solid var(--c-brd);border-radius:0 0 3px 3px;cursor:pointer;z-index:1`;
    k.onclick=()=>{const midi=60+w.idx;playTone(midiToFreq(midi),ctx().currentTime,.4,.18,'sine');};
    if(chordProg.length>0){const last=chordProg[chordProg.length-1];if([last.root,last.third,last.fifth].includes(w.pc))k.classList.add('mk-active');}
    kb.appendChild(k);
  });
  kb.style.width=(whites.length*18+2)+'px';
}
function highlightMiniKb(pcs){buildMiniKb();}
function playChords(){
  if(!chordProg.length)return;const c=ctx();const now=c.currentTime;const dur=60/bpm*2;
  chordProg.forEach((ch,i)=>{[ch.root,ch.third,ch.fifth].forEach(pc=>{const freq=midiToFreq(60+pc);playTone(freq,now+i*dur,dur*.88,.12,'sine');});});
  setStatus('playing');setTimeout(()=>setStatus('ready'),(chordProg.length*dur+.5)*1000);
}
function clearChords(){chordProg=[];renderProgression();}
function shuffleProg(){for(let i=chordProg.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[chordProg[i],chordProg[j]]=[chordProg[j],chordProg[i]];}renderProgression();}
function reverseProg(){chordProg.reverse();renderProgression();}
function exportChordsTxt(){const txt=chordProg.map(c=>NOTES_S[c.root]+' '+c.label).join(' - ');downloadText(`MeloMelo Chord Progression\nBPM: ${bpm}\nKey: ${NOTES_S[chordKey]} ${chordMode}\n\n${txt}`,'melomelo_chords.txt');showToast(lang==='ko'?'코드 진행을 내보냈습니다':'Chords exported!');}

// ── MELODY CANVAS ─────────────────────────────────────────────────────────
function buildMelColorDots(){const w=document.getElementById('melColorDots');w.innerHTML='';MEL_COLORS.forEach((c,i)=>{const d=document.createElement('div');d.className='mcd'+(i===0?' active':'');d.style.background=c;d.onclick=()=>{melColor=c;document.querySelectorAll('.mcd').forEach(x=>x.classList.remove('active'));d.classList.add('active');};w.appendChild(d);});}
function buildMelodyCanvas(){
  melCanvasEl=document.getElementById('melCanvas');
  if(!melCanvasEl)return;
  melCanvasEl.width=melCols*melCellW;
  melCanvasEl.height=melRows*melCellH;
  melCtx2=melCanvasEl.getContext('2d');
  buildMelPitchCol();buildMelRulerRow();drawMelody();
  melCanvasEl.onmousedown=e=>{
    e.preventDefault();
    if(e.button===2){const{col,row}=getMelCell(e);melNotes=melNotes.filter(n=>!(n.row===row&&col>=n.col&&col<n.col+n.len));drawMelody();return;}
    const{col,row}=getMelCell(e);
    if(melTool==='draw'){
      melDrawing=true;
      const ex=melNotes.find(n=>n.row===row&&col>=n.col&&col<n.col+n.len);
      if(!ex){const note={row,col,len:1,color:melColor};melNotes.push(note);melDragNote=note;}
      else melDragNote=ex;
      drawMelody();updateMelInfo(row,col);
    } else if(melTool==='erase'){melNotes=melNotes.filter(n=>!(n.row===row&&col>=n.col&&col<n.col+n.len));drawMelody();}
  };
  melCanvasEl.onmousemove=e=>{
    if(!melDrawing||!melDragNote||melTool!=='draw')return;
    const{col}=getMelCell(e);
    if(col>=melDragNote.col){melDragNote.len=col-melDragNote.col+1;drawMelody();}
  };
  melCanvasEl.onmouseup=()=>{melDrawing=false;melDragNote=null;};
  melCanvasEl.onmouseleave=()=>{melDrawing=false;melDragNote=null;};
  melCanvasEl.oncontextmenu=e=>e.preventDefault();
}
function buildMelPitchCol(){
  const col=document.getElementById('melPitchCol');col.innerHTML='';
  const scaleKey=NOTES_S.indexOf(document.getElementById('melKeySel').value)||0;
  const scaleName=document.getElementById('melScaleSel').value;
  const sc=SCALES[scaleName]||SCALES.major;
  const inScaleNotes=sc.iv.map(iv=>(scaleKey+iv)%12);
  for(let r=0;r<melRows;r++){
    const midi=(melRows-1-r)+60;const pc=midi%12;const oct=Math.floor(midi/12)-1;
    const d=document.createElement('div');
    d.className='mel-pitch-lbl'+(pc===0?' cn':inScaleNotes.includes(pc)?' inscale':'');
    d.style.height=melCellH+'px';
    d.textContent=pc===0?NOTES_S[pc]+oct:pc===7?NOTES_S[pc]:'';
    col.appendChild(d);
  }
}
function buildMelRulerRow(){
  const row=document.getElementById('melRulerRow');row.innerHTML='';
  for(let i=0;i<melCols;i+=4){
    const d=document.createElement('div');
    d.className='mel-ruler-tick'+(i%16===0?' bar':'');
    d.style.width=(4*melCellW)+'px';
    d.textContent=i%16===0?'Bar '+(i/16+1):(i/4+1)+'';
    row.appendChild(d);
  }
}
function getMelCell(e){const rect=melCanvasEl.getBoundingClientRect();const x=(e.clientX-rect.left)*(melCanvasEl.width/rect.width);const y=(e.clientY-rect.top)*(melCanvasEl.height/rect.height);return{col:Math.max(0,Math.min(melCols-1,Math.floor(x/melCellW))),row:Math.max(0,Math.min(melRows-1,Math.floor(y/melCellH)))};}
function drawMelody(){
  if(!melCtx2)return;
  const w=melCanvasEl.width,h=melCanvasEl.height;
  melCtx2.clearRect(0,0,w,h);
  const scaleKey=NOTES_S.indexOf(document.getElementById('melKeySel').value)||0;
  const scaleName=document.getElementById('melScaleSel').value;
  const sc=SCALES[scaleName]||SCALES.major;
  const inScale=sc.iv.map(iv=>(scaleKey+iv)%12);
  for(let r=0;r<melRows;r++){
    const pc=(melRows-1-r+60)%12;
    const isBlk=IS_BLACK[pc];
    melCtx2.fillStyle=isBlk?'#f0eaf8':(inScale.includes(pc)?'#fdfaff':'#f7f4fc');
    melCtx2.fillRect(0,r*melCellH,w,melCellH);
    melCtx2.strokeStyle='rgba(200,180,230,.25)';melCtx2.lineWidth=.5;
    melCtx2.strokeRect(0,r*melCellH,w,melCellH);
  }
  for(let c=0;c<melCols;c++){
    if(c%16===0){melCtx2.strokeStyle='rgba(139,109,212,.45)';melCtx2.lineWidth=1.5;}
    else if(c%4===0){melCtx2.strokeStyle='rgba(139,109,212,.25)';melCtx2.lineWidth=1;}
    else{melCtx2.strokeStyle='rgba(200,180,230,.15)';melCtx2.lineWidth=.5;}
    melCtx2.beginPath();melCtx2.moveTo(c*melCellW,0);melCtx2.lineTo(c*melCellW,h);melCtx2.stroke();
  }
  melNotes.forEach(n=>{
    melCtx2.fillStyle=n.color+'cc';
    melCtx2.beginPath();melCtx2.roundRect(n.col*melCellW+1,n.row*melCellH+1,n.len*melCellW-2,melCellH-2,3);melCtx2.fill();
    melCtx2.strokeStyle=n.color;melCtx2.lineWidth=1.5;melCtx2.stroke();
    if(n.len*melCellW>18){const midi=(melRows-1-n.row)+60;melCtx2.fillStyle=n.color;melCtx2.font='bold 8px DM Mono,monospace';melCtx2.fillText(NOTES_S[midi%12],n.col*melCellW+3,n.row*melCellH+melCellH/2+3);}
  });
  document.getElementById('melNoteCount').textContent=`${melNotes.length} ${lang==='ko'?'음표':'notes'}`;
}
function updateMelInfo(row,col){const midi=(melRows-1-row)+60;const pc=midi%12;const oct=Math.floor(midi/12)-1;document.getElementById('melNoteInfo').textContent=`${NOTES_S[pc]}${oct} · cell ${col+1}`;}
function updateMelCanvas(){buildMelPitchCol();drawMelody();}
function setMelTool(t,el){melTool=t;document.querySelectorAll('.mel-tool-btn').forEach(b=>b.classList.remove('active'));el.classList.add('active');if(melCanvasEl)melCanvasEl.style.cursor=t==='draw'?'crosshair':t==='erase'?'cell':'default';}
function clearMelody(){melNotes=[];drawMelody();}
function playMelody(){
  if(!melNotes.length)return;
  const c=ctx();const now=c.currentTime;const spb=60/bpm/4;
  melNotes.forEach(n=>{const midi=(melRows-1-n.row)+60;const freq=midiToFreq(midi);playTone(freq,now+n.col*spb,n.len*spb*.85,.2,'triangle');});
  setStatus('playing');const maxCol=melNotes.reduce((m,n)=>Math.max(m,n.col+n.len),0);setTimeout(()=>setStatus('ready'),(maxCol*spb+.5)*1000);
}
function exportMelodyMidi(){
  const lines=['MeloMelo Melody Export',`BPM: ${bpm}`,'---Notes---'];
  melNotes.forEach(n=>{const midi=(melRows-1-n.row)+60;lines.push(`${NOTES_S[midi%12]}${Math.floor(midi/12)-1} Cell:${n.col} Len:${n.len}`);});
  downloadText(lines.join('\n'),'melomelo_melody.txt');
  showToast(lang==='ko'?'멜로디를 내보냈습니다':'Melody exported!');
}

// ── ARPEGGIATOR ────────────────────────────────────────────────────────────
function buildArp(){
  const body=document.getElementById('arpBody');body.innerHTML='';
  body.innerHTML=`<div class="arp-grid" style="padding:16px">
    <div class="arp-card">
      <div class="arp-card-title">PATTERN</div>
      <div class="arp-pat-grid" id="arpPatGrid"></div>
    </div>
    <div class="arp-card">
      <div class="arp-card-title">SETTINGS</div>
      <div class="arp-ctrl"><span class="arp-lbl">${lang==='ko'?'속도':'Rate'}</span>
        <select class="ta-sel" id="arpRateSel"><option value="4">1/4</option><option value="8" selected>1/8</option><option value="16">1/16</option><option value="32">1/32</option></select></div>
      <div class="arp-ctrl"><span class="arp-lbl">${lang==='ko'?'옥타브':'Octaves'}</span>
        <select class="ta-sel" id="arpOctSel"><option value="1" selected>1</option><option value="2">2</option><option value="3">3</option></select></div>
      <div class="arp-ctrl"><span class="arp-lbl">Gate %</span>
        <input type="range" min="10" max="100" value="80" id="arpGateSl" class="rt-slider" oninput="arpGate=+this.value"></div>
      <div class="arp-ctrl"><span class="arp-lbl">${lang==='ko'?'악기':'Instrument'}</span>
        <select class="ta-sel" id="arpInstSel"><option value="triangle" selected>Triangle</option><option value="sine">Sine</option><option value="sawtooth">Sawtooth</option><option value="square">Square</option></select></div>
    </div>
    <div class="arp-card" style="grid-column:1/-1">
      <div class="arp-card-title">${lang==='ko'?'음표 선택':'NOTE SELECTION'}</div>
      <div class="arp-keys" id="arpKeysWrap"></div>
    </div>
  </div>`;
  const patterns=['Up','Down','Up-Down','Random','Chord'];
  const pg=document.getElementById('arpPatGrid');
  patterns.forEach((p,i)=>{const b=document.createElement('button');b.className='arp-pat-btn'+(i===0?' active':'');b.textContent=p;b.onclick=()=>{arpPattern=p.toLowerCase().replace('-','');document.querySelectorAll('.arp-pat-btn').forEach(x=>x.classList.remove('active'));b.classList.add('active');};pg.appendChild(b);});
  const kw=document.getElementById('arpKeysWrap');
  NOTES_S.forEach((n,i)=>{const b=document.createElement('button');b.className='arp-key';b.textContent=n+'4';b.onclick=()=>{const midi=60+i;b.classList.toggle('active');if(b.classList.contains('active'))arpKeys.push(midi);else arpKeys=arpKeys.filter(k=>k!==midi);};kw.appendChild(b);});
}
let arpRunning=false;
function toggleArp(){
  arpRunning=!arpRunning;
  const btn=document.getElementById('ta_arpplay');
  if(arpRunning){btn.textContent='⏹ Stop';btn.classList.add('ta-btn-danger');runArp();}
  else{btn.textContent='▶ Start';btn.classList.remove('ta-btn-danger');if(arpTimer)clearTimeout(arpTimer);}
}
function runArp(){
  if(!arpRunning)return;
  const keys=arpKeys.length?arpKeys:NOTES_S.map((_,i)=>60+i);
  const inst=document.getElementById('arpInstSel')?.value||'triangle';
  const rate=+document.getElementById('arpRateSel')?.value||8;
  const spb=60/bpm/rate*1000;
  const midi=keys[arpStep%keys.length];
  const freq=midiToFreq(midi);
  playTone(freq,ctx().currentTime,spb/1000*(arpGate/100),.22,inst);
  arpStep++;
  arpTimer=setTimeout(runArp,spb);
}

// ── SCALE HELPER ───────────────────────────────────────────────────────────
let scaleKey2 = 0;
function buildScaleHelper(){
  const body=document.getElementById('scaleBody');body.innerHTML='';
  const keyRow=document.createElement('div');keyRow.className='scale-key-row';keyRow.style.padding='14px 16px 0';
  NOTES_S.forEach((n,i)=>{const b=document.createElement('button');b.className='kb-btn'+(i===scaleKey2?' active':'');b.textContent=n;b.onclick=()=>{scaleKey2=i;document.querySelectorAll('.scale-key-row .kb-btn').forEach(x=>x.classList.remove('active'));b.classList.add('active');buildScaleHelper();};keyRow.appendChild(b);});
  body.appendChild(keyRow);
  const grid=document.createElement('div');grid.className='scale-cards-grid';grid.style.padding='14px 16px';
  Object.entries(SCALES).forEach(([key,sc])=>{
    const all=NOTES_S.map((n,i)=>({n,i,in:sc.iv.includes(((i-scaleKey2)+12)%12)}));
    const card=document.createElement('div');card.className='scale-card';
    card.innerHTML=`<div class="sc-title">${NOTES_S[scaleKey2]} ${lang==='ko'?sc.ko:sc.en}</div>
      <div class="sc-intervals">${lang==='ko'?'음정:':'Intervals:'} ${sc.iv.join('-')}</div>
      <div class="sc-notes">${all.map(x=>`<span class="scn ${x.i===scaleKey2?'root':x.in?'in':'out'}">${x.n}</span>`).join('')}</div>`;
    card.onclick=()=>{const c2=ctx();const now=c2.currentTime;sc.iv.forEach((iv,i)=>{const midi=60+(scaleKey2+iv)%12;playTone(midiToFreq(midi),now+i*.18,.4,.14,'triangle');});};
    grid.appendChild(card);
  });
  body.appendChild(grid);
}

// ── EQUALIZER ───────────────────────────────────────────────────────────────
function buildEQ(){
  const body=document.getElementById('eqBody');body.innerHTML='';
  const canvasWrap=document.createElement('div');canvasWrap.className='eq-canvas-wrap';
  const canvas=document.createElement('canvas');canvas.id='eqCanvas';canvas.width=600;canvas.height=180;
  canvasWrap.appendChild(canvas);body.appendChild(canvasWrap);
  const bands=document.createElement('div');bands.className='eq-bands';
  EQ_BANDS.forEach((freq,i)=>{
    const band=document.createElement('div');band.className='eq-band';
    band.innerHTML=`<div class="eq-band-freq">${freq>=1000?freq/1000+'k':freq}Hz</div>
      <input type="range" class="eq-band-fader" min="-18" max="18" value="0" orient="vertical"
        oninput="eqGains[${i}]=+this.value;this.nextElementSibling.textContent=this.value+'dB';drawEQCurve()">
      <div class="eq-band-gain">0dB</div>
      <div class="eq-band-type">${i===0?'LPF':i===EQ_BANDS.length-1?'HPF':'PEQ'}</div>`;
    bands.appendChild(band);
  });
  body.appendChild(bands);
  drawEQCurve();
}
function drawEQCurve(){
  const canvas=document.getElementById('eqCanvas');if(!canvas)return;
  const ctx2=canvas.getContext('2d');const w=canvas.width,h=canvas.height;
  ctx2.clearRect(0,0,w,h);
  // Background grid
  ctx2.fillStyle='#f5f0fc';ctx2.fillRect(0,0,w,h);
  ctx2.strokeStyle='rgba(180,150,230,.25)';ctx2.lineWidth=1;
  for(let y=0;y<=h;y+=h/6){ctx2.beginPath();ctx2.moveTo(0,y);ctx2.lineTo(w,y);ctx2.stroke();}
  for(let x=0;x<=w;x+=w/10){ctx2.beginPath();ctx2.moveTo(x,0);ctx2.lineTo(x,h);ctx2.stroke();}
  // Draw curve
  ctx2.strokeStyle='#8b6dd4';ctx2.lineWidth=2.5;ctx2.beginPath();
  for(let x=0;x<w;x++){
    const logFreq=20*Math.pow(20000/20,x/w);
    let gain=0;
    EQ_BANDS.forEach((freq,i)=>{const diff=Math.abs(Math.log2(logFreq/freq));gain+=eqGains[i]*Math.max(0,1-diff);});
    const y=h/2-gain*(h/2/18);
    x===0?ctx2.moveTo(x,y):ctx2.lineTo(x,y);
  }
  ctx2.stroke();
  // Center line
  ctx2.strokeStyle='rgba(139,109,212,.3)';ctx2.lineWidth=1;ctx2.setLineDash([4,4]);
  ctx2.beginPath();ctx2.moveTo(0,h/2);ctx2.lineTo(w,h/2);ctx2.stroke();ctx2.setLineDash([]);
}
function resetEq(){eqGains=new Array(EQ_BANDS.length).fill(0);buildEQ();}
function previewEq(){showToast(lang==='ko'?'EQ 프리뷰 기능은 준비 중입니다.':'EQ preview coming soon!');}

// ── PROJECTS / SAVE/LOAD ────────────────────────────────────────────────────
function getProjectData(){
  return{name:currentProjectName,id:currentProjectId,bpm,timeSig,rollNotes,drumSeq,chordProg,chordKey,chordMode,melNotes,tracks,created:Date.now()};
}
function setProjectData(data){
  if(data.bpm)setBpm(data.bpm);
  if(data.timeSig)timeSig=data.timeSig;
  if(data.rollNotes)rollNotes=data.rollNotes;
  if(data.drumSeq)drumSeq=data.drumSeq;
  if(data.chordProg){chordProg=data.chordProg;renderProgression();}
  if(data.chordKey)chordKey=data.chordKey;
  if(data.chordMode)chordMode=data.chordMode;
  if(data.melNotes){melNotes=data.melNotes;drawMelody();}
  if(data.tracks){tracks=data.tracks;renderTracks();}
  currentProjectName=data.name||'프로젝트';
  currentProjectId=data.id||null;
  updateProjectName();
  buildPianoRoll();
  buildDrumMachine();
  buildChordUI();
}
function updateProjectName(){document.getElementById('sbProject').textContent=currentProjectName;}

function doSaveNew(){
  const name=document.getElementById('saveProjName').value.trim()||'새 프로젝트';
  currentProjectName=name;currentProjectId='proj_'+Date.now();
  saveToStorage(currentProjectId,name);
  closeModal('saveModal');showToast((lang==='ko'?'저장됨: ':'Saved: ')+name);
  updateProjectName();renderProjects();
}
function doOverwrite(){
  if(!currentProjectId){doSaveNew();return;}
  const name=document.getElementById('saveProjName').value.trim()||currentProjectName;
  currentProjectName=name;
  saveToStorage(currentProjectId,name);
  closeModal('saveModal');showToast(lang==='ko'?'덮어쓰기 완료!':'Overwritten!');
  updateProjectName();renderProjects();
}
function saveToStorage(id,name){
  const data={...getProjectData(),name,id,saved:new Date().toLocaleString()};
  localStorage.setItem('mm_proj_'+id, JSON.stringify(data));
  // Update index
  const idx=JSON.parse(localStorage.getItem('mm_proj_index')||'[]');
  const ex=idx.findIndex(x=>x.id===id);
  if(ex>=0)idx[ex]={id,name,saved:data.saved};
  else idx.unshift({id,name,saved:data.saved});
  localStorage.setItem('mm_proj_index', JSON.stringify(idx));
}
function loadProject(id){
  const raw=localStorage.getItem('mm_proj_'+id);
  if(!raw){showToast(lang==='ko'?'프로젝트를 불러올 수 없습니다.':'Cannot load project.');return;}
  const data=JSON.parse(raw);
  setProjectData(data);
  closeModal('saveModal');
  showToast((lang==='ko'?'불러옴: ':'Loaded: ')+data.name);
}
function deleteProject(id){
  if(!confirm(lang==='ko'?'삭제하시겠습니까?':'Delete this project?'))return;
  localStorage.removeItem('mm_proj_'+id);
  const idx=JSON.parse(localStorage.getItem('mm_proj_index')||'[]').filter(x=>x.id!==id);
  localStorage.setItem('mm_proj_index',JSON.stringify(idx));
  renderProjects();renderSavedProjectList();
  showToast(lang==='ko'?'삭제됨':'Deleted');
}
function renderProjects(){
  const body=document.getElementById('projectsBody');if(!body)return;
  body.innerHTML='';
  const idx=JSON.parse(localStorage.getItem('mm_proj_index')||'[]');
  if(!idx.length){body.innerHTML=`<div class="proj-empty">${lang==='ko'?'저장된 프로젝트가 없습니다.':'No saved projects.'}</div>`;return;}
  const grid=document.createElement('div');grid.className='proj-cards';
  idx.forEach(p=>{
    const raw=localStorage.getItem('mm_proj_'+p.id);
    let data={};try{data=JSON.parse(raw||'{}')}catch(e){}
    const card=document.createElement('div');card.className='proj-card';
    card.innerHTML=`<div class="proj-card-name">${p.name||'프로젝트'}</div>
      <div class="proj-card-meta">${p.saved||''} · BPM ${data.bpm||120}</div>
      <div class="proj-card-tags"><span class="proj-tag" style="background:var(--c-lv);color:var(--c-lv3)">${data.rollNotes?.length||0} notes</span><span class="proj-tag" style="background:var(--c-pk);color:var(--c-pk3)">${data.tracks?.length||0} tracks</span></div>
      <div class="proj-card-actions">
        <button class="pca-btn" onclick="loadProject('${p.id}');switchTab('arrange')">📂 ${lang==='ko'?'열기':'Open'}</button>
        <button class="pca-btn" onclick="duplicateProject('${p.id}')">📋 ${lang==='ko'?'복제':'Duplicate'}</button>
        <button class="pca-btn del" onclick="deleteProject('${p.id}')">🗑 ${lang==='ko'?'삭제':'Delete'}</button>
      </div>`;
    grid.appendChild(card);
  });
  body.appendChild(grid);
}
function duplicateProject(id){
  const raw=localStorage.getItem('mm_proj_'+id);if(!raw)return;
  const data=JSON.parse(raw);
  const newId='proj_'+Date.now();
  data.id=newId;data.name=(data.name||'프로젝트')+' (Copy)';data.saved=new Date().toLocaleString();
  localStorage.setItem('mm_proj_'+newId,JSON.stringify(data));
  const idx=JSON.parse(localStorage.getItem('mm_proj_index')||'[]');
  idx.unshift({id:newId,name:data.name,saved:data.saved});
  localStorage.setItem('mm_proj_index',JSON.stringify(idx));
  renderProjects();showToast(lang==='ko'?'복제됨':'Duplicated!');
}
function newProject(){
  if(!confirm(lang==='ko'?'새 프로젝트를 시작하면 현재 작업이 초기화됩니다.':'Start a new project? Current work will be cleared.'))return;
  rollNotes=[];drumSeq={};chordProg=[];melNotes=[];tracks=[];trackId=0;currentProjectName='새 프로젝트';currentProjectId=null;
  buildPianoRoll();buildDrumMachine();buildChordUI();drawMelody();renderTracks();
  updateProjectName();showToast(lang==='ko'?'새 프로젝트 시작':'New project started');
}
function saveProject(){openSaveModal();}
function exportProject(){
  const data=getProjectData();
  downloadText(JSON.stringify(data,null,2), (currentProjectName||'project').replace(/\s+/g,'_')+'.json');
  showToast(lang==='ko'?'JSON으로 내보냈습니다':'Exported as JSON!');
}
function importProject(input){
  const file=input.files[0];if(!file)return;
  const reader=new FileReader();
  reader.onload=e=>{
    try{const data=JSON.parse(e.target.result);setProjectData(data);showToast(lang==='ko'?'가져오기 완료!':'Imported!');renderProjects();}
    catch(err){showToast(lang==='ko'?'파일 형식 오류':'Invalid file format');}
  };reader.readAsText(file);input.value='';
}
function clearAllData(){if(!confirm(lang==='ko'?'모든 데이터를 삭제하시겠습니까?':'Delete all data?'))return;localStorage.clear();location.reload();}

// ── MODALS ───────────────────────────────────────────────────────────────────
function openSaveModal(){
  document.getElementById('saveProjName').value=currentProjectName;
  renderSavedProjectList();
  document.getElementById('saveModal').classList.remove('hidden');
}
function renderSavedProjectList(){
  const list=document.getElementById('savedProjectList');list.innerHTML='';
  const idx=JSON.parse(localStorage.getItem('mm_proj_index')||'[]');
  if(!idx.length){list.innerHTML=`<div style="color:var(--c-txt3);font-size:12px;padding:10px">${lang==='ko'?'저장된 프로젝트 없음':'No saved projects'}</div>`;return;}
  idx.forEach(p=>{
    const item=document.createElement('div');item.className='sp-item';
    item.innerHTML=`<div><div class="sp-name">${p.name||'프로젝트'}</div><div class="sp-meta">${p.saved||''}</div></div>
      <button class="sp-load-btn" onclick="loadProject('${p.id}')">📂</button>
      <button class="sp-del-btn" onclick="deleteProject('${p.id}')">🗑</button>`;
    list.appendChild(item);
  });
}
function switchModalTab(name,el){
  document.querySelectorAll('.mtab').forEach(b=>b.classList.remove('active'));el.classList.add('active');
  document.getElementById('saveTab').classList.toggle('hidden',name!=='save');
  document.getElementById('loadTab').classList.toggle('hidden',name!=='load');
  if(name==='load')renderSavedProjectList();
}
function openSettings(){document.getElementById('settingsModal').classList.remove('hidden');}
function closeModal(id){document.getElementById(id).classList.add('hidden');}
// Close on backdrop click
document.addEventListener('click',e=>{if(e.target.classList.contains('modal-overlay'))e.target.classList.add('hidden');});

// ── LOOPS ─────────────────────────────────────────────────────────────────
const LOOPS=[
  {name:'Boom Bap Kit',bpm:90,key:'Am',tags:['Hip-Hop','Drum'],color:'var(--c-pk)',icon:'🥁'},
  {name:'Lo-Fi Chill',bpm:75,key:'Cm',tags:['Lo-Fi','Chill','Piano'],color:'var(--c-lv)',icon:'🎹'},
  {name:'Summer Pop',bpm:120,key:'G',tags:['Pop','Synth'],color:'var(--c-mn)',icon:'🎸'},
  {name:'Jazz Waltz',bpm:132,key:'Bbm',tags:['Jazz','Piano'],color:'var(--c-yw)',icon:'🎷'},
  {name:'EDM Drop',bpm:128,key:'Fm',tags:['EDM','Bass','Synth'],color:'var(--c-gn)',icon:'🎛'},
  {name:'Bossa Nova',bpm:110,key:'D',tags:['Bossa','Guitar'],color:'var(--c-co)',icon:'🎸'},
  {name:'Trap 808',bpm:140,key:'F#m',tags:['Trap','Drum','Bass'],color:'var(--c-pk)',icon:'🥁'},
  {name:'Ambient Pad',bpm:80,key:'C',tags:['Ambient','Synth','Chill'],color:'var(--c-lv)',icon:'🌙'},
  {name:'Funk Groove',bpm:100,key:'Gm',tags:['Funk','Bass','Guitar'],color:'var(--c-mn)',icon:'🎵'},
  {name:'R&B Melody',bpm:85,key:'Ebm',tags:['R&B','Piano'],color:'var(--c-yw)',icon:'🎼'},
  {name:'Drum & Bass',bpm:174,key:'Dm',tags:['DnB','Drum'],color:'var(--c-gn)',icon:'🥁'},
  {name:'Chillwave',bpm:95,key:'Ab',tags:['Chill','Synth'],color:'var(--c-co)',icon:'〰️'},
];
let activeLoopFilter='All';
function buildLoops(){
  const body=document.getElementById('loopsBody');body.innerHTML='';
  const filterRow=document.createElement('div');filterRow.className='loop-filter-row';filterRow.style.padding='14px 16px 0';
  const allTags=['All',...new Set(LOOPS.flatMap(l=>l.tags))];
  allTags.forEach(tag=>{const b=document.createElement('button');b.className='lf-tag'+(tag===activeLoopFilter?' active':'');b.textContent=tag;b.onclick=()=>{activeLoopFilter=tag;filterRow.querySelectorAll('.lf-tag').forEach(x=>x.classList.remove('active'));b.classList.add('active');renderLoops();};filterRow.appendChild(b);});
  body.appendChild(filterRow);
  const grid=document.createElement('div');grid.className='loop-cards';grid.id='loopCardsGrid';grid.style.padding='12px 16px 16px';
  body.appendChild(grid);renderLoops();
}
function renderLoops(){
  const grid=document.getElementById('loopCardsGrid');if(!grid)return;grid.innerHTML='';
  const filtered=activeLoopFilter==='All'?LOOPS:LOOPS.filter(l=>l.tags.includes(activeLoopFilter));
  filtered.forEach(loop=>{
    const card=document.createElement('div');card.className='loop-card';
    card.innerHTML=`<button class="lc-play" style="background:${loop.color}55;border:none;cursor:pointer" onclick="previewLoop('${loop.name}')">${loop.icon}</button>
      <div class="lc-info">
        <div class="lc-name">${loop.name}</div>
        <div class="lc-meta">${loop.bpm} BPM · ${loop.key}</div>
        <div class="lc-tags">${loop.tags.map(t=>`<span class="lc-tag" style="background:${loop.color}33;color:var(--c-txt2)">${t}</span>`).join('')}</div>
      </div>`;
    grid.appendChild(card);
  });
}
function previewLoop(name){
  const loop=LOOPS.find(l=>l.name===name);if(!loop)return;
  const c=ctx();const now=c.currentTime;const spb=60/loop.bpm/4;
  [0,4,7,12,16].forEach((s,i)=>playTone(440*Math.pow(2,s/12),now+i*spb*.8,.3,.1,'triangle'));
  showToast('▶ '+name);
}

// ── TUTORIAL ──────────────────────────────────────────────────────────────
const TUTS_KO=[
  {icon:'🎹',col:'var(--c-lv)',title:'피아노 롤',desc:'격자에 음표를 클릭해서 멜로디를 만들어요.',steps:['Piano Roll 탭 이동','옥타브와 바(Bars) 선택','격자 클릭으로 음표 배치','드래그로 길이 조절','▶ Play로 재생'],view:'piano'},
  {icon:'🥁',col:'var(--c-pk)',title:'드럼 머신',desc:'비트 패드를 눌러서 리듬을 만들어요.',steps:['Drum Machine 탭 이동','패드 클릭으로 비트 설정','프리셋으로 샘플 비트 로드','Swing/Humanize로 그루브 조절','▶ Play 재생'],view:'drum'},
  {icon:'🎸',col:'var(--c-mn)',title:'코드 빌더',desc:'키와 스케일을 선택하고 코드 진행을 만들어요.',steps:['Chord Builder 탭 이동','Key와 Mode 선택','다이아토닉 코드 클릭으로 추가','프리셋에서 유명 진행 로드','▶ Play 재생'],view:'chord'},
  {icon:'✏️',col:'var(--c-yw)',title:'멜로디 스케치',desc:'캔버스에 자유롭게 음표를 그려요.',steps:['Melody Sketch 탭 이동','스케일과 키 선택','클릭/드래그로 음표 그리기','우클릭으로 삭제','▶ Play 재생'],view:'melody'},
  {icon:'💾',col:'var(--c-gn)',title:'프로젝트 저장',desc:'작업을 저장하고 언제든지 불러올 수 있어요.',steps:['내비바 💾 버튼 클릭','프로젝트 이름 입력','새로 저장 또는 덮어쓰기','JSON으로 내보내기 가능','내 프로젝트에서 관리'],view:'projects'},
  {icon:'🔄',col:'var(--c-co)',title:'아르페지에이터',desc:'코드를 자동으로 분산해서 연주해요.',steps:['Arpeggiator 탭 이동','패턴 선택 (Up/Down/Random)','음표 선택','속도(Rate)와 Gate 조절','▶ Start 재생'],view:'arp'},
];
const TUTS_EN=[
  {icon:'🎹',col:'var(--c-lv)',title:'Piano Roll',desc:'Click grid cells to place notes and build melodies.',steps:['Go to Piano Roll tab','Choose octave and bars','Click to place notes','Drag right to extend','Press ▶ Play'],view:'piano'},
  {icon:'🥁',col:'var(--c-pk)',title:'Drum Machine',desc:'Click pads to build your rhythm pattern.',steps:['Go to Drum Machine tab','Click pads to set beats','Load presets for inspiration','Adjust Swing & Humanize','Press ▶ Play'],view:'drum'},
  {icon:'🎸',col:'var(--c-mn)',title:'Chord Builder',desc:'Pick a key and build chord progressions.',steps:['Go to Chord Builder tab','Select Key and Mode','Click diatonic chords to add','Load famous presets','Press ▶ Play'],view:'chord'},
  {icon:'✏️',col:'var(--c-yw)',title:'Melody Sketch',desc:'Draw notes freely on a canvas.',steps:['Go to Melody Sketch tab','Choose scale and key','Click/drag to draw notes','Right-click to delete','Press ▶ Play'],view:'melody'},
  {icon:'💾',col:'var(--c-gn)',title:'Saving Projects',desc:'Save your work and load it anytime.',steps:['Click 💾 in the navbar','Enter project name','Save new or overwrite','Export as JSON file','Manage in My Projects'],view:'projects'},
  {icon:'🔄',col:'var(--c-co)',title:'Arpeggiator',desc:'Automatically arpeggiate chord notes.',steps:['Go to Arpeggiator tab','Pick a pattern','Select notes','Adjust Rate and Gate','Press ▶ Start'],view:'arp'},
];
function buildTutorial(){
  const body=document.getElementById('tutorialBody');body.innerHTML='';
  const tuts=lang==='ko'?TUTS_KO:TUTS_EN;
  const note=document.createElement('p');note.style.cssText='padding:10px 16px;font-size:12px;color:var(--c-txt2)';
  note.textContent=lang==='ko'?'카드를 클릭하면 해당 기능으로 이동합니다.':'Click a card to navigate to that feature.';
  body.appendChild(note);
  const grid=document.createElement('div');grid.className='tut-cards';grid.style.padding='0 16px 16px';
  tuts.forEach(tut=>{
    const card=document.createElement('div');card.className='tut-card';
    card.innerHTML=`<div class="tut-card-top" style="background:${tut.col}33">${tut.icon}</div>
      <div class="tut-card-body">
        <div class="tut-card-title">${tut.title}</div>
        <div class="tut-card-desc">${tut.desc}</div>
        <ul class="tut-steps">${tut.steps.map((s,i)=>`<li class="tut-step"><div class="tut-snum" style="background:${tut.col}33;color:var(--c-txt2)">${i+1}</div><div>${s}</div></li>`).join('')}</ul>
      </div>`;
    card.onclick=()=>switchTab(tut.view);
    grid.appendChild(card);
  });body.appendChild(grid);
}

// ── GLOSSARY ──────────────────────────────────────────────────────────────
const GLOSS=[
  {t:'BPM',d:{ko:'Beats Per Minute. 1분당 박자 수로 곡의 빠르기를 나타냅니다.',en:'Beats Per Minute. Measures the tempo of music.'},tags:['기초','Basic']},
  {t:'피아노 롤 / Piano Roll',d:{ko:'음표를 격자 위에 시각적으로 배치하는 MIDI 편집 인터페이스.',en:'A visual MIDI editor where notes are placed on a grid.'},tags:['DAW']},
  {t:'옥타브 / Octave',d:{ko:'음높이가 두 배 차이나는 같은 음이름 사이의 간격. C4→C5는 1 옥타브.',en:'The interval between two notes of the same name, one an octave apart.'},tags:['이론','Theory']},
  {t:'코드 / Chord',d:{ko:'둘 이상의 음이 동시에 울리는 것. 3음 구성이 트라이어드(Triad).',en:'Two or more notes sounding simultaneously. Three-note chords are triads.'},tags:['화성','Harmony']},
  {t:'스케일 / Scale',d:{ko:'음악적 규칙에 따라 배열된 음의 집합. 장음계·단음계 등이 있음.',en:'A set of notes arranged by musical rules. Major, minor, etc.'},tags:['이론','Theory']},
  {t:'퀀타이즈 / Quantize',d:{ko:'음표의 타이밍을 정확한 박자로 맞춰주는 기능.',en:'Snapping notes to the nearest grid position to correct timing.'},tags:['DAW']},
  {t:'벨로시티 / Velocity',d:{ko:'MIDI에서 음표를 얼마나 세게 눌렀는지 나타내는 값 (0~127).',en:'A MIDI value (0-127) representing how hard a note is played.'},tags:['MIDI']},
  {t:'스윙 / Swing',d:{ko:'8분음표 타이밍을 불규칙하게 만들어 리듬감을 주는 효과.',en:'Intentional timing irregularity on off-beats for groove.'},tags:['리듬','Rhythm']},
  {t:'아르페지오 / Arpeggio',d:{ko:'코드의 음을 동시가 아닌 순서대로 연주하는 주법.',en:'Playing chord notes sequentially rather than simultaneously.'},tags:['연주법','Technique']},
  {t:'다이아토닉 / Diatonic',d:{ko:'특정 음계 안에서만 사용되는 음 또는 코드.',en:'Notes or chords that belong to a given key/scale.'},tags:['이론','Theory']},
  {t:'EQ / Equalizer',d:{ko:'특정 주파수 대역의 음량을 조절하는 오디오 처리 도구.',en:'An audio tool that adjusts the volume of specific frequency bands.'},tags:['믹싱','Mixing']},
  {t:'페이더 / Fader',d:{ko:'믹서에서 트랙의 음량을 조절하는 슬라이더.',en:'A slider in a mixer used to control track volume.'},tags:['믹싱','Mixing']},
  {t:'패닝 / Panning',d:{ko:'음원을 스테레오 공간에서 좌우로 배치하는 것.',en:'Placing a sound in the left-right stereo field.'},tags:['믹싱','Mixing']},
  {t:'루프 / Loop',d:{ko:'반복 재생되도록 만들어진 짧은 음악 패턴 또는 클립.',en:'A short audio or MIDI pattern designed to repeat seamlessly.'},tags:['DAW']},
  {t:'미디 / MIDI',d:{ko:'Musical Instrument Digital Interface. 전자악기 간 디지털 신호를 주고받는 표준 규격.',en:'Musical Instrument Digital Interface. A standard for digital musical communication.'},tags:['기초','Basic']},
];
function buildGlossary(){renderGlossary('');}
function renderGlossary(q){
  const list=document.getElementById('glossaryList');if(!list)return;list.innerHTML='';
  const filtered=q?GLOSS.filter(g=>g.t.toLowerCase().includes(q.toLowerCase())||g.d[lang].toLowerCase().includes(q.toLowerCase())):GLOSS;
  filtered.forEach(g=>{
    const item=document.createElement('div');item.className='g-item';
    item.innerHTML=`<div class="g-term">${g.t}</div><div class="g-def">${g.d[lang]}</div>
      <div class="g-tags">${g.tags.map(t=>`<span class="g-tag" style="background:var(--c-lv);color:var(--c-lv3)">${t}</span>`).join('')}</div>`;
    list.appendChild(item);
  });
  if(!filtered.length)list.innerHTML=`<div style="color:var(--c-txt3);padding:20px;text-align:center">${lang==='ko'?'결과 없음':'No results'}</div>`;
}

// ── UTILS ─────────────────────────────────────────────────────────────────
function downloadText(content, filename) {
  const a=document.createElement('a');
  a.href='data:text/plain;charset=utf-8,'+encodeURIComponent(content);
  a.download=filename;a.click();
}
function showToast(msg,dur=2200){
  const t=document.getElementById('toast');
  t.textContent=msg;t.classList.remove('hidden');
  setTimeout(()=>t.classList.add('hidden'),dur);
}

// ── KEYBOARD SHORTCUTS ────────────────────────────────────────────────────
document.addEventListener('keydown',e=>{
  if(e.target.tagName==='INPUT'||e.target.tagName==='SELECT'||e.target.tagName==='TEXTAREA')return;
  if(e.code==='Space'){e.preventDefault();transportPlay();}
  if(e.code==='KeyS'&&e.ctrlKey){e.preventDefault();openSaveModal();}
  if(e.code==='KeyZ'&&e.ctrlKey){e.preventDefault();if(rollNotes.length){rollNotes.pop();const s=(rollOct+2)*12-1;const c=rollBars*timeSig*(rollSnap/4);renderRollNotes(s,c);buildVelLane(c);}}
});

// ── INITIAL SETUP ─────────────────────────────────────────────────────────
// Wire up drum play button after DOM loaded
window.addEventListener('load',()=>{
  const dp=document.getElementById('ta_dplay');
  if(dp)dp.onclick=playDrum_all;
  // Apply saved lang to login screen too
  const saved=localStorage.getItem('mm_settings');
  if(saved){try{const o=JSON.parse(saved);if(o.lang)lang=o.lang;applyLoginLang();}catch(e){}}
  // Enter on password field
  document.getElementById('loginPwd').addEventListener('keydown',e=>{if(e.key==='Enter')doLogin();});
});
