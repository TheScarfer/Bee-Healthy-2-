# Bee-Healthy-2-

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Bee-Healthy Pollen Tracker</title>
  <!-- Chart.js CDN -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to bottom, #01140a, #043362);
      color: #333;
      padding: 20px;
    }

    .container {
      max-width: 1200px;
      margin: auto;
    }

    header h1 {
      text-align: center;
      font-size: 2.5rem;
      margin-bottom: 20px;
      color: #2e7d32;
    }

    .card {
      background: white;
      padding: 20px;
      border-radius: 16px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      margin-bottom: 20px;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }

    .card:hover {
      transform: scale(1.02);
      box-shadow: 0 8px 24px rgba(0,0,0,0.2);
    }

    .location-info {
      text-align: center;
      font-size: 1.2rem;
    }

    .high {
      color: #d32f2f;
      font-weight: bold;
    }

    .moderate {
      color: #f57c00;
      font-weight: bold;
    }

    .low {
      color: #388e3c;
      font-weight: bold;
    }

    .two-column {
      display: flex;
      gap: 20px;
      flex-wrap: wrap;
    }

    .two-column .card {
      flex: 1;
      min-width: 300px;
    }

    .full-width {
      width: 100%;
    }

    ul {
      padding-left: 20px;
    }

    .chart-container {
      position: relative;
      height: 300px;
      margin: 20px 0;
    }

    .chart-placeholder {
      height: 200px;
      background: #f3f3f3;
      border: 2px dashed #ccc;
      border-radius: 12px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #888;
      font-size: 1rem;
    }

    .loading {
      color: #666;
      font-style: italic;
    }

    .error {
      color: #d32f2f;
      background: #ffebee;
      padding: 10px;
      border-radius: 8px;
      margin: 10px 0;
    }

    .pollen-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 8px 0;
      border-bottom: 1px solid #eee;
    }

    .pollen-item:last-child {
      border-bottom: none;
    }

    .pollen-value {
      font-weight: bold;
    }

   

    .refresh-btn {
      background: #2196f3;
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 6px;
      cursor: pointer;
      margin-left: 10px;
      font-size: 0.9rem;
    }

    .refresh-btn:hover {
      background: #1976d2;
    }

    .location-btn {
      background: #4caf50;
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 6px;
      cursor: pointer;
      margin-left: 10px;
      font-size: 0.9rem;
    }

    .location-btn:hover {
      background: #388e3c;
    }

    .location-btn:disabled {
      background: #ccc;
      cursor: not-allowed;
    }

    .location-status {
      font-size: 0.9rem;
      margin-top: 5px;
      color: #666;
    }

    .location-error {
      color: #d32f2f;
      font-size: 0.9rem;
      margin-top: 5px;
    }

    .chart-controls {
      display: flex;
      gap: 10px;
      margin-bottom: 15px;
      flex-wrap: wrap;
    }

    .chart-btn {
      background: #f5f5f5;
      border: 1px solid #ddd;
      padding: 5px 12px;
      border-radius: 4px;
      cursor: pointer;
      font-size: 0.85rem;
      transition: all 0.3s ease;
    }

    .chart-btn:hover {
      background: #e0e0e0;
    }

    .chart-btn.active {
      background-color: #2196F3;
      color: white;
    }

    /* Chatbot Styles */
    .chatbot-container {
      background: #f8f9fa;
      border-radius: 12px;
      padding: 20px;
      margin-top: 20px;
    }

    .chat-messages {
      height: 300px;
      overflow-y: auto;
      padding: 15px;
      background: white;
      border-radius: 8px;
      border: 1px solid #e0e0e0;
      margin-bottom: 15px;
    }

    .message {
      margin-bottom: 15px;
      display: flex;
      align-items: flex-start;
    }

    .user-message {
      justify-content: flex-end;
    }

    .bot-message {
      justify-content: flex-start;
    }

    .message-content {
      max-width: 80%;
      padding: 12px 16px;
      border-radius: 18px;
      word-wrap: break-word;
    }

    .user-message .message-content {
      background: #2196F3;
      color: white;
      border-bottom-right-radius: 4px;
    }

    .bot-message .message-content {
      background: #f1f3f4;
      color: #333;
      border-bottom-left-radius: 4px;
    }

    .chat-input-container {
      display: flex;
      gap: 10px;
      margin-bottom: 10px;
    }

    #chat-input {
      flex: 1;
      padding: 12px 16px;
      border: 2px solid #e0e0e0;
      border-radius: 25px;
      font-size: 14px;
      outline: none;
      transition: border-color 0.3s;
    }

    #chat-input:focus {
      border-color: #2196F3;
    }

    #send-chat-btn {
      padding: 12px 24px;
      background: #2196F3;
      color: white;
      border: none;
      border-radius: 25px;
      cursor: pointer;
      font-weight: 600;
      transition: background-color 0.3s;
    }

    #send-chat-btn:hover {
      background: #1976D2;
    }

    #send-chat-btn:disabled {
      background: #ccc;
      cursor: not-allowed;
    }

    .chat-status {
      text-align: center;
      color: #666;
      font-size: 12px;
      font-style: italic;
    }

    .typing-indicator {
      display: flex;
      align-items: center;
      gap: 5px;
      color: #666;
      font-style: italic;
    }

    .typing-dots {
      display: flex;
      gap: 2px;
    }

    .typing-dots span {
      width: 6px;
      height: 6px;
      background: #666;
      border-radius: 50%;
      animation: typing 1.4s infinite ease-in-out;
    }

    .typing-dots span:nth-child(1) { animation-delay: -0.32s; }
    .typing-dots span:nth-child(2) { animation-delay: -0.16s; }

    @keyframes typing {
      0%, 80%, 100% { transform: scale(0); }
      40% { transform: scale(1); }
    }

    .uv-index {
      display: inline-block;
      padding: 4px 8px;
      border-radius: 4px;
      font-weight: bold;
      color: white;
    }

    .uv-low {
      background-color: #4caf50;
    }

    .uv-moderate {
      background-color: #ff9800;
    }

    .uv-high {
      background-color: #f44336;
    }

    .uv-very-high {
      background-color: #9c27b0;
    }

    .uv-extreme {
      background-color: #d32f2f;
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>🐝 Bee-Healthy</h1>
    </header>

    <section class="card location-info">
      <p>📍 Location: <strong id="location">Loading...</strong></p>
      <p>🌼 Today's Pollen Level: <span id="pollen-level" class="loading">Loading...</span></p>
      <p>🌱 Main Allergen: <strong id="main-allergen">Loading...</strong></p>
      <p>🌬️ Air Quality Index: <strong id="aqi">Loading...</strong></p>
      <p>☀️ UV Index: <strong id="uv-index">Loading...</strong></p>
      <div>
        <button class="refresh-btn" onclick="fetchPollenData()">🔄 Refresh Data</button>
        <button class="location-btn" id="location-btn" onclick="detectUserLocation()">📍 Use My Location</button>
      </div>
      <div id="location-status" class="location-status"></div>
    </section>

    <div class="two-column">
      <section class="card">
        <h2>📆 Forecast ⛅</h2>
        <ul id="forecast-list">
          <li class="loading">Loading forecast...</li>
        </ul>
      </section>

      <section class="card">
        <h2>💡 Allergy Tips</h2>
        <ul>
          <li>Wear a mask outdoors</li>
          <li>Keep windows closed</li>
          <li>Change clothes after being outside</li>
          <li>Use air purifiers indoors</li>
          <li>Check pollen levels before outdoor activities</li>
          <li>Apply sunscreen when UV index is high</li>
        </ul>
      </section>
    </div>

    <div class="two-column">
      <section class="card">
        <h2>📊 Current Pollen Breakdown</h2>
        <div id="pollen-breakdown">
          <div class="loading">Loading pollen data...</div>
        </div>
      </section>

      <section class="card">
        <h2>🌡️ Air Quality Details</h2>
        <div id="air-quality-details">
          <div class="loading">Loading air quality data...</div>
        </div>
      </section>
    </div>

    <!-- Pollen Levels Chart -->
    <section class="card full-width">
      <h2>📈 Pollen Levels Over Time</h2>
      <div class="chart-controls">
        <button class="chart-btn active" onclick="switchChartView('pollen')">Pollen Levels</button>
        <button class="chart-btn" onclick="switchChartView('air')">Air Quality</button>
        <button class="chart-btn" onclick="switchChartView('combined')">Combined View</button>
        <button class="chart-btn" onclick="switchChartView('uv')">UV Index</button>
      </div>
      <div class="chart-container">
        <canvas id="pollenChart"></canvas>
      </div>
    </section>

    <!-- Air Quality Chart -->
    <section class="card full-width">
      <h2>🌬️ Air Quality Trends</h2>
      <div class="chart-container">
        <canvas id="airQualityChart"></canvas>
      </div>
    </section>

    <!-- UV Index Chart -->
    <section class="card full-width">
      <h2>☀️ UV Index & Environmental Data</h2>
      <div class="chart-container">
        <canvas id="uvChart"></canvas>
      </div>
    </section>

    <!-- AI Health Assistant Chatbot -->
    <section class="card full-width">
      <h2>🤖 AI Health Assistant</h2>
      <div class="chatbot-container">
        <div class="chat-messages" id="chat-messages">
          <div class="message bot-message">
            <div class="message-content">
              <strong>Bee-Healthy AI:</strong> Hello! I'm your AI health assistant. I can help you with:
              <ul>
                <li>🌿 Pollen allergy information</li>
                <li>💊 Allergy management tips</li>
                <li>🏥 Health advice</li>
                <li>📊 Data interpretation</li>
              </ul>
              Ask me anything about your health or the pollen data!
            </div>
          </div>
        </div>
        <div class="chat-input-container">
          <input type="text" id="chat-input" placeholder="Ask me about pollen, allergies, or health..." maxlength="500">
          <button id="send-chat-btn" onclick="sendMessage()">Send</button>
        </div>
        <div class="chat-status" id="chat-status"></div>
      </div>
    </section>

    

  </div>

  <script>
    // Open-Meteo Air Quality API configuration
    const API_BASE_URL = 'https://air-quality-api.open-meteo.com/v1/air-quality';
    const DEFAULT_LAT = 52.52;
    const DEFAULT_LON = 13.41;

    // Global variables for current location
    let currentLat = DEFAULT_LAT;
    let currentLon = DEFAULT_LON;
    let userLocationDetected = false;

    // Chart instances
    let pollenChart = null;
    let airQualityChart = null;
    let uvChart = null;
    let currentChartView = 'pollen';

    // Pollen level thresholds (grains/m³)
    const POLLEN_LEVELS = {
      LOW: 0,
      MODERATE: 10,
      HIGH: 50,
      VERY_HIGH: 100
    };

    // UV Index categories
    const UV_LEVELS = {
      LOW: 0,
      MODERATE: 3,
      HIGH: 6,
      VERY_HIGH: 8,
      EXTREME: 11
    };

    // Chart colors
    const CHART_COLORS = {
      birch: '#4CAF50',
      alder: '#8BC34A',
      grass: '#CDDC39',
      mugwort: '#FF9800',
      olive: '#795548',
      ragweed: '#9C27B0',
      aqi: '#2196F3',
      pm10: '#FF5722',
      pm25: '#607D8B',
      uv: '#FFC107',
      dust: '#795548',
      carbon: '#607D8B'
    };

    // Get pollen level category
    function getPollenLevelCategory(value) {
      if (value >= POLLEN_LEVELS.VERY_HIGH) return 'Very High';
      if (value >= POLLEN_LEVELS.HIGH) return 'High';
      if (value >= POLLEN_LEVELS.MODERATE) return 'Moderate';
      return 'Low';
    }

    // Get UV level category
    function getUVLevelCategory(value) {
      if (value >= UV_LEVELS.EXTREME) return 'Extreme';
      if (value >= UV_LEVELS.VERY_HIGH) return 'Very High';
      if (value >= UV_LEVELS.HIGH) return 'High';
      if (value >= UV_LEVELS.MODERATE) return 'Moderate';
      return 'Low';
    }

    // Get UV level class
    function getUVLevelClass(level) {
      const levelLower = level.toLowerCase();
      if (levelLower.includes('extreme')) return 'uv-extreme';
      if (levelLower.includes('very high')) return 'uv-very-high';
      if (levelLower.includes('high')) return 'uv-high';
      if (levelLower.includes('moderate')) return 'uv-moderate';
      return 'uv-low';
    }

    // Get pollen level color class
    function getPollenLevelClass(level) {
      const levelLower = level.toLowerCase();
      if (levelLower.includes('very high') || levelLower.includes('high')) return 'high';
      if (levelLower.includes('moderate')) return 'moderate';
      return 'low';
    }

    // Format pollen value
    function formatPollenValue(value) {
      return value ? `${value.toFixed(1)} grains/m³` : '0.0 grains/m³';
    }

    // Get location name from coordinates
    async function getLocationName(lat, lon) {
      try {
        const response = await fetch(`https://api.bigdatacloud.net/data/reverse-geocode-client?latitude=${lat}&longitude=${lon}&localityLanguage=en`);
        const data = await response.json();
        return data.city || data.locality || `${lat.toFixed(2)}, ${lon.toFixed(2)}`;
      } catch (error) {
        return `${lat.toFixed(2)}, ${lon.toFixed(2)}`;
      }
    }

    // Detect user's current location
    function detectUserLocation() {
      const locationBtn = document.getElementById('location-btn');
      const locationStatus = document.getElementById('location-status');
      
      // Disable button and show loading
      locationBtn.disabled = true;
      locationBtn.textContent = '📍 Detecting...';
      locationStatus.textContent = 'Requesting location permission...';
      locationStatus.className = 'location-status';

      if (!navigator.geolocation) {
        showLocationError('Geolocation is not supported by this browser.');
        return;
      }

      navigator.geolocation.getCurrentPosition(
        // Success callback
        async (position) => {
          const lat = position.coords.latitude;
          const lon = position.coords.longitude;
          
          // Update global location variables
          currentLat = lat;
          currentLon = lon;
          userLocationDetected = true;
          
          locationStatus.textContent = `Location detected! (${lat.toFixed(4)}, ${lon.toFixed(4)})`;
          locationStatus.className = 'location-status';
          
          // Fetch data for new location
          await fetchPollenData();
          
          // Re-enable button
          locationBtn.disabled = false;
          locationBtn.textContent = '📍 Use My Location';
        },
        // Error callback
        (error) => {
          let errorMessage = 'Unable to retrieve your location.';
          
          switch(error.code) {
            case error.PERMISSION_DENIED:
              errorMessage = 'Location access denied. Please allow location access in your browser settings.';
              break;
            case error.POSITION_UNAVAILABLE:
              errorMessage = 'Location information is unavailable.';
              break;
            case error.TIMEOUT:
              errorMessage = 'Location request timed out.';
              break;
            case error.UNKNOWN_ERROR:
              errorMessage = 'An unknown error occurred while getting location.';
              break;
          }
          
          showLocationError(errorMessage);
        },
        // Options
        {
          enableHighAccuracy: true,
          timeout: 10000,
          maximumAge: 60000
        }
      );
    }

    // Show location error
    function showLocationError(message) {
      const locationBtn = document.getElementById('location-btn');
      const locationStatus = document.getElementById('location-status');
      
      locationStatus.textContent = message;
      locationStatus.className = 'location-error';
      
      // Re-enable button
      locationBtn.disabled = false;
      locationBtn.textContent = '📍 Use My Location';
    }

    // Switch chart view
    function switchChartView(view) {
      currentChartView = view;
      
      // Update button states
      document.querySelectorAll('.chart-btn').forEach(btn => btn.classList.remove('active'));
      event.target.classList.add('active');
      
      // Recreate charts with new view
      if (pollenChart) {
        updateCharts();
      }
    }

    // Create pollen chart
    function createPollenChart(hourlyData) {
      const ctx = document.getElementById('pollenChart').getContext('2d');
      
      // Destroy existing chart
      if (pollenChart) {
        pollenChart.destroy();
      }

      // Prepare data - show 7 days of data (168 hours)
      const dataPoints = Math.min(168, hourlyData.time.length);
      const labels = hourlyData.time.slice(0, dataPoints).map(time => {
        const date = new Date(time);
        return date.toLocaleDateString('en-US', { 
          month: 'short', 
          day: 'numeric',
          hour: '2-digit',
          minute: '2-digit'
        });
      });

      const datasets = [];
      
      if (currentChartView === 'pollen' || currentChartView === 'combined') {
        datasets.push({
          label: 'Birch Pollen',
          data: hourlyData.birch_pollen.slice(0, dataPoints),
          borderColor: CHART_COLORS.birch,
          backgroundColor: CHART_COLORS.birch + '20',
          tension: 0.4
        });
        datasets.push({
          label: 'Grass Pollen',
          data: hourlyData.grass_pollen.slice(0, dataPoints),
          borderColor: CHART_COLORS.grass,
          backgroundColor: CHART_COLORS.grass + '20',
          tension: 0.4
        });
        datasets.push({
          label: 'Mugwort Pollen',
          data: hourlyData.mugwort_pollen.slice(0, dataPoints),
          borderColor: CHART_COLORS.mugwort,
          backgroundColor: CHART_COLORS.mugwort + '20',
          tension: 0.4
        });
      }

      if (currentChartView === 'air' || currentChartView === 'combined') {
        datasets.push({
          label: 'US AQI',
          data: hourlyData.us_aqi ? hourlyData.us_aqi.slice(0, dataPoints) : [],
          borderColor: CHART_COLORS.aqi,
          backgroundColor: CHART_COLORS.aqi + '20',
          tension: 0.4,
          yAxisID: 'y1'
        });
      }

      if (currentChartView === 'uv') {
        datasets.push({
          label: 'UV Index',
          data: hourlyData.uv_index ? hourlyData.uv_index.slice(0, dataPoints) : [],
          borderColor: CHART_COLORS.uv,
          backgroundColor: CHART_COLORS.uv + '20',
          tension: 0.4
        });
      }

      pollenChart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: labels,
          datasets: datasets
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          interaction: {
            intersect: false,
            mode: 'index'
          },
          scales: {
            x: {
              display: true,
              title: {
                display: true,
                text: 'Date & Time'
              }
            },
            y: {
              type: 'linear',
              display: true,
              position: 'left',
              title: {
                display: true,
                text: currentChartView === 'uv' ? 'UV Index' : 'Pollen (grains/m³)'
              }
            },
            y1: {
              type: 'linear',
              display: currentChartView === 'air' || currentChartView === 'combined',
              position: 'right',
              title: {
                display: true,
                text: 'AQI'
              },
              grid: {
                drawOnChartArea: false,
              },
            }
          },
          plugins: {
            title: {
              display: true,
              text: currentChartView === 'pollen' ? 'Pollen Levels (7 Days)' : 
                    currentChartView === 'air' ? 'Air Quality Index (7 Days)' : 
                    currentChartView === 'uv' ? 'UV Index (7 Days)' :
                    'Pollen Levels & Air Quality (7 Days)'
            },
            legend: {
              position: 'top',
            }
          }
        }
      });
    }

    // Create air quality chart
    function createAirQualityChart(hourlyData) {
      const ctx = document.getElementById('airQualityChart').getContext('2d');
      
      // Destroy existing chart
      if (airQualityChart) {
        airQualityChart.destroy();
      }

      // Prepare data
      const labels = hourlyData.time.slice(0, 24).map(time => {
        const date = new Date(time);
        return date.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' });
      });

      airQualityChart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: labels,
          datasets: [
            {
              label: 'US AQI',
              data: hourlyData.us_aqi ? hourlyData.us_aqi.slice(0, 24) : [],
              borderColor: CHART_COLORS.aqi,
              backgroundColor: CHART_COLORS.aqi + '20',
              tension: 0.4
            },
            {
              label: 'European AQI',
              data: hourlyData.european_aqi ? hourlyData.european_aqi.slice(0, 24) : [],
              borderColor: '#FF5722',
              backgroundColor: '#FF572220',
              tension: 0.4
            },
            {
              label: 'PM2.5 (μg/m³)',
              data: hourlyData.pm2_5 ? hourlyData.pm2_5.slice(0, 24) : [],
              borderColor: CHART_COLORS.pm25,
              backgroundColor: CHART_COLORS.pm25 + '20',
              tension: 0.4,
              yAxisID: 'y1'
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          interaction: {
            intersect: false,
            mode: 'index'
          },
          scales: {
            x: {
              display: true,
              title: {
                display: true,
                text: 'Time'
              }
            },
            y: {
              display: true,
              title: {
                display: true,
                text: 'AQI'
              }
            },
            y1: {
              type: 'linear',
              display: true,
              position: 'right',
              title: {
                display: true,
                text: 'PM2.5 (μg/m³)'
              },
              grid: {
                drawOnChartArea: false,
              },
            }
          },
          plugins: {
            title: {
              display: true,
              text: 'Air Quality Index Trends (24 Hours)'
            },
            legend: {
              position: 'top',
            }
          }
        }
      });
    }

    // Create UV chart
    function createUVChart(hourlyData) {
      const ctx = document.getElementById('uvChart').getContext('2d');
      
      // Destroy existing chart
      if (uvChart) {
        uvChart.destroy();
      }

      // Prepare data
      const labels = hourlyData.time.slice(0, 24).map(time => {
        const date = new Date(time);
        return date.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' });
      });

      uvChart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: labels,
          datasets: [
            {
              label: 'UV Index',
              data: hourlyData.uv_index ? hourlyData.uv_index.slice(0, 24) : [],
              borderColor: CHART_COLORS.uv,
              backgroundColor: CHART_COLORS.uv + '20',
              tension: 0.4
            },
            {
              label: 'Dust (μg/m³)',
              data: hourlyData.dust ? hourlyData.dust.slice(0, 24) : [],
              borderColor: CHART_COLORS.dust,
              backgroundColor: CHART_COLORS.dust + '20',
              tension: 0.4,
              yAxisID: 'y1'
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          interaction: {
            intersect: false,
            mode: 'index'
          },
          scales: {
            x: {
              display: true,
              title: {
                display: true,
                text: 'Time'
              }
            },
            y: {
              type: 'linear',
              display: true,
              position: 'left',
              title: {
                display: true,
                text: 'UV Index'
              }
            },
            y1: {
              type: 'linear',
              display: true,
              position: 'right',
              title: {
                display: true,
                text: 'Dust (μg/m³)'
              },
              grid: {
                drawOnChartArea: false,
              },
            }
          },
          plugins: {
            title: {
              display: true,
              text: 'UV Index & Environmental Data (24 Hours)'
            },
            legend: {
              position: 'top',
            }
          }
        }
      });
    }

    // Update charts
    function updateCharts() {
      if (window.hourlyData) {
        createPollenChart(window.hourlyData);
        createAirQualityChart(window.hourlyData);
        createUVChart(window.hourlyData);
      }
    }

    // Fetch pollen and air quality data from Open-Meteo API
    async function fetchPollenData() {
      try {
        // Show loading state
        document.getElementById("location").textContent = "Loading...";
        document.getElementById("pollen-level").textContent = "Loading...";
        document.getElementById("main-allergen").textContent = "Loading...";
        document.getElementById("aqi").textContent = "Loading...";
        document.getElementById("uv-index").textContent = "Loading...";

        // Clear any existing error messages
        const existingErrors = document.querySelectorAll('.error');
        existingErrors.forEach(error => error.remove());

        // Fetch data from enhanced Open-Meteo API using current location with timezone auto-detection and 7-day forecast
        const response = await fetch(`${API_BASE_URL}?latitude=${currentLat}&longitude=${currentLon}&hourly=pm10,pm2_5,birch_pollen,alder_pollen,grass_pollen,mugwort_pollen,olive_pollen,ragweed_pollen,us_aqi,european_aqi,uv_index,dust&current=european_aqi,us_aqi,birch_pollen,alder_pollen,mugwort_pollen,grass_pollen,olive_pollen,ragweed_pollen,uv_index,pm2_5&timezone=auto&forecast_days=7`);
        
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }

        const data = await response.json();
        
        // Debug: Log the current data to see what's available
        console.log('Current data:', data.current);
        
        // Store hourly data globally for charts
        window.hourlyData = data.hourly;
        
        // Get location name
        const locationName = await getLocationName(currentLat, currentLon);
        
        // Process current data
        const current = data.current;
        
        // Debug: Check if current data exists
        if (!current) {
          console.error('No current data available');
          document.getElementById("pollen-breakdown").innerHTML = '<div class="error">No current data available</div>';
          return;
        }
        
        // Debug: Log the current data structure
        console.log('Current data structure:', Object.keys(current));
        
        const pollenTypes = {
          'Birch': current.birch_pollen || 0,
          'Alder': current.alder_pollen || 0,
          'Grass': current.grass_pollen || 0,
          'Mugwort': current.mugwort_pollen || 0,
          'Olive': current.olive_pollen || 0,
          'Ragweed': current.ragweed_pollen || 0
        };

        // Debug: Log pollen types to see what values we have
        console.log('Pollen types:', pollenTypes);
        
        // Debug: Log individual pollen values for troubleshooting
        console.log('Birch pollen:', current.birch_pollen);
        console.log('Grass pollen:', current.grass_pollen);
        console.log('Mugwort pollen:', current.mugwort_pollen);
        console.log('Alder pollen:', current.alder_pollen);
        console.log('Olive pollen:', current.olive_pollen);
        console.log('Ragweed pollen:', current.ragweed_pollen);
        
        // Check if any pollen data is available
        const hasPollenData = Object.values(pollenTypes).some(value => value > 0);
        if (!hasPollenData) {
          console.log('No significant pollen data detected');
        }

        // Find main allergen (highest pollen count)
        const mainAllergen = Object.entries(pollenTypes).reduce((a, b) => 
          pollenTypes[a[0]] > pollenTypes[b[0]] ? a : b
        );

        // Calculate overall pollen level
        const totalPollen = Object.values(pollenTypes).reduce((sum, value) => sum + (value || 0), 0);
        const overallPollenLevel = getPollenLevelCategory(totalPollen);

        // Get UV index from current data (now available in current object)
        const currentUVIndex = current.uv_index || 0;
        const uvLevel = getUVLevelCategory(currentUVIndex);

        // Update UI with timezone information
        const timezoneInfo = data.timezone ? ` (${data.timezone})` : '';
        const locationText = userLocationDetected ? `${locationName} 🌿 (Your Location)${timezoneInfo}` : `${locationName} 🌿${timezoneInfo}`;
        document.getElementById("location").textContent = locationText;

      const pollenLevelEl = document.getElementById("pollen-level");
        pollenLevelEl.textContent = overallPollenLevel;
        pollenLevelEl.className = getPollenLevelClass(overallPollenLevel);
        
        document.getElementById("main-allergen").textContent = mainAllergen[0];
        document.getElementById("aqi").textContent = `European AQI: ${current.european_aqi} | US AQI: ${current.us_aqi}`;
        
        const uvIndexEl = document.getElementById("uv-index");
        uvIndexEl.textContent = `${currentUVIndex.toFixed(1)} (${uvLevel})`;
        uvIndexEl.className = getUVLevelClass(uvLevel);

        // Update pollen breakdown
        updatePollenBreakdown(pollenTypes);

        // Update air quality details
        updateAirQualityDetails(current);

        // Update forecast with 7-day data
        updateForecast(data.hourly);

        // Create charts
        createPollenChart(data.hourly);
        createAirQualityChart(data.hourly);
        createUVChart(data.hourly);

      } catch (error) {
        console.error('Error fetching pollen data:', error);
        document.getElementById("location").textContent = "Error loading data";
        document.getElementById("pollen-level").textContent = "Error";
        document.getElementById("main-allergen").textContent = "Error";
        document.getElementById("aqi").textContent = "Error";
        document.getElementById("uv-index").textContent = "Error";
        
        // Show error message
        const errorDiv = document.createElement('div');
        errorDiv.className = 'error';
        errorDiv.textContent = `Failed to load data: ${error.message}`;
        document.querySelector('.location-info').appendChild(errorDiv);
      }
    }

    // Update pollen breakdown section
    function updatePollenBreakdown(pollenTypes) {
      const breakdownDiv = document.getElementById("pollen-breakdown");
      breakdownDiv.innerHTML = '';

      // Check if pollenTypes is valid
      if (!pollenTypes || Object.keys(pollenTypes).length === 0) {
        breakdownDiv.innerHTML = '<div class="error">No pollen data available</div>';
        return;
      }

      let hasData = false;
      Object.entries(pollenTypes).forEach(([type, value]) => {
        // Handle null/undefined values
        const pollenValue = value || 0;
        const level = getPollenLevelCategory(pollenValue);
        const levelClass = getPollenLevelClass(level);
        
        const itemDiv = document.createElement('div');
        itemDiv.className = 'pollen-item';
        
        // Always show pollen data, even if it's 0 or low
        const displayValue = pollenValue > 0 ? pollenValue.toFixed(1) : '0.0';
        const displayLevel = pollenValue > 0 ? level : 'None';
        
        itemDiv.innerHTML = `
          <span>${type} Pollen:</span>
          <span class="pollen-value ${levelClass}">${displayValue} grains/m³ (${displayLevel})</span>
        `;
        breakdownDiv.appendChild(itemDiv);
        
        if (pollenValue > 0) {
          hasData = true;
        }
      });

      // If no significant pollen data is available (all values are 0), add a note
      if (!hasData) {
        const noteDiv = document.createElement('div');
        noteDiv.className = 'loading';
        noteDiv.style.marginTop = '10px';
        noteDiv.style.fontStyle = 'italic';
        noteDiv.textContent = 'No significant pollen detected at this time';
        breakdownDiv.appendChild(noteDiv);
      }
    }

    // Update air quality details
    function updateAirQualityDetails(current) {
      const detailsDiv = document.getElementById("air-quality-details");
      const uvLevel = getUVLevelCategory(current.uv_index || 0);
      const uvLevelClass = getUVLevelClass(uvLevel);
      
      detailsDiv.innerHTML = `
        <div class="pollen-item">
          <span>European AQI:</span>
          <span class="pollen-value">${current.european_aqi}</span>
        </div>
        <div class="pollen-item">
          <span>US AQI:</span>
          <span class="pollen-value">${current.us_aqi}</span>
        </div>
        <div class="pollen-item">
          <span>PM2.5:</span>
          <span class="pollen-value">${(current.pm2_5 || 0).toFixed(1)} μg/m³</span>
        </div>
        <div class="pollen-item">
          <span>UV Index:</span>
          <span class="pollen-value ${uvLevelClass}">${(current.uv_index || 0).toFixed(1)} (${uvLevel})</span>
        </div>
        <div class="pollen-item">
          <span>Data Time:</span>
          <span class="pollen-value">${new Date(current.time).toLocaleString()}</span>
        </div>
      `;
    }

    // Update forecast section
    function updateForecast(hourlyData) {
      const forecastList = document.getElementById("forecast-list");
      forecastList.innerHTML = '';

      // Get next 7 days forecast (every 24 hours)
      const days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
      const today = new Date();
      
      for (let i = 1; i <= 7; i++) {
        const forecastDate = new Date(today);
        forecastDate.setDate(today.getDate() + i);
        
        // Get average pollen for the day (using first hour of the day)
        const dayIndex = i * 24;
        const grassPollen = hourlyData.grass_pollen[dayIndex] || 0;
        const birchPollen = hourlyData.birch_pollen[dayIndex] || 0;
        const mugwortPollen = hourlyData.mugwort_pollen[dayIndex] || 0;
        const alderPollen = hourlyData.alder_pollen[dayIndex] || 0;
        const olivePollen = hourlyData.olive_pollen[dayIndex] || 0;
        const ragweedPollen = hourlyData.ragweed_pollen[dayIndex] || 0;
        
        const totalPollen = grassPollen + birchPollen + mugwortPollen + alderPollen + olivePollen + ragweedPollen;
        const level = getPollenLevelCategory(totalPollen);
        
        const li = document.createElement('li');
        li.textContent = `${days[forecastDate.getDay()]} – ${level}`;
        li.className = getPollenLevelClass(level);
        forecastList.appendChild(li);
      }
    }

    // Initialize the app
    document.addEventListener('DOMContentLoaded', function() {
      fetchPollenData();
      
      // Add event listeners for chatbot
      const chatInput = document.getElementById('chat-input');
      const sendBtn = document.getElementById('send-chat-btn');
      
      // Send message on Enter key
      chatInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
          sendMessage();
        }
      });
      
      // Focus on input when page loads
      chatInput.focus();
    });

    // Chatbot functionality
    function sendMessage() {
      const input = document.getElementById('chat-input');
      const message = input.value.trim();
      
      if (!message) return;
      
      // Add user message to chat
      addMessage(message, 'user');
      input.value = '';
      
      // Show typing indicator
      showTypingIndicator();
      
      // Generate AI response
      setTimeout(() => {
        hideTypingIndicator();
        const response = generateAIResponse(message);
        addMessage(response, 'bot');
      }, 1000 + Math.random() * 2000); // Random delay for realistic feel
    }

    function addMessage(text, sender) {
      const chatMessages = document.getElementById('chat-messages');
      const messageDiv = document.createElement('div');
      messageDiv.className = `message ${sender}-message`;
      
      const contentDiv = document.createElement('div');
      contentDiv.className = 'message-content';
      contentDiv.innerHTML = sender === 'user' ? text : `<strong>Bee-Healthy AI:</strong> ${text}`;
      
      messageDiv.appendChild(contentDiv);
      chatMessages.appendChild(messageDiv);
      
      // Scroll to bottom
      chatMessages.scrollTop = chatMessages.scrollHeight;
    }

    function showTypingIndicator() {
      const status = document.getElementById('chat-status');
      status.innerHTML = `
        <div class="typing-indicator">
          AI is typing
          <div class="typing-dots">
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>
      `;
    }

    function hideTypingIndicator() {
      const status = document.getElementById('chat-status');
      status.textContent = '';
    }

    function generateAIResponse(userMessage) {
      const message = userMessage.toLowerCase();
      
      // Pollen and allergy related responses
      if (message.includes('pollen') || message.includes('allergy')) {
        if (message.includes('high') || message.includes('severe')) {
          return "High pollen levels can trigger severe allergy symptoms. Consider staying indoors, using air purifiers, and taking antihistamines as prescribed. Monitor local pollen forecasts and plan outdoor activities when levels are lower.";
        } else if (message.includes('low') || message.includes('mild')) {
          return "Low pollen levels are great news! This is typically the best time for outdoor activities. However, if you're still experiencing symptoms, it might be due to other allergens or indoor triggers.";
        } else if (message.includes('symptom')) {
          return "Common pollen allergy symptoms include sneezing, runny nose, itchy eyes, and congestion. If symptoms persist or worsen, consult with an allergist for proper diagnosis and treatment options.";
        } else {
          return "Pollen allergies can vary by season and location. Spring typically brings tree pollen, summer brings grass pollen, and fall brings ragweed. Monitor your local pollen levels and take precautions accordingly.";
        }
      }
      
      // Health and medication related responses
      if (message.includes('medication') || message.includes('medicine') || message.includes('pill')) {
        return "Always consult with a healthcare provider before taking any medication for allergies. Common treatments include antihistamines, decongestants, and nasal sprays. Some medications may have side effects or interactions.";
      }
      
      if (message.includes('doctor') || message.includes('allergist')) {
        return "If you're experiencing persistent or severe allergy symptoms, it's important to see an allergist. They can perform tests to identify specific allergens and develop a personalized treatment plan for you.";
      }
      
      // Prevention and lifestyle responses
      if (message.includes('prevent') || message.includes('avoid') || message.includes('reduce')) {
        return "To reduce pollen exposure: keep windows closed, use air conditioning, shower after outdoor activities, change clothes, use HEPA filters, and monitor pollen forecasts. Consider wearing a mask during high pollen periods.";
      }
      
      if (message.includes('outdoor') || message.includes('exercise') || message.includes('activity')) {
        return "Exercise is important for health! During high pollen periods, try indoor workouts or exercise early morning/evening when pollen levels are typically lower. Always check current pollen levels before planning outdoor activities.";
      }
      
      // Data interpretation responses
      if (message.includes('data') || message.includes('chart') || message.includes('forecast')) {
        return "The pollen data shows current levels and 7-day forecasts. Green indicates low levels, yellow moderate, and red high. Use this information to plan your day and manage allergy symptoms effectively.";
      }
      
      // General health responses
      if (message.includes('health') || message.includes('wellness')) {
        return "Maintaining good health involves regular exercise, balanced nutrition, adequate sleep, and stress management. For allergy sufferers, this also includes monitoring environmental triggers and following prescribed treatments.";
      }
      
      // Pollen type facts
      if (message.includes('birch')) {
        return "Birch pollen is a pollen found in birch trees, which typically releases pollen in the spring. It is a common allergen in many regions and can cause symptoms like sneezing, runny nose, and itchy eyes.";
      }
      if (message.includes('alder')) {
        return "Alder pollen is a pollen found in alder trees,which like all the other pollens listed on this site are located in North America. It is a common allergen in the spring and can cause allergic reactions .";
      }
      if (message.includes('grass')) {
        return "Grass pollen is a very common allergen found in grass and is a major cause of hay fever, symptoms often peaking in the spring and summer.";
      }
      if (message.includes('mugwort')) {
        return "Mugwort pollen is a weed pollen that can cause allergic reactions, particularly in the late summer and early fall. Some studies suggest that it can also be associated with oral allergy syndrome or OAS.";
      }
      if (message.includes('olive')) {
        return "Olive pollen is produced by olive trees.The pollens contain proteins that can trigger allergic reactions in sensitive individuals, particularly in Mediterranean regions during the spring.";
      }
      if (message.includes('ragweed')) {
        return "Ragweed pollen is a powdery substance released by ragweed plants, they are found in the late summary to early fall.Especially in North America, and can cause severe allergic reactions during the late summer and fall.";
      }
      if (message.includes('manage') || message.includes('managing') || message.includes('control') || message.includes('deal with') || message.includes('treat') && message.includes('pollen')) {
        return "To manage pollen allergies: Monitor pollen counts and limit outdoor activities during peak times. Keep windows closed and use air conditioning or air purifiers. Wear a mask, sunglasses, and a hat outdoors. Shower before bed, wash hair and clothes after being outside, and keep bedding clean. Antihistamines and decongestants can help manage symptoms. Allergy shots (immunotherapy) may be an effective long-term solution. Be aware of potential cross-reactivity between pollens and certain foods, especially for oral allergy syndrome (OAS).";
      }
      
      // Default response for unrecognized queries
      return "I'm here to help with pollen, allergy, and health-related questions. You can ask me about current pollen levels, allergy symptoms, prevention tips, medication advice, or general health information. What would you like to know?";
    }
  </script>
</body>
</html>
