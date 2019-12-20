<template>
    <div class="loginWrapper">
        <div class="loginBox">
            <div class="avaterBox">
                <img src="" alt="">
            </div>
        </div>
    </div>
</template>

<script>
export default {}
</script>

<style lang="less" scoped>
    .loginWrapper {
        height: 100%;
        background-color: cadetblue;
    }
    .loginWrapper .loginBox {
        position: absolute;
        width: 450px;
        height: 300px;
        background-color: #fff;
        border-radius: 3px;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
    }
</style>
