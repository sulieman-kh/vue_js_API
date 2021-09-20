<template>

        <div v-if="character" 
            id="exampleModal"
            tabindex="-1"
            aria-labelledby="exampleModalLabel"
            class="modal fade"
            style="display: none;" aria-hidden="true"
        >
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title">{{ character.name }}</h5>
                        <button type="button"
                                data-bs-dismiss="modal"
                                aria-label="Close"
                                class="btn-close"
                        ></button>
                    </div>
                    <div class="modal-body">
                        <img :src="character.thumbnail"
                            :alt="character.name"
                            style="max-width: 100%;"
                        >
                        <div>
                            <p class="text-muted">Date First Available: 
                                {{character.modified}}</p>
                                {{character.price}}
                                <h5>About this item</h5>
                        <p> {{ character.description }}</p>
                        <h5>More Details:</h5>
                            <ul>
                            <li v-for = "el in character.comics" 
                            :key="el.id"> {{ el.name }}</li>
                        </ul>
                        </div>

                        <div class="modal-footer">
                            <button type="button"
                                    data-bs-dismiss="modal"
                                    class="btn btn-secondary  btn-sm"
                            >
                                Close
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    
</template>

<script>
    export default {
        name: "AppModal",
        props: ['character'],
    }
    
</script>

<style scoped>

</style>
