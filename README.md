<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Photobooth App</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            text-align: center;
            font-family: 'Poppins', sans-serif;
            background: #222;
            color: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            height: 100vh;
            padding-top: 30px;
            overflow: hidden;
        }
        h1 {
            margin-bottom: 15px;
            font-size: 22px;
            font-weight: 600;
        }
        .photobooth-container {
            width: 100%;
            max-width: 600px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        video, canvas, .gallery-container img {
            width: 100%;
            max-width: 100%;
            height: auto;
            border-radius: 10px;
        }
        #canvas {
            display: none;
        }
        .gallery-container {
            display: none;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            width: 100%;
            max-width: 600px;
        }
        .btn-container {
            position: absolute;
            bottom: 70px;
            display: flex;
            gap: 20px;
        }
        #capture {
            width: 70px;
            height: 70px;
            background: white;
            border: none;
            border-radius: 50%;
            cursor: pointer;
            transition: 0.2s;
        }
        #capture:hover {
            background: #ddd;
        }
        #capture:active {
            background: #aaa;
            transform: scale(0.95);
        }
        .action-buttons {
            display: none;
            margin-top: 20px;
            display: flex;
            justify-content: center;
            gap: 20px;
        }
        .action-buttons button {
            padding: 14px;
            font-size: 24px;
            font-weight: bold;
            border: none;
            border-radius: 50%;
            cursor: pointer;
            transition: 0.3s;
            width: 60px;
            height: 60px;
            text-align: center;
            background: #666;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .action-buttons button:hover {
            background: #888;
        }
    </style>
</head>
<body>

    <h1>Photobooth</h1>

    <div class="photobooth-container">
        <video id="video" autoplay></video>
        <div class="btn-container">
            <button id="capture"></button>
        </div>
        <canvas id="canvas"></canvas>
        
        <div class="gallery-container" id="gallery">
            <img id="photo" src="" alt="Captured Photo">
            <div class="action-buttons">
                <button id="download">&#10003;</button>  <!-- ✓ Single-Line Checkmark -->
                <button id="retake">&#8635;</button>  <!-- ↻ Redo Button -->
            </div>
        </div>
    </div>

    <script>
        const video = document.getElementById('video');
        const canvas = document.getElementById('canvas');
        const photo = document.getElementById('photo');
        const captureButton = document.getElementById('capture');
        const downloadButton = document.getElementById('download');
        const retakeButton = document.getElementById('retake');
        const galleryContainer = document.getElementById('gallery');
        const actionButtons = document.querySelector('.action-buttons');

        // Check if getUserMedia is supported
        if (navigator.mediaDevices?.getUserMedia) {
            navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } })
                .then(stream => {
                    video.srcObject = stream;
                })
                .catch(error => {
                    console.error("Webcam error:", error);
                    video.style.display = "none"; // Hide video if camera not accessible
                    captureButton.style.display = "none"; // Hide capture button
                });
        } else {
            video.style.display = "none"; // Hide video if not supported
            captureButton.style.display = "none"; // Hide capture button
        }

        // Capture photo
        captureButton.addEventListener('click', () => {
            const context = canvas.getContext('2d');
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            context.drawImage(video, 0, 0, canvas.width, canvas.height);

            const imageData = canvas.toDataURL('image/png');
            photo.src = imageData;

            video.style.display = 'none';
            captureButton.style.display = 'none';
            galleryContainer.style.display = 'flex';
            actionButtons.style.display = 'flex';
        });

        // Download photo
        downloadButton.addEventListener('click', () => {
            const link = document.createElement('a');
            link.href = photo.src;
            link.download = 'photobooth.png';
            link.click();
        });

        // Retake photo
        retakeButton.addEventListener('click', () => {
            galleryContainer.style.display = 'none';
            actionButtons.style.display = 'none';
            video.style.display = 'block';
            captureButton.style.display = 'block';
        });
    </script>

</body>
</html>
