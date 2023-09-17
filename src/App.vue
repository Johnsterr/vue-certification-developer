<template>
    <div class="container mx-auto">
        <!-- <div class="max-w-full flex flex-col items-center justify-center mx-auto"></div> -->
        <div class="py-14">
            <div
                v-if="showMovieForm"
                class="absolute inset-0 flex items-center justify-center z-20 bg-gray-800/40 backdrop-blur"
            >
                <div
                    class="shrink-0 w-full max-w-2xl rounded-md flex flex-col overflow-hidden shadow-2xl bg-white dark:bg-gray-800 p-4 space-y-4 dark:text-white"
                >
                    <form @submit.prevent="saveMovie">
                        <div class="flex flex-col items-start justify-start w-full">
                            <label for="name">Name</label>
                            <input
                                type="text"
                                name="name"
                                id="name"
                                v-model="form.name"
                                class="w-full dark:bg-gray-900 rounded-md border-2 border-blue-600"
                            />
                            <span class="text-red-700 h-4">{{ errors.name }}</span>
                        </div>
                        <div class="flex flex-col items-start justify-start w-full">
                            <label for="description">Description</label>
                            <textarea
                                type="text"
                                name="description"
                                id="description"
                                v-model="form.description"
                                class="w-full dark:bg-gray-900 h-24 rounded-md border-2 border-blue-600"
                            />
                            <span class="text-red-700 h-4">{{ errors.description }}</span>
                        </div>
                        <div class="flex flex-col items-start justify-start w-full">
                            <label for="image">Image</label>
                            <input
                                type="text"
                                name="image"
                                id="image"
                                v-model="form.image"
                                class="w-full dark:bg-gray-900 rounded-md border-2 border-blue-600"
                            />
                            <span class="text-red-700 h-4">{{ errors.image }}</span>
                        </div>
                        <div class="flex flex-col items-start justify-start w-full">
                            <label for="genre">Genres</label>
                            <select
                                name="genre"
                                id="genre"
                                v-model="form.genres"
                                class="w-full dark:bg-gray-900 rounded-md border-2 border-blue-600"
                                multiple
                            >
                                <option
                                    v-for="option in genres"
                                    :key="option.value"
                                    :value="option.value"
                                >
                                    {{ option.text }}
                                </option>
                            </select>
                            <span class="text-red-700 h-4">
                                {{ errors.genres }}
                            </span>
                        </div>
                        <div class="flex flex-col items-start justify-start w-full">
                            <label
                                for="inTheaters"
                                class="flex items-center justify-start space-x-2"
                            >
                                <input
                                    type="checkbox"
                                    id="inTheaters"
                                    v-model="form.inTheaters"
                                    :true-value="true"
                                    :false-value="false"
                                    class="rounded-md focus:bg-pink-500/25 checked:bg-pink-500 dark:bg-gray-900"
                                />
                                <span>In theaters</span>
                            </label>
                            <span class="text-red-700 h-4">
                                {{ errors.inTheaters }}
                            </span>
                        </div>
                        <div class="flex items-center justify-between w-full">
                            <button
                                type="button"
                                class="bg-gray-500 hover:bg-gray-400 p-2 rounded-md text-white"
                                @click="hideForm"
                            >
                                Cancel
                            </button>
                            <button
                                type="submit"
                                class="bg-cyan-500 hover:bg-cyan-400 p-2 rounded-md text-white"
                            >
                                <span v-if="form.id">Update</span>
                                <span v-else>Create</span>
                            </button>
                        </div>
                    </form>
                </div>
            </div>
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
import { computed, reactive, ref } from "vue";
import MovieItem from "@/MovieItem.vue";
import { items } from "./movies.json";

const movies = ref(items);
const showMovieForm = ref(false);

const errors = reactive({
    name: null,
    description: null,
    image: null,
    inTheaters: null,
    genres: null,
});

const form = reactive({
    id: null,
    name: null,
    description: null,
    image: null,
    inTheaters: false,
    genres: null,
});

const validations = reactive({
    name: "required",
    genres: "required",
});

const genres = reactive([
    { text: "Drama", value: "Drama" },
    { text: "Crime", value: "Crime" },
    { text: "Action", value: "Action" },
    { text: "Comedy", value: "Comedy" },
]);

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
    const movie = movies.value.find((movie) => movie.id === id);

    form.id = movie.id;
    form.name = movie.name;
    form.description = movie.description;
    form.image = movie.image;
    form.inTheaters = movie.inTheaters;
    form.genres = movie.genres;

    showForm();
}

const validationRules = (rule) => {
    if (rule === "required") return /^ *$/;

    return null;
};

function validate() {
    let valid = true;
    clearErrors();
    for (const [field, rule] of Object.entries(validations)) {
        const validation = validationRules(rule);
        if (validation) {
            if (validation.test(form[field] || "") || form[field].length === 0) {
                errors[field] = `${field} is ${rule}`;
                valid = false;
            }
        }
    }

    return valid;
}

function addMovie() {
    if (validate()) {
        const movie = {
            id: Number(Date.now()),
            name: form.name,
            description: form.description,
            image: form.image,
            genres: form.genres,
            inTheaters: form.inTheaters,
            rating: null,
        };
        movies.value.push(movie);
        hideForm();
    }
}

function saveMovie() {
    if (form.id) {
        updateMovie();
    } else {
        addMovie();
    }
}

function updateMovie() {
    if (validate()) {
        const movie = {
            id: form.id,
            name: form.name,
            description: form.description,
            image: form.image,
            genres: form.genres,
            inTheaters: form.inTheaters,
            rating: 0,
        };

        movies.value = movies.value.map((m) => {
            if (m.id === movie.id) {
                movie.rating = m.rating;
                return movie;
            }
            return m;
        });

        hideForm();
    }
}

function cleanUpForm() {
    form.name = null;
    form.description = null;
    form.image = null;
    form.genres = null;
    form.inTheaters = false;
    clearErrors();
}

function clearErrors() {
    errors.name = null;
    errors.description = null;
    errors.image = null;
    errors.genres = null;
    errors.inTheaters = null;
}

function hideForm() {
    showMovieForm.value = false;
    cleanUpForm();
}

function showForm() {
    showMovieForm.value = true;
}

const averageRating = computed(() => {
    const avg = movies.value.map((movie) => parseInt(movie.rating || 0)).reduce((a, b) => a + b, 0);

    return Number(avg / movies.value.length).toFixed(1);
});

const totalMovies = computed(() => {
    return movies.value.length;
});

function removeRatings() {
    movies.value = movies.value.map((movie) => {
        movie.rating = 0;
        return movie;
    });
}
</script>

