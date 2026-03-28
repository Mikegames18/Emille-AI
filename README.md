# Emille-AI
<!DOCTYPE html><html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Emille AI Completa</title>  <!-- Librería matemática -->  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.8.0/math.min.js"></script>  <style>
    body {
      font-family: Arial, sans-serif;
      background: #0f172a;
      color: white;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
    }

    h1 {
      color: #38bdf8;
    }

    #chatbox {
      width: 90%;
      max-width: 500px;
      height: 300px;
      background: #1e293b;
      border-radius: 10px;
      padding: 10px;
      overflow-y: auto;
      margin-bottom: 10px;
    }

    .user {
      text-align: right;
      color: #38bdf8;
      margin: 5px;
    }

    .bot {
      text-align: left;
      color: #22c55e;
      margin: 5px;
    }

    #inputArea {
      display: flex;
      width: 90%;
      max-width: 500px;
      margin-bottom: 20px;
    }

    input {
      flex: 1;
      padding: 10px;
      border-radius: 5px;
      border: none;
      outline: none;
    }

    button {
      padding: 10px;
      margin-left: 5px;
      border: none;
      border-radius: 5px;
      background: #38bdf8;
      color: black;
      cursor: pointer;
    }

    canvas {
      margin-top: 10px;
      max-width: 90%;
      border-radius: 10px;
    }
  </style></head><body><h1>🤖 Emille AI (Chat + Matemáticas + Imágenes)</h1><div id="chatbox"></div><div id="inputArea">
  <input type="text" id="userInput" placeholder="Escribe algo o una operación...">
  <button onclick="sendMessage()">Enviar</button>
</div><!-- Editor de imágenes --><input type="file" id="upload">
<canvas id="canvas"></canvas><div>
  <button onclick="invertir()">Invertir colores</button>
  <button onclick="grises()">Blanco y negro</button>
</div><script>
function sendMessage() {
  const input = document.getElementById("userInput");
  const message = input.value.trim();
  if (message === "") return;

  addMessage("user", message);
  input.value = "";

  setTimeout(() => {
    const response = generateResponse(message);
    addMessage("bot", response);
  }, 400);
}

function addMessage(sender, text) {
  const chatbox = document.getElementById("chatbox");
  const msg = document.createElement("div");
  msg.className = sender;
  msg.textContent = text;
  chatbox.appendChild(msg);
  chatbox.scrollTop = chatbox.scrollHeight;
}

function esExpresionMatematica(texto) {
  return /[0-9+\-*/().^]/.test(texto);
}

function generateResponse(input) {
  input = input.toLowerCase();

  try {
    if (esExpresionMatematica(input)) {
      let resultado = math.evaluate(input);
      return "🧮 Resultado: " + resultado;
    }
  } catch (e) {
    return "⚠️ No pude resolver esa operación.";
  }

  if (input.includes("hola")) {
    return "Hola, soy Emille 🤖 ¿En qué puedo ayudarte?";
  } 
  else if (input.includes("cómo estás")) {
    return "Estoy funcionando perfectamente 😄";
  } 
  else if (input.includes("nombre")) {
    return "Mi nombre es Emille AI.";
  } 
  else if (input.includes("hora")) {
    return "La hora actual es: " + new Date().toLocaleTimeString();
  } 
  else if (input.includes("ecuación")) {
    return "Puedo resolver operaciones matemáticas 😎";
  }
  else if (input.includes("adiós")) {
    return "¡Hasta luego! 👋";
  } 
  else {
    return "No entendí 🤔 intenta con texto o una operación matemática.";
  }
}

// Enter para enviar
document.getElementById("userInput").addEventListener("keypress", function(e) {
  if (e.key === "Enter") {
    sendMessage();
  }
});

// ======================
// Editor de imágenes
// ======================
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

document.getElementById("upload").addEventListener("change", function(e) {
  const file = e.target.files[0];
  const img = new Image();

  img.onload = function() {
    canvas.width = img.width;
    canvas.height = img.height;
    ctx.drawImage(img, 0, 0);
  };

  img.src = URL.createObjectURL(file);
});

function invertir() {
  let imgData = ctx.getImageData(
