<template>
    <div class="container mx-auto">
        <div class="py-14">
            <AppModal
                v-if="showMovieForm"
                :title="currentMovie?.id ? 'Edit Movie' : 'Add Movie'"
                @close="hideForm()"
            >
                <MovieForm
                    @update:modelValue="saveMovie"
                    :modelValue="currentMovie"
                    @cancel="hideForm"
                />
            </AppModal>
            <div class="flex items-center pb-8">
                <div class="flex items-center justify-center space-x-8 text-white text-xl">
                    <span>Total Movies: {{ totalMovies }}</span>
                    <span> / </span>
                    <span>Average Rating: {{ averageRating }}</span>
                </div>
                <button
                    class="text-sm self-end justify-self-end bg-cyan-500 hover:bg-cyan-400 p-2 rounded-md text-white"
                    @click="removeRatings"
                >
                    Remove Ratings
                </button>
                <button
                    class="p-2 rounded-md text-sm self-end justify-self-end"
                    :class="{
                        'bg-cyan-500 hover:bg-cyan-400 p-2 rounded-md text-white': !showMovieForm,
                        'bg-gray-600 text-gray-400 cursor-not-allowed': showMovieForm,
                    }"
                    @click="showForm"
                    :disabled="showMovieForm"
                >
                    Add Movie
                </button>
            </div>
            <div class="flex flex-wrap items-start justify-between">
                <MovieItem
                    v-for="movie in movies"
                    :key="movie.id"
                    :movie="movie"
                    @edit="editMovie"
                    @remove="removeMovie"
                    @update:rating="updateRating"
                />
            </div>
        </div>
    </div>
</template>

<script setup>
import { computed, defineAsyncComponent, ref } from "vue";
import MovieItem from "@/MovieItem.vue";
import { items } from "./movies.json";

const AppModal = defineAsyncComponent(() => import("@/AppModal.vue"));
const MovieForm = defineAsyncComponent(() => import("@/MovieForm.vue"));

const movies = ref(items);
const currentMovie = ref();

function updateRating(id, rating) {
    movies.value = movies.value.map((movie) => {
        if (movie.id === id) {
            movie.rating = rating;
        }
        return movie;
    });
}

function removeMovie(id) {
    movies.value = movies.value.filter((movie) => movie.id !== id);
}

function editMovie(id) {
    currentMovie.value = movies.value.find((movie) => movie.id === id);
    showForm();
}

function saveMovie(data) {
    const isNew = !!movies.value.find((movie) => movie.id === data.id);
    if (!isNew) {
        addMovie(data);
    } else {
        updateMovie(data);
    }
}

function updateMovie(data) {
    movies.value = movies.value.map((m) => {
        if (m.id === data.id) {
            data.rating = m.rating;
            return data;
        }
        return m;
    });
    hideForm();
}

function addMovie(data) {
    movies.value.push(data);
    hideForm();
}

// Modal
const showMovieForm = ref(false);

function hideForm() {
    showMovieForm.value = false;
    currentMovie.value = null;
}

function showForm() {
    showMovieForm.value = true;
}

// Rating
const averageRating = computed(() => {
    const avg = movies.value.map((movie) => parseInt(movie.rating || 0)).reduce((a, b) => a + b, 0);

    return Number(avg / movies.value.length).toFixed(1);
});

const totalMovies = computed(() => {
    return movies.value.length;
});

function removeRatings() {
    movies.value = movies.value.map((movie) => {
        movie.rating = null;
        return movie;
    });
}
</script>

