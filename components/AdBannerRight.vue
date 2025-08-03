<template>
    <div class="w-80 hidden md:block">
        <ins class="adsbygoogle"
             style="display:block; min-height: 250px"
             data-ad-client="ca-pub-XXXXXXXXXXXXXX"
             data-ad-slot="YOUR_SLOT_ID_RIGHT"
             data-ad-format="auto"
             data-full-width-responsive="true">
        </ins>
    </div>
</template>

<script setup>
onMounted(() => {
    if (window.adsbygoogle) {
        try {
            (adsbygoogle = window.adsbygoogle || []).push({});
        } catch (e) {
            console.warn('AdSense failed to load', e);
        }
    }
});
</script>