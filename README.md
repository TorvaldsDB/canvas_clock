# canvas_clock
A clock written on canvas.

Tick Tick Tick Tick Tick ...

<!DOCTYPE html>
<html>

  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="description" content="A clock writen by canvas"/>
    <meta name="keywords" content="Canvas, Clock">
    <meta name="author" content="Torvalds">
    <title>Canvas Clock</title>
    <style>
      div {
        text-align: center;
        margin-top: 250px;
      }

      #clock {
        border: 1px solid #ccc;
      }

    </style>
  </head>

  <body>
    <div>
      <canvas id="clock" height="200px" width="200px"></canvas>
    </div>

    <script type="text/javascript">
      var dom = document.getElementById('clock');
var ctx = dom.getContext('2d');
var width = ctx.canvas.width;
var height = ctx.canvas.height;
var r = width / 2;
var rem = width / 200;

function drawBackground() {
  ctx.save();
  ctx.translate(r, r);
  ctx.beginPath();
  ctx.lineWidth = 10 * rem;
  ctx.arc(0, 0, r - ctx.lineWidth / 2, 0, 2 * Math.PI, false);
  ctx.stroke();

  var hourNumbers = [3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 1, 2];
  ctx.font = 18 * rem + "px Arial";
  ctx.textAlign = 'center';
  ctx.textBaseline = 'middle';
  hourNumbers.forEach(function(number, index) {
    var rad = 2 * Math.PI / 12 * index;
    var x = Math.cos(rad) * (r - 30 * rem);
    var y = Math.sin(rad) * (r - 30 * rem);
    ctx.fillText(number, x, y);
  });

  for (var i = 0; i < 60; i++) {
    var rad = 2 * Math.PI / 60 * i;
    var x = Math.cos(rad) * (r - 18 * rem);
    var y = Math.sin(rad) * (r - 18 * rem);
    ctx.beginPath();
    if (i % 5 === 0) {
      ctx.fillStyle = '#000';
      ctx.arc(x, y, 2 * rem, 0, 2 * Math.PI, false);
    } else {
      ctx.fillStyle = '#ccc';
      ctx.arc(x, y, 2 * rem, 0, 2 * Math.PI, false);
    }
    ctx.fill();
  }
}

function drawHour(hour, minute, second) {
  ctx.save();
  ctx.beginPath();
  var rad = 2 * Math.PI / 12 * hour;
  var mrad = 2 * Math.PI / 12 / 60 * minute;
  var srad = 2 * Math.PI / 12 / 60 / 60 * minute;
  ctx.rotate(rad + mrad + srad);
  ctx.lineWidth = 6 * rem;
  ctx.lineCap = 'round';
  ctx.moveTo(0, 10 * rem);
  ctx.lineTo(0, -r / 2);
  ctx.stroke();
  ctx.restore();
}

function drawMinute(minute, second) {
  ctx.save();
  ctx.beginPath();
  var rad = 2 * Math.PI / 60 * (minute + second / 60);
  ctx.rotate(rad);
  ctx.lineWidth = 3 * rem;
  ctx.lineCap = 'round';
  ctx.moveTo(0, 10 * rem);
  ctx.lineTo(0, -r + 30 * rem);
  ctx.stroke();
  ctx.restore();
}

function drawSecond(second) {
  ctx.save();
  ctx.beginPath();
  ctx.fillStyle = '#c14543'
  var rad = 2 * Math.PI / 60 * second;
  ctx.rotate(rad);
  ctx.moveTo(-2 * rem, 20 * rem);
  ctx.lineTo(2 * rem, 20 * rem);
  ctx.lineTo(1, -r + 18 * rem);
  ctx.lineTo(-1, -r + 18 * rem);
  ctx.fill();
  ctx.restore();
}

function drawDot() {
  ctx.beginPath();
  ctx.fillStyle = '#fff';
  ctx.arc(0, 0, 3 * rem, 0, 2 * Math.PI, false);
  ctx.fill();
}

function draw() {
  ctx.clearRect(0, 0, width, height);
  var now = new Date();
  var hour = now.getHours();
  var minute = now.getMinutes();
  var second = now.getSeconds();

  drawBackground();
  drawHour(hour, minute, second);
  drawMinute(minute, second);
  drawSecond(second);
  drawDot();
  ctx.restore();
}

draw();
setInterval(draw, 1000);

    </script>
  </body>

</html>
