
<template>
    <div id="container" style="width: 1300px;height: 700px;top:50px;left:50px;">
        <svg style="outline: solid 1px red; z-index: 999; position: absolute; left: 0px; pointer-events: none; visibility: hidden;" height="100%" width="100%">
  <defs>
    <marker id="markerArrow" markerWidth="13" markerHeight="13" refX="2" refY="6"
            orient="auto">
      <path d="M2,2 L2,11 L10,6 L2,2" style="fill: #FFFFFF;" />
    </marker>
  </defs>

  <line x1="30" y1="190" x2="100" y2="200" class="arrow" />
</svg>
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
      <div class="knob kright" v-on:mousedown="startArrow($event, block.id)" v-on:mouseup="stopArrow(block.id, block.type, $event)" ></div>
      <div class="knob kleft" v-on:mousedown="startArrow($event, block.id)" v-on:mouseup="stopArrow(block.id, block.type, $event)"></div>
      <div class="knob ktop" v-on:mousedown="startArrow($event, block.id)" v-on:mouseup="stopArrow(block.id, block.type, $event)"></div>
      <div class="knob kbottom" v-on:mousedown="startArrow($event, block.id)" v-on:mouseup="stopArrow(block.id, block.type, $event)"></div>
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
            blockArr: [
                {   
                    id: "f1",
                    type: "Input",
                    top: "40px",
                    left: "30px",
                    color: "#e76f51"
                },
                {
                    id: "f2",
                    type: "Processing",
                    top: "170px",
                    left: "30px",
                    color: "#2a9d8f"
                },
                {
                    id: "f3",
                    type: "Processing",
                    top: "300px",
                    left: "30px",
                    color: "#2a9d8f"
                },
            ],
            arrowSvg: `<svg style="outline: solid 1px red; z-index: 999; position: absolute; left: 0px; pointer-events: none;" height="100%" width="100%">
  <defs>
    <marker id="markerArrow" markerWidth="13" markerHeight="13" refX="2" refY="6"
            orient="auto">
      <path d="M2,2 L2,11 L10,6 L2,2" style="fill: #FFFFFF;" />
    </marker>
  </defs>

  <line x1="30" y1="190" x2="100" y2="200" class="arrow" />
</svg>`,
            droppedBlock: [],
        }
    },
    mounted(){

    },
    methods: {
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
        const cWi = parseInt(document.getElementById(elem).parentElement.style.width);
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
        // console.log(elem, type, evt)
        document.getElementById(elem).parentElement.style.cursor = "default";
        document.onmousemove = () => {};

      },
      startArrow(evt, elem){
        /* eslint-disable no-undef */
        console.log(elem)
        console.log(evt)
        // this.line = new LeaderLine(
        // document.querySelector('#f1'),
        // document.querySelector('#f2'),
        // {
        //     size: 5,
        //     color: 'white'
        // })
        this.tempArrow.push(elem)

      },
      stopArrow(elem) {
        // console.log(elem)
        this.tempArrow.push(elem)
        console.log(this.tempArrow)
        if (this.tempArrow.length === 2){
            this.line = new LeaderLine(
            document.querySelector('#' + this.tempArrow[0]),
            document.querySelector('#' + this.tempArrow[1]),
            {
                size: 5,
                color: 'white'
            })
            this.tempArrow.length = 0
            this.lines.push(this.line)
        }
        document.getElementById(elem).parentElement.style.cursor = "default";
        document.onmousemove = () => {}


      },
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
    </style>  