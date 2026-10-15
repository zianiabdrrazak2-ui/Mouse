# Mouse
No si te parece 

<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Khddam Cerca | Servicios a domicilio</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f4f7fb;
      color: #182238;
    }
    header {
      background: #123c69;
      color: white;
      padding: 22px 16px;
      text-align: center;
    }
    header h1 { margin: 0 0 8px; }
    header p { margin: 0; }
    main {
      max-width: 850px;
      margin: auto;
      padding: 20px 14px;
    }
    .search, .form, .service {
      background: white;
      padding: 18px;
      border-radius: 14px;
      margin-bottom: 16px;
      box-shadow: 0 3px 12px #0000000b;
    }
    h2 { margin-top: 0; font-size: 20px; }
    input, select, textarea, button {
      width: 100%;
      padding: 12px;
      margin: 7px 0;
      border-radius: 9px;
      font-size: 16px;
    }
    input, select, textarea {
      border: 1px solid #ccd5e0;
      background: white;
    }
    button {
      border: 0;
      background: #087f8c;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }
    button:hover { opacity: .9; }
    .categories {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
      margin-bottom: 20px;
    }
    .category {
      background: white;
      padding: 15px 8px;
      text-align: center;
      border-radius: 12px;
      cursor: pointer;
      border: 1px solid #e1e8f0;
    }
    .service h3 { margin: 0 0 8px; }
    .service p { color: #526176; }
    .tag {
      display: inline-block;
      background: #e5f6f4;
      color: #087f8c;
      padding: 5px 9px;
      border-radius: 20px;
      font-size: 13px;
    }
    #empty { color: #667085; text-align: center; }
    footer {
      text-align: center;
      color: #667085;
      padding: 25px 12px;
    }
    @media (min-width: 600px) {
      .categories { grid-template-columns: repeat(4, 1fr); }
    }
  </style>
</head>
<body>

<header>
  <h1>🔧 Khddam Cerca</h1>
  <p>Encuentra profesionales cerca de ti</p>
</header>

<main>
  <section class="search">
    <h2>¿Qué servicio necesitas?</h2>
    <input id="searchCity" placeholder="Tu ciudad, por ejemplo Sevilla">
    <select id="searchCategory">
      <option value="">Todos los servicios</option>
      <option>Fontanería</option>
      <option>Electricidad</option>
      <option>Limpieza</option>
      <option>Pintura</option>
      <option>Reparaciones</option>
      <option>Jardinería</option>
    </select>
    <button onclick="renderServices()">Buscar servicios</button>
  </section>

  <h2>Servicios populares</h2>
  <div class="categories">
    <div class="category" onclick="choose('Fontanería')">🚿<br>Fontanería</div>
    <div class="category" onclick="choose('Electricidad')">💡<br>Electricidad</div>
    <div class="category" onclick="choose('Limpieza')">🧹<br>Limpieza</div>
    <div class="category" onclick="choose('Pintura')">🎨<br>Pintura</div>
    <div class="category" onclick="choose('Reparaciones')">🛠️<br>Reparaciones</div>
    <div class="category" onclick="choose('Jardinería')">🌿<br>Jardinería</div>
  </div>

  <section class="form">
    <h2>Publica una solicitud</h2>
    <form id="requestForm">
      <input id="title" required placeholder="¿Qué necesitas?">
      <select id="category" required>
        <option value="">Elige un servicio</option>
        <option>Fontanería</option>
        <option>Electricidad</option>
        <option>Limpieza</option>
        <option>Pintura</option>
        <option>Reparaciones</option>
        <option>Jardinería</option>
      </select>
      <input id="city" required placeholder="Ciudad">
      <textarea id="description" required
        placeholder="Describe el trabajo que necesitas"></textarea>
      <button type="submit">Publicar solicitud</button>
    </form>
    <small>Demo: las solicitudes se guardan solo en este navegador.</small>
  </section>

  <h2>Solicitudes recientes</h2>
  <div id="services"></div>
</main>

<footer>
  © 2026 Khddam Cerca · Conecta con profesionales de tu zona
</footer>

<script>
  const KEY = "khddam_cerca_demo_requests";

  const initialServices = [
    {
      title: "Reparar un grifo",
      category: "Fontanería",
      city: "Sevilla",
      description: "Necesito reparar un grifo que pierde agua."
    },
    {
      title: "Limpieza de vivienda",
      category: "Limpieza",
      city: "Málaga",
      description: "Busco ayuda para limpiar un piso."
    }
  ];

  function getServices() {
    try {
      const saved = localStorage.getItem(KEY);
      return saved ? JSON.parse(saved) : initialServices;
    } catch {
      return initialServices;
    }
  }

  function saveServices(items) {
    try {
      localStorage.setItem(KEY, JSON.stringify(items));
    } catch {
      alert("No se pudo guardar en este navegador.");
    }
  }

  function choose(category) {
    document.getElementById("searchCategory").value = category;
    renderServices();
    document.getElementById("services").scrollIntoView({
      behavior: "smooth"
    });
  }

  function renderServices() {
    const city = document.getElementById("searchCity")
      .value.trim().toLowerCase();
    const category = document.getElementById("searchCategory").value;

    const items = getServices().filter(item =>
      (!city || item.city.toLowerCase().includes(city)) &&
      (!category || item.category === category)
    );

    const container = document.getElementById("services");
    container.innerHTML = "";

    if (!items.length) {
      container.textContent = "No hay solicitudes para esta búsqueda.";
      container.id = "empty";
      return;
    }

    container.id = "services";

    items.slice().reverse().forEach(item => {
      const card = document.createElement("article");
      card.className = "service";

      const heading = document.createElement("h3");
      heading.textContent = item.title;

      const tag = document.createElement("span");
      tag.className = "tag";
      tag.textContent = item.category;

      const location = document.createElement("p");
      location.textContent = "📍 " + item.city;

      const description = document.createElement("p");
      description.textContent = item.description;

      card.append(heading, tag, location, description);
      container.appendChild(card);
    });
  }

  document.getElementById("requestForm")
    .addEventListener("submit", function(event) {
      event.preventDefault();

      const item = {
        title: document.getElementById("title").value.trim(),
        category: document.getElementById("category").value,
        city: document.getElementById("city").value.trim(),
        description: document.getElementById("description").value.trim()
      };

      const items = getServices();
      items.push(item);
      saveServices(items);

      this.reset();
      renderServices();
      alert("Solicitud añadida a esta demo.");
    });

  renderServices();
</script>

</body>
</html>
