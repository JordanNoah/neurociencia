<template>
  <v-app>
    <v-snackbar :color="snackbarColor" v-model="snackbar" :timeout="snackbarTime" top>{{textSnackbar}}</v-snackbar>
    <v-app-bar color="#76FF03" elevation="0" dense><v-row><v-col class="text-center font-weight-black">PSICONEUROLOGIA</v-col></v-row></v-app-bar>
    <v-container fill-height>
      <v-row>
        <v-col class="col4" @click.stop="partCuerpo">
          <v-card :disabled="partescuerpo">
            <v-card-title>
              Partes del cuerpo
            </v-card-title>
            <v-card-text>
              <v-text-field v-model="nombrepartC" outlined label="Nombre" dense></v-text-field>
              <v-file-input v-model="archpartC" label="Archivo" outlined dense></v-file-input>
            </v-card-text>
            <v-card-actions>
              <v-row>
                <v-col class="col12 d-flex justify-end">
                  <v-btn outlined class="mx-2" color="red">Ignorar</v-btn>
                  <v-btn outlined class="mx-2" color="green" @click.stop="prtCuerpo">Guardar</v-btn>
                </v-col>
              </v-row>
            </v-card-actions>
          </v-card>
        </v-col>
        <v-col class="col4" @click.stop="pntCuerp">
          <v-card :disabled="puntosCuerpo">
            <v-card-title>
              Puntos del cuerpo
            </v-card-title>
            <v-card-text>
              <v-text-field v-model="nombreptoCuerp" outlined label="Nombre" dense></v-text-field>
              <v-textarea v-model="descptoCuerp" outlined name="input-7-4" label="Descripción"></v-textarea>
              <v-select v-model="selectPartCuerpo" outlined dense label="Partes del Cuerpo" :items="itemsprtCuerpo" item-text="nombre" item-value="id" no-data-text="Llene partes del cuerpo"></v-select>
              <v-select v-model="selectEnferme" outlined dense label="Enfermedades" :items="itemsEnfermedades" item-text="nombre" item-value="id" no-data-text="Llene enfermedades"></v-select>
            </v-card-text>
            <v-card-actions>
              <v-row>
                <v-col class="col12 d-flex justify-end">
                  <v-btn outlined class="mx-2" color="red">Ignorar</v-btn>
                  <v-btn outlined class="mx-2" color="green" @click.stop="pntsCrp">Guardar</v-btn>
                </v-col>
              </v-row>
            </v-card-actions>
          </v-card>
        </v-col>
        <v-col class="col4" @click.stop="enferm">
          <v-card :disabled="enfermedades">
            <v-card-title>
              Enfermedades
            </v-card-title>
            <v-card-text>
              <v-text-field v-model="nombreEnfermedades" outlined label="Nombre" dense></v-text-field>
              <v-textarea v-model="descEnfermedades" outlined name="input-7-4" label="Descripción"></v-textarea>
            </v-card-text>
            <v-card-actions>
              <v-row>
                <v-col class="col12 d-flex justify-end">
                  <v-btn outlined class="mx-2" color="red">Ignorar</v-btn>
                  <v-btn outlined class="mx-2" color="green" @click.stop="enfermGruadar">Guardar</v-btn>
                </v-col>
              </v-row>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',

  components: {
    
  },

  data(){
    return{
      partescuerpo:false,
      puntosCuerpo:true,
      enfermedades:true,
      nombrepartC:'',
      archpartC:null,
      textSnackbar:'nada',
      snackbarTime:2000,
      snackbar:false,
      snackbarColor:'info',
      nombreptoCuerp:'',
      descptoCuerp:'',
      selectPartCuerpo:'',
      selectEnferme:'',
      nombreEnfermedades:'',
      descEnfermedades:'',
      itemsprtCuerpo:[],
      itemsEnfermedades:[]
    }  
  },
  methods:{
    partCuerpo(){
      this.partescuerpo=false
      this.puntosCuerpo=true
      this.enfermedades=true
    },
    pntCuerp(){
      this.partescuerpo=true
      this.puntosCuerpo=false
      this.enfermedades=true
      this.selectPrtsCuerpo()
      this.selectEnfermedades()
    },
    enferm(){
      this.partescuerpo=true
      this.puntosCuerpo=true
      this.enfermedades=false
    },
    selectPrtsCuerpo(){
      axios({
        url:"http://192.168.100.54:29/slctPrtCrp.php"
      }).then((res) => {
        if(res.data!="0"){
          this.itemsprtCuerpo = res.data
        }else{
          this.itemsprtCuerpo = []
        }
      })
    },
    selectEnfermedades(){
      axios({
        url:"http://192.168.100.54:29/slctEnfer.php"
      }).then((res) => {
        if(res.data!="0"){
          this.itemsEnfermedades = res.data
        }else{
          this.itemsEnfermedades = []
        }
      })
    },
    prtCuerpo(){
      const params = new URLSearchParams()
      params.append('nombrepartC',this.nombrepartC)
      params.append('archpartC',this.archpartC.name)
      axios({
        method:"post",
        url:"http://192.168.100.54:29/prtsCuerpo.php",
        data:params
      }).then((res) => {
        if(res.data=="1"){
          this.textSnackbar="Guardado"
          this.snackbar=true
          this.snackbarColor="success"
          this.nombrepartC = ''
          this.archpartC = null
        }
        if(res.data=="0"){
          this.textSnackbar="ERROR"
          this.snackbar=true
          this.snackbarColor="error"
        }
      }).catch((error) => {
        this.textSnackbar=error
        this.snackbar=true
        this.snackbarColor="error"
      })
    },
    pntsCrp(){
      const params = new URLSearchParams()
      params.append('nombreptoCuerp',this.nombreptoCuerp)
      params.append('descptoCuerp',this.descptoCuerp)
      params.append('selectPartCuerpo',this.selectPartCuerpo)
      params.append('selectEnferme',this.selectEnferme)
      axios({
        method:"post",
        url:"http://192.168.100.54:29/pntsCuerpo.php",
        data:params
      }).then((res) => {
        if(res.data=="1"){
          this.textSnackbar="Guardado"
          this.snackbar=true
          this.snackbarColor="success"
          this.nombreptoCuerp=""
          this.descptoCuerp=""
          this.selectPartCuerpo = ""
          this.selectEnferme = ""
        }
        if(res.data=="0"){
          this.textSnackbar="ERROR"
          this.snackbar=true
          this.snackbarColor="error"
        }
      }).catch((error) => {
        this.textSnackbar=error
        this.snackbar=true
        this.snackbarColor="error"
      })
    },
    enfermGruadar(){
      const params = new URLSearchParams()
      params.append('nombre',this.nombreEnfermedades)
      params.append('descripcion',this.descEnfermedades)
      axios({
        method:"post",
        url:"http://192.168.100.54:29/safeEnfermedades.php",
        data:params
      }).then((res) => {
        if(res.data=="1"){
          this.textSnackbar="Guardado"
          this.snackbar=true
          this.snackbarColor="success"
          this.nombreEnfermedades=""
          this.descEnfermedades=""
        }
        if(res.data=="0"){
          this.textSnackbar="ERROR"
          this.snackbar=true
          this.snackbarColor="error"
        }
      }).catch((error) => {
        this.textSnackbar=error
        this.snackbar=true
        this.snackbarColor="error"
      })
    }
  }
};
</script>
<style>
  html::-webkit-scrollbar{
      display: none;
    }
</style>