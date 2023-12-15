<template>
    <div class="home">
        <div v-if="selected_movie.backdrop_path" class="selected_movie">
            <div class="information">
                <div class="title">
                    <h2> {{selected_movie.title}} </h2>
                    <span> 
                        {{selected_movie.release_date.slice(0,4)}} | {{parseInt(selected_movie.runtime/60)}}hr {{selected_movie.runtime%60}}min | 
                        <span v-for="(genre, index_genre) in selected_movie.genres" :key="index_genre"> {{ index_genre < selected_movie.genres.length - 1 ? genre.name + ', ' : genre.name + ''}} </span> 
                    </span>
                </div>
                <p>{{selected_movie.overview.length <= 235 ? selected_movie.overview : selected_movie.overview.split('\n')[0].slice(0,235) + '...'}}</p>
                <!-- <div class="cast">
                    <img v-for="(actor, index_actor) in filtered(movie.cast)" :key="index_actor" :src="selected_cast_url + actor.profile_path">
                </div> -->
                <div class="buttons">
                    <button> 
                        <span class="button_tooltip"> Ver detalle </span>
                        <font-awesome-icon :icon="['fas', 'clapperboard']" /> 
                    </button>
                    <button> 
                        <span class="button_tooltip"> Añadir a Favoritos </span>
                        <font-awesome-icon :icon="['fas', 'heart']" /> 
                    </button>
                    <button> 
                        <span class="button_tooltip"> Añadir a Lista </span>
                        <font-awesome-icon :icon="['fas', 'plus']" /> 
                    </button>
                </div>
            </div>
            <div class="gradient"></div>
            <img :src="selected_backdrop_url + selected_movie.backdrop_path" alt="">
        </div>
        <div v-else class="selected_movie">
            <div class="loading_selected"></div>
        </div>
        <!-- <div class="movies-container"> -->
            <div class="movies">
                <div class="icons">
                    <span v-if="selected_movie_index > 0"
                        @click="updateSelectedMovie(movies[selected_movie_index - 1], selected_movie_index - 1)">
                        <font-awesome-icon :icon="['fas', 'chevron-left']" /> 
                    </span>
                    <div v-else></div>

                    <span v-if="selected_movie_index < movies.length - 1"
                        @click="updateSelectedMovie(movies[selected_movie_index + 1], selected_movie_index + 1)">
                        <font-awesome-icon :icon="['fas', 'chevron-right']" /> 
                    </span>
                </div>
                <ul v-if="movies.length !== 0" class="movies_lista" id="movies_lista">
                    <li v-for="(movie, index) in movies" :key="index" class="movie movie-container" @click="updateSelectedMovie(movie, index)" :id="'movie_'+movie.id">
                        <div class="gradient"></div>
                        <div class="image-container">
                            <img :src="poster_url + movie.poster_path" alt="">
                        </div>
                        <h5 class="movie_title"> {{movie.title.length < 23 ? movie.title : movie.title.slice(0,20)+'...'}} </h5>
                    </li>
                </ul>
                <ul v-if="movies.length === 0" class="movies_lista" id="movies_lista">
                    <li v-for="index in 14" :key="index" class="movie">
                        <div class="image-container">
                            <img src="../assets/img/loading.gif" alt="Hola" class="image-loading">
                        </div>
                        <h5 class="movie_title"> Cargando... </h5>
                    </li>
                </ul>

            </div>
        <!-- </div> -->
        <!-- <Footer/> -->
    </div>
</template>

<script>
    import axios from 'axios';

    // import Swiper core and required modules
    import { Navigation, Pagination, Scrollbar, A11y, Autoplay } from 'swiper';
    // Import Swiper Vue.js components
    import { Swiper, SwiperSlide } from 'swiper/vue';
    // Import Swiper styles
    import 'swiper/css';
    import 'swiper/css/navigation';
    import 'swiper/css/pagination';
    import 'swiper/css/scrollbar';
    import Footer from '@/components/Footer';

    export default {
        components: {
            Swiper,
            SwiperSlide,
            Footer
        },
        setup() {
            const onSwiper = (swiper) => {
                console.log(swiper);
            };
            const onSlideChange = () => {
                console.log('slide change');
            };
            return {
                onSwiper,
                onSlideChange,
                modules: [Navigation, Pagination, Scrollbar, A11y, Autoplay],
            };
        },
        data(){
            return {
                showModal : false,
                // API
                api_base_url: 'https://api.themoviedb.org',
                api_authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI3Mjc5NDVlOWI2ZTM1MGE4NTk2ZmY5NjhjMjk2ZTA1NyIsInN1YiI6IjVmZjRiOTM2MWNmZTNhMDAzZjlkNGUzNSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ._p0IvRPjWa1EpdcOMkkUVrzHQNxexUj-hxkzx06F2q0',
                selected_backdrop_url: 'https://www.themoviedb.org/t/p/w1920_and_h1080_bestv2',
                selected_cast_url: 'https://image.tmdb.org/t/p/w300_and_h450_bestv2/',
                poster_url: 'https://www.themoviedb.org/t/p/w600_and_h900_bestv2',
                language: 'es-ES',

                selected_movie_index: 0,
                selected_movie: {},
                movies: [],

                showLeftArrow: false,
                showRightArrow: true
            }
        },
        methods: {
            async getMovieDetails(id){
                return await axios.get(`${this.api_base_url}/3/movie/${id}?`,{
                    params:{
                        'language': this.language,
                    },
                    headers: {
                        'Authorization': this.api_authorization
                    }
                }).then( async (details) =>{
                    return details.data;
                });
            },
            async getPlayingMovies(){
                await axios.get(`${this.api_base_url}/3/movie/now_playing?`,{
                    params:{
                        'page' : 1,
                        'language': this.language,
                    },
                    headers: {
                        'Authorization': this.api_authorization
                    }
                }).then(async (response) => {
                    let nowPlaying = response.data.results;
                    for (let i = 0; i < nowPlaying.length; i++) {
                        const movie = nowPlaying[i];
                        // GET DETAILS FROM MOVIE
                        let movie_details =  await this.getMovieDetails( movie.id );

                        // GET CAST FROM MOVIE 
                        let movie_credits = await axios.get(`${this.api_base_url}/3/movie/${movie.id}/credits?`,{
                            params:{
                            'language': this.language,
                            },
                            headers: {
                                'Authorization': this.api_authorization
                            }
                        }).then( (credits) =>  {
                            return credits.data.cast;
                        })
                        movie_details.cast = movie_credits.slice(0,5);
                        nowPlaying[i] = movie_details;
                    }
                    this.movies = nowPlaying;
                    this.selected_movie = nowPlaying[0];
                });
                document.getElementById(`movie_${this.selected_movie.id}`).click();
            }, 
            moveGenre(direction,genre){
                // Move Genre Slider
                direction == 'right' ? document.getElementById(`genre_${genre}`).style.transform = 'translateX(-50%)'
                                    : document.getElementById(`genre_${genre}`).style.transform = 'translateX(0%)';

                // Change enable and disabled
                var enabled = document.querySelector('.enabled');
                var disabled = document.querySelector('.disabled');
                
                enabled.classList.remove('enabled');
                enabled.classList.add('disabled');

                disabled.classList.remove('disabled');
                disabled.classList.add('enabled');
            },
            filtered( cast ){
                return cast.slice(0,4);
            },
            updateSelectedMovie( movie, index ){
                const clicked_movie = document.querySelector('.clicked_movie');
                if( clicked_movie ){
                    clicked_movie.classList.remove('clicked_movie')
                }
                document.getElementById(`movie_${movie.id}`).classList.add('clicked_movie');
                document.getElementById('movies_lista').style.transform = `translateX(${-23*index}rem)`;
                this.selected_movie = movie;
                this.selected_movie_index = index;
            }
        },
        async mounted(){
            await this.getPlayingMovies();
        }
    }
</script>

<style scope>
    .home{
        width: 100%;
        height: 100%;

        display: flex;
        justify-content: space-between;
        flex-direction: column;
        font-size: 2rem;
    }
    
    .selected_movie{
        height: 62%;
        width: 100%;
        display: flex;
        position: relative;
    }

    .selected_movie .buttons button{
        position: relative;
    }

    .selected_movie .buttons button:hover .button_tooltip{
        display:block
    }
    .button_tooltip{
        display:none;
        width: max-content;
        background-color: #fff;
        color: var(--main-color);
        font-size: 2rem;
        padding:2px 1rem;
        border: 1px solid rgba(255,255,255,.3);
        border-radius: 3px;
        box-shadow: 1px 2px 4px rgba(255,255,255,.3);
        text-align: center;
        transition: all .1s ease;
        /* transform: translateX(45%); */
        animation: bounce 2s infinite;

        position: absolute;
        z-index: 10;
        top: -3.5rem;
        left: 60%;
    }

    .selected_movie .loading_selected{
        width: 100%;
        height: 100%;
        /* background: linear-gradient(67deg, var(--main-color) 0, var(--main-color) 30%, rgba(0,0,0,.4) 55%, rgba(0,0,0,.4) 100%); */
        background: rgba(0,0,0,1);
    }
    .selected_movie .gradient,
    .selected_movie .information{
        position: absolute;
    }
    .selected_movie img{
        margin-left: auto;
        /* width: 182rem; */
        width: 100%;
        height: 100%;
        margin-left: auto;
        object-fit: cover;
        object-position: top;
    }

    .selected_movie .gradient{
            width: 100%;
            height: 100%;
            /* background: linear-gradient(67deg, var(--main-color) 0, var(--main-color) 30%, rgba(0,0,0,.4) 55%, rgba(0,0,0,.4) 100%); */
            background: rgba(0,0,0,.6);
        }
        .selected_movie .information{
            width: 45%;
            z-index: 100;
            overflow: hidden;
            position: absolute;
            top: 24%;
            left: 2rem;
            color: rgba(255,255,255,.8);
            display: flex;
            flex-direction: column;
            justify-content: start
        }
        .selected_movie .title,
        .selected_movie p{
            margin-bottom: 2.3rem;
        }
        .selected_movie .title h2{
            font-size: 5rem;
            font-weight: 600;
        }
        .selected_movie .title > span{
            font-size: 1.5rem;
            font-weight: 600;
        }

        .selected_movie .title > span{
            font-size: 2rem;
            font-weight: 600;
        }

        .selected_movie .buttons button{
            background: rgba(175,35,26,.8);
            outline: none;
            border: none;
            font-size: 2.3rem;
            color:#fff;
            margin-right:1rem;
            width: 44px;
            height: 44px;
            border-radius: 37%;
            box-shadow: 1px 2px 4px rgba(175,35,26,.4);
            cursor: pointer;
            transition: all .5s ease;
        }
        .selected_movie .buttons button:hover{
            background: rgba(175,35,26,1);
            transition: all .5s ease;
        }

    /* ---------------INICIO LISTA DE PELICULAS--------------- */
    .movies{
        width: 100vw;
        height: max-content;
        margin: auto 2rem;
        position: relative;
    }
    .movies .icons{
        width: 100vw;
        height: 100%;
        color: white;
        position: absolute;
        left: -2rem;
        display: flex;
        justify-content: space-between;
    }

    .movies .icons span{
        height: 100%;
        width: 3.5rem;
        color: transparent;
        position: relative;
        display: flex;
        justify-content: space-around;
        align-items: center;
        z-index: 100;
        transition: all .3s ease;
    }

    .movies .icons span:hover{
        background-color: rgba(0,0,0,.7);
        transition: all .5s ease;
        color: white;
        cursor: pointer
    }

    .movies_lista{
        height: 100%;
        display: flex;
        width: max-content;
        transition: all .6s ease;
        padding-left: 2rem;
    }

    .movies .movie{
        width: 22rem;
        position: relative;
        margin-right: 1rem;
        cursor: pointer;
        top:0;
        transition: all .5s ease;
        position: relative;
    }
    .movies .movie .image-container,
    .movies .movie img{
        margin-left: auto;
        height: 31rem;
        width: 100%;
        border-radius: 2px
    }
    .movies .movie img{
        box-shadow: 0 2px 4px rgba(0,0,0,.15);
    }
    .movies .movie .image-loading{
        object-fit: cover;
    }

    .movie-container{
        opacity: 0;
        animation: fadeIn .75s ease forwards;
    }

    .movies .movie .gradient{
        width: 100%;
        height: 31rem;
        position: absolute;
        top: 0;
        background: rgba(0,0,0,.4);
        box-shadow: 0 2px 4px rgba(0,0,0,.15);
        transition: all .3s ease;
    }
    .movies .movie .movie_title{
        color: rgba(0,0,0,.5);
        font-size: 1.7rem;
        font-weight: bold;
        margin-top: .8rem;
        text-shadow: 0 2px 4px rgba(0,0,0,.15);
        transition: all .3s ease;
        position: absolute;
    }

    .movies .clicked_movie{
        top: -2rem;
    }
    .movies .clicked_movie .gradient{
        background: transparent;
    }
    .movies .clicked_movie .movie_title{
        color: rgba(0,0,0,.8);
    }

    @keyframes infinite_slider {
        0%{
            -webkit-transform: translateX(0);
            transform: translateX(0)
        }
        100%{
            -webkit-transform: translateX(calc(-14rem * 12));
            transform: translateX(calc(-14rem * 12));
        }
    }

    @keyframes fadeIn {
        to {
            opacity: 1;
        }
    }
    /* ---------------FIN LISTA DE PELICULAS--------------- */
</style>