# security.github.io
teste

<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', sans-serif;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .calculadora {
            background: #1e1e2e;
            padding: 20px;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4);
            width: 320px;
        }

        .display {
            background: #0f0f1a;
            color: #00ff9d;
            font-size: 2.5rem;
            text-align: right;
            padding: 20px;
            border-radius: 12px;
            margin-bottom: 20px;
            min-height: 80px;
            overflow: hidden;
            box-shadow: inset 0 4px 10px rgba(0, 0, 0, 0.5);
        }

        .botoes {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
        }

        button {
            font-size: 1.4rem;
            height: 70px;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.2s;
            font-weight: 600;
        }

        button:active {
            transform: scale(0.95);
        }

        .numero {
            background: #2d2d44;
            color: #fff;
        }

        .operador {
            background: #ff9500;
            color: white;
        }

        .igual {
            background: #00d4a5;
            color: white;
            grid-column: span 2;
        }

        .clear {
            background: #ff4757;
            color: white;
        }

        button:hover {
            filter: brightness(1.1);
        }
    </style>
</head>
<body>
    <div class="calculadora">
        <div class="display" id="display">0</div>
        
        <div class="botoes">
            <button class="clear" onclick="limpar()">C</button>
            <button class="clear" onclick="apagar()">⌫</button>
            <button class="operador" onclick="inserir('%')">%</button>
            <button class="operador" onclick="inserir('/')">÷</button>
            
            <button class="numero" onclick="inserir('7')">7</button>
            <button class="numero" onclick="inserir('8')">8</button>
            <button class="numero" onclick="inserir('9')">9</button>
            <button class="operador" onclick="inserir('*')">×</button>
            
            <button class="numero" onclick="inserir('4')">4</button>
            <button class="numero" onclick="inserir('5')">5</button>
            <button class="numero" onclick="inserir('6')">6</button>
            <button class="operador" onclick="inserir('-')">−</button>
            
            <button class="numero" onclick="inserir('1')">1</button>
            <button class="numero" onclick="inserir('2')">2</button>
            <button class="numero" onclick="inserir('3')">3</button>
            <button class="operador" onclick="inserir('+')">+</button>
            
            <button class="numero" onclick="inserir('0')" style="grid-column: span 2;">0</button>
            <button class="numero" onclick="inserir('.')">.</button>
            <button class="igual" onclick="calcular()">=</button>
        </div>
    </div>

    <script>
        let display = document.getElementById('display');
        let valorAtual = '0';
        let resetarDisplay = false;

        function atualizarDisplay() {
            display.textContent = valorAtual;
        }

        function inserir(valor) {
            if (resetarDisplay) {
                valorAtual = '0';
                resetarDisplay = false;
            }
            
            if (valorAtual === '0' && valor !== '.') {
                valorAtual = valor;
            } else {
                valorAtual += valor;
            }
            atualizarDisplay();
        }

        function limpar() {
            valorAtual = '0';
            atualizarDisplay();
        }

        function apagar() {
            if (valorAtual.length <= 1) {
                valorAtual = '0';
            } else {
                valorAtual = valorAtual.slice(0, -1);
            }
            atualizarDisplay();
        }

        function calcular() {
            try {
                // Substitui símbolos para cálculo
                let expressao = valorAtual
                    .replace(/×/g, '*')
                    .replace(/÷/g, '/')
                    .replace(/−/g, '-');
                
                let resultado = eval(expressao);
                
                // Formatação do resultado
                if (Number.isInteger(resultado)) {
                    valorAtual = resultado.toString();
                } else {
                    valorAtual = parseFloat(resultado.toFixed(8)).toString();
                }
                
                resetarDisplay = true;
                atualizarDisplay();
            } catch (e) {
                valorAtual = 'Erro';
                resetarDisplay = true;
                atualizarDisplay();
            }
        }

        // Permite usar o teclado
        document.addEventListener('keydown', function(e) {
            if (e.key >= '0' && e.key <= '9') inserir(e.key);
            if (e.key === '.') inserir('.');
            if (e.key === '+') inserir('+');
            if (e.key === '-') inserir('-');
            if (e.key === '*') inserir('*');
            if (e.key === '/') inserir('/');
            if (e.key === 'Enter' || e.key === '=') calcular();
            if (e.key === 'Backspace') apagar();
            if (e.key === 'Escape') limpar();
        });

        // Inicializa
        atualizarDisplay();
    </script>
</body>
</html>