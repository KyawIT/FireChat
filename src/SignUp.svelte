<script>
    import { initializeApp } from "firebase/app";
	import { getAuth, signInWithPopup, GoogleAuthProvider } from "firebase/auth";
    import Navbar from "./components/Navbar.svelte";
    import { createEventDispatcher } from "svelte";

    let errorCodeCheck, resultCheck, user;
    // Firebase Init
    const dispatch = createEventDispatcher();
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
    const provider = new GoogleAuthProvider();

    // Google SingIn
    function GoogleSignIn() {
        signInWithPopup(auth, provider)
        .then((result) => {
            const credential = GoogleAuthProvider.credentialFromResult(result);
            const token = credential.accessToken;
            user = result.user;
            dispatch("GetUser", user);
            localStorage.setItem("UserData", user);
            location.reload();
        }).catch((error) => {
            errorCodeCheck = error.code;
        });
    }
</script>
<Navbar user={user}/>
<h1 class="display-2 text-center text-white mt-5 pb- roboto-font">Welcome to <strong>FireChat</strong>🔥</h1>

<div class="container bg-white p-4 border border-dark rounded mt-5">
    <!--Google Auth-->
    <div class="d-flex justify-content-evenly self-align-center">
        <h1 class="display-5 text-center roboto-font">
            SignIn with 
        </h1>
        <button type="button" class="btn btn-primary btn-lg w-25 ps-1 roboto-font" on:click="{GoogleSignIn}">
            <img src="https://img.icons8.com/stickers/40/000000/google-logo.png" alt="G-Logo" class="pe-4 img-fluid"/>
            SignIn
        </button>
    </div>

    {#if errorCodeCheck === "auth/email-already-in-use"}
        <div class="alert alert-danger mt-3" role="alert">There is already an account registerd with the same data!</div>
        {:else if errorCodeCheck === "auth/invalid-email"}
        <div class="alert alert-danger mt-3" role="alert">Invalid <strong>Email</strong>!</div>
        {:else if errorCodeCheck === "auth/weak-password"}
        <div class="alert alert-danger mt-3" role="alert">Weak Password! Password should be <strong>at least 6 characters</strong> !</div>
        {:else if resultCheck !== undefined}
        <div class="alert alert-success mt-3" role="alert">Your account has been <strong>successfully created</strong> !</div>
    {/if}
</div>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@300&display=swap');
    .roboto-font{
        font-family: 'Roboto Mono', monospace;
    }
</style>