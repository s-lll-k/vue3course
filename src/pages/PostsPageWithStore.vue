<template>
    <div>
        <h1>
            Страница с постами
        </h1>
        <my-input 
            v-focus
            :model-value="searchQuery"
            @update:model-value="setSearchQuery"
            placeholder="Поиск..."
        />
        <div class="app__btns">
            <my-button
                @click="showDialog"
            >
                Создать пост
            </my-button>
            <my-select
                :model-value="selectedSort"
                @update:model-value="setSelectedSort"
                :options="sortOptions"
            />
        </div>
        <my-dialog v-model:show="dialogVisible">
            <post-form
                @create="createPost"
            />
        </my-dialog>
        <post-list 
            :posts="sortedAndSearchedPosts"  
            @remove="removePost"
            v-if="!isPostsLoading"
        />
        <div v-else>Идет загрузка...</div>
        <div v-intersection="loadMorePosts" class="observer"></div>
        <!-- <my-pagination :page="page" :totalPages="totalPages" @changePage="changeCurrentPage" /> -->
    </div>
</template>    

<script>
    import PostForm from '@/components/PostForm';
    import PostList from '@/components/PostList';
    import {mapState, mapGetters, mapMutations, mapActions} from 'vuex';
    export default {
        components: {
            PostForm, PostList
        },
        data() {
            return {
                dialogVisible: false
            }
        },
        methods: {
            ...mapMutations({
                setPage: 'post/setPage',
                setSearchQuery: 'post/setSearchQuery',
                setSelectedSort: 'post/setSelectedSort'
            }),
            ...mapActions({
                loadMorePosts: 'post/loadMorePosts',
                fetchPosts: 'post/fetchPosts',
            }),
            createPost(post){
                this.posts.push(post);
                this.dialogVisible = false;
            },
            removePost(post){
                this.posts = this.posts.filter(p => p.id !== post.id);
            },
            showDialog(){
                this.dialogVisible = true;
            },
            // changeCurrentPage(newPage){
            //     this.page = newPage;
            // },
            
        },
        mounted() {
            this.fetchPosts();
        },
        computed: {
            ...mapState({
                posts: state => state.post.posts,
                isPostsLoading: state => state.post.isPostsLoading,
                selectedSort: state => state.post.selectedSort,
                searchQuery: state => state.post.searchQuery,
                page: state => state.post.page,
                limit: state => state.post.limit,
                totalPages: state => state.post.totalPages,
                sortOptions: state => state.post.sortOptions
            }),
            ...mapGetters({
                sortedPosts: 'post/sortedPosts',
                sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'
            })
        },
        watch: {
            // page(){
            //     this.fetchPosts()
            // }
        }
    }
</script>

<style>
    .app__btns{
        display: flex;
        justify-content: space-between;
        margin: 15px 0;
    }

    .observer{
        height: 30px;
        background-color: teal;
    }
</style>