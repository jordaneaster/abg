<template>
  <div>
    <button @click="toggleTicker" class="toggle-button">Toggle Ticker</button>
    <div v-if="showTicker" class="ticker-container">
      <div v-for="(transaction, index) in transactions" :key="index" class="transaction-item">{{ transactionDetails(transaction) }}</div>
    </div>
    <span v-if="notificationCount > 0" class="notification-count">{{ notificationCount }}</span>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  props: {
    provider: Object // Define a prop to receive the provider from the parent component
  },
  setup(props) {
    const showTicker = ref(true);
    const transactions = ref([]);
    const notificationCount = ref(0);

    const toggleTicker = () => {
      showTicker.value = !showTicker.value;
    };

    const getTransactionDetails = async (transactionHash) => {
      const transaction = await props.provider.getTransaction(transactionHash);
      const receipt = await props.provider.getTransactionReceipt(transactionHash);
      return { transaction, receipt };
    };

    const transactionDetails = async (transactionHash) => {
      try {
        const { transaction, receipt } = await getTransactionDetails(transactionHash);
        return `Transaction Hash: ${transaction.hash}, Status: ${receipt ? 'Confirmed' : 'Pending'}`;
      } catch (error) {
        console.error('Error fetching transaction details:', error);
        return 'Error fetching transaction details';
      }
    };

    onMounted(async () => {
      try {
        props.on('block', async (blockNumber) => {
          const block = await props.provider.getBlock(blockNumber);
          const transactionsInBlock = block.transactions;
          transactions.value.push(...transactionsInBlock);
          notificationCount.value += transactionsInBlock.length;
        });
      } catch (error) {
        console.error('Error setting up block listener:', error);
      }
    });

    return {
      showTicker,
      transactions,
      notificationCount,
      toggleTicker,
      transactionDetails
    };
  }
};
</script>

<style scoped>
.toggle-button {
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 20px;
  padding: 10px 20px;
  cursor: pointer;
}

.ticker-container {
  background-color: #f0f0f0;
  border-radius: 20px;
  margin-top: 10px;
  padding: 10px;
}

.transaction-item {
  margin-bottom: 5px;
}

.notification-count {
  background-color: #ff6347;
  color: #fff;
  border-radius: 50%;
  padding: 5px 10px;
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>
