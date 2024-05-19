<template>
    <div class="single-dog-registration">
      <h2>Single Dog Registration</h2>
      <form @submit.prevent="registerDog">
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="dogName" required>
        
        <label for="breed">Breed:</label>
        <input type="text" id="breed" v-model="breed" required>
        
        <label for="location">Location:</label>
        <input type="text" id="location" v-model="location" required>
        
        <label for="age">Age (in months):</label>
        <input type="number" id="age" v-model.number="ageInMonths" required>
        
        <label for="photo1">Photo 1 URL:</label>
        <input type="url" id="photo1" v-model="photo1" required>
        
        <label for="photo2">Photo 2 URL:</label>
        <input type="url" id="photo2" v-model="photo2" required>
        
        <button type="submit">Register Dog</button>
      </form>
    </div>
  </template>
  
  <script>
  import { ethers } from "ethers";
  
  export default {
    data() {
      return {
        dogName: "",
        breed: "",
        location: "",
        ageInMonths: 0,
        photo1: "",
        photo2: "",
        contractAddress: "0xe7f1725e7734ce288f8367e1bb143e90bb3f0512",
        abi:  [
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
          "internalType": "address",
          "name": "owner",
          "type": "address"
        }
      ],
      "name": "DogRegistered",
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
      "name": "dogs",
      "outputs": [
        {
          "internalType": "string",
          "name": "name",
          "type": "string"
        },
        {
          "internalType": "string",
          "name": "breed",
          "type": "string"
        },
        {
          "internalType": "string",
          "name": "location",
          "type": "string"
        },
        {
          "internalType": "uint256",
          "name": "ageInMonths",
          "type": "uint256"
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
        },
        {
          "internalType": "string",
          "name": "photo1",
          "type": "string"
        },
        {
          "internalType": "string",
          "name": "photo2",
          "type": "string"
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
      "name": "getDog",
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
          "internalType": "address",
          "name": "",
          "type": "address"
        },
        {
          "internalType": "bool",
          "name": "",
          "type": "bool"
        },
        {
          "internalType": "string",
          "name": "",
          "type": "string"
        },
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
      "inputs": [],
      "name": "numDogs",
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
          "name": "_breed",
          "type": "string"
        },
        {
          "internalType": "string",
          "name": "_location",
          "type": "string"
        },
        {
          "internalType": "uint256",
          "name": "_ageInMonths",
          "type": "uint256"
        },
        {
          "internalType": "string",
          "name": "_photo1",
          "type": "string"
        },
        {
          "internalType": "string",
          "name": "_photo2",
          "type": "string"
        }
      ],
      "name": "registerDog",
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
      async registerDog() {
        try {
          this.signer = new ethers.JsonRpcSigner("http://127.0.0.1:8545");
          this.contract = new ethers.Contract(this.contractAddress, this.abi, this.signer);
  
          await this.contract.registerDog(this.dogName, this.breed, this.location, this.ageInMonths, this.photo1, this.photo2);
          
          alert("Dog registered successfully!");
        } catch (error) {
          console.error("Error registering dog:", error.message);
          alert("Error registering dog: " + error.message);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .single-dog-registration {
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
  