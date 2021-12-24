<script>
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    let inputMessage, date, user = localStorage.getItem("UserName"), userImg = localStorage.getItem("UserImage");

    function GetDate(){
        var currentDate = new Date();
        var dd = String(currentDate.getDate()).padStart(2, '0');
        var mm = String(currentDate.getMonth() + 1).padStart(2, '0');
        var yyyy = currentDate.getFullYear();
        var hours = currentDate.getHours();
        var minutes = currentDate.getMinutes();
        minutes < 10 ? minutes = "0"+ minutes : minutes;
        currentDate = dd + '.' + mm + '.' + yyyy + " at " + hours + ":" + minutes;
        return currentDate;
    }

    const SendMsg = e =>{
        if(e.charCode === 13 && inputMessage)
        {
            date = GetDate();
            const Message = {
                user: user,
                userImg: userImg,
                msg: inputMessage,
                date: date
            }
            dispatch("SendMessage", Message);
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