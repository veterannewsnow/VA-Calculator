# VA-Calculator
VA disability and compensation calculator


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
    label {
      display: block;
      margin-top: 10px;
    }
    input[type="number"] {
      width: 100%;
      padding: 8px;
      margin-top: 4px;
      margin-bottom: 10px;
      border-radius: 4px;
      border: 1px solid #ccc;
      font-size: 16px;
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
    <label>Enter each disability rating (0-100):</label>
    <div id="inputs"></div>
    <button onclick="addInput()">+ Add Rating</button>
    <button onclick="calculateRating()">Calculate Combined Rating</button>
    <div class="result" id="result"></div>
  </div>

  <script>
    function addInput() {
      const container = document.getElementById('inputs');
      const input = document.createElement('input');
      input.type = 'number';
      input.min = 0;
      input.max = 100;
      input.placeholder = 'Enter rating %';
      container.appendChild(input);
    }

    function calculateRating() {
      const inputs = document.querySelectorAll('#inputs input');
      const ratings = Array.from(inputs).map(input => parseFloat(input.value)).filter(val => !isNaN(val));
      ratings.sort((a, b) => b - a);

      if (ratings.length === 0) {
        document.getElementById('result').innerText = 'Please enter at least one valid rating.';
        return;
      }

      let combined = ratings[0];
      for (let i = 1; i < ratings.length; i++) {
        combined = combined + (1 - combined / 100) * ratings[i];
        combined = Math.round(combined * 10) / 10;
      }
      const final = Math.min(Math.round((combined + 5) / 10) * 10, 100);
      document.getElementById('result').innerText = `Combined VA Disability Rating: ${final}%`;
    }

    // Initialize with one input
    addInput();
  </script>
</body>
</html>
