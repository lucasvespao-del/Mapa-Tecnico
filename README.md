<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>👨‍🔧 Grupo Tecnicos do Brasil 🇧🇷</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 30px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        h1 {
            color: #1565c0; 
            margin-bottom: 30px;
            text-align: center;
            font-size: 2.5em; 
            text-transform: uppercase;
        }
        .container {
            display: flex;
            flex-wrap: wrap;
            gap: 30px;
            max-width: 1450px; 
            width: 100%;
            justify-content: center;
            align-items: flex-start;
        }
        #mapa {
            width: 900px; 
            height: 650px;
            background-color: white;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
            border-radius: 8px;
            overflow: hidden;
            cursor: pointer;
            position: relative;
        }
        .painel {
            width: 420px;
            background-color: white;
            padding: 25px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
            border-radius: 8px;
            min-height: 250px;
            max-height: 650px;
            overflow-y: auto;
        }
        .painel h2 {
            margin-top: 0;
            color: #0056b3;
            border-bottom: 2px solid #eee;
            padding-bottom: 10px;
            font-size: 1.5em;
        }
        .info {
            font-size: 1.05em; 
            margin: 8px 0;
            color: #444;
            line-height: 1.5;
        }
        .tecnico-card {
            background-color: #f9f9fc;
            border: 1px solid #e0e0e0;
            border-radius: 6px;
            padding: 15px;
            margin-bottom: 15px;
        }
        .instrucao {
            color: #888;
            font-style: italic;
            text-align: center;
            margin-top: 40px;
            font-size: 1.1em;
        }
        .sem-tecnico {
            color: #d9534f;
            font-style: italic;
        }
        .painel::-webkit-scrollbar { width: 8px; }
        .painel::-webkit-scrollbar-track { background: #f1f1f1; }
        .painel::-webkit-scrollbar-thumb { background: #ccc; border-radius: 4px; }
        
        /* Estilo para o link do WhatsApp */
        .link-zap {
            color: #25D366; /* Verde do WhatsApp */
            text-decoration: none;
            font-weight: bold;
            font-size: 1.1em;
        }
        .link-zap:hover {
            text-decoration: underline;
            color: #128C7E;
        }
    </style>
    <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
</head>
<body>

    <h1>👨‍🔧 Grupo Tecnicos do Brasil 🇧🇷</h1>

    <div class="container">
        <div id="mapa"></div>

        <div class="painel">
            <h2>Dados do Atendimento</h2>
            <div id="conteudo-painel">
                <p class="instrucao">👈 Clique em um estado no mapa para ver o(s) técnico(s) responsável(eis).</p>
            </div>
        </div>
    </div>

    <script type="text/javascript">
        // -------------------------------------------------------------
        // DADOS DOS TÉCNICOS COM TELEFONE
        // -------------------------------------------------------------
        
        // Exemplo de como vai ficar: coloque o DDD e o número normalmente
        const tecDhanillo = {
            nome: 'Dhanillo', empresa: 'Rei dos Elevadores', cidade: 'Goiânia – GO',
            telefone: '(62) 99999-9999', 
            atuacao: 'Manutenção de equipamentos em geral, foco em elevadores.'
        };
        const tecAngelo = {
            nome: 'Ângelo Marcelo', empresa: 'Elevamis soluções', cidade: 'Fortaleza - CE',
            telefone: '(85) 99999-9999',
            atuacao: 'Manutenção de equipamentos em geral, foco em elevadores.'
        };
        const tecDiego = {
            nome: 'Diego', empresa: 'CL manutenção de equipamentos automotivo', cidade: 'Curitiba - PR',
            telefone: '(41) 99999-9999',
            atuacao: 'Manutenção de equipamentos em geral, foco em elevadores e compressor.'
        };
        const tecCarlos = {
            nome: 'Carlos hack', empresa: 'Hack manutenções em elevadores', cidade: 'Rio do Sul - SC',
            telefone: '(47) 99999-9999',
            atuacao: 'Manutenção de equipamentos em geral, trabalhamos com montagem de equipamentos novos também !'
        };

        const tecnicos = {
            'BR-GO': [tecDhanillo], 'BR-MT': [tecDhanillo],
            'BR-CE': [tecAngelo], 'BR-PI': [tecAngelo], 'BR-RN': [tecAngelo],
            'BR-PR': [tecDiego, tecCarlos], 'BR-SC': [tecDiego, tecCarlos],
            'BR-RS': [tecCarlos], 'BR-SP': [tecCarlos]
        };

        const nomesEstados = {
            'BR-AC': 'Acre', 'BR-AL': 'Alagoas', 'BR-AP': 'Amapá', 'BR-AM': 'Amazonas',
            'BR-BA': 'Bahia', 'BR-CE': 'Ceará', 'BR-DF': 'Distrito Federal', 'BR-ES': 'Espírito Santo',
            'BR-GO': 'Goiás', 'BR-MA': 'Maranhão', 'BR-MT': 'Mato Grosso', 'BR-MS': 'Mato Grosso do Sul',
            'BR-MG': 'Minas Gerais', 'BR-PA': 'Pará', 'BR-PB': 'Paraíba', 'BR-PR': 'Paraná',
            'BR-PE': 'Pernambuco', 'BR-PI': 'Piauí', 'BR-RJ': 'Rio de Janeiro', 'BR-RN': 'Rio Grande do Norte',
            'BR-RS': 'Rio Grande do Sul', 'BR-RO': 'Rondônia', 'BR-RR': 'Roraima', 'BR-SC': 'Santa Catarina',
            'BR-SP': 'São Paulo', 'BR-SE': 'Sergipe', 'BR-TO': 'Tocantins'
        };

        google.charts.load('current', { 'packages':['geochart'] });
        google.charts.setOnLoadCallback(desenharMapa);

        function desenharMapa() {
            var mapData = [
                ['Estado', 'Região', {type: 'string', role: 'tooltip'}]
            ];

            var regioes = {
                'BR-AC': 1, 'BR-AL': 2, 'BR-AP': 1, 'BR-AM': 1,
                'BR-BA': 2, 'BR-CE': 2, 'BR-DF': 3, 'BR-ES': 4,
                'BR-GO': 3, 'BR-MA': 2, 'BR-MT': 3, 'BR-MS': 3,
                'BR-MG': 4, 'BR-PA': 1, 'BR-PB': 2, 'BR-PR': 5,
                'BR-PE': 2, 'BR-PI': 2, 'BR-RJ': 4, 'BR-RN': 2,
                'BR-RS': 5, 'BR-RO': 1, 'BR-RR': 1, 'BR-SC': 5,
                'BR-SP': 4, 'BR-SE': 2, 'BR-TO': 1
            };

            for (var codigo in regioes) {
                var nome = nomesEstados[codigo];
                var qtd = tecnicos[codigo] ? tecnicos[codigo].length : 0;
                var textoTooltip = nome;
                
                if (qtd > 0) {
                    textoTooltip += ' (' + qtd + ' técnico' + (qtd > 1 ? 's' : '') + ')';
                } else {
                    textoTooltip += ' (Sem técnicos)';
                }

                mapData.push([ {v: codigo, f: nome}, regioes[codigo], textoTooltip ]);
            }

            var data = google.visualization.arrayToDataTable(mapData);

            var options = {
                region: 'BR', 
                displayMode: 'regions',
                resolution: 'provinces', 
                colorAxis: {colors: ['#2e7d32', '#1565c0', '#f9a825', '#c62828', '#6a1b9a']},
                backgroundColor: '#ffffff',
                datalessRegionColor: '#eeeeee',
                defaultColor: '#f5f5f5'
            };

            var chart = new google.visualization.GeoChart(document.getElementById('mapa'));

            google.visualization.events.addListener(chart, 'select', function() {
                var selection = chart.getSelection();
                if (selection.length > 0) {
                    var codigoEstado = data.getValue(selection[0].row, 0); 
                    var nomeDoEstado = nomesEstados[codigoEstado];
                    var listaTecnicos = tecnicos[codigoEstado];
                    
                    var htmlConteudo = `<h3 style="color: #333; margin-top: 0; margin-bottom: 15px;">📍 Estado: ${nomeDoEstado}</h3>`;

                    if (listaTecnicos && listaTecnicos.length > 0) {
                        listaTecnicos.forEach(function(tec) {
                            
                            // Lógica para criar o link do WhatsApp limpando os caracteres
                            var numeroLimpo = tec.telefone.replace(/\D/g, ''); // Tira tudo que não for número
                            if(numeroLimpo !== '' && !numeroLimpo.startsWith('55')) {
                                numeroLimpo = '55' + numeroLimpo; // Adiciona o código do Brasil se não tiver
                            }
                            var linkWhatsapp = `https://wa.me/${numeroLimpo}`;

                            htmlConteudo += `
                            <div class="tecnico-card">
                                <p class="info"><strong>👨‍🔧 Nome:</strong> ${tec.nome}</p>
                                <p class="info"><strong>🏢 Empresa:</strong> ${tec.empresa}</p>
                                <p class="info"><strong>📍 Base:</strong> ${tec.cidade}</p>
                                <p class="info"><strong>📞 Contato:</strong> <a href="${linkWhatsapp}" target="_blank" class="link-zap">${tec.telefone}</a></p>
                                <p class="info"><strong>🔧 Atuação:</strong> ${tec.atuacao}</p>
                            </div>
                            `;
                        });
                    } else {
                        htmlConteudo += `<p class="info sem-tecnico">Ainda não há técnico cadastrado para esta região.</p>`;
                    }

                    document.getElementById('conteudo-painel').innerHTML = htmlConteudo;
                }
            });

            chart.draw(data, options);
        }
    </script>
</body>
</html>
