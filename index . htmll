<!DOCTYPE html>
<html lang="si">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tower Map</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <style>
        /* සිතියම සම්පූර්ණ තිරයේ පෙන්වීමට */
        body, html { margin: 0; padding: 0; height: 100%; font-family: sans-serif; }
        #map { height: 100vh; width: 100%; }

        /* UI කොටස් වල ස්ටයිල් */
        .controls { position: absolute; top: 10px; left: 50%; transform: translateX(-50%); z-index: 1000; background: white; padding: 10px; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.2); }
        .social-buttons { position: absolute; bottom: 20px; left: 10px; z-index: 1000; }
        .social-btn { display: inline-block; padding: 10px; margin-right: 5px; border-radius: 5px; text-decoration: none; color: white; font-weight: bold; }
        .whatsapp { background: #25D366; }
        .facebook { background: #1877F2; }
        
        #adminPanel { 
            display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); 
            z-index: 2000; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 0 20px rgba(0,0,0,0.3); width: 280px;
        }
        .admin-input { width: 100%; margin-bottom: 10px; padding: 8px; box-sizing: border-box; }
        .btn-save { width: 100%; padding: 10px; background: #1a73e8; color: white; border: none; cursor: pointer; }
        
        .loc-button { position: absolute; bottom: 80px; right: 20px; z-index: 1000; background: white; border: none; padding: 10px; border-radius: 50%; font-size: 20px; cursor: pointer; box-shadow: 0 2px 5px rgba(0,0,0,0.3); }
        .secret-admin { position: absolute; bottom: 20px; right: 20px; z-index: 1000; background: transparent; border: none; color: #ccc; cursor: pointer; }
    </style>
</head>
<body>

    <div class="controls">
        <select id="networkSelect" onchange="updateDisplay()" style="padding: 8px; border-radius: 5px;">
            <option value="">ජාලය තෝරන්න (Select Network)</option>
            <option value="Hutch">Hutch</option>
            <option value="Mobitel">Mobitel</option>
            <option value="Dialog">Dialog</option>
        </select>
    </div>

    <div class="social-buttons">
        <a href="https://wa.me/94765825288" target="_blank" class="social-btn whatsapp">WA</a>
        <a href="https://www.facebook.com/share/1CMKzHNNiM/" target="_blank" class="social-btn facebook">FB</a>
    </div>

    <div id="adminPanel">
        <h3 style="margin:0 0 15px 0; color:#1a73e8; text-align:center;">Tower එකතු කරන්න</h3>
        <select id="adminNetSelect" class="admin-input">
            <option value="Hutch">Hutch</option>
            <option value="Mobitel">Mobitel</option>
            <option value="Dialog">Dialog</option>
        </select>
        <input type="text" id="towerName" class="admin-input" placeholder="Tower නම">
        <input type="number" id="towerLat" class="admin-input" step="any" placeholder="Latitude">
        <input type="number" id="towerLng" class="admin-input" step="any" placeholder="Longitude">
        <button class="btn-save" onclick="saveTower()">සේව් කරන්න</button>
        <button onclick="closeAdmin()" style="width:100%; border:none; background:none; margin-top:10px; color:#666; cursor:pointer;">වහන්න</button>
    </div>

    <div id="map"></div>

    <button class="loc-button" onclick="findMyLocation()">📍</button>
    <button class="secret-admin" onclick="openAdmin()">•</button>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        var publicTowers = { 
            "Mobitel": [
                { name: "", lat: 6.665369, lng: 79.929752 }, { name: "", lat: 6.665151, lng: 79.939237 }, 
                { name: "", lat: 6.675131, lng: 79.927113 }, { name: "", lat: 6.684259, lng: 79.919874 },
                { name: "New Mobitel Tower", lat: 6.579512, lng: 79.963490 }
            ], 
            "Dialog": [
                { name: "", lat: 6.665151, lng: 79.939237 }, { name: "Tower 2", lat: 6.563399, lng: 79.983893 },
                { name: "New Dialog Tower", lat: 6.584806, lng: 79.960492 }
            ], 
            "Hutch": [
                { name: "", lat: 6.665151, lng: 79.939237 }, { name: "Tower 1", lat: 6.561812, lng: 79.969762 }
            ] 
        };

        var localData = JSON.parse(localStorage.getItem('myTowers')) || { "Hutch": [], "Mobitel": [], "Dialog": [] };
        var myMap = L.map('map', { zoomControl: false }).setView([6.68, 79.92], 12);
        
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '© OpenStreetMap contributors'
        }).addTo(myMap);

        var markerGroup = L.layerGroup().addTo(myMap);

        function updateDisplay() {
            var net = document.getElementById('networkSelect').value;
            markerGroup.clearLayers();
            if (!net) return;

            var allForNet = publicTowers[net].concat(localData[net]);
            var bounds = [];

            allForNet.forEach(function(tower) {
                L.marker([tower.lat, tower.lng])
                    .bindPopup("<b>" + net + " Tower</b><br>" + (tower.name || "නමක් නැත"))
                    .addTo(markerGroup);
                bounds.push([tower.lat, tower.lng]);
            });

            if(bounds.length > 0) {
                myMap.fitBounds(bounds, { padding: [50, 50] });
            }
        }

        function openAdmin() { 
            if (prompt("Password:") === "20101027") {
                document.getElementById('adminPanel').style.display = 'block';
            } else {
                alert("වැරදි මුරපදයක්!");
            }
        }

        function closeAdmin() { document.getElementById('adminPanel').style.display = 'none'; }

        function saveTower() {
            var net = document.getElementById('adminNetSelect').value;
            var name = document.getElementById('towerName').value;
            var lat = parseFloat(document.getElementById('towerLat').value);
            var lng = parseFloat(document.getElementById('towerLng').value);

            if (!isNaN(lat) && !isNaN(lng)) {
                localData[net].push({ name: name, lat: lat, lng: lng });
                localStorage.setItem('myTowers', JSON.stringify(localData));
                alert("සාර්ථකව සුරැකුණා!");
                updateDisplay();
                closeAdmin();
            } else {
                alert("කරුණාකර නිවැරදි Lat/Lng ඇතුළත් කරන්න.");
            }
        }

        function findMyLocation() {
            myMap.locate({setView: true, maxZoom: 14});
            myMap.on('locationfound', function(e) {
                L.circleMarker(e.latlng, {radius: 8, color: '#1a73e8', fillOpacity: 0.8}).addTo(myMap).bindPopup("ඔබ මෙතැනයි").openPopup();
            });
        }
    </script>
</body>
</html>
