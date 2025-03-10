<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: #f0f0f0;
        }

        .calculator {
            background: #333;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.2);
            width: 320px;
        }

        .display {
            background: #98fb98;
            padding: 20px;
            border-radius: 5px;
            margin-bottom: 20px;
            text-align: right;
            font-size: 24px;
            height: 70px;
            overflow: hidden;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        button {
            padding: 15px;
            font-size: 18px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            background: #666;
            color: white;
            transition: background 0.3s;
        }

        button:hover {
            background: #999;
        }

        .operator {
            background: #ff9500;
        }

        .equals {
            background: #007bff;
        }

        .clear {
            background: #dc3545;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display">0</div>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="operator" onclick="appendOperator('/')">/</button>
            <button class="operator" onclick="appendOperator('*')">×</button>
            <button class="operator" onclick="appendOperator('-')">-</button>
            <button onclick="appendNumber('7')">7</button>
            <button onclick="appendNumber('8')">8</button>
            <button onclick="appendNumber('9')">9</button>
            <button class="operator" onclick="appendOperator('+')">+</button>
            <button onclick="appendNumber('4')">4</button>
            <button onclick="appendNumber('5')">5</button>
            <button onclick="appendNumber('6')">6</button>
            <button onclick="appendNumber('.')">.</button>
            <button onclick="appendNumber('1')">1</button>
            <button onclick="appendNumber('2')">2</button>
            <button onclick="appendNumber('3')">3</button>
            <button class="equals" onclick="calculate()" style="grid-row: span 2">=</button>
            <button onclick="appendNumber('0')" style="grid-column: span 2">0</button>
            <button onclick="appendNumber('00')">00</button>
        </div>
    </div>

    <script>
        let display = document.querySelector('.display');
        let currentValue = '0';
        let firstOperand = null;
        let operator = null;
        let waitingForSecondOperand = false;

        function updateDisplay() {
            display.textContent = currentValue;
        }

        function appendNumber(number) {
            if (waitingForSecondOperand) {
                currentValue = number;
                waitingForSecondOperand = false;
            } else {
                currentValue = currentValue === '0' ? number : currentValue + number;
            }
            updateDisplay();
        }

        function appendOperator(nextOperator) {
            const inputValue = parseFloat(currentValue);
            
            if (operator && waitingForSecondOperand) {
                operator = nextOperator;
                return;
            }

            if (firstOperand === null) {
                firstOperand = inputValue;
            } else if (operator) {
                const result = calculate(firstOperand, inputValue, operator);
                currentValue = ${parseFloat(result.toFixed(5))};
                firstOperand = result;
                updateDisplay();
            }

            operator = nextOperator;
            waitingForSecondOperand = true;
        }

        function calculate() {
            if (operator === null || waitingForSecondOperand) return;
            
            const secondOperand = parseFloat(currentValue);
            let result;
            
            switch(operator) {
                case '+':
                    result = firstOperand + secondOperand;
                    break;
                case '-':
                    result = firstOperand - secondOperand;
                    break;
                case '*':
                    result = firstOperand * secondOperand;
                    break;
                case '/':
                    result = secondOperand === 0 ? 'Error' : firstOperand / secondOperand;
                    break;
                default:
                    return;
            }

            if (result === 'Error') {
                currentValue = 'Error';
                firstOperand = null;
                operator = null;
            } else {
                currentValue = ${parseFloat(result.toFixed(5))};
                firstOperand = result;
                operator = null;
            }
            
            waitingForSecondOperand = false;
            updateDisplay();
        }

        function clearDisplay() {
            currentValue = '0';
            firstOperand = null;
            operator = null;
            waitingForSecondOperand = false;
            updateDisplay();
        }

        document.addEventListener('keydown', (e) => {
            if (e.key >= '0' && e.key <= '9') appendNumber(e.key);
            if (e.key === '.') appendNumber('.');
            if (e.key === '+' || e.key === '-' || e.key === '*' || e.key === '/') 
                appendOperator(e.key);
            if (e.key === 'Enter') calculate();
            if (e.key === 'Escape') clearDisplay();
        });
    </script>
</body>
</html>
