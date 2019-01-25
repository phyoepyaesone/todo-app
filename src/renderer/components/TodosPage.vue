<template>
  <v-layout row justify-center>
    <h1>TODOs</h1>
    <v-btn
      color="#2e0a91"
      dark
      @click="dialog = true"
    >
      ADD
    </v-btn>
    <ul>
      <li v-for="item in items" :key="item">
        {{item}}
      </li>
    </ul>
    <v-dialog
      v-model="dialog"
      max-width="290"
    >
      <v-card>
        <v-card-title class="headline">ADD TODO</v-card-title>
         <v-card-text>
          <v-container grid-list-md>
            <v-layout wrap>
              <v-flex xs12 >
                <v-text-field label="TODO" required v-model="work"></v-text-field>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card-text>

        <v-card-actions>

          <v-btn
            color="red darken-2"
            flat="flat"
            @click="dialog = false"
          >
            Cancel
          </v-btn>

          <v-btn
            color="red darken-2"
            flat="flat"
            @click="save()"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-layout>
</template>

<script>
export default{
  data() {
    return {
      dialog: false,
      work: '',
      items: [],
      light: false,
    }
  },
  mounted() {
    this.$electron.ipcRenderer.on('todo-list', (event, data) => {
      console.log('todos:', data)
      this.items = data
    })
  },
  methods: {
    save() {
      console.log('save', this.work)
      this.$electron.ipcRenderer.send('add-todo', this.work)
      this.dialog = false
    },
  },
}
</script>