<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Claim Your NFT</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #121212;
      color: white;
      text-align: center;
      margin-top: 100px;
    }
    button {
      font-size: 1.2rem;
      padding: 15px 30px;
      margin: 10px;
      cursor: pointer;
      border-radius: 8px;
      border: none;
      font-weight: bold;
    }
    #connect-wallet {
      background-color: #32CD32;
      color: black;
    }
    #claim-nft {
      background-color: #FFD700;
      color: black;
      display: none;
    }
    #status {
      margin-top: 20px;
      min-height: 1.5rem;
    }
  </style>
  <script src="https://cdn.jsdelivr.net/npm/@thirdweb-dev/sdk@3.7.0/dist/browser.umd.min.js"></script>
</head>
<body>

  <h1>Claim Your NFT</h1>
  <button id="connect-wallet">Connect Wallet</button>
  <p id="wallet-address"></p>
  <button id="claim-nft">Claim NFT</button>
  <p id="status"></p>

  <script>
    const contractAddress = "0x6010d15cEA980a0e34C9e3b1c5409862E9b9CDeE"; // your contract address
    const polygonChainId = "0x89"; // Polygon mainnet, change if needed

    let sdk;
    let contract;
    let signer;
    let userAddress;

    const connectBtn = document.getElementById("connect-wallet");
    const claimBtn = document.getElementById("claim-nft");
    const walletAddressElem = document.getElementById("wallet-address");
    const statusElem = document.getElementById("status");

    async function connectWallet() {
      if (!window.ethereum) {
        alert("MetaMask is not installed!");
        return;
      }

      try {
        // Request accounts
        const accounts = await window.ethereum.request({ method: "eth_requestAccounts" });
        userAddress = accounts[0];
        walletAddressElem.innerText = `Connected: ${userAddress}`;

        // Check network and switch if needed
        const chainId = await window.ethereum.request({ method: "eth_chainId" });
        if (chainId !== polygonChainId) {
          try {
            await window.ethereum.request({
              method: "wallet_switchEthereumChain",
              params: [{ chainId: polygonChainId }],
            });
            statusElem.innerText = "Switched to Polygon Network";
          } catch (switchError) {
            statusElem.innerText = "Please switch to Polygon network manually in MetaMask";
            return;
          }
        }

        // Initialize Thirdweb SDK with provider
        sdk = new ThirdwebSDK(window.ethereum);
        contract = await sdk.getContract(contractAddress);

        claimBtn.style.display = "inline-block";
        connectBtn.disabled = true;
      } catch (error) {
        statusElem.innerText = "Connection failed: " + error.message;
      }
    }

    async function claimNFT() {
      if (!contract) {
        alert("Contract not initialized");
        return;
      }
      statusElem.innerText = "Claiming NFT... please wait";

      try {
        // For ERC-1155 claim 1 token - adjust method & quanti
