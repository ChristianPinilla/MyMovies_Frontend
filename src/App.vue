<template>
    <div class="main_container">
        <Navbar/>
        <div class="views_container">
            <router-view/>
        </div>
    </div>
</template>

<script>
// import $ from 'jquery';
import Navbar from '@/components/Navbar';

export default {
    components:{
        Navbar
    },  
    setup() {
        
    },
    mounted(){
        
    }
}
</script>


<style>
    @import url('https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@600&family=Inter&display=swap');
    :root{
        --background-color: #eee;
        --main-color: rgba(14,14,14,1);
    }
    html{
        font-size: 10px;
    } 
    .main_container{
        background-color: var(--background-color);
        height: 100vh;
        width: 100vw;
        overflow: hidden;
        padding: 0;
    }

    .views_container{
        width: 100%;
        height: 100%;
        /* overflow-y: scroll;
        overflow-x: hidden; */
        /* scroll-behavior: smooth; */
    }

    .views_container > *{
        font-family: 'Inter', sans-serif;
    }

    .views_container::-webkit-scrollbar{
        width: 12px ;
        background-color: rgba(255,255,255,.1);
        border-radius: 20px;
        opacity: 0;
    }
    .views_container::-webkit-scrollbar-thumb{
        width: 15px ;
        background-color: rgba(255,255,255,1);
        border-radius: 20px;
    }

    @keyframes bounce {
        0% {
            transform: translateY(30%);
        }
        50%{
            transform: translateY(60%);
        }
        100% {
            transform: translateY(30%);
        }
    }
</style>
