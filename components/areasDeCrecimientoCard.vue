<template>
  <div id="layout">
    <!--Tarjeta de presentacion de areas de crecimiento -->
    <br />
    <div

      id="card_cuadro_de_adelanto"
      :elevation="5"
      color="#000233"
      max-width="100%"
      v-for="area_de_crecimiento in areas_de_crecimientos"
      :key="area_de_crecimiento._id"
    >
      <div id="imagen_estado" v-if="area_de_crecimiento.estado == true" style="background:#42f435" >
        <v-img id="logo" :src="area_de_crecimiento.imagen"></v-img>
      </div>
      <div id="imagen_estado" v-if="area_de_crecimiento.estado == false" style="background: #f44235">
        <v-img id="logo" :src="area_de_crecimiento.imagen"></v-img>
      </div>
      
      <div id="derecha">
        <p id="titulo">{{area_de_crecimiento.nombre}}</p>
        <p id="sub_titulo">{{area_de_crecimiento.estado}}</p>
      </div>
      <br />
      <!------------------------------------------------------------------------------------------------------------ -->
      <v-card-actions>
        <v-spacer></v-spacer>

        <v-btn icon @click="show = !show">
          <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
        </v-btn>
      </v-card-actions>

      <v-expand-transition>
        <div v-show="show">
          <v-divider></v-divider>
          <div
            id="card_requisito"
            :elevation="5"
            color="#000233"
            max-width="100%"
            v-for="requisito in area_de_crecimiento.requisitos"
            :key="requisito._id">
              <div id="color_estado" v-if="requisito.estado == 'No realizado'" style="background: #f44235"></div>
              <div id="color_estado" v-if="requisito.estado == 'Realizado'" style="background: #42f435"></div>
              <div id="color_estado" v-if="requisito.estado == 'Pendiente'" style="background: #f49f35"></div>
                <p id="titulo2">{{requisito.estado}}</p>
                <p id="sub_titulo">{{requisito.descripcion}}</p>
                <hr>
                <!--BOTONES INTEGRANTE-->
                <v-btn v-if="requisito.estado == 'No realizado' && rol == 'Integrante'" id="boton1" text v-on:click="peticion_integrante(requisito._id, area_de_crecimiento.tipo)">Pedir validación</v-btn>
                <v-btn v-else-if="requisito.estado == 'Pendiente' && rol == 'Integrante'" id="boton1" text v-on:click="peticion_integrante(requisito._id, area_de_crecimiento.tipo)">Cancelar petición</v-btn>
                <!--BOTONES JEFE-->
                <!--BOTONE 1-->
                <v-btn v-else-if="requisito.estado == 'No realizado' && rol != 'Integrante'" id="boton1" text v-on:click="realizado_a_no_realizado(requisito._id, area_de_crecimiento.tipo)">Aceptar</v-btn>
                <v-btn v-else-if="requisito.estado == 'Realizado' && rol != 'Integrante'" id="boton1" text v-on:click="realizado_a_no_realizado(requisito._id, area_de_crecimiento.tipo)">Cancelar</v-btn>
                <v-btn v-else-if="requisito.estado == 'Pendiente' && rol != 'Integrante'" id="boton1" text v-on:click="rechazar_peticion(requisito._id, area_de_crecimiento.tipo)">Rechazar petición</v-btn>
                <!--BOTONE 2-->
                <v-btn v-if="requisito.estado == 'Pendiente' && rol != 'Integrante'" id="boton2" text v-on:click="aceptar_peticion(requisito._id, area_de_crecimiento.tipo)">Aceptar petición</v-btn>
              </div>
              <br>
              <br>
          </div>
      </v-expand-transition>
      <!------------------------------------------------------------------------------------------------------------ -->
    </div>
    <br/>
  </div>
</template>
<script>
export default {
  name: 'areasDeCrecimientoCard',
  props: {
    area_de_crecimiento: Object,
    requisito: Object
  },
  data: () => ({
    id_integrante: '',
    unidad: '',
    tipo: '',
    show: false,
    rol: '',
    dialog: false,
    background: '',
    areas_de_crecimientos: []
  }),
  methods: {
    //PETICIONES:
    //JEFES
    //DE NARANJA A VERDE(BUENO)
    //.get('cuadro/aceptar-peticion/' + this.id_integrante + '/' + tipo + '/' + id)
    //DE NARANJA A ROJO(BUENO)
    //.get('cuadro/rechazar-peticion/' + this.id_integrante + '/' + tipo + '/' + id)
    //DE ROJO A VERDE Y VICEVERSA(BUENO)
    //.get('cuadro/validar-cancelar/' + this.id_integrante + '/' + tipo + '/' + id)

    //INTEGRANTE
    //DE ROJO A NARANJA Y DE NARANJA A ROJO(BUENO)
    //.get('cuadro/integrante-peticion-cancelacion/' + this.id_integrante + '/' + tipo + '/' + id)

    //INTEGRANTE: CAMBIA EL ESTADO DE NO REALIZADO A PENDIENTE Y DE PENDIENTE A NO REALIZADO
    peticion_integrante(id, tipo) {
      this.axios
        .get(
          'cuadro/integrante-peticion-cancelacion/' +
            this.id_integrante +
            '/' +
            tipo +
            '/' +
            id
        )
        .then(response => {
          this.areas_de_crecimientos = response.data
        })
        .catch(e => {
          alert('ERROR: ' + e)
        })
    },
    //JEFE: CAMBIA EL ESTADO DE NO REALIZADO A REALIZADO Y DE REALIZADO A NO REALIZADO(ROJO A VERDE Y VERDE A ROJO)
    realizado_a_no_realizado(id, tipo) {
      this.axios
        .get(
          'cuadro/validar-cancelar/' +
            this.id_integrante +
            '/' +
            tipo +
            '/' +
            id
        )
        .then(response => {
          this.areas_de_crecimientos = response.data
        })
        .catch(e => {
          alert('ERROR: ' + e)
        })
    },
    //JEFE: CAMBIA EL ESTADO DE PENDIENTE A REALIZADO(DE NARANJA A VERDE)
    aceptar_peticion(id, tipo) {
      this.axios
        .get(
          'cuadro/aceptar-peticion/' +
            this.id_integrante +
            '/' +
            tipo +
            '/' +
            id
        )
        .then(response => {
          this.areas_de_crecimientos = response.data
        })
        .catch(e => {
          alert('ERROR: ' + e)
        })
      this.dialog = false
    },
    //JEFE: CAMBIA EL ESTADO DE PENDIENTE A NO REALIZADO(DE NARANJA A ROJO)
    rechazar_peticion(id, tipo) {
      this.axios
        .get(
          'cuadro/rechazar-peticion/' +
            this.id_integrante +
            '/' +
            tipo +
            '/' +
            id
        )
        .then(response => {
          this.areas_de_crecimientos = response.data
        })
        .catch(e => {
          alert('ERROR: ' + e)
        })
      this.dialog = false
    }
  },
  //SE LLENA EL PLAN DE ADELANTOS DEL INTEGRANTE
  mounted() {
    //OBTENEMOS EL ID DEL INTEGRANTE
    if (localStorage._id_integrante) {
      this.id_integrante = localStorage._id_integrante
    } else {
      alert('unidad no existe')
    }
    //OBTENEMOS EL ROL SEA DE JEFE O DE INTEGRANTE PARA SABER COMO REALIZAR LA PAGE
    if (localStorage.rol) {
      this.rol = localStorage.rol
    } else {
      alert('rol no existe')
    }
    //OBTENEOS LA UNIDAD DEL INTEGRANTE
    if (localStorage.unidad_integrante) {
      this.unidad = localStorage.unidad_integrante
    } else {
      alert('unidad no existe')
    }
    //CLASIFICACION DEL TIPO DE CUADRO DE ADELANTO PARA LAS AREAS DE CRECIMIENTO
    if (this.unidad == 'Familia') {
      this.tipo = 'Dentellada'
    } else if (this.unidad == 'Manada') {
      this.tipo = 'Preza de caza'
    } else if (this.unidad == 'Tropa') {
      this.tipo = 'Aventura'
    } else if (this.unidad == 'Sociedad') {
      this.tipo = 'Desafios de acción'
    } else {
      this.tipo = 'Rutas para rovers'
    }
    //lLENAMOS LA INFORMACION DE LAS AREAS DE CRECIMIENTO DEL PLAN DE ADELANTO
    this.axios
      .get('cuadro/lista/' + this.tipo + '/' + this.id_integrante)
      .then(response => {
        this.areas_de_crecimientos = response.data
      })
      .catch(e => {
        alert('ERROR: ' + e)
      })
  }
}
</script>
<style scoped>
#layout {
  font-family: 'Muli';
  width: 100%;
  height: auto;
  margin: auto;
}
#card_cuadro_de_adelanto {
  margin-bottom:10px;
  background: #222b5c;
  max-width: 100%;
  width: 100%;
  max-height: 70px;
  display: inline-block;
}
#imagen_estado {
  width: 70px;
  height: 70px;
  background: #f44235;
  float: left;
  position: relative;
}
#logo {
  max-width: 100%;
  display: block;
  margin: auto;
  width: 90%;
  height: auto;
}
#titulo {
  margin: auto;
  color: #ffe255;
  text-align: left;
  margin-top: 5%;
  font-size: 20px;
  font-weight: bold;
}
#sub_titulo {
  color: #bababa;
  font-size: 15px;
  font-weight: bold;
  text-align: left;
  margin-left: 12px;
  margin-bottom: 8px;
}
#derecha {
  padding-left: 10px;
  margin-left: 10px;
  position: relative;
  float: left;
  width: 45%;
  height: auto;
}
/*ESTILO REQUISITOS */
#card_requisito {
  margin-top: 2.5%;
  background: #222b5c;
  max-width: 100%;
  width: 100%;
  height: auto;
  display: inline-block;
}

#color_estado {
  width: 100%;
  height: 40px;
  background: #f44235;
}
#titulo2 {
  margin: auto;
  color: #ffe255;
  font-size: 20px;
  margin-top: 10px;
  margin-left: 15px;
  font-weight: bold;
}
#sub_titulo2 {
  color: #bababa;
  font-size: 15px;
  font-weight: bold;
  text-align: justify;
  margin-left: 15px;
  margin-top: 7px;
  width: 90%;
}
#boton1 {
  background: #ffe255;
  margin: auto;
  max-width: 90%;
  width: 130px;
  float: left;
  position: relative;
  font-size: 10px;
  height: 40px;
  font-weight: bold;
  color: #121d3c;
  margin: 1%;

}
#boton2 {
  background: #ffe255;
  margin: auto;
  max-width: 90%;
  width: 130px;
  font-size: 10px;
  height: 40px;
  margin: 1%;
  position: relative;
  font-weight: bold;
  color: #121d3c;
}
#mostrado{
  margin-top: 30%;
}

</style>