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
// Mock restaurant data
const restaurants = [
  { name: "Kebab King", lat: 40.7128, lon: -74.0060, rating: 4.5, prepTime: 15 },
  { name: "Tasty Grill", lat: 40.7228, lon: -74.0160, rating: 4.0, prepTime: 20 }
];

// Mock user location (replace with real geolocation later)
const userLocation = { lat: 40.7128, lon: -74.0060 };

// Calculate distance (simplified)
function getDistance(lat1, lon1, lat2, lon2) {
  return Math.sqrt(Math.pow(lat2 - lat1, 2) + Math.pow(lon2 - lon1, 2));
}

// Choose restaurant
function chooseRestaurant() {
  let bestRestaurant = restaurants[0];
  let bestScore = -Infinity;

  restaurants.forEach(r => {
    const distance = getDistance(userLocation.lat, userLocation.lon, r.lat, r.lon);
    const score = r.rating / (distance + 0.1); // Simple scoring
    if (score > bestScore) {
      bestScore = score;
      bestRestaurant = r;
    }
  });

  return bestRestaurant;
}

// Update recognition.onresult
recognition.onresult = (event) => {
  const userInput = event.results[0][0].transcript.toLowerCase();
  transcript.textContent = `You said: ${userInput}`;
  
  if (userInput.includes('kebab')) {
    if (userInput.includes('chicken')) {
      const restaurant = chooseRestaurant();
      speak(`Great, I’ve ordered a chicken kebab from ${restaurant.name}. It’ll be ready in about ${restaurant.prepTime} minutes.`);
    } else {
      speak('What meat would you prefer? Chicken, lamb, or mixed?');
      recognition.start();
    }
  } else {
    speak('Sorry, I only handle kebabs for now. Try again?');
  }
};
navigator.geolocation.getCurrentPosition(pos => {
  userLocation.lat = pos.coords.latitude;
  userLocation.lon = pos.coords.longitude;
});
// Mock drivers
const drivers = [
  { name: "Alex", lat: 40.7128, lon: -74.0060, available: true },
  { name: "Sam", lat: 40.7228, lon: -74.0160, available: true }
];

// Assign driver
function assignDriver() {
  return drivers.find(d => d.available) || { name: "No driver available" };
}

// Update order logic
if (userInput.includes('chicken')) {
  const restaurant = chooseRestaurant();
  const driver = assignDriver();
  speak(`Great, I’ve ordered a chicken kebab from ${restaurant.name}. ${driver.name} is picking it up, ready in about ${restaurant.prepTime} minutes.`);
  console.log(`Order: Chicken kebab from ${restaurant.name}, driver: ${driver.name}`);
  // Mock saving preference
  localStorage.setItem('lastOrder', 'chicken kebab');
}
git init
git add .
git commit -m "Initial Routye MVP"
git remote add origin <your-repo-url>
git push -u origin main
npx serve
if (localStorage.getItem('lastOrder')) {
  speak(`Welcome back! Want another ${localStorage.getItem('lastOrder')}?`);
}
