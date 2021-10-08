<script context="module">
    export const load = async({page, fetch}) => {
        const id = page.params.id;
        // 1. 
        // const res = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`);
        // const user = await res.json();

        // const resPosts = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${id}`);
        // const posts = await resPosts.json();

        // 2.
        // const [ res, resPosts ] = await Promise.all([
        //     fetch(`https://jsonplaceholder.typicode.com/users/${id}`),
        //     fetch(`https://jsonplaceholder.typicode.com/posts?userId=${id}`),
        // ])
        // const user = await res.json();
        // const posts = await resPosts.json();

        // 3.
        const res = await fetch(`https://jsonplaceholder.typicode.com/users/${id}?_embed=posts`);
        const user = await res.json();
        const posts = user.posts;
        
        return {
            props: {
                user,
                posts
            }
        }
    }
</script> 

<script>
import { each } from "svelte/internal";

    export let user;
    export let posts;
</script>


<h1>Author page</h1>
<p>{user.name}</p>
<p>{user.email}<p>
<p>{user.company.catchPhrase}<p>
<h2>Posts by this {user.name}</h2>
<ul>
    {#each posts as post}
        <li>
            <a sveltekit:prefetch href={`/blog/${post.id}`}>{post.title}</a>
        </li>
    {/each}
</ul>

