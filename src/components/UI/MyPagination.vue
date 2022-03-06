<template>
    <div class="page__wrapper">
        <my-button 
            class="page__btn" 
            :key="pageNum" 
            v-for="pageNum in totalPages"
            :class="{
                'current-page': page === pageNum,
            }"
            @click="changePage"
        >
            {{ pageNum }}
        </my-button>
    </div>
</template>

<script>
import MyButton from './MyButton.vue'
export default {
  components: { MyButton },
    name: 'my-pagination',
    props: {
        page: {
            type: Number
        },
        totalPages: {
            type: Number
        }
    },
    methods: {
        changePage(event){
            this.$emit('changePage', +event.target.textContent);
        }
    }
}
</script>

<style scoped>
    .page__wrapper{
        margin-top: 15px;
        display: flex;
        flex-wrap: wrap;
        margin-left: -15px;
        margin-bottom: -15px;
    }
    .page__btn{
        margin-left: 15px;
        margin-bottom: 15px;
    }

    .current-page{
        border-width: 3px;
    }
</style>