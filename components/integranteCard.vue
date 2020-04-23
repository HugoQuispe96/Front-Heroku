<template>
  <div id="layout">
    <!--Tarjeta de presentacion de Integrante -->
    <v-card
      id="card_integrante"
      :elevation="10"
      v-for="integrante in integrantes"
      :key="integrante.id"
    >
      <v-list-item three-line>
        <v-list-item-content>
          <v-list-item-title id="cards_subtitle">{{integrante.unidad}}</v-list-item-title>
          <v-list-item-title id="cards_title">{{integrante.nombre+" "+integrante.apellidos}}</v-list-item-title>
          <v-list-item-subtitle id="cards_subtitle">{{integrante.id}}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-card-actions>
        <!--BOTON VISUALIZAR -->
        <v-row>
          <v-dialog v-model="dialog" fullscreen hide-overlay transition="dialog-bottom-transition">
            <template v-slot:activator="{ on }">
              <v-btn
                id="botones_cortos_GSK31"
                v-on="on"
                v-on:click="visualizar(integrante.id)"
              >VISUALIZAR</v-btn>
            </template>
            <v-card color="#121D3C">
              <v-toolbar color="#222b5c">
                <v-btn icon dark @click="cambiaso()">
                  <v-icon>mdi-close</v-icon>
                </v-btn>
                <v-toolbar-title id="cards_title">INFORMACIÓN DEL INTEGRANTE</v-toolbar-title>
                <v-spacer></v-spacer>
              </v-toolbar>
              <v-list three-line subheader color="#121D3C">
                <br/>
                <v-subheader id="cards_title">INFORMACIÓN PERSONAL</v-subheader>
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Número de identificación:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.id"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Nombres:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.nombre"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>

                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Apellidos:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.apellidos"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Fecha de nacimiento:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.fechaNacimiento"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Celular:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.celular"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Telefono:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.telefono"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Correo:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.correo"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Direción:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.direccion"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Barrio:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.barrio"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Inscrito:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.inscrito"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                <v-subheader id="cards_title">INFORMACIÓN EXTRA</v-subheader>

                    <div v-if="verDato">
                      <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Nombre de la madre:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['nombreMadre']"
                            background-color="#eee"
                            outlined
                            disabled
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>

                      <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Celular del madre:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['celularMadre']"
                            background-color="#eee"
                            outlined
                            disabled
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>

                      <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Nombre del padre:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['nombrePadre']"
                            background-color="#eee"
                            outlined
                            disabled
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>
            
                  <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Celular del padre:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['celularPadre']"
                            background-color="#eee"
                            outlined
                            disabled
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>
                                    <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Nombre de emergencia:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['nombreEmergencia']"
                            background-color="#eee"
                            outlined
                            disabled
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>
                                    <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Celular de emergencia:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['celularEmergencia']"
                            background-color="#eee"
                            outlined
                            disabled
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>
                    </div>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </v-card>
          </v-dialog>
        </v-row>
        <!--BOTON ACTUALIZAR -->
        <v-row>
          <v-dialog v-model="dialog2" fullscreen hide-overlay transition="dialog-bottom-transition">
            <template v-slot:activator="{ on }">
              <v-btn
                id="botones_cortos_GSK31"
                v-on="on"
                v-on:click="visualizar(integrante.id)"
              >ACTUALIZAR</v-btn>
            </template>
            <v-card color="#121D3C">
              <v-toolbar color="#222b5c">
                <v-btn icon dark @click="cambiaso()">
                  <v-icon>mdi-close</v-icon>
                </v-btn>
              <v-toolbar-title id="cards_title">INFORMACIÓN DEL INTEGRANTE</v-toolbar-title>
                <v-spacer></v-spacer>
              </v-toolbar>
              <v-list three-line subheader color="#000233">
                <v-subheader id="cards_title">INFORMACIÓN PERSONAL</v-subheader>
                <v-list-item>
                  <v-list-item-content>
                   <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Número de identificación:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.id"
                          background-color="#eee"
                          outlined
                          disabled
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Nombres:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.nombre"
                          background-color="#eee"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>

                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Apellidos:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.apellidos"
                          background-color="#eee"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Fecha de nacimiento:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.fechaNacimiento"
                          background-color="#eee"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Celular:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.celular"
                          background-color="#eee"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Telefono:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.telefono"
                          background-color="#eee"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Correo:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.correo"
                          background-color="#eee"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Direción:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.direccion"
                          background-color="#eee"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Barrio:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.barrio"
                          background-color="#eee"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                    <v-list-item-title>
                      <v-col class="camposGSK31">
                        <p id="indices">Inscrito:</p>
                        <v-text-field
                          color="#FFE255"
                          v-model="individuo.inscrito"
                          background-color="#eee"
                          outlined
                        ></v-text-field>
                      </v-col>
                    </v-list-item-title>
                <v-subheader id="cards_title">INFORMACIÓN EXTRA</v-subheader>

                    <div v-if="verDato">
                      <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Nombre de la madre:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['nombreMadre']"
                            background-color="#eee"
                            outlined
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>

                      <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Celular del madre:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['celularMadre']"
                            background-color="#eee"
                            outlined
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>

                      <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Nombre del padre:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['nombrePadre']"
                            background-color="#eee"
                            outlined
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>
            
                  <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Celular del padre:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['celularPadre']"
                            background-color="#eee"
                            outlined
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>
                                    <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Nombre de emergencia:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['nombreEmergencia']"
                            background-color="#eee"
                            outlined
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>
                                    <v-list-item-title>
                        <v-col class="camposGSK31">
                          <p id="indices">Celular de emergencia:</p>
                          <v-text-field
                            color="#FFE255"
                            v-model="individuo['adicional']['celularEmergencia']"
                            background-color="#eee"
                            outlined
                          ></v-text-field>
                        </v-col>
                      </v-list-item-title>
                    </div>
                      
                  </v-list-item-content>
                </v-list-item>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn id="botones_cortos_GSK31" text @click="cambiaso()">Cancelar</v-btn>
                  <v-btn id="botones_cortos_GSK31" text v-on:click="actualizar()">Confirmar</v-btn>
                </v-card-actions>
              </v-list>
            </v-card>
          </v-dialog>
        </v-row>
        <!--BOTON ELIMINAR -->
        <v-row>
          <v-btn id="botones_cortos_GSK31" @click.stop="dialog3 = true">Desactivar</v-btn>
          <v-dialog v-model="dialog3" max-width="290">
            <v-card color="#222b5c">
              <v-card-title id="cards_title">¿Seguro que desea desactivar el integrante?</v-card-title>

              <v-card-text id="cards_subtitle">Si desactivas a este no podras visualizar o actualizar su información</v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>

                <v-btn color="yellow" text @click="dialog3 = false">Cancelar</v-btn>
                <v-btn color="yellow" text v-on:click="eliminar(integrante._id)">Aceptar</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios, axios)
axios.defaults.baseURL = 'https://backgsk31.herokuapp.com/'
export default {
  name: 'integranteCard',
  props: {
    integrante: Object
  },

  data: () => ({
    verDato: false,
    integrantes: [],
    individuo: [],
    dialog: false,
    dialog2: false,
    dialog3: false,
    date: new Date().toISOString().substr(0, 10),
    menu: false,
    modal: false,
    menu2: false,
    id_local: '',
    unidad_jefe: ''
  }),
  methods: {
    visualizar(id_integrante) {
      this.individuo = []
      this.axios.get('scout/listar/' + id_integrante).then(respuesta => {
        this.individuo = respuesta.data
        this.verDato = true
      }).catch(e => {
            alert(e)
          })
    },

    // En este metodo solo se cambia el valor de estas variables, funciona para corregir algunos bugs
    cambiaso() {
      this.verDato = false
      this.dialog = false
      this.dialog2 = false
      this.dialog3 = false
    },

    actualizar() {
      this.axios
        .put(
          `scout/actualizar/${this.individuo._id}/${this.individuo.adicional.progreso_plan}`,
          this.individuo
        )
        .then(response => {
          location.reload()
        }).catch(e => {
             alert("Error al actualizar, compruebe la información e intente mas tade.")
          })
    },
    eliminar(id_integrante) {
      this.visualizar(id_integrante)
      this.axios.put('scout/eliminar/' + id_integrante).then(respuesta => {
        this.dialog3 = false
        // vm.$forceUpdate()
        location.reload()
      }).catch(e => {
            alert("Error al desactivar el integrante, intente mas tade.")
          })
    }
  },
  mounted() {
    if (localStorage.unidad_jefe) {
      this.unidad = localStorage.unidad_jefe
    } else {
      alert('unidad no existe')
    }
    this.axios.get('scout/integrantes/' + this.unidad).then(response => {
      this.integrantes = response.data
    }).catch(e => {
            alert(e)
          })
  },
  created() {}
}
</script>
<style scoped>
#layout {
  font-family: 'Muli';
  width: 100%;
  height: auto;
  margin: auto;
}
 #card_integrante {
  background: #222b5c;
  max-width: 100%;
  max-height: auto;
  margin: 5%;
}
#cards_title {
  font-size: 20px;
  font-weight: bold;
  color: #ffe255;
}
#cards_subtitle {
  color: #bababa;
  font-size: 14px;
  font-weight: bold;

  margin-bottom: -8px;
  text-align: justify;
}
.camposGSK31 {
  margin: auto;
  width: 100%;
  height: 100%;
}
#indices {
  color: #bababa;
  font-size: 15px;
  font-weight: bold;
}
#botones_cortos_GSK31 {
  background: #ffe255;
  margin: auto;
  max-width: 90%;
  width: 80px;
  display: block;
  font-size: 10px;
  height: 30px;
  font-weight: bold;
  color: #121d3c;
}
</style>