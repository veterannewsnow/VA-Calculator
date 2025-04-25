# VA-Calculators
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>VA Disability Rating Calculator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f2f2f2;
      color: #1a1a1a;
      padding: 20px;
    }
    .container {
      max-width: 600px;
      margin: auto;
      padding: 20px;
      background: white;
      border-radius: 8px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    h1 {
      text-align: center;
      color: #002868;
    }
    .input-group {
      display: flex;
      flex-direction: column;
      margin-bottom: 10px;
    }
    .input-group input[type="number"], .input-group input[type="text"] {
      padding: 8px;
      margin-top: 4px;
      border-radius: 4px;
      border: 1px solid #ccc;
      font-size: 16px;
    }
    .input-group input[type="text"] {
      margin-top: 8px;
    }
    button {
      background-color: #d30000;
      color: white;
      border: none;
      padding: 10px;
      font-size: 16px;
      border-radius: 4px;
      cursor: pointer;
      margin-top: 10px;
      width: 100%;
    }
    button:hover {
      background-color: #a60000;
    }
    .result {
      margin-top: 20px;
      font-size: 1.2em;
      font-weight: bold;
      color: #007a33;
      text-align: center;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>VA Disability Rating Calculator</h1>
    <label>Enter each disability rating (0-100) and description:</label>
    <div id="inputs"></div>
    <button onclick="addInput()">+ Add Rating</button>
    <button onclick="calculateRating()">Calculate Combined Rating</button>
    <div class="result" id="result"></div>
  </div>

  <script>
    function addInput() {
      const container = document.getElementById('inputs');
      const group = document.createElement('div');
      group.className = 'input-group';

      const ratingInput = document.createElement('input');
      ratingInput.type = 'number';
      ratingInput.min = 0;
      ratingInput.max = 100;
      ratingInput.placeholder = 'Enter rating %';

      const descriptionInput = document.createElement('input');
      descriptionInput.type = 'text';
      descriptionInput.placeholder = 'Enter short description';

      group.appendChild(ratingInput);
      group.appendChild(descriptionInput);
      container.appendChild(group);
    }

    function calculateRating() {
      const groups = document.querySelectorAll('#inputs .input-group');
      let ratings = [];

      groups.forEach(group => {
        const inputs = group.querySelectorAll('input');
        const value = parseFloat(inputs[0].value);
        if (!isNaN(value)) ratings.push(value);
      });

      ratings.sort((a, b) => b - a);

      if (ratings.length === 0) {
        document.getElementById('result').innerText = 'Please enter at least one valid rating.';
        return;
      }

      let combined = 0;
      let remaining = 100;

      for (let i = 0; i < ratings.length; i++) {
        let additional = remaining * (ratings[i] / 100);
        combined += additional;
        remaining = 100 - combined;
      }

      let final = Math.round(combined / 10) * 10;
      final = Math.min(final, 100);

      let pay = getPay(final);
      document.getElementById('result').innerText = `Combined VA Disability Rating: ${final}%\nEstimated Monthly Compensation: $${pay.toLocaleString()}`;
    }

    function getPay(rating) {
      // 2024 rates for veteran alone (no dependents)
      const rates = {
        10: 171.23,
        20: 338.49,
        30: 524.31,
        40: 755.28,
        50: 1075.16,
        60: 1361.88,
        70: 1716.28,
        80: 1995.01,
        90: 2241.91,
        100: 3737.85
      };
      return rates[rating] || 0;
    }

    // Initialize with 5 input fields
    for (let i = 0; i < 5; i++) addInput();
  </script>
</body>
</html>
