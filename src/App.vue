<template>
    <!-- This is where your template goes	-->
    <div class="max-w-full flex items-center justify-center h-screen mx-auto">
        <div class="flex items-center justify-center space-x-4">
            <div
                class="w-96 h-auto bg-white rounded-md flex flex-col items-center justify-start overflow-hidden shadow-2xl"
                v-for="movie in movies"
                :key="movie.id"
            >
                <div class="h-[520px] overflow-hidden w-full relative">
                    <img
                        class="object-cover object-center h-[600px]"
                        :src="movie.image"
                        :alt="movie.name"
                    />
                </div>

                <div class="h-56 p-4 flex flex-col items-start justify-start w-full">
                    <div class="h-16 shrink-0 w-full">
                        <h3 class="text-2xl">{{ movie.name }}</h3>
                        <div class="flex items-center justify-start space-x-1">
                            <span
                                v-for="genre in movie.genres"
                                :key="`${movie.id}-${genre}`"
                                class="text-xs bg-indigo-500 text-white py-0.5 px-2 rounded-full"
                            >
                                {{ genre }}
                            </span>
                        </div>
                    </div>
                    <div class="h-24 overflow-auto flex-1">
                        <p class="text-sm">{{ movie.description }}</p>
                    </div>
                    <div class="w-full flex items-center justify-start h-8 shrink-0">
                        <span class="text-xs mr-2 leading-7"> Rating: ({{ movie.rating }}/5) </span>
                        <StarIcon
                            v-for="star in movie.rating"
                            :key="`star-${star}`"
                            class="h-5 w-5 text-yellow-500"
                        />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { reactive } from "vue";
import { items } from "./movies.json";
import { StarIcon } from "@heroicons/vue/24/solid";
/*
 This is an Icon that you can use to represent the stars if you like
 otherwise you could just use a simple ⭐️ emoji, or * character.
*/
const movies = reactive(items);
</script>

