<template>
    <v-container class="overflow-hidden mx-auto " style="padding-right: 0px; padding-left: 0px;">
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
        <span class=" font-weight-bold headline font  color-c ma-3 my-4">Reserva en cualquier lugarr</span>
          <v-divider class="grey mx-2"></v-divider>
            <v-row  justify="center" class="ma-1">
            <v-col  v-for="(company, index) in companies" :key="index"> 
                <v-hover >
                  <v-card :elevation=12  style="border-radius: 10px;" class="link light-green ma-2 lighten-5"  max-width="300"   v-if="company.fields.length == 0" disabled>
                    <v-img :src="company.image" style="border-radius: 10px 10px 35px 35px;" height="150px" gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,0.9)">
                    </v-img>
                      <v-row  no-gutters>
                        <v-col >
                        <div>
                          <span class="title ma-2 font ">{{ company.name }}</span><br>
                          <span class="body-2 ma-2 font "> {{ company.address}}</span>
                        </div> 
                            <span class="body-2 ma-2">
                              <v-icon color="black" size="15" class="body-2 font">mdi-map-marker</v-icon>{{ company.town.name}}, {{ company.town.department.name }}
                            </span>
                            </v-col>
                    <v-card-actions class="">
                      <div class="reserve link" v-if="company.fields.length == 0" outlined >
                        <v-row  class="ma-1">
                          <div >
                            <v-chip outlined rounded  class=" ma-1" disabled color="red" >Sin canchas <v-icon right >mdi-emoticon-sad-outline</v-icon></v-chip>                          
                          </div>
                        </v-row>
                      </div>
                    </v-card-actions>
                    </v-row>
                  </v-card>
                  <v-card :elevation=12  style="border-radius: 10px;" class="link light-green lighten-5" max-width="350" v-else v-bind:to=" '/do_reserve/'+company.id+ '/reserve'">
                    <v-img :src="company.image" style="border-radius: 10px 10px 35px 35px;" height="175px" gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,0.9)">
                    </v-img>
                      <v-row  no-gutters >
                        <v-col>
                        <div>
                          <span class="title ma-2 font ">{{ company.name }}</span><br>
                          <span class="body-2 ma-2 font "> {{ company.address}}</span>    
                        </div> 
                            <span class="body-2 ma-2 font">
                              <v-icon color="black" size="15" class="body-2 font link">mdi-map-marker</v-icon>{{ company.town.name}}, {{ company.town.department.name }}
                            </span>
                          </v-col>
                    <v-card-actions>
                        <v-row  class="ma-1">
                          <div class="my-1">
                            <span class="subtitle-1 font-weight-bold color-c font-color font link">Canchas {{ company.fields.length }}</span><br>
                          <span text small v-bind:to=" '/do_reserve/'+company.id+ '/reserve' " class="link font-weight-bold font-color font my-1 ma-9" ><v-icon  size=25>mdi-stadium</v-icon>
                            </span>
                          </div>
                            <!--<span text small v-bind:to=" '/do_reserve/'+company.id+ '/reserve' " class="link font-weight-bold font-color font my-1" >Ver canchas<v-icon  size=20>mdi-stadium</v-icon>
                            </span>-->
                        </v-row>

                    </v-card-actions>
                    </v-row>
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
        
  data ()  {
    return {
        reservations: [ ] ,
        companies: [ ],
        users: [ ],
        show: false,
        months: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sep', 'Oct', 'Nov', 'Dic'],
        dayss: ['Dom', 'Lun', 'Ma', 'Mie', 'Jue', 'Vie', 'Sab',],
        activeBtn: 1,
        showNav: true,
        user_name: '',
        name: '',
        last_name: '',
        drawer: false, 
        items : [
                {title: 'Inicio', icon: 'mdi-home', to  : '/home'},
                {title: 'Mis Reservaciones', icon: 'mdi-calendar', to: '/account'},
            ],
          }
    },
     components: {
    BottomNavigation
  },
   computed: {
    isLoggedIn (){
      return this.$store.getters.isLoggedIn
    }
   }, 
    methods: {
      getCompanies() {
      const path = URL+'api/field-company/'
      //const path = 'https://api-backend-canchas.herokuapp.com/api/field-company/'
      axios.get(path).then((response)=> {
        this.companies = response.data
        //console.log(response.data);
      })
      },
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
      this.getCompanies(),
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
  
  .v-card {
    transition: opacity .4s ease-in-out;
  }
  .v-card:not(.on-hover) {
    opacity: 0.9;
  }
  .show-btns {
    color: rgba(255, 255, 255, 1) !important;
  }
  .round{
    border-radius: 10px;
   }
   .reserve {
     width: 100%;
     border-radius: 5px;
     margin-top: -0.5em; 
   }
   .calendar {
     position: absolute;
     margin-top: 10.1em;
   }
   .span {
     position: relative;
     margin-top: -0.5em;
   }
   .description {
     position: absolute;
     margin-top: -0.8em;
     margin-left: 1em;
   }
   .back {
     position: absolute;
     margin-top: -1.5em;
     margin-left: -0.2em;
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
     padding-bottom: 50px;
     padding-right: 0px;
     margin-top: -15px;
     padding-left: 0px;
   }
   .container {
    max-width: 100%;
    max-height: 100%;
  }
  .back-ground {
    background-color: #011427;
  }
  .font-color {
    color: #011427 !important;
  }
  .color-c {
    color: #011427 !important;
  }
</style>