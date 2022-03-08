<template>
    <div>
        <h1>
            Страница с постами
        </h1>
        <my-input 
            v-focus
            v-model="searchQuery"
            placeholder="Поиск..."
        />
        <div class="app__btns">
            <my-button
                @click="showDialog"
            >
                Создать пост
            </my-button>
            <my-select
                v-model="selectedSort"
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
    import axios from 'axios';
    export default {
        components: {
            PostForm, PostList
        },
        data() {
            return {
                posts: [],
                dialogVisible: false,
                isPostsLoading: false,
                selectedSort: '',
                searchQuery: '',
                page: 1,
                limit: 10,
                totalPages: 0,
                sortOptions: [
                    {value: 'title', name: 'По названию'},
                    {value: 'body', name: 'По содержимому'}
                ],
            }
        },
        methods: {
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
            async fetchPosts(){
                try {
                    this.isPostsLoading = true;
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                        params: {
                            _page: this.page,
                            _limit: this.limit
                        }
                    });
                    this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
                    this.posts = response.data;
                } catch (error) {
                    alert('Ошибка!')
                } finally {
                    this.isPostsLoading = false;
                }
            },
            async loadMorePosts(){
                try {
                    this.page += 1;
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                        params: {
                            _page: this.page,
                            _limit: this.limit
                        }
                    });
                    this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
                    this.posts = [...this.posts, ...response.data];
                } catch (error) {
                    alert('Ошибка!')
                }
            },
        },
        mounted() {
            this.fetchPosts();
        },
        computed: {
            sortedPosts(){
                return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]));
            },
            sortedAndSearchedPosts() {
                return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()));
            }
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