# Consulta CEP – Itanhaém (PWA)

Este projeto é um **site PWA instalável** que permite consultar o **CEP de endereços em Itanhaém** a partir de um arquivo local de referência.

---

## 📁 Estrutura de Arquivos

```
/consulta-cep-itanhaem
 ├── index.html
 ├── style.css
 ├── app.js
 ├── manifest.json
 └── service-worker.js
```

---

## 📄 index.html

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Consulta CEP - Itanhaém</title>
  <link rel="stylesheet" href="style.css" />
  <link rel="manifest" href="manifest.json" />
</head>
<body>
  <div class="container">
    <h1>Consulta CEP - Itanhaém</h1>
    <p>Digite o nome da rua (com ou sem número)</p>

    <input type="text" id="endereco" placeholder="Ex: Av. Rui Barbosa 123" />
    <button onclick="consultarCEP()">Consultar CEP</button>

    <div id="resultado"></div>
  </div>

  <script src="app.js"></script>
</body>
</html>
```

---

## 🎨 style.css

```css
* {
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
}

body {
  background: #f2f4f8;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 420px;
  margin: 50px auto;
  background: #fff;
  padding: 25px;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  text-align: center;
}

h1 {
  margin-bottom: 10px;
  color: #4b0082;
}

input {
  width: 100%;
  padding: 12px;
  margin-top: 15px;
  border-radius: 6px;
  border: 1px solid #ccc;
}

button {
  margin-top: 15px;
  width: 100%;
  padding: 12px;
  border: none;
  border-radius: 6px;
  background: #4b0082;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}

button:hover {
  background: #360060;
}

#resultado {
  margin-top: 20px;
  font-weight: bold;
}
```

---

## ⚙️ app.js

```javascript
// Base de dados exemplo (pode ser expandida com o PDF real)
const enderecos = [
  { rua: "Avenida Rui Barbosa", cep: "11740-000" },
  { rua: "Rua Cunha Moreira", cep: "11740-120" },
  { rua: "Rua Antônio Olívio de Araújo", cep: "11740-210" }
];

function normalizar(texto) {
  return texto.toLowerCase().normalize("NFD").replace(/[^a-z0-9 ]/g, "");
}

function consultarCEP() {
  const input = document.getElementById("endereco").value;
  const resultado = document.getElementById("resultado");

  if (!input) {
    resultado.innerText = "Digite um endereço.";
    return;
  }

  const busca = normalizar(input);

  const encontrado = enderecos.find(e => busca.includes(normalizar(e.rua)));

  if (encontrado) {
    resultado.innerText = `CEP encontrado: ${encontrado.cep}`;
  } else {
    resultado.innerText = "CEP não encontrado na base local.";
  }
}

// Registro do Service Worker
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('service-worker.js');
}
```

---

## 📱 manifest.json

```json
{
  "name": "Consulta CEP - Itanhaém",
  "short_name": "Consulta CEP",
  "start_url": "./index.html",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#4b0082",
  "icons": [
    {
      "src": "icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

---

## 🔌 service-worker.js

```javascript
const CACHE_NAME = "consulta-cep-cache-v1";
const FILES_TO_CACHE = [
  "./",
  "./index.html",
  "./style.css",
  "./app.js",
  "./manifest.json"
];

self.addEventListener("install", event => {
  event.waitUntil(
    caches.open(CACHE_NAME).then(cache => cache.addAll(FILES_TO_CACHE))
  );
});

self.addEventListener("fetch", event => {
  event.respondWith(
    caches.match(event.request).then(response => response || fetch(event.request))
  );
});
```

---

## 🚀 Como usar

1. Coloque os arquivos em uma pasta
2. Abra via servidor local ou hospedagem (GitHub Pages)
3. No navegador, clique em **"Instalar aplicativo"**
4. Use offline normalmente

---

## 🔜 Próximos upgrades

- Importar automaticamente o PDF oficial de CEPs
- Autocomplete de ruas
- Botão copiar CEP
- Histórico de consultas
- Logo e identidade visual da empresa

