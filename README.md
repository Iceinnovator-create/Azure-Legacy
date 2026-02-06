# Azure-Legacy
Esse é um jogo Inspirado em One Piece 
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Azure Legacy v0.0.1 - Original</title>
    <style>
        :root {
            --bg: #020617; --panel: #1e293b; --gold: #fbbf24;
            --red: #ef4444; --blue: #3b82f6; --green: #22c55e;
            --white: #f8fafc;
        }
        * { box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }
        body { background: var(--bg); color: var(--white); margin: 0; display: flex; justify-content: center; height: 100vh; overflow: hidden; }
        
        #game-window {
            width: 100%; max-width: 480px; height: 100%; background: var(--panel);
            position: relative; display: flex; flex-direction: column; border: 2px solid #334155;
        }

        .screen { padding: 15px; flex: 1; display: flex; flex-direction: column; overflow-y: auto; }
        .hidden { display: none !important; }

        .btn {
            background: #334155; border: 1px solid var(--gold); color: var(--gold);
            padding: 10px; margin: 4px 0; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%;
            transition: 0.2s; text-transform: uppercase; font-size: 0.75rem;
        }
        .btn:hover:not(:disabled) { background: #475569; }
        .btn:disabled { opacity: 0.3; cursor: not-allowed; }
        .btn-green { border-color: var(--green); color: var(--green); }

        .nav-bar { display: flex; background: #000; padding: 4px; gap: 2px; border-bottom: 2px solid var(--gold); }
        .nav-btn { flex: 1; font-size: 0.6rem; padding: 8px 2px; background: #1e293b; border: 1px solid #444; color: #fff; cursor: pointer; }
        .nav-btn.active { background: var(--gold); color: #000; font-weight: bold; }

        .bar-outer { width: 100%; height: 12px; background: #000; border-radius: 6px; margin-bottom: 6px; overflow: hidden; border: 1px solid #444; }
        .bar-inner { height: 100%; transition: width 0.3s; width: 100%; }
        
        #log { flex: 1; background: #000; padding: 8px; font-size: 0.8rem; overflow-y: auto; margin: 8px 0; border-radius: 4px; border: 1px solid #334155; color: #cbd5e1; }
        
        .card { background: #0f172a; padding: 10px; border-radius: 6px; margin-bottom: 8px; border-left: 3px solid var(--gold); }
        .passives-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 5px; font-size: 0.7rem; }
        .passive-tag { background: #1e293b; padding: 4px; border-radius: 4px; border: 1px solid #334155; text-align: center; }

        input, select { background: #0f172a; border: 1px solid #444; color: #fff; padding: 10px; width: 100%; margin-bottom: 8px; border-radius: 4px; }
    </style>
</head>
<body>

<div id="game-window">
    <div id="main-nav" class="nav-bar hidden">
        <button class="nav-btn" id="nav-combat" onclick="ui.show('screen-combat')">BATALHA</button>
        <button class="nav-btn" id="nav-stats" onclick="ui.show('screen-stats')">PERFIL</button>
        <button class="nav-btn" id="nav-haki" onclick="ui.show('screen-haki')">ESPÍRITO</button>
    </div>

    <div id="screen-start" class="screen">
        <h1 style="text-align:center; color:var(--gold); margin-top:40px;">AZURE LEGACY <br><small>v0.0.1 ORIGEM</small></h1>
        <div style="flex:1; display:flex; flex-direction:column; align-items:center; justify-content:center;">
            <div style="font-size:4rem;">🧭</div>
            <p id="save-info" style="font-size: 0.8rem; color: #94a3b8; text-align:center;"></p>
        </div>
        <button class="btn btn-green" id="btn-continue" onclick="game.load()" style="display:none; height: 50px;">RETOMAR EXPEDIÇÃO</button>
        <button class="btn" onclick="ui.show('screen-fruit-maker')">NOVA JORNADA</button>
    </div>

    <div id="screen-fruit-maker" class="screen hidden">
        <h2>Despertar Astral</h2>
        <input type="text" id="f-name" placeholder="Nome da sua Essência (Poder)">
        <select id="f-type">
            <option value="Manifestante">Manifestante (Econ. Energia)</option>
            <option value="Primordial">Primordial (Regeneração HP)</option>
            <option value="Elemental">Elemental (Intangibilidade)</option>
        </select>
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 5px;">
            <input type="text" id="f-m1" placeholder="Técnica 1">
            <input type="text" id="f-m2" placeholder="Técnica 2">
            <input type="text" id="f-m3" placeholder="Técnica 3">
            <input type="text" id="f-m4" placeholder="ARCANO FINAL">
        </div>
        <button class="btn btn-green" onclick="game.init()">ZARPAR PARA O ABISMO</button>
    </div>

    <div id="screen-combat" class="screen hidden">
        <div id="enemy-ui">
            <div style="display:flex; justify-content:space-between; align-items: center; margin-bottom: 2px;">
                <b id="e-name" style="color:#ef4444">Oponente</b>
                <span id="e-haki" style="font-size: 0.6rem; color: var(--gold); display:none;">● VONTADE ATIVA</span>
            </div>
            <div class="bar-outer"><div id="e-hp-bar" class="bar-inner" style="background:#ef4444;"></div></div>
        </div>

        <div id="log"></div>

        <div id="player-ui">
            <div style="display:flex; justify-content:space-between; font-size:0.7rem;">
                <span>VIGOR: <b id="p-hp-txt">100/100</b></span>
                <span>FÔLEGO: <b id="p-en-txt">100/100</b></span>
            </div>
            <div class="bar-outer"><div id="p-hp-bar" class="bar-inner" style="background:#22c55e;"></div></div>
            <div class="bar-outer"><div id="p-en-bar" class="bar-inner" style="background:#3b82f6;"></div></div>
        </div>

        <div style="display:grid; grid-template-columns: 1fr 1fr; gap:5px;">
            <button class="btn" onclick="battle.hit(0)" id="btn-m1">T1</button>
            <button class="btn" onclick="battle.hit(1)" id="btn-m2">T2</button>
            <button class="btn" onclick="battle.hit(2)" id="btn-m3">T3</button>
            <button class="btn" onclick="battle.hit(3)" id="btn-m4">T4</button>
            <button class="btn btn-green" style="grid-column: span 2;" onclick="battle.recharge()">CONCENTRAR ENERGIA</button>
        </div>
    </div>

    <div id="screen-stats" class="screen hidden">
        <h2>Perfil do Explorador</h2>
        
        <div class="card">
            <div class="passives-grid">
                <div class="passive-tag">Linhagem: <b id="val-race">Humano</b></div>
                <div class="passive-tag">Facção: <b id="val-aff">Nômade</b></div>
                <div class="passive-tag">Pontos: <b id="stat-pts">0</b></div>
                <div class="passive-tag">Aureus: <b id="val-berries">0</b></div>
            </div>
        </div>

        <h3>Alterar Destino</h3>
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 8px;">
            <button class="btn" onclick="game.rollRace()">Mudar Linhagem (5k)</button>
            <button class="btn" onclick="game.rollAffiliation()">Mudar Facção (5k)</button>
        </div>

        <h3>Melhorias</h3>
        <button class="btn" onclick="game.statUp('hp')">+80 VITALIDADE</button>
        <button class="btn" onclick="game.statUp('en')">+50 ESTAMINA</button>
        <button class="btn" onclick="game.statUp('atk')">+15 PODER</button>
    </div>

    <div id="screen-haki" class="screen hidden">
        <h2>Vontade Espiritual</h2>
        <div class="card">Kote (Ataque): +<span id="h-atk">0</span> <button class="btn" onclick="game.train('atk')">Treinar (3k)</button></div>
        <div class="card">Satori (Esquiva): <span id="h-def">0</span>% <button class="btn" onclick="game.train('def')">Treinar (3k)</button></div>
    </div>
</div>

<script>
/* --- AZURE LEGACY ORIGINAL v0.0.1 --- */

let player = {
    hp: 120, maxHp: 120, en: 100, maxEn: 100, atk: 20,
    berries: 10000, bounty: 0, points: 0,
    race: "Humano", affiliation: "Nômade",
    fruit: { name: "", type: "", moves: [] },
    haki: { atk: 0, def: 0 },
    turnActive: false
};

const LINHAGENS = {
    "Humano": { hp: 1, atk: 1 },
    "Abissal": { hp: 1.5, atk: 1.1 },
    "Feral": { hp: 0.9, atk: 1.4 },
    "Alado": { hp: 0.8, atk: 1 },
    "Mecânico": { hp: 1.3, atk: 1.2 }
};

const ui = {
    show: (id) => {
        document.querySelectorAll('.screen').forEach(s => s.classList.add('hidden'));
        document.getElementById(id).classList.remove('hidden');
        document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
        const navId = "nav-" + id.split('-')[1];
        if(document.getElementById(navId)) document.getElementById(navId).classList.add('active');
        game.updateUI();
    },
    log: (msg, color = "#eee") => {
        const l = document.getElementById('log');
        const d = document.createElement('div');
        d.style.color = color;
        d.innerHTML = "> " + msg;
        l.prepend(d);
        if(l.childNodes.length > 15) l.lastChild.remove();
    }
};

const game = {
    init: () => {
        const fName = document.getElementById('f-name').value;
        if(!fName) return alert("Defina o nome do seu poder!");
        player.fruit.name = fName;
        player.fruit.type = document.getElementById('f-type').value;
        player.fruit.moves = [
            { name: document.getElementById('f-m1').value || "Golpe 1", cost: 10, dmg: 18 },
            { name: document.getElementById('f-m2').value || "Golpe 2", cost: 25, dmg: 40 },
            { name: document.getElementById('f-m3').value || "Golpe 3", cost: 45, dmg: 75 },
            { name: document.getElementById('f-m4').value || "ARCANO", cost: 90, dmg: 150 }
        ];
        player.hp = player.maxHp;
        document.getElementById('main-nav').classList.remove('hidden');
        game.save();
        ui.show('screen-combat');
        battle.newEnemy();
    },
    rollRace: () => {
        if(player.berries >= 5000) {
            player.berries -= 5000;
            const r = Object.keys(LINHAGENS);
            player.race = r[Math.floor(Math.random() * r.length)];
            ui.log("Nova Linhagem: " + player.race, "#3b82f6");
            game.save(); game.updateUI();
        }
    },
    rollAffiliation: () => {
        if(player.berries >= 5000) {
            player.berries -= 5000;
            const a = ["Saqueador", "Guardião", "Revolucionário"];
            player.affiliation = a[Math.floor(Math.random() * a.length)];
            ui.log("Nova Facção: " + player.affiliation, "#a855f7");
            game.save(); game.updateUI();
        }
    },
    statUp: (type) => {
        if(player.points > 0) {
            player.points--;
            if(type === 'hp') player.maxHp += 80;
            if(type === 'en') player.maxEn += 50;
            if(type === 'atk') player.atk += 15;
            game.save(); game.updateUI();
        }
    },
    train: (type) => {
        if(player.berries >= 3000) {
            player.berries -= 3000;
            if(type === 'atk') player.haki.atk += 20;
            else player.haki.def = Math.min(60, player.haki.def + 5);
            game.save(); game.updateUI();
        }
    },
    save: () => localStorage.setItem('azure_v001_orig', JSON.stringify(player)),
    load: () => {
        const d = localStorage.getItem('azure_v001_orig');
        if(d) {
            player = JSON.parse(d);
            document.getElementById('main-nav').classList.remove('hidden');
            ui.show('screen-combat');
            battle.newEnemy();
        }
    },
    updateUI: () => {
        document.getElementById('val-race').innerText = player.race;
        document.getElementById('val-aff').innerText = player.affiliation;
        document.getElementById('val-berries').innerText = player.berries.toLocaleString();
        document.getElementById('stat-pts').innerText = player.points;
        document.getElementById('h-atk').innerText = player.haki.atk;
        document.getElementById('h-def').innerText = player.haki.def;
        if(player.fruit.moves.length > 0) {
            for(let i=0; i<4; i++) document.getElementById('btn-m'+(i+1)).innerText = player.fruit.moves[i].name;
        }
        battle.updateBars();
    }
};

const battle = {
    enemy: { name: "", hp: 100, maxHp: 100, atk: 10, hasHaki: false },
    newEnemy: () => {
        const pool = ["Mercenário da Orla", "Sentinela de Aço", "Besta Profunda", "Saqueador de Ar"];
        battle.enemy.name = pool[Math.floor(Math.random()*pool.length)];
        const lv = 1 + (player.bounty / 500000);
        battle.enemy.maxHp = Math.floor(150 * lv);
        battle.enemy.hp = battle.enemy.maxHp;
        battle.enemy.atk = Math.floor(18 * lv);
        battle.enemy.hasHaki = (player.bounty > 1000000);
        document.getElementById('e-name').innerText = battle.enemy.name;
        document.getElementById('e-haki').style.display = battle.enemy.hasHaki ? "block" : "none";
        battle.updateBars();
    },
    hit: (idx) => {
        if(player.turnActive || player.hp <= 0) return;
        const move = player.fruit.moves[idx];
        if(player.en >= move.cost) {
            player.turnActive = true;
            if(!(player.fruit.type === 'Manifestante' && Math.random() < 0.2)) player.en -= move.cost;
            let d = Math.floor((move.dmg + player.atk + player.haki.atk) * (LINHAGENS[player.race].atk));
            battle.enemy.hp -= d;
            ui.log(`Você ativou ${move.name}! (-${d} HP)`, "#22c55e");
            battle.updateBars();
            if(battle.enemy.hp <= 0) setTimeout(battle.win, 600);
            else battle.enemyTurn();
        }
    },
    enemyTurn: () => {
        setTimeout(() => {
            if(battle.enemy.hp <= 0) return;
            if(player.fruit.type === 'Elemental' && !battle.enemy.hasHaki) {
                ui.log("O ataque atravessou você! (Forma Elemental)", "#fff");
            } else if(Math.random() * 100 < player.haki.def) {
                ui.log("Você previu e desviou!", "#3b82f6");
            } else {
                player.hp -= battle.enemy.atk;
                ui.log(`${battle.enemy.name} atacou! (-${battle.enemy.atk} HP)`, "#ef4444");
            }
            battle.updateBars();
            player.turnActive = false;
            if(player.hp <= 0) { alert("Sua jornada terminou."); location.reload(); }
        }, 700);
    },
    recharge: () => {
        if(player.turnActive) return;
        player.en = Math.min(player.maxEn, player.en + 50);
        if(player.fruit.type === 'Primordial') player.hp = Math.min(player.maxHp, player.hp + 40);
        ui.log("Recuperando fôlego...", "#3b82f6");
        battle.updateBars();
        battle.enemyTurn();
    },
    win: () => {
        const r = 3000;
        player.berries += r; player.bounty += 200000; player.points += 1;
        ui.log("Inimigo abatido! +3k Aureus", "#fbbf24");
        player.turnActive = false;
        game.save(); game.updateUI();
        battle.newEnemy();
    },
    updateBars: () => {
        document.getElementById('p-hp-bar').style.width = (player.hp/player.maxHp*100) + "%";
        document.getElementById('p-en-bar').style.width = (player.en/player.maxEn*100) + "%";
        document.getElementById('e-hp-bar').style.width = (battle.enemy.hp/battle.enemy.maxHp*100) + "%";
        document.getElementById('p-hp-txt').innerText = Math.floor(player.hp) + "/" + player.maxHp;
        document.getElementById('p-en-txt').innerText = Math.floor(player.en) + "/" + player.maxEn;
    }
};

window.onload = () => {
    if(localStorage.getItem('azure_v001_orig')) {
        document.getElementById('btn-continue').style.display = "block";
        document.getElementById('save-info').innerText = "Dados da expedição encontrados.";
    }
};
</script>
</body>
</html>
