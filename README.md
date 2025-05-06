<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora Penal Copacabana RP</title>
    <style>
        body {
            font-family: sans-serif;
            margin: 20px;
            background-color: #f4f4f4;
        }
        .container {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        #calcularTotal {
            background-color: #007bff;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        #calcularTotal:hover {
            background-color: #0056b3;
        }
        #resultadoTotal {
            margin-top: 20px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 4px;
            background-color: #f9f9f9;
        }
        #resultadoTotal h2 {
            color: #333;
            margin-top: 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Calculadora Penal Copacabana RP</h1>
        <table>
            <thead>
                <tr>
                    <th>Selecionar</th>
                    <th>Crime</th>
                    <th>Pena (min)</th>
                    <th>Multa</th>
                    <th>Fiança</th>
                </tr>
            </thead>
            <tbody id="crimesTableBody">
                </tbody>
        </table>
        <button id="calcularTotal" onclick="calcularTotal()">Calcular Pena Total</button>
        <div id="resultadoTotal" style="display: none;">
            <h2>Resultado Total:</h2>
            <p id="penaTotalResultado"></p>
            <p id="multaTotalResultado"></p>
            <p id="fiancaTotalResultado"></p>
        </div>
    </div>

    <script>
        const crimesData = {
            "Art. 1.1 - Homicídio Doloso Qualificado": { meses: 120, multa: 50000, fianca: 150000 },
            "Art. 1.2 - Homicídio Doloso": { meses: 90, multa: 40000, fianca: 120000 },
            "Art. 1.3 - Tentativa de Homicídio": { meses: 60, multa: 30000, fianca: 90000 },
            "Art. 1.4 - Homicídio Culposo": { meses: 30, multa: 15000, fianca: 45000 },
            "Art. 2.1 - Lesão Corporal": { meses: 15, multa: 5000, fianca: 15000 },
            "Art. 2.2 - Sequestro": { meses: 75, multa: 35000, fianca: 105000 },
            "Art. 2.3 - Cárcere Privado": { meses: 60, multa: 30000, fianca: 90000 },
            "Art. 3.1 - Desmanche de Veículos": { meses: 45, multa: 20000, fianca: 60000 },
            "Art. 3.2 - Furto": { meses: 10, multa: 3000, fianca: 9000 },
            "Art. 3.3 - Receptação de Veículos": { meses: 30, multa: 15000, fianca: 45000 },
            "Art. 3.4 - Roubo de Veículos": { meses: 60, multa: 25000, fianca: 75000 },
            "Art. 3.5 - Furto de Veículos": { meses: 20, multa: 10000, fianca: 30000 },
            "Art. 4.1 - Roubo": { meses: 40, multa: 18000, fianca: 54000 },
            "Art. 4.2 - Furto a caixa eletrônico": { meses: 70, multa: 32000, fianca: 96000 },
            "Art. 4.3 - Extorsão": { meses: 50, multa: 22000, fianca: 66000 },
            "Art. 5.1 - Posse de peças de armas": { meses: 5, multa: 1000, fianca: 3000 },
            "Art. 5.2 - Posse de Capsula": { meses: 3, multa: 500, fianca: 1500 },
            "Art. 5.3 - Tráfico de Armas": { meses: 90, multa: 45000, fianca: 135000 },
            "Art. 5.4 - Porte de Arma Pesada": { meses: 60, multa: 28000, fianca: 84000 },
            "Art. 5.5 - Porte de Arma Leve": { meses: 30, multa: 12000, fianca: 36000 },
            "Art. 5.6 - Disparo de Arma": { meses: 25, multa: 11000, fianca: 33000 },
            "Art. 5.7 - Tráfico de Munições (+100)": { meses: 40, multa: 16000, fianca: 48000 },
            "Art. 5.8 - Porte de Munição (-100)": { meses: 10, multa: 2000, fianca: 6000 },
            "Art. 5.9 - Posse de Colete": { meses: 15, multa: 6000, fianca: 18000 },
            "Art. 5.10 - Posse de Arma Branca": { meses: 5, multa: 750, fianca: 2250 },
            "Art. 5.11 - Tráfico de drogas (+100)": { meses: 80, multa: 40000, fianca: 120000 },
            "Art. 5.12 - Aviãozinho (6 a 100)": { meses: 35, multa: 15000, fianca: 45000 },
            "Art. 5.13 - Posse de Componentes Narcóticos": { meses: 20, multa: 9000, fianca: 27000 },
            "Art. 5.14 - Posse de drogas (1 a 5)": { meses: 10, multa: 4000, fianca: 12000 },
            "Art. 5.15 - Dinheiro sujo": { meses: 20, multa: 12000, fianca: 36000 },
            "Art. 5.16 - Contrabando": { meses: 25, multa: 13000, fianca: 39000 },
            "Art. 5.17 - Descaminho": { meses: 15, multa: 8000, fianca: 24000 },
            "CL - Combat Logging": { meses: 5, multa: 1000, fianca: 3000 },
            "Art. 6.1 - Falsidade ideológica": { meses: 30, multa: 14000, fianca: 42000 },
            "Art. 6.2 - Formação de quadrilha": { meses: 60, multa: 30000, fianca: 90000 },
            "Art. 6.3 - Apologia ao crime": { meses: 10, multa: 5000, fianca: 15000 },
            "Art. 6.4 - Posse de arma em público": { meses: 20, multa: 9000, fianca: 27000 },
            "Art. 6.5 - Suborno": { meses: 45, multa: 25000, fianca: 75000 },
            "Art. 6.6 - Ameaça": { meses: 10, multa: 4000, fianca: 12000 },
            "Art. 6.7 - Falsa comunicação de crime": { meses: 20, multa: 10000, fianca: 30000 },
            "Art. 6.8 - Uso indevido de 190/192": { meses: 5, multa: 2000, fianca: 6000 },
            "Art. 6.10 - Posse de itens ilegais": { meses: 15, multa: 7000, fianca: 21000 },
            "Art. 6.11 - Assédio moral": { meses: 10, multa: 3000, fianca: 9000 },
            "Art. 6.12 - Atentado ao pudor": { meses: 40, multa: 18000, fianca: 54000 },
            "Art. 6.13 - Vandalismo": { meses: 5, multa: 2500, fianca: 7500 },
            "Art. 6.14 - Dano ao Patrimonio Público": { meses: 15, multa: 8000, fianca: 24000 },
            "Art. 6.15 - Invasão de propriedade": { meses: 10, multa: 4500, fianca: 13500 },
            "Art. 6.16 - Abuso de autoridade": { meses: 30, multa: 15000, fianca: 45000 },
            "Art. 6.17 - Uso de máscara": { meses: 2, multa: 500, fianca: 1500 },
            "Art. 6.18 - Uso de equipamentos restritos": { meses: 20, multa: 11000, fianca: 33000 },
            "Art. 6.19 - Omissão de socorro": { meses: 15, multa: 6500, fianca: 19500 },
            "Art. 6.20 - Tentativa de fuga": { meses: 10, multa: 3500, fianca: 10500 },
            "Art. 6.21 - Desacato 1": { meses: 3, multa: 1000, fianca: 3000 },
            "Art. 6.22 - Desacato 2": { meses: 5, multa: 1500, fianca: 4500 },
            "Art. 6.23 - Desacato 3": { meses: 7, multa: 2000, fianca: 6000 },
            "Art. 6.24 - Resistência a prisão": { meses: 10, multa: 4000, fianca: 12000 },
            "Art. 6.25 - Réu Reincidente": { meses: 20, multa: 10000, fianca: 30000 },
            "Art. 6.26 - Cúmplice": { meses: 0, multa: 0, fianca: 0, isCumplice: true }, // Precisa de lógica específica
            "Art. 6.27 - Obstrução à Justiça": { meses: 25, multa: 12000, fianca: 36000 },
            "Art. 6.28 - Ocultação de Provas": { meses: 20, multa: 9500, fianca: 28500 },
            "Art. 6.29 - Zaralho em recrutamento policial": { meses: 5, multa: 1500, fianca: 4500 },
            "Art. 6.30 - Prisão Militar": { meses: 0, multa: 0, fianca: 0, isMilitar: true }, // Lógica específica
            "Art. 6.31 - Prevaricação": { meses: 30, multa: 16000, fianca: 48000 },
            "Art. 6.32 - Invasão de Departamento Policial": { meses: 40, multa: 20000, fianca: 60000 },
            "Art. 6.33 - Vadiagem": { meses: 2, multa: 250, fianca: 750 },
            "Art. 6.34 - Desobediência": { meses: 3, multa: 750, fianca: 2250 },
            "Art. 7.1 - Condução imprudente": { meses: 2, multa: 1000, fianca: 3000 },
            "Art. 7.2 - Dirigir na contra mão": { meses: 3, multa: 1500, fianca: 4500 },
            "Art. 7.3 - Alta velocidade": { meses: 5, multa: 2000, fianca: 6000 },
            "Art. 7.4 - Poluição sonora": { meses: 2, multa: 800, fianca: 2400 },
            "Art. 7.5 - Corridas Ilegais": { meses: 15, multa: 7000, fianca: 21000 },
            "Art. 7.6 - Uso excessivo de insulfilm": { meses: 1, multa: 300, fianca: 900 },
            "Art. 7.7 - Veículo muito danificado": { meses: 1, multa: 500, fianca: 1500 },
            "Art. 7.8 - Veículo Ilegalmente estacionado": { meses: 1, multa: 250, fianca: 750 },
            "Art. 7.11 - Impedir o fluxo do tráfego": { meses: 5, multa: 2500, fianca: 7500 },
            "Art. 7.12 - Colisão Proposital em viatura policial": { meses: 30, multa: 15000, fianca: 45000 }
        };

        const crimesTableBody = document.getElementById("crimesTableBody");
        const penaTotalResultado = document.getElementById("penaTotalResultado");
        const multaTotalResultado = document.getElementById("multaTotalResultado");
        const fiancaTotalResultado = document.getElementById("fiancaTotalResultado");
        const resultadoTotalDiv = document.getElementById("resultadoTotal");

        // Preenche a tabela de crimes
        for (const crime in crimesData) {
            const row = crimesTableBody.insertRow();
            const selectCell = row.insertCell();
            const crimeCell = row.insertCell();
            const penaCell = row.insertCell();
            const multaCell = row.insertCell();
            const fiancaCell = row.insertCell();

            const checkbox = document.createElement("input");
            checkbox.type = "checkbox"; // Correção aqui
            checkbox.value = crime;
            selectCell.appendChild(checkbox);

            crimeCell.textContent = crime;
            penaCell.textContent = crimesData[crime].meses || 0;
            multaCell.textContent = crimesData[crime].multa || 0;
            fiancaCell.textContent = crimesData[crime].fianca || 0; // Adiciona o valor da fiança

        }

        function calcularTotal() {
            let totalPena = 0;
            let totalMulta = 0;
            let totalFianca = 0;
            const checkboxes = document.querySelectorAll("#crimesTableBody input[type='checkbox']:checked");


            checkboxes.forEach(checkbox => {
                const crimeNome = checkbox.value;
                const crimeData = crimesData[crimeNome];
                if (crimeData) {
                    totalPena += crimeData.meses || 0;
                    totalMulta += crimeData.multa || 0;
                    totalFianca += crimeData.fianca || 0;
                }
            });

            penaTotalResultado.textContent = `Pena Total: ${totalPena} minutos`;
            multaTotalResultado.textContent = `Multa Total: $${totalMulta}`;
            fiancaTotalResultado.textContent = `Fiança Total: $${totalFianca}`;
            resultadoTotalDiv.style.display = "block";
        }
    </script>
</body>
</html>
