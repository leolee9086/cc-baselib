<template>
  <vue-draggable-resizable
    v-if="value"
    ref="container"
    @activated="激活 = true"
    @deactivated="激活 = false"
    @dragging="dragging"
    @dragstop="dragstop"
    @resizing="resizing"
    @resizestop="resizestop"
    :resizable="!carddata.folded"
    :min-width="100"
    :min-height="25"
    :y="carddata.top"
    :w="carddata.width"
    :h="carddata.height"
    :x="carddata.left"

    :z="200"
    class-name-handle="my-handle-class"
    class-name="my-class"
   
  >
    <el-popover
      popper-class="tagpoper"
      trigger="hover"
      :open-delay="300"
      placement="right"
      :width="100"
      v-if="carddata.folded"
    >
      <div
        class="lableFolded"
        slot="reference"
        :style="`position: absolute;                
        background-color:${carddata.backgroundColor};
        color:${carddata.color};
        border:solid ${carddata.borderColor} 2px;
        width:15px;
        height:15px;
        min-width:15px;
        min-height:15px;
        max-width:15px;
        max-height:15px;
        border-radius:100%;
        text-align:center;
        padding-bottom:3px
        `"
        @dblclick="carddata.folded = !carddata.folded"
      >{{index }}</div>
      <div
        :style="`
        overflow:hidden;
        border:solid ${carddata.borderColor} 1px;
        background-color:${carddata.backgroundColor};
        border-radius:5px`"
      >
        <cc-link-siyuan :style="`color:${carddata.color};`" :锚文本="carddata.anchor" :链接id="carddata.def_block"></cc-link-siyuan>

        <div :style="`font-size:xx-small;color:${carddata.color}`">双击标记展开,拖拽移动</div>
      </div>
    </el-popover>
    <div v-if="!carddata.folded"  @dblclick="carddata.folded = !carddata.folded">
      <div
        :style="`position:absolute;
                        overflow:hidden;
                        border:solid ${carddata.borderColor} ${边框宽度}px;
                        background-color:${carddata.backgroundColor};
                        width:${carddata.width-5 + 'px'};
                        height:${carddata.height-5 + 'px'};
                        border-radius:5px`
        "
      >
      <div>
        <span style="float:right">{{index}}</span>

        
        <span class="el-icon-siyuan" v-if="carddata.def_block"></span>

        <span class="el-icon-delete"  v-on:click="删除()"></span>
        <span class="el-icon-share" @click="开始连接()"></span>
        <span class="el-icon-edit" v-if="!文字可编辑" @click="文字可编辑=true"></span>
        <span class="el-icon-check" v-if="文字可编辑" @click="文字可编辑=false"></span>

        <div :style="`color:${carddata.color};`">
          <cc-link-siyuan v-if="carddata.anchor" :style="`color:${carddata.color};`" :锚文本="carddata.anchor" :链接id="carddata.def_block"></cc-link-siyuan>
          <cc-vditor-vue v-model="carddata.markdown"></cc-vditor-vue>
        </div>
      </div>
    </div>
    </div>
  </vue-draggable-resizable>
</template>

<script>
module.exports = {
  name: "cc-block-card",
  props: ["value", "index","移除标签"],
  components: componentList,
  model: { prop: "value", event: "change" },
  data() {
    return {
      carddata:"",

      激活: false,
      正在编辑: false,
      开始监听: false,
      边框宽度:1,
      文字可编辑:false,
    }
  },
  mounted() {
    let block = this.value
    console.log("block", block)
    this.carddata = JSON.parse(JSON.stringify(block)),
   

    this.开始监听 = true
  },
  watch: {
    
    folded(val){
      if(val){
        this.$refs.container["min-width"]=30
        this.$refs.container["min-height"]=30
        this.$refs.container["width"]=30
        this.$refs.container["height"]=30
      }
      else{
        this.$refs.container["min-width"]=100
        this.$refs.container["min-height"]=30
        this.$refs.container["width"]=this.width
        this.$refs.container["height"]=this.height
      }
    },
    value(val) {
      if (val){
      let block = this.value
      this.carddata = JSON.parse(JSON.stringify(block))
      
      }
    
    },
   
    激活(val) {
      if (val) {
        this.边框宽度=3
        this.$emit("activated", "")
        this.$emit("callbacklink", "")
        this.$emit("calloutgoinglinks", "")
      }
      else {
                this.边框宽度=1
 
        this.$emit("deactivated", "") 
        }
    },
    正在编辑(val) {
      if (val) {
        this.$emit("editing", "")
      }
    },
   
  },
  computed: {
    
  },
  methods: {
    开始连接(){
      this.$emit("startlink", "")
    },
    删除(){
      if(this.开始监听 = true){
      this.移除标签
      this.$emit("delete", this.index)
      }
      },
    dblclick:function(){
      this.$emit("dblclick")
    },
    展开链接: function () {
      this.$emit("callbacklink", this.carddata.def_block)
    },
    dragging: function (x, y) {
      this.carddata.top = y
      this.carddata.left = x
      
      if (this.carddata){
      this.$emit("change", this.carddata)}
    },
    dragstop() {       if (this.carddata){
      this.$emit("change", this.carddata)}
},
    resizing: function (x, y, width, height) {
      this.carddata.top = y
      this.carddata.left = x
      this.carddata.width = width
      this.carddata.height = height
      if (this.tagdefine){
      this.$emit("change", this.carddata)}

    },
    resizestop: function (x, y, width, height) {
      this.carddata.top = y
      this.carddata.left = x
      this.carddata.width = width
      this.carddata.height = height
      this.$emit("change", this.carddata)
    },
  },
}
</script>

<style scoped>
.my-handle-class {
  position: absolute;
  background-color: lightskyblue;
  border: 1px solid black;
  border-radius: 50%;
  height: 7px;
  width: 7px;
  box-model: border-box;
  -webkit-transition: all 300ms linear;
  -ms-transition: all 300ms linear;
  transition: all 300ms linear;
}

.my-handle-class-tl {
  top: -7px;
  left: -7px;
  cursor: nw-resize;
}

.my-handle-class-tm {
  top: -7px;
  left: 50%;
  margin-left: -3.5px;
  cursor: n-resize;
}

.my-handle-class-tr {
  top: -7px;
  right: -7px;
  cursor: ne-resize;
}

.my-handle-class-ml {
  top: 50%;
  margin-top: -3.5px;
  left: -7px;
  cursor: w-resize;
}

.my-handle-class-mr {
  top: 50%;
  margin-top: -3.5px;
  right: -7px;
  cursor: e-resize;
}

.my-handle-class-bl {
  bottom: -7px;
  left: -7px;
  cursor: sw-resize;
}

.my-handle-class-bm {
  bottom: -7px;
  left: 50%;
  margin-left: -3.5px;
  cursor: s-resize;
}

.my-handle-class-br {
  bottom: -7px;
  right: -7px;
  cursor: se-resize;
}

.my-handle-class-tl:hover,
.my-handle-class-tr:hover,
.my-handle-class-bl:hover,
.my-handle-class-br:hover {
  background-color: lightyellow;
  transform: scale(1.4);
}
.el-card {
  width: 100%;
  height: 100%;
}
.my-class {
  position: absolute;
  background-color: none;
  border: none;
}
</style>