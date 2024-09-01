<template>
    <div class="w-full h-full bg-gray-900 text-gray-200">
        <!-- Sidebar Header -->
        <div class="px-6 pt-8 flex items-center justify-between">
            <a
                href="dashboard"
                class="bg-blue-600 p-1.5 rounded flex items-center justify-center"
            >
                <Icon :icon="homeIcon" class="w-5 h-5 text-white" />
            </a>
            <button class="p-1 rounded bg-gray-800">
                <Icon :icon="chevronLeftIcon" class="w-3 h-3 text-gray-300" />
            </button>
        </div>

        <!-- Search Input -->
        <div class="px-6 pt-4">
            <div class="relative">
                <div class="absolute inset-y-0 left-0 flex items-center pl-2">
                    <Icon
                        :icon="magnifyingGlassIcon"
                        class="w-4 h-4 text-gray-500"
                    />
                </div>
                <input
                    type="text"
                    class="w-full rounded pl-8 pr-4 py-2.5 bg-gray-800 text-gray-400 placeholder-gray-500"
                    placeholder="Search"
                />
            </div>
        </div>

        <!-- Menu Items -->
        <div class="px-6 pt-4">
            <ul class="flex flex-col space-y-2">
                <li v-for="item in menuItems" :key="item.text" class="relative">
                    <template v-if="item.children">
                        <button
                            @click="handleMenuClick(item)"
                            :class="{
                                'bg-gray-800 text-white':
                                    activeMenu === item.text,
                                'text-gray-500': activeMenu !== item.text,
                            }"
                            class="flex items-center w-full py-2 px-4 rounded focus:outline-none"
                        >
                            <Icon :icon="item.icon" class="w-5 h-5 mr-3" />
                            <span>{{ item.text }}</span>
                            <Icon
                                icon="iconoir:nav-arrow-down"
                                :class="{
                                    'rotate-180': activeMenu === item.text,
                                }"
                                class="w-5 h-5 ml-auto transition-transform duration-300"
                            />
                        </button>
                        <ul
                            v-show="activeMenu === item.text"
                            class="pl-4 space-y-2"
                        >
                            <li
                                v-for="subItem in item.children"
                                :key="subItem.text"
                            >
                                <Link
                                    :href="subItem.href"
                                    @click="handleSubItemClick(subItem)"
                                    :class="{
                                        'bg-gray-800 text-white':
                                            activeSubitem === subItem.text,
                                        'text-gray-500':
                                            activeSubitem !== subItem.text,
                                    }"
                                    class="block py-2 px-4 rounded hover:bg-gray-800 flex items-center"
                                >
                                    <Icon
                                        :icon="subItem.icon"
                                        class="w-5 h-5 mr-3"
                                    />
                                    {{ subItem.text }}
                                </Link>
                            </li>
                        </ul>
                    </template>
                    <template v-else>
                        <Link
                            :href="item.href"
                            @click="handleMenuClick(item)"
                            :class="{
                                'bg-gray-800 text-white':
                                    activeMenu === item.text && !activeSubitem,
                                'text-gray-500':
                                    activeMenu !== item.text || activeSubitem,
                            }"
                            class="block py-2 px-4 rounded hover:bg-gray-800 flex items-center"
                        >
                            <Icon :icon="item.icon" class="w-5 h-5 mr-3" />
                            {{ item.text }}
                        </Link>
                    </template>
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";
import { Icon } from "@iconify/vue";
import { Link } from "@inertiajs/vue3"; // Import Inertia Link component

// Define icon names
const homeIcon = "icon-park-outline:home";
const chevronLeftIcon = "icon-park-outline:chevron-left";
const magnifyingGlassIcon = "icon-park-outline:magnifying-glass";

// Define state and methods
const emit = defineEmits();
const activeMenu = ref(null);
const activeSubitem = ref(null);

const menuItems = [
    {
        text: "Dashboard",
        href: "/dashboard",
        icon: "ic:baseline-dashboard",
    },
    {
        text: "Activities",
        icon: "tdesign:activity",
        children: [
            {
                text: "Instructors",
                href: "/instructors",
                icon: "icon-park-outline:user-group",
            },
            {
                text: "Video Library",
                href: "/video-library",
                icon: "icon-park-outline:video-camera",
            },
        ],
    },
    {
        text: "Media",
        href: "/media",
        icon: "dashicons:media-interactive",
    },
    { 
        text: "Songbooks", 
        href: route('songbooks.index'),
        icon: "foundation:book"
    },
    { 
        text: "Management Contributions", 
        href: "/management-contribution",
        icon: "foundation:book"
    },
    {
        text: "Reporting",
        href: "/reporting",
        icon: "icon-park-outline:chart-bar",
    },
    {
        text: "Settings",
        icon: "fluent:settings-28-filled",
        children: [
            { text: "Church", href: "/churchs", icon: "map:church" },
            { text: "Roles", href: "/roles", icon: "carbon:user-role" },
        ],
    },
];

function handleMenuClick(item) {
    activeMenu.value = activeMenu.value === item.text ? null : item.text;
    activeSubitem.value = null;

    if (!item.children) {
        emit("update-breadcrumbs", [
            { text: "Home", href: "/" },
            { text: item.text, href: item.href },
        ]);
    }
}

function handleSubItemClick(subItem) {
    activeSubitem.value = subItem.text;
    activeMenu.value = null;

    emit("update-breadcrumbs", [
        { text: "Home", href: "/" },
        { text: subItem.text, href: subItem.href },
    ]);
}
</script>
