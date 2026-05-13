<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Smart Smoke & Gas Detector</title>

  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

  <style>

    *{
      margin:0;
      padding:0;
      box-sizing:border-box;
      font-family: Arial, sans-serif;
    }

    body{
      background:#0f172a;
      color:white;
      display:flex;
      justify-content:center;
      align-items:center;
      min-height:100vh;
      padding:20px;
    }

    .dashboard{
      width:100%;
      max-width:900px;
      background:#111827;
      border-radius:20px;
      padding:30px;
      box-shadow:0 0 20px rgba(0,0,0,0.5);
    }

    h1{
      text-align:center;
      margin-bottom:30px;
      color:#38bdf8;
    }

    .cards{
      display:grid;
      grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
      gap:20px;
      margin-bottom:30px;
    }

    .card{
      background:#1e293b;
      padding:25px;
      border-radius:15px;
      text-align:center;
    }

    .card h2{
      margin-bottom:10px;
      color:#cbd5e1;
    }

    .value{
      font-size:45px;
      font-weight:bold;
      color:#38bdf8;
    }

    .status{
      margin-top:10px;
      font-size:20px;
      font-weight:bold;
      padding:10px;
      border-radius:10px;
    }

    .safe{
      background:green;
    }

    .danger{
      background:red;
      animation:blink 1s infinite;
    }

    @keyframes blink{
      50%{
        opacity:0.5;
      }
    }

    .alert-box{
      margin-top:20px;
      padding:15px;
      border-radius:10px;
      text-align:center;
      font-size:18px;
      background:#334155;
    }

    canvas{
      background:white;
      border-radius:10px;
      padding:10px;
    }

    .footer{
      margin-top:20px;
      text-align:center;
      color:#94a3b8;
      font-size:14px;
    }

  </style>
</head>

<body>

  <div class="dashboard">

    <h1>SMART SMOKE & GAS DETECTOR</h1>

    <div class="cards">

      <div class="card">
        <h2>Gas Level</h2>
        <div class="value" id="gasValue">0</div>
      </div>

      <div class="card">
        <h2>System Status</h2>
        <div class="status safe" id="status">
          SAFE
        </div>
      </div>

      <div class="card">
        <h2>Alert</h2>
        <div class="alert-box" id="alertBox">
          No danger detected
        </div>
      </div>

    </div>

    <canvas id="gasChart"></canvas>

    <div class="footer">
      Developed using HTML, CSS, JavaScript & IoT Concepts
    </div>

  </div>

  <script>

    const gasValue = document.getElementById("gasValue");
    const statusBox = document.getElementById("status");
    const alertBox = document.getElementById("alertBox");

    const ctx = document.getElementById("gasChart").getContext("2d");

    let labels = [];
    let data = [];

    const gasChart = new Chart(ctx, {
      type: 'line',
      data: {
        labels: labels,
        datasets: [{
          label: 'Gas Levels',
          data: data,
          borderWidth: 3,
          tension: 0.3
        }]
      },
      options: {
        responsive:true,
        scales:{
          y:{
            beginAtZero:true,
            max:500
          }
        }
      }
    });

    function updateDashboard(){

      let gasLevel = Math.floor(Math.random() * 500);

      gasValue.innerText = gasLevel;

      if(gasLevel > 300){

        statusBox.innerText = "DANGER";
        statusBox.classList.remove("safe");
        statusBox.classList.add("danger");

        alertBox.innerText = "WARNING! Smoke or Gas Detected";

      }else{

        statusBox.innerText = "SAFE";
        statusBox.classList.remove("danger");
        statusBox.classList.add("safe");

        alertBox.innerText = "Environment is safe";

      }

      let currentTime = new Date().toLocaleTimeString();

      labels.push(currentTime);
      data.push(gasLevel);

      if(labels.length > 10){
        labels.shift();
        data.shift();
      }

      gasChart.update();
    }

    setInterval(updateDashboard, 2000);

  </script>

</body>
</html>
