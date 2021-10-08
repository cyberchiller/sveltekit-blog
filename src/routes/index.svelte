<script context="module">

    export const load = async ({fetch}) => {
        const res = await fetch("https://jsonplaceholder.typicode.com/posts");
        const posts = await res.json();
        return {
            props: {
                posts,
            }
        }
    }
</script>

<script>
    // import { paginate, LightPaginationNav } from 'svelte-paginate';
    export let posts;
    let searchTerm = "";
    $: searchedPosts = posts.filter((post) => {
        return post.title.includes(searchTerm);
    })

    // let items = (!searchedPosts? posts: searchedPosts);
    // let items = posts
    // let currentPage = 1;
    // let pageSize = 4;
    // $: paginatedItems = paginate({ items, pageSize, currentPage });

</script>

<h1>Posts</h1>
<input type="text" placeholder="Enter search term" bind:value={searchTerm}>
<div class="posts">
    {#each searchedPosts as item}
        <div class="post">
            <h2>{item.title.substring(0,20)}</h2>
            <p>{item.body.substring(0,80)}</p>
            <p class="link"><a sveltekit:prefetch href={`/blog/${item.id}`}>Read More</a></p>
        </div>
    {/each}
</div>

<!-- <LightPaginationNav
totalItems="{items.length}"
pageSize="{pageSize}"
currentPage="{currentPage}"
limit="{1}"
showStepOptions="{true}"
on:setPage="{(e) => currentPage = e.detail.page}"
/> -->

<style>
    .posts {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 15px;
        margin: 15px 0;
    }

    .post {
        border: 1px solid #ddd;
        padding: 10px;
        box-shadow: 0 0 10px #eee;
    }

    input {
        border: 1px solid #ccc;
        padding: 10px 20px;
        border-radius: 5px;
    }
    
    h2 {
        margin:0;
    }

    .link {
        text-align: right;
    }

    @media screen and (max-width: 600px){
        .posts {
        display: grid;
        grid-template-columns: 1fr;
        gap: 15px;
        margin: 30px 0;
    }

    }
</style>

