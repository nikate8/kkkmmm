<!DOCTYPE html>  
<html lang="en">  
<head>  
  <meta charset="UTF-8">  
  <meta name="viewport" content="width=device-width, initial-scale=1.0">  
  <title>sam world</title>  
  
  <style>  
    body {  
      margin: 0;  
      height: 100vh;  
      display: flex;  
      justify-content: center;  
      align-items: center;  
      background: black;  
      overflow: hidden;  
      font-family: Arial, sans-serif;  
      color: white;  
    }  
  
    .bg {  
      position: absolute;  
      width: 200%;  
      height: 200%;  
      background:  
        radial-gradient(circle at center, rgba(255,255,255,0.08), transparent 40%),  
        linear-gradient(45deg, #111, #000, #111);  
      animation: move 15s linear infinite;  
      filter: blur(20px);  
    }  
  
    @keyframes move {  
      0% { transform: translate(-10%, -10%) rotate(0deg); }  
      100% { transform: translate(-10%, -10%) rotate(360deg); }  
    }  
  
    .content {  
      position: relative;  
      text-align: center;  
      z-index: 1;  
    }  
  
    h1 {  
      font-size: 4rem;  
      margin: 0;  
      letter-spacing: 8px;  
    }  
  
    p {  
      opacity: 0.7;  
      margin-top: 10px;  
    }  
  
    .button {  
      margin-top: 30px;  
      display: inline-block;  
      padding: 12px 24px;  
      border: 1px solid white;  
      color: white;  
      text-decoration: none;  
      transition: 0.3s;  
    }  
  
    .button:hover {  
      background: white;  
      color: black;  
    }  
  </style>  
</head>  
  
<body>  
  <div class="bg"></div>  
  
  <div class="content">  
    <h1>SAM</h1>  
    <p>first website online</p>  
  
    <a class="button" href="#">  
      enter  
    </a>  
  </div>  
</body>  
</html>  
