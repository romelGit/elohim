<template>
    <AppLayout>
        <Card>
            <Table
                :title="'Songbooks'"
                :headers="tableHeaders"
                :items="tableItems"
                :itemsPerPage="10"
                :searchableColumns="['title', 'author', 'key']"
                search
            >
                <template #header_add>
                    <Link :href="route('songbooks.create')" class="px-4 py-2 bg-blue-500 text-white rounded-full hover:bg-blue-600 focus:outline-none">
                        Create New Songs
                    </Link>
                </template>
                <template #action="{ item }">
                    <div class="flex gap-2">
                        <button
                            @click="viewItem(item)"
                            class="p-2 bg-gray-700 rounded-full hover:bg-gray-800 focus:outline-none transition ease-in-out duration-150"
                            aria-label="View"
                            title="View"
                        >
                            <Icon icon="mdi:eye" class="text-white text-lg" />
                        </button>
                        <button
                            @click="printItem(item)"
                            class="p-2 bg-green-600 rounded-full hover:bg-green-700 focus:outline-none transition ease-in-out duration-150"
                            aria-label="Print"
                            title="Print"
                        >
                            <Icon icon="mdi:printer" class="text-white text-lg" />
                        </button>
                        <button
                            @click="editItem(item)"
                            class="p-2 bg-blue-600 rounded-full hover:bg-blue-700 focus:outline-none transition ease-in-out duration-150"
                            aria-label="Edit"
                            title="Edit"
                        >
                            <Icon icon="mdi:edit" class="text-white text-lg" />
                        </button>
                        <button
                            @click="deleteItem(item)"
                            class="p-2 bg-red-600 rounded-full hover:bg-red-700 focus:outline-none transition ease-in-out duration-150"
                            aria-label="Delete"
                            title="Delete"
                        >
                            <Icon icon="mdi:delete" class="text-white text-lg" />
                        </button>
                    </div>
                </template>
            </Table>
        </Card>
    </AppLayout>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { Link } from '@inertiajs/vue3';
import AppLayout from '@/Layouts/AppLayout.vue';
import Card from '@/Components/UI/Card/index.vue';
import Table from '@/Components/Custom/Table/index.vue';
import { usePage, useForm } from '@inertiajs/vue3';
import { Icon } from '@iconify/vue'; // Import the Icon component

const tableHeaders = ref([
    { text: 'Title', value: 'title', align: 'left' },
    { text: 'Author', value: 'author', align: 'left' },
    { text: 'Key', value: 'key', align: 'left' },
    { text: 'Action', value: 'action', align: 'left' }
]);

const tableItems = ref([]);
const { props } = usePage();

const form = useForm({
    search: '',
});

onMounted(() => {
    tableItems.value = props.songbooks;
});

const editItem = (item) => {
    console.log('Editing', item);
};

const deleteItem = (item) => {
    console.log('Deleting', item);
};
</script>
