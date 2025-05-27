<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Claim Your NFT</title>
  <link rel="stylesheet" href="style.css" />
  <script src="https://cdn.jsdelivr.net/npm/@thirdweb-dev/sdk@latest/dist/browser.umd.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/ethers@5.7.2/dist/ethers.umd.min.js"></script>
</head>
<body>
  <div class="container">
    <h1>Claim Your NFT</h1>
    <button id="connect-wallet">Connect Wallet</button>
    <p id="wallet-address"></p>
    <button id="claim-nft" style="display:none">Claim NFT</button>
    <p id="status"></p>
  </div>
  <script src="script.js"></script>
</body>
</html>
