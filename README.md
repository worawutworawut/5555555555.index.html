<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ESP32 Servo Control</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f9;
      text-align: center;
      padding: 50px;
    }
    .login-container {
      background: #fff;
      padding: 20px;
      margin: auto;
      max-width: 300px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
      border-radius: 10px;
    }
    input[type="text"], input[type="password"] {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
    button {
      background: #007bff;
      color: white;
      padding: 10px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background: #0056b3;
    }
    #status {
      margin-top: 20px;
      font-size: 16px;
      color: green;
    }
  </style>
</head>
<body>
  <div class="login-container">
    <h1>Login</h1>
    <form id="loginForm">
      <input type="text" id="username" placeholder="ชื่อผู้ใช้งาน" required>
      <input type="password" id="password" placeholder="รหัสผ่าน" required>
      <button type="submit">Login</button>
    </form>
    <p id="status"></p>
  </div>

  <script>
    const esp32URL = "http://172.20.10.2/control"; // แก้ไข IP ให้ตรงกับ ESP32 ของคุณ

    document.getElementById("loginForm").addEventListener("submit", (e) => {
      e.preventDefault();

      const username = document.getElementById("username").value;
      const password = document.getElementById("password").value;

      fetch(esp32URL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ username, password }),
      })
        .then((response) => response.json())
        .then((data) => {
          if (data.status === "success") {
            document.getElementById("status").textContent = `ยินดีต้อนรับ, ${data.username}! Servo ทำงานสำเร็จ`;
          } else {
            document.getElementById("status").textContent = "Login ไม่สำเร็จ";
          }
        })
        .catch((error) => {
          document.getElementById("status").textContent = `Error: ${error.message}`;
        });
    });
  </script>
</body>
</html>
