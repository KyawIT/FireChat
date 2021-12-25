<script>
	import SignUp from "./SignUp.svelte";
	import Navbar from "./components/Navbar.svelte";
	import Input from "./components/Input.svelte";
	import Message from "./components/Message.svelte";
	import { initializeApp } from "firebase/app";
    import { getFirestore, collection, onSnapshot, query, doc} from "firebase/firestore";  
    const firebaseConfig = {
		apiKey: "AIzaSyARRsepKYfvzCk7VT5BjVOeLr-4roSPcb4",
		authDomain: "svelte-chat-app-988d6.firebaseapp.com",
		projectId: "svelte-chat-app-988d6",
		storageBucket: "svelte-chat-app-988d6.appspot.com",
		messagingSenderId: "1019685330748",
		appId: "1:1019685330748:web:c107ffa2ac4e9e58568839"
	};
	const app = initializeApp(firebaseConfig);
    const db = getFirestore();
    const docRef = collection(db, "Chat");

	let dataMsg = [], userLogged = localStorage.getItem("UserData");

	onSnapshot(docRef,(snapShot) =>{
		dataMsg = []
		dataMsg = (snapShot.docs.map(doc => doc.data()))
		console.log(dataMsg)
	} )

	const AddMsg = e =>{
		let newMsg = e.detail;
		console.log(dataMsg)
		dataMsg = [newMsg, ...dataMsg];
	}

	const GetUserInfo = e =>{
		localStorage.setItem("UserName", e.detail.displayName);
		localStorage.setItem("UserImage", e.detail.photoURL);
		localStorage.setItem("UID", e.detail.uid);
	}
	
</script>
{#if !userLogged}
<SignUp on:GetUser={GetUserInfo}/>
{/if}
{#if userLogged}
	<Navbar user={userLogged}/>
	<main>
		{#if dataMsg}
			{#each dataMsg.sort((a,b) => a.date - b.date) as element}
				<Message user={element.user} userImg={element.userImg} message={element.msg} date={element.date}/>
			{/each}
		{/if}
	</main>
	<Input on:SendMessage={AddMsg}/>
{/if}