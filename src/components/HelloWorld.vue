<template>
  <div class="hello">
    <H3>Improved drawing demo</H3>
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
    <div id="controls">
      <div id="inputs">
        <span>Nr of squares:</span><input v-model="rectNum" type="number">
        <button @click="Generate()">Generate</button>
      </div>

      <button disabled style="user-select:none;cursor:unset" @click="Undo()">Undo</button>
      <button @click="Reset()">Reset</button>
      <!-- <button @click="Finis()">Finish</button> -->
    </div>
  </div>
</template>

<script>
export default {
  name: "PlayG",
  data() {
    return {
      rectangles: [],
      Selections: [],
      MyPath: "",
      chartHeight: 500,
      chartWidth: 500,

      rectNum: 15,
      rectWidth: 50,
      rectHeight: 50
    };
  },
  created() {
    this.Generate();
  },
  methods: {
    Generate() {
      var rectSize = this.chartWidth / this.rectNum; //100
      this.rectWidth = rectSize;
      this.rectHeight = rectSize;
      var row, col;
      var myX = 0,
        myY = 0;

      var data_x, data_y, data_CX, data_CY;

      for (row = 0; row < this.rectNum; row++) {
        data_x = myX + rectSize * row;
        data_CX = data_x + rectSize / 2;

        for (col = 0; col < this.rectNum; col++) {
          data_y = myY + rectSize * col;
          data_CY = data_y + rectSize / 2;
          // debugger;
          this.rectangles.push({
            x: data_x,
            y: data_y,
            CX: data_CX,
            CY: data_CY,
            Slcted: false,
            Disabled: false, //row != 0,
            idCol: row + 1,
            idRow: col + 1
          });
        }
      }
    },
    SelRect(rct, index) {
      //  debugger
      //  exit if disabled
      if (rct.Disabled) {
        return false;
      }
      //update disabled
      // if (rct.idCol > 1) {
      this.rectangles.forEach(elm => {
        debugger;
        elm.Disabled = elm.idCol < rct.idCol + 1;
      });
      // }
      //update slected
      rct.Slcted = !rct.Slcted;
      //  add or remove selections
      if (rct.Slcted) {
        this.Selections.push({ id: index, lCX: rct.CX, lCY: rct.CY });
      } else {
        var rmove = this.Selections.findIndex(x => x.id == index);
        this.Selections.splice(rmove, 1);
      }
      // create path
      var newMyPath = "";
      var vueObj = this;
      vueObj.Selections.forEach((points, indx) => {
        if (indx == 0) {
          newMyPath = "M" + points.lCX + " " + points.lCY;
        } else {
          newMyPath += "L" + points.lCX + " " + points.lCY;
        }
      });
      this.MyPath = newMyPath;
    },
    Undo() {},
    Reset() {
      var vueObj = this;
      vueObj.Selections = [];
      vueObj.rectangles.forEach(elm => {
        elm.Slcted = false;
        elm.Disabled = false;
      });
      vueObj.MyPath = "";
    },
    Finis() {
      var vueObj = this;
      vueObj.rectangles.forEach(elm => {
        elm.Disabled = true;
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.myRect {
  fill: #dedeff;
  stroke-width: 3;
  /* stroke: rgb(0, 0, 0); */
  stroke: #8a8383;
}
.selected {
  fill: red !important;
}
#app {
  width: 800px;
  margin: 5vh auto;
}
svg {
  background-color: lightblue;
}
path {
  fill: none;
  stroke: black;
  stroke-width: 10;
}
.noSelect {
  fill: grey;
  user-select: none;
}
#inputs {
  margin: 10px;
}
#inputs button {
  background-color: aqua;
  height: 50px;
  border-radius: 5px;
  padding: 10px;
}
#inputs input {
  width: 50px;
}
button {
  cursor: pointer;
}
</style>

