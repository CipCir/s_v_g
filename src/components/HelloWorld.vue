<template>
  <div class="hello">
    <svg :width="chartWidth" :height="chartHeight" id="SVGtag">
      <g v-for="(rct,index) in rectangles" :key="index">
        <circle :cx="rct.CX" :cy="rct.CY" r="5"/>
        <rect
          id="myRect"
          :x="rct.x" 
          :y="rct.y"
          :width="rectWidth" 
          :height="rectHeight"
          @click="SelRect(rct,index)" 
          class="myRect" 
          :class="[{'selected':rct.Slcted},{'noSelect':rct.Disabled}]"
        />
        <!-- <circle :cx="rct.x+rct.width/2" :cy="rct.y+rct.height/2" r="5"/> -->
      </g>
         
        <path :d="MyPath" id="graphLine" />
    </svg>
    <button @click="Undo()">Undo</button>
    <button @click="Reset()">Reset</button>
    <button @click="Finis()">Finish</button>
  </div>
</template>

<script>
export default {
  name: "PlayG",
  data() {
    return {      
      rectangles:[
        {x:10,y:20,CX:35,CY:45,Slcted:false,Disabled:false,col:1},
        {x:10,y:70,CX:35,CY:95,Slcted:false,Disabled:false,col:1},
        {x:10,y:120,CX:35,CY:145,Slcted:false,Disabled:false,col:1},
        {x:10,y:170,CX:35,CY:195,Slcted:false,Disabled:false,col:1},
        {x:10,y:220,CX:35,CY:245,Slcted:false,Disabled:false,col:1},

        {x:60,y:20,CX:85,CY:45,Slcted:false,Disabled:false,col:2},
        {x:60,y:70,CX:85,CY:95,Slcted:false,Disabled:false,col:2},
        {x:60,y:120,CX:85,CY:145,Slcted:false,Disabled:false,col:2},
        {x:60,y:170,CX:85,CY:195,Slcted:false,Disabled:false,col:2},
        {x:60,y:220,CX:85,CY:245,Slcted:false,Disabled:false,col:2},

        {x:110,y:20,CX:135,CY:45,Slcted:false,Disabled:false,col:3},
        {x:110,y:70,CX:135,CY:95,Slcted:false,Disabled:false,col:3},
        {x:110,y:120,CX:135,CY:145,Slcted:false,Disabled:false,col:3},
        {x:110,y:170,CX:135,CY:195,Slcted:false,Disabled:false,col:3},
        {x:110,y:220,CX:135,CY:245,Slcted:false,Disabled:false,col:3},

        {x:160,y:20,CX:185,CY:45,Slcted:false,Disabled:false,col:4},
        {x:160,y:70,CX:185,CY:95,Slcted:false,Disabled:false,col:4},
        {x:160,y:120,CX:185,CY:145,Slcted:false,Disabled:false,col:4},
        {x:160,y:170,CX:185,CY:195,Slcted:false,Disabled:false,col:4},
        {x:160,y:220,CX:185,CY:245,Slcted:false,Disabled:false,col:4},

        {x:210,y:20,CX:235,CY:45,Slcted:false,Disabled:false,col:5},
        {x:210,y:70,CX:235,CY:95,Slcted:false,Disabled:false,col:5},
        {x:210,y:120,CX:235,CY:145,Slcted:false,Disabled:false,col:5},
        {x:210,y:170,CX:235,CY:195,Slcted:false,Disabled:false,col:5},
        {x:210,y:220,CX:235,CY:245,Slcted:false,Disabled:false,col:5},
        ],
      Selections:[],  
      MyPath:"",
      chartHeight: 500,
      chartWidth: 500,

      
      rectWidth:50,
      rectHeight:50,
    };
  },
   methods: {
     SelRect(rct,index){      
      //  debugger 
      //  exit if disabled
       if(rct.Disabled){
         return false;
       }
      //update disabled
      if (rct.col>0){
        this.rectangles.forEach(elm =>{
          elm.Disabled=elm.col<rct.col
        })
      }
      //update slected
       rct.Slcted=!rct.Slcted
      //  add or remove selections
        if (rct.Slcted){
          this.Selections.push({id:index,lCX:rct.CX,lCY:rct.CY})
        }else{
          var rmove = this.Selections.findIndex(x=>x.id==index)
          this.Selections.splice(rmove,1)
        }
      // create path
        var newMyPath=""
        var vueObj=this
        vueObj.Selections.forEach((points,indx) =>{
          if(indx==0){
            newMyPath="M"+points.lCX+" "+points.lCY
          }else{
            newMyPath+="L"+points.lCX+" "+points.lCY
          }
        })
        this.MyPath=newMyPath        
     },
   Undo(){
     
     },
   Reset(){
     var vueObj=this
     vueObj.Selections=[]
     vueObj.rectangles.forEach(elm =>{
       elm.Slcted=false;
       elm.Disabled=false;
     })
     vueObj.MyPath=""
   },
   Finis(){
     var vueObj=this
     vueObj.rectangles.forEach(elm =>{       
       elm.Disabled=true;
     })
   }
   },
 
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.myRect {
  fill: rgb(0, 0, 255);
  stroke-width: 3;
  stroke: rgb(0, 0, 0);
}
.selected{
  fill: red
}
#app {
  width: 800px;
  margin: 5vh auto;
}
svg{
  background-color: lightblue
}
path {
  fill: none;
  stroke: black;
  stroke-width:10
}
.noSelect{
  fill:grey;
  user-select: none;
}
</style>

