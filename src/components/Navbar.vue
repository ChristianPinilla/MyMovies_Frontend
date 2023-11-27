<template>
    <nav class="navbar">
        <ul class="nav">
            <li class="logo"> <img src="../assets/img/logo.png" alt="Logo"> </li>
            <li class="views">
                <ul class="views_list">
                    <li>
                        <!-- <span class="views_tooltip"> Inicio </span> -->
                        <router-link to="/" class="views_link selected"> 
                            <!-- <font-awesome-icon :icon="['fas', 'house']" />  -->
                            Inicio
                        </router-link>
                    </li>
                    <li>
                        <!-- <span class="views_tooltip"> Mis Listas </span> -->
                        <router-link to="/" class="views_link"> 
                            <!-- <font-awesome-icon :icon="['fas', 'list']" />  -->
                            Mis Listas
                        </router-link>
                    </li>
                    <li class="li_search">
                        <input type="text" placeholder="Buscar..." class="search_input">
                        <!-- <span class="views_tooltip"> Buscar </span>     -->
                        <router-link :to="getSearchLink" class="views_link"> 
                            <font-awesome-icon :icon="['fas', 'magnifying-glass']" /> 
                        </router-link>
                    </li>
                </ul>
            </li>
            <li class="profile">
                <img src="../assets/img/profile.jpg" alt="Christian Pinilla" @click="changeVisibility()">
                <div class="list" id="profile_list">
                    <div class="theme">
                        <div for=""> 
                            <input type="checkbox" id="theme_checkbox"> 
                            <label class="check" for="theme_checkbox"> Modo oscuro </label>
                        </div>
                    </div>
                    <div class="settings">
                        <router-link to="/"> 
                            <font-awesome-icon :icon="['fas', 'gear']" /> 
                            <span> Configuración </span>
                        </router-link>
                    </div>
                    <div class="logout">
                        <router-link to="/"> 
                            <font-awesome-icon  :icon="['fas', 'right-from-bracket']" /> 
                            <span> Cerrar Sesión </span>
                        </router-link>
                    </div>
                </div>
            </li>
        </ul>
    </nav>
</template>

<script>
export default {
    name:'Sidebar',
    data(){
        return {
            searchQuery: ''
        }
    },
    methods:{
        changeVisibility(){
            const list = document.getElementById('profile_list');
            list.classList.toggle('visible');
        },
        getSearchLink() {
            return {
                path: '/search',
                query: {
                    search: this.searchQuery
                }
            };
        }
    },
}
</script>

<style scoped>
    .navbar{
        position: fixed;
        z-index: 1000;
        top: 0;
        width: 100%;
        height: 7.7rem;
        padding: 0 2rem;
        background-color: transparent
    }
        .nav{
            width:100%;
            height: 100%;
            /* margin: .2rem 1.2rem; */

            display: flex;
            justify-content: space-between;
            align-items: center;

            font-size: 2rem;
            font-weight: 600;
            font-family: 'Barlow Condensed', sans-serif;
            color: #fff
        }
            .logo{
                display: flex;
                align-items: center;
                width: 18.3rem;
                height: 7.8rem;
            }

            .views{
                width: 62.9rem;
                height: 14.5%;
            }
            .views_list{
                display: flex;
                height: 100%;
                width: 100%;
                justify-content: space-between;
                align-items: center;
                font-size: 3rem;
                font-weight: 600;
            }
            .views_list li{
                position: relative;
            }
            .views_list li:hover .views_tooltip{
                display:block
            }
            .views_link{
                color:#fff;
                padding: 7px;
                transition: all 1s ease;
                position: relative;
            }
            .views_link.selected{
                color: rgba(175,35,26,1);
                border-bottom: 2px solid rgba(175,35,26,1)
            }
            .views_tooltip{
                display:none;
                width: 7.5rem;
                background-color: #fff;
                color: var(--main-color);
                font-size: 2rem;
                padding:1px 0 1px 2px;
                border: 1px solid rgba(255,255,255,.3);
                border-radius: 3px;
                box-shadow: 1px 2px 4px var(--background-color);
                text-align: start;
                transition: all .1s ease;
                transform: translateX(45%);
                animation: bounce 2s infinite;

                position: absolute;
                z-index: 10;
                top: -3.5rem;
            }
            .search_input{
                background: rgba(255,255,255,.25);
                border-radius: 20px;
                border: 1px solid rgba(255,255,255,.25);
                outline: none;
                padding-left: 4rem;
                padding: .5rem 4rem;
                box-shadow: 1px 2px 3px rgba(0,0,0,.2);
                font-size: 3rem;
                color: #ffffff;
            }
            .search_input::placeholder{
                color: rgba(255,255,255,.8);
            }
            .li_search .views_link{
                position: absolute !important;
                left: 1rem;
                top: 2px;
                padding: 0;
            }
            .li_search .svg-inline--fa{
                height: .8em;
            }
            .profile{
                position: relative;
                cursor: pointer;
            }
            .profile hr{
                position: absolute;
                top: -1.5rem;
                left: -1rem;
                width: 140%;
                border: 1px solid rgba(255,255,255,.2);
            }
            .profile img{
                width: 4rem;
                height: 4rem;
                border-radius: 50%
            }
            .profile .list{
                display:none;
                flex-direction: column;
                justify-content: space-evenly;
                width: max-content;
                height: 10rem;
                background-color: #fff;
                color: rgba(0,0,0,1);
                padding: 4px 6px;
                border-radius: 3px;
                box-shadow: 1px 2px 4px rgba(0,0,0,.4);

                position:absolute;
                z-index: 100;
                top: 2.9rem;
                right: 100%;
                transform: translateX(19%);
                transition: all 1s ease;
            }
            .profile .list.visible{
                display: flex;
            }
            .profile .list *{
                color: rgba(0,0,0,1);
                height: 2.2rem;
            }
            .profile input[type="checkbox"]{
                visibility: hidden;
                display: none;
            }
            .profile .check{
                display: block;
                width: 100%;
                height: 2.3rem;
                padding-left: 2.5rem;
                padding-right: 1rem;
                background: #fff;
                border-radius: 20px;
                box-shadow: 1px 2px 4px rgba(0,0,0,.4);
                cursor:pointer;
                overflow:hidden;
                transition: all .5s ease;

                position: relative;
                display: flex;
                align-items: center;
            }
            .profile input[type="checkbox"]:checked ~ .check{
                background-color: #000;
                color: #fff;
                padding-left: 1rem;
                padding-right: 2.5rem;
            }
            .check:before{
                content: '';
                position: absolute;
                top: 0;
                bottom: 0;
                margin: auto;
                left: 4px;
                transform: translateX(10%);
                background: #000;
                width: 1.6rem;
                height: 1.6rem;
                border-radius: 50%;
                box-shadow: 1px 2px 4px rgba(0,0,0,.4);
                transition: all 0.5s ease;
            }

            .profile input[type="checkbox"]:checked ~ .check:before{
                background-color: #fff;
                transform: translateX(-200%);
            }

            .check:after{
                content: '';
                position: absolute;
                top: 0;
                bottom: 0;
                margin: auto;
                right: 4px;
                transform: translateX(200%);
                background: #000;
                width: 1.6rem;
                height: 1.6rem;
                border-radius: 50%;
                box-shadow: 1px 2px 4px rgba(0,0,0,.4);
                transition: all 0.5s ease;
            }

            .profile input[type="checkbox"]:checked ~ .check:after{
                background-color: #fff;
                transform: translateX(0%);
            }
    
</style>