<template>
    <el-button @click="加载句柄()" icon="el-icon-folder" size="mini"></el-button>
</template>
<script>
    module.exports={
        props:["type"],
        name:"cc-loader-file",
        data(){
            return {
                fileHandle:{},
                directoryHandle:{}
            }
        },
        methods: {
            加载句柄:function(){
                this.type=="file"?this.加载文件():this.加载文件夹()
            },
            加载文件:async function(){
                this.fileHandle=await window.showOpenFilePicker()
                this.$emit("filechange",this.fileHandle)
            },
            加载文件夹:async function(){
                this.directoryHandle = await window.showDirectoryPicker()
                this.$emit("filechange",this.directoryHandle)
            }
        },
    }
</script>