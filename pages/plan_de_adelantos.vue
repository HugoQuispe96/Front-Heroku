<template>
  <div id="layout">
    <v-img id="logo" src="https://i.ibb.co/h73PVvz/familia.png"></v-img>
<p id="titulo">{{nombre}}</p>
<p id="sub_titulo">{{id}}</p>
    <v-tabs v-model="tab" background-color="#222b5c" id="tabs" v-if="unidad='Familia'">
      <v-tab color="#ffe255" id="tabs" v-for="item in familia" :key="item">{{ item }}</v-tab>
    </v-tabs>
    <v-tabs v-else-if="unidad='Manada'" id="tabs" v-model="tab">
      <v-tab class="overline" v-for="item in manada" :key="item">{{ item }}</v-tab>
    </v-tabs>
    <v-tabs v-else-if="unidad='Tropa'" id="tabs" v-model="tab">
      <v-tab class="overline" v-for="item in tropa" :key="item">{{ item }}</v-tab>
    </v-tabs>
    <v-tabs v-else-if="unidad='Sociedad'" id="tabs" v-model="tab">
      <v-tab class="overline" v-for="item in sociedad" :key="item">{{ item }}</v-tab>
    </v-tabs>
    <v-tabs v-else-if="unidad='Clan'" id="tabs" v-model="tab">
      <v-tab class="overline" v-for="item in clan" :key="item">{{ item }}</v-tab>
    </v-tabs>
    <v-tabs-items id="tabs_items" v-model="tab">
      <v-tab-item>
        <areasDeCrecimientoCard />
      </v-tab-item>
      <v-tab-item>
        <especialidadesCard />
      </v-tab-item>
      <v-tab-item>
        <adelantosCard />
      </v-tab-item>
    </v-tabs-items>
  </div>
</template>
<script>
import areasDeCrecimientoCard from '../components/areasDeCrecimientoCard'
import especialidadesCard from '../components/especialidadesCard'
import adelantosCard from '../components/adelantosCard'
export default {
  components: {
    areasDeCrecimientoCard,
    especialidadesCard,
    adelantosCard
  },
  data() {
    return {
      id: '',
      nombre:'',
      individuo: [],
      tab: null,
      tab2: null,
      unidad: '',
      familia: ['Dentelladas', 'Especialidades', 'Adelantos'],
      manada: ['Prezas de caza', 'Especialidades', 'Adelantos'],
      tropa: ['Aventuras', 'Especialidades', 'Adelantos'],
      sociedad: ['Desafios de acción', 'Especialidades', 'Adelantos'],
      clan: ['Rutas para rovers', 'Proyectos', 'Adelantos'],
      especialidades: ['Basicas', 'Optativas']
    }
  },
  mounted() {
    //LIMIPIAMOS EL LOCAL STORAGE
    if (localStorage.unidad_integrante) {
      this.unidad = localStorage.unidad_integrante
    }
    if (localStorage.id_integrante) {
      this.id = localStorage.id_integrante
    }
    this.axios
      .get('scout/listar/' + localStorage.id_integrante)
      .then(respuesta => {
        this.id = respuesta.data.id
        this.nombre = respuesta.data.nombre
      })
      .catch(e => {
        alert(e)
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
#tabs {
  font-size: 10px;
  font-weight: bold;
  margin: auto;
  width: (100px/3px);
  color: #ffe255;
}
#tabs_items {
  background: #121d3c;
}
p {
  text-align: center;
}
#logo {
  max-width: 100%;
  display: block;
  margin: auto;
  width: 100px;
  height: auto;
  margin-top: 5%;
  margin-bottom: 20px;
}
#titulo {
  margin: auto;
  color: #ffe255;
  text-align: center;
  font-size: 20px;
  font-weight: bold;
}
#sub_titulo {
  color: #bababa;
  font-size: 15px;
  font-weight: bold;
  text-align: center;
  margin-left: 12px;
  margin-bottom: 8px;
}
</style>