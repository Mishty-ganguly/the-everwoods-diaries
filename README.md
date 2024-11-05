<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>The Everwoods Diaries</title>
  <style>
    /* General Styling */
    body, html {
      font-family: 'Arial', sans-serif;
      margin: 0;
      padding: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      background-color: #E3D8C4;
      transition: background-color 0.3s;
      color: #4F746C;
    }

    /* Main title and start button */
    .main-title {
      font-size: 3em;
      margin-bottom: 20px;
      text-align: center;
      color: #810900;
    }

    .start-button {
      background-color: #BF8859;
      color: white;
      padding: 15px 30px;
      font-size: 1.5em;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.3s;
    }

    .start-button:hover {
      background-color: #755C57;
    }

    /* Aesthetic Selection */
    .aesthetic-selection, .writing-page, .entry-list {
      display: none;
      text-align: center;
      width: 80%;
      max-width: 800px;
      margin: 20px auto;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 15px rgba(0,0,0,0.2);
    }

    .aesthetic-button {
      background-color: #8E836F;
      color: white;
      padding: 10px 20px;
      margin: 10px;
      font-size: 1.2em;
      border: none;
      cursor: pointer;
      border-radius: 5px;
      transition: background-color 0.3s;
    }

    .aesthetic-button:hover {
      background-color: #4F746C;
    }

    /* Writing Page */
    .writing-page {
      background-color: #FFD5B1; /* Default color */
      border: 1px solid #BF8859;
      padding: 20px;
    }

    .writing-page textarea {
      width: 100%;
      height: 300px;
      font-size: 1.2em;
      padding: 10px;
      border: 2px solid #BF8859;
      border-radius: 5px;
      resize: none;
      margin-bottom: 20px;
    }

    .button {
      padding: 10px 20px;
      font-size: 1em;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-weight: bold;
      color: #FFF;
      transition: background-color 0.3s;
    }

    .save-button { background-color: #8E836F; }
    .save-button:hover { background-color: #607961; }

    .view-button { background-color: #487784; }
    .view-button:hover { background-color: #4F746C; }

    .change-button { background-color: #BF8859; }
    .change-button:hover { background-color: #8E836F; }

    /* Entry List */
    .entry-list {
      max-width: 600px;
      padding: 20px;
    }

    .entry-item {
      padding: 10px;
      background-color: #C07869;
      border-radius: 5px;
      margin-bottom: 10px;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <!-- Main Title and Start Button -->
  <div class="main-title">THE EVERWOODS DIARIES</div>
  <button class="start-button" onclick="startApp()">Start</button>

  <!-- Aesthetic Selection Screen -->
  <div class="aesthetic-selection" id="aestheticSelection">
    <h2>Select Your Aesthetic:</h2>
    <button class="aesthetic-button" onclick="selectAesthetic('school')">school</button>
    <button class="aesthetic-button" onclick="selectAesthetic('secret')">Cottagecore</button>
    <button class="aesthetic-button" onclick="selectAesthetic(' diary as a friend')">diary as a friend</button>
    <button class="aesthetic-button" onclick="selectAesthetic('mood')">mood</button>
    <button class="aesthetic-button" onclick="selectAesthetic('Story')">story</button>
    <button class="aesthetic-button" onclick="selectAesthetic('memory')">Memory</button>
    <button class="aesthetic-button" onclick="selectAesthetic('health')">health</button>
  </div>

  <!-- Writing Page -->
  <div class="writing-page" id="writingPage">
    <h2 id="aestheticTitle"></h2>
    <textarea id="entryText" placeholder="Write your thoughts..."></textarea>
    <button class="button save-button" onclick="saveEntry()">Save</button>
    <button class="button view-button" onclick="viewEntries()">View Previous Entries</button>
    <button class="button change-button" onclick="changeAesthetic()">Change Aesthetic</button>
  </div>

  <!-- Entry List -->
  <div class="entry-list" id="entryList">
    <h2>Saved Entries</h2>
    <div id="entries"></div>
    <button class="button change-button" onclick="newEntry()">New Entry</button>
    <button class="button change-button" onclick="returnToSelection()">Return to Aesthetic Selection</button>
  </div>

  <audio id="bgm" loop>
    <source src="backgroung-cottagcorebgm.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>

  <script>
    // Function to start the app
    function startApp() {
      document.querySelector('.main-title').style.display = 'none';
      document.querySelector('.start-button').style.display = 'none';
      document.getElementById('bgm').play();
      document.getElementById('bgm').volume = 0.1; // Low volume
      document.getElementById('aestheticSelection').style.display = 'block';
    }

    // Aesthetic selection handler
    function selectAesthetic(aesthetic) {
      document.getElementById('aestheticSelection').style.display = 'none';
      document.getElementById('writingPage').style.display = 'block';
      document.getElementById('aestheticTitle').textContent = aesthetic + " Diary";

      // Set different styles based on the selected aesthetic
      const page = document.getElementById('writingPage');
      switch (aesthetic) {
        case 'school':
          page.style.backgroundColor = '#F2BA81';
          break;
        case 'secret':
          page.style.backgroundColor = '#EBD2B4';
          break;
        case 'diary as a friend':
          page.style.backgroundColor = '#F39474';
          break;
        case 'mood':
          page.style.backgroundColor = '#4F746C';
          break;
        case 'story':
          page.style.backgroundColor = '#8DA279';
          break;
        case 'memory':
          page.style.backgroundColor = '#BBD9E1';
          break;
        case 'health':
          page.style.backgroundColor = '#BF8859';
          break;
      }
    }

    // Save entry with the date
    function saveEntry() {
      const text = document.getElementById('entryText').value;
      if (text) {
        const date = new Date().toLocaleString();
        const entry = { text: text, date: date };
        let entries = JSON.parse(localStorage.getItem('entries')) || [];
        entries.push(entry);
        localStorage.setItem('entries', JSON.stringify(entries));
        alert('Entry saved!');
        document.getElementById('entryText').value = '';  // Clear the textarea
      } else {
        alert('Please write something before saving.');
      }
    }

    // View saved entries
    function viewEntries() {
      document.getElementById('writingPage').style.display = 'none';
      document.getElementById('entryList').style.display = 'block';

      const entriesDiv = document.getElementById('entries');
      entriesDiv.innerHTML = '';  // Clear existing entries
      const entries = JSON.parse(localStorage.getItem('entries')) || [];
      entries.forEach((entry) => {
        const entryItem = document.createElement('div');
        entryItem.className = 'entry-item';
        entryItem.textContent = entry.date + ": " + entry.text.slice(0, 50) + "...";
        entryItem.onclick = () => alert(entry.date + ": " + entry.text);
        entriesDiv.appendChild(entryItem);
      });
    }

    // Function to create a new entry
    function newEntry() {
      document.getElementById('entryList').style.display = 'none';
      document.getElementById('writingPage').style.display = 'block';
    }

    // Return to aesthetic selection screen
    function returnToSelection() {
      document.getElementById('entryList').style.display = 'none';
      document.getElementById('writingPage').style.display = 'none';
      document.getElementById('aestheticSelection').style.display = 'block';
    }

    // Change aesthetic from the writing page
    function changeAesthetic() {
      document.getElementById('writingPage').style.display = 'none';
      document.getElementById('aestheticSelection').style.display = 'block';
    }
  </script>

</body>
</html>


