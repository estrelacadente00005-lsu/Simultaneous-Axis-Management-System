<html lang="pt-BR" class="h-full bg-slate-950 text-slate-100 select-none">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SAMS Dimensional - Interface de Controle Hexa-Dimensional (Hex-IC)</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;800;900&family=JetBrains+Mono:wght@300;400;600;700&display=swap');

        body {
            font-family: 'JetBrains Mono', monospace;
            background-color: #030712;
            color: #f1f5f9;
            overflow-x: hidden;
        }

        .font-orbitron {
            font-family: 'Orbitron', sans-serif;
        }

        /* Glassmorphism & Cyberpunk glow utility classes */
        .cyber-card {
            background: rgba(15, 23, 42, 0.75);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(56, 189, 248, 0.2);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5), inset 0 0 12px rgba(56, 189, 248, 0.05);
        }

        .cyber-card-danger {
            border-color: rgba(244, 63, 94, 0.5);
            box-shadow: 0 0 20px rgba(244, 63, 94, 0.2), inset 0 0 15px rgba(244, 63, 94, 0.1);
        }

        .cyber-card-success {
            border-color: rgba(34, 197, 94, 0.5);
            box-shadow: 0 0 20px rgba(34, 197, 94, 0.2), inset 0 0 15px rgba(34, 197, 94, 0.1);
        }

        .glow-text-cyan {
            text-shadow: 0 0 10px rgba(6, 182, 212, 0.7), 0 0 20px rgba(6, 182, 212, 0.4);
        }

        .glow-text-rose {
            text-shadow: 0 0 10px rgba(244, 63, 94, 0.8), 0 0 20px rgba(244, 63, 94, 0.5);
        }

        /* Custom Range Slider Styling */
        input[type=range] {
            -webkit-appearance: none;
            background: rgba(30, 41, 59, 0.8);
            border-radius: 6px;
            height: 6px;
            border: 1px solid rgba(56, 189, 248, 0.3);
        }

        input[type=range]::-webkit-slider-thumb {
            -webkit-appearance: none;
            height: 18px;
            width: 18px;
            border-radius: 50%;
            background: #06b6d4;
            cursor: pointer;
            box-shadow: 0 0 10px #06b6d4;
            transition: transform 0.1s, background-color 0.2s;
        }

        input[type=range]::-webkit-slider-thumb:hover {
            transform: scale(1.2);
            background: #38bdf8;
        }

        /* Grid Background Pattern */
        .bg-grid {
            background-size: 30px 30px;
            background-image: 
                linear-gradient(to right, rgba(56, 189, 248, 0.05) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(56, 189, 248, 0.05) 1px, transparent 1px);
        }

        /* CRT Screen Scanline effect */
        .scanlines {
            position: relative;
            overflow: hidden;
        }
        .scanlines::before {
            content: " ";
            display: block;
            position: absolute;
            top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%);
            background-size: 100% 4px;
            z-index: 20;
            pointer-events: none;
            opacity: 0.6;
        }

        @keyframes pulse-fast {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.3; }
        }

        .animate-pulse-fast {
            animation: pulse-fast 0.6s infinite;
        }
    </style>
</head>
<body class="h-full bg-grid scanlines flex flex-col min-h-screen text-xs md:text-sm">

    <!-- TOP HEADER / SYSTEM STATUS BAR -->
    <header class="border-b border-cyan-500/30 bg-slate-900/90 px-4 py-3 flex flex-wrap justify-between items-center z-30 backdrop-blur-md gap-4">
        <div class="flex items-center gap-3">
            <div class="p-2 bg-cyan-950/80 border border-cyan-500/50 rounded-lg text-cyan-400">
                <i data-lucide="atom" class="w-6 h-6 animate-spin-slow"></i>
            </div>
            <div>
                <h1 class="font-orbitron font-black text-lg md:text-xl text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 via-sky-200 to-indigo-400 tracking-wider">
                    SAMS <span class="text-xs font-normal text-cyan-500 border border-cyan-500/30 px-1.5 py-0.5 rounded ml-1">v6.0.4-TOPOLOGY</span>
                </h1>
                <p class="text-[10px] text-slate-400 font-mono tracking-widest uppercase">Sistema de Gerenciamento de Eixos Simultâneos • Hex-IC</p>
            </div>
        </div>

        <!-- System Controls & Target Universe Selector -->
        <div class="flex flex-wrap items-center gap-4">
            <div class="flex items-center gap-2 bg-slate-950/80 border border-slate-800 px-3 py-1.5 rounded-lg">
                <i data-lucide="globe" class="w-4 h-4 text-cyan-400"></i>
                <label for="universe-select" class="text-xs text-slate-400 font-semibold uppercase">Universo Alvo:</label>
                <select id="universe-select" class="bg-slate-900 text-cyan-300 font-mono text-xs border border-cyan-500/30 rounded px-2 py-1 outline-none focus:border-cyan-400">
                    <option value="alpha">Universo Alfa (Origem Standard)</option>
                    <option value="reptile_grav">Universo Réptil (Gravidade 2.0x)</option>
                    <option value="high_entropy">Universo Causal-Caótico (Alta Entropia)</option>
                    <option value="electro_mirror">Universo Espelho Electro-Fraco (Permitividade ε x0.4)</option>
                    <option value="dense_vacuum">Universo Super-Denso (Vácuo ρ x3.2)</option>
                    <option value="custom">Personalizado (Manual)</option>
                </select>
            </div>

            <!-- Emergency Reversion Button -->
            <button id="btn-emergency" class="px-4 py-2 bg-rose-950/80 hover:bg-rose-900 border border-rose-600/60 text-rose-300 font-orbitron text-xs font-bold rounded-lg shadow-lg shadow-rose-950/50 flex items-center gap-2 transition-all active:scale-95">
                <i data-lucide="shield-alert" class="w-4 h-4 text-rose-500 animate-pulse"></i>
                REVERSÃO DE EMERGÊNCIA
            </button>
        </div>
    </header>

    <!-- CRITICAL ALERT BANNER (HIDDEN BY DEFAULT) -->
    <div id="colapse-alert-banner" class="hidden bg-rose-600 text-white font-orbitron font-extrabold px-4 py-2 text-center text-sm md:text-base tracking-widest border-y-2 border-rose-300 animate-pulse-fast shadow-[0_0_30px_rgba(225,29,72,0.8)] z-40 flex justify-center items-center gap-3">
        <i data-lucide="alert-triangle" class="w-6 h-6"></i>
        <span>ALERTA CRÍTICO: DIVERGÊNCIA TENSORIAL ≥ 1.000 — COLAPSO DE MEMBRANA IMINENTE!</span>
        <i data-lucide="alert-triangle" class="w-6 h-6"></i>
    </div>

    <!-- MAIN DASHBOARD CONTENT -->
    <main class="flex-1 p-3 md:p-5 grid grid-cols-1 lg:grid-cols-12 gap-4 max-w-[1920px] mx-auto w-full">

        <!-- COLUMN 1: CONTROLS & GAUGES (4 Cols) -->
        <section class="lg:col-span-4 flex flex-col gap-4">
            
            <!-- CONTROLE ESPACIAL 3D: MANÍPULO DE VETOR DE ENTRADA (MVE) -->
            <div class="cyber-card rounded-xl p-4 flex flex-col relative overflow-hidden">
                <div class="flex justify-between items-center mb-2">
                    <h2 class="font-orbitron font-bold text-sm text-cyan-300 flex items-center gap-2">
                        <i data-lucide="move-3d" class="w-4 h-4 text-cyan-400"></i>
                        Manípulo de Vetor de Entrada (MVE)
                    </h2>
                    <span class="text-[10px] bg-cyan-950/80 border border-cyan-500/30 text-cyan-300 px-2 py-0.5 rounded font-mono">Espaço [X, Y, Z]</span>
                </div>
                
                <p class="text-[11px] text-slate-400 mb-2">Joystick de levitação magnética. Arraste no Canvas para X/Y ou use os seletores.</p>

                <!-- Canvas MVE -->
                <div class="relative w-full aspect-square max-h-[260px] bg-slate-950/90 rounded-lg border border-slate-800 flex justify-center items-center my-1 overflow-hidden">
                    <canvas id="canvas-mve" class="w-full h-full cursor-crosshair"></canvas>
                    <div id="mve-feedback-badge" class="absolute top-2 right-2 text-[10px] font-mono px-2 py-0.5 rounded bg-slate-900/80 border border-slate-700 text-slate-300">
                        Resistência: <span id="mve-torque-val" class="text-cyan-400 font-bold">0.00 N·m</span>
                    </div>
                </div>

                <!-- Manual Controls MVE -->
                <div class="grid grid-cols-3 gap-2 mt-2 text-xs">
                    <div>
                        <div class="flex justify-between text-[11px] text-slate-400"><span>Eixo X</span><span id="val-x" class="text-cyan-400 font-mono">0.00</span></div>
                        <input id="slider-x" type="range" min="-5" max="5" step="0.01" value="0" class="w-full">
                    </div>
                    <div>
                        <div class="flex justify-between text-[11px] text-slate-400"><span>Eixo Y</span><span id="val-y" class="text-cyan-400 font-mono">0.00</span></div>
                        <input id="slider-y" type="range" min="-5" max="5" step="0.01" value="0" class="w-full">
                    </div>
                    <div>
                        <div class="flex justify-between text-[11px] text-slate-400"><span>Eixo Z</span><span id="val-z" class="text-cyan-400 font-mono">0.00</span></div>
                        <input id="slider-z" type="range" min="-5" max="5" step="0.01" value="0" class="w-full">
                    </div>
                </div>
            </div>

            <!-- CONTROLE DE MEMBRANA: ANÉIS DE PROJEÇÃO TENSORIAL (APT) -->
            <div class="cyber-card rounded-xl p-4 flex flex-col">
                <div class="flex justify-between items-center mb-2">
                    <h2 class="font-orbitron font-bold text-sm text-indigo-300 flex items-center gap-2">
                        <i data-lucide="disc" class="w-4 h-4 text-indigo-400"></i>
                        Anéis de Projeção Tensorial (APT)
                    </h2>
                    <span class="text-[10px] bg-indigo-950/80 border border-indigo-500/30 text-indigo-300 px-2 py-0.5 rounded font-mono">Membrana [M1, M2, M3]</span>
                </div>

                <!-- Sliders Anéis -->
                <div class="space-y-3 my-1">
                    <!-- M1 / ρ -->
                    <div class="p-2 rounded bg-slate-900/60 border border-slate-800">
                        <div class="flex justify-between items-center text-xs mb-1">
                            <span class="font-semibold text-cyan-300 flex items-center gap-1">
                                <span class="w-2 h-2 rounded-full bg-cyan-400 inline-block"></span>
                                Anel 1: Densidade de Vácuo (ρ / M1)
                            </span>
                            <span id="val-m1" class="font-mono text-cyan-400 font-bold">1.000</span>
                        </div>
                        <input id="slider-m1" type="range" min="0.1" max="5.0" step="0.001" value="1.0" class="w-full">
                    </div>

                    <!-- M2 / ε -->
                    <div class="p-2 rounded bg-slate-900/60 border border-slate-800">
                        <div class="flex justify-between items-center text-xs mb-1">
                            <span class="font-semibold text-emerald-300 flex items-center gap-1">
                                <span class="w-2 h-2 rounded-full bg-emerald-400 inline-block"></span>
                                Anel 2: Permitividade Relativa (ε / M2)
                            </span>
                            <span id="val-m2" class="font-mono text-emerald-400 font-bold">1.000</span>
                        </div>
                        <input id="slider-m2" type="range" min="0.1" max="5.0" step="0.001" value="1.0" class="w-full">
                    </div>

                    <!-- M3 / χ -->
                    <div class="p-2 rounded bg-slate-900/60 border border-slate-800">
                        <div class="flex justify-between items-center text-xs mb-1">
                            <span class="font-semibold text-purple-300 flex items-center gap-1">
                                <span class="w-2 h-2 rounded-full bg-purple-400 inline-block"></span>
                                Anel 3: Gradiente Causal (χ / M3)
                            </span>
                            <span id="val-m3" class="font-mono text-purple-400 font-bold">1.000</span>
                        </div>
                        <input id="slider-m3" type="range" min="0.1" max="5.0" step="0.001" value="1.0" class="w-full">
                    </div>
                </div>

                <!-- Auto-Align Assist Switch -->
                <div class="mt-2 pt-2 border-t border-slate-800 flex justify-between items-center">
                    <span class="text-xs text-slate-400 flex items-center gap-1">
                        <i data-lucide="cpu" class="w-3.5 h-3.5 text-cyan-400"></i> Servo-Sincronização Quântica:
                    </span>
                    <button id="btn-autosync" class="px-3 py-1 bg-cyan-950 hover:bg-cyan-900 border border-cyan-500/50 text-cyan-300 text-xs font-bold rounded transition">
                        DESATIVADO
                    </button>
                </div>
            </div>

        </section>

        <!-- COLUMN 2: HYPERSPACE CALABI-YAU & APT DISPLAY (5 Cols) -->
        <section class="lg:col-span-5 flex flex-col gap-4">

            <!-- CANAL VISUAL CALABI-YAU E ROTAÇÃO DOS ANÉIS -->
            <div class="cyber-card rounded-xl p-4 flex flex-col flex-1 relative min-h-[360px]">
                <div class="flex justify-between items-center mb-2 z-10">
                    <h2 class="font-orbitron font-bold text-sm text-sky-300 flex items-center gap-2">
                        <i data-lucide="eye" class="w-4 h-4 text-sky-400"></i>
                        Projeção do Hiper-Espaço Calabi-Yau & APT
                    </h2>
                    <span class="text-[10px] bg-slate-900/80 border border-slate-700 text-slate-300 px-2 py-0.5 rounded font-mono">Visualização 6D</span>
                </div>

                <!-- Main Canvas Calabi-Yau -->
                <div class="relative flex-1 w-full h-full min-h-[280px] bg-slate-950/90 rounded-lg border border-slate-800 overflow-hidden flex justify-center items-center">
                    <canvas id="canvas-calabi" class="w-full h-full"></canvas>
                    
                    <!-- Overlay status badges -->
                    <div class="absolute bottom-3 left-3 bg-slate-900/80 border border-slate-700 px-2.5 py-1 rounded text-[11px] font-mono text-slate-300">
                        Fase de Membrana: <span id="calabi-phase-str" class="text-cyan-400 font-bold">ESTÁVEL</span>
                    </div>

                    <div class="absolute bottom-3 right-3 bg-slate-900/80 border border-slate-700 px-2.5 py-1 rounded text-[11px] font-mono text-slate-300">
                        Torque Quântico: <span id="haptic-torque-val" class="text-amber-400 font-bold">0.02 N·m</span>
                    </div>
                </div>

                <!-- ANCORAGEM QUÂNTICA ACTION BUTTON -->
                <div class="mt-3 flex gap-3">
                    <button id="btn-anchor" class="flex-1 py-3 bg-gradient-to-r from-cyan-600 via-sky-600 to-indigo-600 hover:from-cyan-500 hover:to-indigo-500 text-white font-orbitron font-extrabold text-xs md:text-sm rounded-lg shadow-lg shadow-cyan-950/80 border border-cyan-400/40 tracking-wider flex justify-center items-center gap-2 transition active:scale-98">
                        <i data-lucide="anchor" class="w-5 h-5"></i>
                        INICIAR TRANSIÇÃO / ANCORAGEM QUÂNTICA
                    </button>
                </div>
            </div>

            <!-- GRÁFICO DE ESTABILIDADE DA ASSINATURA DE MASSA -->
            <div class="cyber-card rounded-xl p-4 flex flex-col">
                <div class="flex justify-between items-center mb-2">
                    <h2 class="font-orbitron font-bold text-sm text-emerald-300 flex items-center gap-2">
                        <i data-lucide="activity" class="w-4 h-4 text-emerald-400"></i>
                        Estabilidade da Assinatura de Massa [||ΔT_µν||]
                    </h2>
                    <div class="flex items-center gap-2">
                        <span class="text-[10px] text-slate-400">Limite Crítico: <strong class="text-rose-400 font-mono">1.00</strong></span>
                    </div>
                </div>

                <!-- Canvas Chart -->
                <div class="relative w-full h-[140px] bg-slate-950/90 rounded-lg border border-slate-800 overflow-hidden">
                    <canvas id="canvas-chart" class="w-full h-full"></canvas>
                </div>

                <!-- Chart Footnote metrics -->
                <div class="grid grid-cols-3 gap-2 mt-2 text-center text-xs">
                    <div class="p-1.5 bg-slate-900/50 rounded border border-slate-800">
                        <div class="text-[10px] text-slate-400">Divergência Atual</div>
                        <div id="metric-div" class="font-mono font-bold text-cyan-400">0.000000</div>
                    </div>
                    <div class="p-1.5 bg-slate-900/50 rounded border border-slate-800">
                        <div class="text-[10px] text-slate-400">Delta Gravitacional</div>
                        <div id="metric-grav" class="font-mono font-bold text-emerald-400">1.00x</div>
                    </div>
                    <div class="p-1.5 bg-slate-900/50 rounded border border-slate-800">
                        <div class="text-[10px] text-slate-400">Pressão de Gauge</div>
                        <div id="metric-gauge" class="font-mono font-bold text-indigo-400">0.00 kPa</div>
                    </div>
                </div>
            </div>

        </section>

        <!-- COLUMN 3: TELEMETRY & QUANTUM LOGS (3 Cols) -->
        <section class="lg:col-span-3 flex flex-col gap-4">

            <!-- MATRIZ DE TRANSIÇÃO SAMS & VETOR DE ESTADO -->
            <div class="cyber-card rounded-xl p-4 flex flex-col">
                <h2 class="font-orbitron font-bold text-sm text-amber-300 flex items-center gap-2 mb-2">
                    <i data-lucide="binary" class="w-4 h-4 text-amber-400"></i>
                    Matriz de Estado Hexa-Dimensional
                </h2>

                <div class="space-y-2 text-xs">
                    <!-- Vector Ψ_Hex -->
                    <div class="p-2 bg-slate-950/80 rounded border border-slate-800 font-mono">
                        <div class="text-[10px] text-slate-400 mb-1">Vetor de Estado Ψ_Hex:</div>
                        <div id="state-vector-display" class="text-cyan-300 break-all text-[11px]">
                            [0.00, 0.00, 0.00, 1.00, 1.00, 1.00]
                        </div>
                    </div>

                    <!-- Target Constants Display -->
                    <div class="p-2 bg-slate-950/80 rounded border border-slate-800 font-mono">
                        <div class="text-[10px] text-slate-400 mb-1">Constantes Alvo (Universo Destino):</div>
                        <div class="grid grid-cols-3 gap-1 text-[11px] text-center">
                            <div>G: <span id="target-g" class="text-emerald-400">1.0</span></div>
                            <div>c: <span id="target-c" class="text-emerald-400">1.0</span></div>
                            <div>S/dt: <span id="target-s" class="text-emerald-400">1.0</span></div>
                        </div>
                    </div>

                    <!-- Gauge Equilibrium Equations -->
                    <div class="p-2 bg-slate-950/80 rounded border border-slate-800 font-mono">
                        <div class="text-[10px] text-slate-400 mb-1">Balanço do Tensor SAMS T_µν:</div>
                        <div class="text-[10px] text-slate-300 leading-relaxed">
                            R_µν - ½g_µν R + <span class="text-cyan-400">Λ_local</span> = <span class="text-amber-400">(8πG/c⁴) T_µν</span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- QUANTUM PHOTONIC COMPUTER LOGS -->
            <div class="cyber-card rounded-xl p-4 flex flex-col flex-1 min-h-[220px]">
                <div class="flex justify-between items-center mb-2">
                    <h2 class="font-orbitron font-bold text-sm text-cyan-300 flex items-center gap-2">
                        <i data-lucide="terminal" class="w-4 h-4 text-cyan-400"></i>
                        Logs do Computador Quântico
                    </h2>
                    <span class="w-2 h-2 rounded-full bg-emerald-400 animate-ping"></span>
                </div>

                <div id="quantum-logs" class="flex-1 bg-slate-950 p-2.5 rounded-lg border border-slate-800 font-mono text-[10px] text-slate-300 overflow-y-auto max-h-[280px] space-y-1.5 leading-relaxed">
                    <div class="text-slate-500">[SAMS-INIT] Processador Fotônico Topológico online.</div>
                    <div class="text-cyan-400">[GAUGE] Matriz do Universo Alfa carregada com sucesso.</div>
                    <div class="text-emerald-400">[HAPTIC] Sincronização de torque estabilizada.</div>
                </div>
            </div>

        </section>

    </main>

    <!-- FOOTER STATUS -->
    <footer class="border-t border-slate-800 bg-slate-950 px-4 py-2 text-[11px] text-slate-500 flex justify-between items-center font-mono">
        <div>SAMS Dimensional • Tradutor de Leis Físicas Inter-Universais</div>
        <div class="flex items-center gap-4">
            <span>Status: <strong id="footer-status" class="text-emerald-400">OPERACIONAL</strong></span>
            <span>Freq. Fotônica: <strong>842 THz</strong></span>
        </div>
    </footer>

    <!-- LOGIC & ALGORITHMS JS -->
    <script>
        // ====================================================================
        // SAMS SYSTEM STATE & MATHEMATICAL CORE
        // ====================================================================

        // Target Universes Preset Database
        const TARGET_UNIVERSES = {
            alpha: { name: "Universo Alfa (Origem)", X: 0, Y: 0, Z: 0, M1: 1.0, M2: 1.0, M3: 1.0, G: 1.0, c: 1.0, S: 1.0 },
            reptile_grav: { name: "Universo Réptil Gravitacional 2x", X: 0, Y: 0, Z: 0, M1: 2.0, M2: 1.0, M3: 1.0, G: 2.0, c: 1.0, S: 1.0 },
            high_entropy: { name: "Universo Alta Entropia", X: 0, Y: 0, Z: 0, M1: 1.2, M2: 0.8, M3: 3.5, G: 1.1, c: 0.9, S: 3.5 },
            electro_mirror: { name: "Universo Espelho Electro-Fraco", X: 0, Y: 0, Z: 0, M1: 0.9, M2: 0.4, M3: 1.0, G: 0.85, c: 1.5, S: 1.0 },
            dense_vacuum: { name: "Universo Vácuo Super-Denso", X: 0, Y: 0, Z: 0, M1: 3.2, M2: 1.8, M3: 0.7, G: 1.6, c: 0.7, S: 0.7 },
            custom: { name: "Personalizado", X: 0, Y: 0, Z: 0, M1: 1.0, M2: 1.0, M3: 1.0, G: 1.0, c: 1.0, S: 1.0 }
        };

        // Hexa-Dimensional State Vector: Ψ = [X, Y, Z, M1 (ρ), M2 (ε), M3 (χ)]
        const state = {
            // Current user inputs
            X: 0.0,
            Y: 0.0,
            Z: 0.0,
            M1: 1.0, // Vacuum Density ρ
            M2: 1.0, // Relative Permittivity ε
            M3: 1.0, // Causal Gradient χ

            // Current target universe
            currentTargetKey: 'alpha',
            target: { ...TARGET_UNIVERSES.alpha },

            // Computed Tensor Variance Metrics
            deltaTensor: 0.0,
            mveTorque: 0.0,
            hapticTorque: 0.0,

            // System Flags
            isAutoSync: false,
            isAnchoring: false,
            isCollapsed: false,
            
            // Historical divergence for real-time chart (50 points)
            chartHistory: Array(50).fill(0.0)
        };

        // DOM Elements
        const dom = {
            universeSelect: document.getElementById('universe-select'),
            sliderX: document.getElementById('slider-x'),
            sliderY: document.getElementById('slider-y'),
            sliderZ: document.getElementById('slider-z'),
            sliderM1: document.getElementById('slider-m1'),
            sliderM2: document.getElementById('slider-m2'),
            sliderM3: document.getElementById('slider-m3'),

            valX: document.getElementById('val-x'),
            valY: document.getElementById('val-y'),
            valZ: document.getElementById('val-z'),
            valM1: document.getElementById('val-m1'),
            valM2: document.getElementById('val-m2'),
            valM3: document.getElementById('val-m3'),

            mveTorqueVal: document.getElementById('mve-torque-val'),
            hapticTorqueVal: document.getElementById('haptic-torque-val'),
            calabiPhaseStr: document.getElementById('calabi-phase-str'),

            metricDiv: document.getElementById('metric-div'),
            metricGrav: document.getElementById('metric-grav'),
            metricGauge: document.getElementById('metric-gauge'),

            stateVectorDisplay: document.getElementById('state-vector-display'),
            targetG: document.getElementById('target-g'),
            targetC: document.getElementById('target-c'),
            targetS: document.getElementById('target-s'),

            btnAutoSync: document.getElementById('btn-autosync'),
            btnAnchor: document.getElementById('btn-anchor'),
            btnEmergency: document.getElementById('btn-emergency'),

            collapseBanner: document.getElementById('colapse-alert-banner'),
            quantumLogs: document.getElementById('quantum-logs'),
            footerStatus: document.getElementById('footer-status'),

            canvasMve: document.getElementById('canvas-mve'),
            canvasCalabi: document.getElementById('canvas-calabi'),
            canvasChart: document.getElementById('canvas-chart')
        };

        // Canvas Contexts
        const ctxMve = dom.canvasMve.getContext('2d');
        const ctxCalabi = dom.canvasCalabi.getContext('2d');
        const ctxChart = dom.canvasChart.getContext('2d');

        // ====================================================================
        // ALGORITHM: SAMS JACOBIAN & DIMENSIONAL TRANSLATION (PYTHON PORTED TO JS)
        // ====================================================================

        /**
         * Calculates the Jacobian matrix difference between the current Hex-State 
         * and the target universe parameters. Returns the scalar tensor divergence ||ΔT_µν||.
         */
        function computeDimensionalJacobian() {
            // Spatial vector deviation
            const dx = state.X - state.target.X;
            const dy = state.Y - state.target.Y;
            const dz = state.Z - state.target.Z;
            const spatialDelta = Math.sqrt(dx*dx + dy*dy + dz*dz) * 0.05;

            // Membrane dimensions deviation (rho, epsilon, chi)
            const dM1 = Math.abs(state.M1 - state.target.M1) / state.target.M1;
            const dM2 = Math.abs(state.M2 - state.target.M2) / state.target.M2;
            const dM3 = Math.abs(state.M3 - state.target.M3) / state.target.M3;

            // Weighted Jacobian norm
            const membraneDelta = Math.sqrt(dM1*dM1 * 0.4 + dM2*dM2 * 0.3 + dM3*dM3 * 0.3);

            // Total Divergence Delta Tensor
            state.deltaTensor = spatialDelta + membraneDelta;

            // Haptic Torque calculations
            state.mveTorque = (spatialDelta * 12.5) + (membraneDelta * 5.0);
            state.hapticTorque = membraneDelta * 18.2;

            // Check Critical Threshold
            if (state.deltaTensor >= 1.0 && !state.isCollapsed) {
                triggerMembraneCollapse();
            } else if (state.deltaTensor < 1.0 && state.isCollapsed) {
                recoverMembraneCollapse();
            }

            // Auto-sync algorithm if enabled
            if (state.isAutoSync && !state.isCollapsed) {
                runAutoSyncStep();
            }
        }

        /**
         * Servo-synchronization step: gradually aligns APT rings and MVE joystick to target
         */
        function runAutoSyncStep() {
            const lerp = (start, end, amt) => (1 - amt) * start + amt * end;
            
            state.M1 = lerp(state.M1, state.target.M1, 0.08);
            state.M2 = lerp(state.M2, state.target.M2, 0.08);
            state.M3 = lerp(state.M3, state.target.M3, 0.08);

            state.X = lerp(state.X, state.target.X, 0.08);
            state.Y = lerp(state.Y, state.target.Y, 0.08);
            state.Z = lerp(state.Z, state.target.Z, 0.08);

            updateSliderUIValues();
        }

        /**
         * Triggered when tensor divergence reaches >= 1.0
         */
        function triggerMembraneCollapse() {
            state.isCollapsed = true;
            dom.collapseBanner.classList.remove('hidden');
            dom.footerStatus.textContent = "COLAPSO CRÍTICO";
            dom.footerStatus.className = "text-rose-500 animate-pulse font-bold";
            logQuantumMessage("ALERTA EVASÃO CAUSAL: Divergência ||ΔT_µν|| >= 1.0! Colapso de membrana iminente!", "rose");
        }

        function recoverMembraneCollapse() {
            state.isCollapsed = false;
            dom.collapseBanner.classList.add('hidden');
            dom.footerStatus.textContent = "OPERACIONAL";
            dom.footerStatus.className = "text-emerald-400 font-bold";
            logQuantumMessage("Divergência estabilizada abaixo do limite crítico. Integridade restaurada.", "emerald");
        }

        // ====================================================================
        // UI & EVENT HANDLERS
        // ====================================================================

        function logQuantumMessage(msg, color = "cyan") {
            const time = new Date().toISOString().substring(11, 19);
            const entry = document.createElement('div');
            
            let colorClass = "text-cyan-300";
            if (color === "rose") colorClass = "text-rose-400 font-bold";
            if (color === "emerald") colorClass = "text-emerald-400";
            if (color === "amber") colorClass = "text-amber-300";

            entry.className = colorClass;
            entry.innerHTML = `<span class="text-slate-600">[${time}]</span> ${msg}`;
            dom.quantumLogs.appendChild(entry);
            dom.quantumLogs.scrollTop = dom.quantumLogs.scrollHeight;
        }

        function updateSliderUIValues() {
            dom.sliderX.value = state.X;
            dom.sliderY.value = state.Y;
            dom.sliderZ.value = state.Z;
            dom.sliderM1.value = state.M1;
            dom.sliderM2.value = state.M2;
            dom.sliderM3.value = state.M3;

            dom.valX.textContent = state.X.toFixed(2);
            dom.valY.textContent = state.Y.toFixed(2);
            dom.valZ.textContent = state.Z.toFixed(2);
            dom.valM1.textContent = state.M1.toFixed(3);
            dom.valM2.textContent = state.M2.toFixed(3);
            dom.valM3.textContent = state.M3.toFixed(3);
        }

        // Attach event listeners to sliders
        function setupEventListeners() {
            const onInputUpdate = () => {
                state.X = parseFloat(dom.sliderX.value);
                state.Y = parseFloat(dom.sliderY.value);
                state.Z = parseFloat(dom.sliderZ.value);
                state.M1 = parseFloat(dom.sliderM1.value);
                state.M2 = parseFloat(dom.sliderM2.value);
                state.M3 = parseFloat(dom.sliderM3.value);

                if (state.currentTargetKey !== 'custom') {
                    // Switch to custom if modified manually
                    dom.universeSelect.value = 'custom';
                    state.currentTargetKey = 'custom';
                    state.target = { ...TARGET_UNIVERSES.custom, M1: state.M1, M2: state.M2, M3: state.M3 };
                }

                updateSliderUIValues();
            };

            [dom.sliderX, dom.sliderY, dom.sliderZ, dom.sliderM1, dom.sliderM2, dom.sliderM3].forEach(el => {
                el.addEventListener('input', onInputUpdate);
            });

            // Target Universe Selection
            dom.universeSelect.addEventListener('change', (e) => {
                const key = e.target.value;
                state.currentTargetKey = key;
                state.target = { ...TARGET_UNIVERSES[key] };

                dom.targetG.textContent = state.target.G.toFixed(1) + 'x';
                dom.targetC.textContent = state.target.c.toFixed(1) + 'x';
                dom.targetS.textContent = state.target.S.toFixed(1) + 'x';

                logQuantumMessage(`Universo Alvo alterado para: ${state.target.name}`, "amber");
            });

            // AutoSync Button
            dom.btnAutoSync.addEventListener('click', () => {
                state.isAutoSync = !state.isAutoSync;
                if (state.isAutoSync) {
                    dom.btnAutoSync.textContent = "ATIVADO";
                    dom.btnAutoSync.className = "px-3 py-1 bg-cyan-500 text-slate-950 font-bold text-xs rounded border border-cyan-300 animate-pulse";
                    logQuantumMessage("Servo-sincronização quântica ativada.", "cyan");
                } else {
                    dom.btnAutoSync.textContent = "DESATIVADO";
                    dom.btnAutoSync.className = "px-3 py-1 bg-cyan-950 hover:bg-cyan-900 border border-cyan-500/50 text-cyan-300 text-xs font-bold rounded transition";
                    logQuantumMessage("Servo-sincronização desativada. Controle totalmente manual.", "slate");
                }
            });

            // Emergency Reversion Button
            dom.btnEmergency.addEventListener('click', () => {
                logQuantumMessage("INICIANDO REVERSÃO DE EMERGÊNCIA! Ancorando no Universo Alfa...", "rose");
                state.currentTargetKey = 'alpha';
                dom.universeSelect.value = 'alpha';
                state.target = { ...TARGET_UNIVERSES.alpha };
                state.X = 0; state.Y = 0; state.Z = 0;
                state.M1 = 1.0; state.M2 = 1.0; state.M3 = 1.0;
                updateSliderUIValues();
                state.isAutoSync = false;
                dom.btnAutoSync.textContent = "DESATIVADO";
                dom.btnAutoSync.className = "px-3 py-1 bg-cyan-950 hover:bg-cyan-900 border border-cyan-500/50 text-cyan-300 text-xs font-bold rounded transition";
            });

            // Anchor Transition Button
            dom.btnAnchor.addEventListener('click', () => {
                if (state.deltaTensor > 1e-4) {
                    logQuantumMessage(`FALHA NA ANCORAGEM: Divergência (${state.deltaTensor.toFixed(6)}) acima de 1e-6. Calibre a interface!`, "rose");
                } else {
                    state.isAnchoring = true;
                    logQuantumMessage("ÂNCORA QUÂNTICA FIXADA! Transição de Membrana concluída com sucesso!", "emerald");
                    setTimeout(() => { state.isAnchoring = false; }, 3000);
                }
            });

            // Mouse Interactive Dragging for MVE Canvas Joystick
            let isDraggingMve = false;
            const handleMveMove = (e) => {
                const rect = dom.canvasMve.getBoundingClientRect();
                const x = e.clientX - rect.left;
                const y = e.clientY - rect.top;
                const normX = ((x / rect.width) * 2 - 1) * 5;
                const normY = -((y / rect.height) * 2 - 1) * 5;

                state.X = Math.max(-5, Math.min(5, normX));
                state.Y = Math.max(-5, Math.min(5, normY));
                updateSliderUIValues();
            };

            dom.canvasMve.addEventListener('mousedown', (e) => { isDraggingMve = true; handleMveMove(e); });
            window.addEventListener('mousemove', (e) => { if (isDraggingMve) handleMveMove(e); });
            window.addEventListener('mouseup', () => { isDraggingMve = false; });
        }

        // ====================================================================
        // CANVAS DRAWING & GRAPHICS RENDER LOOPS
        // ====================================================================

        let animTime = 0;

        /**
         * 1. Render MVE Spherical Joystick Control Canvas
         */
        function renderMveCanvas() {
            const width = dom.canvasMve.width = dom.canvasMve.clientWidth;
            const height = dom.canvasMve.height = dom.canvasMve.clientHeight;
            const ctx = ctxMve;

            ctx.clearRect(0, 0, width, height);

            const cx = width / 2;
            const cy = height / 2;
            const radius = Math.min(width, height) * 0.38;

            // Target spot
            const targetPx = cx + (state.target.X / 5) * radius;
            const targetPy = cy - (state.target.Y / 5) * radius;

            ctx.beginPath();
            ctx.arc(targetPx, targetPy, 8, 0, Math.PI * 2);
            ctx.strokeStyle = '#34d399';
            ctx.lineWidth = 2;
            ctx.setLineDash([3, 3]);
            ctx.stroke();
            ctx.setLineDash([]);

            // Grid concentric circles
            ctx.strokeStyle = 'rgba(56, 189, 248, 0.15)';
            ctx.lineWidth = 1;
            for (let r = 0.2; r <= 1.0; r += 0.2) {
                ctx.beginPath();
                ctx.arc(cx, cy, radius * r, 0, Math.PI * 2);
                ctx.stroke();
            }

            // Crosshair axes
            ctx.beginPath();
            ctx.moveTo(cx - radius, cy); ctx.lineTo(cx + radius, cy);
            ctx.moveTo(cx, cy - radius); ctx.lineTo(cx, cy + radius);
            ctx.strokeStyle = 'rgba(56, 189, 248, 0.25)';
            ctx.stroke();

            // Current Position Joystick Ball
            const ballPx = cx + (state.X / 5) * radius;
            const ballPy = cy - (state.Y / 5) * radius;

            // Vector line
            ctx.beginPath();
            ctx.moveTo(cx, cy);
            ctx.lineTo(ballPx, ballPy);
            ctx.strokeStyle = state.deltaTensor >= 1.0 ? '#f43f5e' : '#06b6d4';
            ctx.lineWidth = 2;
            ctx.stroke();

            // Spherical Glow Effect
            const grad = ctx.createRadialGradient(ballPx - 3, ballPy - 3, 2, ballPx, ballPy, 14);
            if (state.deltaTensor >= 1.0) {
                grad.addColorStop(0, '#fda4af');
                grad.addColorStop(1, '#e11d48');
            } else {
                grad.addColorStop(0, '#7dd3fc');
                grad.addColorStop(1, '#0284c7');
            }

            ctx.beginPath();
            ctx.arc(ballPx, ballPy, 12, 0, Math.PI * 2);
            ctx.fillStyle = grad;
            ctx.shadowColor = state.deltaTensor >= 1.0 ? '#f43f5e' : '#38bdf8';
            ctx.shadowBlur = 12;
            ctx.fill();
            ctx.shadowBlur = 0;

            // Z Axis indicator ring around ball
            const zRadius = 14 + (state.Z * 2);
            ctx.beginPath();
            ctx.arc(ballPx, ballPy, Math.max(4, zRadius), 0, Math.PI * 2);
            ctx.strokeStyle = 'rgba(255, 255, 255, 0.5)';
            ctx.lineWidth = 1.5;
            ctx.stroke();
        }

        /**
         * 2. Render Calabi-Yau Hyperspace Projection & APT Concentric Rings
         */
        function renderCalabiCanvas() {
            const width = dom.canvasCalabi.width = dom.canvasCalabi.clientWidth;
            const height = dom.canvasCalabi.height = dom.canvasCalabi.clientHeight;
            const ctx = ctxCalabi;

            ctx.clearRect(0, 0, width, height);
            const cx = width / 2;
            const cy = height / 2;

            // Calabi-Yau Pseudo-3D Geometric Mesh
            ctx.save();
            ctx.translate(cx, cy);

            const numPoints = 60;
            const scale = Math.min(width, height) * 0.22;

            ctx.beginPath();
            for (let i = 0; i < numPoints; i++) {
                const u = (i / numPoints) * Math.PI * 2;
                for (let j = 0; j < numPoints; j += 4) {
                    const v = (j / numPoints) * Math.PI;

                    // Complex 6D Calabi-Yau trigonometric equation projection
                    const x = scale * (Math.cos(u) * Math.sin(v) + 0.3 * Math.cos(state.M1 * u + animTime) * Math.sin(state.M2 * v));
                    const y = scale * (Math.sin(u) * Math.sin(v) + 0.3 * Math.sin(state.M2 * u + animTime) * Math.cos(state.M3 * v));

                    if (j === 0) ctx.moveTo(x, y);
                    else ctx.lineTo(x, y);
                }
            }
            ctx.strokeStyle = state.deltaTensor >= 1.0 ? 'rgba(244, 63, 94, 0.35)' : 'rgba(56, 189, 248, 0.25)';
            ctx.lineWidth = 1;
            ctx.stroke();

            // Concentric Rings of Tensor Projection (APT)
            const ringRadii = [scale * 1.3, scale * 1.65, scale * 2.0];
            const ringSpeeds = [animTime * state.M1 * 0.5, -animTime * state.M2 * 0.4, animTime * state.M3 * 0.3];
            const ringColors = ['#06b6d4', '#10b981', '#a855f7'];

            ringRadii.forEach((r, idx) => {
                ctx.save();
                ctx.rotate(ringSpeeds[idx]);

                ctx.beginPath();
                ctx.ellipse(0, 0, r, r * 0.4, ringSpeeds[idx], 0, Math.PI * 2);
                ctx.strokeStyle = ringColors[idx];
                ctx.lineWidth = 2;
                ctx.globalAlpha = 0.7;
                ctx.stroke();

                // Calibration nodes on ring
                for (let k = 0; k < 4; k++) {
                    const angle = (k * Math.PI / 2);
                    const nx = r * Math.cos(angle);
                    const ny = r * 0.4 * Math.sin(angle);
                    ctx.beginPath();
                    ctx.arc(nx, ny, 4, 0, Math.PI * 2);
                    ctx.fillStyle = ringColors[idx];
                    ctx.fill();
                }
                ctx.restore();
            });

            ctx.restore();
        }

        /**
         * 3. Render Real-Time Mass Signature Stability Chart
         */
        function renderChartCanvas() {
            const width = dom.canvasChart.width = dom.canvasChart.clientWidth;
            const height = dom.canvasChart.height = dom.canvasChart.clientHeight;
            const ctx = ctxChart;

            ctx.clearRect(0, 0, width, height);

            // Shift chart history
            state.chartHistory.push(state.deltaTensor);
            if (state.chartHistory.length > 60) state.chartHistory.shift();

            // Critical Limit Line (1.0)
            const critY = height * 0.2; // 1.0 threshold height
            ctx.beginPath();
            ctx.moveTo(0, critY);
            ctx.lineTo(width, critY);
            ctx.strokeStyle = 'rgba(244, 63, 94, 0.6)';
            ctx.setLineDash([4, 4]);
            ctx.lineWidth = 1.5;
            ctx.stroke();
            ctx.setLineDash([]);

            // Label Critical Line
            ctx.fillStyle = '#f43f5e';
            ctx.font = '9px JetBrains Mono';
            ctx.fillText("CRÍTICO 1.0", 5, critY - 4);

            // Chart Line Drawing
            ctx.beginPath();
            const step = width / (state.chartHistory.length - 1);

            state.chartHistory.forEach((val, idx) => {
                // Map val (0.0 to 1.5) to Canvas Y
                const normVal = Math.min(1.5, val);
                const y = height - (normVal / 1.2) * (height - 20) - 10;
                const x = idx * step;

                if (idx === 0) ctx.moveTo(x, y);
                else ctx.lineTo(x, y);
            });

            const currentDivergence = state.chartHistory[state.chartHistory.length - 1];
            ctx.strokeStyle = currentDivergence >= 1.0 ? '#f43f5e' : (currentDivergence < 1e-4 ? '#34d399' : '#38bdf8');
            ctx.lineWidth = 2;
            ctx.stroke();

            // Fill area under line
            ctx.lineTo(width, height);
            ctx.lineTo(0, height);
            ctx.closePath();
            const fillGrad = ctx.createLinearGradient(0, 0, 0, height);
            fillGrad.addColorStop(0, currentDivergence >= 1.0 ? 'rgba(244, 63, 94, 0.3)' : 'rgba(56, 189, 248, 0.2)');
            fillGrad.addColorStop(1, 'rgba(15, 23, 42, 0)');
            ctx.fillStyle = fillGrad;
            ctx.fill();
        }

        /**
         * Update Telemetry Data Displays
         */
        function updateTelemetryUI() {
            dom.metricDiv.textContent = state.deltaTensor.toFixed(6);
            dom.metricDiv.className = state.deltaTensor >= 1.0 ? "font-mono font-bold text-rose-500 animate-pulse" : (state.deltaTensor < 1e-4 ? "font-mono font-bold text-emerald-400" : "font-mono font-bold text-cyan-400");

            dom.metricGrav.textContent = (state.M1 * state.target.G).toFixed(2) + 'x';
            dom.metricGauge.textContent = (state.deltaTensor * 101.3).toFixed(2) + ' kPa';

            dom.mveTorqueVal.textContent = state.mveTorque.toFixed(2) + ' N·m';
            dom.hapticTorqueVal.textContent = state.hapticTorque.toFixed(2) + ' N·m';

            dom.stateVectorDisplay.textContent = `[${state.X.toFixed(2)}, ${state.Y.toFixed(2)}, ${state.Z.toFixed(2)}, ${state.M1.toFixed(3)}, ${state.M2.toFixed(3)}, ${state.M3.toFixed(3)}]`;

            if (state.deltaTensor < 1e-4) {
                dom.calabiPhaseStr.textContent = "ALINHADO (1e-6)";
                dom.calabiPhaseStr.className = "text-emerald-400 font-bold";
            } else if (state.deltaTensor >= 1.0) {
                dom.calabiPhaseStr.textContent = "COLAPSO";
                dom.calabiPhaseStr.className = "text-rose-500 font-bold animate-pulse";
            } else {
                dom.calabiPhaseStr.textContent = "EM CALIBRAÇÃO";
                dom.calabiPhaseStr.className = "text-cyan-400 font-bold";
            }
        }

        // Main Animation & Loop Engine
        function mainLoop() {
            animTime += 0.02;

            computeDimensionalJacobian();
            renderMveCanvas();
            renderCalabiCanvas();
            renderChartCanvas();
            updateTelemetryUI();

            requestAnimationFrame(mainLoop);
        }

        // Initialization
        window.addEventListener('DOMContentLoaded', () => {
            lucide.createIcons();
            setupEventListeners();
            updateSliderUIValues();
            logQuantumMessage("SAMS Hex-IC Inicializado. Monitorando tensores inter-universais.", "cyan");
            requestAnimationFrame(mainLoop);
        });

    </script>
</body>
</html>
