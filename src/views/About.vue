<template>
  <div class="about">
<b-container class="bv-example-row">
  <b-row align-v="center" align-h="center">
    <b-col md="4">
      <!-- <img :src="particularshow.image.medium" class="img-responsive" id="image" /> -->
      <p v-if="particularshow.image!= null">
              <img v-bind:src="particularshow.image.medium" id="image" />
            </p>
            <p v-else>
              <img src="../assets/images.jpg" />
            </p>
      </b-col>
    <b-col md="8">
      <div class="data">
         <h1>{{particularshow.name}}</h1>
        <h4> Language<b-icon icon="play"></b-icon> : {{particularshow.language}}</h4>
        <h4> RunTime<b-icon icon="clock"></b-icon>  : {{particularshow.runtime}}</h4>
        <h4> Status<b-icon icon="calendar-check"></b-icon>   : {{particularshow.status}}</h4>
        <h4> Rating<b-icon icon="star"></b-icon>   : {{particularshow.rating.average}}</h4>
        <h4> Genres : </h4>
        <h4>{{particularshow.genres.join(",")}}</h4>
         <h4>Summary : </h4>
        <h5><span v-html="particularshow.summary" /></h5>
        <b-button size="sm" class="my-2 my-sm-0" type="submit" @click.prevent="episodes()" variant="primary">
          <b-icon icon="easel"></b-icon>Show Episodes
          </b-button>
        <b-button size="sm" class="my-2 my-sm-0" type="submit" @click.prevent="backToHome()" variant="secondary" >
          <b-icon icon="arrow-left-circle"></b-icon>Back
          </b-button>
        <b-button size="sm" class="my-2 my-sm-0" type="submit" @click.prevent="showCast()" variant="primary">
          <b-icon icon="person"></b-icon>Show Cast
          </b-button>
    </div></b-col>
  </b-row>
</b-container>

    <div>
      <br />
        <b-container class="bv-example-row" v-if="conditionalRender == false">
          <b-row>

           <b-col md="4" lg="4" sm="6" v-for="(e,index) in episodesdetails" :key="index" id="grid"  >
               
                <h4>{{e.name}}</h4>
                <p v-if="e.image != null">
               <img :src="e.image.medium" class="episodeimage"/>
                </p>
           </b-col>
          </b-row>
        </b-container>  
        <b-container class="bv-example-row" v-else>
           <b-row>
           <b-col md="4" lg="4" sm="6" v-for="(c,index) in cast" :key="index" id="grid"  >
                <h4>{{c.person.name}}</h4>
                <p v-if="c.person.image != null">
               <img :src="c.person.image.medium" class="episodeimage"/>
                </p>
          </b-col>
           </b-row>
        </b-container>
    </div>
    
  </div>
</template>
<script>

import {viewShowDetails,viewEpisodeDetails,castDetails} from '../show.service.js'
export default {
  name:"About",
  data(){
    return{
      particularshow:[],
      showId:this.$route.params.id,
      episodesdetails:[],
      cast:[],
      conditionalRender:false
    }
  },
  
  created(){
    
    console.log("showID",this.showId)
    
     viewShowDetails(this.showId).then(res=>{
       console.log("getting results from axios",res.data)
       this.particularshow=res.data;
     }).catch(()=>{
       this.$router.push({ name: "PageNotFound" });
     });
     if(this.particularshow.length>0){
       console.log("present")
     }
     else{
       console.log("not present")
     }
     
  },
  methods:{
    backToHome(){
      
      this.$router.push("/");
   
     },
     episodes(){
       this.conditionalRender=false;
       viewEpisodeDetails(this.showId).then(res =>(this.episodesdetails=res.data));
       console.log(this.episodesdetails)
     },
     showCast(){
       this.conditionalRender=true;
       castDetails(this.showId).then(res=>{
         this.cast=res.data;
         console.log(this.cast)
       })
     }
  }
 
}
</script>
<style scoped>
.data{
  /* width: 800px; */
  /* border-radius: 10px; */
  /* border:1px solid #f1f1f1; */
  /* margin:15px; */
  /* float: left; */
  /* height: 700px; */
  background-color:black;
  color:lightcyan;

}
#image{
  width: 100%;
  /* border:1px solid #f1f1f1; */
  margin-top: 50px;
  height: 100%;

}
#grid{
  width: 100%;
  border-radius: 10px;
  border:1px solid #f1f1f1;
  /* margin:45px; */
  /* float: left; */
  height: 100%;
  /* padding: 20px; */
  
 
  
}
.episodeimage{
  width:100%;
  height:350px;
}
.about{
  background-color: black;
}
h4{
  color: azure;
}
</style>