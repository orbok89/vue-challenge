<template>
<div> 
  <div class="container" v-if="this.visible_form">
    <div>Nome:</div>
    <input type="text" id="Nome" v-model="nome"  > 
    <div>Cognome:</div>
    <input type="text" id="Cognome" v-model="cognome"   > 
    <div>Github Username:</div>
    <input type="text" id="Github_Username"  v-model="Github_Username" :placeholder= [this.campo_errore] > <br> 
    <input  class="margine" type="submit" value="Submit" @click='ritorno_avatar()'>
  </div>
  <div v-if="this.visibile" class="container">
    <div >Benvenuto {{nome}} {{cognome}}</div> 
    <div><img v-bind:src=immagine alt=""> </div>
    <input  class="margine" type="submit" value="Return" @click='reset()'>
  </div>
  <div class="container" v-if="this.visibile_err">
    <div  >Ci spiace {{nome}} {{cognome}}, ma non ti abbiamo trovato</div>
    <input  class="margine" type="submit" value="Return" @click='reset()'>
  </div>
</div>
</template>

<script  >

import axios from 'axios'

  export default{
    name:'App',
    data(){
      return{
        nome:'',
        cognome:'',
        Github_Username:'',
        visibile:false,
        immagine:'',
        campo_errore:'',
        visibile_err:false,
        visible_form:true
      }
    },
    methods:{
      ritorno_avatar(){
        if(this.Github_Username!=''){
        this.visible_form=false 
        axios.get('https://api.github.com/users/'+this.Github_Username).then(res=>{
        this.immagine=res.data.avatar_url
        
        this.visibile=true}).catch(error => {
        this.visibile_err=true
  })

        }
        else{
          this.campo_errore='Campo obbligatorio'
        }
      },
      reset(){
        this.nome='',
        this.cognome='',
        this.Github_Username='',
        this.visibile=false,
        this.immagine='',
        this.campo_errore='',
        this.visibile_err=false,
        this.visible_form=true
      }
    }

  }
</script>

<style>
.container{
  border: 1px solid black;
  width: 50%;
  margin: 30px auto;
  
  text-align: center;
  flex-direction:column;
  height: 300px;
}
.container1{
  border: 1px solid black;
  width: 50%;
  margin: 30px auto;
  text-align: center;
  flex-direction:column;
  height: 300px;
}
input:focus::placeholder {
  color: transparent;
}
img{
  width: 20%;
  text-align: center;
}
.margine{
  margin: 5px;
}
</style>
