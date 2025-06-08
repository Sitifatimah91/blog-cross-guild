!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Beli Berry Coin</title>
  <script src="https://cdn.jsdelivr.net/npm/web3@latest/dist/web3.min.js"></script>
  <style>
    body {
      background-color: #000;
      color: #fff;
      font-family: Arial, sans-serif;
      padding: 2em;
      text-align: center;
    }

    button {
      background-color: #d1a3f8;
      color: #000;
      padding: 1em 2em;
      margin: 1em;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      cursor: pointer;
    }

    button:hover {
      background-color: #e0c3fc;
    }

    #status {
      margin-top: 1em;
      font-weight: bold;
    }

    #txLink {
      margin-top: 0.5em;
      color: #9cf;
    }
  </style>
</head>
<body>
  <h1>Beli Berry Coin</h1>
  <p>Kirim 0.01 ETH untuk mendapatkan 10.000 BERRY</p>

  <button onclick="connectWallet()">Hubungkan Wallet</button>
  <button onclick="sendTransaction()">Beli Sekarang</button>

  <div id="status"></div>
  <div id="txLink"></div>

  <script>
    let web3;
    let account;
    const targetNetworkId = "0x1"; // Ethereum Mainnet
    const toAddress = "0x2AaB533E723F614b1f1deA8Bc995059655E5e716";
    const amountInEth = "0.01";

    async function connectWallet() {
      if (window.ethereum) {
        web3 = new Web3(window.ethereum);
        try {
          const chainId = await window.ethereum.request({ method: 'eth_chainId' });
          if (chainId !== targetNetworkId) {
            document.getElementById("status").innerText = "Silakan pindah ke jaringan Ethereum Mainnet.";
            return;
          }

          const accounts = await ethereum.request({ method: 'eth_requestAccounts' });
          account = accounts[0];
          document.getElementById("status").innerText = "Wallet terhubung: " + account;
        } catch (err) {
          document.getElementById("status").innerText = "Gagal menghubungkan wallet: " + err.message;
        }
      } else {
        alert("MetaMask belum terpasang.");
      }
    }

    async function sendTransaction() {
      if (!account) {
        alert("Silakan hubungkan wallet terlebih dahulu.");
        return;
      }

      try {
        const tx = await web3.eth.sendTransaction({
          from: account,
          to: toAddress,
          value: web3.utils.toWei(amountInEth, "ether")
        });

        document.getElementById("status").innerText = "Transaksi berhasil!";
        document.getElementById("txLink").innerHTML =
          `TX Hash: <a href="https://etherscan.io/tx/${tx.transactionHash}" target="_blank">${tx.transactionHash}</a>`;
      } catch (error) {
        document.getElementById("status").innerText = "Transaksi gagal: " + error.message;
      }
    }
  </script>
</body>
</html>
