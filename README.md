<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="Compress your images to a specific size. Free, fast, and SEO optimized.">
    <meta name="keywords" content="image compressor, online image resize, optimize images, reduce image size">
    <meta name="author" content="Your Name">
    <title>Image Compressor with Target Size</title>

    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #f0f2f5;
            margin: 0;
            padding: 0;
            color: #333;
        }

        header {
            background: linear-gradient(90deg, #004e92, #000428);
            color: white;
            padding: 1.5rem;
            text-align: center;
        }

        main {
            max-width: 900px;
            margin: 2rem auto;
            padding: 1rem;
            background: #fff;
            box-shadow: 0 0 15px rgba(0,0,0,0.1);
            border-radius: 8px;
        }

        .ads {
            display: flex;
            justify-content: space-between;
            gap: 1rem;
            margin: 2rem 0;
        }

        .ad-box {
            flex: 1;
            height: 120px;
            background: url('https://via.placeholder.com/300x120?text=Ad+Banner') center/cover no-repeat;
            border-radius: 8px;
            box-shadow: 0 2px 6px rgba(0,0,0,0.2);
        }

        .upload-box {
            text-align: center;
            padding: 2rem;
            border: 2px dashed #2196f3;
            border-radius: 10px;
            margin-bottom: 2rem;
        }

        input[type="file"],
        input[type="number"] {
            margin: 1rem 0;
            padding: 0.5rem;
        }

        button {
            display: none;
            margin-top: 1rem;
            padding: 0.8rem 1.5rem;
            background: #1976d2;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
        }

        canvas {
            max-width: 100%;
            margin-top: 1rem;
            display: none;
        }

        .download-btn {
            display: none;
            margin-top: 1rem;
            padding: 0.8rem 1.5rem;
            background: #4caf50;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
        }

        .info {
            margin-top: 1rem;
            font-size: 0.95rem;
        }

        footer {
            text-align: center;
            padding: 1rem;
            margin-top: 2rem;
            font-size: 0.9rem;
            background: #eee;
        }
    </style>
</head>
<body>
    <header>
        <h1>Advanced Image Compressor</h1>
        <p>Compress your images to your target size in kilobytes (KB).</p>
    </header>

    <main>
        <div class="ads">
            <div class="ad-box"></div>
            <div class="ad-box"></div>
        </div>

        <div class="upload-box">
            <h2>Upload & Compress Image</h2>
            <input type="file" accept="image/*" id="imgInput" />
            <br />
            <label>Target size (KB):</label>
            <input type="number" id="targetSize" placeholder="e.g. 200" min="10" />
            <br />
            <button id="compressBtn">Compress</button>
            <br />
            <canvas id="canvas"></canvas>
            <a id="downloadBtn" class="download-btn" download="compressed-image.jpg">Download Compressed Image</a>

            <div class="info" id="sizeInfo"></div>
        </div>
    </main>

    <footer>
        &copy; 2025 Advanced Image Compressor. All rights reserved.
    </footer>

    <script>
        const imgInput = document.getElementById('imgInput');
        const targetSizeInput = document.getElementById('targetSize');
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        const compressBtn = document.getElementById('compressBtn');
        const downloadBtn = document.getElementById('downloadBtn');
        const sizeInfo = document.getElementById('sizeInfo');

        let uploadedImage = null;
        let originalSizeKB = 0;

        imgInput.addEventListener('change', (event) => {
            const file = event.target.files[0];
            if (!file) return;

            originalSizeKB = Math.round(file.size / 1024);
            const reader = new FileReader();
            reader.onload = function(e) {
                const img = new Image();
                img.onload = function() {
                    uploadedImage = img;
                    compressBtn.style.display = 'inline-block';
                    sizeInfo.innerHTML = `Original Size: <strong>${originalSizeKB} KB</strong>`;
                };
                img.src = e.target.result;
            };
            reader.readAsDataURL(file);
        });

        compressBtn.addEventListener('click', () => {
            const targetSize = parseInt(targetSizeInput.value);
            if (!uploadedImage || isNaN(targetSize) || targetSize < 10) {
                alert('Please upload an image and enter a valid target size (min 10 KB).');
                return;
            }

            // Try compressing by reducing quality in loop
            let quality = 0.9;
            let compressedDataUrl;
            let dataSizeKB = 0;

            // Use canvas to resize (maintain full size for now)
            canvas.width = uploadedImage.width;
            canvas.height = uploadedImage.height;
            ctx.drawImage(uploadedImage, 0, 0);

            do {
                compressedDataUrl = canvas.toDataURL('image/jpeg', quality);
                const byteString = atob(compressedDataUrl.split(',')[1]);
                dataSizeKB = Math.round(byteString.length / 1024);
                quality -= 0.05;
            } while (dataSizeKB > targetSize && quality > 0.05);

            downloadBtn.href = compressedDataUrl;
            downloadBtn.style.display = 'inline-block';
            canvas.style.display = 'block';

            const percent = ((dataSizeKB / originalSizeKB) * 100).toFixed(1);
            sizeInfo.innerHTML += `<br>Compressed Size: <strong>${dataSizeKB} KB</strong> (${percent}% of original)`;
        });
    </script>
</body>
</html>
