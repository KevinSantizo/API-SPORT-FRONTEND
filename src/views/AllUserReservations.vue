<template>
   <v-container  class="overflow-hidden mx-auto">
   <v-app-bar  class="white" text height=85 app>
              <v-app-bar-nav-icon color="black" x-large @click="drawer = !drawer"></v-app-bar-nav-icon>
                <v-row justify="center">
                  <div class="ma-7">
                        <span class="font subtitle-1" >Reservaciones: {{ this.quant }}</span><br>
                  </div>
                  <v-divider inset vertical class="transparent mx-4"></v-divider>
                    <v-icon color="black" size="35" class="my-2 ">mdi-soccer</v-icon>
                </v-row>
        </v-app-bar>
    <v-navigation-drawer dense dark app v-model="drawer"  temporary class="grey lighten-2" style="border-radius: 0px 35px 35px 0px;">
      <template v-slot:prepend>
          <v-list-item> 
              <v-avatar class="profile my-5" tile style="border-radius: 10px;" size="75"> 
                    <img src="https://static.platzi.com/static/website/v2/images/avatar_default.afdd5b436fc2.png" alt="">
                </v-avatar> 
                <v-divider inset vertical class="mx-2 transparent"> 
                </v-divider> 
                <v-list-item-content>
                    <v-list-item-title  class="font-weight-medium font title black--text">{{name}}</v-list-item-title>
                    <span class="font black--text" >{{last_name}}</span>
                </v-list-item-content>
                <v-btn icon @click="drawer = !drawer">
                <v-icon color="black" size=40>mdi-chevron-left</v-icon>
                </v-btn>
          </v-list-item>
        </template>
           <v-divider class="grey darken-1 "></v-divider>
          <v-list shaped>
          <v-list-item-group  v-model="items" class="link" color="">
              <v-list-item  class="link"   v-for="item in items" :key="item.title" router :to="item.to" min-width="2" >
                  <v-list-item-icon class="link">
                      <v-icon medium class="link black--text" size=25>{{ item.icon }}</v-icon>
                  </v-list-item-icon>
                    <v-list-item-content class="link">
                      <v-list-item-title class="font-weight-medium subtitle-1 link font black--text">{{ item.title }}</v-list-item-title>
                  </v-list-item-content>
              </v-list-item>
              <v-divider class="grey darken-1 "></v-divider>
               <v-btn  v-if="isLoggedIn" v-bind:to="{ name: 'logout' }"  small class="ma-2 link" tile text color="white">
                 <v-icon left color="black">mdi-power</v-icon> <span class="font black--text">Cerrar Sesión</span>
               </v-btn>
              </v-list-item-group>
          </v-list>
      </v-navigation-drawer>
        <BottomNavigation/>
        <v-container class="bottom " style="border-radius: 25px 25px 0px 0px;" >
          <div>
            <span class="headline font color-c font-weight-medium">{{ this.name }} {{ this.last_name }}</span><br>
            <span class="font subtitle-1 font-weight-bold primary--text">Estas son todas tus reservaciones.</span><br>
          </div>
          <v-divider></v-divider>
          <v-row justify="center">
             <v-hover v-for="(reservation, i) in this.user_reservations.reservations" :key="i">
               <v-card class="mx-auto ma-1" outlined>
                    <v-list-item three-line>
                    <v-list-item-content>
                        <div class="headline font-weight-bold mb-4">{{reservation.field_reserve.company.name}}</div>
                        <v-list-item-title class="title mb-1">Cancha {{reservation.field_reserve.name}}</v-list-item-title>
                        <v-list-item-subtitle>
                            <span class="body-2 font-weight-bold font  black--text">Fecha: {{ reservation.schedule_date }}</span><br>
                            <span class="body-2 font-weight-bold font  black--text">Hora: {{ reservation.schedule.start_time}} </span><br>
                            <span v-if="reservation.field_reserve.type == 1" class="body-2 font-weight-bold font  black--text">Fútbol 5</span>
                            <span v-else-if="reservation.field_reserve.type == 2" class="body-2 font-weight-bold font  black--text">Fútbol 7</span>
                            <span v-else class="body-2 font-weight-bold font  black--text">Fútbol 11</span><br>
                            <span class="body-2 font-weight-bold font  black--text">Hora: {{ reservation.schedule.start_time}} </span><br>
                        </v-list-item-subtitle>
                    </v-list-item-content>
                    <v-hover>
                        <v-card :elevation=12  width=125 heigth=75 style="border-radius: 10px;">
                            <v-img   :src="'https://api-backend-canchas.herokuapp.com'+reservation.field_reserve.company.image"   alt="Image" width=125  height=75>
                            </v-img>
                        </v-card>
                    </v-hover>
                    </v-list-item>
                </v-card>
          </v-hover>
          </v-row>
        </v-container>
   </v-container>
</template>

<script>
import axios from 'axios'
import BottomNavigation from '@/components/BottomNavigation'

let URL = 'https://api-backend-canchas.herokuapp.com/'
export default {
  components: {
    BottomNavigation
  },
  data: () => ({
    reservations:[ ],
    user_reservations: [ ],
    months: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sep', 'Oct', 'Nov', 'Dic'],
    dayss: ['Dom', 'Lun', 'Ma', 'Mie', 'Jue', 'Vie', 'Sab',],
    user_name: '',
    name: '',
    last_name: '',
    phone: '',
    email: '',
    quant: '',
    rating: 4.3,
    hidden: false,
    quantity: '',
    card: [
      {src: 'https://img.freepik.com/foto-gratis/representacion-3d-balon-futbol-linea-campo-futbol_41667-272.jpg?size=626&ext=jpg'}
    ],
     drawer: false, 
        items : [
          {title: 'Inicio', icon: 'mdi-home', to  : '/home'},
          {title: 'Mis Reservaciones', icon: 'mdi-calendar', to: '/account'},
      ],
    }),
    computed: {
     isLoggedIn (){
       return this.$store.getters.isLoggedIn
   }, 
},

mounted(){
      let path = URL+'api/user-reservations/'
      const requestOne = axios.get(path)
      requestOne.then((response)=>{
      this.reservations = response.data
      //console.log(this.reservations)
      let find_user_reservations = this.reservations.find(v => v.id == this.$store.state.user)
      this.user_reservations = find_user_reservations
      this.user_name = find_user_reservations.username
      this.name = find_user_reservations.first_name
      this.last_name = find_user_reservations.last_name
      //console.log(find_user_reservations);
      this.quant = find_user_reservations.quantity
      //console.log('Cantidad' + ' ' + this.quant);
      this.phone = find_user_reservations.phone
      this.email = find_user_reservations.email
    })
},

}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Ubuntu&display=swap');
  .font {
     font-family: 'Ubuntu', sans-serif;
   }
   .back-ground {
    background-color: #011427 !important;
  }
  .color-c {
    color: #011427 !important;
  }
  .bottom {
     padding-bottom: 70px;

   }
   .link {
     text-decoration: none !important;
   }
</style>
