<template>
    <div class="w-full mx-auto p-4">
        <h1 class="text-2xl font-bold mb-6">Create New Songbook</h1>

        <form @submit.prevent="submitForm">
            <div class="grid grid-cols-3 gap-4">
                <!-- Title Field -->
                <TextInput
                    v-model="form.title"
                    id="title"
                    label="Title"
                    required
                />

                <!-- Author Field -->
                <TextInput
                    v-model="form.author"
                    id="author"
                    label="Author"
                    required
                />

                <!-- Key Field -->
                <TextInput v-model="form.key" id="key" label="Key" required />
            </div>

            <!-- Content Field (Quill Editor) -->
            <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700">
                    Content
                </label>
                <QuillEditor
                    v-model:content="form.content"
                    class="mt-1 w-full"
                    :options="editorOptions"
                    style="height: 490px"
                    ref="quillEditor"
                />
            </div>

            <!-- Submit Button -->
            <div class="flex justify-end gap-2">
                <Link
                    type="button"
                    class="px-4 py-2 bg-gray-500 text-white rounded-full hover:bg-blue-600 focus:outline-none"
                    :href="route('songbooks.index')"
                >
                    Cancel
                </Link>

                <button
                    type="submit"
                    class="px-4 py-2 bg-blue-500 text-white rounded-full hover:bg-blue-600 focus:outline-none"
                >
                    Save
                </button>
            </div>
        </form>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { useForm } from "@inertiajs/vue3";
import { Link } from "@inertiajs/vue3";
import { QuillEditor } from "@vueup/vue-quill";
import "quill/dist/quill.snow.css"; // Import the Quill stylesheet
import TextInput from "@/Components/Custom/UI/TextInput/index.vue";

const form = useForm({
    title: "",
    author: "",
    content: "", // Initialize as an empty string
    key: "",
});

const quillEditor = ref(null);

const editorOptions = {
    theme: "snow",
    modules: {
        toolbar: [
            [{ font: [] }],
            [{ size: [] }],
            ["bold", "italic", "underline"],
            [{ color: [] }, { background: [] }],
            [{ list: "ordered" }, { list: "bullet" }],
            ["link"],
            [{ align: [] }],
            ["clean"], // Remove formatting button
        ],
    },
    formats: [
        "font",
        "size",
        "bold",
        "italic",
        "underline",
        "color",
        "background",
        "list",
        "bullet",
        "link",
        "align",
    ],
};

const submitForm = () => {
    if (quillEditor.value && quillEditor.value.quill) {
        form.content = quillEditor.value.quill.root.innerHTML; // Convert to HTML string
    }

    form.post(route('songbooks.store'), {
        onSuccess: () => {
            console.log('Songbook created successfully');
        },
        onError: (errors) => {
            console.error('Error creating songbook', errors);
        }
    });
};
</script>

<style>
/* Add any additional styles if needed */
</style>
