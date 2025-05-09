
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Simple Tycoon</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 0;
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
      font-size: 18px;
      background-color: white;
      padding: 5px 10px;
      border-radius: 8px;
      box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
    }
    button {
      font-size: 20px;
      padding: 10px 20px;
      margin: 10px;
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
  <div id="idioma"></div>

  <h1 id="titulo">Simple Tycoon</h1>

  <button id="boton" onclick="ganarDinero()">Ganar dinero</button>
  <button onclick="comprarMejora()" id="mejoraBtn">Comprar mejora</button>
  <button onclick="comprarAutoClicker()" id="autoClickerBtn">Comprar autoclicker</button>
  <button onclick="cambiarIdioma()">Cambiar idioma</button>
  
  <script>
    // Variables
    let dinero = parseInt(localStorage.getItem("dinero")) || 0;
    let multiplicador = parseInt(localStorage.getItem("multiplicador")) || 1;
    let precioMejora = parseInt(localStorage.getItem("precioMejora")) || 50;
    let precioAutoClicker = parseInt(localStorage.getItem("precioAutoClicker")) || 100;
    let autoClickerActivo = parseInt(localStorage.getItem("autoClickerActivo")) || 0;
    let idioma = localStorage.getItem("idioma") || "es";

    // Textos en ambos idiomas
    const textos = {
      es: {
        titulo: "Simple Tycoon",
        ganar: "Ganar dinero",
        mejora: "Comprar mejora ($",
        autoclicker: "Comprar autoclicker ($",
        resultado: "Haz clic para empezar",
        idioma: "Idioma: Español",
        cambiar: "Cambiar idioma"
      },
      en: {
        titulo: "Simple Tycoon",
        ganar: "Earn Money",
        mejora: "Buy upgrade ($",
        autoclicker: "Buy autoclicker ($",
        resultado: "Click to start",
        idioma: "Language: English",
        cambiar: "Change Language"
      }
    };

    function actualizarInterfaz() {
      document.getElementById("dinero").textContent = (idioma === "es" ? "Tu dinero: $" : "Your money: $") + dinero;
      document.getElementById("precio").textContent = precioMejora;
      document.getElementById("precioAutoClicker").textContent = precioAutoClicker;
      document.getElementById("idioma").textContent = textos[idioma].idioma;

      // Actualizar textos
      document.getElementById("titulo").textContent = textos[idioma].titulo;
      document.getElementById("boton").textContent = textos[idioma].ganar;
      document.getElementById("mejoraBtn").innerHTML = `${textos[idioma].mejora}<span id="precio">${precioMejora}</span>)`;
      document.getElementById("autoClickerBtn").innerHTML = `${textos[idioma].autoclicker}<span id="precioAutoClicker">${precioAutoClicker}</span>)`;
      document.querySelector("button:last-of-type").textContent = textos[idioma].cambiar;
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
      } else {
        alert(idioma === "es" ? "No tienes suficiente dinero" : "You don't have enough money");
      }
    }

    function comprarAutoClicker() {
      if (dinero >= precioAutoClicker) {
        dinero -= precioAutoClicker;
        autoClickerActivo += 1;
        precioAutoClicker *= 2;
        actualizarInterfaz();
        guardarProgreso();
        activarAutoClicker();
      } else {
        alert(idioma === "es" ? "No tienes suficiente dinero" : "You don't have enough money");
      }
    }

    function activarAutoClicker() {
      if (autoClickerActivo > 0 && !window.autoClickerInterval) {
        window.autoClickerInterval = setInterval(() => {
          dinero += multiplicador * autoClickerActivo;
          actualizarInterfaz();
          guardarProgreso();
        }, 5000);
      }
    }

    function activarAutoClickerAlCargar() {
      if (autoClickerActivo > 0) {
        activarAutoClicker();
      }
    }

    function cambiarIdioma() {
      idioma = idioma === "es" ? "en" : "es";
      actualizarInterfaz();
      guardarProgreso();
    }

    // Inicializar
    actualizarInterfaz();
    activarAutoClickerAlCargar();
    document.getElementById("resultado").textContent = textos[idioma].resultado;
  </script>

</body>
</html>
