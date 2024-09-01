<template>
 <span>
   <Icon :icon="icon" :width="size" :height="size" />
 </span>
</template>
<script>
import { Icon } from "@iconify/vue";
export default {
 components: {
   Icon,
 },
 props: {
   icon: {
     type: String,
     default: "heroicons-outline:home",
   },
   size: {
     type: Number,
     default: 16,
   },
 },
};
</script>
<style lang=""></style>
