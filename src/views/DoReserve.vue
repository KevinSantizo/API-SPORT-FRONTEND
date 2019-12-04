<template>
  <v-container class="overflow-hidden mx-auto" style="padding-left: 0px; padding-right: 0px;">
     <v-app-bar  class="white" text height=85 app>
              <v-app-bar-nav-icon color="black" x-large @click="drawer = !drawer"></v-app-bar-nav-icon>
                <v-row justify="center">
                  <div class="ma-7">
                    <v-icon color="black" size="25">mdi-calendar</v-icon><v-divider inset vertical class="mx-1"></v-divider><span class="font-weight-bold caption font" > {{ this.dayss[new Date().getDay() ]}}, {{  this.months[new Date().getMonth()] }} - {{ new Date().getDate()}} | {{ new Date().getFullYear() }}</span>
                  </div>
                  <v-divider inset vertical class="transparent mx-4"></v-divider>
                    <v-icon color="black" size="35" class="my-2 ">mdi-soccer</v-icon>
                </v-row>
        </v-app-bar>
     <v-navigation-drawer dense dark app v-model="drawer"  temporary class="grey lighten-2" style="border-radius: 0px 35px 35px 0px;">
      <template v-slot:prepend>
          <v-list-item> 
              <v-avatar class="profile my-5"  size="75"> 
                    <img src="https://static.platzi.com/static/website/v2/images/avatar_default.afdd5b436fc2.png" alt="">
                </v-avatar> 
                <v-divider inset vertical class="mx-2 transparent"> 
                </v-divider> 
                <v-list-item-content>
                    <v-list-item-title  class="font-weight-medium title font black--text">{{ name }}</v-list-item-title>
                    <span class="font black--text">{{last_name}}</span>
                </v-list-item-content>
                <v-btn icon @click="drawer = !drawer">
                <v-icon class="color-c" size=40>mdi-chevron-left</v-icon>
                </v-btn>           
          </v-list-item>
        </template>
           <v-divider class="grey darken-1 "></v-divider>
          <v-list shaped>
          <v-list-item-group  v-model="items" class="link">
              <v-list-item  class="link black--text" v-for="item in items" :key="item.title" router :to="item.to" min-width="2" >
                  <v-list-item-icon>
                      <v-icon medium class="link black--text" size=25>{{ item.icon }}</v-icon>
                  </v-list-item-icon>
                    <v-list-item-content>
                      <v-list-item-title class="font-weight-medium subtitle-1 link font black--text">{{ item.title }}</v-list-item-title>
                  </v-list-item-content>
              </v-list-item>
              <v-divider class="grey darken-1 "></v-divider>
               <v-btn  v-if="isLoggedIn" v-bind:to="{ name: 'logout' }"  small class="ma-2 link indigo--text" tile text>
                 <v-icon color="black" left>mdi-power</v-icon> <span class="font black--text">Cerrar Sesión</span>
               </v-btn>
              </v-list-item-group> 
          </v-list>
      </v-navigation-drawer>
    <BottomNavigation/> 
    <v-container class="bottom">
      <v-row  class="mb-6"  no-gutters>
      <v-col  sm="5" md="6">
        <v-card class="transparent"  outlined tile>
          <span class="title font-weight-bold ma-2 font indigo--text">{{ company.name }}</span><v-icon size=40 style="position: absolute;" color="black">mdi-stadium</v-icon><br>
          <span class="body-2 font-weight-bold ma-2 font "> {{ company.address}}</span><br>
           <span class="body-2 ma-2 font font-weight-bold">
            <v-icon color="black" size="15" class="body-2 font link">mdi-map-marker</v-icon>{{ company.town.name}}, {{ company.town.department.name }}
          </span>
        </v-card>
      </v-col>
      <v-col sm="5" offset-sm="2" md="6" offset-md="0">
        <v-card  class=" transparent " outlined>
          <v-row justify="center"> 
          <v-hover>
            <v-card :elevation=12  width=125 heigth=75 style="border-radius: 10px;">
                <v-img  :src="company.image"  alt="Image" width=125  height=75>
                </v-img>
        </v-card>
             <!-- <v-icon :elevation=10 size=85 color="black">mdi-stadium</v-icon>-->
          </v-hover>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
 
      <v-divider></v-divider>
      <span class=" font-weight-bold headline font indigo--text ma-3">Elije una de las canchas</span>
      
      <v-row dense class="ma-2">
        <v-col v-for="(field, i) in company.fields" :key="i" cols="12">
          <v-hover>
          <v-card class="card-color link my-1" :elevation=12  dark v-bind:to=" '/field/' +field.id+'/reservar'" style="border-radius: 10px 10px 10px 10px;">
            <div class="d-flex flex-no-wrap justify-space-between">
              <div>
                <v-card-title ><span class="headline font">Cancha {{ field.name }}</span> </v-card-title>
                <v-card-subtitle>
                  <span v-if="field.type == 1" class="body-1 font-weight-bold font">Fútbol 5</span>
                  <span v-else-if="field.type == 2" class="body-1 font-weight-bold font">Fútbol 7</span>
                  <span v-else class="body-1 font-weight-bold font">Fútbol 11</span>          
                </v-card-subtitle>
              </div>
                <v-img  class="shrink ma-2" contain src="https://img.freepik.com/foto-gratis/representacion-3d-balon-futbol-linea-campo-futbol_41667-276.jpg?size=626&ext=jpg" style="border-radius:  10px 10px 10px 10px; flex-basis: 125px;" gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,0.5)">
                <v-row class="fill-height" align="center" justify="center">
                <div class="headline"><span class="font">Q{{ field.price }}.00</span><br>
                </div>
              </v-row>
                </v-img>
            </div>
              <v-divider dark></v-divider>
    <v-card-actions class="">
      <v-chip style="bottom: 1em;" small class="light-green accent-4 font-weight-bold font" dark>Destacada</v-chip>      
      <v-spacer></v-spacer>
      <v-rating style="bottom: 1em;"
        readonly
        v-model="rating"
        background-color="white"
        color="yellow accent-4"
        dense
        half-increments
        size="18"
      ></v-rating>
    </v-card-actions>
          </v-card>
          </v-hover>
        </v-col>
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
  data(){  
    return {
      companyId: this.$route.params.companyId,
      company: [],
      field: [],
       user_reservation_today: [ ],
      user_today_reservation:[ ],
      user_name: '',
      slide: 'first',
      name: '',
      rating: 4.3,
      last_name: '',
      drawer: false, 
      itemss: [
        {
          color: '#1F7087',
          src: 'https://cdn.vuetifyjs.com/images/cards/foster.jpg',
          title: 'Supermodel',
          artist: 'Foster the People',
        },
        {
          color: '#952175',
          src: 'https://cdn.vuetifyjs.com/images/cards/halcyon.png',
          title: 'Halcyon Days',
          artist: 'Ellie Goulding',
        },
      ],
      items : [
              {title: 'Inicio', icon: 'mdi-home', to  : '/home'},
              {title: 'Mis Reservaciones', icon: 'mdi-calendar', to: '/account'},
          ],
      months: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sep', 'Oct', 'Nov', 'Dic'],
      dayss: ['Dom', 'Lun', 'Ma', 'Mie', 'Jue', 'Vie', 'Sab',],
      images: [
        { src: "https://bogota.gov.co/sites/default/files/styles/despliegue_1366x768_px/public/field/image/Nueva%20cancha%20sint%C3%A9tica%20en%20el%20Parque%20Las%20Cruces%20beneficiar%C3%A1%20comunidad%20de%20tres%20localidades%20P.jpg"},
        { src: "https://lh3.googleusercontent.com/6ygjCkkb-sYeWWJLh964wzsu-rnQcpquw2I9ebvQ4xzKCxoFnE0tWpuPXN7yV85rCdgEWqJq=w1080-h608-p-no-v0"},
        { src: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR1vYclJ6emIr9MCaXt8cH754_vIRt-ouh_I6IuIj58t_SPrjxd&s"},
        { src: "https://www.parqueygrama.com/wp-content/uploads/2017/03/grama-sintetica-para-canchas-de-futbol-2.png"},
        { src: "https://www.pqs.pe/sites/default/files/styles/852x479/public/archivos/2015/actualidad/01/sabugattas/pastosintetico-lacanchita-futbol7-3.jpg?itok=awaMBCao"},
        { src: "http://www.tucaqueta.com/wp-content/uploads/2017/01/Cancha.jpg"},
        { src: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTQEgGMX5i7iqw9ALETWvwt1shQtrmQp7LBnCqqY3DNLgKAMV7-sA&s"},
        { src: "https://files.alerta.rcnradio.com/alerta_tolima_prod/public/styles/article_desktop/public/migration/canchas14deoctubre.png?itok=thGx-QGr"},
        { src: "https://www.eluniversal.com.co/sites/default/files/201706/cancha_2.jpg"},
        { src: "http://www.eje21.com.co/site/wp-content/uploads/2016/04/Cancha-sintetica-de-la-terminal-de-manizales.jpg"},
        { src: "https://files.rcnradio.com/public/styles/img_galeria/public/2019-02/whatsapp_image_2019-02-11_at_4.22.50_pm_1_0.jpeg?itok=pjSrd8tA"}
      ],
      }
  },
  computed: {
    isLoggedIn (){
      return this.$store.getters.isLoggedIn
    }
   }, 
    methods: {
       getCompany() {
        const path = `https://api-backend-canchas.herokuapp.com/api/field-company/${this.companyId}/`
        //const path = `https://api-backend-canchas.herokuapp.com/api/field-company/${this.companyId}/`
          axios.get(path).then((response)=> {
          this.company = response.data;
          //console.log('Company ' + this.company);
          return axios.get(URL+'api/thefield/')
          //return axios.get('https://api-backend-canchas.herokuapp.com/api/field-schedule/')
      }).then((response)=>{
        this.field = response.data
        console.log(this.field);
      })
      },
      getUser(){
        axios.get(URL+'api/user-reservation-today/').then((response)=>{
          this.user_reservation_today = response.data
          let find_user = this.user_reservation_today.find (v => v.id == this.$store.state.user)
          this.user_today_reservation = find_user
          this.user_name = find_user.username
          this.name = find_user.first_name
          this.last_name = find_user.last_name
          //console.log(this.user_today_reservation);
          //console.log(this.user_name);
          //console.log(this.name);
          //console.log(this.user_reservation_today);
        })
      }
    },
    created(){
      this.getCompany(),
      this.getUser()
    },
    /*created(){
      this.getDate()
    }*/
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Ubuntu&display=swap');
  .font {
     font-family: 'Ubuntu', sans-serif;
   }
   .link {
     text-decoration: none !important;
   }
   .heart {
     position: absolute;
     margin-left: 13.7em;
     margin-top:  0.2em;
   }
   .bottom {
    padding-bottom: 70px;
     margin-top: 0px;
     padding-right: 0px;
     padding-left: 0px;
     border-radius: 25px 25px 0px 0px;
   }
 .back-ground {
    background-color: #011427 !important;
  }
  .font-color {
    color: #011427 !important;
  }
  .card-color {
    background-color:  #154360;
  }
</style>