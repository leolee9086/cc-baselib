<template>
  <div>
    <h5 style="margin: 0px">
      {{ title }}:
      <el-tooltip effect="dark" v-if="展开" content="收起" placement="top-start">
        <span class="el-icon-arrow-up" @click="展开 = !展开"></span>
      </el-tooltip>
      <el-tooltip effect="dark" v-if="!展开" content="展开" placement="top-start">
        <span class="el-icon-arrow-down" @click="展开 = !展开"></span>
      </el-tooltip>
      <span v-if="blocklist"> {{ count || blocklist.length }}</span>
    </h5>
    <div v-if="展开" style="margin: 5px 0px" v-for="item in blocklist">
      <svg class="b3-list-item__graphic popover__block" :data-id="item.id">
      <use :xlink:href="生成图标属性(item)"></use>
      </svg>
      <cc-link-siyuan :链接id="item.id"></cc-link-siyuan>
      <slot name="action_doc" :data="item.id"></slot>
      <div
        style="
          font-size: x-small;
          margin: 5px 10px;
          color: gray;
          overflow: hidden;
          text-overflow: ellipsis;
        "
        v-for="block in item.blocks"
      >
         <svg class="b3-list-item__graphic popover__block" :data-id="item.id">
      <use :xlink:href="生成图标属性(item)"></use>
      </svg>
        <cc-link-siyuan :链接id="block.id"></cc-link-siyuan>
        <slot name="action_child" :data="block.id"></slot>
      </div>
    </div>
  </div>
</template>
<script>
module.exports = {
  props: ["blocklist", "title", "count"],
  components: componentsList,
  data() {
    return {
      展开: true,
      反链图标对照:{
        "NodeDocument":"#iconFile",
        "NodeParagraph":"#iconParagraph",
        
      },
      块子类型图标对照:{
        "h1":"#iconH1",
        "h2":"#iconH2",
        "h3":"#iconH3", 
        "h4":"#iconH4", 
        "h5":"#iconH5", 
        "h6":"#iconH6", 
        "p":"iconParagraph"
      },
      块类型图标对照:{
        "p":"#iconParagraph",
        "d":"#iconFile"
      }
    };
  },
  methods: {
    
    生成图标属性(item){
      if(item.nodeType){
        return this.反链图标对照[item.nodeType]||"#iconFile"
      }
      else if(item.depth){
       return this.反链图标对照[item.type]||"#iconFile"
      }
      else if (item.subtype){
        return this.块子类型图标对照[item.subType]||"#iconFile"
      }
      else if(item.type){
       return this.块类型图标对照[item.type]||"#iconFile"
      }
    }
  },
};
</script>
<style scoped>
  div{
    max-height: auto !important;
  }
</style>