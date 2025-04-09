# AIP-Project-
const express = require("express");
const bodyParser = require("body-parser");
const path = require("path");
const dotenv = require("dotenv");
dotenv.config();

const app = express();
const PORT = process.env.PORT || 3000;

// Use fetch workaround (for Node.js < 18)
const fetch = (...args) => import('node-fetch').then(({ default: fetch }) => fetch(...args));

// Middleware
app.use(bodyParser.json());
app.use(express.static(path.join(__dirname, "public")));

// Chat API endpoint
app.post("/api/chat", async (req, res) => {
  const userMessage = req.body.message;
  try {
    const response = await fetch("http://localhost:11434/api/generate", {
   
   method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        model: "llama2",
        prompt: userMessage,
        stream: false
      }),
    });

    const data = await response.json();
    const botReply = data.response || "Sorry, I didn't understand that.";
    res.json({ reply: botReply });

  } catch (err) {
    console.error("Local LLM error:", err);
    const errorMessage = err.message || "Something went wrong.";
    res.status(500).json({ reply: `Local LLM Error: ${errorMessage}` });
  }
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
•	Frontend Files:
	index.html – Main HTML layout for chatbot interface structure
	style.css – Custom CSS for styling layout, dark mode, chat messages, and voice button
	script.js – Handles chat submission, response rendering, dark mode toggle, and voice input logic
	voice-button (inline in index.html) – A button used to trigger voice recognition input
	#chatbox (div in index.html) – Displays chat history dynamically
	toggleDark button (index.html) – UI button to switch between dark/light themes
	voice-toggle button (index.html) – UI button to show/hide the voice recognition input button.
