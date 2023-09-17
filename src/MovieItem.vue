<template>
    <div
        class="w-96 shrink-0 h-auto bg-white rounded-md flex flex-col items-center justify-start overflow-hidden shadow-2xl mr-8 mt-8 group"
    >
        <div class="h-[520px] overflow-hidden w-full relative">
            <div class="absolute top-0 right-0">
                <div class="relative">
                    <StarIcon
                        class="h-16 w-16"
                        :class="{
                            'text-yellow-500': !notRated,
                            'text-gray-500': notRated,
                        }"
                    />
                    <span
                        v-if="!notRated"
                        class="absolute block h-5 top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 text-yellow-800"
                    >
                        {{ movie.rating }}
                    </span>
                    <span
                        v-else
                        class="absolute block h-5 top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 text-gray-800"
                    >
                        -
                    </span>
                </div>
            </div>
            <img
                v-if="movie?.image"
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
                <div class="items-center justify-start flex flex-1 space-x-1">
                    <button
                        v-for="star in 5"
                        :key="star"
                        class="rounded-md disabled:cursor-not-allowed"
                        :class="[star <= movie.rating ? 'text-yellow-500' : 'text-gray-500']"
                        :disabled="star === movie.rating"
                        @click="updateRating(star)"
                    >
                        <StarIcon class="h-5 w-5" />
                    </button>
                </div>
                <div class="group-hover:flex shrink-0 items-center justify-end space-x-2">
                    <button class="movie-item-action-edit-button" @click="editMovie()">
                        <PencilIcon class="w-4 h-4" />
                    </button>
                    <button class="movie-item-action-remove-button" @click="removeMovie()">
                        <TrashIcon class="w-4 h-4" />
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { computed } from "vue";
import { PencilIcon, StarIcon, TrashIcon } from "@heroicons/vue/24/solid";

const props = defineProps({
    movie: { type: Object, default: null },
});

const emit = defineEmits(["edit", "remove", "update:rating"]);

const notRated = computed(() => {
    return Boolean(!props.movie?.rating);
});

function updateRating(rating) {
    emit("update:rating", props.movie.id, rating);
}

function removeMovie() {
    emit("remove", props.movie.id);
}

function editMovie() {
    emit("edit", props.movie.id);
}
</script>
