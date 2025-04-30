<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <title>Mehmet S. Yapay Zeka</title>
  <style>
    body {
      background-color: #111;
      color: white;
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 40px;
    }
    input, button {
      padding: 10px;
      font-size: 16px;
      margin: 10px;
      border-radius: 8px;
    }
    img {
      margin-top: 20px;
      max-width: 90%;
      border: 2px solid white;
      border-radius: 12px;
    }
  </style>
</head>
<body>
  <h1>Mehmet S. Yapay Zeka</h1>
  <input type="text" id="prompt" placeholder="Ne çizilsin? (örnek: kırmızı araba)">
  <br>
  <button onclick="generateImage()">Görsel Oluştur</button>
  <div id="result"></div>
  <p>Bu proje Mehmet Sengon tarafından yapay zeka destekli olarak geliştirilmiştir.</p>

  <script>
    async function generateImage() {
      const prompt = document.getElementById("prompt").value;
      const apiKey = "r8_3gEtBLRCjBrBUEM4SIb7rk1QLgFJNE72si5Vt";

      const response = await fetch("https://api.replicate.com/v1/predictions", {
        method: "POST",
        headers: {
          "Authorization": "Token " + apiKey,
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          version: "a9758cb5c68f8d52b68d6acb74e94d218abf57e1e0aa1e5f39a64b53ce7dcb37",
          input: { prompt: prompt }
        })
      });

      const data = await response.json();

      if (data?.urls?.get) {
        setTimeout(async () => {
          const imageResponse = await fetch(data.urls.get);
          const imageData = await imageResponse.json();
          if (imageData.output && imageData.output[0]) {
            document.getElementById("result").innerHTML =
              `<img src="${imageData.output[0]}" alt="Görsel">`;
          } else {
            document.getElementById("result").innerHTML = "Görsel oluşturulamadı.";
          }
        }, 8000);
      } else {
        document.getElementById("result").innerHTML = "Bir hata oluştu.";
      }
    }
  </script>
</body>
</html>
