<template>
  <div class="bloodline-database">
    <h2>Bloodline Database</h2>
    <form @submit.prevent="handleRegistration">
      <label for="breed">Breed:</label>
      <select id="breed" v-model="selectedBreed" required>
        <option v-for="breed in breeds" :key="breed.name" :value="breed.name">{{ breed.name }}</option>
      </select>

      <label for="description">Description:</label>
      <textarea id="description" v-model="description" required></textarea>
      
      <button type="submit">Register Bloodline</button>
    </form>

    <Ticker v-if="provider" :provider="provider" />

    <div v-if="receipt" class="transaction-receipt">
      Transaction Receipt: {{ receipt }}
    </div>
  </div>
</template>

<script>
import { ethers } from "ethers";
import Ticker from "./TransactionTicker.vue";

export default {
  components: {
    Ticker
  },
  data() {
    return {
      provider: null,
      selectedBreed: "",
      description: "",
      contractAddress: "0xDc64a140Aa3E981100a9becA4E685f962f0cF6C9",
      abi: [/* Your ABI */],
      receipt: null,
      breeds: [
        { name: "American Bulldog", description: "The “Bully” American Bulldog Standard..." },
        { name: "American Bully", description: "The American Bully should give the impression..." },
        { name: "American Pit Bull Terrier", description: "The American Pit Bull Terrier has been around..." },
        { name: "American Staffordshire Terrier", description: "The American Staffordshire Terrier should present itself..." },
        { name: "Boston Terrier", description: "The Colored Boston Terrier is a lively, highly intelligent..." },
        { name: "Bull Terrier", description: "The Bull Terrier is a stocky, strongly built, well balanced..." },
        { name: "Cane Corso", description: "Ancient Italian breed medium-large size Molossus Dog..." },
        { name: "Dogo Argentino", description: "Molossian normal type, mesomorphic and macrothalic..." },
        { name: "English Bulldog", description: "The perfect Bulldog must be of medium size and smooth coat..." },
        { name: "French Bulldog", description: "The French Bulldog has the appearance of an active, intelligent..." },
        { name: "Minature Bull Terrier", description: "The Miniature Bull Terrier, a smaller compact version..." },
        { name: "New Age Bulldog", description: "The New Age Bulldog is a small to medium size dog..." },
        { name: "Olde English Bulldogge", description: "The ideal Olde English Bulldogge is a loyal, courageous dog..." },
        { name: "Pacific Bulldog", description: "The Pacific Bulldog is compact and powerful..." },
        { name: "Praiter", description: "" }, // Assuming no description provided
        { name: "Save-A-Bully", description: "The Save-A-Bully Class is a licensed conformation class..." },
        { name: "Shorty Bull", description: "Shorty Bulls® as they are often referred to are a compact..." }
      ]
    };
  },
  async mounted() {
    try {
      this.provider = ethers.getDefaultProvider();
    } catch (error) {
      console.error("Error getting provider: " + error.message);
    }
  },
  methods: {
    async registerBloodline(signer) {
      try {
        const contract = new ethers.Contract(this.contractAddress, this.abi, signer);
        const tx = await contract.registerBloodline(this.selectedBreed, this.description);
        await tx.wait();
        const receipt = await signer.provider.getTransactionReceipt(tx.hash);
        console.log("Transaction Receipt:", receipt);
        this.receipt = receipt;
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
    },
    async getSigner() {
      try {
        const provider = new ethers.JsonRpcProvider("http://127.0.0.1:8545");
        return provider.getSigner();
      } catch (error) {
        console.error("Error getting signer: " + error.message);
        throw error;
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

input, textarea, select {
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

.transaction-receipt {
  background-color: #4caf50;
  color: white;
  padding: 10px;
  border-radius: 5px;
  margin-top: 20px;
}
</style>
