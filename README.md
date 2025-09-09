<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Escáner QR</title>
    <script src="https://unpkg.com/html5-qrcode@2.3.4/html5-qrcode.min.js"></script>
    <style>
        body { margin: 0; padding: 0; background-color: #000; display: flex; justify-content: center; align-items: center; height: 100vh; }
        #reader { width: 100%; max-width: 400px; }
    </style>
</head>
<body>
    <div id="reader"></div>
    <script>
        const html5QrcodeScanner = new Html5QrcodeScanner("reader", { fps: 10, qrbox: { width: 250, height: 250 } }, false);
        
        let isProcessingScan = false;

        function onScanSuccess(decodedText, decodedResult) {
            if (isProcessingScan) {
                return;
            }
            
            isProcessingScan = true;
            console.log(`Código escaneado: ${decodedText}`);

            // Envía el texto escaneado a la página principal
            window.parent.postMessage({ type: 'qr-scan', data: decodedText }, '*');

            // Pausa la detección por 1.5 segundos
            setTimeout(() => {
                isProcessingScan = false;
            }, 1500);
        }

        function onScanFailure(error) {
            if (error.includes("No QR code found")) {
                return;
            }
            console.warn(`Error de escaneo: ${error}`);
        }

        html5QrcodeScanner.render(onScanSuccess, onScanFailure);
    </script>
</body>
</html>
