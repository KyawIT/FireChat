<script>
    import { initializeApp } from "firebase/app";
    import { getFirestore, collection, addDoc } from "firebase/firestore";  
    import { createEventDispatcher } from "svelte";
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

    const dispatch = createEventDispatcher();
    let inputMessage, date, user = localStorage.getItem("UserName"), userImg = localStorage.getItem("UserImage"), userId = localStorage.getItem("UID");

    function GetDate(){
        var currentDate = new Date();
        var dd = String(currentDate.getDate()).padStart(2, '0');
        var mm = String(currentDate.getMonth() + 1).padStart(2, '0');
        var yyyy = currentDate.getFullYear();
        var hours = currentDate.getHours(); 
        var minutes = currentDate.getMinutes();
        var seconds = currentDate.getSeconds();
        minutes < 10 ? minutes = "0"+ minutes : minutes;
        seconds < 10 ? seconds = "0"+seconds : seconds;
        currentDate = dd + '.' + mm + '.' + yyyy + " at " + hours + ":" + minutes + ":" + seconds;
        return currentDate;
    }

    const SendMsg = e =>{
        if(e.charCode === 13 && inputMessage)
        {
            date = GetDate();
            const MessageInfo = {
                user: user,
                userId: userId,
                userImg: userImg,
                msg: inputMessage,
                date: Date.now()
            }
            addDoc(docRef, MessageInfo); // Add to Firebase
            dispatch("SendMessage", MessageInfo);
            inputMessage = "";
        }
    }
</script>

<div class="input-group fixed-bottom">
    <input on:keypress="{SendMsg}" bind:value="{inputMessage}" type="text" class="form-control" placeholder="Message" aria-label="Message" aria-describedby="basic-addon1">
</div>

<style>
    input{
        background-color: #33363a;
        border: none;
        color:white;
    }
    input:focus{
        background-color: #33363a;
        border: none;
        color:white;
    }
</style>