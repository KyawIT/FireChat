<script>
	import { getAuth, signOut, GoogleAuthProvider } from "firebase/auth";
  import { initializeApp } from "firebase/app";
  export let user;
  let userName = localStorage.getItem("UserName");
  let userImage = localStorage.getItem("UserImage");
  const firebaseConfig = {
		apiKey: "AIzaSyARRsepKYfvzCk7VT5BjVOeLr-4roSPcb4",
		authDomain: "svelte-chat-app-988d6.firebaseapp.com",
		projectId: "svelte-chat-app-988d6",
		storageBucket: "svelte-chat-app-988d6.appspot.com",
		messagingSenderId: "1019685330748",
		appId: "1:1019685330748:web:c107ffa2ac4e9e58568839"
	};
	const app = initializeApp(firebaseConfig);
	const auth = getAuth(app);
  function LogOut() {
    signOut(auth)
    .then(() =>{
      localStorage.clear();
      location.reload();
    }).catch(err =>{
      console.log(err)
    });
  }
</script>

<nav class="navbar navbar-expand-lg navbar-dark bg-dark p-3">
  <div class="container-fluid">
    <span class="navbar-brand bigger-txt">FireChat🔥</span>
    {#if user}
      <div>
        <span class="navbar-brand bigger-txt">WELCOME <u>{userName}</u></span>
        <img src="{userImage}" alt="ProfilePicture" />
      </div>
    {/if}
    <ul class="nav justify-content-end">
      {#if user}
        <li class="nav-item">
          <span class="nav-link other-txt" on:click="{LogOut}">Logout</span>
        </li>
      {/if}
    </ul>
  </div>
</nav>

<style>
  .bigger-txt{
    font-size: 2em;
    padding: 0;
  }

  .other-txt{
    font-size: 1.5em;
    transition: 0.2s;
  }

  .other-txt:hover{
    cursor: pointer;
  }

  img{
    width: 64px;
    height: 64px;
    object-fit: cover;
    border-radius: 2em;
    background-color: rgb(85, 84, 84);
  }
</style>

