<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>لعبة سواقة على الشوارع الحقيقية</title>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<style>
  html, body, #map { height: 100%; margin:0; padding:0; }
  #controls {
    position: absolute;
    bottom: 20px; left: 50%;
    transform: translateX(-50%);
    display: flex; gap: 10px;
    z-index: 1000;
  }
  .btn {
    padding: 10px 15px;
    background: rgba(255,255,255,0.9);
    border: 1px solid #333;
    border-radius: 5px;
    font-size: 18px;
  }
</style>
</head>
<body>
<div id="map"></div>
<div id="controls">
  <button class="btn" id="left">⬅️</button>
  <button class="btn" id="up">⬆️</button>
  <button class="btn" id="down">⬇️</button>
  <button class="btn" id="right">➡️</button>
</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
  // بداية الخريطة (القاهرة مثال)
  const map = L.map('map').setView([30.0444, 31.2357], 17);

  // خريطة OSM
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '© OpenStreetMap contributors'
  }).addTo(map);

  // الطريق الحقيقي على شكل خط (Polyline)
  const roadCoords = [
    [30.0444, 31.2357],
    [30.0447, 31.2360],
    [30.0450, 31.2365],
    [30.0453, 31.2370]
  ];
  const road = L.polyline(roadCoords, {color: 'blue'}).addTo(map);

  // ايقونة العربية
  const carIcon = L.icon({
    iconUrl: 'car.png', // ضع car.png في نفس المجلد
    iconSize: [40,40],
    iconAnchor: [20,20]
  });

  // Marker للعربية
  let carMarker = L.marker(roadCoords[0], {icon: carIcon}).addTo(map);

  let index = 0; // موقع العربية على الطريق
  const step = 0.0003; // سرعة الحركة

  function moveCar(dir) {
    if(dir === 'up' && index < roadCoords.length -1) index++;
    if(dir === 'down' && index > 0) index--;
    carMarker.setLatLng(roadCoords[index]);
    map.panTo(roadCoords[index]);
  }

  // أزرار التحكم
  document.getElementById('up').addEventListener('click', ()=> moveCar('up'));
  document.getElementById('down').addEventListener('click', ()=> moveCar('down'));
  document.getElementById('left').addEventListener('click', ()=> moveCar('up')); // تعديل لاحق: دوران
  document.getElementById('right').addEventListener('click', ()=> moveCar('up')); // تعديل لاحق: دوران

</script>
</body>
</html>
