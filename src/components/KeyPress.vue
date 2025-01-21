<template>
  <div>
    <h1>Key Press Logger</h1>
    <p>Press any key to see the log below:</p>
    <ul>
      <li v-for="(entry, index) in keyLogs" :key="index">
        {{ entry }}
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
      let key = event.key; // Get the key pressed
      if (key === " ") {
        key = "Spacebar"; // Replace space key with "Spacebar"
      }
      const logEntry = `${key} (${event.type})`; // Add the event type (keydown or keyup)
      keyLogs.value.push(logEntry); // Add the entry to the logs
      if (keyLogs.value.length > 10) {
        // Limit the log size to 10 entries
        keyLogs.value.shift();
      }
    };

    onMounted(() => {
      // Register global keydown and keyup listeners
      window.addEventListener("keydown", logKeyPress);
      window.addEventListener("keyup", logKeyPress);
    });

    onBeforeUnmount(() => {
      // Remove global listeners when component is unmounted
      window.removeEventListener("keydown", logKeyPress);
      window.removeEventListener("keyup", logKeyPress);
    });

    return {
      keyLogs, // Expose reactive variable to the template
    };
  },
};
</script>

<style>
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
