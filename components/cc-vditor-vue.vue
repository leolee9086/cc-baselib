<template>
    <div id = "vditor-container" ref="vditorContainer"></div>
    
</template>
<script>
    module.exports={
        name:"cc-viditor-vue",
        props:["value"],
        model:{event:"change",data:"value"},
        mounted(){
            let that =this
            this.editordata=this.value
            this.Editor = new Vditor(
                this.$refs.vditorContainer,{
                height:600,
                height: 360,
                toolbarConfig: {
                    pin: true,
                },
                cache: {
                    enable: false,
                },
                input:(value=>
                    that.保存缓存内容(value)
                )	,
                after:(()=>
                    that.Editor.setValue(that.value)
                )
            },
            )
        },
        data(){
            return{
                Editor: '',
                
            }

        },
        methods:{
            保存缓存内容:function (value){
                this.$emit("change",value)
            }
        },
    }

</script>
<style>
</style>