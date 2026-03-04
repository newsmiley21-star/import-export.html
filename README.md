<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CT241 - LOGISTIQUE SÉCURISÉE</title>
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

        /* --- UI ELEMENTS --- */
        input, select, textarea { width: 100%; padding: 12px; border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box; margin-top: 5px; font-size: 0.9rem; transition: 0.2s; }
        input:focus, select:focus { border-color: var(--gabon-bleu); outline: none; box-shadow: 0 0 0 3px rgba(58, 117, 196, 0.1); }
        .btn-primary { background: var(--gabon-vert); color: white; border: none; padding: 12px 20px; border-radius: 10px; cursor: pointer; font-weight: bold; width: 100%; }
        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.5); align-items: center; justify-content: center; z-index: 1500; backdrop-filter: blur(4px); }
        .modal-content { background: white; padding: 30px; border-radius: 20px; width: 450px; max-height: 90vh; overflow-y: auto; }
        .btn-delete { background: #fee2e2; color: var(--danger); border: none; padding: 10px; border-radius: 8px; cursor: pointer; transition: 0.2s; }
        .btn-delete:hover { background: var(--danger); color: white; }
    </style>
</head>
<body>

<!-- INPUT CACHÉ POUR LA CAPTURE PHOTO -->
<input type="file" id="hidden-camera-input" accept="image/*" capture="environment" style="display: none;">

<!-- LOGIN SCREEN -->
<div id="login-screen">
    <div class="login-card">
        <div class="login-header">
            <div style="background: var(--gabon-vert); width: 60px; height: 60px; border-radius: 15px; margin: 0 auto 15px; display: flex; align-items: center; justify-content: center; color: white;">
                <i data-lucide="lock" size="30"></i>
            </div>
            <h2>Portail CT241</h2>
            <p>Connectez-vous pour gérer la logistique</p>
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
        
        <div style="margin-top: 25px; font-size: 0.7rem; color: #94a3b8; text-align: center;">
            Besoin d'aide ? Contactez l'administrateur système.
        </div>
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
                <p>Sélectionnez un dossier dans la liste pour voir les détails</p>
            </div>

            <div id="project-details" style="display: none;">
                <!-- HEADER CARTE -->
                <div class="card">
                    <div style="display: flex; justify-content: space-between;">
                        <div>
                            <span id="view-badge" class="badge"></span>
                            <h2 id="view-title" style="margin: 10px 0 5px 0;"></h2>
                            <p id="view-desc" style="font-size: 0.8rem; color: #64748b; margin: 0;"></p>
                        </div>
                        <button class="btn-delete" onclick="deleteProject()"><i data-lucide="trash-2" size="18"></i></button>
                    </div>

                    <div class="client-card">
                        <i data-lucide="user" style="color: var(--gabon-bleu);"></i>
                        <div>
                            <div style="font-size: 0.7rem; text-transform: uppercase; color: #94a3b8; font-weight: bold;">Propriétaire</div>
                            <div id="view-client-info" style="font-weight: 600; color: var(--dark);">---</div>
                        </div>
                    </div>

                    <div class="stepper">
                        <div class="step" onclick="selectStep(0)">
                            <div class="step-circle">1</div><div class="step-label">Achat</div>
                        </div>
                        <div class="step" onclick="selectStep(1)">
                            <div class="step-circle">2</div><div class="step-label">Fret</div>
                        </div>
                        <div class="step" onclick="selectStep(2)">
                            <div class="step-circle">3</div><div class="step-label">Douane</div>
                        </div>
                        <div class="step" onclick="selectStep(3)">
                            <div class="step-circle">4</div><div class="step-label">Livré</div>
                        </div>
                    </div>
                </div>

                <div class="card">
                    <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom: 15px;">
                        <h3 style="margin:0">Preuves d'étape : <span id="current-step-name" style="color:var(--gabon-bleu)">---</span></h3>
                        <button id="btn-validate-step" class="btn-primary" style="display:none; width: auto;" onclick="validateCurrentStep()">Valider l'étape</button>
                    </div>
                    
                    <div class="photo-grid" id="photo-container"></div>
                </div>

                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
                    <div class="card">
                        <h3 style="margin-top:0">Finances Estimées</h3>
                        <div id="financial-breakdown" style="font-size: 0.8rem; color: #64748b; line-height: 1.6;">
                            <!-- Détails de calcul insérés ici -->
                        </div>
                        <p style="font-size: 1.2rem; border-top: 1px solid var(--border); margin-top: 10px; padding-top: 10px;">Total Client : <strong id="val-total" style="color:var(--gabon-vert)">0 F</strong></p>
                    </div>
                    <div class="card">
                        <h3 style="margin-top:0">Notes internes</h3>
                        <textarea id="view-notes" style="height:80px; resize:none;" onblur="saveNotes()" placeholder="Ajouter une observation..."></textarea>
                    </div>
                </div>
            </div>
        </main>
    </div>
</div>

<!-- MODAL NOUVELLE MISSION -->
<div id="modal-project" class="modal">
    <div class="modal-content">
        <h3 style="margin-top:0; border-bottom: 1px solid var(--border); padding-bottom: 15px;">Nouveau Dossier Logistique</h3>
        
        <div style="margin-top: 15px;">
            <label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">DÉSIGNATION</label>
            <input type="text" id="new-name" placeholder="Ex: Scooter Yamaha 125cc">
        </div>

        <div style="margin-top: 15px; display: grid; grid-template-columns: 1fr 1fr; gap: 15px;">
            <div>
                <label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">NOM CLIENT</label>
                <input type="text" id="new-client-name" placeholder="M. Jean Dupont">
            </div>
            <div>
                <label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">TÉLÉPHONE</label>
                <input type="text" id="new-client-phone" placeholder="077 XX XX XX">
            </div>
        </div>

        <div style="margin-top: 15px;">
            <label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">CATÉGORIE DE MARCHANDISE</label>
            <select id="new-category" onchange="updatePricePreview()">
                <option value="divers">Divers / Colis standard</option>
                <option value="electronique">Électronique / Informatique</option>
                <option value="vehicule_leger">Véhicule Léger (Moto/Scooter)</option>
                <option value="automobile">Automobile / Gros Engin</option>
            </select>
        </div>

        <div style="margin-top: 15px; display:grid; grid-template-columns:1fr 1fr; gap:15px;">
            <div>
                <label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">PRIX ACHAT UNITAIRE (F)</label>
                <input type="number" id="new-achat" placeholder="Ex: 500000" oninput="updatePricePreview()">
            </div>
            <div>
                <label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">POIDS EST. (KG) / UNITÉ</label>
                <input type="number" id="new-weight" value="1" oninput="updatePricePreview()">
            </div>
        </div>
        
        <div style="margin-top: 15px; display:grid; grid-template-columns:1fr 1fr; gap:15px;">
            <div>
                <label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">QUANTITÉ</label>
                <input type="number" id="new-qty" value="1" oninput="updatePricePreview()">
            </div>
            <div>
                <label style="font-size: 0.75rem; font-weight: bold; color: #64748b;">HUB D'EXPÉDITION</label>
                <select id="new-city">
                    <option>Guangzhou</option><option>Shenzhen</option><option>Yiwu</option><option>Jiangsu</option>
                </select>
            </div>
        </div>

        <div id="price-preview" style="margin-top: 15px; padding: 12px; background: #eff6ff; border-radius: 8px; font-size: 0.8rem; color: var(--gabon-bleu); border: 1px dashed var(--gabon-bleu);">
            Estimation totale : <strong>0 F CFA</strong>
        </div>

        <div class="actions" style="margin-top:25px; border-top: 1px solid var(--border); padding-top: 20px; display: flex; gap: 10px;">
            <button onclick="hideModal()" style="flex:1; border:none; border-radius:10px; cursor:pointer; background:#f1f5f9; padding:12px; font-weight: bold; color: #64748b;">Annuler</button>
            <button onclick="addProject()" class="btn-primary" style="flex:1;">Créer le dossier</button>
        </div>
    </div>
</div>

<script>
    // --- CONFIGURATION DE TARIFICATION DYNAMIQUE ---
    const FREIGHT_CONFIG = {
        divers: { fret_kg: 8500, douane_fixe: 25000, label: "Divers" },
        electronique: { fret_kg: 12000, douane_fixe: 45000, label: "Électronique" },
        vehicule_leger: { fret_kg: 150000, douane_fixe: 120000, label: "Véhicule Léger", is_flat_fret: true },
        automobile: { fret_kg: 850000, douane_fixe: 650000, label: "Automobile", is_flat_fret: true }
    };

    const USERS = [
        { email: 'mbengmveyadmin@gmail.com', password: '12901564', name: 'Super Admin', role: 'ADMINISTRATEUR' },
        { email: 'assistantgestion@ct241.com', password: '20022029', name: 'Equipe Logistique', role: 'GESTIONNAIRE' }
    ];

    let projects = JSON.parse(localStorage.getItem('ct241_secured_v2')) || [];
    let currentId = null;
    let currentUser = null;
    let currentFilter = 'active';
    let targetStepForUpload = null;

    const STEP_NAMES = ["Achat Chine", "Fret Maritime", "Douane Owendo", "Livraison Client"];

    window.onload = function() {
        lucide.createIcons();
        document.getElementById('btn-submit-login').addEventListener('click', attemptLogin);
        document.getElementById('login-password').addEventListener('keypress', (e) => e.key === 'Enter' && attemptLogin());
        document.getElementById('hidden-camera-input').addEventListener('change', handleFileSelect);
    }

    function calculateTotal(p) {
        const config = FREIGHT_CONFIG[p.category || 'divers'];
        const fretBase = config.is_flat_fret ? config.fret_kg : (config.fret_kg * (p.weight || 1));
        const totalUnitaire = p.achat + fretBase + config.douane_fixe;
        return {
            total: totalUnitaire * p.qty,
            details: {
                achat: p.achat * p.qty,
                fret: fretBase * p.qty,
                douane: config.douane_fixe * p.qty,
                unitaire: totalUnitaire
            }
        };
    }

    function updatePricePreview() {
        const achat = parseInt(document.getElementById('new-achat').value) || 0;
        const weight = parseFloat(document.getElementById('new-weight').value) || 1;
        const qty = parseInt(document.getElementById('new-qty').value) || 1;
        const cat = document.getElementById('new-category').value;
        
        const mockP = { achat, weight, qty, category: cat };
        const result = calculateTotal(mockP);
        
        document.getElementById('price-preview').innerHTML = `Estimation : <strong>${result.total.toLocaleString()} F CFA</strong> <br> <small>(Achat + Fret + Douane incluse)</small>`;
    }

    function attemptLogin() {
        const emailInput = document.getElementById('login-email').value.trim().toLowerCase();
        const passInput = document.getElementById('login-password').value;
        const userFound = USERS.find(u => u.email.toLowerCase() === emailInput && u.password === passInput);

        if (userFound) {
            currentUser = userFound;
            document.getElementById('login-screen').style.display = 'none';
            document.getElementById('app-content').style.display = 'flex';
            document.getElementById('current-user-name').innerText = userFound.name;
            document.getElementById('current-user-role').innerText = userFound.role;
            renderList();
        } else {
            const errorMsg = document.getElementById('login-error');
            errorMsg.style.display = 'block';
            setTimeout(() => { errorMsg.style.display = 'none'; }, 3000);
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

        if(filtered.length === 0) {
            list.innerHTML = `<div style="text-align:center; padding:30px; color:#94a3b8; font-size:0.75rem;">Aucun dossier trouvé</div>`;
        }

        filtered.forEach(p => {
            const li = document.createElement('li');
            li.className = `project-item ${p.id === currentId ? 'active' : ''}`;
            li.innerHTML = `
                <div style="display:flex; justify-content:space-between; font-weight:bold; font-size:0.85rem;">
                    <span>${p.name}</span> 
                    <span style="color:var(--gabon-bleu)">x${p.qty}</span>
                </div>
                <div style="font-size:0.75rem; color:#64748b;">👤 ${p.clientName}</div>
                <div style="font-size:0.65rem; color:#94a3b8; display:flex; justify-content:space-between; margin-top:5px;">
                    <span>${FREIGHT_CONFIG[p.category]?.label || 'Standard'}</span>
                    <span style="color:${p.step >= 4 ? 'var(--gabon-vert)' : 'var(--dark)'}">${p.step >= 4 ? 'TERMINÉ' : STEP_NAMES[p.step]}</span>
                </div>
            `;
            li.onclick = () => selectProject(p.id);
            list.appendChild(li);
        });
        lucide.createIcons();
    }

    function selectProject(id) {
        currentId = id;
        const p = projects.find(proj => proj.id === id);
        if (!p) return;

        const financial = calculateTotal(p);
        
        document.getElementById('empty-state').style.display = 'none';
        document.getElementById('project-details').style.display = 'block';
        document.getElementById('view-title').innerText = p.name;
        document.getElementById('view-desc').innerText = `Hub: ${p.city} | Créé le ${p.date}`;
        document.getElementById('view-client-info').innerText = `${p.clientName} (${p.clientPhone || 'Pas de contact'})`;
        document.getElementById('view-notes').value = p.notes || "";
        
        document.getElementById('financial-breakdown').innerHTML = `
            Achat global : ${financial.details.achat.toLocaleString()} F<br>
            Fret estimé : ${financial.details.fret.toLocaleString()} F<br>
            Forfait Douane : ${financial.details.douane.toLocaleString()} F
        `;
        document.getElementById('val-total').innerText = financial.total.toLocaleString() + " F CFA";
        
        const badge = document.getElementById('view-badge');
        badge.innerText = p.step >= 4 ? "Livraison Effectuée" : "En cours de Transit";
        badge.className = `badge ${p.step >= 4 ? 'badge-completed' : 'badge-active'}`;

        selectStep(p.step >= 4 ? 3 : p.step);
        renderList();
    }

    function selectStep(idx) {
        const p = projects.find(proj => proj.id === currentId);
        if (!p) return;
        document.querySelectorAll('.step').forEach((s, i) => {
            s.classList.remove('completed', 'current');
            if(i < p.step) s.classList.add('completed');
            if(i === idx) s.classList.add('current');
        });
        document.getElementById('current-step-name').innerText = STEP_NAMES[idx];
        const btnVal = document.getElementById('btn-validate-step');
        btnVal.style.display = (idx === p.step && p.step < 4) ? 'block' : 'none';
        renderPhotos(idx);
    }

    function renderPhotos(stepIdx) {
        const p = projects.find(proj => proj.id === currentId);
        const container = document.getElementById('photo-container');
        container.innerHTML = '';
        const stepPhotos = p.photos[stepIdx] || [];

        stepPhotos.forEach(src => {
            const img = document.createElement('img');
            img.src = src; img.className = 'photo-item';
            img.onclick = () => window.open(src);
            container.appendChild(img);
        });

        if (p.step < 4 && stepIdx === p.step) {
            const addBtn = document.createElement('div');
            addBtn.className = 'photo-upload-placeholder';
            addBtn.innerHTML = `<i data-lucide="camera" size="20"></i><span>Capturer preuve</span>`;
            addBtn.onclick = () => triggerCamera(stepIdx);
            container.appendChild(addBtn);
        }
        lucide.createIcons();
    }

    function triggerCamera(stepIdx) {
        targetStepForUpload = stepIdx;
        document.getElementById('hidden-camera-input').click();
    }

    function handleFileSelect(event) {
        const file = event.target.files[0];
        if (!file) return;
        const reader = new FileReader();
        reader.onload = (e) => {
            const p = projects.find(proj => proj.id === currentId);
            p.photos[targetStepForUpload].push(e.target.result);
            save(); renderPhotos(targetStepForUpload);
        };
        reader.readAsDataURL(file);
        event.target.value = '';
    }

    function validateCurrentStep() {
        const p = projects.find(proj => proj.id === currentId);
        if(!p.photos[p.step].length) return alert("Une photo de preuve est requise.");
        if(confirm("Confirmer la validation de cette étape ?")) {
            p.step++;
            save();
            if (p.step >= 4) switchList('done');
            selectProject(currentId);
        }
    }

    function addProject() {
        const name = document.getElementById('new-name').value;
        const client = document.getElementById('new-client-name').value;
        if(!name || !client) return alert("Nom du produit et du client obligatoires.");

        const newP = {
            id: Date.now(),
            name,
            clientName: client,
            clientPhone: document.getElementById('new-client-phone').value,
            achat: parseInt(document.getElementById('new-achat').value) || 0,
            weight: parseFloat(document.getElementById('new-weight').value) || 1,
            category: document.getElementById('new-category').value,
            qty: parseInt(document.getElementById('new-qty').value) || 1,
            city: document.getElementById('new-city').value,
            step: 0,
            notes: "",
            photos: {0:[], 1:[], 2:[], 3:[]},
            date: new Date().toLocaleDateString()
        };
        projects.unshift(newP);
        save(); hideModal(); switchList('active'); selectProject(newP.id);
    }

    function deleteProject() {
        if(currentUser.role !== 'ADMINISTRATEUR') return alert("Action réservée aux admins.");
        if(confirm("Supprimer ce dossier ?")) {
            projects = projects.filter(p => p.id !== currentId);
            currentId = null;
            document.getElementById('project-details').style.display = 'none';
            document.getElementById('empty-state').style.display = 'flex';
            save(); renderList();
        }
    }

    function save() { localStorage.setItem('ct241_secured_v2', JSON.stringify(projects)); }
    function saveNotes() { 
        const p = projects.find(proj => proj.id === currentId);
        if(p) { p.notes = document.getElementById('view-notes').value; save(); }
    }
    function showModal() { document.getElementById('modal-project').style.display = 'flex'; updatePricePreview(); }
    function hideModal() { document.getElementById('modal-project').style.display = 'none'; }
    function logout() { location.reload(); }
</script>
</body>
</html>
