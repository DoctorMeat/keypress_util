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

      // Check if the key is any F key (F1 to F12) and prevent its default behavior
      if (/^F\d{1,2}$/.test(key)) {
        event.preventDefault(); // Disable default browser behavior for F keys
      }

      // Handle space key replacement
      if (key === " ") {
        key = "Spacebar";
      }

      // Add descriptive log entry for keydown or keyup
      const action = event.type === "keydown" ? "Key Pressed" : "Key Released";
      const logEntry = `${action}: ${key}`;
      keyLogs.value.push(logEntry);

      // Limit log size to 10 entries
      if (keyLogs.value.length > 10) {
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
