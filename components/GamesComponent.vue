<template>
  <div>
    <div>
      <v-btn color="primary" dark @click.stop="dialog = true">
        Adicionar
      </v-btn>

      <v-dialog v-model="dialog" transition="dialog-bottom-transition" max-width="600">
        <v-card>
          <v-toolbar color="primary" dark class="text-h5">
            Adicionar novo jogo
          </v-toolbar>
          <v-card-text>
            <v-form>
              <v-text-field v-model="newGame.name" label="Nome">
              </v-text-field>

              <v-text-field v-model="newGame.description" label="Descrição">
              </v-text-field>

              <v-row>
                <v-col cols="12" sm="6">
                  <p>Avaliação</p>
                </v-col>
                <v-col cols="12" sm="6">
                  <v-rating background-color="grey lighten-2" color="warning" empty-icon="mdi-star-outline"
                    full-icon="mdi-star" hover length="5" v-model="newGame.rating" value="4.5"></v-rating>
                </v-col>
              </v-row>

              <v-text-field type="number" step="0.1" v-model="newGame.amount" label="Preço">
              </v-text-field>
            </v-form>
          </v-card-text>
          <v-card-actions class="justify-end">
            <v-btn text @click="dialog = false">Close</v-btn>
            <v-btn color="primary" @click="editing ? updateGame(newGame.id) : submitData()">Salvar</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>

    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">
              #
            </th>
            <th class="text-left">
              Nome
            </th>
            <th class="text-left">
              Descrição
            </th>
            <th class="text-left">
              Avaliação
            </th>
            <th class="text-left">
              Preço
            </th>
            <th class="text-left">
              Opções
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="game in games" :key="game.id">
            <td>{{ game.id }}</td>
            <td>{{ game.name }}</td>
            <td>{{ game.description }}</td>
            <td>
              <v-rating background-color="grey lighten-2" size="10" color="warning" empty-icon="mdi-star-outline"
                full-icon="mdi-star" hover length="5" :value="game.rating"></v-rating>
            </td>
            <td>{{ game.amount }}</td>
            <td>
              <v-btn icon v-bind="attrs" v-on="on" @click="editGame(game.id)">
                <v-icon>
                  mdi-pencil
                </v-icon>
              </v-btn>
              <v-btn icon color="error" @click="removeGame(game.id)">
                <v-icon>
                  mdi-close-circle
                </v-icon>
              </v-btn>
            </td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        dialog: false,
        games: [],
        newGame: {
          name: '',
          description: '',
          rating: '',
          amount: '',
        },
        editedGame: {
          name: '',
          description: '',
          rating: '',
          amount: '',
        }
      }
    },
    methods: {
      getData() {
        let vm = this
        this.$axios
          .$get('/api/games')
          .then(function (response) {
            // handle success
            vm.games = response.data;
          })
          .catch(function (error) {
            // handle error
            console.log(error);
          })

      },
      submitData() {
        let vm = this

        this.$axios
          .$post('/api/games', this.newGame)
          .then(function () {
            vm.newGame = {}
            vm.dialog.value = false

          })
          .catch(function (error) {
            console.log(error)
          })
        this.getData()
        this.dialog = false
      },
      editGame(id) {
        this.editing = true
        this.newGame = this.games.find(x => x.id === id);
        this.dialog = true





      },
      updateGame(id) {
        const config = {
          headers: {
            'Content-Type': 'application/json'
          }
        };

        this.$axios
          .$put('api/games/' + id, this.newGame, config)
          .then(response => {
            console.log(response)
          })
          .catch(error => {
            console.log(error);
          })
        this.getData()
        this.editing = false
        this.dialog = false

      },

      removeGame(id) {
        this.$axios
          .$delete('api/games/' + id)
          .then(response => {
            if (response.status == 204) {
              alert('Jogo excluido com sucesso!')
            }
          })
          .catch(error => {
            console.log(error);
          })
        this.getData()

      }
    },
    mounted() {
      this.getData();
    }
  }

</script>
