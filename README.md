<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D Christmas Gift Box</title>
    <style>
        body {
            background-color: #f0f0f0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            perspective: 1000px;
            font-family: sans-serif;
        }

        /* The 3D Scene */
        .scene {
            width: 200px;
            height: 200px;
            position: relative;
            transform-style: preserve-3d;
            transform: rotateX(-20deg) rotateY(45deg);
            transition: transform 0.5s;
            cursor: pointer;
        }

        .scene:hover {
            transform: rotateX(-25deg) rotateY(120deg);
        }

        /* Common Face Styles */
        .face {
            position: absolute;
            width: 200px;
            height: 200px;
            /* Green Metallic Base */
            background-color: #1e8045; 
            /* Polka Dot Pattern */
            background-image: 
                radial-gradient(#c62828 15%, transparent 16%),
                radial-gradient(#fdd835 5%, transparent 6%); /* Red and Gold dots */
            background-size: 40px 40px;
            background-position: 0 0, 20px 20px;
            box-shadow: inset 0 0 20px rgba(0,0,0,0.3);
            border: 1px solid #145a32;
        }

        /* Positioning the Faces to make a Cube */
        .front { transform: translateZ(100px); }
        .back  { transform: rotateY(180deg) translateZ(100px); }
        .right { transform: rotateY(90deg) translateZ(100px); filter: brightness(0.85); } /* Darker side */
        .left  { transform: rotateY(-90deg) translateZ(100px); }
        .top   { transform: rotateX(90deg) translateZ(100px); filter: brightness(1.2); } /* Lighter top */
        .bottom{ transform: rotateX(-90deg) translateZ(100px); box-shadow: 0 0 50px rgba(0,0,0,0.5); background: rgba(0,0,0,0.2); }

        /* The Lid (Slightly larger overlap) */
        .lid-face {
            position: absolute;
            background-color: #1e8045;
            /* Same pattern for lid */
            background-image: 
                radial-gradient(#c62828 15%, transparent 16%),
                radial-gradient(#fdd835 5%, transparent 6%);
            background-size: 40px 40px;
        }
        
        .lid-top {
            width: 210px; height: 210px;
            transform: rotateX(90deg) translateZ(95px) translateX(-5px) translateY(-5px);
            filter: brightness(1.25);
        }
        .lid-side {
            width: 210px; height: 40px;
            background-position: 0 -10px, 20px 10px;
        }
        .lid-front { transform: translateZ(105px) translateX(-5px) translateY(-20px); }
        .lid-right { transform: rotateY(90deg) translateZ(105px) translateX(-5px) translateY(-20px); filter: brightness(0.9); }
        .lid-left  { transform: rotateY(-90deg) translateZ(105px) translateX(-5px) translateY(-20px); }
        .lid-back  { transform: rotateY(180deg) translateZ(105px) translateX(-5px) translateY(-20px); }


        /* The Red Ribbon Strips */
        .ribbon-v, .ribbon-h {
            position: absolute;
            background: linear-gradient(90deg, #8a1c1c, #d32f2f, #8a1c1c); /* Shiny Red */
            box-shadow: 0 0 2px rgba(0,0,0,0.3);
        }

        /* Vertical Ribbon on faces */
        .face .ribbon-v { width: 40px; height: 100%; left: 80px; top: 0; }
        .lid-side .ribbon-v { width: 40px; height: 100%; left: 85px; top: 0; }
        
        /* Top Ribbon Cross */
        .lid-top .ribbon-v { width: 40px; height: 100%; left: 85px; top: 0; }
        .lid-top .ribbon-h { width: 100%; height: 40px; left: 0; top: 85px; background: linear-gradient(0deg, #8a1c1c, #d32f2f, #8a1c1c); }

        /* The Bow (Complex CSS Shapes) */
        .bow-container {
            position: absolute;
            top: -50px;
            left: 50%;
            transform: translateX(-50%) translateZ(100px) rotateX(-10deg);
            width: 120px;
            height: 100px;
            z-index: 10;
        }

        .loop {
            position: absolute;
            width: 40px;
            height: 70px;
            border-radius: 50% 50% 0 50%;
            background: radial-gradient(circle at 30% 30%, #ff5252, #b71c1c);
            border: 1px solid #8a0000;
            box-shadow: 0 2px 5px rgba(0,0,0,0.3);
            transform-origin: bottom center;
            bottom: 10px;
            left: 40px;
        }

        /* Creating the flower shape for the bow */
        .loop:nth-child(1) { transform: rotate(0deg) translateY(5px); }
        .loop:nth-child(2) { transform: rotate(45deg) translateY(5px); }
        .loop:nth-child(3) { transform: rotate(90deg) translateY(5px); }
        .loop:nth-child(4) { transform: rotate(135deg) translateY(5px); }
        .loop:nth-child(5) { transform: rotate(180deg) translateY(5px); }
        .loop:nth-child(6) { transform: rotate(225deg) translateY(5px); }
        .loop:nth-child(7) { transform: rotate(270deg) translateY(5px); }
        .loop:nth-child(8) { transform: rotate(315deg) translateY(5px); }
        
        /* Inner Knot */
        .knot {
            position: absolute;
            width: 30px;
            height: 25px;
            background: #d32f2f;
            border-radius: 50%;
            top: 60px;
            left: 45px;
            z-index: 20;
            box-shadow: 0 2px 4px rgba(0,0,0,0.4);
        }

        /* Floor Shadow */
        .shadow {
            position: absolute;
            width: 220px;
            height: 220px;
            background: radial-gradient(rgba(0,0,0,0.3), transparent 70%);
            transform: rotateX(90deg) translateZ(-110px);
            filter: blur(10px);
        }
        
        p {
            position: absolute;
            bottom: 20px;
            color: #666;
        }

    </style>
</head>
<body>

    <div class="scene">
        <div class="face front"><div class="ribbon-v"></div></div>
        <div class="face back"><div class="ribbon-v"></div></div>
        <div class="face right"><div class="ribbon-v"></div></div>
        <div class="face left"><div class="ribbon-v"></div></div>
        <div class="face bottom"></div>
        <div class="face top"></div> <div class="lid-face lid-top">
            <div class="ribbon-v"></div>
            <div class="ribbon-h"></div>
        </div>
        <div class="lid-face lid-side lid-front"><div class="ribbon-v"></div></div>
        <div class="lid-face lid-side lid-right"><div class="ribbon-v"></div></div>
        <div class="lid-face lid-side lid-left"><div class="ribbon-v"></div></div>
        <div class="lid-face lid-side lid-back"><div class="ribbon-v"></div></div>

        <div class="bow-container">
            <div class="loop"></div>
            <div class="loop"></div>
            <div class="loop"></div>
            <div class="loop"></div>
            <div class="loop"></div>
            <div class="loop"></div>
            <div class="loop"></div>
            <div class="loop"></div>
            <div class="knot"></div>
        </div>
        
        <div class="shadow"></div>
    </div>
    
    <p>Hover over the box to rotate it!</p>

</body>
</html>
