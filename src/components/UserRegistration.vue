<template>
    <div class="user-registration">
      <h2>User Registration</h2>
      <form @submit.prevent="registerUser">
        <label for="username">Username:</label>
        <input type="text" id="username" v-model="username" required>
        
        <button type="submit">Register User</button>
      </form>
    </div>
  </template>
  
  <script>
  import { ethers } from "ethers";
  
  export default {
    data() {
      return {
        username: "",
        contractAddress: "0x5fbdb2315678afecb367f032d93f642f64180aa3",
        abi: [
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": false,
          "internalType": "string",
          "name": "username",
          "type": "string"
        },
        {
          "indexed": false,
          "internalType": "address",
          "name": "userAddress",
          "type": "address"
        }
      ],
      "name": "UserRegistered",
      "type": "event"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "_userAddress",
          "type": "address"
        }
      ],
      "name": "getUser",
      "outputs": [
        {
          "internalType": "string",
          "name": "",
          "type": "string"
        }
      ],
      "stateMutability": "view",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "string",
          "name": "_username",
          "type": "string"
        }
      ],
      "name": "registerUser",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "",
          "type": "address"
        }
      ],
      "name": "users",
      "outputs": [
        {
          "internalType": "string",
          "name": "username",
          "type": "string"
        },
        {
          "internalType": "address",
          "name": "userAddress",
          "type": "address"
        }
      ],
      "stateMutability": "view",
      "type": "function"
    }
  ], // Add your contract ABI here
        signer: null,
        contract: null,
      };
    },
    methods: {
      async registerUser() {
        try {
          console.log(this.contractAddress+"<--------CONTRACT ADDRESS")
          this.signer = new ethers.JsonRpcSigner("http://127.0.0.1:8545");
          this.contract = new ethers.Contract(this.contractAddress, this.abi, this.signer);
  
          await this.contract.registerUser(this.username);
          
          alert("User registered successfully!");
        } catch (error) {
          console.error("Error registering user:", error.message);
          alert("Error registering user: " + error.message);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .user-registration {
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
  