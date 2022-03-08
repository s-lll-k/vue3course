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
            <my-button>
                Создать пост
            </my-button>
            <my-select
                v-model="selectedSort"
                :options="sortOptions"
            />
        </div>
        <my-dialog v-model:show="dialogVisible">
            <post-form/>
        </my-dialog>
        <post-list 
            :posts="sortedAndSearchedPosts"  
            v-if="!isPostsLoading"
        />
        <div v-else>Идет загрузка...</div>
    </div>
</template>    

<script>
    import PostForm from '@/components/PostForm';
    import PostList from '@/components/PostList';
    import usePosts from '@/hooks/usePosts';
    import useSortedPosts from '@/hooks/useSortedPosts';
    import useSortedAndSearchedPosts from '@/hooks/useSortedAndSearchedPosts';
    export default {
        components: {
            PostForm, PostList
        },
        data() {
            return {
                dialogVisible: false,
                sortOptions: [
                    {value: 'title', name: 'По названию'},
                    {value: 'body', name: 'По содержимому'}
                ],
            }
        },
        setup(props){
            const {posts, totalPages, isPostsLoading} = usePosts(10);
            const {selectedSort, sortedPosts} = useSortedPosts(posts);
            const {searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts);

            return {
                posts,
                totalPages, 
                isPostsLoading,
                selectedSort,
                sortedPosts,
                searchQuery,
                sortedAndSearchedPosts
            }
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