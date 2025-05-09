<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>ARK Image Compresser</title>
  <style>
    * {margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif;}
    body {
      background: linear-gradient(to right, #0f2027, #203a43, #2c5364);
      color: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem 1rem 6rem;
    }
    h1 {font-size: 2rem; margin-bottom: 1rem; text-align: center; color: #00f9ff;}
    input[type="file"] {
      background: #111;
      padding: 1rem;
      border: 2px dashed #00f9ff;
      border-radius: 12px;
      width: 100%;
      max-width: 400px;
      margin: 1rem 0;
      cursor: pointer;
    }
    .preview {
      margin-top: 1rem;
      max-width: 100%;
    }
    .slider-container {
      margin-top: 1rem;
      width: 90%;
      max-width: 400px;
      text-align: center;
    }
    input[type="range"] {
      width: 100%;
    }
    .buttons {
      margin-top: 1.5rem;
      display: flex;
      flex-direction: column;
      gap: 1rem;
      width: 100%;
      max-width: 400px;
    }
    button {
      padding: 0.8rem;
      font-size: 1rem;
      background-color: transparent;
      color: #00f9ff;
      border: 2px solid #00f9ff;
      border-radius: 10px;
      cursor: pointer;
      transition: all 0.3s ease;
    }
    button:hover {
      background-color: #00f9ff;
      color: #000;
      transform: scale(1.05);
    }
    .ad-container {
      margin-top: 3rem;
      text-align: center;
    }
    @media (max-width: 480px) {
      h1 { font-size: 1.5rem; }
      button { font-size: 0.9rem; }
    }
  </style>
</head>
<body>

  <h1>ARK Image Compresser</h1>

  <input type="file" id="imageInput" accept="image/*" />
  <img id="preview" class="preview" src="" alt="Preview" hidden />

  <div class="slider-container">
    <label for="qualityRange">Select Compression Quality: <span id="qualityLabel">0.8</span></label>
    <input type="range" id="qualityRange" min="0.1" max="1" step="0.1" value="0.8" />
  </div>

  <div class="buttons">
    <button onclick="compressImage()">Compress Image</button>
    <a id="downloadLink" download="compressed-image.jpg" hidden>
      <button>Download Compressed Image</button>
    </a>
  </div>

  <div class="ad-container">
    <!-- Adsterra Ad Code -->
    <script type="text/javascript">
      atOptions = {
        'key': '84552c523dbccbcb6b5cf4085802cf2f',
        'format': 'iframe',
        'height': 50,
        'width': 320,
        'params': {}
      };
    </script>
    <script type="text/javascript" src="//www.highperformanceformat.com/84552c523dbccbcb6b5cf4085802cf2f/invoke.js"></script>
  </div>

  <script>
    const imageInput = document.getElementById("imageInput");
    const preview = document.getElementById("preview");
    const qualityRange = document.getElementById("qualityRange");
    const qualityLabel = document.getElementById("qualityLabel");
    const downloadLink = document.getElementById("downloadLink");

    qualityRange.addEventListener("input", () => {
      qualityLabel.textContent = qualityRange.value;
    });

    imageInput.addEventListener("change", function () {
      const file = this.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function (e) {
          preview.src = e.target.result;
          preview.hidden = false;
        };
        reader.readAsDataURL(file);
      }
    });

    function compressImage() {
      const file = imageInput.files[0];
      if (!file) {
        alert("Please upload an image first.");
        return;
      }

      const reader = new FileReader();
      reader.readAsDataURL(file);

      reader.onload = function (event) {
        const img = new Image();
        img.src = event.target.result;

        img.onload = function () {
          const canvas = document.createElement("canvas");
          const ctx = canvas.getContext("2d");

          canvas.width = img.width;
          canvas.height = img.height;
          ctx.drawImage(img, 0, 0);

          const quality = parseFloat(qualityRange.value);
          const compressedDataUrl = canvas.toDataURL("image/jpeg", quality);

          downloadLink.href = compressedDataUrl;
          downloadLink.hidden = false;
        };
      };
    }
  </script>
</body>
</html>
