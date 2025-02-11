<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مشروع الاكتتاب BMS</title>
    <script src="https://cdn.jsdelivr.net/npm/web3@1.6.1/dist/web3.min.js"></script>
    <style>
        body { font-family: Arial, sans-serif; }
        .container { width: 50%; margin: auto; text-align: center; }
        button { padding: 10px 20px; margin: 20px; background-color: #ff7f00; color: white; border: none; cursor: pointer; }
        button:hover { background-color: #e06c00; }
        #walletAddress { margin-top: 20px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>الاكتتاب لعملة BMS</h1>
        <button onclick="connectMetaMask()">اتصل بـ MetaMask</button>
        <h2>اشترك في الاكتتاب</h2>
        <input type="number" id="amount" placeholder="المبلغ بالـ ETH" />
        <button onclick="buyTokens()">شراء عملة BMS</button>
        <p id="walletAddress"></p>
        <p id="balance"></p>
    </div>

    <script>
        const contractAddress = '0xYourContractAddress'; // استبدل بعنوان عقدك الذكي
        const contractABI = [ /* ABI الخاصة بعقد BMS */ ];

        let web3;
        let userAccount;
        let bmsContract;

        // الاتصال بـ MetaMask
        async function connectMetaMask() {
            if (typeof window.ethereum !== 'undefined') {
                web3 = new Web3(window.ethereum);
                await window.ethereum.request({ method: 'eth_requestAccounts' });
                const accounts = await web3.eth.getAccounts();
                userAccount = accounts[0];
                bmsContract = new web3.eth.Contract(contractABI, contractAddress);
                document.getElementById('walletAddress').innerText = عنوان المحفظة: ${userAccount};
                checkBalance();
            } else {
                alert("يرجى تثبيت MetaMask أولاً!");
            }
        }

        // شراء عملة BMS
        async function buyTokens() {
            const amountInEth = document.getElementById('amount').value;
            if (amountInEth > 0) {
                const amountInWei = web3.utils.toWei(amountInEth, 'ether');
                try {
                    await bmsContract.methods.buyTokens().send({ from: userAccount, value: amountInWei });
                    alert('تم شراء عملة BMS بنجاح');
                } catch (error) {
                    console.error("خطأ في المعاملة:", error);
                    alert("حدث خطأ أثناء شراء العملة");
                }
            } else {
                alert('الرجاء إدخال مبلغ صالح');
            }
        }

        // التحقق من رصيد المستخدم
        async function checkBalance() {
            const balance = await web3.eth.getBalance(userAccount);
            const balanceInEth = web3.utils.fromWei(balance, 'ether');
            document.getElementById('balance').innerText = رصيدك من ETH: ${balanceInEth} ETH;
        }
    </script>
</body>
</html>
