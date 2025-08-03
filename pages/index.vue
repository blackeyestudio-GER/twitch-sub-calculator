<template>
    <div class="bg-night min-h-screen text-white px-4 py-6 max-w-3xl mx-auto">
        <div class="bg-eerie-black p-6 rounded-lg shadow-lg">
            <ThankYou class="hidden md:grid md:mb-6" />
            <!-- Trigger Button for Mobile -->
            <button
                @click="showModal = true"
                class="w-16 h-16 rounded-full overflow-hidden shadow-lg border-2 border-white md:hidden flex m-auto mb-6"
            >
                <img src="/logo.png" alt="Info" class="w-full h-full object-cover" />
            </button>
            <!-- Modal -->
            <div
                v-if="showModal"
                class="fixed inset-0 bg-black/70 z-50 flex items-center justify-center p-4 md:hidden"
            >
                <div class="bg-eerie-black text-white rounded-lg max-w-md w-full p-6 relative overflow-y-auto max-h-[90vh]">
                    <button
                            @click="showModal = false"
                            class="absolute top-3 right-3 w-8 h-8 flex items-center justify-center rounded-full bg-gray-600 hover:bg-gray-500 text-white text-lg font-bold"
                    >
                        ✕
                    </button>
                    <div class="text-sm text-gray-300 space-y-4 leading-relaxed">
                        <ThankYou />
                    </div>
                </div>
            </div>

            <div class="bg-twitch-purple p-4 text-white rounded">
            <!-- Sub Inputs -->
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
                    <div>
                        <label class="block text-sm mb-1">Prime Subs ({{ prices.prime }} €</label>
                        <input v-model.number="subs.prime" type="number" min="0" class="w-full p-2 rounded text-black" />
                    </div>
                    <div>
                        <label class="block text-sm mb-1">Tier 1 Subs {{ prices.tier1 }} €</label>
                        <input v-model.number="subs.tier1" type="number" min="0" class="w-full p-2 rounded text-black" />
                    </div>
                    <div>
                        <label class="block text-sm mb-1">Tier 2 Subs {{ prices.tier2 }} €</label>
                        <input v-model.number="subs.tier2" type="number" min="0" class="w-full p-2 rounded text-black" />
                    </div>
                    <div>
                        <label class="block text-sm mb-1">Tier 3 Subs {{ prices.tier3 }} €</label>
                        <input v-model.number="subs.tier3" type="number" min="0" class="w-full p-2 rounded text-black" />
                    </div>
                    <div class="md:col-start-3">
                        <label class="block text-sm mb-1">Bits (100 Bits = {{ prices.bits }} €)</label>
                        <input v-model.number="bits" type="number" min="0" class="w-full p-2 rounded text-black" />
                    </div>
                </div>
            </div>

            <!-- Results -->
            <div class="bg-eerie-black p-4 rounded-lg">
                <h2 class="text-xl font-semibold mb-4">Ergebnisse</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4 text-sm">
                    <div class="text-green-400 outline outline-2 outline-green-400 p-2 rounded">
                        <h3 class="font-semibold mb-1">Zahlung durch den Nutzer</h3>
                        <p>{{ totals.user.toFixed(2) }} €</p>
                    </div>
                    <div class="text-twitch-purple outline outline-2 outline-twitch-purple p-2 rounded">
                        <h3 class="font-semibold mb-1">Anteil Twitch</h3>
                        <p>{{ totals.twitch.toFixed(2) }} €</p>
                    </div>
                    <div class="text-red-400 outline outline-2 outline-red-400 p-2 rounded">
                        <h3 class="font-semibold mb-1">Anteil Streamer</h3>
                        <p>{{ totals.streamer.toFixed(2) }} €</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- Bottom Ad Banner -->
        <div class="mt-8">
            <AdBannerBottom />
        </div>
    </div>
</template>

<script setup>
import { reactive, computed } from 'vue';
const showModal = ref(false);

const subs = reactive({
    prime: 0,
    tier1: 0,
    tier2: 0,
    tier3: 0,
});

const bits = ref(0);

const prices = {
    prime: 4.99,
    tier1: 4.99,
    tier2: 7.99,
    tier3: 19.99,
    bits: 1.59,
};

const revenueSplitTwitch = {
    prime: 0.50,
    tier1: 0.50,
    tier2: 0.50,
    tier3: 0.50,
    bits: 0.20,
};
const revenueSplitStreamer = {
    prime: 0.50,
    tier1: 0.50,
    tier2: 0.50,
    tier3: 0.50,
    bits: 0.80,
};

const totals = computed(() => {
    const userTotal =
            subs.prime * prices.prime +
            subs.tier1 * prices.tier1 +
            subs.tier2 * prices.tier2 +
            subs.tier3 * prices.tier3 +
            bits.value / 100 * prices.bits;

    const streamerShare =
            subs.prime * prices.prime * revenueSplitStreamer.prime +
            subs.tier1 * prices.tier1 * revenueSplitStreamer.tier1 +
            subs.tier2 * prices.tier2 * revenueSplitStreamer.tier2 +
            subs.tier3 * prices.tier3 * revenueSplitStreamer.tier3 +
            bits.value / 100 * prices.bits * revenueSplitStreamer.bits;

    const twitchShare =
            subs.prime * prices.prime * revenueSplitTwitch.prime +
            subs.tier1 * prices.tier1 * revenueSplitTwitch.tier1 +
            subs.tier2 * prices.tier2 * revenueSplitTwitch.tier2 +
            subs.tier3 * prices.tier3 * revenueSplitTwitch.tier3 +
            bits.value / 100 * prices.bits * revenueSplitTwitch.bits;

    return {
        user: userTotal,
        twitch: twitchShare,
        streamer: streamerShare,
    };
});
</script>