<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>We're Temporarily Closed</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(135deg, #6e8efb, #a777e3);
            font-family: 'Arial', sans-serif;
            color: #fff;
            text-align: center;
            width: 100%;
            box-sizing: border-box;
        }
        .container {
            max-width: 90%;
            width: 600px;
            padding: 2rem;
        }
        h1 {
            font-size: clamp(1.8rem, 5vw, 2.5rem);
            margin-bottom: 1.5rem;
        }
        p {
            font-size: clamp(1rem, 3vw, 1.2rem);
            line-height: 1.6;
            margin-bottom: 1.5rem;
        }
        .icon {
            font-size: clamp(2.5rem, 8vw, 4rem);
            margin-bottom: 1.5rem;
        }
        @media (max-width: 768px) {
            .container {
                padding: 1.5rem;
            }
            h1 {
                font-size: clamp(1.5rem, 4.5vw, 2rem);
            }
            p {
                font-size: clamp(0.9rem, 2.8vw, 1.1rem);
            }
            .icon {
                font-size: clamp(2rem, 7vw, 3rem);
            }
        }
        @media (max-width: 480px) {
            .container {
                padding: 1rem;
            }
            h1 {
                font-size: clamp(1.3rem, 4vw, 1.8rem);
            }
            p {
                font-size: clamp(0.8rem, 2.5vw, 1rem);
            }
            .icon {
                font-size: clamp(1.8rem, 6vw, 2.5rem);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="icon">🛠️</div>
        <h1>We're Temporarily Closed for Maintenance</h1>
        <p>We apologize for the inconvenience. Our team is working hard to fix bugs and roll out exciting new updates to enhance your experience. We'll be back online soon, better than ever!</p>
        <p>Thank you for your patience and understanding.</p>
    </div>
</body>
</html>
