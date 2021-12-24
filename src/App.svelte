<script>
	import SignUp from "./SignUp.svelte";
	import Navbar from "./components/Navbar.svelte";
	import Input from "./components/Input.svelte";
	import Message from "./components/Message.svelte";
    import { data } from "./db.js";

	let NewData = data, userLogged = localStorage.getItem("UserData");
	const AddMsg = e =>{
		let newMsg = e.detail;
		NewData = [newMsg, ...NewData];
	}

	const GetUserInfo = e =>{
		// displayName = Name
		// photoURL = Img
		localStorage.setItem("UserName", e.detail.displayName);
		localStorage.setItem("UserImage", e.detail.photoURL);
		
	}
</script>
{#if !userLogged}
<SignUp on:GetUser={GetUserInfo}/>
{/if}
{#if userLogged}
	<Navbar user={userLogged}/>
	<main>
		{#if NewData}
			{#each NewData as element}
				<Message user={element.user} userImg={element.userImg} message={element.msg} date={element.date}/>
			{/each}
		{/if}
	</main>
	<Input on:SendMessage={AddMsg}/>
{/if}