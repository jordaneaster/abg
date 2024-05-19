<template>
  <div class="bloodline-database">
    <h2>Bloodline Database</h2>
    <form @submit.prevent="handleRegistration">
      <label for="name">Name:</label>
      <input type="text" id="name" v-model="name" required>

      <label for="description">Description:</label>
      <textarea id="description" v-model="description" required></textarea>
      
      <button type="submit">Register Bloodline</button>
    </form>

    <!-- Pass props to the Ticker component -->
    <Ticker :provider="provider" />
  </div>
</template>


<script>
import { ethers } from "ethers";
import Ticker from "./Ticker.vue"; // Import the Ticker component

export default {
  components: {
    Ticker // Register the Ticker component
  },
  data() {
    return {
      provider: "",
      name: "",
      description: "",
      contractAddress: "0xDc64a140Aa3E981100a9becA4E685f962f0cF6C9", // Your contract address
      abi: [
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "internalType": "uint256",
          "name": "id",
          "type": "uint256"
        },
        {
          "indexed": false,
          "internalType": "string",
          "name": "name",
          "type": "string"
        },
        {
          "indexed": false,
          "internalType": "string",
          "name": "description",
          "type": "string"
        },
        {
          "indexed": false,
          "internalType": "address",
          "name": "owner",
          "type": "address"
        }
      ],
      "name": "BloodlineRegistered",
      "type": "event"
    },
    {
      "inputs": [
        {
          "internalType": "uint256",
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "bloodlines",
      "outputs": [
        {
          "internalType": "string",
          "name": "name",
          "type": "string"
        },
        {
          "internalType": "string",
          "name": "description",
          "type": "string"
        },
        {
          "internalType": "address",
          "name": "owner",
          "type": "address"
        },
        {
          "internalType": "bool",
          "name": "isRegistered",
          "type": "bool"
        }
      ],
      "stateMutability": "view",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "uint256",
          "name": "_id",
          "type": "uint256"
        }
      ],
      "name": "getBloodline",
      "outputs": [
        {
          "internalType": "string",
          "name": "",
          "type": "string"
        },
        {
          "internalType": "string",
          "name": "",
          "type": "string"
        },
        {
          "internalType": "address",
          "name": "",
          "type": "address"
        },
        {
          "internalType": "bool",
          "name": "",
          "type": "bool"
        }
      ],
      "stateMutability": "view",
      "type": "function"
    },
    {
      "inputs": [],
      "name": "numBloodlines",
      "outputs": [
        {
          "internalType": "uint256",
          "name": "",
          "type": "uint256"
        }
      ],
      "stateMutability": "view",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "string",
          "name": "_name",
          "type": "string"
        },
        {
          "internalType": "string",
          "name": "_description",
          "type": "string"
        }
      ],
      "name": "registerBloodline",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    }
  ],
    };
  },
  methods: {
    async getSigner() {
      try {
        const provider = new ethers.JsonRpcProvider("http://127.0.0.1:8545");
        // No need for private key here as we're using MetaMask or another provider
        return provider.getSigner(); // Return the signer
      } catch (error) {
        console.error("Error getting signer: " + error.message);
        throw error;
      }
    },
    async registerBloodline(signer) {
      try {
        const contract = new ethers.Contract(this.contractAddress, this.abi, signer);
        console.log("Contract address " + this.contractAddress);
        console.log("Signer: ", signer);
        const tx = await contract.registerBloodline(this.name, this.description);
        await tx.wait(); // Wait for the transaction to be mined
        const receipt = await signer.provider.getTransactionReceipt(tx.hash);
        console.log("Transaction Receipt:", receipt);
        alert("Bloodline registered successfully!");
      } catch (error) {
        console.error("Error registering bloodline: " + error.message);
        alert("Error registering bloodline: " + error.message);
      }
    },
    async handleRegistration() {
      try {
        const signer = await this.getSigner();
        await this.registerBloodline(signer);
      } catch (error) {
        console.error("Error handling registration: " + error.message);
        alert("Error handling registration: " + error.message);
      }
    }
  }
};
</script>

  <style scoped>
  .bloodline-database {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    border-radius: 10px;
    background-color: #f0f0f0;
  }
  
  label {
    display: block;
    margin-bottom: 10px;
  }
  
  input, textarea {
    width: 100%;
    padding: 8px;
    margin-bottom: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  
  button {
    width: 100%;
    padding: 10px;
    border: none;
    border-radius: 5px;
    background-color: #007bff;
    color: #fff;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #0056b3;
  }
  </style>
  