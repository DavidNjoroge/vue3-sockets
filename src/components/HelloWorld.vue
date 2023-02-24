<template>
  <v-card>
    <v-card-title>Customer list app</v-card-title>
    <v-card-text>
      <ul>

      </ul>
    </v-card-text>
  </v-card>
</template>
<script>
import { ref } from 'vue';

export default {
  setup() {
    const eventsFromServer = ref([
      { name: 'Michael', age: 34, gender: 'M' },
      { name: 'Gideon', age: 25, gender: 'F' },
      { name: 'Stuart', age: 19, gender: 'M' },
    ]);
    // Paste actual production url line 24
    const serverUrl =
      process.env.NODE_ENV === 'production'
        ? 'YOUR_WEBSCOKET URL' 
        : 'ws://localhost:3001';

    const socket = new WebSocket(serverUrl);
    socket.addEventListener('open', (event) => {
      // socket.send('https://nytimes.com');
      console.log(event);
    });

    socket.addEventListener('message', function (event) {
      console.log('Message from server ', event.data);
      eventsFromServer.value.push({ time: new Date(), data: event.data });
    });
  }

}
</script>