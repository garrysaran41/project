
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ultrasonic Distance Monitor</title>
  <style>
    body {
      font-family: 'Poppins', sans-serif;
      text-align: center;
      background: linear-gradient(120deg, #00c6ff, #0072ff);
      color: white;
      height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
    }
    h1 { font-size: 2.5rem; margin-bottom: 20px; }
    .distance {
      font-size: 3rem;
      background: rgba(255,255,255,0.1);
      padding: 20px;
      border-radius: 10px;
      display: inline-block;
    }
    button {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 1rem;
      border: none;
      background: white;
      color: #0072ff;
      border-radius: 10px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>Arduino Ultrasonic Distance</h1>
  <div class="distance" id="distance">-- cm</div>
  <br>
  <button id="connect">🔌 Connect Arduino</button>

  <script>
    const distanceDisplay = document.getElementById('distance');
    const connectButton = document.getElementById('connect');

    connectButton.addEventListener('click', async () => {
      try {
        const port = await navigator.serial.requestPort();
        await port.open({ baudRate: 9600 });
        const decoder = new TextDecoderStream();
        port.readable.pipeTo(decoder.writable);
        const reader = decoder.readable.getReader();

        while (true) {
          const { value, done } = await reader.read();
          if (done) break;
          if (value) {
            distanceDisplay.textContent = value.trim() + " cm";
          }
        }
      } catch (err) {
        alert("Serial connection failed: " + err);
      }
    });
  </script>
</body>
</html>
