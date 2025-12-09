<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AI STEM Practice Problem Generator — v0.1</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6fb;
      padding: 2rem;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .container {
      background: white;
      padding: 1.5rem;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.08);
      width: 100%;
      max-width: 450px;
    }

    input, button {
      width: 100%;
      padding: 0.7rem;
      margin-top: 0.8rem;
      border-radius: 8px;
      border: 1px solid #ccc;
      font-size: 1rem;
    }

    button {
      background: #007bff;
      color: white;
      border: none;
      cursor: pointer;
      font-weight: bold;
    }

    button:hover {
      opacity: 0.9;
    }

    #output {
      white-space: pre-wrap;
      background: #eef2ff;
      padding: 1rem;
      border-radius: 8px;
      margin-top: 1.2rem;
      border: 1px solid #cdd1ff;
    }
  </style>
</head>
<body>

  <h1>AI STEM Practice Problem Generator</h1>
  <h3>v0.1 — Created by Vyom Jain</h3>

  <div class="container">
    <label>Enter a topic:</label>
    <input id="topicInput" type="text" placeholder="e.g., Newton's Laws, Quadratics" />

    <button id="generateBtn">Generate Questions</button>

    <div id="output"></div>
  </div>

  <script>
    const API_KEY = "AIzaSyCwh1yk0FTQ9yCc4qYG-yODi8N5O8__inc"; // <-- Replace this

    async function generateProblems() {
      const topic = document.getElementById("topicInput").value.trim();
      const output = document.getElementById("output");

      if (!topic) {
        alert("Please enter a topic.");
        return;
      }

      output.textContent = "Generating questions... (5–10 sec)";

      try {
        const response = await fetch(
          `https://generativelanguage.googleapis.com/v1/models/gemini-1.5-flash:generateContent?key=${API_KEY}`,
          {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
              contents: [{
                parts: [{
                  text: `Generate 3 high school STEM practice problems about: ${topic}.
                  Do NOT include solutions. Only questions.`
                }]
              }]
            })
          }
        );

        const data = await response.json();

        // DEBUG: Inspect the full returned object in the console
        console.log("Full API Response:", data);

        // Extract model text safely
        const text = data?.candidates?.[0]?.content?.parts?.[0]?.text || 
                     "Error: Could not read model response.";

        output.textContent = text;

      } catch (error) {
        console.error(error);
        output.textContent = "API Error: " + error.message;
      }
    }

    document.getElementById("generateBtn")
      .addEventListener("click", generateProblems);
  </script>

</body>
</html>
