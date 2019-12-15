<template>
      <v-container class="overflow-hidden mx-auto " style="padding-left: 3px; padding-right: 3px;">
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
        <v-container class="bottom "  style="border-radius: 25px 25px 0px 0px;">
            <v-row  justify="space-around"  class="ma-1">
              <v-col v-for="(company, i) in companies" :key="i" >
                  <v-hover >
                    <v-card  v-bind:to=" 'companies/'+company.id+'/info' " class="link " max-width="320" :elevation=12 style="border-radius: 10px;">
                        <v-img  :src="company.image" height="10em" class="text-right pa-2" gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,0.9)">
                            <v-card-title class="title white--text">
                              <v-row class="fill-height flex-column" justify="space-between">
                                <div class="description">
                                  <span class="mt-4  caption  text-left font">{{ company.name }},</span>
                                  <span class="mt-4  caption  text-left font"> {{ company.address }}</span>
                                  <v-divider inset vertical class="mx-1"></v-divider>
                                  <span>
                                    <v-icon size=18 color="amber accent-4">mdi-star</v-icon> 
                                    <v-icon size=18 color="amber accent-4">mdi-star</v-icon>
                                    <v-icon size=18 color="amber accent-4">mdi-star</v-icon>
                                    <v-icon size=18 color="amber accent-4">mdi-star</v-icon>
                                    <v-icon size=18 color="amber accent-4">mdi-star</v-icon>
                                  </span>
                              </div>
                            </v-row>
                          </v-card-title>
                        </v-img>
                        <v-card-actions>
                          <div class="">
                            <v-row>
                              <div>
                                <span class="ma-5 font-weight-bold font font-color"  >Email: {{ company.email }}</span><br>
                                  <span class="ma-5 caption font-weight-bold font font-color">Tel. {{ company.phone }}</span>
                              </div>
                            </v-row>
                          </div>
                          <v-spacer></v-spacer>
                          <v-row>
                          <v-btn  outlined v-bind:to=" 'companies/'+company.id+'/info' " class="link font-color"  style="bottom: 0.5em; position: absolute; right: 0.5em;" icon><v-icon>mdi-information-variant</v-icon></v-btn>
                          </v-row>                    
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

  data: () => ({
    companies: [ ],
    months: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sep', 'Oct', 'Nov', 'Dic'],
    dayss: ['Dom', 'Lun', 'Ma', 'Mie', 'Jue', 'Vie', 'Sab',],
    user_name: '',
      slide: 'first',
      name: '',
      rating: 4.3,
      last_name: '',
      drawer: false, 
       items : [
                {title: 'Inicio', icon: 'mdi-home', to  : '/home'},
                {title: 'Mis Reservaciones', icon: 'mdi-calendar', to: '/account'},
            ],
  }),

  mounted () {
    //const path = 'https://api-backend-canchas.herokuapp.com/api/companies/'
    const path = URL+'api/companies/'
    axios.get(path).then((response)=> {
    this.companies = response.data
    //console.log(this.companies);
    })   
  },
  computed: {
    isLoggedIn (){
      return this.$store.getters.isLoggedIn
    }
   }, 
  methods: {
       getUser(){
        axios.get(URL+'api/user-reservation-today/').then((response)=>{
          this.users = response.data
          let find_user = this.users.find (v => v.id == this.$store.state.user)
          this.user_name = find_user.username
          this.name = find_user.first_name
          this.last_name = find_user.last_name
          //console.log(this.user_today_reservation);
          console.log(this.user_name);
          //console.log(this.name);
          //console.log(this.user_reservation_today);
        })
      }
  },
  created(){
      this.getUser()
    },
}
</script>

<style scoped>
   @import url('https://fonts.googleapis.com/css?family=Ubuntu&display=swap');

   .description {
     position: absolute;
     margin-top: 5.5em;
     margin-left: 0.2em;
   }
   .bottom {
     padding-bottom: 70px;
     margin-top: -20px;
     padding-right: 3px;
     padding-left: 3px;
   }
   .font {
     font-family: 'Ubuntu', sans-serif;
   }
   .link {
     text-decoration: none !important;
   }
   .back-ground {
    background-color: #011427;
  }
  .font-color {
    color: #011427 !important;
  }
</style>
