<!-- 
<template>
    <div id="container" style="width: 1300px; height: 700px;top:50px;left:50px;">
        <b-button @click="addBlock('input')" variant="outline-primary" style="margin-top: 20px; right: 20px; position: absolute;">Input Block</b-button>
        <b-button @click="addBlock('processing')" variant="outline-primary" style="margin-top: 20px; right: 20px; top: 50px; position: absolute;">Processing Block</b-button>
        <b-button @click="addBlock('decision')" variant="outline-primary" style="margin-top: 20px; right: 20px; top: 100px; position: absolute;">Decision Block</b-button>
        <b-button @click="addBlock('math')" variant="outline-primary" style="margin-top: 20px; right: 20px; top: 150px; position: absolute;">Math Block</b-button>
        <b-button @click="addBlock('vizualization')" variant="outline-primary" style="margin-top: 20px; right: 20px; top: 200px; position: absolute;">Vizualization Block</b-button>
        <b-button @click="runFlow()" variant="outline-primary" style="margin-top: 20px; right: 20px; bottom: 20px; position: absolute;">Run Flow</b-button>
        <div v-for="(block, index) in blockArr"
        :id="block.id"
        :key="index"
        :ref="block.type"
        style="width: 150px;height: 100px;"
        :style="{
            backgroundColor: block.color,
            top: block.top,
            left: block.left
        }"
        class="elem"
      > {{ block.type }} Block <br> ID: {{block.id}}

      <div
        :id="block.id"
        :key="index"
        :ref="block.type"
        v-on:mousedown="startMoving(block.id, $event)"
        v-on:mouseup="stopMoving(block.id, block.type, $event)"
        style="width: 150px;height: 100px; position: absolute; top: 0px">

      </div>
      <div class="knob kright" v-on:mousedown="startArrow(block.id)" v-on:mouseup="stopArrow(block.id, block.type, $event)" ></div>
      <div class="knob kleft" v-on:mousedown="startArrow(block.id)" v-on:mouseup="stopArrow(block.id, block.type, $event)"></div>
      <div class="knob ktop" v-on:mousedown="startArrow(block.id)" v-on:mouseup="stopArrow(block.id, block.type, $event)"></div>
      <div class="knob kbottom" v-on:mousedown="startArrow(block.id)" v-on:mouseup="stopArrow(block.id, block.type, $event)"></div>
      </div>
    </div>
  </template>
  
  <script>
// import LeaderLine from 'leader-line';





  export default {
    data() {
        return {
            tempArrow: [],
            lines: [],
            activeElement: null,
            blockid: 1,
            blockArr: [],
            droppedBlock: [],
        }
    },
    mounted(){

    },
    methods: {
    runFlow(){
      console.log(this.lines)
      this.lines.forEach(function(leaderLine) {
      leaderLine.setOptions({
        dash: {
            animation: true,
            duration: 5000, 
        },
        color: "red"
      });
  });
    },  
    addBlock(type){
        // console.log(type)
        if(type === "input"){
            this.blockArr.push({
                id: this.blockid,
                type: "Input",
                top: "40px",
                left: "30px",
                color: "#ef476f",
                inputType:"text"
            })
            this.blockid += 1
        }
        if(type === "processing"){
            this.blockArr.push({
                id: this.blockid,
                type: "Processing",
                top: "40px",
                left: "30px",
                color: "#ffd166"
            })
            this.blockid += 1
        }
        if(type === "decision"){
            this.blockArr.push({
                id: this.blockid,
                type: "Decision",
                top: "40px",
                left: "30px",
                color: "#06d6a0"
            })
            this.blockid += 1
        }
        if(type === "math"){
            this.blockArr.push({
                id: this.blockid,
                type: "Math",
                top: "40px",
                left: "30px",
                color: "#118ab2"
            })
            this.blockid += 1
        }
        if(type === "vizualization"){
            this.blockArr.push({
                id: this.blockid,
                type: "Vizualization",
                top: "40px",
                left: "30px",
                color: "#2698bd"
            })
            this.blockid += 1
        }
        console.log(this.blockArr)
    },
      move(divid, xpos, ypos) {
        divid.style.left = xpos + "px";
        divid.style.top = ypos + "px";
      },
      startMoving(elem, evt) {
        evt = evt || window.event;
        const posX = evt.clientX;
        const posY = evt.clientY;
        let divTop = document.getElementById(elem).style.top;
        let divLeft = document.getElementById(elem).style.left;
        const eWi = parseInt(document.getElementById(elem).style.width);
        const eHe = parseInt(document.getElementById(elem).style.height);
        const cWi = parseInt(document.getElementById(elem).parentElement.style.width) - 200;
        const cHe = parseInt(document.getElementById(elem).parentElement.style.height);
        this.$el.parentElement.style.cursor = "move";
        divTop = divTop.replace("px", "");
        divLeft = divLeft.replace("px", "");
        const diffX = posX - divLeft;
        const diffY = posY - divTop;
        document.onmousemove = (evt) => {
          evt = evt || window.event;
          const posX = evt.clientX;
          const posY = evt.clientY;
          let aX = posX - diffX;
          let aY = posY - diffY;
          if (aX < 0) aX = 0;
          if (aY < 0) aY = 0;
          if (aX + eWi > cWi) aX = cWi - eWi;
          if (aY + eHe > cHe) aY = cHe - eHe;
          this.move(document.getElementById(elem), aX, aY);
          if (this.lines.length !== 0){
            this.lines.forEach((line) => {
                line.position()
            })
          }
        };
      },
      stopMoving(elem) {
        document.getElementById(elem).parentElement.style.cursor = "default";
        document.onmousemove = () => {};

      },
      startArrow(elem){
        /* eslint-disable no-undef */
        // const buffArr = [elem]
        this.tempArrow.length = 0
        this.tempArrow.push(elem)

      },
      stopArrow(elem) {
        // this.tempArrow.push(elem)
        // console.log(this.tempArrow)
        // console.log(this.tempArrow[this.tempArrow.length - 1])
        this.tempArrow.push(elem)
        // this.drawArrow()
        // this.tempArrow.forEach((line) => {
        //     this.line = new LeaderLine(
        //     document.getElementById(line[0]),
        //     document.getElementById(line[1]),
        //     {
        //         size: 5,
        //         color: 'white'
        //     })
        // })
        this.line = new LeaderLine(
            document.getElementById(this.tempArrow[0]),
            document.getElementById(this.tempArrow[1]),
            {
                size: 5,
                color: 'white',
                dash: false,
            })
        // console.log(this.line)
        this.lines.push(this.line)
        // if (this.tempArrow.length === 2){
        //     this.line = new LeaderLine(
        //     document.getElementById(this.tempArrow[0]),
        //     document.getElementById(this.tempArrow[1]),
        //     {
        //         size: 5,
        //         color: 'white'
        //     })
        //     this.tempArrow.length = 0
        //     this.lines.push(this.line)
        // }
        // console.log("List line", this.lines)
        document.getElementById(elem).parentElement.style.cursor = "default";
        document.onmousemove = () => {}


      },
      drawArrow(){
        // if (this.lines.length !== 0){
        //     this.lines.forEach((line) => {
        //         line.remove()
        //     })
        //   }
        this.tempArrow.forEach((line) => {
            this.line = new LeaderLine(
            document.getElementById(line[0]),
            document.getElementById(line[1]),
            {
                size: 5,
                color: 'white'
            })
        })
        // console.log(this.line)
        this.lines.push(this.line)
        // console.log(this.lines)
      }
    },
  };
  </script>
        <style>
        #container {
            position:absolute;
            background-color: rgb(36, 36, 36);
            }
        .elem{
            padding-top: 25px;
            position: absolute;
            background-color: rgb(26, 0, 128);
            -webkit-user-select: none;
            -moz-user-select: none;
            -o-user-select: none;
            -ms-user-select: none;
            -khtml-user-select: none;     
            user-select: none;
        }
        .knob{
            position: absolute;
            width: 15px;
            height: 15px;
            border-radius: 50%;
            background-color: rgb(158, 158, 158);
            outline: solid 1px black;
        }

        .kright{
            right: -7px;
            top: 40px;
        }
        .kleft{
            left: -7px;
            top: 40px;
        }
        .ktop{
            right: 70px;
            top: -8px;
        }
        .kbottom {
            right: 70px;
            bottom: -8px;
        }
        .arrow{
  stroke:rgb(255, 255, 255);
  stroke-width:2; 
  marker-end:url(#markerArrow)
}
    </style>   -->
    <template>
      <div>
        <div ref="flowchartContainer" />
      </div>
    </template>
    
    <script>
    import * as Drawflow from 'drawflow';
    // eslint-disable-next-line no-unused-vars
    import * as leaderline from 'leader-line';
    
    export default {
      data() {
        return {
          flowchart: null,
          connectors: [],
        };
      },
      mounted() {
        this.flowchart = new Drawflow(this.$refs.flowchartContainer);
    
        this.flowchart.addNode({
          id: 'node1',
          x: 100,
          y: 100
        });
        // const startShape = this.flowchart.addShape({
        //   type: 'rect',
        //   x: 50,
        //   y: 50,
        //   width: 100,
        //   height: 50,
        //   label: 'Start',
        // });
        // const endShape = this.flowchart.addShape({
        //   type: 'rect',
        //   x: 250,
        //   y: 50,
        //   width: 100,
        //   height: 50,
        //   label: 'End',
        // });
    
        // const connector = leaderline.pathfinder([startShape.getNode(), endShape.getNode()], {
        //   startPlug: 'disc',
        //   endPlug: 'arrow2',
        //   color: '#333',
        //   endPlugColor: '#333',
        //   path: [
        //     {x: 50, y: 100},
        //     {x: 250, y: 100},
        //   ],
        //   startSocket: 'bottom',
        //   endSocket: 'top',
        // });
        // this.connectors.push(connector);
      },
      beforeDestroy() {
        this.connectors.forEach(connector => connector.remove());
      },
    };
    </script>