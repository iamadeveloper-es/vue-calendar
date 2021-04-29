<template>
    <div @click.prevent="changeTheme()" class="change-theme">
        <span class="ico fas fa-moon"
        :class="isActive ? 'ico-light' : 'ico-dark'"
        ></span>
    </div>
</template>

<script>
export default {
    data(){
        return{
            isActive: false
        }
    },
    methods:{
        changeTheme(){
            this.isActive = !this.isActive
            this.$emit('change-theme')
        }
    }
}
</script>

<style lang="scss" scoped>
.change-theme{
    width: 55px;
    background-color: #ffffff;
    border: 1px solid #d3d3d3;
    border-radius: 15px;
    overflow: hidden;
    margin-left: auto;
    margin-bottom: 25px;
    cursor: pointer;
    .ico {
        font-size: 16px;
        width: 25px;
        height: 25px;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 6px;
        border-radius: 50%;
        transition: all .2s ease-in-out;
        &.ico-dark{
            color: #e2e2e2;
            background-color: #242f3c;
            border: 1px solid #d3d3d3;
            transform: translateX(0);
        }
        &.ico-light{
            color: #242f3c;
            background-color: #f5f5f5;
            border: 1px solid #d3d3d3;
            transform: translateX(30px);
        }
    }
}
</style>