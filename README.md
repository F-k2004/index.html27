<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8">
<title>💙 Blue Plasma Energy</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
  body {
    argin: 0;
    overflow: hidden;
    background: #000010;
  }
  canvas {
    display: block;
    filter: brightness(1.4) blur(1.3px);
  }
</style>
</head>

<body>
<canvas id="plasma"></canvas>

<script>
const canvas = document.getElementById("plasma");
const ctx = canvas.getContext("2d");

let w, h;

function resize() {
  w = canvas.width = window.innerWidth;
  h = canvas.height = window.innerHeight;
}
window.addEventListener("resize", resize);
resize();

// پارامتر پلاسما
let time = 0;
const speed = 0.025;

// الگوریتم پلاسما
function plasma(x, y, t) {
  let v1 = Math.sin(x * 0.02 + t);
  let v2 = Math.cos(y * 0.03 + t * 1.5);
  let v3 = Math.sin(Math.sqrt(x * x + y * y) * 0.025 - t * 2.3);

  return (v1 + v2 + v3 + 3) / 6; // نرمال شده بین 0 و 1
}

function draw() {
  let img = ctx.createImageData(w, h);
  let data = img.data;

  let index = 0;

  for (let y = 0; y < h; y++) {
    for (let x = 0; x < w; x++) {
      let p = plasma(x, y, time);

      // 🎨 طیف آبی انرژی:
      // بین آبی یخی → آبی نئونی → فیروزه‌ای
      let hue = 180 + p * 40;   // رنگ بین 180 تا 220
      let sat = 100;            // اشباع بالا
      let light = 40 + p * 40;  // روشنایی از 40 تا 80

      // HSL → RGB
      let c = (1 - Math.abs((2 * light / 100) - 1));
      let x2 = c * (1 - Math.abs((hue / 60) % 2 - 1));
      let m = light / 100 - c / 2;
      let r, g, b;

      if (hue < 60) [r, g, b] = [c, x2, 0];
      else if (hue < 120) [r, g, b] = [x2, c, 0];
      else if (hue < 180) [r, g, b] = [0, c, x2];
      else if (hue < 240) [r, g, b] = [0, x2, c];
      else if (hue < 300) [r, g, b] = [x2, 0, c];
      else [r, g, b] = [c, 0, x2];

      data[index++] = (r + m) * 255;
      data[index++] = (g + m) * 255;
      data[index++] = (b + m) * 255;
      data[index++] = 255;
    }
  }

  ctx.putImageData(img, 0, 0);
  time += speed;

  requestAnimationFrame(draw);
}

draw();
</script>

</body>
</html>
