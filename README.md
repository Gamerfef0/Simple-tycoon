<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Simple Tycoon</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding-top: 100px;
      background-color: #f0f0f0;
    }
    #dinero {
      position: absolute;
      top: 10px;
      left: 10px;
      font-size: 18px;
      background-color: white;
      padding: 5px 10px;
      border-radius: 8px;
      box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
    }
    #idioma {
      position: absolute;
      top: 10px;
      right: 10px;
      font-size: 14px;
      cursor: pointer;
      background-color: white;
      padding: 5px 10px;
      border-radius: 8px;
      box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
    }
    button {
      font-size: 20px;
      padding: 10px 20px;
      margin: 10px;
      cursor: pointer;
    }
    #resultado {
      margin-top: 20px;
      font-size: 36px;
      color: #333;
    }
  </style>
</head>
<body>

  <div id="dinero"></div>
  <div id="idioma" onclick="cambiarIdioma()">🌐 Idioma: Español</div>

  <h1 id="titulo">Simple Tycoon</h1>

  <button id="boton" onclick="ganarDinero()">Ganar dinero</button><br>
  <button onclick="comprarMejora()" id="mejoraBtn">Comprar mejora ($<span id="precio">50</span>)</button><br>
  <button onclick="comprarAutoClicker()" id="autoBtn">Comprar autoclicker ($<span id="precioAutoClicker">100</span>)</button>

  <div id="resultado">Haz clic para empezar</div>

  <script>
    // Estado del juego
    let dinero = parseInt(localStorage.getItem("dinero")) || 0;
    let multiplicador = parseInt(localStorage.getItem("multiplicador")) || 1;
    let precioMejora = parseInt(localStorage.getItem("precioMejora")) || 50;
    let precioAutoClicker = parseInt(localStorage.getItem("precioAutoClicker")) || 100;
    let autoClickerActivo = parseInt(localStorage.getItem("autoClickerActivo")) || 0;
    let idioma = localStorage.getItem("idioma") || "es";

    // Traducciones
    const textos = {
      es: {
        titulo: "Simple Tycoon",
        ganar: "Ganar dinero",
        mejora: "Comprar mejora",
        auto: "Comprar autoclicker",
        dinero: "Tu dinero",
        inicio: "Haz clic para empezar",
        idioma: "🌐 Idioma: Español"
      },
      en: {
        titulo: "Simple Tycoon",
        ganar: "Earn Money",
        mejora: "Buy Upgrade",
        auto: "Buy Autoclicker",
        dinero: "Your money",
        inicio: "Click to start",
        idioma: "🌐 Language: English"
      }
    };

    function traducirInterfaz() {
      const t = textos[idioma];
      document.getElementById("titulo").textContent = t.titulo;
      document.getElementById("boton").textContent = t.ganar;
      document.getElementById("mejoraBtn").innerHTML = `${t.mejora} ($<span id="precio">${precioMejora}</span>)`;
      document.getElementById("autoBtn").innerHTML = `${t.auto} ($<span id="precioAutoClicker">${precioAutoClicker}</span>)`;
      document.getElementById("dinero").textContent = `${t.dinero}: $${dinero}`;
      document.getElementById("resultado").textContent = t.inicio;
      document.getElementById("idioma").textContent = t.idioma;
    }

    function cambiarIdioma() {
      idioma = idioma === "es" ? "en" : "es";
      localStorage.setItem("idioma", idioma);
      traducirInterfaz();
    }

    function actualizarInterfaz() {
      document.getElementById("dinero").textContent = `${textos[idioma].dinero}: $${dinero}`;
      document.getElementById("precio").textContent = precioMejora;
      document.getElementById("precioAutoClicker").textContent = precioAutoClicker;
    }

    function guardarProgreso() {
      localStorage.setItem("dinero", dinero);
      localStorage.setItem("multiplicador", multiplicador);
      localStorage.setItem("precioMejora", precioMejora);
      localStorage.setItem("precioAutoClicker", precioAutoClicker);
      localStorage.setItem("autoClickerActivo", autoClickerActivo);
      localStorage.setItem("idioma", idioma);
    }

    function ganarDinero() {
      dinero += multiplicador;
      document.getElementById("resultado").textContent = "+$" + multiplicador;
      actualizarInterfaz();
      guardarProgreso();
    }

    function comprarMejora() {
      if (dinero >= precioMejora) {
        dinero -= precioMejora;
        multiplicador *= 2;
        precioMejora *= 2;
        actualizarInterfaz();
        guardarProgreso();
        traducirInterfaz(); // para actualizar el texto con nuevo precio
      } else {
        alert("No tienes suficiente dinero.");
      }
    }

    function comprarAutoClicker() {
      if (dinero >= precioAutoClicker) {
        dinero -= precioAutoClicker;
        autoClickerActivo += 1;
        precioAutoClicker *= 2;
        activarAutoClicker();
        actualizarInterfaz();
        guardarProgreso();
        traducirInterfaz();
      } else {
        alert("No tienes suficiente dinero.");
      }
    }

    function activarAutoClicker() {
      if (!window.autoClickerInterval && autoClickerActivo > 0) {
        window.autoClickerInterval = setInterval(() => {
          dinero += multiplicador * autoClickerActivo;
          actualizarInterfaz();
          guardarProgreso();
        }, 5000);
      }
    }

    // Inicializar todo al cargar
    traducirInterfaz();
    activarAutoClicker();
  </script>

</body>
</html>
