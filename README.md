# R.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>QR Code Generator</title>

    <!-- QR Code Library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>

    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
        }

        body {
            min-height: 100vh;
            background: #141726;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            width: 100%;
            max-width: 500px;
            background: #1c2033;
            padding: 30px;
            border-radius: 18px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.4);
            text-align: center;
        }

        h1 {
            color: #33ccff;
            font-size: 28px;
            margin-bottom: 25px;
            letter-spacing: 1px;
        }

        input {
            width: 100%;
            height: 55px;
            padding: 0 15px;
            font-size: 17px;
            color: white;
            background: #262b42;
            border: 2px solid #444866;
            border-radius: 8px;
            outline: none;
            margin-bottom: 15px;
        }

        input:focus {
            border-color: #33ccff;
        }

        input::placeholder {
            color: #999;
        }

        button {
            width: 100%;
            height: 55px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            margin-bottom: 15px;
            transition: 0.2s;
        }

        .generate {
            background: #33ccff;
            color: #081018;
        }

        .generate:hover {
            background: #66d9ff;
            transform: translateY(-2px);
        }

        .save {
            background: #e6cc4a;
            color: #151515;
        }

        .save:hover {
            background: #f3dc65;
            transform: translateY(-2px);
        }

        #qrBox {
            width: 320px;
            min-height: 320px;
            margin: 15px auto;
            padding: 10px;
            background: white;
            border-radius: 10px;

            display: none;
            align-items: center;
            justify-content: center;
        }

        #qrBox img {
            width: 300px;
            height: 300px;
        }

        #status {
            color: #e6cc4a;
            font-size: 16px;
            margin: 15px 0;
            min-height: 20px;
        }

        .footer {
            margin-top: 15px;
            color: #777;
            font-size: 12px;
        }

        @media (max-width: 450px) {

            .container {
                padding: 22px;
            }

            h1 {
                font-size: 24px;
            }

            #qrBox {
                width: 280px;
                min-height: 280px;
            }

            #qrBox img {
                width: 250px;
                height: 250px;
            }
        }
    </style>
</head>

<body>

    <div class="container">

        <h1>QR CODE GENERATOR</h1>

        <input
            type="text"
            id="qrText"
            placeholder="Enter text, URL, phone number, etc."
        >

        <button class="generate" onclick="generateQR()">
            GENERATE QR CODE
        </button>

        <div id="qrBox">
            <div id="qrcode"></div>
        </div>

        <div id="status">
            Enter something and generate QR
        </div>

        <button class="save" onclick="saveQR()">
            SAVE QR CODE
        </button>

        <div class="footer">
            QR Code Generator • HTML Version
        </div>

    </div>


    <script>

        let qrGenerated = false;

        function generateQR() {

            const text = document.getElementById("qrText").value.trim();
            const status = document.getElementById("status");
            const qrBox = document.getElementById("qrBox");
            const qrCode = document.getElementById("qrcode");

            if (text === "") {
                status.innerText = "Please enter some text!";
                return;
            }

            // Clear previous QR
            qrCode.innerHTML = "";

            try {

                new QRCode(qrCode, {
                    text: text,
                    width: 300,
                    height: 300,
                    colorDark: "#000000",
                    colorLight: "#ffffff",
                    correctLevel: QRCode.CorrectLevel.M
                });

                qrBox.style.display = "flex";

                status.innerText =
                    "QR code generated successfully!";

                qrGenerated = true;

            } catch (error) {

                status.innerText =
                    "Error generating QR code.";

                qrGenerated = false;
            }
        }


        function saveQR() {

            const status = document.getElementById("status");

            if (!qrGenerated) {

                status.innerText =
                    "Generate a QR code first!";

                return;
            }

            const qrImage =
                document.querySelector("#qrcode img");

            if (!qrImage) {

                status.innerText =
                    "QR code image not found!";

                return;
            }

            const link = document.createElement("a");

            link.href = qrImage.src;
            link.download = "QR_Code.png";

            document.body.appendChild(link);

            link.click();

            document.body.removeChild(link);

            status.innerText =
                "QR code saved successfully!";
        }


        // Press Enter to generate
        document
            .getElementById("qrText")
            .addEventListener("keydown", function(event) {

                if (event.key === "Enter") {
                    generateQR();
                }

            });

    </script>

</body>
</html>
