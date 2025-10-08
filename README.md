# UMCH-Cheek-up-update
Report Download new
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>PDF Access Portal</title>
  <style>
    body { font-family: Arial, sans-serif; max-width:700px; margin:30px auto; text-align:center; }
    input, button { padding:8px; font-size:16px; margin:5px; }
    #viewer{margin-top:20px;}
    h1{color:#0056b3;}
    .box {
      border:1px solid #ccc; 
      border-radius:8px; 
      padding:20px; 
      margin-top:20px;
      box-shadow:0 2px 8px rgba(0,0,0,0.1);
    }
  </style>
</head>
<body>
  <h1>UNIVERSAL MEDICAL COLLEGE & HOSPITAL LTD</h1>

  <!-- Registration Section -->
  <div id="registerSection" class="box">
    <h2>New User? Register Here</h2>
    <input id="regUser" placeholder="Create User ID" /><br>
    <input id="regPass" type="password" placeholder="Create Password" /><br>
    <button onclick="register()">Register</button>
    <p id="regMsg" style="color:green;"></p>
  </div>

  <!-- Login Section -->
  <div id="loginSection" class="box">
    <h2>Login</h2>
    <input id="userId" placeholder="User ID লিখুন" /><br>
    <input id="password" type="password" placeholder="Password লিখুন" /><br>
    <button onclick="login()">Login</button>
    <p id="loginMsg" style="color:red;"></p>
  </div>

  <!-- PDF Section -->
  <div id="pdfSection" class="box" style="display:none;">
    <h2> Admission ID  দিয়ে পিডিএফ দেখুন / ডাউনলোড</h2>
    <input id="pdfIdInput" placeholder="নম্বর লিখুন (যেমন Dia)" /> 
    <button onclick="openPDF()">Open</button>
    <div id="viewer"></div>
    <br>
    <button onclick="logout()">Logout</button>
  </div>

  <script>
    // ==== REGISTER FUNCTION ====
    function register(){
      const uid = document.getElementById('regUser').value.trim();
      const pass = document.getElementById('regPass').value.trim();
      const msg = document.getElementById('regMsg');

      if(!uid || !pass){
        msg.style.color='red';
        msg.textContent = "User ID ও Password দিন।";
        return;
      }

      if(localStorage.getItem(uid)){
        msg.style.color='red';
        msg.textContent = "এই User ID আগেই আছে!";
        return;
      }

      localStorage.setItem(uid, pass);
      msg.style.color='green';
      msg.textContent = "User ID তৈরি হয়েছে! এখন Login করুন।";
      document.getElementById('regUser').value='';
      document.getElementById('regPass').value='';
    }

    // ==== LOGIN FUNCTION ====
    function login(){
      const uid = document.getElementById('userId').value.trim();
      const pass = document.getElementById('password').value.trim();
      const msg = document.getElementById('loginMsg');
      const storedPass = localStorage.getItem(uid);

      if(storedPass && storedPass === pass){
        msg.textContent = "";
        document.getElementById('loginSection').style.display = 'none';
        document.getElementById('registerSection').style.display = 'none';
        document.getElementById('pdfSection').style.display = 'block';
      } else {
        msg.textContent = "ভুল User ID বা Password!";
      }
    }

    // ==== LOGOUT ====
    function logout(){
      document.getElementById('pdfSection').style.display = 'none';
      document.getElementById('loginSection').style.display = 'block';
      document.getElementById('registerSection').style.display = 'block';
      document.getElementById('userId').value='';
      document.getElementById('password').value='';
    }

    // ==== PDF VIEW ====
    function openPDF(){
      const num = document.getElementById('pdfIdInput').value.trim();
      const viewer = document.getElementById('viewer');
      viewer.innerHTML = '';

      if(!num){ 
        viewer.innerHTML = "<p style='color:red;'>নম্বর দিন।</p>"; 
        return; 
      }

      const url = num + ".pdf";
      const iframe = document.createElement('iframe');
      iframe.src = url;
      iframe.width = "100%";
      iframe.height = "600";
      iframe.style.border = "1px solid #ccc";
      viewer.appendChild(iframe);

      const dl = document.createElement('p');
      dl.innerHTML = `<a href="${url}" target="_blank" download>Download PDF</a>`;
      viewer.appendChild(dl);
    }
  </script>
</body>
</html>
