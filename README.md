
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
        
        :root {
            --bg-color: #ffffff;
            --text-color: #333333;
            --border-color: #dddddd;
            --input-bg: #ffffff;
            --upload-bg: #f8f9fa;
            --upload-hover: #f0f8fa;
            --primary-color: #4db8c9;
            --primary-hover: #3aa5b7;
            --disabled-color: #cccccc;
            --footer-color: #666666;
            --shadow-color: rgba(0, 0, 0, 0.1);
            --heart-color: #e74c3c;
        }
        
        .dark-mode {
            --bg-color: #1a1a1a;
            --text-color: #ffffff;
            --border-color: #444444;
            --input-bg: #2d2d2d;
            --upload-bg: #2a2a2a;
            --upload-hover: #333333;
            --primary-color: #4db8c9;
            --primary-hover: #3aa5b7;
            --disabled-color: #555555;
            --footer-color: #999999;
            --shadow-color: rgba(0, 0, 0, 0.3);
            --heart-color: #e74c3c;
        }
        
        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
            background: var(--bg-color);
            color: var(--text-color);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
            transition: all 0.3s ease;
        }
        
        .container {
            width: 100%;
            max-width: 400px;
            text-align: center;
        }
        
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .logo {
            width: 180px;
        }
        
        .theme-toggle {
            background: var(--input-bg);
            border: 1px solid var(--border-color);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
            color: var(--text-color);
            transition: all 0.3s ease;
        }
        
        .theme-toggle:hover {
            background: var(--upload-hover);
            transform: scale(1.1);
        }
        
        .upload-area {
            border: 2px dashed var(--border-color);
            border-radius: 12px;
            padding: 40px 20px;
            margin-bottom: 25px;
            cursor: pointer;
            transition: all 0.3s;
            background: var(--upload-bg);
        }
        
        .upload-area:hover {
            border-color: var(--primary-color);
            background: var(--upload-hover);
        }
        
        .upload-icon {
            font-size: 40px;
            margin-bottom: 10px;
            opacity: 0.6;
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
            border: 1px solid var(--border-color);
            background: var(--input-bg);
            color: var(--text-color);
            font-size: 15px;
            width: 100%;
            transition: all 0.2s;
        }
        
        input:focus, select:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 2px rgba(77, 184, 201, 0.1);
        }
        
        input::placeholder {
            color: var(--footer-color);
        }
        
        select {
            appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' fill='%23666666' viewBox='0 0 16 16'%3E%3Cpath d='M7.247 11.14 2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z'/%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 15px center;
            background-size: 12px;
        }
        
        .dark-mode select {
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' fill='%23999999' viewBox='0 0 16 16'%3E%3Cpath d='M7.247 11.14 2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z'/%3E%3C/svg%3E");
        }
        
        button {
            background: var(--primary-color);
            border: none;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.2s;
            color: white;
        }
        
        button:hover {
            background: var(--primary-hover);
        }
        
        button:disabled {
            background: var(--disabled-color);
            cursor: not-allowed;
            color: var(--footer-color);
        }
        
        .preview {
            margin-top: 25px;
            display: none;
        }
        
        .preview img {
            max-width: 100%;
            max-height: 300px;
            border-radius: 8px;
            box-shadow: 0 4px 12px var(--shadow-color);
            border: 1px solid var(--border-color);
        }
        
        .download {
            display: none;
            margin-top: 20px;
            padding: 12px 20px;
            background: var(--primary-color);
            border-radius: 8px;
            text-decoration: none;
            color: white;
            font-weight: 500;
            transition: background 0.2s;
        }
        
        .download:hover {
            background: var(--primary-hover);
        }
        
        footer {
            margin-top: 40px;
            font-size: 14px;
            color: var(--footer-color);
        }
        
        .heartbeat {
            animation: heartbeat 1.5s ease-in-out infinite both;
            display: inline-block;
            color: var(--heart-color);
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
        <div class="header">
            <img src="https://i.ibb.co/m7ykF1Y/Photorific.png" alt="Photorific" class="logo" border="0">
            <button class="theme-toggle" id="themeToggle" title="Toggle dark mode">
                <span id="themeIcon">🌙</span>
            </button>
        </div>
        
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
        const themeToggle = document.getElementById('themeToggle');
        const themeIcon = document.getElementById('themeIcon');

        let img = new Image();
        let originalWidth = 0;
        let originalHeight = 0;
        let isDarkMode = false;

        // Theme toggle functionality
        themeToggle.addEventListener('click', () => {
            isDarkMode = !isDarkMode;
            document.body.classList.toggle('dark-mode', isDarkMode);
            
            if (isDarkMode) {
                themeIcon.textContent = '☀️';
            } else {
                themeIcon.textContent = '🌙';
            }
            
            // Save preference to localStorage
            localStorage.setItem('darkMode', isDarkMode);
        });

        // Check for saved theme preference
        const savedDarkMode = localStorage.getItem('darkMode') === 'true';
        if (savedDarkMode) {
            isDarkMode = true;
            document.body.classList.add('dark-mode');
            themeIcon.textContent = '☀️';
        }

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
