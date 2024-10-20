<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref } from 'vue';
import { useSlideContext } from '@slidev/client'

function genBgm() {
    // http://soundfile.sapp.org/doc/WaveFormat/
    const SAMPLE_RATE = 48000
    const NUM_SAMPLES = SAMPLE_RATE * 20   // 20s

    const NUM_CHANNELS = 1
    const BITS_PER_SAMPLE = 16

    const SUB_CHUNK2_SIZE = NUM_SAMPLES * NUM_CHANNELS * BITS_PER_SAMPLE / 8
    const BYTE_RATE = SAMPLE_RATE * NUM_CHANNELS * BITS_PER_SAMPLE / 8
    const BLOCK_ALIGN = NUM_CHANNELS * BITS_PER_SAMPLE / 8

    const bytes = new Uint8Array(44 + SUB_CHUNK2_SIZE)
    const view = new DataView(bytes.buffer)

    view.setUint32(0, 0x52494646)        // "RIFF"
    view.setUint32(4, 36 + SUB_CHUNK2_SIZE, true)
    view.setUint32(8, 0x57415645)        // "WAVE"
    view.setUint32(12, 0x666D7420)        // "fmt "
    view.setUint32(16, 16, true)          // Subchunk1Size (PCM = 16)
    view.setUint16(20, 1, true)           // AudioFormat (PCM = 1)
    view.setUint16(22, NUM_CHANNELS, true)
    view.setUint32(24, SAMPLE_RATE, true)
    view.setUint32(28, BYTE_RATE, true)
    view.setUint16(32, BLOCK_ALIGN, true)
    view.setUint16(34, BITS_PER_SAMPLE, true)
    view.setUint32(36, 0x64617461)        // "data"
    view.setUint32(40, SUB_CHUNK2_SIZE, true)

    const soundData = new Uint16Array(bytes.buffer, 44)
    soundData.fill(10)

    const blob = new Blob([bytes], {
        type: 'audio/wav',
    })
    return URL.createObjectURL(blob)
}
const { $slidev } = useSlideContext();

function nextSlide() {
    console.log("nextSlide");
    $slidev.nav.next();
}

function prevSlide() {
    console.log("prevSlide");
    $slidev.nav.prev();
}

let audio: HTMLAudioElement;
const isPlaying = ref(false);

function initSlidPods() {
    if ("mediaSession" in navigator) {
        console.log("init slidepods");
        audio = new Audio(genBgm());
        audio.loop = true;
        audio.autoplay = true;
        audio.onended = () => {
            audio.play();
        };
        audio.controls = false;
        audio.style.display = 'none';
        document.body.appendChild(audio);
        navigator.mediaSession.metadata = new MediaMetadata({
            title: 'Slidev',
            artist: 'miku',
        });
        // navigator.mediaSession.setActionHandler('previoustrack', () => {
        //     console.log("previoustrack");
        // })
        navigator.mediaSession.setActionHandler('pause', nextSlide);
        navigator.mediaSession.setActionHandler('play', nextSlide);
        navigator.mediaSession.setActionHandler('nexttrack', prevSlide);
    }
}

function playAudio() {
    audio.play();
    isPlaying.value = true;
}

function destroySlidPods() {
    if (audio) {
        audio.remove();
    }
    navigator.mediaSession.setActionHandler('pause', null);
    navigator.mediaSession.setActionHandler('play', null);
    navigator.mediaSession.setActionHandler('nexttrack', null);
}

onMounted(() => {
    initSlidPods();
})

onBeforeUnmount(() => {
    destroySlidPods();
})
</script>

<template>
    <footer class="z-[9999] relative" v-if="!isPlaying">
        <div class="inline-flex items-center gap-2 shadow m-3 text-sm opacity-80 hover:opacity-100 cursor-pointer" @click="playAudio">
            <svg class="size-8 fill-current" xmlns="http://www.w3.org/2000/svg" enable-background="new 0 0 100 100" viewBox="0 0 100 100" id="airpods">
                <path d="M50 38.93c-.5 0-.96.19-1.31.55-.72.72-.73 1.9 0 2.63.36.36.84.54 1.31.54s.95-.18 1.32-.55c.72-.73.72-1.91 0-2.63C50.96 39.13 50.5 38.93 50 38.93 50 38.93 50 38.93 50 38.93zM50.81 41.6c-.21.21-.51.33-.81.34 0 0 0 0 0 0-.3 0-.59-.12-.8-.33-.44-.44-.44-1.16 0-1.61.22-.22.51-.33.8-.33.29 0 .58.11.81.33C51.25 40.43 51.25 41.16 50.81 41.6zM57.56 34.73c.38 0 .77-.15 1.06-.44.59-.59.59-1.54 0-2.12-4.75-4.75-12.49-4.75-17.24 0-.59.59-.59 1.54 0 2.12s1.54.59 2.12 0c3.58-3.58 9.42-3.58 13 0C56.79 34.59 57.18 34.73 57.56 34.73z"></path>
                <path d="M64.38 27.91c.38 0 .77-.15 1.06-.44.59-.59.59-1.54 0-2.12-8.51-8.52-22.37-8.52-30.88 0-.59.59-.59 1.54 0 2.12.59.59 1.54.59 2.12 0 3.56-3.56 8.29-5.52 13.32-5.52s9.76 1.96 13.32 5.52C63.61 27.77 64 27.91 64.38 27.91zM79.9 40.34h-1.07c-1.68 0-3.2.83-4.1 2.17h-9.25c-2.92 0-5.67 1.13-7.73 3.2-2.05 2.08-3.19 4.82-3.19 7.72 0 2.48.86 4.87 2.41 6.82v13.74c0 3.89 3.18 7.06 7.08 7.06.03 0 .06 0 .09 0 .01 0 .03 0 .04 0 .04 0 .09-.01.13-.01 3.77-.14 6.8-3.24 6.8-7.05v-9.64h3.62c.91 1.34 2.42 2.17 4.1 2.17h1.07c4.01 0 7.28-3.26 7.28-7.28V47.62C87.18 43.6 83.91 40.34 79.9 40.34zM59.88 47.82c1.49-1.49 3.48-2.31 5.6-2.31h8.37v15.84h-4.24-4.13c-.28 0-.56-.02-.84-.04-.3-.03-.6-.09-.9-.15-1.59-.36-3.05-1.2-4.16-2.44-1.31-1.47-2.03-3.34-2.03-5.28C57.56 51.32 58.39 49.33 59.88 47.82zM68.11 73.98c0 1.66-1 3.08-2.43 3.71V70.8c0-.83-.67-1.5-1.5-1.5s-1.5.67-1.5 1.5v6.99c-1.57-.56-2.71-2.05-2.71-3.81V62.85c.01 0 .01.01.02.01.38.22.77.41 1.17.59.12.05.24.09.36.14.29.11.59.22.89.31.14.04.29.08.44.12.3.07.6.13.91.18.14.02.27.05.41.07.44.05.88.09 1.32.09h2.63V73.98zM84.18 59.24c0 2.36-1.92 4.28-4.28 4.28h-1.07c-.81 0-1.53-.48-1.84-1.24-.09-.22-.14-.48-.14-.74V45.31c0-.25.05-.51.15-.75.3-.75 1.02-1.23 1.83-1.23h1.07c2.36 0 4.28 1.92 4.28 4.28V59.24zM25.27 64.35h3.62v9.64c0 3.89 3.17 7.06 7.06 7.06 0 0 0 0 .01 0 0 0 0 0 0 0 0 0 .01 0 .01 0 3.89-.01 7.06-3.17 7.06-7.06V60.24c1.55-1.94 2.41-4.34 2.41-6.82 0-2.9-1.13-5.64-3.19-7.73-2.06-2.06-4.8-3.19-7.73-3.19h-9.25c-.91-1.34-2.42-2.17-4.1-2.17H20.1c-4.01 0-7.28 3.26-7.28 7.28v11.63c0 4.01 3.26 7.28 7.28 7.28h1.07C22.85 66.52 24.37 65.69 25.27 64.35zM40.03 73.98c0 1.71-1.07 3.17-2.57 3.76V70.8c0-.83-.67-1.5-1.5-1.5s-1.5.67-1.5 1.5v6.95c-1.5-.6-2.57-2.06-2.57-3.77v-9.64h2.63c.45 0 .89-.04 1.32-.09.14-.02.27-.04.41-.07.3-.05.61-.11.9-.18.15-.04.29-.08.44-.12.3-.09.59-.19.88-.31.12-.05.24-.09.36-.14.4-.17.79-.36 1.17-.58.01 0 .02-.01.02-.01V73.98zM40.12 47.82c1.5 1.51 2.32 3.5 2.32 5.61 0 1.94-.72 3.82-2.03 5.28-1.5 1.67-3.65 2.63-5.89 2.63h-4.13-4.24V45.51h8.37C36.64 45.51 38.63 46.33 40.12 47.82zM21.17 63.52H20.1c-2.36 0-4.28-1.92-4.28-4.28V47.62c0-2.36 1.92-4.28 4.28-4.28h1.07c.81 0 1.53.48 1.84 1.24.09.22.14.48.14.74v16.23c0 .25-.05.51-.15.75C22.7 63.04 21.98 63.52 21.17 63.52z"></path>
            </svg>
            <div>Click to use Slidepods</div>
        </div>
    </footer>
</template>
