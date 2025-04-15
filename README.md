<!DOCTYPE html>
<html>
<head>
  <title>Routye Voice Ordering</title>
</head>
<body>
  <h1>Routye.com Voice Order</h1>
  <button onclick="startListening()">Speak Order</button>
  <p id="transcript">Say something...</p>

  <script>
    const transcript = document.getElementById('transcript');
    const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
    recognition.lang = 'en-US';

    recognition.onresult = (event) => {
      const userInput = event.results[0][0].transcript.toLowerCase();
      transcript.textContent = `You said: ${userInput}`;
      
      // Simple logic for demo
      if (userInput.includes('kebab')) {
        speak('What meat would you prefer? Chicken, lamb, or mixed?');
        recognition.start(); // Listen for next input
      } else {
        speak('Sorry, I only handle kebabs for now. Try again?');
      }
    };

    function startListening() {
      recognition.start();
      transcript.textContent = 'Listening...';
    }

    function speak(text) {
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = 'en-US';
      window.speechSynthesis.speak(utterance);
    }
  </script>
</body>
</html>
