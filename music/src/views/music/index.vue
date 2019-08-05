<template>
    <div>music</div>
</template>
<script>
export default {
    created(){
        this.$api.music.getMusic().then(res => {
            console.log(res)
        })
    }
}
</script>