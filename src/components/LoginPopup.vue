<template>
   <div class="popup">
       <div class="content">
           <h4> Login to your account </h4>
           <input type="text" placeholder="Email" />
           <input type="password" placeholder="Password" />
           <button> Log in </button>
       </div>
   </div>
</template>

<script>
</script>

<style scoped>
.popup {
    position: fixed;
    width: 100%;
    top: 0; 
    left: 0;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.2);
}
.content {
    min-width: 200px;
    width: 30%;
    background: #fff;
    height: 200px;
    padding: 10px;
    margin: 100px auto 0;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

}
input[type="text"], 
input[type="password"] {
    border: none;
    outline: none;
    width: 80%;
    padding: 10px;
    margin: 0 auto 10px;
    font-size: 16px;
}
button {
   border: 1px solid #000;
   margin-top: 10px;
   background-color:#8e44ad;
   color: #fff;
   padding: 5px 10px;
   font-size: 16px;
}
</style>