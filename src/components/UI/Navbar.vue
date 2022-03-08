<template>
    <nav class="navbar">
        <h2 @click="$router.push('/')">
            Vue 3
        </h2>
        <div class="navbar__btns">
            <my-button class="navbar__btn" @click="$router.push('/posts')">Посты</my-button>
            <my-button class="navbar__btn" @click="$router.push('/about')">Об этом сайте</my-button>
            <my-button class="navbar__btn" @click="$router.push('/store')">store</my-button>
            <my-button class="navbar__btn" @click="$router.push('/composition')">composition</my-button>
        </div>
    </nav>
</template>

<script>
import MyButton from './MyButton.vue'
export default {
  components: { MyButton },

}
</script>

<style scoped>
    .navbar{
        height: 50px;
        background-color: rgb(255, 255, 255);
        box-shadow: 2px 2px 4px gray;
        display: flex;
        align-items: center;
        padding: 0 15px;
    }
    .navbar > h2 {
        cursor: pointer;
    }
    .navbar__btns{
        margin-left: auto;
    }
    .navbar__btn + .navbar__btn{
        margin-left: 15px;
    }
</style>