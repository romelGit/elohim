<template>
    <div class="layout-container">
        <aside class="sidebar">
            <SidebarComponent />
        </aside>

        <div class="main-content">
            <HeaderComponent />

            <main role="main" class="layout-main">
                <slot />
            </main>

            <FooterComponent />
        </div>
    </div>
</template>

<script>
    import HeaderComponent from "@/Components/Header/index.vue";
    import FooterComponent from "@/Components/Footer/index.vue";
    import SidebarComponent from "@/Components/Sidebar/index.vue";

    export default {
        name: "LayoutComponent",
        components: {
            HeaderComponent,
            FooterComponent,
            SidebarComponent,
        },
    };
</script>

<style scoped>
    /* Scoped styles in LayoutComponent.vue */
    .layout-container {
        display: flex;
        height: 100vh;
        overflow: hidden; /* Prevent any scrolling on the outer container */
    }

    .sidebar {
        width: 300px; /* Width of the sidebar */
        background-color: #f4f4f4; /* Example background color for sidebar */
        position: fixed; /* Fixed positioning for the sidebar */
        height: 100%; /* Full height for the sidebar */
        top: 0; /* Align to the top */
        left: 0; /* Align to the left */
    }

    .main-content {
        margin-left: 300px; /* Offset by the sidebar's width */
        display: flex;
        flex-direction: column;
        flex: 1; /* Grow to fill the remaining space */
    }

    .layout-main {
        flex: 1; /* Grow to fill available space */
        overflow-y: auto; /* Allow scrolling if content overflows */
        padding: 20px;
    }

    .layout-header,
    .layout-footer {
        width: 100%; /* Ensure full width */
    }
</style>
