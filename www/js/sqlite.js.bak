// ---------- SQLite inicijalizacija ----------
let db;
document.addEventListener('deviceready', () => {
    db = window.sqlitePlugin.openDatabase({name: 'kandz.db', location: 'default'});

    db.transaction(tx => {
        tx.executeSql(`CREATE TABLE IF NOT EXISTS rezultati (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            domacin TEXT,
            gost TEXT,
            total TEXT,
            twoH TEXT,
            oneH TEXT
        )`);

        tx.executeSql(`CREATE TABLE IF NOT EXISTS statistika (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            tim TEXT,
            odigrano INTEGER,
            golovi_dati INTEGER,
            golovi_primljeni INTEGER,
            GG INTEGER,
            twoPlus INTEGER,
            pctGG INTEGER,
            pct2Plus INTEGER,
            NG INTEGER,
            pctNG INTEGER
        )`);

        tx.executeSql(`CREATE TABLE IF NOT EXISTS future (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            domacin TEXT,
            gost TEXT
        )`);
    }, err => console.error('DB Error: ', err), () => loadAllData());
});

// ---------- Load podaci ----------
function loadAllData(){
    loadResults();
    loadFuture();
}

function loadResults(){
    db.transaction(tx=>{
        tx.executeSql('SELECT * FROM rezultati',[],(tx,res)=>{
            const tbody=document.querySelector('#resultsTable tbody');
            tbody.innerHTML='';
            for(let i=0;i<res.rows.length;i++){
                const r=res.rows.item(i);
                addRowFromData('resultsTable',[r.domacin,r.gost,r.total,r.twoH,r.oneH]);
            }
        });
    });
}

function loadFuture(){
    db.transaction(tx=>{
        tx.executeSql('SELECT * FROM future',[],(tx,res)=>{
            const tbody=document.querySelector('#futureTable tbody');
            tbody.innerHTML='';
            for(let i=0;i<res.rows.length;i++){
                const r=res.rows.item(i);
                addRowFromData('futureTable',[r.domacin,r.gost]);
            }
        });
    });
}

// ---------- Dodavanje redova ----------
function addRowFromData(tableId, values){
    const tbody=document.getElementById(tableId).querySelector('tbody');
    const row=document.createElement('tr');

    values.forEach(val=>{
        const td=document.createElement('td');
        const input=document.createElement('input');
        input.value=val;
        td.appendChild(input);
        row.appendChild(td);
    });

    const delTd=document.createElement('td');
    const delBtn=document.createElement('span');
    delBtn.innerText='X';
    delBtn.className='row-btn';
    delBtn.onclick=()=>{
        if(tableId==='resultsTable') deleteResultRow(row);
        if(tableId==='futureTable') deleteFutureRow(row);
        row.remove();
    };
    delTd.appendChild(delBtn);
    row.appendChild(delTd);
    tbody.appendChild(row);

    // Automatski save
    if(tableId==='resultsTable'){
        const inputs=row.querySelectorAll('input');
        inputs.forEach(input=>{
            input.addEventListener('change',()=>{
                saveResultRow(inputs[0].value,inputs[1].value,inputs[2].value,inputs[3].value,inputs[4].value);
            });
        });
    }

    if(tableId==='futureTable'){
        const inputs=row.querySelectorAll('input');
        inputs.forEach(input=>{
            input.addEventListener('change',()=>{
                saveFutureRow(inputs[0].value,inputs[1].value);
            });
        });
    }
}

// ---------- Brisanje ----------
function deleteResultRow(row){
    const tds=row.querySelectorAll('td input');
    db.transaction(tx=>{
        tx.executeSql('DELETE FROM rezultati WHERE domacin=? AND gost=? AND total=? LIMIT 1',
            [tds[0].value,tds[1].value,tds[2].value]);
    });
}

function deleteFutureRow(row){
    const tds=row.querySelectorAll('td input');
    db.transaction(tx=>{
        tx.executeSql('DELETE FROM future WHERE domacin=? AND gost=? LIMIT 1',
            [tds[0].value,tds[1].value]);
    });
}

// ---------- Trajno čuvanje ----------
function saveResultRow(domacin, gost, total, twoH, oneH){
    db.transaction(tx=>{
        tx.executeSql("SELECT id FROM rezultati WHERE domacin=? AND gost=? AND total=?", [domacin, gost, total], (tx,res)=>{
            if(res.rows.length > 0){
                const id = res.rows.item(0).id;
                tx.executeSql("UPDATE rezultati SET domacin=?, gost=?, total=?, twoH=?, oneH=? WHERE id=?", [domacin, gost, total, twoH, oneH, id]);
            } else {
                tx.executeSql("INSERT INTO rezultati (domacin, gost, total, twoH, oneH) VALUES (?,?,?,?,?)", [domacin, gost, total, twoH, oneH]);
            }
        });
    });
}
                tx.executeSql("INSERT INTO rezultati (domacin, gost, total, twoH, oneH) VALUES (?,?,?,?,?)", [domacin, gost, total, twoH, oneH]);
            }
        });
    });
}

function addRowFromData(tableId, values){
    const tbody=document.getElementById(tableId).querySelector("tbody");
    const row=document.createElement("tr");
    values.forEach(val=>{
        const td=document.createElement("td");
        const input=document.createElement("input");
        input.value=val;
        td.appendChild(input);
        row.appendChild(td);
    });
    const delTd=document.createElement("td");
    const delBtn=document.createElement("span");
    delBtn.innerText="X";
    delBtn.className="row-btn";
    delBtn.onclick=()=>{
        if(tableId==="resultsTable") deleteResultRow(row);
        row.remove();
    };
    delTd.appendChild(delBtn);
    row.appendChild(delTd);
    tbody.appendChild(row);
    if(tableId==="resultsTable"){
        const inputs=row.querySelectorAll("input");
        saveResultRow(inputs[0].value, inputs[1].value, inputs[2].value, inputs[3].value, inputs[4].value);
        inputs.forEach(input=>{
            input.addEventListener("change",()=>{
                saveResultRow(inputs[0].value, inputs[1].value, inputs[2].value, inputs[3].value, inputs[4].value);
            });
        });
    }
}
            }
        );
    });
}
            }

            const tbody=document.getElementById('predictionTable').querySelector('tbody');
            tbody.innerHTML='';

            future.forEach(r=>{
                const dom=r.querySelector('td:nth-child(1) input').value.trim();
                const gost=r.querySelector('td:nth-child(2) input').value.trim();
                const domStats = stats[dom] || {gg:0,two:0,ng:0};
                const gostStats = stats[gost] || {gg:0,two:0,ng:0};

                const ggPct = Math.round(domStats.gg*0.6 + gostStats.gg*0.4);
                const twoPct = Math.round(domStats.two*0.6 + gostStats.two*0.4);
                const ngPct = Math.round(domStats.ng*0.6 + gostStats.ng*0.4);

                const row=document.createElement('tr');
                row.innerHTML=`<td>${dom}</td><td>${gost}</td><td>${ggPct}%</td><td>${ngPct}%</td><td>${twoPct}%</td>`;
                tbody.appendChild(row);
            });
        });
    });
}
