<template>
  <div>
    <template>

      <div>
        <v-dialog transition="dialog-top-transition" max-width="600">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="primary" v-bind="attrs" v-on="on">Adicionar</v-btn>
          </template>
          <template v-slot:default="dialog">
            <v-card>
              <v-toolbar color="primary" dark>Adicionar novo jogo</v-toolbar>
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
                <v-btn text @click="dialog.value = false">Close</v-btn>
                <v-btn color="primary" @click="submitData">Salvar</v-btn>
              </v-card-actions>

            </v-card>
          </template>
        </v-dialog>
      </div>


      <v-simple-table>
        <template v-slot:default>
          <thead>
            <tr>
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
            </tr>
          </thead>
          <tbody>
            <tr v-for="game in games" :key="game.name">
              <td>{{ game.name }}</td>
              <td>{{ game.description }}</td>
              <td>
                <v-rating background-color="grey lighten-2" size="10" color="warning" empty-icon="mdi-star-outline"
                  full-icon="mdi-star" hover length="5" :value="game.rating"></v-rating>
              </td>
              <td>{{ game.amount }}</td>
            </tr>
          </tbody>
        </template>
      </v-simple-table>
    </template>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        games: [],
        newGame: {
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
            vm.getData();
          })
          .catch(function (error) {
            console.log(error)
          })
      }
    },
    mounted() {
      this.getData();
    }
  }

</script>
