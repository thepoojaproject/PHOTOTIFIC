
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Photorific - Image Resizer</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
            background: #094d58;
            color: white;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        
        .container {
            width: 100%;
            max-width: 400px;
            text-align: center;
        }
        
        .logo {
            width: 180px;
            margin-bottom: 30px;
        }
        
        .upload-area {
            border: 2px dashed rgba(255, 255, 255, 0.3);
            border-radius: 12px;
            padding: 40px 20px;
            margin-bottom: 25px;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .upload-area:hover {
            border-color: rgba(255, 255, 255, 0.5);
            background: rgba(255, 255, 255, 0.05);
        }
        
        .upload-icon {
            font-size: 40px;
            margin-bottom: 10px;
            opacity: 0.7;
        }
        
        .controls {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-bottom: 25px;
        }
        
        .dimensions {
            display: flex;
            gap: 10px;
        }
        
        input, select, button {
            padding: 12px 15px;
            border-radius: 8px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            background: rgba(255, 255, 255, 0.1);
            color: white;
            font-size: 15px;
            width: 100%;
        }
        
        input::placeholder {
            color: rgba(255, 255, 255, 0.5);
        }
        
        select {
            appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' fill='white' viewBox='0 0 16 16'%3E%3Cpath d='M7.247 11.14 2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z'/%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 15px center;
            background-size: 12px;
        }
        
        button {
            background: #4db8c9;
            border: none;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.2s;
        }
        
        button:hover {
            background: #3aa5b7;
        }
        
        button:disabled {
            background: rgba(255, 255, 255, 0.2);
            cursor: not-allowed;
        }
        
        .preview {
            margin-top: 25px;
            display: none;
        }
        
        .preview img {
            max-width: 100%;
            max-height: 300px;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
        }
        
        .download {
            display: none;
            margin-top: 20px;
            padding: 12px 20px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            text-decoration: none;
            color: white;
            font-weight: 500;
            transition: background 0.2s;
        }
        
        .download:hover {
            background: rgba(255, 255, 255, 0.15);
        }
        
        footer {
            margin-top: 40px;
            font-size: 14px;
            opacity: 0.7;
        }
        
        .heartbeat {
            animation: heartbeat 1.5s ease-in-out infinite both;
            display: inline-block;
        }
        
        @keyframes heartbeat {
            from {
                transform: scale(1);
                transform-origin: center center;
                animation-timing-function: ease-out;
            }
            10% {
                transform: scale(0.91);
                animation-timing-function: ease-in;
            }
            17% {
                transform: scale(0.98);
                animation-timing-function: ease-out;
            }
            33% {
                transform: scale(0.87);
                animation-timing-function: ease-in;
            }
            45% {
                transform: scale(1);
                animation-timing-function: ease-out;
            }
        }
        
        .file-input {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <img src="https://i.ibb.co/m7ykF1Y/Photorific.png" alt="Photorific" class="logo">
        
        <div class="upload-area" id="uploadArea">
            <div class="upload-icon">📁</div>
            <p>Click to upload image</p>
            <input type="file" id="fileInput" class="file-input" accept="image/*">
        </div>
        
        <div class="controls">
            <div class="dimensions">
                <input type="number" id="width" placeholder="Width">
                <input type="number" id="height" placeholder="Height">
            </div>
            
            <select id="format">
                <option value="image/jpeg">JPG</option>
                <option value="image/png">PNG</option>
                <option value="image/webp">WEBP</option>
            </select>
            
            <input type="number" id="maxSize" placeholder="Max size in KB (optional)">
            
            <button id="processBtn" disabled>Process Image</button>
        </div>
        
        <div class="preview" id="preview">
            <img id="previewImg" src="" alt="Preview">
        </div>
        
        <a href="#" class="download" id="downloadLink">Download Image</a>
        
        <footer>
            Made with <span class="heartbeat">❤</span> By Armeen
        </footer>
    </div>

    <canvas id="canvas" style="display: none;"></canvas>

    <script>
        const uploadArea = document.getElementById('uploadArea');
        const fileInput = document.getElementById('fileInput');
        const widthInput = document.getElementById('width');
        const heightInput = document.getElementById('height');
        const formatSelect = document.getElementById('format');
        const maxSizeInput = document.getElementById('maxSize');
        const processBtn = document.getElementById('processBtn');
        const preview = document.getElementById('preview');
        const previewImg = document.getElementById('previewImg');
        const downloadLink = document.getElementById('downloadLink');
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');

        let img = new Image();
        let originalWidth = 0;
        let originalHeight = 0;

        // Upload area click handler
        uploadArea.addEventListener('click', () => {
            fileInput.click();
        });

        // File input change handler
        fileInput.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (!file) return;
            
            if (!file.type.match('image.*')) {
                alert('Please select an image file');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(event) {
                img.src = event.target.result;
            }
            reader.readAsDataURL(file);
        });

        img.onload = () => {
            originalWidth = img.width;
            originalHeight = img.height;
            
            widthInput.value = originalWidth;
            heightInput.value = originalHeight;
            
            processBtn.disabled = false;
            preview.style.display = 'block';
            previewImg.src = img.src;
        }

        // Process button handler
        processBtn.addEventListener('click', () => {
            let w = parseInt(widthInput.value) || originalWidth;
            let h = parseInt(heightInput.value) || originalHeight;
            
            // Validate dimensions
            if (w <= 0 || h <= 0) {
                alert('Please enter valid dimensions');
                return;
            }
            
            canvas.width = w;
            canvas.height = h;
            ctx.drawImage(img, 0, 0, w, h);

            const format = formatSelect.value;
            let maxSizeKB = parseInt(maxSizeInput.value) || 0;

            function compress(quality = 0.9) {
                canvas.toBlob((blob) => {
                    let sizeKB = blob.size / 1024;
                    
                    if (maxSizeKB && sizeKB > maxSizeKB && quality > 0.1 && format !== 'image/png') {
                        compress(quality - 0.05);
                    } else {
                        const url = URL.createObjectURL(blob);
                        previewImg.src = url;
                        downloadLink.href = url;
                        downloadLink.download = 'resized_image.' + format.split('/')[1];
                        downloadLink.style.display = 'inline-block';
                        downloadLink.textContent = `Download (${(blob.size/1024).toFixed(1)} KB)`;
                    }
                }, format, quality);
            }

            compress();
        });
    </script>
</body>
</html>
