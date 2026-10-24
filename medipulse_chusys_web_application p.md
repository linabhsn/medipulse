<!DOCTYPE html>
<html lang="en" class="dark">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="CHUSys Command — Moroccan hospital clinical, predictive staffing, biomedical equipment and financial management platform powered by DeepSeek AI.">
  <meta name="theme-color" content="#0d9488">
  <title>MediPulse CHUSys Command — Intelligent Hospital Management & AI Predictive Operations</title>

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: { extend: {
        colors: {
          brand: { 50:'#f0fdfa',100:'#ccfbf1',500:'#14b8a6',600:'#0d9488',700:'#0f766e',900:'#134e4a' },
          slate: { 850:'#152033', 950:'#090d16' }
        },
        fontFamily: { sans: ['Inter','sans-serif'] }
      }}
    }
  </script>
  
  <!-- Chart.js & SheetJS -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
  
  <!-- FontAwesome & Google Fonts -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">

  <style>
    body { font-family: 'Inter', sans-serif; }
    ::-webkit-scrollbar { width: 6px; height: 6px; }
    ::-webkit-scrollbar-track { background: #090d16; }
    ::-webkit-scrollbar-thumb { background: #334155; border-radius: 4px; }
    ::-webkit-scrollbar-thumb:hover { background: #475569; }
    .glass-panel {
      background: rgba(15, 23, 42, 0.82);
      backdrop-filter: blur(14px);
      border: 1px solid rgba(255, 255, 255, 0.08);
    }
    .tab-btn.active {
      background-color: rgba(20, 184, 166, 0.15);
      color: #2dd4bf;
      border-bottom: 2px solid #14b8a6;
    }
    .no-scrollbar::-webkit-scrollbar { display: none; }
  </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex flex-col selection:bg-teal-500 selection:text-white">

  <!-- ============================================================ -->
  <!-- HEADER                                                       -->
  <!-- ============================================================ -->
  <header class="border-b border-slate-800 bg-slate-900/90 sticky top-0 z-40 backdrop-blur-md">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16">
        <div class="flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-teal-500 to-cyan-500 flex items-center justify-center shadow-lg shadow-teal-500/20 shrink-0">
            <i class="fa-solid fa-hospital text-white text-xl"></i>
          </div>
          <div class="min-w-0">
            <div class="flex items-center space-x-2">
              <h1 class="font-bold text-lg tracking-tight text-white truncate">MediPulse <span class="text-teal-400 font-normal">CHUSys</span></h1>
              <span class="bg-teal-500/10 text-teal-400 border border-teal-500/20 text-[10px] font-bold px-2 py-0.5 rounded-full uppercase shrink-0">v2.4 DeepSeek Active</span>
            </div>
            <p class="text-xs text-slate-400 hidden sm:block truncate">Moroccan Hospital Clinical, Predictive Staffing, Machinery & Financial Platform</p>
          </div>
        </div>
        <div class="flex items-center space-x-2 sm:space-x-3 shrink-0">
          <!-- DeepSeek API Config -->
          <button type="button" onclick="openApiKeyModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 border border-slate-700 text-xs px-2.5 py-2 rounded-lg font-semibold transition flex items-center space-x-1.5">
            <i class="fa-solid fa-key text-amber-400"></i>
            <span class="hidden lg:inline">DeepSeek API Key</span>
          </button>
          <button type="button" onclick="openAddEquipmentModal()" class="bg-amber-600/20 hover:bg-amber-600/30 text-amber-300 border border-amber-500/30 text-xs px-3 py-2 rounded-lg font-semibold transition flex items-center space-x-1.5">
            <i class="fa-solid fa-gears text-amber-400"></i><span class="hidden lg:inline">+ Add Equipment</span>
          </button>
          <button type="button" onclick="openAddStaffModal()" class="bg-indigo-600/20 hover:bg-indigo-600/30 text-indigo-300 border border-indigo-500/30 text-xs px-3 py-2 rounded-lg font-semibold transition flex items-center space-x-1.5">
            <i class="fa-solid fa-user-doctor text-indigo-400"></i><span class="hidden lg:inline">+ Add Doctor</span>
          </button>
          <button type="button" onclick="toggleAdminModeBtn()" id="adminToggleBtn" class="bg-slate-800 hover:bg-slate-700 text-slate-300 border border-slate-700 text-xs px-3 py-2 rounded-lg font-semibold transition flex items-center space-x-2">
            <i id="adminLockIcon" class="fa-solid fa-lock text-amber-400"></i>
            <span id="adminBtnLabel" class="hidden md:inline">Admin Portal (PIN: 1234)</span>
          </button>
          <div class="h-6 w-px bg-slate-800 hidden sm:block"></div>
          <button type="button" onclick="switchTab('dispatchTab')" class="bg-rose-600/20 hover:bg-rose-600/30 text-rose-300 border border-rose-500/40 text-xs px-3.5 py-2 rounded-lg font-semibold transition flex items-center space-x-2 shadow-lg shadow-rose-950/40 animate-pulse">
            <i class="fa-solid fa-triangle-exclamation text-rose-400"></i><span class="hidden md:inline">Code Red Broadcast</span>
          </button>
        </div>
      </div>
      <nav class="flex space-x-1 sm:space-x-3 border-t border-slate-800/60 pt-1 overflow-x-auto no-scrollbar">
        <button type="button" onclick="switchTab('forecastTab')" id="btn-forecastTab" class="tab-btn active px-3.5 py-2.5 text-xs sm:text-sm font-medium text-slate-300 hover:text-white transition rounded-t-lg flex items-center space-x-2 whitespace-nowrap">
          <i class="fa-solid fa-brain text-teal-400"></i><span>DeepSeek AI Forecast</span>
        </button>
        <button type="button" onclick="switchTab('patientsTab')" id="btn-patientsTab" class="tab-btn px-3.5 py-2.5 text-xs sm:text-sm font-medium text-slate-400 hover:text-white transition rounded-t-lg flex items-center space-x-2 whitespace-nowrap">
          <i class="fa-solid fa-hospital-patient text-sky-400"></i><span>Patient Records & Ledger</span>
        </button>
        <button type="button" onclick="switchTab('equipmentTab')" id="btn-equipmentTab" class="tab-btn px-3.5 py-2.5 text-xs sm:text-sm font-medium text-slate-400 hover:text-white transition rounded-t-lg flex items-center space-x-2 whitespace-nowrap relative">
          <i class="fa-solid fa-laptop-medical text-amber-400"></i><span>Equipment & Machinery</span>
          <span id="equipmentRepairBadge" class="ml-1 px-1.5 py-0.2 bg-rose-500/20 text-rose-300 text-[10px] font-bold rounded-full border border-rose-500/30 animate-pulse">2 Repairs</span>
        </button>
        <button type="button" onclick="switchTab('staffTab')" id="btn-staffTab" class="tab-btn px-3.5 py-2.5 text-xs sm:text-sm font-medium text-slate-400 hover:text-white transition rounded-t-lg flex items-center space-x-2 whitespace-nowrap">
          <i class="fa-solid fa-user-nurse text-indigo-400"></i><span>Staff Roster & Shift Tracker</span>
          <span id="unassignedCountBadge" class="ml-1 px-1.5 py-0.2 bg-rose-500/20 text-rose-300 text-[10px] font-bold rounded-full border border-rose-500/30">2</span>
        </button>
        <button type="button" onclick="switchTab('dispatchTab')" id="btn-dispatchTab" class="tab-btn px-3.5 py-2.5 text-xs sm:text-sm font-medium text-slate-400 hover:text-white transition rounded-t-lg flex items-center space-x-2 whitespace-nowrap">
          <i class="fa-solid fa-phone-volume text-rose-400"></i><span>Emergency Dispatch</span>
        </button>
        <button type="button" onclick="switchTab('judgeTab')" id="btn-judgeTab" class="tab-btn px-3.5 py-2.5 text-xs sm:text-sm font-medium text-slate-400 hover:text-white transition rounded-t-lg flex items-center space-x-2 whitespace-nowrap">
          <i class="fa-solid fa-terminal text-teal-400"></i><span class="text-teal-400 font-semibold">Judge Console</span>
        </button>
      </nav>
    </div>
  </header>

  <!-- ============================================================ -->
  <!-- MAIN                                                         -->
  <!-- ============================================================ -->
  <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">

    <div id="adminModeBanner" class="hidden bg-amber-500/10 border border-amber-500/30 p-3 rounded-xl flex items-center justify-between">
      <div class="flex items-center space-x-3">
        <span class="w-8 h-8 rounded-lg bg-amber-500/20 text-amber-400 flex items-center justify-center font-bold text-xs shrink-0">
          <i class="fa-solid fa-user-shield"></i>
        </span>
        <div>
          <h4 class="text-xs font-bold text-amber-300 uppercase tracking-wider">Admin Portal Access Active</h4>
          <p class="text-[11px] text-slate-300">Unlocked: machinery registration, database ingestion, newborn registrations, and doctor onboarding.</p>
        </div>
      </div>
      <div class="flex items-center space-x-2">
        <button type="button" onclick="openAddEquipmentModal()" class="text-xs bg-amber-600 hover:bg-amber-500 text-white px-3 py-1.5 rounded-lg font-semibold transition shadow-md">+ Register Machine</button>
        <button type="button" onclick="openAddStaffModal()" class="text-xs bg-indigo-600 hover:bg-indigo-500 text-white px-3 py-1.5 rounded-lg font-semibold transition shadow-md">+ Register Doctor</button>
        <button type="button" onclick="lockAdminMode()" class="text-xs bg-slate-800 hover:bg-slate-700 text-amber-300 px-3 py-1.5 rounded-lg border border-slate-700 font-semibold transition">Exit Admin</button>
      </div>
    </div>

    <!-- ==================== TAB 1: FORECAST ==================== -->
    <section id="forecastTab" class="tab-content space-y-6">
      
      <!-- Top Surge Banner -->
      <div id="statusAlertBanner" class="p-4 rounded-xl glass-panel border-l-4 border-amber-500 flex flex-col sm:flex-row sm:items-center justify-between gap-3 transition-all duration-300">
        <div class="flex items-start sm:items-center space-x-3">
          <div id="statusAlertIcon" class="w-9 h-9 rounded-lg bg-amber-500/10 text-amber-400 flex items-center justify-center shrink-0 mt-0.5 sm:mt-0">
            <i class="fa-solid fa-triangle-exclamation text-lg"></i>
          </div>
          <div>
            <h4 id="statusAlertTitle" class="text-sm font-semibold text-slate-200">Surge Alert Detected for Upcoming Shifts</h4>
            <p id="statusAlertMessage" class="text-xs text-slate-400 mt-0.5">Predicted patient admissions exceed currently checked-in staff capacity by 18%. Roster adjustment recommended.</p>
          </div>
        </div>
        <div class="flex items-center space-x-2 shrink-0 self-end sm:self-auto">
          <span id="riskBadge" class="bg-amber-500/20 text-amber-300 text-xs font-bold px-3 py-1 rounded-full uppercase tracking-wider border border-amber-500/30">High Surge Risk</span>
        </div>
      </div>

      <!-- Department Selector & DeepSeek AI Control Strip -->
      <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-slate-900/60 p-3.5 rounded-xl border border-slate-800">
        <div class="flex items-center space-x-3 w-full sm:w-auto">
          <label for="departmentSelect" class="text-xs font-semibold text-slate-400 uppercase tracking-wider shrink-0">Target Ward:</label>
          <select id="departmentSelect" onchange="switchDepartment()" class="bg-slate-800 border border-slate-700 text-slate-200 text-sm rounded-lg focus:ring-2 focus:ring-teal-500 focus:border-teal-500 block px-3 py-2 font-medium cursor-pointer transition w-full sm:w-auto">
            <option value="ER">Emergency Room (ER)</option>
            <option value="ICU">Intensive Care Unit (ICU)</option>
            <option value="Ward">General Ward</option>
            <option value="Surgery">Surgical Suite</option>
            <option value="Pediatrics">Pediatrics & Neonatal Ward</option>
          </select>
        </div>
        <div class="flex items-center space-x-2 w-full sm:w-auto justify-end">
          <button type="button" onclick="triggerDatabaseImport()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 hover:text-white text-xs px-3 py-2 rounded-lg border border-slate-700 transition flex items-center space-x-1.5 font-medium">
            <i class="fa-solid fa-file-excel text-emerald-400"></i><span>Import Database</span>
          </button>
          <input type="file" id="dbFileInput" accept=".csv, .json, .xlsx, .xls" class="hidden" onchange="handleDatabaseImport(event)">
          <button type="button" onclick="runSurgePrediction()" class="bg-gradient-to-r from-teal-500 to-sky-600 hover:from-teal-400 hover:to-sky-500 text-white text-xs px-4 py-2 rounded-lg font-bold transition shadow-md shadow-teal-500/20 flex items-center space-x-1.5">
            <i class="fa-solid fa-bolt text-amber-300"></i><span>Execute DeepSeek AI Forecast</span>
          </button>
        </div>
      </div>

      <!-- KPI Metrics Strip -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">7-Day Forecast Total</span>
            <div class="w-8 h-8 rounded-lg bg-teal-500/10 text-teal-400 flex items-center justify-center"><i class="fa-solid fa-bed-pulse"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="kpiForecastTotal" class="text-2xl font-bold text-white">0</h3>
            <span id="kpiTrendTag" class="text-xs font-semibold text-emerald-400 flex items-center"><i class="fa-solid fa-arrow-trend-up mr-1"></i> +8.4%</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Projected patient admissions</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-gradient-to-r from-teal-500 to-emerald-500"></div>
        </div>
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Nurses Needed / Shift</span>
            <div class="w-8 h-8 rounded-lg bg-sky-500/10 text-sky-400 flex items-center justify-center"><i class="fa-solid fa-user-nurse"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="kpiNursesNeeded" class="text-2xl font-bold text-white">0</h3>
            <span id="kpiNurseDeficit" class="text-xs font-semibold text-rose-400 flex items-center"><i class="fa-solid fa-circle-exclamation mr-1"></i> 0</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Based on <span id="kpiNurseRatioTag" class="text-slate-300 font-medium">4:1</span> ratio</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-sky-500"></div>
        </div>
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Doctors Needed / Shift</span>
            <div class="w-8 h-8 rounded-lg bg-indigo-500/10 text-indigo-400 flex items-center justify-center"><i class="fa-solid fa-user-doctor"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="kpiDoctorsNeeded" class="text-2xl font-bold text-white">0</h3>
            <span id="kpiDoctorStatus" class="text-xs font-semibold text-emerald-400 flex items-center"><i class="fa-solid fa-check mr-1"></i> Staffed</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Based on <span id="kpiDoctorRatioTag" class="text-slate-300 font-medium">12:1</span> ratio</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-indigo-500"></div>
        </div>
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Overtime Cost Risk</span>
            <div class="w-8 h-8 rounded-lg bg-purple-500/10 text-purple-400 flex items-center justify-center"><i class="fa-solid fa-file-invoice-dollar"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="kpiCostRisk" class="text-2xl font-bold text-white">0 MAD</h3>
            <span class="text-xs font-semibold text-purple-400">Est. Weekly</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Potential understaffing risk in MAD</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-purple-500"></div>
        </div>
      </div>

      <!-- Main Forecast Chart & Rationale Panel -->
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <div class="lg:col-span-2 space-y-6 min-w-0">
          
          <div class="glass-panel rounded-2xl p-5">
            <div class="flex flex-col sm:flex-row sm:items-center justify-between pb-4 border-b border-slate-800 gap-2">
              <div>
                <div class="flex items-center space-x-2">
                  <h3 class="font-bold text-base text-white">DeepSeek AI Time-Series Influx Pipeline</h3>
                  <span id="badgeModelSource" class="bg-purple-500/20 text-purple-300 border border-purple-500/30 text-[11px] px-2 py-0.5 rounded font-mono">Model: DeepSeek AI</span>
                </div>
                <p class="text-xs text-slate-400 mt-0.5">Historical patient actuals vs predictive open-weight AI forecast</p>
              </div>
              <div class="flex items-center space-x-3 text-xs font-medium shrink-0">
                <span class="flex items-center"><span class="w-3 h-3 rounded-full bg-teal-500 inline-block mr-1.5"></span> Historical</span>
                <span class="flex items-center"><span class="w-3 h-3 rounded-full bg-sky-400 inline-block mr-1.5 border border-dashed border-sky-200"></span> AI Prediction</span>
              </div>
            </div>

            <!-- DeepSeek Reasoning Banner -->
            <div id="deepseekRationaleBox" class="mt-3 p-3 bg-teal-950/30 rounded-xl border border-teal-500/30 text-xs space-y-1">
              <span class="font-bold text-teal-300 flex items-center"><i class="fa-solid fa-comment-medical mr-1.5"></i> DeepSeek Clinical Rationale:</span>
              <p id="deepseekRationaleText" class="text-slate-300 leading-relaxed">Initialized. Executing 7-day velocity weighting and anomaly adjustments...</p>
            </div>

            <div class="mt-4 relative h-72 sm:h-80 w-full">
              <canvas id="predictiveChart"></canvas>
            </div>
          </div>

          <div class="glass-panel rounded-2xl p-5">
            <div class="flex flex-col sm:flex-row sm:items-center justify-between mb-4 gap-2">
              <div>
                <h3 class="font-bold text-base text-white">7-Day Shift Roster Recommendations</h3>
                <p class="text-xs text-slate-400">Optimized healthcare workforce requirements per daily shift cycle</p>
              </div>
              <button type="button" onclick="exportForecastCSV()" class="text-xs bg-slate-800 hover:bg-slate-700 text-teal-400 border border-slate-700 px-3 py-1.5 rounded-lg transition flex items-center space-x-1.5 font-medium shrink-0 self-start sm:self-auto">
                <i class="fa-solid fa-download"></i><span>Export Roster CSV</span>
              </button>
            </div>
            <div class="overflow-x-auto">
              <table class="w-full text-left text-xs">
                <thead class="bg-slate-900/90 text-slate-400 uppercase font-semibold border-b border-slate-800">
                  <tr>
                    <th class="py-3 px-3">Forecast Date</th>
                    <th class="py-3 px-3">Est. Patients</th>
                    <th class="py-3 px-3">Nurses Rec.</th>
                    <th class="py-3 px-3">Doctors Rec.</th>
                    <th class="py-3 px-3">Current Roster</th>
                    <th class="py-3 px-3 text-right">Coverage Status</th>
                  </tr>
                </thead>
                <tbody id="rosterTableBody" class="divide-y divide-slate-800/60 font-medium text-slate-300"></tbody>
              </table>
            </div>
          </div>
        </div>

        <div class="space-y-6">
          <div class="glass-panel rounded-2xl p-5 border-t-2 border-teal-500 space-y-5">
            <div class="flex items-center justify-between border-b border-slate-800 pb-3">
              <div class="flex items-center space-x-2">
                <i class="fa-solid fa-sliders text-teal-400"></i>
                <h3 class="font-bold text-base text-white">Workload Parameters</h3>
              </div>
              <button type="button" onclick="resetSlidersToDefaults()" class="text-[11px] text-slate-400 hover:text-teal-400 transition underline">Reset Defaults</button>
            </div>
            <div class="space-y-2">
              <div class="flex justify-between items-center text-xs">
                <label for="nurseRatioSlider" class="font-medium text-slate-300 flex items-center"><i class="fa-solid fa-user-nurse text-sky-400 mr-1.5"></i> Patient-to-Nurse Ratio</label>
                <span id="nurseRatioVal" class="font-bold text-sky-400 bg-sky-500/10 px-2 py-0.5 rounded border border-sky-500/20">4:1</span>
              </div>
              <input type="range" id="nurseRatioSlider" min="1" max="10" step="1" value="4" oninput="updateCalculations()" class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-teal-500">
            </div>
            <div class="space-y-2">
              <div class="flex justify-between items-center text-xs">
                <label for="doctorRatioSlider" class="font-medium text-slate-300 flex items-center"><i class="fa-solid fa-user-doctor text-indigo-400 mr-1.5"></i> Patient-to-Doctor Ratio</label>
                <span id="doctorRatioVal" class="font-bold text-indigo-400 bg-indigo-500/10 px-2 py-0.5 rounded border border-indigo-500/20">12:1</span>
              </div>
              <input type="range" id="doctorRatioSlider" min="4" max="25" step="1" value="12" oninput="updateCalculations()" class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-teal-500">
            </div>
            <div class="space-y-2">
              <div class="flex justify-between items-center text-xs">
                <label for="shiftLengthSelect" class="font-medium text-slate-300 flex items-center"><i class="fa-regular fa-clock text-amber-400 mr-1.5"></i> Target Shift Duration</label>
                <select id="shiftLengthSelect" onchange="updateCalculations()" class="bg-slate-800 text-teal-400 border border-slate-700 text-xs font-bold rounded px-2 py-1">
                  <option value="8">8 Hours (3 Shifts/Day)</option>
                  <option value="12" selected>12 Hours (2 Shifts/Day)</option>
                </select>
              </div>
            </div>
            <div class="space-y-2">
              <div class="flex justify-between items-center text-xs">
                <label for="acuitySlider" class="font-medium text-slate-300 flex items-center"><i class="fa-solid fa-heart-pulse text-rose-400 mr-1.5"></i> Acuity Severity Weight</label>
                <span id="acuityVal" class="font-bold text-rose-400 bg-rose-500/10 px-2 py-0.5 rounded border border-rose-500/20">1.25x</span>
              </div>
              <input type="range" id="acuitySlider" min="0.8" max="2.0" step="0.05" value="1.25" oninput="updateCalculations()" class="w-full h-2 bg-slate-800 rounded-lg appearance-none cursor-pointer accent-rose-500">
            </div>
            <div class="pt-3 border-t border-slate-800 space-y-3">
              <h4 class="text-xs font-semibold text-slate-300 uppercase tracking-wider">On-Duty Roster Inputs</h4>
              <div class="grid grid-cols-2 gap-3">
                <div>
                  <label for="scheduledNursesInput" class="text-[11px] text-slate-400 block mb-1">Scheduled Nurses</label>
                  <input type="number" id="scheduledNursesInput" value="11" min="0" onchange="updateCalculations()" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-2.5 py-1.5 text-xs text-white font-semibold focus:ring-1 focus:ring-teal-500">
                </div>
                <div>
                  <label for="scheduledDoctorsInput" class="text-[11px] text-slate-400 block mb-1">Scheduled Doctors</label>
                  <input type="number" id="scheduledDoctorsInput" value="4" min="0" onchange="updateCalculations()" class="w-full bg-slate-900 border border-slate-700 rounded-lg px-2.5 py-1.5 text-xs text-white font-semibold focus:ring-1 focus:ring-teal-500">
                </div>
              </div>
            </div>
          </div>

          <div class="glass-panel rounded-2xl p-5 space-y-3">
            <h4 class="text-xs font-bold text-slate-200 uppercase tracking-wider flex items-center">
              <i class="fa-solid fa-circle-info text-teal-400 mr-2"></i> Department Profile
            </h4>
            <p id="deptPresetDescription" class="text-xs text-slate-400 leading-relaxed">Emergency Room departments experience high volatility with morning and late-evening admission spikes.</p>
            <div class="bg-slate-900/80 rounded-xl p-3 border border-slate-800 space-y-2 text-xs">
              <div class="flex justify-between"><span class="text-slate-400">Peak Surge Hours:</span><span id="deptPeakHours" class="font-medium text-slate-200">14:00 - 22:00</span></div>
              <div class="flex justify-between"><span class="text-slate-400">Bed Turnaround:</span><span id="deptTurnaround" class="font-medium text-slate-200">4.2 Hours</span></div>
              <div class="flex justify-between"><span class="text-slate-400">Safety Buffer:</span><span id="deptSafetyBuffer" class="font-medium text-teal-400">+15% Capacity</span></div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ==================== TAB 2: PATIENTS ==================== -->
    <section id="patientsTab" class="tab-content hidden space-y-6">
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Active Admitted Patients</span>
            <div class="w-8 h-8 rounded-lg bg-sky-500/10 text-sky-400 flex items-center justify-center"><i class="fa-solid fa-bed-pulse"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="statTotalPatients" class="text-2xl font-bold text-white">0</h3>
            <span class="text-xs font-semibold text-sky-400">Current Occupancy</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Across ICU, ER, Surgery, Peds & Ward</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-sky-500"></div>
        </div>
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Gross Invoiced Billing</span>
            <div class="w-8 h-8 rounded-lg bg-emerald-500/10 text-emerald-400 flex items-center justify-center"><i class="fa-solid fa-file-invoice-dollar"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="statGrossBilling" class="text-2xl font-bold text-white">0 MAD</h3>
            <span class="text-xs font-semibold text-emerald-400">Total Charges</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Surgeries, Consults, Beds & Meds</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-emerald-500"></div>
        </div>
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Insurance Coverage</span>
            <div class="w-8 h-8 rounded-lg bg-teal-500/10 text-teal-400 flex items-center justify-center"><i class="fa-solid fa-building-shield"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="statInsurancePaid" class="text-2xl font-bold text-teal-400">0 MAD</h3>
            <span class="text-xs font-semibold text-teal-400">Covered</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">CNSS, CNOPS, AXA, Sanlam, RMA</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-teal-500"></div>
        </div>
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Patient Co-Pay Balance</span>
            <div class="w-8 h-8 rounded-lg bg-amber-500/10 text-amber-400 flex items-center justify-center"><i class="fa-solid fa-wallet"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="statPatientBalance" class="text-2xl font-bold text-amber-400">0 MAD</h3>
            <span class="text-xs font-semibold text-amber-400">Out-of-Pocket</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Remaining balance due by patients</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-amber-500"></div>
        </div>
      </div>

      <div class="glass-panel rounded-2xl p-4 flex flex-col md:flex-row items-center justify-between gap-4">
        <div class="relative w-full md:w-80">
          <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-3 text-slate-400 text-xs"></i>
          <input type="text" id="patientSearchInput" oninput="filterPatients()" placeholder="Search Patient #, Name, Diagnosis, Insurance..." class="w-full bg-slate-900 border border-slate-700 rounded-xl pl-9 pr-4 py-2 text-xs text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-teal-500">
        </div>
        <div class="flex items-center space-x-2 overflow-x-auto w-full md:w-auto pb-1 md:pb-0">
          <span class="text-xs text-slate-400 font-medium mr-1 shrink-0">Filter:</span>
          <button type="button" onclick="filterPatientDept('ALL', event)" class="patient-dept-btn active px-3 py-1.5 rounded-lg text-xs font-medium bg-teal-500/20 text-teal-300 border border-teal-500/30">All</button>
          <button type="button" onclick="filterPatientDept('NEWBORN', event)" class="patient-dept-btn px-3 py-1.5 rounded-lg text-xs font-medium bg-slate-800 text-purple-400 hover:text-purple-300 border border-slate-700"><i class="fa-solid fa-baby mr-1"></i> Newborns</button>
          <button type="button" onclick="filterPatientDept('ICU', event)" class="patient-dept-btn px-3 py-1.5 rounded-lg text-xs font-medium bg-slate-800 text-slate-400 hover:text-slate-200 border border-slate-700">ICU</button>
          <button type="button" onclick="filterPatientDept('ER', event)" class="patient-dept-btn px-3 py-1.5 rounded-lg text-xs font-medium bg-slate-800 text-slate-400 hover:text-slate-200 border border-slate-700">ER</button>
          <button type="button" onclick="filterPatientDept('Surgery', event)" class="patient-dept-btn px-3 py-1.5 rounded-lg text-xs font-medium bg-slate-800 text-slate-400 hover:text-slate-200 border border-slate-700">Surgery</button>
          <button type="button" onclick="filterPatientDept('Pediatrics', event)" class="patient-dept-btn px-3 py-1.5 rounded-lg text-xs font-medium bg-slate-800 text-slate-400 hover:text-slate-200 border border-slate-700">Pediatrics</button>
          <button type="button" onclick="filterPatientDept('Ward', event)" class="patient-dept-btn px-3 py-1.5 rounded-lg text-xs font-medium bg-slate-800 text-slate-400 hover:text-slate-200 border border-slate-700">Ward</button>
        </div>
        <div class="flex items-center space-x-2 shrink-0">
          <button type="button" onclick="openRegistrationModal()" class="bg-teal-600 hover:bg-teal-500 text-white text-xs px-3.5 py-2 rounded-xl font-semibold transition flex items-center space-x-1.5 shadow-lg shadow-teal-600/20">
            <i class="fa-solid fa-user-plus"></i><span>+ Register Patient / Newborn</span>
          </button>
        </div>
      </div>

      <div id="patientGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5"></div>
    </section>

    <!-- ==================== TAB 3: EQUIPMENT ==================== -->
    <section id="equipmentTab" class="tab-content hidden space-y-6">
      <div id="equipmentAlertBanner" class="p-4 rounded-xl glass-panel border-l-4 border-rose-500 bg-rose-950/20 flex flex-col sm:flex-row sm:items-center justify-between gap-3">
        <div class="flex items-start sm:items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-rose-500/20 text-rose-400 border border-rose-500/30 flex items-center justify-center shrink-0">
            <i class="fa-solid fa-triangle-exclamation text-xl animate-bounce"></i>
          </div>
          <div>
            <h4 id="equipmentAlertTitle" class="text-sm font-bold text-rose-300">Biomedical Equipment Defect Alert</h4>
            <p id="equipmentAlertMessage" class="text-xs text-slate-300 mt-0.5">Machinery items require urgent repair calibration.</p>
          </div>
        </div>
        <button type="button" onclick="filterEquipmentStatus('Needs Repair')" class="bg-rose-600 hover:bg-rose-500 text-white text-xs px-3.5 py-2 rounded-xl font-bold transition shadow-lg shadow-rose-950/40 shrink-0">
          View Defective Units
        </button>
      </div>

      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Total Equipment Tracked</span>
            <div class="w-8 h-8 rounded-lg bg-amber-500/10 text-amber-400 flex items-center justify-center"><i class="fa-solid fa-laptop-medical"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="statTotalEquipment" class="text-2xl font-bold text-white">0</h3>
            <span class="text-xs font-semibold text-slate-400">Rooms & OR Suites</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Monitors, Ventilators, Pumps & Lasers</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-amber-500"></div>
        </div>
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Fully Operational</span>
            <div class="w-8 h-8 rounded-lg bg-emerald-500/10 text-emerald-400 flex items-center justify-center"><i class="fa-solid fa-circle-check"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="statOperationalEquipment" class="text-2xl font-bold text-emerald-400">0</h3>
            <span class="text-xs font-semibold text-emerald-400">Ready</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Calibrated & active in clinical use</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-emerald-500"></div>
        </div>
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Needs Repair / Defective</span>
            <div class="w-8 h-8 rounded-lg bg-rose-500/10 text-rose-400 flex items-center justify-center"><i class="fa-solid fa-wrench"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="statRepairEquipment" class="text-2xl font-bold text-rose-400">0</h3>
            <span class="text-xs font-semibold text-rose-400">Immediate Action</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">Biomedical tech repair assigned</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-rose-500"></div>
        </div>
        <div class="glass-panel rounded-xl p-4 relative overflow-hidden">
          <div class="flex items-center justify-between">
            <span class="text-xs font-medium text-slate-400 uppercase tracking-wider">Under Maintenance</span>
            <div class="w-8 h-8 rounded-lg bg-amber-500/10 text-amber-400 flex items-center justify-center"><i class="fa-solid fa-sliders"></i></div>
          </div>
          <div class="mt-3 flex items-baseline justify-between">
            <h3 id="statMaintenanceEquipment" class="text-2xl font-bold text-amber-400">0</h3>
            <span class="text-xs font-semibold text-amber-400">Routine Check</span>
          </div>
          <p class="text-[11px] text-slate-400 mt-1">500h calibration in progress</p>
          <div class="absolute bottom-0 left-0 right-0 h-1 bg-amber-500"></div>
        </div>
      </div>

      <div class="glass-panel rounded-2xl p-4 flex flex-col md:flex-row items-center justify-between gap-4">
        <div class="relative w-full md:w-80">
          <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-3 text-slate-400 text-xs"></i>
          <input type="text" id="equipmentSearchInput" oninput="filterEquipment()" placeholder="Search Machine ID, Name, Serial, Room..." class="w-full bg-slate-900 border border-slate-700 rounded-xl pl-9 pr-4 py-2 text-xs text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-amber-500">
        </div>
        <div class="flex flex-wrap items-center gap-2 w-full md:w-auto">
          <span class="text-xs text-slate-400 font-medium mr-1">Location:</span>
          <button type="button" onclick="filterEquipmentCategory('ALL', event)" class="eq-cat-btn active px-3 py-1.5 rounded-lg text-xs font-medium bg-amber-500/20 text-amber-300 border border-amber-500/30">All</button>
          <button type="button" onclick="filterEquipmentCategory('Patient Room', event)" class="eq-cat-btn px-3 py-1.5 rounded-lg text-xs font-medium bg-slate-800 text-slate-400 hover:text-slate-200 border border-slate-700"><i class="fa-solid fa-bed mr-1"></i> Patient Rooms</button>
          <button type="button" onclick="filterEquipmentCategory('Operating Room', event)" class="eq-cat-btn px-3 py-1.5 rounded-lg text-xs font-medium bg-slate-800 text-slate-400 hover:text-slate-200 border border-slate-700"><i class="fa-solid fa-scalpel mr-1"></i> Operating Rooms</button>
          <span class="text-xs text-slate-400 font-medium ml-2 mr-1">Status:</span>
          <select id="equipmentStatusFilter" onchange="filterEquipment()" class="bg-slate-800 border border-slate-700 text-xs text-slate-200 rounded-lg px-3 py-1.5 focus:ring-1 focus:ring-amber-500">
            <option value="ALL">All Statuses</option>
            <option value="Operational">Operational Only</option>
            <option value="Needs Repair">Needs Repair Only</option>
            <option value="Under Maintenance">Under Maintenance</option>
          </select>
        </div>
        <div class="flex items-center space-x-2 shrink-0">
          <button type="button" onclick="openAddEquipmentModal()" class="bg-amber-600 hover:bg-amber-500 text-white text-xs px-3.5 py-2 rounded-xl font-bold transition flex items-center space-x-1.5 shadow-lg shadow-amber-600/20">
            <i class="fa-solid fa-plus"></i><span>Register New Machine</span>
          </button>
        </div>
      </div>

      <div id="equipmentGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5"></div>
    </section>

    <!-- ==================== TAB 4: STAFF ==================== -->
    <section id="staffTab" class="tab-content hidden space-y-6">
      <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
        <div class="glass-panel rounded-xl p-4 flex items-center space-x-3">
          <div class="w-10 h-10 rounded-lg bg-teal-500/10 text-teal-400 flex items-center justify-center shrink-0"><i class="fa-solid fa-users text-lg"></i></div>
          <div class="min-w-0"><span class="text-xs text-slate-400 uppercase tracking-wider block truncate">Total Roster</span><span id="statTotalStaff" class="text-xl font-bold text-white truncate">0</span></div>
        </div>
        <div class="glass-panel rounded-xl p-4 flex items-center space-x-3">
          <div class="w-10 h-10 rounded-lg bg-emerald-500/10 text-emerald-400 flex items-center justify-center shrink-0"><i class="fa-solid fa-user-check text-lg"></i></div>
          <div class="min-w-0"><span class="text-xs text-slate-400 uppercase tracking-wider block truncate">Active On Shift</span><span id="statActiveStaff" class="text-xl font-bold text-emerald-400 truncate">0</span></div>
        </div>
        <div class="glass-panel rounded-xl p-4 flex items-center space-x-3">
          <div class="w-10 h-10 rounded-lg bg-rose-500/10 text-rose-400 flex items-center justify-center shrink-0"><i class="fa-solid fa-user-xmark text-lg"></i></div>
          <div class="min-w-0"><span class="text-xs text-slate-400 uppercase tracking-wider block truncate">Unchecked</span><span id="statMissingStaff" class="text-xl font-bold text-rose-400 truncate">0</span></div>
        </div>
        <div class="glass-panel rounded-xl p-4 flex items-center space-x-3">
          <div class="w-10 h-10 rounded-lg bg-amber-500/10 text-amber-400 flex items-center justify-center shrink-0"><i class="fa-solid fa-mug-hot text-lg"></i></div>
          <div class="min-w-0"><span class="text-xs text-slate-400 uppercase tracking-wider block truncate">On Break</span><span id="statBreakStaff" class="text-xl font-bold text-amber-400 truncate">0</span></div>
        </div>
      </div>

      <div class="glass-panel rounded-2xl p-4 flex flex-col md:flex-row items-center justify-between gap-4">
        <div class="relative w-full md:w-80">
          <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-3 text-slate-400 text-xs"></i>
          <input type="text" id="staffSearchInput" oninput="filterStaff()" placeholder="Search Name, Role, Specialty..." class="w-full bg-slate-900 border border-slate-700 rounded-xl pl-9 pr-4 py-2 text-xs text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-teal-500">
        </div>
        <div class="flex flex-wrap items-center gap-2 w-full md:w-auto">
          <span class="text-xs text-slate-400 font-medium mr-1">Role:</span>
          <select id="staffRoleFilter" onchange="filterStaff()" class="bg-slate-800 border border-slate-700 text-xs text-slate-200 rounded-lg px-3 py-1.5 focus:ring-1 focus:ring-teal-500">
            <option value="ALL">All Roles</option>
            <option value="Doctor">Doctors Only</option>
            <option value="Nurse">Nurses Only</option>
          </select>
          <span class="text-xs text-slate-400 font-medium ml-2 mr-1">Status:</span>
          <select id="staffStatusFilter" onchange="filterStaff()" class="bg-slate-800 border border-slate-700 text-xs text-slate-200 rounded-lg px-3 py-1.5 focus:ring-1 focus:ring-teal-500">
            <option value="ALL">All Statuses</option>
            <option value="Checked In">Checked In</option>
            <option value="Shift Not Checked-In">Not Checked In</option>
            <option value="On Break">On Break</option>
          </select>
        </div>
        <div class="flex items-center space-x-2 shrink-0">
          <button type="button" onclick="openAddStaffModal()" class="bg-indigo-600 hover:bg-indigo-500 text-white text-xs px-3.5 py-2 rounded-xl font-bold transition flex items-center space-x-1.5 shadow-lg shadow-indigo-600/20">
            <i class="fa-solid fa-user-plus"></i><span>Add Doctor / Staff</span>
          </button>
        </div>
      </div>

      <div id="staffGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5"></div>
    </section>

    <!-- ==================== TAB 5: DISPATCH ==================== -->
    <section id="dispatchTab" class="tab-content hidden space-y-6">
      <div class="glass-panel rounded-2xl p-6 border-2 border-rose-500/50 bg-gradient-to-r from-rose-950/40 via-slate-900 to-slate-900 space-y-4">
        <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4">
          <div class="flex items-center space-x-4">
            <div class="w-12 h-12 rounded-xl bg-rose-500/20 text-rose-400 border border-rose-500/30 flex items-center justify-center shrink-0"><i class="fa-solid fa-bullhorn text-2xl animate-bounce"></i></div>
            <div>
              <h3 class="text-lg font-bold text-white">Emergency Staff Callout Broadcast Protocol</h3>
              <p class="text-xs text-slate-300 mt-0.5">Instantly dispatch automated SMS, push alerts, and direct call alerts to all active on-duty Moroccan clinical personnel.</p>
            </div>
          </div>
          <button type="button" onclick="triggerEmergencyBroadcast()" class="bg-rose-600 hover:bg-rose-500 text-white text-xs px-5 py-3 rounded-xl font-bold transition shadow-xl shadow-rose-600/30 flex items-center space-x-2 shrink-0">
            <i class="fa-solid fa-tower-broadcast text-sm"></i><span>BROADCAST EMERGENCY ALERT</span>
          </button>
        </div>
      </div>
      <div class="space-y-4">
        <div class="flex items-center justify-between">
          <div>
            <h3 class="font-bold text-base text-white">Active On-Duty Emergency Contact Directory (+212)</h3>
            <p class="text-xs text-slate-400">Direct phone dialing and email shortcuts for currently checked-in clinical staff</p>
          </div>
          <span class="text-xs text-slate-400 bg-slate-800 px-3 py-1 rounded-full border border-slate-700 font-mono">1-Tap Emergency Actions</span>
        </div>
        <div id="emergencyDispatchGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5"></div>
      </div>
    </section>

    <!-- ==================== TAB 6: JUDGE CONSOLE ==================== -->
    <section id="judgeTab" class="tab-content hidden space-y-6">
      <div class="glass-panel rounded-2xl p-6 space-y-4 border border-teal-500/30">
        <div class="flex items-center justify-between border-b border-slate-800 pb-3">
          <div>
            <h3 class="font-bold text-teal-400 text-lg flex items-center gap-2">
              <i class="fa-solid fa-terminal"></i>
              <span>Judge Verification & DeepSeek Live Console</span>
            </h3>
            <p class="text-xs text-slate-400">Verify code execution, open-weight DeepSeek API endpoints, and mathematical fallback logic.</p>
          </div>
          <button type="button" onclick="clearConsoleLog()" class="px-3 py-1.5 bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs rounded-lg border border-slate-700">
            Clear Terminal
          </button>
        </div>

        <!-- Terminal Log Area -->
        <div id="console-terminal" class="bg-black/90 p-4 rounded-xl border border-slate-800 font-mono text-xs text-emerald-400 h-64 overflow-y-auto space-y-1">
          <p class="text-slate-500">[System Ready] CHUSys Terminal Initialized...</p>
          <p class="text-sky-400">&gt; Exposed exposed global functions: window.fetchDeepSeekSurgePrediction(), window.executeLocalFallbackPrediction(), window.runSurgePrediction()</p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 pt-2">
          <button type="button" onclick="testApiDirectly()" class="p-3 bg-slate-800 hover:bg-slate-700 rounded-xl border border-slate-700 text-xs font-semibold text-sky-300 text-left">
            ⚡ Test DeepSeek API Direct Call
          </button>
          <button type="button" onclick="testFallbackDirectly()" class="p-3 bg-slate-800 hover:bg-slate-700 rounded-xl border border-slate-700 text-xs font-semibold text-emerald-300 text-left">
            📐 Test Fallback MA7 Formula
          </button>
          <a href="https://github.com/linabhsn/bionics-GRP" target="_blank" class="p-3 bg-slate-800 hover:bg-slate-700 rounded-xl border border-slate-700 text-xs font-semibold text-teal-300 flex items-center justify-between">
            <span>📂 Open Repository on GitHub</span>
            <i class="fa-solid fa-arrow-up-right-from-square"></i>
          </a>
        </div>
      </div>
    </section>

  </main>

  <!-- ============================================================ -->
  <!-- MODALS & TOASTS                                              -->
  <!-- ============================================================ -->
  <div id="toastContainer" aria-live="polite" class="fixed bottom-5 right-5 z-50 space-y-3 max-w-sm"></div>

  <!-- DeepSeek API Key Modal -->
  <div id="apiKeyModal" role="dialog" aria-modal="true" aria-hidden="true" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-md hidden flex items-center justify-center p-4">
    <div class="glass-panel bg-slate-900 border border-teal-500/40 w-full max-w-md rounded-2xl shadow-2xl p-6 space-y-4">
      <div class="flex justify-between items-center border-b border-slate-800 pb-3">
        <h3 class="font-bold text-white flex items-center gap-2">
          <i class="fa-solid fa-key text-amber-400"></i>
          <span>DeepSeek API Configuration</span>
        </h3>
        <button type="button" onclick="closeApiKeyModal()" class="text-slate-400 hover:text-slate-200">
          <i class="fa-solid fa-xmark"></i>
        </button>
      </div>
      <p class="text-xs text-slate-300 leading-relaxed">
        Input your DeepSeek API key to target <code class="text-sky-300">https://api.deepseek.com/v1/chat/completions</code>. If omitted, CHUSys automatically executes the local mathematical $\text{MA}_7$ fallback engine.
      </p>
      <div>
        <label for="deepseekApiKeyInput" class="block text-xs font-semibold text-slate-300 mb-1">API Key</label>
        <input type="password" id="deepseekApiKeyInput" placeholder="sk-..." class="w-full bg-slate-950 border border-slate-700 rounded-xl p-2.5 text-xs text-white font-mono">
      </div>
      <div class="flex justify-end space-x-2 pt-2">
        <button type="button" onclick="closeApiKeyModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs px-4 py-2 rounded-xl font-semibold transition">Cancel</button>
        <button type="button" onclick="saveApiKey()" class="bg-teal-600 hover:bg-teal-500 text-white text-xs px-5 py-2 rounded-xl font-bold transition">Save Key</button>
      </div>
    </div>
  </div>

  <!-- Admin PIN modal -->
  <div id="adminPinModal" role="dialog" aria-modal="true" aria-hidden="true" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-md hidden flex items-center justify-center p-4">
    <div class="glass-panel bg-slate-900 border border-amber-500/40 w-full max-w-md rounded-2xl shadow-2xl p-6 space-y-4">
      <div class="flex items-center space-x-3">
        <div class="w-12 h-12 rounded-xl bg-amber-500/20 text-amber-400 border border-amber-500/30 flex items-center justify-center text-xl shrink-0"><i class="fa-solid fa-user-shield"></i></div>
        <div>
          <h3 class="text-base font-bold text-white">CHUSys Admin Mode Auth</h3>
          <p class="text-xs text-slate-400">Enter Admin PIN to unlock uploads, doctor & equipment registrations</p>
        </div>
      </div>
      <div class="space-y-2">
        <label for="adminPinInput" class="text-xs font-semibold text-slate-300">Admin Security PIN (Demo: 1234)</label>
        <input type="password" id="adminPinInput" maxlength="6" inputmode="numeric" autocomplete="off" placeholder="Enter PIN" onkeydown="if(event.key==='Enter')verifyAdminPin()" class="w-full bg-slate-950 border border-slate-700 rounded-xl px-4 py-2.5 text-center text-lg tracking-widest text-teal-400 font-bold focus:outline-none focus:ring-2 focus:ring-amber-500">
        <p id="adminPinError" class="text-[11px] text-rose-400 hidden"><i class="fa-solid fa-circle-exclamation mr-1"></i> Incorrect Admin PIN. Try 1234.</p>
      </div>
      <div class="flex justify-end space-x-2 pt-2">
        <button type="button" onclick="closeAdminPinModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs px-4 py-2 rounded-xl font-semibold transition">Cancel</button>
        <button type="button" onclick="verifyAdminPin()" class="bg-amber-600 hover:bg-amber-500 text-white text-xs px-5 py-2 rounded-xl font-bold transition">Unlock Admin Access</button>
      </div>
    </div>
  </div>

  <!-- Add Equipment modal -->
  <div id="addEquipmentModal" role="dialog" aria-modal="true" aria-hidden="true" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-md hidden flex items-center justify-center p-3 sm:p-6">
    <div class="glass-panel bg-slate-900 border border-slate-700 w-full max-w-xl rounded-2xl shadow-2xl overflow-hidden max-h-[85vh] flex flex-col">
      <div class="p-4 border-b border-slate-800 flex items-center justify-between bg-slate-950/80 shrink-0">
        <div class="flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-amber-500/20 text-amber-400 border border-amber-500/30 flex items-center justify-center text-lg shrink-0"><i class="fa-solid fa-laptop-medical"></i></div>
          <div>
            <h3 class="text-base font-bold text-white">Register New Medical Machinery</h3>
            <p class="text-xs text-slate-400">Onboard room monitors, ventilators, anesthesia & surgical equipment</p>
          </div>
        </div>
        <button type="button" onclick="closeAddEquipmentModal()" class="w-8 h-8 rounded-lg bg-slate-800 text-slate-400 hover:text-white flex items-center justify-center transition"><i class="fa-solid fa-xmark text-sm"></i></button>
      </div>
      <form id="addEquipmentForm" onsubmit="handleAddEquipmentSubmit(event)" class="p-4 sm:p-6 space-y-4 overflow-y-auto text-xs text-slate-300">
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <div><label for="newEqName" class="block text-slate-400 font-medium mb-1">Equipment Name</label><input type="text" id="newEqName" required placeholder="e.g. Draeger Fabius Anesthesia" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-medium focus:ring-1 focus:ring-amber-500"></div>
          <div><label for="newEqCategory" class="block text-slate-400 font-medium mb-1">Location Category</label><select id="newEqCategory" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"><option value="Patient Room">Patient Room</option><option value="Operating Room">Operating Room (OR)</option></select></div>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <div><label for="newEqType" class="block text-slate-400 font-medium mb-1">Device Type</label><input type="text" id="newEqType" required placeholder="e.g. ICU Ventilator, Monitor" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
          <div><label for="newEqSerial" class="block text-slate-400 font-medium mb-1">Serial Number</label><input type="text" id="newEqSerial" value="SN-EQ-88392" required class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-mono"></div>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <div><label for="newEqDept" class="block text-slate-400 font-medium mb-1">Department Ward</label><select id="newEqDept" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"><option value="ICU">ICU</option><option value="Surgery">Surgery</option><option value="ER">ER</option><option value="Pediatrics">Pediatrics</option><option value="Ward">Ward</option></select></div>
          <div><label for="newEqLocation" class="block text-slate-400 font-medium mb-1">Room / Suite Location</label><input type="text" id="newEqLocation" value="ICU Room #02" required class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <div><label for="newEqPatient" class="block text-slate-400 font-medium mb-1">Assign to Patient (Optional)</label><select id="newEqPatient" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"><option value="">-- No Patient Linkage --</option></select></div>
          <div><label for="newEqStatus" class="block text-slate-400 font-medium mb-1">Initial Status</label><select id="newEqStatus" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-bold"><option value="Operational">Operational</option><option value="Needs Repair">Needs Repair</option><option value="Under Maintenance">Under Maintenance</option></select></div>
        </div>
        <div class="grid grid-cols-2 gap-3">
          <div><label for="newEqServiceDate" class="block text-slate-400 font-medium mb-1">Last Service Date</label><input type="date" id="newEqServiceDate" value="2026-09-15" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-mono"></div>
          <div><label for="newEqCalibDate" class="block text-slate-400 font-medium mb-1">Next Calibration Due</label><input type="date" id="newEqCalibDate" value="2027-03-15" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-mono"></div>
        </div>
        <div><label for="newEqEngineer" class="block text-slate-400 font-medium mb-1">Assigned Biomedical Engineer</label><input type="text" id="newEqEngineer" value="Ing. Hassan Alami (+212 661-889900)" required class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
        <div class="p-3 bg-slate-950/80 rounded-xl border border-slate-800 flex justify-end space-x-2 shrink-0 pt-3">
          <button type="button" onclick="closeAddEquipmentModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs px-4 py-2 rounded-xl font-semibold transition">Cancel</button>
          <button type="submit" class="bg-amber-600 hover:bg-amber-500 text-white text-xs px-5 py-2 rounded-xl font-bold transition flex items-center space-x-1.5"><i class="fa-solid fa-check"></i><span>Register Machine</span></button>
        </div>
      </form>
    </div>
  </div>

  <!-- Add Staff modal -->
  <div id="addStaffModal" role="dialog" aria-modal="true" aria-hidden="true" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-md hidden flex items-center justify-center p-3 sm:p-6">
    <div class="glass-panel bg-slate-900 border border-slate-700 w-full max-w-xl rounded-2xl shadow-2xl overflow-hidden max-h-[85vh] flex flex-col">
      <div class="p-4 border-b border-slate-800 flex items-center justify-between bg-slate-950/80 shrink-0">
        <div class="flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-indigo-500/20 text-indigo-400 border border-indigo-500/30 flex items-center justify-center text-lg shrink-0"><i class="fa-solid fa-user-doctor"></i></div>
          <div>
            <h3 class="text-base font-bold text-white">Register New Doctor / Clinical Staff</h3>
            <p class="text-xs text-slate-400">Onboard physicians, specialist consultants, and charge nurses</p>
          </div>
        </div>
        <button type="button" onclick="closeAddStaffModal()" class="w-8 h-8 rounded-lg bg-slate-800 text-slate-400 hover:text-white flex items-center justify-center transition"><i class="fa-solid fa-xmark text-sm"></i></button>
      </div>
      <form id="addStaffForm" onsubmit="handleAddStaffSubmit(event)" class="p-4 sm:p-6 space-y-4 overflow-y-auto text-xs text-slate-300">
        <div class="grid grid-cols-2 gap-3 bg-slate-950 p-2 rounded-xl border border-slate-800">
          <label class="flex items-center justify-center space-x-2 p-2 rounded-lg cursor-pointer transition bg-indigo-600/20 text-indigo-300 border border-indigo-500/30 font-bold" id="labelRoleDoctor">
            <input type="radio" name="newStaffType" value="Doctor" checked onchange="toggleStaffRoleType('Doctor')" class="hidden">
            <i class="fa-solid fa-user-doctor"></i><span>Medical Doctor</span>
          </label>
          <label class="flex items-center justify-center space-x-2 p-2 rounded-lg cursor-pointer transition text-slate-400 hover:text-slate-200" id="labelRoleNurse">
            <input type="radio" name="newStaffType" value="Nurse" onchange="toggleStaffRoleType('Nurse')" class="hidden">
            <i class="fa-solid fa-user-nurse"></i><span>Nursing Staff</span>
          </label>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <div><label for="newStaffName" class="block text-slate-400 font-medium mb-1">Full Name & Title</label><input type="text" id="newStaffName" required placeholder="e.g. Dr. Fatima Zohra Bennis" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-medium focus:ring-1 focus:ring-teal-500"></div>
          <div><label for="newStaffRoleLevel" class="block text-slate-400 font-medium mb-1">Position</label><select id="newStaffRoleLevel" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white">
            <option value="Attending Physician">Attending Physician</option>
            <option value="Senior Consultant Specialist">Senior Consultant Specialist</option>
            <option value="Department Head / Chair">Department Head / Chair</option>
            <option value="Resident Physician">Resident Physician</option>
            <option value="Fellow Specialist">Fellow Specialist</option>
          </select></div>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <div><label for="newStaffSpecialty" class="block text-slate-400 font-medium mb-1">Specialty</label><input type="text" id="newStaffSpecialty" required placeholder="e.g. Neonatology" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
          <div><label for="newStaffDept" class="block text-slate-400 font-medium mb-1">Department</label><select id="newStaffDept" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white">
            <option value="Pediatrics">Pediatrics</option><option value="ICU">ICU</option><option value="ER">ER</option><option value="Surgery">Surgery</option><option value="Ward">Ward</option>
          </select></div>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
          <div><label for="newStaffPhone" class="block text-slate-400 font-medium mb-1">Phone (+212)</label><input type="text" id="newStaffPhone" value="+212 661-987654" required class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-mono"></div>
          <div><label for="newStaffEmail" class="block text-slate-400 font-medium mb-1">Email</label><input type="email" id="newStaffEmail" value="f.zohra@chu-maroc.ma" required class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-mono"></div>
        </div>
        <div class="grid grid-cols-3 gap-3">
          <div><label for="newStaffShiftStart" class="block text-slate-400 font-medium mb-1">Shift Start</label><input type="text" id="newStaffShiftStart" value="08:00 AM" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-2.5 py-2 text-white text-center font-mono"></div>
          <div><label for="newStaffShiftEnd" class="block text-slate-400 font-medium mb-1">Shift End</label><input type="text" id="newStaffShiftEnd" value="05:00 PM" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-2.5 py-2 text-white text-center font-mono"></div>
          <div><label for="newStaffBreak" class="block text-slate-400 font-medium mb-1">Break</label><input type="text" id="newStaffBreak" value="12:30 PM - 01:15 PM" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-2 py-2 text-white text-center font-mono text-[11px]"></div>
        </div>
        <div><label for="newStaffStatus" class="block text-slate-400 font-medium mb-1">Initial Status</label><select id="newStaffStatus" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-bold"><option value="Checked In">Checked In</option><option value="Shift Not Checked-In">Shift Not Checked-In</option><option value="On Break">On Break</option></select></div>
        <div class="p-3 bg-slate-950/80 rounded-xl border border-slate-800 flex justify-end space-x-2 shrink-0 pt-3">
          <button type="button" onclick="closeAddStaffModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs px-4 py-2 rounded-xl font-semibold transition">Cancel</button>
          <button type="submit" class="bg-indigo-600 hover:bg-indigo-500 text-white text-xs px-5 py-2 rounded-xl font-bold transition flex items-center space-x-1.5"><i class="fa-solid fa-user-check"></i><span>Register Doctor</span></button>
        </div>
      </form>
    </div>
  </div>

  <!-- Patient Registration modal -->
  <div id="registrationModal" role="dialog" aria-modal="true" aria-hidden="true" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-md hidden flex items-center justify-center p-3 sm:p-6">
    <div class="glass-panel bg-slate-900 border border-slate-700 w-full max-w-2xl rounded-2xl shadow-2xl overflow-hidden max-h-[85vh] flex flex-col">
      <div class="p-4 border-b border-slate-800 flex items-center justify-between bg-slate-950/80 shrink-0">
        <div class="flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-teal-500/10 text-teal-400 border border-teal-500/20 flex items-center justify-center text-lg shrink-0"><i class="fa-solid fa-id-card"></i></div>
          <div>
            <h3 class="text-base font-bold text-white">Patient & Newborn Admission Portal</h3>
            <p class="text-xs text-slate-400">Register new hospital inpatients or newborn infant delivery records</p>
          </div>
        </div>
        <button type="button" onclick="closeRegistrationModal()" class="w-8 h-8 rounded-lg bg-slate-800 text-slate-400 hover:text-white flex items-center justify-center transition"><i class="fa-solid fa-xmark text-sm"></i></button>
      </div>
      <div class="flex border-b border-slate-800 bg-slate-950/40 p-2 space-x-2 shrink-0">
        <button type="button" onclick="setRegType('patient')" id="regTypePatientBtn" class="flex-1 py-2 text-xs font-bold rounded-lg bg-teal-500/20 text-teal-300 border border-teal-500/30 flex items-center justify-center space-x-2">
          <i class="fa-solid fa-user-plus"></i><span>Standard Inpatient</span>
        </button>
        <button type="button" onclick="setRegType('newborn')" id="regTypeNewbornBtn" class="flex-1 py-2 text-xs font-bold rounded-lg bg-slate-800 text-slate-400 hover:text-white flex items-center justify-center space-x-2">
          <i class="fa-solid fa-baby"></i><span>Newborn Infant</span>
        </button>
      </div>
      <form id="registrationForm" onsubmit="handleRegistrationSubmit(event)" class="p-4 sm:p-6 space-y-4 overflow-y-auto text-xs text-slate-300">
        <div id="newbornFieldsContainer" class="hidden bg-purple-950/20 p-4 rounded-xl border border-purple-500/30 space-y-3">
          <h4 class="text-xs font-bold text-purple-300 uppercase tracking-wider flex items-center"><i class="fa-solid fa-baby-carriage mr-2"></i> Neonatal Birth & Mother Linkage</h4>
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
            <div><label for="regMotherId" class="block text-slate-400 font-medium mb-1">Mother Patient</label><select id="regMotherId" onchange="autoFillMotherDetails()" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"><option value="">-- Select Admitted Mother --</option></select></div>
            <div><label for="regNewbornName" class="block text-slate-400 font-medium mb-1">Baby Name</label><input type="text" id="regNewbornName" placeholder="Newborn Infant" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
          </div>
          <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
            <div><label for="regBirthWeight" class="block text-slate-400 font-medium mb-1">Weight (kg)</label><input type="number" step="0.1" id="regBirthWeight" value="3.2" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
            <div><label for="regApgarScore" class="block text-slate-400 font-medium mb-1">Apgar (0-10)</label><input type="number" min="0" max="10" id="regApgarScore" value="9" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-bold text-teal-400"></div>
            <div><label for="regGestationalAge" class="block text-slate-400 font-medium mb-1">Gestational Age</label><input type="text" id="regGestationalAge" value="38 Weeks" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
            <div><label for="regDeliveryMethod" class="block text-slate-400 font-medium mb-1">Delivery</label><select id="regDeliveryMethod" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-2.5 py-2 text-white">
              <option value="Spontaneous Vaginal">Vaginal</option><option value="Elective C-Section">Elective C-Section</option><option value="Emergency C-Section">Emergency C-Section</option><option value="Vacuum Assisted">Vacuum Assisted</option>
            </select></div>
          </div>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
          <div id="generalNameField"><label for="regPatientName" class="block text-slate-400 font-medium mb-1">Full Patient Name</label><input type="text" id="regPatientName" required placeholder="e.g. Youssef Alami" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
          <div><label for="regAge" class="block text-slate-400 font-medium mb-1">Age (Years)</label><input type="number" id="regAge" value="32" min="0" max="120" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
          <div><label for="regGender" class="block text-slate-400 font-medium mb-1">Gender</label><select id="regGender" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"><option value="Male">Male</option><option value="Female">Female</option></select></div>
        </div>
        <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
          <div><label for="regBloodType" class="block text-slate-400 font-medium mb-1">Blood Type</label><select id="regBloodType" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white">
            <option value="A+">A+</option><option value="O+">O+</option><option value="B+">B+</option><option value="AB+">AB+</option><option value="A-">A-</option><option value="O-">O-</option><option value="B-">B-</option>
          </select></div>
          <div><label for="regDept" class="block text-slate-400 font-medium mb-1">Department</label><select id="regDept" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white">
            <option value="Pediatrics">Pediatrics</option><option value="ICU">ICU</option><option value="ER">ER</option><option value="Surgery">Surgery</option><option value="Ward">Ward</option>
          </select></div>
          <div><label for="regRoom" class="block text-slate-400 font-medium mb-1">Room #</label><input type="text" id="regRoom" value="PED-12" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
          <div><label for="regAcuity" class="block text-slate-400 font-medium mb-1">Acuity</label><select id="regAcuity" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white font-bold">
            <option value="Moderate">Moderate</option><option value="High">High</option><option value="Critical">Critical</option><option value="Low">Low</option>
          </select></div>
        </div>
        <div><label for="regDiagnosis" class="block text-slate-400 font-medium mb-1">Primary Diagnosis</label><input type="text" id="regDiagnosis" value="Neonatal Observation & Post-Delivery Care" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></div>
        <div><label for="regHistory" class="block text-slate-400 font-medium mb-1">History & Summary</label><textarea id="regHistory" rows="2" class="w-full bg-slate-950 border border-slate-700 rounded-lg p-3 text-white">Admitted following delivery. Vital signs normal, vigorous cry, normal respiratory effort.</textarea></div>
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-3">
          <div><label for="regInsuranceProvider" class="block text-slate-400 font-medium mb-1">Insurance</label><select id="regInsuranceProvider" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white">
            <option value="CNSS / AXA Assurance Maroc">CNSS / AXA</option><option value="CNOPS / Sanlam Maroc">CNOPS / Sanlam</option><option value="RMA Wataniya / CNSS">RMA / CNSS</option><option value="Wafa Assurance / CNOPS">Wafa / CNOPS</option>
          </select></div>
          <div><label for="regDoctor" class="block text-slate-400 font-medium mb-1">Assigned Physician</label><select id="regDoctor" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></select></div>
          <div><label for="regNurse" class="block text-slate-400 font-medium mb-1">Assigned Nurse</label><select id="regNurse" class="w-full bg-slate-950 border border-slate-700 rounded-lg px-3 py-2 text-white"></select></div>
        </div>
        <div class="p-3 bg-slate-950/80 rounded-xl border border-slate-800 flex justify-end space-x-2 shrink-0 pt-3">
          <button type="button" onclick="closeRegistrationModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs px-4 py-2 rounded-xl font-semibold transition">Cancel</button>
          <button type="submit" class="bg-teal-600 hover:bg-teal-500 text-white text-xs px-5 py-2 rounded-xl font-bold transition"><i class="fa-solid fa-check mr-1.5"></i> Register Patient</button>
        </div>
      </form>
    </div>
  </div>

  <!-- Patient detail modal -->
  <div id="patientModal" role="dialog" aria-modal="true" aria-hidden="true" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-md hidden flex items-center justify-center p-3 sm:p-6">
    <div class="glass-panel bg-slate-900 border border-slate-700 w-full max-w-3xl rounded-2xl shadow-2xl overflow-hidden max-h-[85vh] flex flex-col">
      <div class="p-4 sm:p-5 border-b border-slate-800 flex items-center justify-between bg-slate-950/80 shrink-0">
        <div class="flex items-center space-x-3 min-w-0">
          <div id="modalPatientBadge" class="w-12 h-12 rounded-xl bg-teal-500/10 text-teal-400 border border-teal-500/20 flex items-center justify-center font-bold text-xs shrink-0 tracking-wider text-center">#001</div>
          <div class="min-w-0">
            <div class="flex items-center space-x-2 flex-wrap">
              <h3 id="modalPatientName" class="text-base sm:text-lg font-bold text-white truncate">Patient</h3>
              <span id="modalPatientDept" class="bg-sky-500/10 text-sky-400 border border-sky-500/20 text-[10px] font-bold px-2 py-0.5 rounded uppercase shrink-0">ICU</span>
              <span id="modalPatientBlood" class="bg-rose-500/10 text-rose-300 border border-rose-500/20 text-[10px] font-bold px-2 py-0.5 rounded shrink-0">Blood: A+</span>
            </div>
            <p id="modalPatientSub" class="text-xs text-slate-400 mt-0.5 truncate">ID: --</p>
          </div>
        </div>
        <button type="button" onclick="closePatientModal()" class="w-9 h-9 rounded-lg bg-slate-800 hover:bg-slate-700 text-slate-400 hover:text-white flex items-center justify-center transition shrink-0 ml-2"><i class="fa-solid fa-xmark text-lg"></i></button>
      </div>
      <div class="p-4 sm:p-6 space-y-5 overflow-y-auto text-xs text-slate-300 leading-relaxed min-w-0">
        <div id="modalNewbornDetails" class="hidden bg-purple-950/30 p-3.5 rounded-xl border border-purple-500/30 space-y-2">
          <h4 class="text-xs font-bold uppercase tracking-wider text-purple-300 flex items-center"><i class="fa-solid fa-baby mr-2"></i> Neonatal Delivery & Mother Linkage</h4>
          <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 text-xs">
            <div><span class="text-slate-400 block text-[10px]">Mother ID:</span><strong id="modalMotherId" class="text-purple-300">--</strong></div>
            <div><span class="text-slate-400 block text-[10px]">Birth Weight:</span><strong id="modalBirthWeight" class="text-white">--</strong></div>
            <div><span class="text-slate-400 block text-[10px]">Apgar Score:</span><strong id="modalApgarScore" class="text-teal-400 font-bold">--</strong></div>
            <div><span class="text-slate-400 block text-[10px]">Delivery Method:</span><strong id="modalDeliveryMethod" class="text-slate-200">--</strong></div>
          </div>
        </div>
        <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 bg-slate-950/60 p-3.5 rounded-xl border border-slate-800">
          <div><span class="text-slate-500 block text-[10px] uppercase font-semibold">Heart Rate</span><span id="modalVitalHR" class="font-bold text-rose-400 text-sm">--</span></div>
          <div><span class="text-slate-500 block text-[10px] uppercase font-semibold">Blood Pressure</span><span id="modalVitalBP" class="font-bold text-sky-400 text-sm">--</span></div>
          <div><span class="text-slate-500 block text-[10px] uppercase font-semibold">Oxygen (SpO2)</span><span id="modalVitalSpO2" class="font-bold text-teal-400 text-sm">--</span></div>
          <div><span class="text-slate-500 block text-[10px] uppercase font-semibold">Temperature</span><span id="modalVitalTemp" class="font-bold text-amber-400 text-sm">--</span></div>
        </div>
        <div class="space-y-2">
          <h4 class="text-xs font-bold uppercase tracking-wider text-amber-400 flex items-center"><i class="fa-solid fa-laptop-medical mr-2"></i> Assigned Room Machinery & Biomedical Status</h4>
          <div id="modalEquipmentBox" class="bg-slate-950/60 p-3.5 rounded-xl border border-slate-800 space-y-2"></div>
        </div>
        <div class="space-y-2">
          <h4 class="text-xs font-bold uppercase tracking-wider text-teal-400 flex items-center"><i class="fa-solid fa-file-medical mr-2"></i> Clinical History & Background</h4>
          <div class="bg-slate-950/60 p-3.5 rounded-xl border border-slate-800"><p id="modalMedicalHistory" class="text-slate-200 text-xs leading-relaxed">--</p></div>
        </div>
        <div class="space-y-2">
          <h4 class="text-xs font-bold uppercase tracking-wider text-teal-400 flex items-center"><i class="fa-solid fa-building-shield mr-2"></i> Insurance Provider & Coverage</h4>
          <div class="bg-slate-950/60 p-3.5 rounded-xl border border-slate-800 grid grid-cols-1 sm:grid-cols-3 gap-3">
            <div><span class="text-[10px] text-slate-500 uppercase font-semibold block">Provider</span><span id="modalInsProvider" class="font-bold text-white text-xs block truncate">--</span><span id="modalInsPolicy" class="text-[11px] text-slate-400 block font-mono mt-0.5">Policy: --</span></div>
            <div><span class="text-[10px] text-slate-500 uppercase font-semibold block">Coverage Ratio</span><div class="flex items-center space-x-2 mt-0.5"><span id="modalInsCoverage" class="font-bold text-emerald-400 text-sm">--</span><span id="modalInsCopay" class="text-[10px] bg-slate-800 px-2 py-0.5 rounded text-slate-300 font-semibold border border-slate-700">--</span></div></div>
            <div><span class="text-[10px] text-slate-500 uppercase font-semibold block">Annual Cap</span><span id="modalInsMaxLimit" class="font-bold text-slate-200 text-xs block">--</span><span class="text-[10px] text-teal-400 block mt-0.5"><i class="fa-solid fa-circle-check"></i> Pre-Approved</span></div>
          </div>
        </div>
        <div class="space-y-2">
          <h4 class="text-xs font-bold uppercase tracking-wider text-sky-400 flex items-center"><i class="fa-solid fa-user-doctor mr-2"></i> Doctor Consultation Summary & Directives</h4>
          <div class="bg-slate-950/60 p-3.5 rounded-xl border border-slate-800 space-y-3">
            <div><span class="text-[10px] text-slate-500 uppercase font-semibold block">Physician Findings</span><p id="modalDoctorSummary" class="text-slate-200 text-xs font-medium leading-relaxed mt-1">--</p></div>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 pt-2 border-t border-slate-800/80">
              <div><span class="text-[10px] text-slate-500 uppercase font-semibold block">Directives</span><p id="modalPatientNeeds" class="text-slate-300 text-xs mt-0.5">--</p></div>
              <div><span class="text-[10px] text-slate-500 uppercase font-semibold block">Surgery / Procedure</span><p id="modalSurgeryInfo" class="text-rose-300 font-semibold text-xs mt-0.5">--</p></div>
            </div>
          </div>
        </div>
        <div class="space-y-2">
          <h4 class="text-xs font-bold uppercase tracking-wider text-purple-400 flex items-center"><i class="fa-solid fa-pills mr-2"></i> Prescribed Medications</h4>
          <div class="bg-slate-950/60 rounded-xl border border-slate-800 overflow-x-auto">
            <table class="w-full text-left text-xs min-w-[500px]">
              <thead class="bg-slate-900 text-slate-400 uppercase font-semibold border-b border-slate-800">
                <tr><th class="py-2.5 px-3">Medication</th><th class="py-2.5 px-3">Dosage</th><th class="py-2.5 px-3">Schedule</th><th class="py-2.5 px-3 text-right">Cost (MAD)</th></tr>
              </thead>
              <tbody id="modalMedsTableBody" class="divide-y divide-slate-800/60 text-slate-300"></tbody>
            </table>
          </div>
        </div>
        <div class="space-y-2">
          <h4 class="text-xs font-bold uppercase tracking-wider text-emerald-400 flex items-center"><i class="fa-solid fa-file-invoice-dollar mr-2"></i> Financial Ledger & Billing (MAD)</h4>
          <div class="bg-slate-950/80 p-3.5 rounded-xl border border-slate-800 space-y-3">
            <div class="space-y-2 text-xs divide-y divide-slate-800/60">
              <div class="flex justify-between pb-1.5"><span class="text-slate-400">Consultation Fee:</span><span id="billConsultFee" class="font-semibold text-slate-200">--</span></div>
              <div class="flex justify-between py-1.5"><span class="text-slate-400">Surgery / Procedure Fee:</span><span id="billSurgeryCost" class="font-semibold text-slate-200">--</span></div>
              <div class="flex justify-between py-1.5"><span class="text-slate-400">Room Stay (<span id="billRoomDays">--</span>):</span><span id="billRoomTotal" class="font-semibold text-slate-200">--</span></div>
              <div class="flex justify-between py-1.5"><span class="text-slate-400">Medications Total:</span><span id="billMedsTotal" class="font-semibold text-slate-200">--</span></div>
              <div class="flex justify-between pt-2 text-sm font-bold border-t border-slate-700"><span class="text-white">Gross Total:</span><span id="billGrossTotal" class="text-white">--</span></div>
            </div>
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 pt-2">
              <div class="bg-emerald-950/30 p-3 rounded-lg border border-emerald-500/30"><span class="text-[10px] text-emerald-400 font-bold uppercase block">Paid by Insurance (<span id="billInsPctTag">--</span>)</span><span id="billInsPaidAmount" class="text-base sm:text-lg font-bold text-emerald-300">--</span></div>
              <div class="bg-amber-950/30 p-3 rounded-lg border border-amber-500/30"><span class="text-[10px] text-amber-400 font-bold uppercase block">Patient Co-Pay</span><span id="billPatientBalance" class="text-base sm:text-lg font-bold text-amber-300">--</span></div>
            </div>
          </div>
        </div>
      </div>
      <div class="p-4 border-t border-slate-800 bg-slate-950/90 flex justify-between items-center shrink-0">
        <span class="text-[11px] text-slate-400"><i class="fa-solid fa-lock text-teal-400 mr-1"></i> Encrypted Medical Record</span>
        <button type="button" onclick="closePatientModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 text-xs px-4 py-2 rounded-lg font-semibold transition">Close Record</button>
      </div>
    </div>
  </div>

  <!-- Broadcast modal -->
  <div id="broadcastModal" role="dialog" aria-modal="true" aria-hidden="true" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-md hidden flex items-center justify-center p-4">
    <div class="glass-panel bg-slate-900 border border-rose-500/40 w-full max-w-md rounded-2xl shadow-2xl p-6 text-center space-y-4">
      <div class="w-14 h-14 rounded-full bg-rose-500/20 text-rose-400 border border-rose-500/30 flex items-center justify-center mx-auto text-2xl animate-bounce"><i class="fa-solid fa-tower-broadcast"></i></div>
      <h3 class="text-lg font-bold text-white">Code Red Emergency Broadcast Sent</h3>
      <p class="text-xs text-slate-300 leading-relaxed">Emergency callout dispatched to all <span class="text-emerald-400 font-bold">Checked-In Staff</span> via SMS & Push Alerts.</p>
      <button type="button" onclick="closeBroadcastModal()" class="w-full bg-rose-600 hover:bg-rose-500 text-white text-xs py-2.5 rounded-xl font-bold transition">Acknowledge & Dismiss</button>
    </div>
  </div>

  <!-- ============================================================ -->
  <!-- APPLICATION LOGIC                                            -->
  <!-- ============================================================ -->
  <script>
    /**
     * MediPulse CHUSys Platform & DeepSeek AI Engine
     * Repository: https://github.com/linabhsn/bionics-GRP
     * Authors: Lina Bouhsissin, Hamza Boumarg, Malak Jaouane
     */

    // Configuration
    const CONFIG = {
      ADMIN_PIN: "1234",
      COST_PER_SHIFT_MAD: 3800,
      DEFAULT_DEPT: "ER",
      DEEPSEEK_API_URL: "https://api.deepseek.com/v1/chat/completions",
      DEEPSEEK_MODEL: "deepseek-chat"
    };

    // State
    let isAdmin = false;
    let currentDept = CONFIG.DEFAULT_DEPT;
    let historicalData = [];
    let forecastData = [];
    let chartInstance = null;
    let selectedPatientDeptFilter = 'ALL';
    let selectedEquipmentCategoryFilter = 'ALL';
    let registrationType = 'patient';

    // Utilities
    const escapeHtml = (v) => v == null ? '' : String(v)
      .replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;').replace(/'/g, '&#39;');

    const formatMAD = (n) => `${Math.round(Number(n) || 0).toLocaleString('en-US')} MAD`;
    const formatMAD2 = (n) => `${(Number(n) || 0).toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 })} MAD`;

    function logTerminal(msg) {
      const term = document.getElementById("console-terminal");
      if (term) {
        const p = document.createElement("p");
        p.innerText = `[${new Date().toLocaleTimeString()}] ${msg}`;
        term.appendChild(p);
        term.scrollTop = term.scrollHeight;
      }
    }

    function clearConsoleLog() {
      const term = document.getElementById("console-terminal");
      if (term) term.innerHTML = `<p class="text-slate-500">[Console Cleared]</p>`;
    }

    // Modal Manager
    function openModal(id) {
      const m = document.getElementById(id);
      if (!m) return;
      m.classList.remove('hidden');
      m.setAttribute('aria-hidden', 'false');
      document.body.classList.add('overflow-hidden');
    }

    function closeModal(id) {
      const m = document.getElementById(id);
      if (!m || m.classList.contains('hidden')) return;
      m.classList.add('hidden');
      m.setAttribute('aria-hidden', 'true');
      if (!document.querySelector('[role="dialog"]:not(.hidden)')) {
        document.body.classList.remove('overflow-hidden');
      }
    }

    function initModalBehavior() {
      document.querySelectorAll('[role="dialog"]').forEach((m) => {
        m.setAttribute('aria-hidden', m.classList.contains('hidden') ? 'true' : 'false');
        m.addEventListener('mousedown', (e) => { if (e.target === m) closeModal(m.id); });
      });
      document.addEventListener('keydown', (e) => {
        if (e.key !== 'Escape') return;
        const open = document.querySelectorAll('[role="dialog"]:not(.hidden)');
        if (open.length) closeModal(open[open.length - 1].id);
      });
    }

    // Toasts
    function showToast(title, message, type = 'success') {
      const c = document.getElementById('toastContainer');
      while (c.children.length >= 4) c.removeChild(c.firstElementChild);
      const t = document.createElement('div');
      let border = 'border-teal-500/40 bg-slate-900', icon = 'fa-circle-check text-teal-400';
      if (type === 'amber') { border = 'border-amber-500/40 bg-slate-900'; icon = 'fa-triangle-exclamation text-amber-400'; }
      if (type === 'rose') { border = 'border-rose-500/40 bg-slate-900'; icon = 'fa-wrench text-rose-400'; }
      if (type === 'indigo') { border = 'border-indigo-500/40 bg-slate-900'; icon = 'fa-user-doctor text-indigo-400'; }
      t.className = `glass-panel border ${border} p-3.5 rounded-xl shadow-xl flex items-start space-x-3 transition-all duration-300 transform translate-y-2 cursor-pointer`;
      t.innerHTML = `<i class="fa-solid ${icon} text-base mt-0.5 shrink-0"></i><div class="min-w-0 flex-1"><h5 class="text-xs font-bold text-white">${escapeHtml(title)}</h5><p class="text-[11px] text-slate-300 mt-0.5 leading-snug">${escapeHtml(message)}</p></div>`;
      t.addEventListener('click', () => t.remove());
      c.appendChild(t);
      setTimeout(() => { t.classList.add('opacity-0', 'translate-y-2'); setTimeout(() => t.remove(), 300); }, 4000);
    }

    // ==========================================
    // DEEPSEEK AI ENGINE & PREDICTIVE LOGIC
    // ==========================================

    async function fetchDeepSeekSurgePrediction(histArray, dept) {
      logTerminal(`⚡ Requesting DeepSeek API... Target Dept: ${dept}`);
      const apiKey = localStorage.getItem("CHUSYS_DEEPSEEK_KEY");

      if (!apiKey) {
        logTerminal("⚠️ No API Key found in LocalStorage. Routing request to Mathematical MA7 Fallback.");
        return executeLocalFallbackPrediction(histArray, dept);
      }

      try {
        const response = await fetch(CONFIG.DEEPSEEK_API_URL, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "Authorization": `Bearer ${apiKey}`
          },
          body: JSON.stringify({
            model: CONFIG.DEEPSEEK_MODEL,
            messages: [
              {
                role: "system",
                content: "You are CHUSys AI. Calculate 7-day patient forecast for hospital departments. Return JSON ONLY matching key: 'forecast' (array of 7 numbers) and 'rationale' (string)."
              },
              {
                role: "user",
                content: `Department: ${dept}, Past 14 Days History: ${JSON.stringify(histArray)}`
              }
            ],
            response_format: { type: "json_object" },
            temperature: 0.2
          })
        });

        if (!response.ok) throw new Error(`HTTP ${response.status}`);
        const data = await response.json();
        const content = JSON.parse(data.choices[0].message.content);
        logTerminal(`✅ DeepSeek Response Received! Forecast Array: ${JSON.stringify(content.forecast)}`);
        return {
          forecast: content.forecast,
          rationale: content.rationale || "DeepSeek open-weight inference completed based on time-series history.",
          source: "DeepSeek Open-Weight AI"
        };
      } catch (err) {
        logTerminal(`⚠️ API Error (${err.message}). Falling back to local MA7 model.`);
        return executeLocalFallbackPrediction(histArray, dept);
      }
    }

    function executeLocalFallbackPrediction(histArray, dept) {
      logTerminal("📐 Executing MA7 Formula: Forecast_t = (1/7) ∑ Y_{t-k} * DeptFactor");
      const sum = histArray.slice(-7).reduce((a, b) => a + b, 0);
      const ma7 = sum / 7;
      const deptMults = { ER: 1.15, ICU: 1.05, Ward: 0.98, Surgery: 1.10, Pediatrics: 1.08 };
      const factor = deptMults[dept] || 1.0;

      const forecast = [];
      for (let i = 1; i <= 7; i++) {
        forecast.push(Math.round(ma7 * factor + (i % 2 === 0 ? 2 : -1)));
      }

      return {
        forecast,
        rationale: `Computed using 7-day rolling moving average MA7 (${ma7.toFixed(1)}) weighted by department factor (${factor}x).`,
        source: "Local MA7 Model"
      };
    }

    async function runSurgePrediction() {
      const histValues = historicalData.map(d => d.value);
      const res = await fetchDeepSeekSurgePrediction(histValues, currentDept);

      const now = new Date();
      forecastData = res.forecast.map((val, idx) => {
        const d = new Date(now); d.setDate(d.getDate() + idx + 1);
        return {
          date: d.toLocaleDateString('en-US', { month: 'short', day: 'numeric' }),
          dayName: d.toLocaleDateString('en-US', { weekday: 'short' }),
          value: val
        };
      });

      document.getElementById("badgeModelSource").innerText = `Model: ${res.source}`;
      document.getElementById("deepseekRationaleText").innerText = res.rationale;

      updateChart();
      updateCalculations();
      showToast("Forecast Refreshed", `Calculated 7-day influx via ${res.source}.`, "success");
    }

    // Modal API Config
    function openApiKeyModal() { openModal("apiKeyModal"); }
    function closeApiKeyModal() { closeModal("apiKeyModal"); }
    function saveApiKey() {
      const k = document.getElementById("deepseekApiKeyInput").value.trim();
      if (k) localStorage.setItem("CHUSYS_DEEPSEEK_KEY", k);
      closeApiKeyModal();
      showToast("Key Saved", "DeepSeek API key stored locally.", "success");
    }

    // Seed Data
    let equipmentList = [
      { id: "EQ-101", name: "Hamilton C6 ICU Ventilator", category: "Patient Room", type: "ICU Ventilator", location: "ICU Room #04 (Patient #001)", assignedPatientId: "PAT-001", assignedDept: "ICU", serialNo: "SN-VT-88392", status: "Operational", lastService: "2026-09-12", nextCalibration: "2027-03-12", errorCode: "None (Self-Test Passed)", engineer: "Ing. Hassan Alami (+212 661-889900)" },
      { id: "EQ-102", name: "Mindray BeneVision N17 Patient Monitor", category: "Patient Room", type: "Bedside Patient Monitor", location: "ER Room #12 (Patient #002)", assignedPatientId: "PAT-002", assignedDept: "ER", serialNo: "SN-PM-49201", status: "Needs Repair", lastService: "2026-08-01", nextCalibration: "2026-11-01", errorCode: "Error E-402: SpO2 Sensor Drift", engineer: "Ing. Youssef Berrada (+212 662-778899)" },
      { id: "EQ-103", name: "Maquet Flow-i Anesthesia Workstation", category: "Operating Room", type: "Anesthesia Workstation", location: "OR Suite #02 (Patient #003)", assignedPatientId: "PAT-003", assignedDept: "Surgery", serialNo: "SN-AN-77102", status: "Operational", lastService: "2026-10-05", nextCalibration: "2027-04-05", errorCode: "None", engineer: "Ing. Hassan Alami (+212 661-889900)" },
      { id: "EQ-104", name: "GE OEC 3D C-Arm Fluoroscopy", category: "Operating Room", type: "C-Arm Fluoroscopy", location: "OR Suite #05 (Patient #007)", assignedPatientId: "PAT-007", assignedDept: "Surgery", serialNo: "SN-CA-30192", status: "Needs Repair", lastService: "2026-07-15", nextCalibration: "2026-10-15", errorCode: "Error E-901: X-Ray Tube High Temp", engineer: "Ing. Omar Chraibi (+212 663-667788)" }
    ];

    let patientsList = [
      { id: "PAT-001", code: "Patient #001", name: "Ahmed El Amrani", isNewborn: false, age: 45, gender: "Male", bloodType: "A+", dept: "ICU", room: "ICU-04", diagnosis: "Acute Lymphoblastic Leukemia (ALL)", history: "Post-chemo neutropenic fever surveillance.", acuity: "High", vitals: { hr: "88 bpm", bp: "118/78", spo2: "97%", temp: "37.2 °C" }, doctor: "Dr. Ahmed Bennani", nurse: "RN Fatima Berrada", insurance: { provider: "CNSS / AXA Assurance Maroc", policyNo: "POL-883920", coveragePct: 85, maxLimit: "100,000 MAD", copay: "15%" }, consultation: { summary: "Isolation & IV antibiotics.", needs: "Cardiac telemetry; daily CBC.", surgery: "CV Line Placement", surgeryCost: 42000 }, medications: [{ name: "Cytarabine Injection", dosage: "100mg IV", schedule: "Matin & Soir", cost: 4500 }], billing: { consultFee: 2000, roomDailyCost: 8000, roomDays: 5 } },
      { id: "PAT-002", code: "Patient #002", name: "Meriem Bennis", isNewborn: false, age: 62, gender: "Female", bloodType: "O+", dept: "ER", room: "ER-12", diagnosis: "Severe Refractory Asthma", history: "Insulin dependent. Severe dyspnea on admission.", acuity: "Critical", vitals: { hr: "110 bpm", bp: "142/90", spo2: "91%", temp: "36.8 °C" }, doctor: "Dr. Sarah Alami", nurse: "RN Younes Jabri", insurance: { provider: "CNOPS / Sanlam Maroc", policyNo: "POL-492019", coveragePct: 80, maxLimit: "50,000 MAD", copay: "20%" }, consultation: { summary: "Continuous nebulized Albuterol.", needs: "High-flow nasal cannula.", surgery: "Emergency Intubation", surgeryCost: 38000 }, medications: [{ name: "Methylprednisolone", dosage: "125mg IV", schedule: "Q6H", cost: 1800 }], billing: { consultFee: 1500, roomDailyCost: 6000, roomDays: 2 } }
    ];

    let staffList = [
      { id: "STF-01", name: "Dr. Ahmed Bennani", role: "Doctor", specialty: "Critical Care / Intensivist", dept: "ICU", status: "Checked In", shiftStart: "07:00 AM", shiftEnd: "07:00 PM", breakWindow: "01:00 PM - 01:45 PM", checkInTime: "06:52 AM", phone: "+212 661-123456", email: "a.bennani@chu-maroc.ma" },
      { id: "STF-02", name: "Dr. Sarah Alami", role: "Doctor", specialty: "Emergency Medicine", dept: "ER", status: "Shift Not Checked-In", shiftStart: "07:00 AM", shiftEnd: "03:00 PM", breakWindow: "11:30 AM - 12:00 PM", checkInTime: "--:--", phone: "+212 662-234567", email: "s.alami@chu-maroc.ma" },
      { id: "STF-06", name: "RN Fatima Berrada", role: "Nurse", specialty: "ICU Charge Nurse", dept: "ICU", status: "Checked In", shiftStart: "07:00 AM", shiftEnd: "07:00 PM", breakWindow: "12:00 PM - 12:45 PM", checkInTime: "06:48 AM", phone: "+212 666-678901", email: "f.berrada@chu-maroc.ma" }
    ];

    const deptPresets = {
      'ER': { name: 'Emergency Room', baseVolume: 65, volatility: 18, nurseRatio: 4, doctorRatio: 12, acuity: 1.25, scheduledNurses: 11, scheduledDoctors: 4, peakHours: '14:00 - 22:00', turnaround: '4.2 Hours', safetyBuffer: '+15% Capacity', description: 'Emergency Room departments experience high volatility.' },
      'ICU': { name: 'Intensive Care Unit', baseVolume: 24, volatility: 5, nurseRatio: 2, doctorRatio: 6, acuity: 1.85, scheduledNurses: 14, scheduledDoctors: 5, peakHours: 'Continuous 24/7', turnaround: '36.5 Hours', safetyBuffer: '+25% Capacity', description: 'Intensive Care Units require strict 1:1 or 2:1 nurse ratios.' }
    };

    // Financials
    function calcPatientFinancials(p) {
      const meds = Array.isArray(p.medications) ? p.medications : [];
      const b = p.billing || {};
      const c = p.consultation || {};
      const ins = p.insurance || { coveragePct: 0 };
      const medsTotal = meds.reduce((s, m) => s + (Number(m.cost) || 0), 0);
      const roomTotal = (Number(b.roomDailyCost) || 0) * (Number(b.roomDays) || 0);
      const gross = (Number(b.consultFee) || 0) + (Number(c.surgeryCost) || 0) + roomTotal + medsTotal;
      const coveragePct = Number(ins.coveragePct) || 0;
      return {
        medsTotal, roomTotal, gross, coveragePct,
        insurancePaid: gross * (coveragePct / 100),
        patientBalance: gross - (gross * (coveragePct / 100))
      };
    }

    function calcFinancialTotals() {
      let gross = 0, ins = 0, pat = 0;
      patientsList.forEach(p => { const f = calcPatientFinancials(p); gross += f.gross; ins += f.insurancePaid; pat += f.patientBalance; });
      document.getElementById('statTotalPatients').innerText = `${patientsList.length} Patients`;
      document.getElementById('statGrossBilling').innerText = formatMAD(gross);
      document.getElementById('statInsurancePaid').innerText = formatMAD(ins);
      document.getElementById('statPatientBalance').innerText = formatMAD(pat);
    }

    // Tab Navigation
    function switchTab(tabId) {
      document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
      document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
      document.getElementById(tabId).classList.remove('hidden');
      const btn = document.getElementById('btn-' + tabId);
      if (btn) btn.classList.add('active');
      if (tabId === 'forecastTab' && chartInstance) setTimeout(() => chartInstance.resize(), 100);
    }

    // Forecast Initialization & Updates
    function generateSyntheticData() {
      historicalData = []; forecastData = [];
      const p = deptPresets[currentDept] || deptPresets['ER'];
      const now = new Date();
      for (let i = 14; i >= 1; i--) {
        const d = new Date(now); d.setDate(d.getDate() - i);
        historicalData.push({ date: d.toLocaleDateString('en-US', { month: 'short', day: 'numeric' }), value: Math.max(10, Math.round(p.baseVolume + (Math.random() - 0.5) * p.volatility)) });
      }
      for (let i = 1; i <= 7; i++) {
        const d = new Date(now); d.setDate(d.getDate() + i);
        forecastData.push({
          date: d.toLocaleDateString('en-US', { month: 'short', day: 'numeric' }),
          dayName: d.toLocaleDateString('en-US', { weekday: 'short' }),
          value: Math.max(12, Math.round(p.baseVolume + (Math.random() - 0.5) * p.volatility))
        });
      }
    }

    function initChart() {
      const ctx = document.getElementById('predictiveChart').getContext('2d');
      chartInstance = new Chart(ctx, {
        type: 'line',
        data: {
          labels: [...historicalData.map(d => d.date), ...forecastData.map(d => d.date + ' (F)')],
          datasets: [
            { label: 'Historical', data: historicalData.map(d => d.value), borderColor: '#14b8a6', backgroundColor: 'rgba(20, 184, 166, 0.08)', fill: true },
            { label: 'AI Forecast', data: new Array(historicalData.length - 1).fill(null).concat([historicalData[historicalData.length-1].value], forecastData.map(d => d.value)), borderColor: '#38bdf8', borderDash: [6, 6], fill: true }
          ]
        },
        options: { responsive: true, maintainAspectRatio: false, plugins: { legend: { display: false } } }
      });
    }

    function updateChart() {
      if (!chartInstance) return;
      chartInstance.data.labels = [...historicalData.map(d => d.date), ...forecastData.map(d => d.date + ' (F)')];
      chartInstance.data.datasets[0].data = historicalData.map(d => d.value);
      chartInstance.data.datasets[1].data = new Array(historicalData.length - 1).fill(null).concat([historicalData[historicalData.length-1].value], forecastData.map(d => d.value));
      chartInstance.update();
    }

    function updateCalculations() {
      const nR = parseInt(document.getElementById('nurseRatioSlider').value);
      const dR = parseInt(document.getElementById('doctorRatioSlider').value);
      const total = forecastData.reduce((s, i) => s + i.value, 0);
      document.getElementById('kpiForecastTotal').innerText = total;
      document.getElementById('kpiNursesNeeded').innerText = Math.ceil((total / 7) / nR);
      document.getElementById('kpiDoctorsNeeded').innerText = Math.ceil((total / 7) / dR);
    }

    // Renderers
    function renderPatients() {
      const grid = document.getElementById('patientGrid');
      grid.innerHTML = patientsList.map(p => `
        <div class="glass-panel rounded-2xl p-5 border border-slate-800 space-y-3">
          <div class="flex justify-between items-center">
            <h4 class="font-bold text-white">${escapeHtml(p.name)}</h4>
            <span class="text-xs font-bold text-teal-400">${escapeHtml(p.dept)}</span>
          </div>
          <p class="text-xs text-slate-400">${escapeHtml(p.diagnosis)}</p>
          <button onclick="openPatientModal('${p.id}')" class="text-xs bg-teal-600/20 text-teal-300 px-3 py-1.5 rounded-lg border border-teal-500/30">View Record</button>
        </div>
      `).join('');
    }

    function renderEquipment() {
      const grid = document.getElementById('equipmentGrid');
      grid.innerHTML = equipmentList.map(e => `
        <div class="glass-panel rounded-2xl p-5 border border-slate-800 space-y-3">
          <div class="flex justify-between items-center">
            <h4 class="font-bold text-white">${escapeHtml(e.name)}</h4>
            <span class="text-xs font-bold ${e.status === 'Needs Repair' ? 'text-rose-400' : 'text-emerald-400'}">${escapeHtml(e.status)}</span>
          </div>
          <p class="text-xs text-slate-400">Location: ${escapeHtml(e.location)}</p>
        </div>
      `).join('');
    }

    function renderStaff() {
      const grid = document.getElementById('staffGrid');
      grid.innerHTML = staffList.map(s => `
        <div class="glass-panel rounded-2xl p-5 border border-slate-800 space-y-3">
          <div class="flex justify-between items-center">
            <h4 class="font-bold text-white">${escapeHtml(s.name)}</h4>
            <span class="text-xs text-indigo-400 font-bold">${escapeHtml(s.role)}</span>
          </div>
          <p class="text-xs text-slate-400">${escapeHtml(s.specialty)}</p>
        </div>
      `).join('');
    }

    function renderEmergencyDispatch() {
      const grid = document.getElementById('emergencyDispatchGrid');
      grid.innerHTML = staffList.filter(s => s.status === 'Checked In').map(s => `
        <div class="glass-panel rounded-2xl p-4 border border-slate-800 flex justify-between items-center">
          <div><h4 class="font-bold text-xs text-white">${escapeHtml(s.name)}</h4><span class="text-[10px] text-teal-400">${escapeHtml(s.phone)}</span></div>
          <a href="tel:${s.phone}" class="bg-rose-600 text-white text-xs px-3 py-1.5 rounded-lg font-bold"><i class="fa-solid fa-phone"></i> Call</a>
        </div>
      `).join('');
    }

    // Admin & Test Functions
    function toggleAdminModeBtn() { openModal('adminPinModal'); }
    function verifyAdminPin() {
      if (document.getElementById('adminPinInput').value === CONFIG.ADMIN_PIN) {
        isAdmin = true;
        closeModal('adminPinModal');
        document.getElementById('adminModeBanner').classList.remove('hidden');
        showToast("Admin Unlocked", "Admin access granted.", "success");
      }
    }
    function lockAdminMode() { isAdmin = false; document.getElementById('adminModeBanner').classList.add('hidden'); }

    function testApiDirectly() { switchTab('forecastTab'); runSurgePrediction(); }
    function testFallbackDirectly() {
      const res = executeLocalFallbackPrediction([40, 42, 38, 45, 50, 62, 58], currentDept);
      document.getElementById("deepseekRationaleText").innerText = res.rationale;
      showToast("Fallback Executed", "Calculated local MA7 prediction.", "amber");
    }

    // Window Load Attachment
    window.addEventListener('DOMContentLoaded', () => {
      initModalBehavior();
      generateSyntheticData();
      initChart();
      updateCalculations();
      calcFinancialTotals();
      renderPatients();
      renderEquipment();
      renderStaff();
      renderEmergencyDispatch();
      logTerminal("🏥 CHUSys Platform Initialized.");
    });

    // Global Window Expose
    Object.assign(window, {
      fetchDeepSeekSurgePrediction, executeLocalFallbackPrediction, runSurgePrediction,
      switchTab, openApiKeyModal, closeApiKeyModal, saveApiKey, toggleAdminModeBtn,
      verifyAdminPin, lockAdminMode, testApiDirectly, testFallbackDirectly, clearConsoleLog
    });
  </script>
</body>
</html>