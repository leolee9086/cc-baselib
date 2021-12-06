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
    :resizable="!folded"
    :min-width="100"
    :min-height="25"
    :y="top"
    :w="width"
    :h="height"
    :x="left"

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
      v-if="folded"
    >
      <div
        class="lableFolded"
        slot="reference"
        :style="`position: absolute;                
        background-color:${backgroundColor};
        color:${color};
        border:solid ${borderColor} 2px;
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
        @dblclick="folded = !folded"
      >{{ index }}</div>
      <div
        :style="`
        overflow:hidden;
        border:solid ${borderColor} 1px;
        background-color:${backgroundColor};
        border-radius:5px`"
      >
        <cc-link-siyuan :style="`color:${color};`" :锚文本="anchor" :链接id="def_block"></cc-link-siyuan>

        <div :style="`font-size:xx-small;color:${color}`">双击标记展开,拖拽移动</div>
      </div>
    </el-popover>
    <div v-if="!folded"  @dblclick="folded = !folded">
      <div
        :style="`position:absolute;
                        overflow:hidden;
                        border:solid ${borderColor} ${边框宽度}px;
                        background-color:${backgroundColor};
                        width:${width-5 + 'px'};
                        height:${height-5 + 'px'};
                        border-radius:5px`
        "
      >
      <div>
        <span style="float:right">{{index}}</span>

        
        <span class="el-icon-siyuan" v-if="def_block"></span>

        <span class="el-icon-delete"  v-on:click="删除()"></span>
        <span class="el-icon-share" @click="开始连接()"></span>

        <div :style="`color:${color};`">
          <cc-link-siyuan :style="`color:${color};`" :锚文本="anchor" :链接id="def_block"></cc-link-siyuan>
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
      def_block: "",
      anchor: "锚文本为空时显示来源块内容",
      top: 100,
      left: 100,
      width: 100,
      height: 100,
      color: "black",
      backgroundColor: "yellow",
      borderColor: "red",
      showhandler: false,
      folded: true,
      id:"",
      激活: false,
      正在编辑: false,
      开始监听: false,
      边框宽度:1,
      
    }
  },
  mounted() {
    let block = this.value
    console.log("block", block)
    this.def_block = block.def_block,
    this.anchor = block.anchor,
    this.top = block.top
    this.left = block.left
    this.width = block.width
    this.height = block.height
    this.color = block.color
    this.backgroundColor = block.backgroundColor
    this.borderColor = block.borderColor
    this.showhandler = block.showhandler
    this.folded = block.folded
    this.id = block.id

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
      this.def_block = block.def_block,
      this.anchor = block.anchor,
      this.top = block.top
      this.left = block.left
      this.width = block.width
      this.height = block.height
      this.color = block.color
      this.backgroundColor = block.backgroundColor
      this.borderColor = block.borderColor
      this.showhandler = block.showhandler
      this.folded = block.folded
      this.id = block.id
      }

    },
    block(val) {
      for (prop in val) {
        this[prop] = block[prop]
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
    tagdefine() {
      if (!this.开始监听||!this.value) { return undefined }

      let obj = {}
      obj.def_block = this.def_block,
      obj.anchor = this.anchor,
      obj.top = this.top
      obj.left = this.left
      obj.width = this.width
      obj.height = this.height
      obj.color = this.color
      obj.backgroundColor = this.backgroundColor
      obj.borderColor = this.borderColor
      obj.showhandler = this.showhandler
      obj.folded = this.folded
      obj.id = this.id
      return obj
    
    }
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
      this.$emit("callbacklink", this.def_block)
    },
    dragging: function (x, y) {
      this.top = y
      this.left = x
      console.log(this.tagdefine)
      
      if (this.tagdefine){
      this.$emit("change", this.tagdefine)}
    },
    dragstop() {       if (this.tagdefine){
      this.$emit("change", this.tagdefine)}
},
    resizing: function (x, y, width, height) {
      this.top = y
      this.left = x
      this.width = width
      this.height = height
      if (this.tagdefine){
      this.$emit("change", this.tagdefine)}

    },
    resizestop: function (x, y, width, height) {
      this.top = y
      this.left = x
      this.width = width
      this.height = height
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