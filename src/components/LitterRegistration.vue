<template>
    <div class="litter-registration">
      <h2>Litter Registration</h2>
      <form @submit.prevent="registerLitter">
        <label for="motherName">Mother's Name:</label>
        <input type="text" id="motherName" v-model="motherName" required>
  
        <label for="fatherName">Father's Name:</label>
        <input type="text" id="fatherName" v-model="fatherName" required>
  
        <label for="numberOfPuppies">Number of Puppies:</label>
        <input type="number" id="numberOfPuppies" v-model.number="numberOfPuppies" required>
  
        <label for="breed">Breed:</label>
        <input type="text" id="breed" v-model="breed" required>
        
        <button type="submit">Register Litter</button>
      </form>
    </div>
  </template>
  
  <script>
  import { ethers } from "ethers";
  
  export default {
    data() {
      return {
        motherName: "",
        fatherName: "",
        numberOfPuppies: 0,
        breed: "",
        contractAddress: "0xcf7ed3acca5a467e9e704c703e8d87f634fb0fc9",
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
          "name": "motherName",
          "type": "string"
        },
        {
          "indexed": false,
          "internalType": "string",
          "name": "fatherName",
          "type": "string"
        },
        {
          "indexed": false,
          "internalType": "uint256",
          "name": "numberOfPuppies",
          "type": "uint256"
        },
        {
          "indexed": false,
          "internalType": "string",
          "name": "breed",
          "type": "string"
        },
        {
          "indexed": false,
          "internalType": "address",
          "name": "owner",
          "type": "address"
        }
      ],
      "name": "LitterRegistered",
      "type": "event"
    },
    {
      "inputs": [
        {
          "internalType": "uint256",
          "name": "_id",
          "type": "uint256"
        }
      ],
      "name": "getLitter",
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
          "internalType": "uint256",
          "name": "",
          "type": "uint256"
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
      "inputs": [
        {
          "internalType": "uint256",
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "litters",
      "outputs": [
        {
          "internalType": "string",
          "name": "motherName",
          "type": "string"
        },
        {
          "internalType": "string",
          "name": "fatherName",
          "type": "string"
        },
        {
          "internalType": "uint256",
          "name": "numberOfPuppies",
          "type": "uint256"
        },
        {
          "internalType": "string",
          "name": "breed",
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
      "inputs": [],
      "name": "numLitters",
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
          "name": "_motherName",
          "type": "string"
        },
        {
          "internalType": "string",
          "name": "_fatherName",
          "type": "string"
        },
        {
          "internalType": "uint256",
          "name": "_numberOfPuppies",
          "type": "uint256"
        },
        {
          "internalType": "string",
          "name": "_breed",
          "type": "string"
        }
      ],
      "name": "registerLitter",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    }
  ], // Add your contract ABI here
        signer: null,
        contract: null,
      };
    },
    methods: {
      async registerLitter() {
        try {
          this.signer = new ethers.JsonRpcSigner("http://127.0.0.1:8545");
          this.contract = new ethers.Contract(this.contractAddress, this.abi, this.signer);
  
          await this.contract.registerLitter(this.motherName, this.fatherName, this.numberOfPuppies, this.breed);
          
          alert("Litter registered successfully!");
        } catch (error) {
          console.error("Error registering litter:", error.message);
          alert("Error registering litter: " + error.message);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .litter-registration {
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
  
  input {
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
  