<script setup>
import { reactive } from "vue";

const props = defineProps({
    modelValue: { type: Object, default: null },
});
const emit = defineEmits(["update:modelValue", "cancel"]);

const genres = reactive([
    { text: "Drama", value: "Drama" },
    { text: "Crime", value: "Crime" },
    { text: "Action", value: "Action" },
    { text: "Comedy", value: "Comedy" },
]);

const errors = reactive({
    name: null,
    description: null,
    image: null,
    inTheaters: null,
    genres: null,
    rating: null,
});

const form = reactive({
    id: props.modelValue?.id,
    name: props.modelValue?.name,
    description: props.modelValue?.description,
    image: props.modelValue?.image,
    inTheaters: props.modelValue?.inTheaters || false,
    genres: props.modelValue?.genres || [],
    rating: props.modelValue?.rating || null,
});

const validations = reactive({
    name: "required",
    genres: "required",
});

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
function cancelForm() {
    cleanUpForm();
    emit("cancel");
}

function saveMovie() {
    if (validate()) {
        const data = {
            id: form.id || Number(Date.now()),
            name: form.name,
            description: form.description,
            image: form.image,
            genres: form.genres,
            inTheaters: form.inTheaters,
            rating: form.rating,
        };
        emit("update:modelValue", data);
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
</script>

<template>
    <form @submit.prevent="saveMovie">
        <input type="hidden" name="id" v-model="form.id" />
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
                <option v-for="option in genres" :key="option.value" :value="option.value">
                    {{ option.text }}
                </option>
            </select>
            <span class="text-red-700 h-4">
                {{ errors.genres }}
            </span>
        </div>
        <div class="flex flex-col items-start justify-start w-full">
            <label for="inTheaters" class="flex items-center justify-start space-x-2">
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
                @click="cancelForm"
            >
                Cancel
            </button>
            <button type="submit" class="bg-cyan-500 hover:bg-cyan-400 p-2 rounded-md text-white">
                <span v-if="form.id">Save</span>
                <span v-else>Create</span>
            </button>
        </div>
    </form>
</template>
