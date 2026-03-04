<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CT241 - LOGISTIQUE SÉCURISÉE V3</title>
    <script src="https://cdn.jsdelivr.net/npm/lucide@0.344.0/dist/umd/lucide.min.js"></script>
    <style>
        :root {
            --gabon-vert: #009E60;
            --gabon-jaune: #FCD116;
            --gabon-bleu: #3A75C4;
            --dark: #1e293b;
            --light: #f8fafc;
            --border: #e2e8f0;
            --danger: #ef4444;
            --info: #0ea5e9;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background-color: var(--light);
            color: var(--dark);
            margin: 0;
            min-height: 100vh;
        }

        /* --- LOGIN SCREEN --- */
        #login-screen {
            position: fixed;
            inset: 0;
            background: linear-gradient(135deg, var(--dark) 0%, #0f172a 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
        }

        .login-card {
            background: white;
            padding: 40px;
            border-radius: 24px;
            width: 380px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
            border-top: 8px solid var(--gabon-jaune);
        }

        .login-header { text-align: center; margin-bottom: 30px; }
        .login-header h2 { margin: 0; color: var(--dark); }
        .login-header p { color: #64748b; font-size: 0.9rem; margin-top: 5px; }

        .form-group { margin-bottom: 15px; text-align: left; }
        .form-group label { display: block; font-size: 0.8rem; font-weight: 600; color: #475569; margin-bottom: 5px; }

        /* --- APP CONTENT --- */
        #app-content { display: none; flex-direction: column; min-height: 100vh; }

        header {
            background: var(--dark);
            color: white;
            padding: 0.8rem 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 4px solid var(--gabon-jaune);
        }

        .user-info { display: flex; align-items: center; gap: 15px; font-size: 0.85rem; }
        .btn-logout { background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.2); color: white; padding: 6px 12px; border-radius: 6px; cursor: pointer; transition: 0.2s; }
        .btn-logout:hover { background: var(--danger); border-color: var(--danger); }

        .main-container {
            display: grid;
            grid-template-columns: 320px 1fr;
            flex: 1;
            height: calc(100vh - 65px);
        }

        /* --- SIDEBAR --- */
        aside { background: white; border-right: 1px solid var(--border); padding: 20px; display: flex; flex-direction: column; gap: 15px; }
        
        .tabs-nav {
            display: flex;
            background: #f1f5f9;
            padding: 4px;
            border-radius: 8px;
            margin-bottom: 10px;
        }
        .tab-btn {
            flex: 1;
            padding: 8px;
            border: none;
            background: none;
            font-size: 0.75rem;
            font-weight: 600;
            cursor: pointer;
            border-radius: 6px;
            color: #64748b;
        }
        .tab-btn.active {
            background: white;
            color: var(--gabon-bleu);
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        .project-list { list-style: none; padding: 0; margin: 0; overflow-y: auto; flex: 1; }
        .project-item {
            padding: 12px; border-radius: 8px; cursor: pointer; margin-bottom: 8px; border: 1px solid var(--border);
            display: flex; flex-direction: column; gap: 4px; transition: 0.2s;
            background: #fff;
        }
        .project-item:hover { background: #f8fafc; border-color: #cbd5e1; }
        .project-item.active { background: #eff6ff; border-color: var(--gabon-bleu); }
        
        .btn-add { width: 100%; padding: 12px; background: var(--gabon-vert); color: white; border: none; border-radius: 8px; cursor: pointer; font-weight: bold; margin-bottom: 10px; display: flex; align-items: center; justify-content: center; gap: 8px; }

        /* --- CONTENT --- */
        main { padding: 30px; overflow-y: auto; }
        .card { background: white; padding: 25px; border-radius: 15px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05); margin-bottom: 25px; border: 1px solid var(--border); }
        .badge { padding: 4px 12px; border-radius: 20px; font-size: 0.75rem; font-weight: bold; text-transform: uppercase; }
        .badge-active { background: #dcfce7; color: #166534; }
        .badge-completed { background: #eff6ff; color: #1e40af; }

        .client-card {
            background: #f8fafc;
            border-left: 4px solid var(--gabon-jaune);
            padding: 12px 15px;
            border-radius: 0 8px 8px 0;
            margin-top: 10px;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        /* --- STEPPER --- */
        .stepper { display: flex; justify-content: space-between; margin: 30px 0; position: relative; }
        .stepper::before { content: ''; position: absolute; top: 15px; left: 0; right: 0; height: 2px; background: #e2e8f0; z-index: 1; }
        .step { position: relative; z-index: 2; background: white; text-align: center; width: 85px; cursor: pointer; transition: transform 0.2s; }
        .step-circle { width: 32px; height: 32px; border-radius: 50%; background: #e2e8f0; margin: 0 auto 8px; display: flex; align-items: center; justify-content: center; font-weight: bold; border: 2px solid white; }
        .step.completed .step-circle { background: var(--gabon-vert); color: white; }
        .step.current .step-circle { background: var(--gabon-bleu); color: white; border-color: var(--gabon-bleu); }
        .step-label { font-size: 0.7rem; font-weight: 600; }

        /* --- PHOTOS SECTION --- */
        .photo-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); gap: 10px; margin-top: 15px; }
        .photo-item { width: 100px; height: 100px; border-radius: 8px; object-fit: cover; border: 1px solid var(--border); cursor: pointer; transition: 0.2s; }
        .photo-item:hover { transform: scale(1.05); box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        .photo-upload-placeholder { 
            width: 100px; height: 100px; border: 2px dashed var(--border); border-radius: 8px; 
            display: flex; flex-direction: column; align-items: center; justify-content: center; 
            color: #94a3b8; font-size: 0.6rem; cursor: pointer; 
        }

        /* --- MARITIME INFO BLOCK --- */
        .maritime-info-card {
            background: #f0f7ff;
            border: 1px solid #bae6fd;
            border-radius: 12px;
            padding: 15px;
            margin-top: 15px;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }
        .m-field label { font-size: 0.65rem; color: #64748b; font-weight: bold; text-transform: uppercase; display: block; }
        .m-field span { font-size: 0.85rem; font-weight: 700; color: #0369a1; }

        /* --- UI ELEMENTS --- */
        input, select, textarea { width: 100%; padding: 12px; border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box; margin-top: 5px; font-size: 0.9rem; transition: 0.2s; }
        input:focus, select:focus { border-color: var(--gabon-bleu); outline: none; box-shadow: 0 0 0 3px rgba(58, 117, 196, 0.1); }
        .btn-primary { background: var(--gabon-vert); color: white; border: none; padding: 12px 20px; border-radius: 10px; cursor: pointer; font-weight: bold; width: 100%; }
        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.5); align-items: center; justify-content: center; z-index: 1500; backdrop-filter: blur(4px); }
        .modal-content { background: white; padding: 30px; border-radius: 20px; width: 500px; max-height: 95vh; overflow-y: auto; }
        .btn-delete { background: #fee2e2; color: var(--danger); border: none; padding: 10px; border-radius: 8px; cursor: pointer; transition: 0.2s; }
        .btn-delete:hover { background: var(--danger); color: white; }

        .calc-row { display: flex; justify-content: space-between; font-size: 0.85rem; margin-bottom: 5px; border-bottom: 1px dashed #eee; padding-bottom: 4px; }
        .calc-total { font-weight: bold; color: var(--gabon-vert); font-size: 1.1rem; border-top: 2px solid #eee; margin-top: 10px; padding-top: 10px; display: flex; justify-content: space-between; }
        .type-label { font-size: 0.7rem; color: var(--info); font-weight: bold; background: #e0f2fe; padding: 2px 6px; border-radius: 4px; }
    </style>
</head>
<body>

<input type="file" id="hidden-camera-input" accept="image/*" capture="environment" style="display: none;">

<!-- LOGIN SCREEN -->
<div id="login-screen">
    <div class="login-card">
        <div class="login-header">
            <div style="background: var(--gabon-vert); width: 60px; height: 60px; border-radius: 15px; margin: 0 auto 15px; display: flex; align-items: center; justify-content: center; color: white;">
                <i data-lucide="lock" size="30"></i>
            </div>
            <h2>Portail CT241</h2>
            <p>Système Logistique Sécurisé v3</p>
        </div>
        <div class="form-group">
            <label>Adresse E-mail</label>
            <input type="email" id="login-email" placeholder="nom@ct241.ga" autocomplete="email">
        </div>
        <div class="form-group">
            <label>Mot de passe</label>
            <input type="password" id="login-password" placeholder="••••••••">
        </div>
        <button class="btn-primary" id="btn-submit-login" style="margin-top: 10px;">SE CONNECTER</button>
        <p id="login-error" style="color: var(--danger); font-size: 0.8rem; margin-top: 15px; display: none; text-align: center; background: #fee2e2; padding: 8px; border-radius: 6px;">Identifiants incorrects.</p>
    </div>
</div>

<div id="app-content">
    <header>
        <h1 style="margin:0; font-size: 1.1rem; display: flex; align-items: center; gap: 10px;">
            <i data-lucide="shield-check" style="color: var(--gabon-jaune);"></i> CT241 LOGISTIQUE
        </h1>
        <div class="user-info">
            <div style="text-align: right">
                <div id="current-user-name" style="font-weight: bold; font-size: 0.8rem;">---</div>
                <div id="current-user-role" style="font-size: 0.65rem; color: #94a3b8;">---</div>
            </div>
            <button class="btn-logout" onclick="logout()">Déconnexion</button>
        </div>
    </header>

    <div class="main-container">
        <aside>
            <button class="btn-add" onclick="showModal()">
                <i data-lucide="plus-circle" size="18"></i> Nouveau Dossier
            </button>
            <div class="tabs-nav">
                <button id="tab-active" class="tab-btn active" onclick="switchList('active')">EN COURS (<span id="count-active">0</span>)</button>
                <button id="tab-done" class="tab-btn" onclick="switchList('done')">TERMINÉES (<span id="count-done">0</span>)</button>
            </div>
            <ul id="project-list" class="project-list"></ul>
        </aside>

        <main id="main-view">
            <div id="empty-state" style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; color: #94a3b8;">
                <i data-lucide="layout" size="48" style="margin-bottom: 15px; opacity: 0.3;"></i>
                <p>Sélectionnez un dossier pour voir le détail logistique</p>
            </div>

            <div id="project-details" style="display: none;">
                <div class="card">
                    <div style="display: flex; justify-content: space-between;">
                        <div>
                            <span id="view-badge" class="badge"></span>
                            <span id="view-transport-type" class="type-label" style="margin-left: 10px;">---</span>
                            <h2 id="view-title" style="margin: 10px 0 5px 0;"></h2>
                            <p id="view-desc" style="font-size: 0.8rem; color: #64748b; margin: 0;"></p>
                        </div>
                        <button class="btn-delete" onclick="deleteProject()"><i data-lucide="trash-2" size="18"></i></button>
                    </div>

                    <div class="client-card">
                        <i data-lucide="user" style="color: var(--gabon-bleu);"></i>
                        <div>
                            <div style="font-size: 0.7rem; text-transform: uppercase; color: #94a3b8; font-weight: bold;">Propriétaire de la cargaison</div>
                            <div id="view-client-info" style="font-weight: 600; color: var(--dark);">---</div>
                        </div>
                    </div>

                    <div class="stepper">
                        <div class="step" onclick="selectStep(0)"><div class="step-circle">1</div><div class="step-label">Achat</div></div>
                        <div class="step" onclick="selectStep(1)"><div class="step-circle">2</div><div class="step-label">Transit</div></div>
                        <div class="step" onclick="selectStep(2)"><div class="step-circle">3</div><div class="step-label">Douane</div></div>
                        <div class="step" onclick="selectStep(3)"><div class="step-circle">4</div><div class="step-label">Livraison</div></div>
                    </div>
                </div>

                <!-- INFO TRANSPORT SPECIFIQUE -->
                <div id="transport-details-block" class="card" style="display:none; border-top: 4px solid var(--gabon-bleu);">
                    <h3 style="margin-top:0; font-size: 0.9rem; color: var(--gabon-bleu); display: flex; align-items: center; gap: 8px;">
                        <i id="transport-icon" data-lucide="ship" size="18"></i> Suivi Transport International
                    </h3>
                    <div class="maritime-info-card">
                        <div class="m-field"><label id="lbl-company">Cie / Armateur</label><span id="v-armateur">---</span></div>
                        <div class="m-field"><label id="lbl-tracking">N° BL / LTA</label><span id="v-bl">---</span></div>
                        <div class="m-field"><label id="lbl-unit">Conteneur / Vol</label><span id="v-container">---</span></div>
                        <div class="m-field"><label id="lbl-vessel">Navire / Avion</label><span id="v-vessel">---</span></div>
                        <div class="m-field" style="grid-column: span 2;"><label>Date estimée d'arrivée (ETA Libreville)</label><span id="v-date-ship">---</span></div>
                    </div>
                </div>

                <div class="card">
                    <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom: 15px;">
                        <h3 style="margin:0">Preuves Visuelles : <span id="current-step-name" style="color:var(--gabon-bleu)">---</span></h3>
                        <button id="btn-validate-step" class="btn-primary" style="display:none; width: auto;" onclick="validateCurrentStep()">Valider cette étape</button>
                    </div>
                    <div class="photo-grid" id="photo-container"></div>
                </div>

                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
                    <div class="card">
                        <h3 style="margin-top:0">Bilan Financier Est.</h3>
                        <div id="financial-breakdown" style="font-size: 0.85rem; color: #475569;"></div>
                        <div class="calc-total">
                            <span>TOTAL NET À PAYER</span>
                            <span id="val-total">0 F CFA</span>
                        </div>
                    </div>
                    <div class="card">
                        <h3 style="margin-top:0">Notes Logistiques</h3>
                        <textarea id="view-notes" style="height:100px; resize:none;" onblur="saveNotes()" placeholder="Observations sur l'état de la marchandise ou délais..."></textarea>
                    </div>
                </div>
            </div>
        </main>
    </div>
</div>

<!-- MODAL NOUVELLE MISSION -->
<div id="modal-project" class="modal">
    <div class="modal-content">
        <h3 style="margin-top:0; border-bottom: 2px solid var(--gabon-jaune); padding-bottom: 15px;">Nouveau Dossier Logistique</h3>
        
        <div style="margin-top: 15px;">
            <label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">DÉSIGNATION MARCHANDISE</label>
            <input type="text" id="new-name" placeholder="Ex: Scooter, Pièces Auto, Cosmétiques...">
        </div>

        <div style="margin-top: 15px; display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
            <div><label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">CLIENT</label><input type="text" id="new-client-name"></div>
            <div><label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">TÉLÉPHONE</label><input type="text" id="new-client-phone"></div>
        </div>

        <div style="margin-top: 15px;">
            <label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">MODE DE TRANSPORT & TARIF</label>
            <select id="new-transport-mode" onchange="toggleTransportFields()">
                <optgroup label="MARITIME (Standard)">
                    <option value="sea_lcl_divers">Maritime LCL - Divers (9 500 F/kg)</option>
                    <option value="sea_lcl_elec">Maritime LCL - Électronique (14 000 F/kg)</option>
                    <option value="sea_moto">Maritime - Forfait Moto/Scooter (350 000 F)</option>
                </optgroup>
                <optgroup label="MARITIME (Conteneur Plein FCL)">
                    <option value="sea_fcl_20">Conteneur 20' (Forfait 4 200 000 F)</option>
                    <option value="sea_fcl_40">Conteneur 40' HC (Forfait 6 800 000 F)</option>
                </optgroup>
                <optgroup label="AÉRIEN (Express)">
                    <option value="air_standard">Aérien Standard (12 500 F/kg)</option>
                    <option value="air_premium">Aérien Premium/Fragile (16 000 F/kg)</option>
                </optgroup>
            </select>
        </div>

        <div id="transport-tracking-fields" style="background: #f1f5f9; padding: 15px; border-radius: 12px; margin-top: 15px; border: 1px solid #e2e8f0;">
            <label style="font-size: 0.7rem; font-weight: 800; color: var(--gabon-bleu); display: block; margin-bottom: 10px; text-transform: uppercase;">Tracking Initial</label>
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                <input type="text" id="new-armateur" placeholder="Cie (CMA, DHL, ...)" style="padding:8px;">
                <input type="text" id="new-bl" placeholder="N° Tracking / BL" style="padding:8px;">
                <input type="text" id="new-container" placeholder="N° Conteneur / Colis" style="padding:8px;">
                <input type="text" id="new-vessel" placeholder="Navire / Vol" style="padding:8px;">
                <div style="grid-column: span 2;">
                    <label style="font-size: 0.6rem; font-weight: bold;">DATE ESTIMÉE ARRIVÉE (ETA)</label>
                    <input type="date" id="new-date-ship" style="padding:8px;">
                </div>
            </div>
        </div>

        <div style="margin-top: 15px; display:grid; grid-template-columns:1fr 1fr 1fr; gap:10px;">
            <div><label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">PRIX ACHAT ($)</label><input type="number" id="new-achat-usd" value="0" oninput="updatePricePreview()"></div>
            <div><label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">POIDS (KG)</label><input type="number" id="new-weight" value="1" oninput="updatePricePreview()"></div>
            <div><label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">VOL. (CBM)</label><input type="number" id="new-cbm" value="0.1" step="0.1" oninput="updatePricePreview()"></div>
        </div>

        <div id="price-preview" style="margin-top: 15px; padding: 15px; background: #f0fdf4; border-radius: 10px; border: 1px solid #bbf7d0;">
            <!-- Rendu dynamique par JS -->
        </div>

        <div class="actions" style="margin-top:20px; display: flex; gap: 10px;">
            <button onclick="hideModal()" style="flex:1; border:none; border-radius:10px; cursor:pointer; background:#f1f5f9; padding:12px; font-weight: bold; color: #64748b;">Fermer</button>
            <button onclick="addProject()" class="btn-primary" style="flex:2;">CRÉER LE DOSSIER</button>
        </div>
    </div>
</div>

<script>
    // CONFIGURATION LOGISTIQUE HOMOLOGUÉE
    const LOGISTICS_CONFIG = {
        // MARITIME LCL (Groupage)
        sea_lcl_divers: { type: 'MARITIME', label: 'LCL Divers', rate: 9500, mode: 'kg', fixed_douane: 45000 },
        sea_lcl_elec: { type: 'MARITIME', label: 'LCL Électronique', rate: 14000, mode: 'kg', fixed_douane: 65000 },
        sea_moto: { type: 'MARITIME', label: 'Forfait Moto', rate: 350000, mode: 'flat', fixed_douane: 0 },
        
        // MARITIME FCL (Plein)
        sea_fcl_20: { type: 'MARITIME', label: "Conteneur 20'", rate: 4200000, mode: 'flat', fixed_douane: 0 },
        sea_fcl_40: { type: 'MARITIME', label: "Conteneur 40' HC", rate: 6800000, mode: 'flat', fixed_douane: 0 },
        
        // AÉRIEN
        air_standard: { type: 'AÉRIEN', label: 'Aérien Std', rate: 12500, mode: 'kg_vol', fixed_douane: 35000 },
        air_premium: { type: 'AÉRIEN', label: 'Aérien Premium', rate: 16000, mode: 'kg_vol', fixed_douane: 55000 }
    };

    const USD_TO_XAF = 620; // Taux de change moyen logistique

    const USERS = [
        { email: 'mbengmveyadmin@gmail.com', password: '12901564', name: 'Super Admin', role: 'ADMINISTRATEUR' },
        { email: 'assistantgestion@ct241.com', password: '20022029', name: 'Equipe Logistique', role: 'GESTIONNAIRE' }
    ];

    let projects = JSON.parse(localStorage.getItem('ct241_secured_v3')) || [];
    let currentId = null;
    let currentUser = null;
    let currentFilter = 'active';

    const STEP_NAMES = ["Achat & Groupage", "Transit International", "Dédouanement", "Livré / Dépot"];

    window.onload = function() {
        lucide.createIcons();
        document.getElementById('btn-submit-login').addEventListener('click', attemptLogin);
        updatePricePreview();
    }

    // --- MOTEUR DE CALCUL INTELLIGENT ---
    function calculateLogistics(p) {
        const config = LOGISTICS_CONFIG[p.transport_mode] || LOGISTICS_CONFIG.sea_lcl_divers;
        let fret_calc = 0;
        let details_fret = "";

        if (config.mode === 'kg') {
            fret_calc = p.weight * config.rate;
            details_fret = `${p.weight} kg x ${config.rate.toLocaleString()} F`;
        } else if (config.mode === 'flat') {
            fret_calc = config.rate;
            details_fret = `Forfait fixe ${config.label}`;
        } else if (config.mode === 'kg_vol') {
            // Règle du poids volumétrique aérien (L*l*h/6000 ou 167kg/m3)
            const weight_vol = p.cbm * 167;
            const final_weight = Math.max(p.weight, weight_vol);
            fret_calc = final_weight * config.rate;
            details_fret = `${final_weight.toFixed(1)} kg (taxable) x ${config.rate.toLocaleString()} F`;
        }

        const achat_xaf = p.achat_usd * USD_TO_XAF;
        const total = achat_xaf + fret_calc + config.fixed_douane;

        return {
            total,
            achat_xaf,
            fret_calc,
            douane_fixed: config.fixed_douane,
            details_fret,
            type: config.type
        };
    }

    function updatePricePreview() {
        const dummyProject = {
            transport_mode: document.getElementById('new-transport-mode').value,
            weight: parseFloat(document.getElementById('new-weight').value) || 0,
            cbm: parseFloat(document.getElementById('new-cbm').value) || 0,
            achat_usd: parseFloat(document.getElementById('new-achat-usd').value) || 0
        };

        const res = calculateLogistics(dummyProject);
        const container = document.getElementById('price-preview');
        
        container.innerHTML = `
            <div class="calc-row"><span>Achat (Conv. USD/XAF)</span><span>${res.achat_xaf.toLocaleString()} F</span></div>
            <div class="calc-row"><span>Fret (${res.type})</span><span>${res.fret_calc.toLocaleString()} F</span></div>
            <div class="calc-row"><span>Frais Douane Fixes</span><span>${res.douane_fixed.toLocaleString()} F</span></div>
            <div class="calc-total"><span>ESTIMATION TOTALE</span><span>${res.total.toLocaleString()} F CFA</span></div>
            <small style="color: #666; display:block; margin-top:5px;">* ${res.details_fret}</small>
        `;
    }

    function toggleTransportFields() {
        const mode = document.getElementById('new-transport-mode').value;
        const icon = mode.startsWith('air') ? 'plane' : 'ship';
        // On pourrait changer les placeholders dynamiquement ici
        updatePricePreview();
    }

    // --- AUTH & NAVIGATION ---
    function attemptLogin() {
        const email = document.getElementById('login-email').value.trim().toLowerCase();
        const pass = document.getElementById('login-password').value;
        const user = USERS.find(u => u.email.toLowerCase() === email && u.password === pass);
        if (user) {
            currentUser = user;
            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('app-content').style.display = 'flex';
            document.getElementById('current-user-name').innerText = user.name;
            document.getElementById('current-user-role').innerText = user.role;
            renderList();
        } else {
            document.getElementById('login-error').style.display = 'block';
        }
    }

    function switchList(type) {
        currentFilter = type;
        document.getElementById('tab-active').classList.toggle('active', type === 'active');
        document.getElementById('tab-done').classList.toggle('active', type === 'done');
        renderList();
    }

    function renderList() {
        const list = document.getElementById('project-list');
        list.innerHTML = '';
        const filtered = projects.filter(p => currentFilter === 'active' ? p.step < 4 : p.step >= 4);
        document.getElementById('count-active').innerText = projects.filter(p => p.step < 4).length;
        document.getElementById('count-done').innerText = projects.filter(p => p.step >= 4).length;

        filtered.forEach(p => {
            const li = document.createElement('li');
            li.className = `project-item ${p.id === currentId ? 'active' : ''}`;
            const transport = LOGISTICS_CONFIG[p.transport_mode]?.type || 'SEA';
            li.innerHTML = `
                <div style="display:flex; justify-content:space-between; align-items:center;">
                    <span style="font-weight:bold; font-size:0.9rem;">${p.name}</span>
                    <span style="font-size:0.6rem; background:#eee; padding:2px 4px; border-radius:3px;">${transport}</span>
                </div>
                <div style="font-size:0.75rem; color:#64748b;">👤 ${p.clientName}</div>
                <div style="font-size:0.65rem; color:var(--gabon-bleu); font-weight:600;">Étape: ${STEP_NAMES[p.step] || 'Clôturé'}</div>
            `;
            li.onclick = () => selectProject(p.id);
            list.appendChild(li);
        });
    }

    function selectProject(id) {
        currentId = id;
        const p = projects.find(proj => proj.id === id);
        if (!p) return;

        const res = calculateLogistics(p);
        document.getElementById('empty-state').style.display = 'none';
        document.getElementById('project-details').style.display = 'block';
        document.getElementById('view-title').innerText = p.name;
        document.getElementById('view-transport-type').innerText = res.type;
        document.getElementById('view-badge').innerText = p.step >= 4 ? "TERMINÉ" : "EN TRANSIT";
        document.getElementById('view-badge').className = `badge ${p.step >= 4 ? 'badge-completed' : 'badge-active'}`;
        
        document.getElementById('view-desc').innerText = `Créé le ${p.date} | Poids: ${p.weight}kg | Vol: ${p.cbm}CBM`;
        document.getElementById('view-client-info').innerText = `${p.clientName} (${p.clientPhone})`;
        
        // Tracking
        document.getElementById('transport-details-block').style.display = 'block';
        document.getElementById('v-armateur').innerText = p.tracking?.armateur || "---";
        document.getElementById('v-bl').innerText = p.tracking?.bl || "Attente...";
        document.getElementById('v-container').innerText = p.tracking?.container || "---";
        document.getElementById('v-vessel').innerText = p.tracking?.vessel || "---";
        document.getElementById('v-date-ship').innerText = p.tracking?.dateShip ? new Date(p.tracking.dateShip).toLocaleDateString() : "Non définie";

        // Icon Transport
        const iconEl = document.getElementById('transport-icon');
        iconEl.setAttribute('data-lucide', res.type === 'AÉRIEN' ? 'plane' : 'ship');

        // Bilan financier
        document.getElementById('financial-breakdown').innerHTML = `
            <div class="calc-row"><span>Valeur Marchandise ($ -> F)</span><span>${res.achat_xaf.toLocaleString()} F</span></div>
            <div class="calc-row"><span>Prestation Fret (${res.type})</span><span>${res.fret_calc.toLocaleString()} F</span></div>
            <div class="calc-row"><span>Forfait Douane Gabon</span><span>${res.douane_fixed.toLocaleString()} F</span></div>
            <p style="font-size: 0.7rem; color: #94a3b8; margin-top:5px;">Calcul basé sur : ${res.details_fret}</p>
        `;
        document.getElementById('val-total').innerText = res.total.toLocaleString() + " F CFA";
        document.getElementById('view-notes').value = p.notes || "";
        
        selectStep(p.step >= 4 ? 3 : p.step);
        lucide.createIcons();
        renderList();
    }

    function selectStep(idx) {
        const p = projects.find(proj => proj.id === currentId);
        document.querySelectorAll('.step').forEach((s, i) => {
            s.classList.remove('completed', 'current');
            if(i < p.step) s.classList.add('completed');
            if(i === idx) s.classList.add('current');
        });
        document.getElementById('current-step-name').innerText = STEP_NAMES[idx];
        document.getElementById('btn-validate-step').style.display = (idx === p.step && p.step < 4) ? 'block' : 'none';
        renderPhotos(idx);
    }

    function renderPhotos(stepIdx) {
        const p = projects.find(proj => proj.id === currentId);
        const container = document.getElementById('photo-container');
        container.innerHTML = '';
        (p.photos[stepIdx] || []).forEach(src => {
            const img = document.createElement('img');
            img.src = src; img.className = 'photo-item';
            img.onclick = () => window.open(src);
            container.appendChild(img);
        });
        if (p.step < 4 && stepIdx === p.step) {
            const addBtn = document.createElement('div');
            addBtn.className = 'photo-upload-placeholder';
            addBtn.innerHTML = `<i data-lucide="camera"></i><span>Ajouter</span>`;
            addBtn.onclick = () => document.getElementById('hidden-camera-input').click();
            container.appendChild(addBtn);
        }
        lucide.createIcons();
    }

    document.getElementById('hidden-camera-input').onchange = (e) => {
        const file = e.target.files[0];
        if(!file) return;
        const reader = new FileReader();
        reader.onload = (ev) => {
            const p = projects.find(proj => proj.id === currentId);
            if(!p.photos[p.step]) p.photos[p.step] = [];
            p.photos[p.step].push(ev.target.result);
            save(); renderPhotos(p.step);
        };
        reader.readAsDataURL(file);
    };

    function validateCurrentStep() {
        const p = projects.find(proj => proj.id === currentId);
        if(confirm(`Passer à l'étape : ${STEP_NAMES[p.step + 1] || 'Terminé'} ?`)) {
            p.step++; 
            save(); selectProject(currentId);
        }
    }

    function addProject() {
        const name = document.getElementById('new-name').value;
        if(!name) return alert("Nom requis");

        const newP = {
            id: Date.now(),
            name: name,
            clientName: document.getElementById('new-client-name').value,
            clientPhone: document.getElementById('new-client-phone').value,
            transport_mode: document.getElementById('new-transport-mode').value,
            weight: parseFloat(document.getElementById('new-weight').value) || 1,
            cbm: parseFloat(document.getElementById('new-cbm').value) || 0.1,
            achat_usd: parseFloat(document.getElementById('new-achat-usd').value) || 0,
            step: 0,
            notes: "",
            tracking: {
                armateur: document.getElementById('new-armateur').value,
                bl: document.getElementById('new-bl').value,
                container: document.getElementById('new-container').value,
                vessel: document.getElementById('new-vessel').value,
                dateShip: document.getElementById('new-date-ship').value
            },
            photos: {0:[], 1:[], 2:[], 3:[]},
            date: new Date().toLocaleDateString()
        };
        projects.unshift(newP);
        save(); hideModal(); switchList('active'); selectProject(newP.id);
    }

    function saveNotes() {
        const p = projects.find(proj => proj.id === currentId);
        if(p) {
            p.notes = document.getElementById('view-notes').value;
            save();
        }
    }

    function deleteProject() {
        if(currentUser.role !== 'ADMINISTRATEUR') return alert("Action réservée aux Admins.");
        if(confirm("Supprimer définitivement ce dossier ?")) {
            projects = projects.filter(p => p.id !== currentId);
            currentId = null; save(); renderList();
            document.getElementById('project-details').style.display = 'none';
            document.getElementById('empty-state').style.display = 'flex';
        }
    }

    function save() { localStorage.setItem('ct241_secured_v3', JSON.stringify(projects)); }
    function showModal() { document.getElementById('modal-project').style.display = 'flex'; }
    function hideModal() { document.getElementById('modal-project').style.display = 'none'; }
    function logout() { location.reload(); }
</script>
</body>
</html>
