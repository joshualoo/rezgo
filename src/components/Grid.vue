<template>
    <section class="section grid-section">
        <div class="tile available-tile">
            <h2 class="available has-text-info" ><i class="fas fa-info-circle"></i> There are currently {{ num > 0 ? num : 0 }} tours available </h2>
        </div>

        <div class="columns is-multiline">
            <div class="column item-column is-4-desktop single_card is-6-tablet" v-for="item in items" :key="item.item" data-aos="fade-up">

              <span class="is-hidden">
                <!-- calculations between today's date and last updated -->
                {{ new Date(item.updated*1000) }}
                {{ today = Math.round((new Date()).getTime() / 1000) }}
                {{ difference = today - item.updated }}
              </span>
              <!-- add new label if its less than three weeks old -->
              <span v-if="newLabelTime > difference" class="new is-size-7">new</span>

              <span v-if="item.media.image != null"> 
              <carousel :nav="false" :items=1>
                <li class="images" v-for="image in item.media.image" :key="image.path" >     
                    <img :src="(`${image.path}`)" class="carousel-image">
                </li>
              </carousel>
              </span>
              <span v-else>
                <carousel :nav="false" :items=1>
                <img class="carousel-image" src="https://via.placeholder.com/1280x720">
                <img class="carousel-image" src="https://picsum.photos/1280/720?grayscale">
                <img class="carousel-image" src="https://picsum.photos/1280/720?blur">
                <img class="carousel-image" src="https://picsum.photos/1280/720">
              </carousel>
              </span>

              <div class="column">
                <h2 class="name is-size-4">{{item.item}}</h2>
                <p class="where is-size-6">{{item.city}}, {{item.state}}, {{item.country.toUpperCase()}}</p>

                  <div class="tile tags">
                    <span class="is-hidden">
                      {{ tags = item.tags.replace(/,,/g, ",").split(',').slice(0, 3)}}
                    </span>
                    <li v-for="tag in tags" :key="tag.name" class="activity-name is-size-7">
                      {{tag}}
                    </li>
                  </div>
               </div>

              <div class="column details">
                <p class="duration">
                  <i class="fas fa-clock"></i> 
                  Duration: 
                    <span v-if="item.duration.length >= 0">
                    {{item.duration}}
                    </span>
                    <span v-else>
                      TBA
                    </span>
                </p>
                <p class="dates">
                  <i class="fas fa-calendar"></i>
                  Dates: 
                  <span v-if="item.start_date">
                    from 
                    {{new Date(item.start_date*1000).toLocaleDateString('en-US',{  year: 'numeric', month: 'short', day: 'numeric' })}} 
                    -
                    {{new Date(item.end_date*1000).toLocaleDateString('en-US',{  year: 'numeric', month: 'short', day: 'numeric' })}}
                  </span>
                  <span v-else>
                    TBA
                  </span>
                </p>
              </div>  

              <div class="column">
                <p class="rating">
                  <span v-if="typeof item.rating !== 'undefined'"> 
                  <!-- star -->
                  <svg viewBox="0 0 1000 1000" role="presentation" aria-hidden="true" focusable="false" style="height: 1em; width: 1em; fill: #f76c00;"><path d="M972 380c9 28 2 50-20 67L725 619l87 280c11 39-18 75-54 75-12 0-23-4-33-12L499 790 273 962a58 58 0 0 1-78-12 50 50 0 0 1-8-51l86-278L46 447c-21-17-28-39-19-67 8-24 29-40 52-40h280l87-279c7-23 28-39 52-39 25 0 47 17 54 41l87 277h280c24 0 45 16 53 40z"></path></svg> 
                    {{item.rating}} ({{item.rating_count}})
                  </span>
                  <span v-else> 
                    <!-- star -->
                    <svg viewBox="0 0 1000 1000" role="presentation" aria-hidden="true" focusable="false" style="height: 1em; width: 1em; fill: #f76c00;"><path d="M972 380c9 28 2 50-20 67L725 619l87 280c11 39-18 75-54 75-12 0-23-4-33-12L499 790 273 962a58 58 0 0 1-78-12 50 50 0 0 1-8-51l86-278L46 447c-21-17-28-39-19-67 8-24 29-40 52-40h280l87-279c7-23 28-39 52-39 25 0 47 17 54 41l87 277h280c24 0 45 16 53 40z"></path></svg> 
                    No ratings yet
                  </span>
                </p>
              </div>

              <div class="column overview">
                <p class="overview__copy overview" v-bind:class="[isClicked ? 'clicked' : '']"><span v-html="item.details.overview.trim()"></span></p>  
                <p class="overview__copy itinerary" v-bind:class="[isClicked ? 'clicked' : '']"><span v-html="item.details.itinerary"></span></p>  

                <a class="more-info" v-on:click="isHidden = !isHidden; toggleClass()">
                  <i class="fas fa-align-left"></i>
                  <span v-if="isHidden"> more</span> <span v-else> less</span> info
                </a>
              </div>
            </div>    
        </div>
    </section>    
</template>

<script>

import axios from 'axios'
import AOS from 'aos'
import 'aos/dist/aos.css';
import carousel from 'vue-owl-carousel'

AOS.init({
  once:true,
  duration:250,
  easing: 'linear',
  
});

export default {
    components: { carousel },
    name:'Grid',
    data () {
        return {
            num:null,
            items: null,
            isHidden:true,
            isClicked:false,
            newLabelTime:1814400
        }
    },
    methods:{
      toggleClass: function(){
        this.isClicked = !this.isClicked;
      }
    },
    mounted(){
      // const API_URL = 'http://parsernew.rzg.ca/test/awesome.json';
      const API_URL = '//joshualoo.ca/test/awesome.json';
        axios
            .get(API_URL)
            .then(response => (
                //console.log(response.data.item),
                this.num = response.data.total,
                this.items = response.data.item),
            )
            .catch(function(error){
                   console.log(error);
            })
    }
}
</script>

<style scoped>
/* OVERALL COMPONENT STYLES */
section.section{
  padding-bottom:5rem;
}
.columns{
  justify-content: start;
  align-items:start;
}
.column.item-column{
  padding:0 0 1rem 0!important;
  width:calc(33.333% - 2rem);
  margin:0.75rem;
}
.column.details{
  margin:1rem 0 0;  
  line-height:1.8;
}
.column.overview{
  transition:height 0.3s ease;
}
@media (max-width:1200px){
  .column.item-column{
    width:calc(50% - 1.5rem);
  }
}
@media (max-width:768px){
  .column.item-column{
    width:calc(100% - 1.5rem);
    margin-bottom:3rem;
  }
}
.svg-inline--fa{
  margin-right:0.15rem;
}
/* END - OVERALL COMPONENT STYLES */

/* AVIALBALE TOUR STYLES */
.available-tile{
  margin:3rem 0;
  width:fit-content;
  border:1px solid #167df0;
  border-radius:16px;
  padding:10px 20px;
}
h1.available{
  font-size:1.1rem;
}
/* END - AVIALBALE TOUR STYLES */

/* SINGLE CARD STYLES */
.single_card{
  background:white;
  position:relative;
  margin-bottom:2rem;
  border-radius:20px;
  /* padding:1rem 1rem 5rem 1rem; */
  padding-bottom:5rem;
  transition:0.2s ease all;
}
.single_card:hover{
  transform: translate3d(0px, -5px, 0px);
  box-shadow: rgba(0, 0, 0, 0.08) 0px 3px 10px 0px;
}
.carousel-image{
  border-radius:20px;
}
h2.name{
    font-size:1.33rem;
    font-weight:600;
}
p.details{
  font-size:0.9rem;
  line-height:1.8;
}
a.more-info{
  font-size:0.9rem;
  color:#f76c00;
  transition:0.4s ease all;
}
a.more-info:hover{
  opacity:0.85;
}
p.overview__copy{
  font-size:0.8rem;
  line-height:1.6;
  max-height: 0;
  transition: all 0.4s ease-out;
  overflow: hidden;
}
.overview__copy.itinerary{
}
.overview__copy.overview:before{
  content:'Overview';
  font-weight:500;
  font-size:0.9rem;
}
.overview__copy.itinerary:before{
  content:'What you\'ll be doing';
  font-weight:500;
  font-size:0.9rem;
}
.overview__copy.clicked{
  padding-bottom:0.75rem;
  max-height: 500px;
  transition: max-height 0.4s ease-in;
}
span.new{
  background:#f76c00;
  position:absolute;
  display:block;
  top:0.75rem;
  left:0.75rem;
  color:#ffffff;
  z-index:99;
  padding:2px 7px;
  border-radius:20px;
}

/* column group placements */
.tile.tags{
  flex-wrap:wrap;
}
.activity-name{
  color:#f76c00;
  border:1px solid #f76c00;
  border-radius:14px;
  width:fit-content;
  padding:3px 10px;
  margin:0.75rem .5rem 0 0;
}
/* end - column group placements */

/* END - SINGLE CARD STYLES */
</style>