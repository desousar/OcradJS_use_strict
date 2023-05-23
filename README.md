# OcradJS_use_strict
Ocrad.js for use strict env

base : https://antimatter15.com/ocrad.js/ocrad.js from https://github.com/antimatter15/ocrad.js

demo : https://codepen.io/zerbene/pen/WNaLZEQ
or here the code 

```
<!--   <script src="https://antimatter15.com/ocrad.js/ocrad.js"></script> -->
<script src="https://cdn.jsdelivr.net/gh/desousar/OcradJS_use_strict@v2/ocrad.js"></script>

  <h1>Utilisation d'OCRad.js avec une image</h1>
  <input type="file" id="imageInput">
  <pre id="result"></pre>
  
  <script>
    function readImage(input) {
      if (input.files && input.files[0]) {
        var reader = new FileReader();

        reader.onload = function(e) {
          var image = new Image();
          
          image.onload = function() {
            
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');

            const upscaleBy = 1
            canvas.width = image.width * upscaleBy;
            canvas.height = image.height * upscaleBy;
            ctx.drawImage(image, 0, 0, canvas.width, canvas.height);

           const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
            for (let i = 0; i < imageData.data.length; i += 4) {
              const r = imageData.data[i];
              const g = imageData.data[i + 1];
              const b = imageData.data[i + 2];
              const brightness = (r + g + b) / 3;
              const bw = brightness < 128 ? brightness : 255;
              imageData.data[i] = bw;
              imageData.data[i + 1] = bw;
              imageData.data[i + 2] = bw;
            }
            ctx.putImageData(imageData, 0, 0);
            
            const base64Image = canvas.toDataURL('image/jpeg', 1);
            
            const image2 = new Image();
            image2.onload = function() {
              const result = OCRAD(image2);

              document.getElementById('result').textContent = result;
            };
            image2.src = base64Image;
          };
          
          image.src = e.target.result;
        };

        reader.readAsDataURL(input.files[0]);
      }
    }

    document.getElementById('imageInput').addEventListener('change', function() {
      readImage(this);
    });
  </script>
```
