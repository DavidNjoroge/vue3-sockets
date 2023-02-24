<template>
  <div>

    <v-card>
      <v-card-title class="d-flex justify-space-between">
        <p>Customer list app</p>
        <v-badge
          content="6"
          class="pt-4">

          <v-icon icon="mdi-bell" size="x-large"></v-icon>
        </v-badge>

      </v-card-title>
      <v-card-text>

      </v-card-text>

    </v-card>
    <v-table>
      <thead>
      <tr>
        <th class="text-left">
          Name
        </th>
        <th class="text-left">
          age
        </th>
        <th class="text-left">
          gender
        </th>
        <th class="text-left">
          phone
        </th>
        <th class="text-left">
          created_at
        </th>
        <th class="text-left">
          updated_at
        </th>
      </tr>
      </thead>
      <tbody>
      <tr
        v-for="customer in eventsFromServer"
        :key="customer.id"
      >
        <td>{{ customer.name }}</td>
        <td>{{ customer.age }}</td>
        <td>{{ customer.gender }}</td>
        <td>{{ customer.phone }}</td>
        <td>{{ customer.created_at }}</td>
        <td>{{ customer.updated_at }}</td>
      </tr>
      </tbody>
    </v-table>
  </div>
</template>
<script>
import { ref } from 'vue';

export default {
  setup() {
    const eventsFromServer = ref([
      { id: 1, name: 'Michael', age: 34, gender: 'M', email: 'dav@mail.com', phone: '077777777', created_at: '2022', updated_at: '2022'  },
      { id: 2,name: 'Gideon', age: 25, gender: 'F', email: 'dav@mail.com', phone: '077777777', created_at: '2022', updated_at: '2022' },
      { id: 3,name: 'Stuart', age: 19, gender: 'M', email: 'dav@mail.com', phone: '077777777', created_at: '2022', updated_at: '2022'  },
    ]);
    // Paste actual production url line 24
    const serverUrl = 'ws://localhost:3000/cable';

    const socket = new WebSocket(serverUrl);

    socket.onopen = function(){

      console.log('Subscribe to the channel');
      socket.send(JSON.stringify({"command": "subscribe","identifier":"{\"channel\":\"CustomersChannel\"}"}))
    }

    socket.onmessage = function(msg) {
      let messageData = JSON.parse(msg.data).message

      if(!!messageData && typeof messageData !== "number") {
        eventsFromServer.value.push( messageData.customer);
      }
    }

    return { eventsFromServer };
  },
  mounted() {
    // this.eventsFromServer.
    fetch('http://localhost:3000/api/customers')
      .then((response) => response.json())
      .then((data) => {
        console.log('sdgfsdgdf',data.customers )
        this.eventsFromServer = data.customers
        console.log(data)
      });
  }

}
</script>
