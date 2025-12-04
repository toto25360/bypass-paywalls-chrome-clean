<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Organisation Nouvel An</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            position: relative;
            overflow: hidden;
        }

        .container::before {
            content: '❄️';
            position: absolute;
            top: -20px;
            right: -20px;
            font-size: 150px;
            opacity: 0.1;
        }

        h1 {
            text-align: center;
            color: #4a5568;
            margin-bottom: 10px;
            font-size: 2.5em;
        }

        .subtitle {
            text-align: center;
            color: #718096;
            margin-bottom: 30px;
            font-size: 1.2em;
        }

        .add-btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            margin-bottom: 20px;
            transition: transform 0.2s;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
        }

        .add-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(102, 126, 234, 0.6);
        }

        table {
            width: 100%;
            border-collapse: separate;
            border-spacing: 0;
            margin-top: 20px;
            overflow: hidden;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        thead {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        th {
            padding: 15px;
            text-align: left;
            font-weight: 600;
            font-size: 16px;
        }

        tbody tr {
            background: white;
            transition: background 0.3s;
        }

        tbody tr:nth-child(even) {
            background: #f7fafc;
        }

        tbody tr:hover {
            background: #edf2f7;
        }

        td {
            padding: 15px;
            border-bottom: 1px solid #e2e8f0;
        }

        input, select {
            width: 100%;
            padding: 8px;
            border: 2px solid #e2e8f0;
            border-radius: 8px;
            font-size: 14px;
            transition: border-color 0.3s;
        }

        input:focus, select:focus {
            outline: none;
            border-color: #667eea;
        }

        .category-salée {
            background: #fed7d7;
            color: #742a2a;
            padding: 5px 15px;
            border-radius: 20px;
            display: inline-block;
            font-weight: 600;
        }

        .category-sucrée {
            background: #feebc8;
            color: #7c2d12;
            padding: 5px 15px;
            border-radius: 20px;
            display: inline-block;
            font-weight: 600;
        }

        .delete-btn {
            background: #fc8181;
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 8px;
            cursor: pointer;
            transition: background 0.3s;
        }

        .delete-btn:hover {
            background: #f56565;
        }

        .export-btn {
            background: #48bb78;
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            margin-left: 10px;
            transition: transform 0.2s;
            box-shadow: 0 4px 15px rgba(72, 187, 120, 0.4);
        }

        .export-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(72, 187, 120, 0.6);
        }

        .snowflake {
            position: fixed;
            top: -10px;
            color: white;
            font-size: 20px;
            animation: fall linear infinite;
            z-index: 1;
            pointer-events: none;
        }

        @keyframes fall {
            to {
                transform: translateY(100vh);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎊 Réveillon du Nouvel An 2025 ❄️</h1>
        <p class="subtitle">Qui apporte quoi ?</p>
        
        <div>
            <button class="add-btn" onclick="addRow()">➕ Ajouter une personne</button>
            <button class="export-btn" onclick="exportToCSV()">📥 Télécharger CSV</button>
        </div>

        <table id="menuTable">
            <thead>
                <tr>
                    <th>Nom</th>
                    <th>Catégorie</th>
                    <th>Plat apporté</th>
                    <th>Remarques</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody id="tableBody">
                <tr>
                    <td><input type="text" placeholder="Prénom" value=""></td>
                    <td>
                        <select onchange="updateCategory(this)">
                            <option value="">Choisir...</option>
                            <option value="Salée">🧀 Salée</option>
                            <option value="Sucrée">🍰 Sucrée</option>
                        </select>
                    </td>
                    <td><input type="text" placeholder="Ex: Quiche lorraine"></td>
                    <td><input type="text" placeholder="Allergies, quantité..."></td>
                    <td><button class="delete-btn" onclick="deleteRow(this)">🗑️</button></td>
                </tr>
            </tbody>
        </table>
    </div>

    <script>
        // Effet de neige
        function createSnowflake() {
            const snowflake = document.createElement('div');
            snowflake.classList.add('snowflake');
            snowflake.textContent = '❄';
            snowflake.style.left = Math.random() * window.innerWidth + 'px';
            snowflake.style.animationDuration = Math.random() * 3 + 2 + 's';
            snowflake.style.opacity = Math.random();
            snowflake.style.fontSize = Math.random() * 10 + 10 + 'px';
            
            document.body.appendChild(snowflake);
            
            setTimeout(() => {
                snowflake.remove();
            }, 5000);
        }

        setInterval(createSnowflake, 300);

        function addRow() {
            const tbody = document.getElementById('tableBody');
            const newRow = tbody.rows[0].cloneNode(true);
            
            // Réinitialiser les valeurs
            newRow.querySelectorAll('input').forEach(input => input.value = '');
            newRow.querySelector('select').selectedIndex = 0;
            
            tbody.appendChild(newRow);
        }

        function deleteRow(btn) {
            const tbody = document.getElementById('tableBody');
            if (tbody.rows.length > 1) {
                btn.closest('tr').remove();
            } else {
                alert('Il faut garder au moins une ligne !');
            }
        }

        function updateCategory(select) {
            const value = select.value;
            if (value) {
                select.className = 'category-' + value.toLowerCase();
            }
        }

        function exportToCSV() {
            const table = document.getElementById('menuTable');
            let csv = '\uFEFF'; // UTF-8 BOM pour Excel
            
            // En-têtes
            csv += 'Nom;Catégorie;Plat apporté;Remarques\n';
            
            // Données
            const rows = table.querySelectorAll('tbody tr');
            rows.forEach(row => {
                const inputs = row.querySelectorAll('input');
                const select = row.querySelector('select');
                
                const nom = inputs[0].value || '';
                const categorie = select.value || '';
                const plat = inputs[1].value || '';
                const remarques = inputs[2].value || '';
                
                csv += `${nom};${categorie};${plat};${remarques}\n`;
            });
            
            // Créer un lien de téléchargement
            const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
            const url = window.URL.createObjectURL(blob);
            const link = document.createElement('a');
            link.setAttribute('href', url);
            link.setAttribute('download', 'reveillon_nouvel_an_2025.csv');
            link.style.visibility = 'hidden';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }
    </script>
</body>
</html>
