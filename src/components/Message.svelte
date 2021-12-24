<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    export let user;
    export let userImg;
    export let message;
    export let date;
    

</script>
<div class="card container msg-box mt-4 mb-4">
    <div class="card-body">
        <p class="float-end text-white">{date}</p>
        <div class="d-flex align-items-center">
            <img src="{userImg}" alt="Profile Pic"> 
            <p class="text-white ps-3 mt-3"><strong>{user}</strong></p>
        </div>
        <p class="text-white">{message}</p>
    </div>
</div>

<style>
    .msg-box{
	    background-color: #35383d;
    }

    img{
        width: 50px;
        height: 50px;
        object-fit: cover;
        border-radius: 2em;
    }
</style>