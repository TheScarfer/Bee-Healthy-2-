<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Bee-Healthy Pollen Tracker</title>
  <!-- Chart.js CDN -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <!-- Font Awesome for icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root {
      --primary: #2e7d32;
      --primary-light: #4caf50;
      --primary-dark: #1b5e20;
      --secondary: #2196f3;
      --secondary-dark: #1976d2;
      --danger: #d32f2f;
      --warning: #f57c00;
      --success: #388e3c;
      --text: #333;
      --text-light: #666;
      --bg-light: #f8f9fa;
      --card-bg: #ffffff;
      --shadow: 0 4px 12px rgba(0,0,0,0.1);
      --shadow-hover: 0 8px 24px rgba(0,0,0,0.2);
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, sans-serif;
      background: linear-gradient(to bottom, #01140a, #043362);
      color: var(--text);
      padding: 20px;
      line-height: 1.6;
    }

    .container {
      max-width: 1200px;
      margin: auto;
    }

    header {
      text-align: center;
      margin-bottom: 30px;
    }

    header h1 {
      font-size: 2.8rem;
      margin-bottom: 10px;
      color: var(--primary-light);
      text-shadow: 0 2px 4px rgba(0,0,0,0.2);
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 15px;
    }

    header p {
      color: rgba(255, 255, 255, 0.8);
      font-size: 1.1rem;
      max-width: 600px;
      margin: 0 auto;
    }

    .card {
      background: var(--card-bg);
      padding: 24px;
      border-radius: 16px;
      box-shadow: var(--shadow);
      margin-bottom: 24px;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      position: relative;
      overflow: hidden;
    }

    .card:hover {
      transform: translateY(-5px);
      box-shadow: var(--shadow-hover);
    }

    .card h2 {
      margin-top: 0;
      color: var(--primary);
      display: flex;
      align-items: center;
      gap: 10px;
      font-size: 1.5rem;
    }

    .location-info {
      text-align: center;
      font-size: 1.2rem;
    }

    .stat-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 15px;
      margin: 20px 0;
    }

    .stat-item {
      background: var(--bg-light);
      padding: 15px;
      border-radius: 12px;
      text-align: center;
    }

    .stat-item i {
      font-size: 1.8rem;
      margin-bottom: 10px;
      display: block;
    }

    .stat-value {
      font-size: 1.5rem;
      font-weight: bold;
      margin: 5px 0;
    }

    .stat-label {
      font-size: 0.9rem;
      color: var(--text-light);
    }

    .high {
      color: var(--danger);
      font-weight: bold;
    }

    .moderate {
      color: var(--warning);
      font-weight: bold;
    }

    .low {
      color: var(--success);
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

    li {
      margin-bottom: 8px;
    }

    .chart-container {
      position: relative;
      height: 300px;
      margin: 20px 0;
    }

    .loading {
      color: var(--text-light);
      font-style: italic;
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .loading-spinner {
      width: 20px;
      height: 20px;
      border: 2px solid #f3f3f3;
      border-top: 2px solid var(--primary);
      border-radius: 50%;
      animation: spin 1s linear infinite;
    }

    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    .error {
      color: var(--danger);
      background: #ffebee;
      padding: 12px;
      border-radius: 8px;
      margin: 10px 0;
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .pollen-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px 0;
      border-bottom: 1px solid #eee;
    }

    .pollen-item:last-child {
      border-bottom: none;
    }

    .pollen-value {
      font-weight: bold;
    }

    .btn {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      padding: 10px 18px;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 600;
      transition: all 0.2s ease;
      border: none;
      font-size: 0.95rem;
    }

    .btn:hover {
      transform: translateY(-2px);
    }

    .btn-primary {
      background: var(--primary);
      color: white;
    }

    .btn-primary:hover {
      background: var(--primary-dark);
    }

    .btn-secondary {
      background: var(--secondary);
      color: white;
    }

    .btn-secondary:hover {
      background: var(--secondary-dark);
    }

    .btn:disabled {
      opacity: 0.7;
      cursor: not-allowed;
      transform: none;
    }

    .location-status {
      font-size: 0.9rem;
      margin-top: 10px;
    }

    .location-error {
      color: var(--danger);
      font-size: 0.9rem;
      margin-top: 10px;
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
      padding: 8px 16px;
      border-radius: 6px;
      cursor: pointer;
      font-size: 0.9rem;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .chart-btn:hover {
      background: #e0e0e0;
    }

    .chart-btn.active {
      background-color: var(--secondary);
      color: white;
      border-color: var(--secondary);
    }

    .uv-index {
      display: inline-block;
      padding: 6px 12px;
      border-radius: 20px;
      font-weight: bold;
      color: white;
    }

    .uv-low {
      background-color: var(--success);
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

    /* Notification system */
    .notification {
      position: fixed;
      top: 20px;
      right: 20px;
      padding: 15px 20px;
      border-radius: 8px;
      background: white;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
      display: flex;
      align-items: center;
      gap: 10px;
      z-index: 1000;
      transform: translateX(100%);
      transition: transform 0.3s ease;
    }

    .notification.show {
      transform: translateX(0);
    }

    .notification-warning {
      border-left: 4px solid var(--warning);
    }

    .notification-info {
      border-left: 4px solid var(--secondary);
    }

    /* Modal styles */
    .modal {
      display: none;
      position: fixed;
      z-index: 1000;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0,0,0,0.5);
      align-items: center;
      justify-content: center;
    }

    .modal-content {
      background-color: white;
      padding: 25px;
      border-radius: 12px;
      max-width: 500px;
      width: 90%;
      max-height: 80vh;
      overflow-y: auto;
      box-shadow: 0 5px 25px rgba(0,0,0,0.2);
    }

    .close {
      color: #aaa;
      float: right;
      font-size: 28px;
      font-weight: bold;
      cursor: pointer;
      line-height: 1;
    }

    .close:hover {
      color: black;
    }

    /* Tooltip */
    .tooltip {
      position: relative;
      display: inline-block;
      cursor: pointer;
    }

    .tooltip .tooltiptext {
      visibility: hidden;
      width: 200px;
      background-color: #333;
      color: #fff;
      text-align: center;
      border-radius: 6px;
      padding: 8px;
      position: absolute;
      z-index: 1;
      bottom: 125%;
      left: 50%;
      transform: translateX(-50%);
      opacity: 0;
      transition: opacity 0.3s;
      font-size: 0.85rem;
      font-weight: normal;
    }

    .tooltip:hover .tooltiptext {
      visibility: visible;
      opacity: 1;
    }

    /* Responsive adjustments */
    @media (max-width: 768px) {
      body {
        padding: 15px;
      }
      
      header h1 {
        font-size: 2.2rem;
      }
      
      .card {
        padding: 18px;
      }
      
      .stat-grid {
        grid-template-columns: 1fr 1fr;
      }
      
      .two-column .card {
        min-width: 100%;
      }
    }

    @media (max-width: 480px) {
      .stat-grid {
        grid-template-columns: 1fr;
      }
      
      .chart-controls {
        flex-direction: column;
        align-items: stretch;
      }
      
      .chart-btn {
        justify-content: center;
      }
    }

    /* Print styles */
    @media print {
      body {
        background: white;
        color: black;
        padding: 0;
      }
      
      .card {
        box-shadow: none;
        border: 1px solid #ddd;
        page-break-inside: avoid;
      }
      
      .btn, .chart-controls {
        display: none;
      }
    }

    /* Animation for cards */
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }
    
    .card {
      animation: fadeIn 0.5s ease forwards;
    }
    
    .card:nth-child(1) { animation-delay: 0.1s; }
    .card:nth-child(2) { animation-delay: 0.2s; }
    .card:nth-child(3) { animation-delay: 0.3s; }
    .card:nth-child(4) { animation-delay: 0.4s; }
    .card:nth-child(5) { animation-delay: 0.5s; }

    /* Badges for status indicators */
    .badge {
      display: inline-block;
      padding: 4px 8px;
      border-radius: 20px;
      font-size: 0.75rem;
      font-weight: bold;
      margin-left: 8px;
    }
    
    .badge-info {
      background: #e3f2fd;
      color: var(--secondary);
    }
    
    .badge-success {
      background: #e8f5e9;
      color: var(--success);
    }
    
    .badge-warning {
      background: #fff3e0;
      color: var(--warning);
    }
    
    .badge-danger {
      background: #ffebee;
      color: var(--danger);
    }

    /* Footer */
    footer {
      text-align: center;
      margin-top: 40px;
      padding: 20px;
      color: rgba(255, 255, 255, 0.7);
      font-size: 0.9rem;
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1><i class="fas fa-bee"></i> Bee-Healthy</h1>
      <p>Track pollen levels, air quality, and UV index to manage your allergies effectively</p>
    </header>

    <!-- Notification Area -->
    <div id="notification" class="notification">
      <i class="fas fa-info-circle"></i>
      <div id="notification-text"></div>
    </div>

    <section class="card location-info">
      <h2><i class="fas fa-map-marker-alt"></i> Location Information</h2>
      <p>📍 <strong id="location">Loading...</strong></p>
      
      <div class="stat-grid">
        <div class="stat-item">
          <i class="fas fa-seedling" style="color: #4CAF50;"></i>
          <div class="stat-value" id="pollen-level">Loading...</div>
          <div class="stat-label">Pollen Level</div>
        </div>
        
        <div class="stat-item">
          <i class="fas fa-wind" style="color: #2196F3;"></i>
          <div class="stat-value" id="aqi">Loading...</div>
          <div class="stat-label">Air Quality</div>
        </div>
        
        <div class="stat-item">
          <i class="fas fa-sun" style="color: #FF9800;"></i>
          <div class="stat-value" id="uv-index">Loading...</div>
          <div class="stat-label">UV Index</div>
        </div>
        
        <div class="stat-item">
          <i class="fas fa-temperature-high" style="color: #F44336;"></i>
          <div class="stat-value" id="temperature">Loading...</div>
          <div class="stat-label">Temperature</div>
        </div>
      </div>
      
      <p>🌱 Main Allergen: <strong id="main-allergen">Loading...</strong></p>
      
      <div style="margin-top: 20px;">
        <button class="btn btn-primary" onclick="fetchPollenData()">
          <i class="fas fa-sync-alt"></i> Refresh Data
        </button>
        <button class="btn btn-secondary" id="location-btn" onclick="detectUserLocation()">
          <i class="fas fa-location-arrow"></i> Use My Location
        </button>
        <button class="btn" onclick="showHelp()" style="background: #f5f5f5;">
          <i class="fas fa-question-circle"></i> Help
        </button>
      </div>
      <div id="location-status" class="location-status"></div>
    </section>

    <div class="two-column">
      <section class="card">
        <h2><i class="fas fa-calendar-alt"></i> 7-Day Forecast</h2>
        <ul id="forecast-list">
          <li class="loading"><span class="loading-spinner"></span> Loading forecast...</li>
        </ul>
      </section>

      <section class="card">
        <h2><i class="fas fa-lightbulb"></i> Allergy Tips</h2>
        <ul>
          <li>Wear a mask outdoors when pollen levels are high</li>
          <li>Keep windows closed during high pollen seasons</li>
          <li>Change clothes after being outside for extended periods</li>
          <li>Use air purifiers with HEPA filters indoors</li>
          <li>Check pollen levels before planning outdoor activities</li>
          <li>Apply sunscreen when UV index is 3 or higher</li>
          <li>Shower before bed to remove pollen from hair and skin</li>
          <li>Consider allergy medication before symptoms begin</li>
        </ul>
      </section>
    </div>

    <div class="two-column">
      <section class="card">
        <h2><i class="fas fa-microscope"></i> Current Pollen Breakdown</h2>
        <div id="pollen-breakdown">
          <div class="loading"><span class="loading-spinner"></span> Loading pollen data...</div>
        </div>
      </section>

      <section class="card">
        <h2><i class="fas fa-wind"></i> Air Quality Details</h2>
        <div id="air-quality-details">
          <div class="loading"><span class="loading-spinner"></span> Loading air quality data...</div>
        </div>
      </section>
    </div>

    <!-- Pollen Levels Chart -->
    <section class="card full-width">
      <h2><i class="fas fa-chart-line"></i> Pollen Levels Over Time</h2>
      <div class="chart-controls">
        <button class="chart-btn active" onclick="switchChartView('pollen')">
          <i class="fas fa-seedling"></i> Pollen Levels
        </button>
        <button class="chart-btn" onclick="switchChartView('air')">
          <i class="fas fa-wind"></i> Air Quality
        </button>
        <button class="chart-btn" onclick="switchChartView('combined')">
          <i class="fas fa-layer-group"></i> Combined View
        </button>
        <button class="chart-btn" onclick="switchChartView('uv')">
          <i class="fas fa-sun"></i> UV Index
        </button>
        <select class="chart-btn" onchange="changeChartRange(this.value)" style="margin-left: auto;">
          <option value="24">Last 24 Hours</option>
          <option value="72" selected>Last 3 Days</option>
          <option value="168">Last Week</option>
        </select>
      </div>
      <div class="chart-container">
        <canvas id="pollenChart"></canvas>
      </div>
    </section>

    <!-- Pollen Calendar Section -->
    <section class="card full-width">
      <h2><i class="fas fa-calendar-day"></i> Pollen Season Calendar</h2>
      <div id="pollen-calendar">
        <div class="loading"><span class="loading-spinner"></span> Loading pollen calendar...</div>
      </div>
    </section>

    <!-- Action Buttons -->
    <div class="two-column">
      <section class="card">
        <h2><i class="fas fa-bell"></i> Notifications</h2>
        <p>Get alerts when pollen levels are high in your area.</p>
        <div>
          <label>
            <input type="checkbox" id="notify-high-pollen"> Notify me when pollen is high
          </label>
        </div>
        <div style="margin-top: 10px;">
          <button class="btn btn-primary" onclick="testNotification()">
            <i class="fas fa-bell"></i> Test Notification
          </button>
        </div>
      </section>

      <section class="card">
        <h2><i class="fas fa-share-alt"></i> Share Report</h2>
        <p>Share current conditions with others.</p>
        <div>
          <button class="btn" onclick="shareReport()" style="background: #4267B2; color: white;">
            <i class="fab fa-facebook"></i> Facebook
          </button>
          <button class="btn" onclick="shareReport()" style="background: #1DA1F2; color: white;">
            <i class="fab fa-twitter"></i> Twitter
          </button>
          <button class="btn" onclick="printReport()">
            <i class="fas fa-print"></i> Print Report
          </button>
        </div>
      </section>
    </div>

    <footer>
      <p>Bee-Healthy Pollen Tracker &copy; 2023 | Data provided by Open-Meteo</p>
      <p>This information is for educational purposes only and should not replace professional medical advice.</p>
    </footer>
  </div>

  <!-- Help Modal -->
  <div id="helpModal" class="modal">
    <div class="modal-content">
      <span class="close" onclick="closeHelp()">&times;</span>
      <h2><i class="fas fa-question-circle"></i> Bee-Healthy Help Guide</h2>
      <h3>Understanding Pollen Levels</h3>
      <p>Pollen levels are measured in grains per cubic meter of air:</p>
      <ul>
        <li><span class="low">Low</span>: 0-9 grains/m³ - Minimal impact for most people</li>
        <li><span class="moderate">Moderate</span>: 10-49 grains/m³ - Symptoms possible for sensitive individuals</li>
        <<li><span class="high">High</span>: 50-499 grains/m³ - Symptoms likely for sensitive individuals</li>
        <li><span class="high">Very High</span>: 500+ grains/m³ - Symptoms affect most sensitive individuals</li>
      </ul>
      
      <h3>Using the Charts</h3>
      <p>Switch between different data views using the chart controls. Hover over data points to see exact values.</p>
      
      <h3>Location Services</h3>
      <p>For the most accurate data, allow location access. Your data is not stored on our servers.</p>
      
      <button class="btn btn-primary" onclick="closeHelp()">Got it!</button>
    </div>
  </div>

  <!-- Pollen Info Modal -->
  <div id="pollenInfoModal" class="modal">
    <div class="modal-content">
      <span class="close" onclick="hidePollenInfo()">&times;</span>
      <h2 id="pollenInfoTitle"></h2>
      <div id="pollenInfoText"></div>
      <div id="pollenSeasonInfo" style="margin-top: 15px; padding: 10px; background: #f9f9f9; border-radius: 8px;"></div>
    </div>
  </div>

  <script>
    // Open-Meteo Air Quality API configuration
    const API_BASE_URL = 'https://air-quality-api.open-meteo.com/v1/air-quality';
    const WEATHER_API_URL = 'https://api.open-meteo.com/v1/forecast';
    const DEFAULT_LAT = 52.52;
    const DEFAULT_LON = 13.41;

    // Global variables for current location
    let currentLat = DEFAULT_LAT;
    let currentLon = DEFAULT_LON;
    let userLocationDetected = false;
    let chartRange = 72; // Default to 3 days

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

    // Pollen season information
    const POLLEN_SEASONS = {
      'Birch': { season: 'Spring', months: 'March to May', peak: 'April' },
      'Alder': { season: 'Early Spring', months: 'February to April', peak: 'March' },
      'Grass': { season: 'Late Spring to Summer', months: 'May to July', peak: 'June' },
      'Mugwort': { season: 'Late Summer', months: 'August to September', peak: 'Late August' },
      'Olive': { season: 'Spring', months: 'April to June', peak: 'May' },
      'Ragweed': { season: 'Late Summer to Fall', months: 'August to October', peak: 'September' }
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
      carbon: '#607D8B',
      temperature: '#F44336',
      humidity: '#03A9F4'
    };

    // Initialize the app
    document.addEventListener('DOMContentLoaded', function() {
      // Set up close button event after DOM is ready
      document.getElementById('closePollenInfoModal').onclick = function() {
        document.getElementById('pollenInfoModal').style.display = 'none';
      };

      // Load saved location preference
      const savedLat = localStorage.getItem('beeHealthyLat');
      const savedLon = localStorage.getItem('beeHealthyLon');
      
      if (savedLat && savedLon) {
        currentLat = parseFloat(savedLat);
        currentLon = parseFloat(savedLon);
        userLocationDetected = true;
        fetchPollenData();
      } else {
        // Try to detect location immediately on load
        detectUserLocation();
      }
      
      // Initialize pollen calendar
      renderPollenCalendar();
    });

    // Show notification
    function showNotification(message, type = 'info') {
      const notification = document.getElementById('notification');
      const notificationText = document.getElementById('notification-text');
      
      notification.className = 'notification';
      notification.classList.add(`notification-${type}`);
      notificationText.textContent = message;
      
      notification.classList.add('show');
      
      // Auto hide after 5 seconds
      setTimeout(() => {
        notification.classList.remove('show');
      }, 5000);
    }

    // Test notification
    function testNotification() {
      showNotification('This is a test notification. You will be alerted when pollen levels are high.', 'info');
    }

    // Show help modal
    function showHelp() {
      document.getElementById('helpModal').style.display = 'flex';
    }

    // Close help modal
    function closeHelp() {
      document.getElementById('helpModal').style.display = 'none';
    }

    // Print report
    function printReport() {
      window.print();
    }

    // Share report
    function shareReport() {
      const location = document.getElementById('location').textContent;
      const pollenLevel = document.getElementById('pollen-level').textContent;
      
      if (navigator.share) {
        navigator.share({
          title: 'Bee-Healthy Pollen Report',
          text: `Current pollen levels in ${location}: ${pollenLevel}`,
          url: window.location.href
        })
        .catch(error => {
          showNotification('Sharing failed. Please try again.', 'info');
        });
      } else {
        showNotification('Web Share API not supported in your browser.', 'info');
      }
    }

    // Change chart range
    function changeChartRange(range) {
      chartRange = parseInt(range);
      updateCharts();
    }

    // Render pollen calendar
    function renderPollenCalendar() {
      const calendarDiv = document.getElementById('pollen-calendar');
      let html = '<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 15px;">';
      
      for (const [pollenType, info] of Object.entries(POLLEN_SEASONS)) {
        html += `
          <div style="padding: 12px; border-radius: 8px; background: #f9f9f9;">
            <h3 style="margin: 0 0 8px 0;">${pollenType}</h3>
            <p style="margin: 4px 0;"><strong>Season:</strong> ${info.season}</p>
            <p style="margin: 4px 0;"><strong>Months:</strong> ${info.months}</p>
            <p style="margin: 4px 0;"><strong>Peak:</strong> ${info.peak}</p>
          </div>
        `;
      }
      
      html += '</div>';
      calendarDiv.innerHTML = html;
    }

    // ... (rest of your existing JavaScript code with the enhancements integrated) ...
    // Note: I've kept the core functionality from your original code but added the new features
    // The complete code would be too long to include here, but the structure shows how to integrate

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
      locationBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Detecting...';
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
          
          // Save to local storage
          localStorage.setItem('beeHealthyLat', lat);
          localStorage.setItem('beeHealthyLon', lon);
          
          locationStatus.textContent = `Location detected! (${lat.toFixed(4)}, ${lon.toFixed(4)})`;
          locationStatus.className = 'location-status';
          
          // Fetch data for new location
          await fetchPollenData();
          
          // Re-enable button
          locationBtn.disabled = false;
          locationBtn.innerHTML = '<i class="fas fa-location-arrow"></i> Use My Location';
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
      locationBtn.innerHTML = '<i class="fas fa-location-arrow"></i> Use My Location';
    }

    // Switch chart view
    function switchChartView(view) {
      currentChartView = view;
      
      // Update button states
      document.querySelectorAll('.chart-btn').forEach(btn => {
        if (btn.nodeName === 'BUTTON') {
          btn.classList.remove('active');
        }
      });
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

      // Prepare data - show selected range of data
      const dataPoints = Math.min(chartRange, hourlyData.time.length);
      const labels = hourlyData.time.slice(0, dataPoints).map(time => {
        const date = new Date(time);
        if (chartRange <= 24) {
          return date.toLocaleTimeString('en-US', { 
            hour: '2-digit',
            minute: '2-digit'
          });
        } else {
          return date.toLocaleDateString('en-US', { 
            month: 'short', 
            day: 'numeric',
            hour: '2-digit'
          });
        }
      });

      const datasets = [];
      
      if (currentChartView === 'pollen' || currentChartView === 'combined') {
        datasets.push({
          label: 'Birch Pollen',
          data: hourlyData.birch_pollen.slice(0, dataPoints),
          borderColor: CHART_COLORS.birch,
          backgroundColor: CHART_COLORS.birch + '20',
          tension: 0.4,
          borderWidth: 2
        });
        datasets.push({
          label: 'Grass Pollen',
          data: hourlyData.grass_pollen.slice(0, dataPoints),
          borderColor: CHART_COLORS.grass,
          backgroundColor: CHART_COLORS.grass + '20',
          tension: 0.4,
          borderWidth: 2
        });
        datasets.push({
          label: 'Mugwort Pollen',
          data: hourlyData.mugwort_pollen.slice(0, dataPoints),
          borderColor: CHART_COLORS.mugwort,
          backgroundColor: CHART_COLORS.mugwort + '20',
          tension: 0.4,
          borderWidth: 2
        });
      }

      if (currentChartView === 'air' || currentChartView === 'combined') {
        datasets.push({
          label: 'US AQI',
          data: hourlyData.us_aqi ? hourlyData.us_aqi.slice(0, dataPoints) : [],
          borderColor: CHART_COLORS.aqi,
          backgroundColor: CHART_COLORS.aqi + '20',
          tension: 0.4,
          borderWidth: 2,
          yAxisID: 'y1'
        });
      }

      if (currentChartView === 'uv') {
        datasets.push({
          label: 'UV Index',
          data: hourlyData.uv_index ? hourlyData.uv_index.slice(0, dataPoints) : [],
          borderColor: CHART_COLORS.uv,
          backgroundColor: CHART_COLORS.uv + '20',
          tension: 0.4,
          borderWidth: 2
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
                text: chartRange <= 24 ? 'Time of Day' : 'Date'
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
              text: currentChartView === 'pollen' ? `Pollen Levels (${chartRange <= 24 ? '24 Hours' : chartRange === 72 ? '3 Days' : '7 Days'})` : 
                    currentChartView === 'air' ? `Air Quality Index (${chartRange <= 24 ? '24 Hours' : chartRange === 72 ? '3 Days' : '7 Days'})` : 
                    currentChartView === 'uv' ? `UV Index (${chartRange <= 24 ? '24 Hours' : chartRange === 72 ? '3 Days' : '7 Days'})` :
                    `Pollen Levels & Air Quality (${chartRange <= 24 ? '24 Hours' : chartRange === 72 ? '3 Days' : '7 Days'})`
            },
            legend: {
              position: 'top',
            },
            tooltip: {
              mode: 'index',
              intersect: false
            }
          }
        }
      });
    }

    // Fetch pollen and air quality data from Open-Meteo API
    async function fetchPollenData() {
      try {
        // Show loading state
        document.getElementById("location").innerHTML = '<span class="loading-spinner"></span> Loading...';
        document.getElementById("pollen-level").innerHTML = '<span class="loading-spinner"></span> Loading...';
        document.getElementById("main-allergen").innerHTML = '<span class="loading-spinner"></span> Loading...';
        document.getElementById("aqi").innerHTML = '<span class="loading-spinner"></span> Loading...';
        document.getElementById("uv-index").innerHTML = '<span class="loading-spinner"></span> Loading...';
        document.getElementById("temperature").innerHTML = '<span class="loading-spinner"></span> Loading...';

        // Clear any existing error messages
        const existingErrors = document.querySelectorAll('.error');
        existingErrors.forEach(error => error.remove());

        // Fetch air quality data
        const aqResponse = await fetch(`${API_BASE_URL}?latitude=${currentLat}&longitude=${currentLon}&hourly=pm10,pm2_5,birch_pollen,alder_pollen,grass_pollen,mugwort_pollen,olive_pollen,ragweed_pollen,us_aqi,european_aqi,uv_index,dust&current=european_aqi,us_aqi,birch_pollen,alder_pollen,mugwort_pollen,grass_pollen,olive_pollen,ragweed_pollen,uv_index,pm2_5&timezone=auto&forecast_days=7`);
        
        if (!aqResponse.ok) {
          throw new Error(`Air quality API error! status: ${aqResponse.status}`);
        }

        const aqData = await aqResponse.json();
        
        // Fetch weather data
        const weatherResponse = await fetch(`${WEATHER_API_URL}?latitude=${currentLat}&longitude=${currentLon}&current=temperature_2m,relative_humidity_2m&hourly=temperature_2m,relative_humidity_2m&timezone=auto&forecast_days=1`);
        
        if (!weatherResponse.ok) {
          throw new Error(`Weather API error! status: ${weatherResponse.status}`);
        }

        const weatherData = await weatherResponse.json();
        
        // Combine data
        const combinedData = {
          ...aqData,
          weather: weatherData
        };
        
        // Store hourly data globally for charts
        window.hourlyData = combinedData.hourly;
        
        // Get location name
        const locationName = await getLocationName(currentLat, currentLon);
        
        // Process current data
        const current = combinedData.current;
        
        // Debug: Check if current data exists
        if (!current) {
          console.error('No current data available');
          document.getElementById("pollen-breakdown").innerHTML = '<div class="error">No current data available</div>';
          return;
        }
        
        const pollenTypes = {
          'Birch': current.birch_pollen || 0,
          'Alder': current.alder_pollen || 0,
          'Grass': current.grass_pollen || 0,
          'Mugwort': current.mugwort_pollen || 0,
          'Olive': current.olive_pollen || 0,
          'Ragweed': current.ragweed_pollen || 0
        };

        // Check if any pollen data is available
        const hasPollenData = Object.values(pollenTypes).some(value => value > 0);

        // Find main allergen (highest pollen count)
        const mainAllergen = Object.entries(pollenTypes).reduce((a, b) => 
          pollenTypes[a[0]] > pollenTypes[b[0]] ? a : b
        );

        // Calculate overall pollen level
        const totalPollen = Object.values(pollenTypes).reduce((sum, value) => sum + (value || 0), 0);
        const overallPollenLevel = getPollenLevelCategory(totalPollen);

        // Get UV index from current data
        const currentUVIndex = current.uv_index || 0;
        const uvLevel = getUVLevelCategory(currentUVIndex);

        // Get temperature from weather data
        const currentTemp = combinedData.weather.current ? combinedData.weather.current.temperature_2m : null;

        // Update UI with timezone information
        const timezoneInfo = combinedData.timezone ? ` (${combinedData.timezone})` : '';
        const locationText = userLocationDetected ? `${locationName} 🌿 (Your Location)${timezoneInfo}` : `${locationName} 🌿${timezoneInfo}`;
        document.getElementById("location").textContent = locationText;

        const pollenLevelEl = document.getElementById("pollen-level");
        pollenLevelEl.textContent = overallPollenLevel;
        pollenLevelEl.className = getPollenLevelClass(overallPollenLevel);
        
        document.getElementById("main-allergen").textContent = mainAllergen[0];
        document.getElementById("aqi").textContent = `EU: ${current.european_aqi} | US: ${current.us_aqi}`;
        
        const uvIndexEl = document.getElementById("uv-index");
        uvIndexEl.textContent = `${currentUVIndex.toFixed(1)} (${uvLevel})`;
        uvIndexEl.className = getUVLevelClass(uvLevel);
        
        if (currentTemp) {
          document.getElementById("temperature").textContent = `${currentTemp}°C`;
        }

        // Update pollen breakdown
        updatePollenBreakdown(pollenTypes);

        // Update air quality details
        updateAirQualityDetails(current, combinedData.weather.current);

        // Update forecast with 7-day data
        updateForecast(combinedData.hourly);

        // Create charts
        createPollenChart(combinedData.hourly);

        // Show notification if pollen is high
        if (overallPollenLevel === 'High' || overallPollenLevel === 'Very High') {
          showNotification(`High pollen alert! Current level: ${overallPollenLevel}. Take precautions if you have allergies.`, 'warning');
        }

      } catch (error) {
        console.error('Error fetching data:', error);
        document.getElementById("location").textContent = "Error loading data";
        document.getElementById("pollen-level").textContent = "Error";
        document.getElementById("main-allergen").textContent = "Error";
        document.getElementById("aqi").textContent = "Error";
        document.getElementById("uv-index").textContent = "Error";
        document.getElementById("temperature").textContent = "Error";
        
        // Show error message
        const errorDiv = document.createElement('div');
        errorDiv.className = 'error';
        errorDiv.innerHTML = `<i class="fas fa-exclamation-triangle"></i> Failed to load data: ${error.message}`;
        document.querySelector('.location-info').appendChild(errorDiv);
      }
    }

    // Update pollen breakdown section
    function updatePollenBreakdown(pollenTypes) {
      const breakdownDiv = document.getElementById("pollen-breakdown");
      breakdownDiv.innerHTML = '';

      if (!pollenTypes || Object.keys(pollenTypes).length === 0) {
        breakdownDiv.innerHTML = '<div class="error"><i class="fas fa-exclamation-circle"></i> No pollen data available</div>';
        return;
      }

      let hasData = false;
      Object.entries(pollenTypes).forEach(([type, value]) => {
        const pollenValue = value || 0;
        const level = getPollenLevelCategory(pollenValue);
        const levelClass = getPollenLevelClass(level);

        const displayValue = pollenValue > 0 ? pollenValue.toFixed(1) : '0.0';
        const displayLevel = pollenValue > 0 ? level : 'None';

        const itemDiv = document.createElement('div');
        itemDiv.className = 'pollen-item';

        itemDiv.innerHTML = `
          <span>
            ${type} Pollen:
            <button onclick="showPollenInfo('${type}')" style="margin-left:6px; background:none; border:none; cursor:pointer; font-size:1.1em;" aria-label="More info about ${type} pollen">ℹ️</button>
          </span>
          <span class="pollen-value ${levelClass}">${displayValue} grains/m³ 
            <span class="badge ${levelClass}">${displayLevel}</span>
          </span>
        `;
        breakdownDiv.appendChild(itemDiv);

        if (pollenValue > 0) {
          hasData = true;
        }
      });

      if (!hasData) {
        const noteDiv = document.createElement('div');
        noteDiv.className = 'loading';
        noteDiv.style.marginTop = '10px';
        noteDiv.style.fontStyle = 'italic';
        noteDiv.innerHTML = '<i class="fas fa-check"></i> No significant pollen detected at this time';
        breakdownDiv.appendChild(noteDiv);
      }
    }

    // Update air quality details
    function updateAirQualityDetails(current, weatherCurrent) {
      const detailsDiv = document.getElementById("air-quality-details");
      const uvLevel = getUVLevelCategory(current.uv_index || 0);
      const uvLevelClass = getUVLevelClass(uvLevel);
      
      let html = `
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
      `;
      
      if (weatherCurrent) {
        html += `
          <div class="pollen-item">
            <span>Temperature:</span>
            <span class="pollen-value">${weatherCurrent.temperature_2m}°C</span>
          </div>
          <div class="pollen-item">
            <span>Humidity:</span>
            <span class="pollen-value">${weatherCurrent.relative_humidity_2m}%</span>
          </div>
        `;
      }
      
      html += `
        <div class="pollen-item">
          <span>Data Time:</span>
          <span class="pollen-value">${new Date(current.time).toLocaleString()}</span>
        </div>
      `;
      
      detailsDiv.innerHTML = html;
    }

    // Update forecast section i think???
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
        const levelClass = getPollenLevelClass(level);
        
        const li = document.createElement('li');
        li.innerHTML = `${days[forecastDate.getDay()]} – <span class="${levelClass}">${level}</span>`;
        forecastList.appendChild(li);
      }
    }

    // Show pollen info modal
    function showPollenInfo(type) {
      document.getElementById('pollenInfoTitle').textContent = type + " Pollen";
      document.getElementById('pollenInfoText').textContent = POLLEN_INFO[type] || "No info available.";
      
      // Add season information
      const seasonInfo = POLLEN_SEASONS[type];
      let seasonHtml = "<strong>Seasonal Information:</strong><br>";
      
      if (seasonInfo) {
        seasonHtml += `
          Season: ${seasonInfo.season}<br>
          Months: ${seasonInfo.months}<br>
          Peak: ${seasonInfo.peak}
        `;
      } else {
        seasonHtml += "No seasonal data available.";
      }
      
      document.getElementById('pollenSeasonInfo').innerHTML = seasonHtml;
      document.getElementById('pollenInfoModal').style.display = 'flex';
    }

    // Hide pollen info modal
    function hidePollenInfo() {
      document.getElementById('pollenInfoModal').style.display = 'none';
    }

    // Update charts
    function updateCharts() {
      if (window.hourlyData) {
        createPollenChart(window.hourlyData);
      }
    }

    // Pollen information
    const POLLEN_INFO = {
      'Birch': "Birch pollen is a common allergen in many regions, typically released in the spring. It can cause symptoms like sneezing, runny nose, and itchy eyes. Those with birch pollen allergies may also experience cross-reactivity with certain foods like apples, carrots, and almonds.",
      'Alder': "Alder pollen is released by alder trees, primarily in early spring. It's a significant allergen in many parts of the world and can trigger hay fever symptoms. Alder trees are often found near water sources like rivers and streams.",
      'Grass': "Grass pollen is one of the most common causes of hay fever, with symptoms peaking in late spring and summer. There are many types of grass that produce pollen, and levels are typically highest on warm, dry days with mild winds.",
      'Mugwort': "Mugwort pollen is released by the mugwort plant, a weed that blooms in late summer. It's a common allergen in many regions and can cause severe allergic reactions. Mugwort pollen counts are highest in rural areas.",
      'Olive': "Olive pollen is prevalent in Mediterranean regions where olive trees are cultivated. The pollination season typically occurs in late spring. Olive pollen can cause significant allergic reactions in sensitive individuals.",
      'Ragweed': "Ragweed pollen is a major cause of seasonal allergies in North America, particularly in the fall. A single ragweed plant can produce up to a billion grains of pollen per season, which can travel hundreds of miles on the wind."
    };
  </script>
</body>
</html>
