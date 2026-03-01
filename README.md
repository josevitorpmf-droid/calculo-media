<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculador de Média Escolar</title>
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
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 15px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            padding: 40px;
            max-width: 500px;
            width: 100%;
        }

        h1 {
            color: #667eea;
            text-align: center;
            margin-bottom: 30px;
            font-size: 28px;
        }

        .form-group {
            margin-bottom: 25px;
        }

        label {
            display: block;
            color: #333;
            font-weight: 600;
            margin-bottom: 8px;
            font-size: 16px;
        }

        input[type="number"], select {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        input[type="number"]:focus, select:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .radio-group {
            display: flex;
            gap: 20px;
            margin-top: 10px;
        }

        .radio-option {
            display: flex;
            align-items: center;
            gap: 8px;
        }

        input[type="radio"] {
            cursor: pointer;
            width: 18px;
            height: 18px;
            accent-color: #667eea;
        }

        .radio-option label {
            margin: 0;
            font-weight: 500;
            cursor: pointer;
        }

        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 30px;
        }

        button {
            flex: 1;
            padding: 14px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }

        .btn-calcular {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-calcular:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(102, 126, 234, 0.4);
        }

        .btn-limpar {
            background: #f0f0f0;
            color: #333;
        }

        .btn-limpar:hover {
            background: #e0e0e0;
        }

        .resultado {
            margin-top: 30px;
            padding: 20px;
            border-radius: 8px;
            display: none;
            animation: slideIn 0.3s ease-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(-10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .resultado.aprovado {
            background: #d4edda;
            border: 2px solid #28a745;
            color: #155724;
        }

        .resultado.reprovado {
            background: #f8d7da;
            border: 2px solid #dc3545;
            color: #721c24;
        }

        .resultado.neutro {
            background: #cfe2ff;
            border: 2px solid #0d6efd;
            color: #084298;
        }

        .resultado h2 {
            margin-bottom: 10px;
            font-size: 20px;
        }

        .resultado p {
            font-size: 16px;
            line-height: 1.6;
            margin-bottom: 8px;
        }

        .resultado p:last-child {
            margin-bottom: 0;
        }

        .icon {
            font-size: 24px;
            margin-right: 8px;
        }

        .campo-simulado {
            display: none;
        }

        .campo-simulado.ativo {
            display: block;
        }

        .info-box {
            background: #f8f9fa;
            border-left: 4px solid #667eea;
            padding: 12px;
            margin-top: 20px;
            border-radius: 4px;
            font-size: 14px;
            color: #555;
        }

        @media (max-width: 480px) {
            .container {
                padding: 25px;
            }

            h1 {
                font-size: 24px;
            }

            .button-group {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📊 Calculador de Média</h1>

        <form id="formMedia">
            <!-- Nota da Parcial -->
            <div class="form-group">
                <label for="notaParcial">📝 Nota da Parcial (0-10)</label>
                <input 
                    type="number" 
                    id="notaParcial" 
                    min="0" 
                    max="10" 
                    step="0.1" 
                    placeholder="Digite sua nota"
                    required
                >
            </div>

            <!-- Nota Bimestral -->
            <div class="form-group">
                <label for="notaBimestral">📋 Nota Bimestral (0-10)</label>
                <input 
                    type="number" 
                    id="notaBimestral" 
                    min="0" 
                    max="10" 
                    step="0.1" 
                    placeholder="Digite sua nota"
                    required
                >
            </div>

            <!-- Simulado -->
            <div class="form-group">
                <label>🎯 Você tem a nota do simulado?</label>
                <div class="radio-group">
                    <div class="radio-option">
                        <input 
                            type="radio" 
                            id="temSim" 
                            name="temSimulado" 
                            value="sim"
                            onchange="toggleSimulado()"
                        >
                        <label for="temSim">Sim</label>
                    </div>
                    <div class="radio-option">
                        <input 
                            type="radio" 
                            id="temNao" 
                            name="temSimulado" 
                            value="nao"
                            onchange="toggleSimulado()"
                            checked
                        >
                        <label for="temNao">Não</label>
                    </div>
                </div>
            </div>

            <!-- Campo Nota Simulado -->
            <div class="form-group campo-simulado" id="campoSimulado">
                <label for="notaSimulado">📌 Nota do Simulado (0-10)</label>
                <input 
                    type="number" 
                    id="notaSimulado" 
                    min="0" 
                    max="10" 
                    step="0.1" 
                    placeholder="Digite sua nota"
                >
            </div>

            <!-- Botões -->
            <div class="button-group">
                <button type="button" class="btn-calcular" onclick="calcularMedia()">
                    ✓ Calcular
                </button>
                <button type="button" class="btn-limpar" onclick="limparFormulario()">
                    🔄 Limpar
                </button>
            </div>

            <!-- Info -->
            <div class="info-box">
                <strong>💡 Pesos:</strong><br>
                Parcial: 35% | Bimestral: 45% | Simulado: 20%<br>
                <strong>Aprovação:</strong> Média ≥ 6.0
            </div>
        </form>

        <!-- Resultado -->
        <div id="resultado" class="resultado"></div>
    </div>

    <script>
        function toggleSimulado() {
            const temSim = document.getElementById('temSim').checked;
            const campoSimulado = document.getElementById('campoSimulado');
            
            if (temSim) {
                campoSimulado.classList.add('ativo');
                document.getElementById('notaSimulado').required = true;
            } else {
                campoSimulado.classList.remove('ativo');
                document.getElementById('notaSimulado').required = false;
                document.getElementById('notaSimulado').value = '';
            }
        }

        function calcularMedia() {
            // Pega os valores
            const notaParcial = parseFloat(document.getElementById('notaParcial').value);
            const notaBimestral = parseFloat(document.getElementById('notaBimestral').value);
            const temSim = document.getElementById('temSim').checked;
            const notaSimulado = parseFloat(document.getElementById('notaSimulado').value);

            // Valida campos obrigatórios
            if (!notaParcial || !notaBimestral) {
                alert('⚠️ Por favor, preencha as notas da parcial e bimestral!');
                return;
            }

            if (temSim && !notaSimulado) {
                alert('⚠️ Por favor, preencha a nota do simulado!');
                return;
            }

            const resultado = document.getElementById('resultado');

            if (temSim) {
                // Calcular média normal
                const notaParcialPonderada = notaParcial * 0.35;
                const notaBimestralPonderada = notaBimestral * 0.45;
                const notaSimuladoPonderada = notaSimulado * 0.20;

                const media = (notaParcialPonderada + notaBimestralPonderada + notaSimuladoPonderada).toFixed(2);

                if (media < 6) {
                    resultado.className = 'resultado reprovado';
                    resultado.innerHTML = `
                        <h2>❌ RECUPERAÇÃO</h2>
                        <p><strong>Média Final:</strong> ${media}</p>
                        <p>Infelizmente você ficou de recuperação. Estude bastante!</p>
                    `;
                } else {
                    resultado.className = 'resultado aprovado';
                    resultado.innerHTML = `
                        <h2>✓ APROVADO</h2>
                        <p><strong>Média Final:</strong> ${media}</p>
                        <p>Parabéns! Você foi aprovado! 🎉</p>
                    `;
                }
            } else {
                // Calcular nota necessária no simulado
                const notaParcialPonderada = notaParcial * 0.35;
                const notaBimestralPonderada = notaBimestral * 0.45;
                const somaAtual = notaParcialPonderada + notaBimestralPonderada;
                
                const notaNecessaria = ((6 - somaAtual) / 0.20).toFixed(2);

                resultado.className = 'resultado neutro';

                if (notaNecessaria > 10) {
                    resultado.innerHTML = `
                        <h2>⚠️ ATENÇÃO!</h2>
                        <p><strong>Nota necessária no simulado:</strong> ${notaNecessaria}</p>
                        <p>❌ Infelizmente, mesmo com 10 no simulado, você ficará de recuperação.</p>
                        <p>Sua soma atual é ${somaAtual.toFixed(2)}, precisaria de ${(6 - somaAtual).toFixed(2)} pontos dos 20% do simulado.</p>
                    `;
                } else if (notaNecessaria < 0) {
                    resultado.innerHTML = `
                        <h2>✓ BOM SINAL!</h2>
                        <p>Sua média já está garantida! Qualquer nota no simulado mantém você aprovado.</p>
                        <p><strong>Sua soma atual:</strong> ${somaAtual.toFixed(2)} pontos</p>
                    `;
                } else {
                    resultado.innerHTML = `
                        <h2>🎯 OBJETIVO</h2>
                        <p>Para atingir média 6.0:</p>
                        <p><strong>Você precisa tirar NO MÍNIMO ${notaNecessaria} no simulado</strong></p>
                        <p>Sua soma atual é ${somaAtual.toFixed(2)}, faltam ${(6 - somaAtual).toFixed(2)} pontos dos 20% do simulado.</p>
                    `;
                }
            }

            resultado.style.display = 'block';
        }

        function limparFormulario() {
            document.getElementById('formMedia').reset();
            document.getElementById('resultado').style.display = 'none';
            document.getElementById('temNao').checked = true;
            toggleSimulado();
        }

        // Permite calcular com Enter
        document.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                calcularMedia();
            }
        });
    </script>
</body>
</html>
