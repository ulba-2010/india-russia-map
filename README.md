<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Маршрут: Индия-Россия</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <script src="https://api.mapbox.com/mapbox-gl-js/v2.10.0/mapbox-gl.js"></script>
  <link href="https://api.mapbox.com/mapbox-gl-js/v2.10.0/mapbox-gl.css" rel="stylesheet">
  <style>
    body { margin: 0; padding: 0; }
    #map { position: absolute; top: 0; bottom: 0; width: 100%; }
  </style>
</head>
<body>
  <div id="map"></div>
  <script>
    // Замените на ваш Mapbox Access Token
    mapboxgl.accessToken = 'ВАШ_ACCESS_TOKEN';

    // Инициализация карты
    const map = new mapboxgl.Map({
      container: 'map',
      style: 'mapbox://styles/mapbox/streets-v11', // Стиль карты
      center: [77.2090, 28.6139], // Центр карты (Дели, Индия)
      zoom: 3
    });

    // Координаты маршрута (Дели -> Москва)
    const route = {
      type: 'Feature',
      properties: {},
      geometry: {
        type: 'LineString',
        coordinates: [
          [77.2090, 28.6139], // Дели, Индия
          [69.1607, 34.5553], // Кабул, Афганистан
          [60.5236, 41.3775], // Ташкент, Узбекистан
          [37.6173, 55.7558]  // Москва, Россия
        ]
      }
    };

    // Добавление маршрута на карту
    map.on('load', () => {
      map.addLayer({
        id: 'route',
        type: 'line',
        source: {
          type: 'geojson',
          data: route
        },
        paint: {
          'line-color': '#FF0000', // Цвет линии
          'line-width': 3 // Ширина линии
        }
      });

      // Добавление маркеров для точек начала и конца
      new mapboxgl.Marker({ color: '#00FF00' })
        .setLngLat([77.2090, 28.6139]) // Дели, Индия
        .setPopup(new mapboxgl.Popup().setHTML('<b>Дели, Индия</b>'))
        .addTo(map);

      new mapboxgl.Marker({ color: '#0000FF' })
        .setLngLat([37.6173, 55.7558]) // Москва, Россия
        .setPopup(new mapboxgl.Popup().setHTML('<b>Москва, Россия</b>'))
        .addTo(map);

      // Анимация движения вдоль маршрута
      const coordinates = route.geometry.coordinates;
      let currentIndex = 0;

      function animateRoute() {
        if (currentIndex >= coordinates.length) return;

        // Перемещение центра карты к текущей точке
        map.setCenter(coordinates[currentIndex]);

        // Увеличение индекса для следующей точки
        currentIndex++;

        // Задержка для анимации (в миллисекундах)
        setTimeout(animateRoute, 500);
      }

      // Запуск анимации
      animateRoute();
    });
  </script>
</body>
</html>
