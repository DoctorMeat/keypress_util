<template>
  <div @keydown="logKeyPress" tabindex="0">
    <h1>Key Press Logger</h1>
    <p>Press any key to see the log below:</p>
    <ul>
      <li v-for="(key, index) in keyLogs" :key="index">
        {{ key }}
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from "vue";

export default {
  setup() {
    const keyLogs = ref([]); // Reactive variable to store key logs

    const logKeyPress = (event) => {
      const key = event.key; // Capture the key pressed
      keyLogs.value.push(key); // Add the key to the logs
      if (keyLogs.value.length > 10) {
        // Limit the log size to 10 entries
        keyLogs.value.shift();
      }
    };

    onMounted(() => {
      // Register the global keydown event listener
      window.addEventListener("keydown", logKeyPress);
    });

    onBeforeUnmount(() => {
      // Clean up the listener
      window.removeEventListener("keydown", logKeyPress);
    });

    return {
      keyLogs, // Expose reactive variable to the template
      logKeyPress, // Expose the method (in case you want to use it locally)
    };
  },
};
</script>

<style>
/* Add some basic styling */
div {
  text-align: center;
  margin: 50px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin: 5px 0;
  font-family: monospace;
}
</style>
