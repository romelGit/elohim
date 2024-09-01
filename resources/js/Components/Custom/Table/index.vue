<template>
    <div>
        <div class="flex items-center justify-between mb-4">
            <h2 v-if="title" class="text-xl font-semibold">{{ title }}</h2>
            <div class="flex items-center space-x-4">
                <input
                    v-if="search"
                    type="text"
                    v-model="searchQuery"
                    placeholder="Search..."
                    class="px-4 py-2 border border-gray-300 rounded-full w-96"
                />
                <slot name="header_add"></slot>
            </div>
        </div>

        <table class="min-w-full bg-white border border-gray-300">
            <thead>
                <tr>
                    <th
                        v-for="header in headers"
                        :key="header.value"
                        :class="getHeaderClass(header.align)"
                        class="px-4 py-2 border-b"
                        :style="{ width: header.width }"
                    >
                        {{ header.text }}
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr
                    v-for="item in paginatedItems"
                    :key="item.id"
                    class="hover:bg-gray-100"
                >
                    <td
                        v-for="header in headers"
                        :key="header.value"
                        :class="[getItemClass(header.align)]"
                        class="px-4 py-2 border-b"
                        :style="{ width: header.width }"
                    >
                        <slot :name="header.value" :item="item">
                            {{ item[header.value] }}
                        </slot>
                    </td>
                </tr>
            </tbody>
        </table>

        <div
            v-if="totalPages > 1"
            class="flex justify-end items-center mt-4 space-x-2"
        >
            <button
                @click="prevPage"
                :disabled="currentPage === 1"
                class="px-4 py-2 bg-blue-500 text-white rounded-full hover:bg-blue-600 disabled:bg-gray-300 disabled:cursor-not-allowed focus:outline-none"
            >
                Previous
            </button>
            <span>Page {{ currentPage }} of {{ totalPages }}</span>
            <button
                @click="nextPage"
                :disabled="currentPage === totalPages"
                class="px-4 py-2 bg-blue-500 text-white rounded-full hover:bg-blue-600 disabled:bg-gray-300 disabled:cursor-not-allowed focus:outline-none"
            >
                Next
            </button>
        </div>
    </div>
</template>

<script setup>
    import { ref, computed } from 'vue';

    const props = defineProps({
        headers: Array,
        items: Array,
        search: Boolean,
        title: String,
        itemsPerPage: Number,
        searchableColumns: Array,
    });

    const searchQuery = ref("");
    const currentPage = ref(1);

    const filteredItems = computed(() => {
        if (!searchQuery.value) return props.items;
        
        const query = searchQuery.value.toLowerCase();

        return props.items.filter(item => {
            return props.searchableColumns.some(column => {
                const value = item[column]?.toString().toLowerCase() || '';
                return value.includes(query);
            });
        });
    });

    const totalPages = computed(() => {
        return Math.ceil(filteredItems.value.length / props.itemsPerPage);
    });

    const paginatedItems = computed(() => {
        const start = (currentPage.value - 1) * props.itemsPerPage;
        const end = start + props.itemsPerPage;
        return filteredItems.value.slice(start, end);
    });

    const prevPage = () => {
        if (currentPage.value > 1) {
            currentPage.value--;
        }
    };

    const nextPage = () => {
        if (currentPage.value < totalPages.value) {
            currentPage.value++;
        }
    };

    const getHeaderClass = (align) => {
        return align === "center" ? "text-center" : align === "right" ? "text-right" : "text-left";
    };

    const getItemClass = (align) => {
        return align === "center" ? "text-center" : align === "right" ? "text-right" : "text-left";
    };
</script>
