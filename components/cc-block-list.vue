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
      <cc-link-siyuan :链接id="item.id"></cc-link-siyuan>
      <el-tooltip effect="dark" content="复制到剪贴板" placement="top-start">
        <span class="el-icon-document-copy" @click="$copyText(item.id)"></span>
      </el-tooltip>
      <div
        style="
          font-size: x-small;
          margin: 5px 0px;
          color: gray;
          max-height: 3em;
          overflow: hidden;
          text-overflow: ellipsis;
        "
        v-for="block in item.blocks"
      >
        <el-tooltip effect="dark" content="复制到剪贴板" placement="top-start">
          <span class="el-icon-document-copy" @click="$copyText(block.id)"></span>
        </el-tooltip>
        <cc-link-siyuan :链接id="block.id"></cc-link-siyuan>
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
    };
  },
};
</script>
