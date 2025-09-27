us-election-map/
├── index.html
├── style.css
├── script.js
└── us-states.geojson
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>US Election Map</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css"/>
  <link rel="stylesheet" href="style.css"/>
</head>
<body>
  <h1>US Election Map</h1>
  <div id="map"></div>

  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script src="script.js"></script>
</body>
</html>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  text-align: center;
}

#map {
  width: 100%;
  const map = L.map('map').setView([37.8, -96], 4);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
  attribution: '© OpenStreetMap contributors'
}).addTo(map);

// Load GeoJSON data for US states
fetch('us-states.geojson')
  .then(response => response.json())
  .then(data => {
    L.geoJSON(data, {
      style: feature => ({
        fillColor: getColor(feature.properties.party),
        weight: 2,
        opacity: 1,
        color: 'white',
        dashArray: '3',
        fillOpacity: 0.7
      }),
      onEachFeature: (feature, layer) => {
        layer.on({
          mouseover: highlightFeature,
          mouseout: resetHighlight,
          click: zoomToFeature
        });
        layer.bindPopup(`<strong>${feature.properties.name}</strong><br>Party: ${feature.properties.party}`);
      }
    }).addTo(map);
  });

function getColor(party) {
  switch(party) {
    case 'Democrat': return '#357EDD';
    case 'Republican': return '#E91D0E';
    case 'Independent': return '#888888';
    default: return '#CCCCCC';
  }
}

function highlightFeature(e) {
  const layer = e.target;
  layer.setStyle({
    weight: 3,
    color: '#666',
    dashArray: '',
    fillOpacity: 0.9
  });
}

function resetHighlight(e) {
  geojson.resetStyle(e.target);
}

function zoomToFeature(e) {
  map.fitBounds(e.target.getBounds());
}
"properties": {
  "name": "California",
  "party": "Democrat"
}
  height: 90vh;
}
