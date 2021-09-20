<template>
    <div id="app">
        <app-header :changeSearch="changeSearch"/>
        <div class="container">
                    <h1 class="pt-3 pb-3">Categories:</h1>
                    <!-- <pre>search : {{search}}</pre> -->
                    
                    <!-- <pre>{{characters}}</pre> -->
                    <app-modal :character="characters[characterIndex]"/>
                    <spinner v-if = "loading"/>
                    <!-- <pre>{{characterIndex}}</pre> -->
                                        
                    <div v-else class="row">
                            <h5 v-if="!searchCharacters.length && 
                                !loading">
                                Ничего не найдено по запросу: 
                                {{search}}</h5>
                                <div id="app">
                            <div class="btn-group">
                            <!-- Sort by Date -->
                            <button type="button" class="btn btn-light" v-on:click="sortByDate()">
                                <span v-if="oldestFirst">Oldest First</span>
                                <span v-else>Newest First</span>
                            </button>
                            <ul>
                                <li v-for="el in name" :key="el.id">{{el.modified}} </li>
                            </ul>
                            <ul>
                                <li v-for="el in newDates" :key="el.id">{{el.modified}}</li>
                            </ul>
                            <!-- Sort By Price -->
                            <!-- <button type="button" 
                            class="btn btn-light" 
                            v-on:click="sortByPrice()">
                                <span v-if="cheapestFirst">RUB MIN</span>
                                <span v-else>RUB MAX</span>
                            </button>
                            <ul>
                                <li v-for="el in name" :key="el.id">{{el.price}} </li>
                            </ul>
                            <ul>
                                <li v-for="el in newPrice" :key="el.id">{{el.price}}</li>
                            </ul> -->
                                </div>
                            </div>


                            
                            <div v-for="(el, idx) in searchCharacters"
                                :key="el.id"
                                class="card mb-3 col-sm-12 col-md-6 col-lg-4">
                            <div class="row g-0">
                                <div class="col-4">
                                    <img 
                                        :src="el.thumbnail"
                                        style="max-width: 100%;">
                                </div>
                                <div class="col-8">
                                    <div class="card-body">
                                        <h5 class="card-title">{{el.name}}</h5>
                                            <button type="button"
                                                    data-bs-toggle="modal"
                                                    data-bs-target="#exampleModal"
                                                    class="btn btn-secondary btn-sm"
                                                    @click="characterIndex = idx">
                                                        Click to see it
                                            </button>
                                <h5>RUB {{el.price}}</h5>
                                <p class="date">Date First Available: {{el.modified}}</p>         
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Spinner from "./components/Spinner";
    import AppModal from "./components/AppModal";
    import AppHeader from "./components/AppHeader";
    
    export default {
        name: 'App',
        components: {
            AppHeader,
            AppModal,
            Spinner
        },
        data() {
            return {
                loading: false,
                characters: [],
                characterIndex: 0,
                search: '',
                oldestFirst: false,
                cheapestFirst: true
            }
        },
        methods: {
            fetchCharacters: function() {
                return fetch('https://api.jsonbin.io/b/61280bbd076a223676b1b0c5/4')
                        .then(res => res.json())
                        .then(json => this.characters = json)
                        
            },
            changeSearch: function (value) {
                this.search = value
            },
            sortByDate: function() {
                this.oldestFirst = !this.oldestFirst;
            },
            sortByPrice: function() {
                this.cheapestFirst = !this.cheapestFirst;
            }
        },
        computed: {
            searchCharacters: function() {
                const {search, characters} = this;
                return characters.filter((characters) => {
                    return characters.name.toLowerCase()
                    .indexOf(search.toLowerCase()) !== -1
                })
            },
            newDates: function() {
                var order = this.oldestFirst ? 1 : -1;
                this.characters.sort(function(a, b) {
                    a = new Date(a.modified);
                    b = new Date(b.modified);
                    var results = a > b ? -1 : a < b ? 1 : 0;
                    return results * order
                })
            },
            
            newPrice: function() {
                var cheap = this.cheapestFirst ? 1 : -1;
                this.characters.sort(function(a, b) {
                var results = (a.price - b.price) > 0 ? -1 : 
                                (a.price - b.price) < 0 ? 1 :0;
                    return results * cheap
                })
                
            }
        },
        async mounted() {
            this.loading = true
            await this.fetchCharacters()
            this.loading = false
        }
    }

</script>


<style>
.row {
    justify-content: space-around
}
.thumbnail {
    margin-block: 1.2rem
}
.date {
    font-size: x-small
}
.btn-group, .btn-group-vertical {
    margin-bottom: 10px;
}
img, svg {
    margin-top: 14px;
}
</style>
