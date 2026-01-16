<template>
    <Layout>
        <template #nav>
            <Nav />
        </template>

        <!-- Mobile Layout -->
        <div class="lg:hidden mt-20 px-4 pb-10">
            <DetailBackButton linkHref="/art-course" />
            
            <!-- Main Image -->
            <div class="relative w-full md:w-1/2 md:mx-auto aspect-[2/3] mt-6 rounded-2xl shadow-2xl overflow-hidden ring-1 ring-white/10">
                <div class="absolute inset-0 bg-gradient-to-t from-gray-900 via-transparent to-transparent z-10"></div>
                <img :src="`/img/course/${ArtLibrary.image_path}`" class="w-full h-full object-cover" @click="openModal()">
            </div>

            <!-- Title & Info -->
            <div class="mt-6">
                <h1 class="text-white text-2xl lg:text-3xl font-bold leading-tight drop-shadow-md">{{ ArtLibrary.title_en }}</h1>
                <div class="flex flex-wrap items-center gap-2 mt-3">
                    <span class="inline-block text-emerald-300 bg-emerald-900/40 px-3 py-1 border border-emerald-500/30 rounded-full text-xs font-bold shadow-sm backdrop-blur-sm">{{ ArtLibrary.publisher }}</span>
                    <span class="text-gray-400 text-xs font-medium">{{ ArtLibrary.year }}</span>
                </div>
            </div>

            <!-- Actions -->
            <div class="flex flex-col gap-3 mt-6">
                <template v-if="ArtLibrary.link && ArtLibrary.link !== 'N/A'">
                    <a :href="ArtLibrary.link" target="_blank" class="flex items-center justify-center gap-2 bg-gradient-to-r from-yellow-400 to-yellow-500 hover:from-yellow-300 hover:to-yellow-400 text-gray-900 font-bold py-3 px-6 rounded-full text-sm shadow-lg shadow-yellow-500/20 transition-all active:scale-95">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 15a4 4 0 004 4h9a5 5 0 10-.1-9.999 5.002 5.002 0 10-9.78 2.096A4.001 4.001 0 003 15z"></path></svg>
                        Google Drive
                    </a>
                </template>
                <template v-else>
                    <button disabled class="flex items-center justify-center gap-2 bg-gray-700/50 text-gray-400 font-bold py-3 px-6 rounded-full text-sm border border-gray-600 cursor-not-allowed">
                        Link N/A
                    </button>
                </template>

                <template v-if="ArtLibrary.path && ArtLibrary.path !== 'N/A'">
                    <a :href="ArtLibrary.path" target="_blank" class="flex items-center justify-center gap-2 bg-gradient-to-r from-blue-400 to-blue-500 hover:from-blue-300 hover:to-blue-400 text-white font-bold py-3 px-6 rounded-full text-sm shadow-lg shadow-blue-500/20 transition-all active:scale-95">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.75 17L9 20l-1 1h8l-1-1-.75-3M3 13h18M5 17h14a2 2 0 002-2V5a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path></svg>
                        Local Directory
                    </a>
                </template>
                <template v-else>
                    <button disabled class="flex items-center justify-center gap-2 bg-gray-700/50 text-gray-400 font-bold py-3 px-6 rounded-full text-sm border border-gray-600 cursor-not-allowed">
                        Path N/A
                    </button>
                </template>
            </div>

            <!-- Description -->
            <div class="mt-8 bg-gray-800/40 rounded-xl p-5 border border-white/5 shadow-inner">
                <h3 class="text-white text-lg font-semibold mb-2">Details</h3>
                <div class="space-y-3 text-sm text-gray-300">
                    <div class="flex justify-between border-b border-gray-700/50 pb-2">
                        <span class="text-gray-500">Original</span>
                        <span class="text-right">{{ ArtLibrary.original }}</span>
                    </div>
                    <div class="flex justify-between border-b border-gray-700/50 pb-2">
                        <span class="text-gray-500">Author</span>
                        <span class="text-right">{{ ArtLibrary.author }}</span>
                    </div>
                    <div class="flex justify-between border-b border-gray-700/50 pb-2">
                        <span class="text-gray-500">Released Year</span>
                        <span class="text-right">{{ ArtLibrary.year }}</span>
                    </div>
                    <div class="flex justify-between border-b border-gray-700/50 pb-2">
                        <span class="text-gray-500">Language</span>
                        <span class="text-right">{{ ArtLibrary.lang }}</span>
                    </div>
                    <div class="flex justify-between border-b border-gray-700/50 pb-2">
                        <span class="text-gray-500">Video</span>
                        <span class="text-right">{{ ArtLibrary.video }}</span>
                    </div>
                    <div class="pt-2">
                        <span class="text-gray-500 block mb-1">Description</span>
                        <p class="leading-relaxed">{{ ArtLibrary.desc }}</p>
                    </div>
                </div>
            </div>

             <!-- Sub Items (Mobile) -->
            <div v-if="SubArtLibrary && SubArtLibrary.length" class="mt-6 flex flex-col gap-4">
                <div v-for="(sub, idx) in SubArtLibrary" :key="idx" class="bg-gray-800/60 rounded-xl p-4 border border-white/10">
                    <p class="text-white font-bold">{{ sub.title }}</p>
                    <p class="text-gray-400 text-xs mb-3">{{ sub.sub_desc }}</p>
                    <div class="flex gap-2">
                        <a v-if="sub.link && sub.link !== 'N/A'" :href="sub.link" target="_blank" class="flex-1 text-center bg-yellow-500/90 text-gray-900 font-bold py-2 rounded-lg text-xs">Drive</a>
                        <a v-if="sub.path && sub.path !== 'N/A'" :href="sub.path" target="_blank" class="flex-1 text-center bg-blue-500/90 text-white font-bold py-2 rounded-lg text-xs">Local</a>
                    </div>
                </div>
            </div>
        </div>

        <!-- Desktop Layout -->
        <div class="hidden lg:block mt-16 relative w-[100%] h-auto">
            <img :src="`/img/course/${ArtLibrary.image_path}`" class="w-[100%] h-[60vh] object-cover blur-2xl z-[-10] clip-path opacity-50">
            <DetailBackButton linkHref="/art-course"/>

            <div class="absolute top-0 mx-[27%] my-20 leading-7 w-[50%]">
                <h1 class="text-white font-bold text-[30px] mb-2 leading-8 drop-shadow-md">{{ ArtLibrary.title_en }}</h1>
                <span class="inline-block cursor-default text-emerald-300 bg-emerald-900/40 px-3 py-1 border border-emerald-500/30 rounded-full text-xs font-bold shadow-sm backdrop-blur-sm">{{ ArtLibrary.publisher }}</span>

                <table class="text-white mt-3">
                    <tr><td class="font-semibold">Original</td><td> : {{ ArtLibrary.original }}</td></tr>
                    <tr><td class="font-semibold">Author</td><td> : {{ ArtLibrary.author }}</td></tr>
                    <tr><td class="font-semibold">Released Year</td><td> : {{ ArtLibrary.year }}</td></tr>
                    <tr><td class="font-semibold">Language</td><td> : {{ ArtLibrary.lang }}</td></tr>
                    <tr><td class="font-semibold">Video</td><td> : {{ ArtLibrary.video }}</td></tr>
                    <tr><td class="font-semibold">Status</td><td> : {{ ArtLibrary.status }}</td></tr>
                    <tr><td class="font-semibold">Description</td><td> : {{ ArtLibrary.desc }}</td></tr>
                </table>
            </div>

            <div class="absolute top-0 w-[15%] h-[80%] mx-[10%] mt-14 rounded-lg">
                <img :src="`/img/course/${ArtLibrary.image_path}`" class="h-full w-full object-cover rounded-lg cursor-pointer shadow-2xl transition hover:scale-[1.02]" @click="openModal()">
            </div>

            <div class="absolute bg-gradient-to-br from-gray-800/90 to-gray-900/90 backdrop-blur-md h-[55vh] w-[20%] top-0 right-0 rounded-2xl my-5 mx-5 p-5 overflow-auto shadow-2xl border border-white/5">
                <div class="border border-white/10 rounded-xl p-4 w-full bg-gray-800/30">
                    <p class="text-white text-xl font-bold leading-tight drop-shadow-md">{{ ArtLibrary.title_en }}</p>
                    <p class="text-gray-300 mb-4 text-xs font-semibold tracking-wide uppercase mt-1">Master Data • {{ ArtLibrary.desc }}</p>
                    <div class="flex flex-col gap-3">
                        <template v-if="ArtLibrary.link && ArtLibrary.link !== 'N/A'">
                            <a :href="ArtLibrary.link" target="_blank" class="flex items-center justify-center gap-2 bg-gradient-to-r from-yellow-400 to-yellow-500 hover:from-yellow-300 hover:to-yellow-400 text-gray-900 font-bold py-3 px-6 rounded-full text-sm shadow-lg shadow-yellow-500/20 transition-all hover:scale-105 active:scale-95">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 15a4 4 0 004 4h9a5 5 0 10-.1-9.999 5.002 5.002 0 10-9.78 2.096A4.001 4.001 0 003 15z"></path></svg>
                                Google Drive
                            </a>
                        </template>
                        <template v-else>
                            <span class="flex items-center justify-center gap-2 bg-gray-700/50 text-gray-400 font-semibold py-3 px-6 rounded-full text-sm border border-gray-600 cursor-not-allowed">
                                Link N/A
                            </span>
                        </template>

                        <template v-if="ArtLibrary.path && ArtLibrary.path !== 'N/A'">
                            <a :href="ArtLibrary.path" target="_blank" class="flex items-center justify-center gap-2 bg-gradient-to-r from-blue-400 to-blue-500 hover:from-blue-300 hover:to-blue-400 text-white font-bold py-3 px-6 rounded-full text-sm shadow-lg shadow-blue-500/20 transition-all hover:scale-105 active:scale-95">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.75 17L9 20l-1 1h8l-1-1-.75-3M3 13h18M5 17h14a2 2 0 002-2V5a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"></path></svg>
                                Local Directory
                            </a>
                        </template>
                        <template v-else>
                            <span class="flex items-center justify-center gap-2 bg-gray-700/50 text-gray-400 font-semibold py-3 px-6 rounded-full text-sm border border-gray-600 cursor-not-allowed">
                                Path N/A
                            </span>
                        </template>
                    </div>
                </div>

                <div class="border border-white/10 rounded-xl mt-4 p-4 w-full bg-gray-800/30" v-for="(subArtLib, index) in SubArtLibrary" :key="index">
                    <p class="text-white text-lg font-semibold leading-6 mb-1">{{ subArtLib.title }}</p>
                    <p class="text-gray-400 mb-3 text-xs font-medium">{{ subArtLib.sub_desc }}</p>
                    <div class="flex gap-2">
                        <a v-if="subArtLib.link && subArtLib.link !== 'N/A'" :href="subArtLib.link" target="_blank" class="flex-1 text-center bg-gradient-to-r from-yellow-400 to-yellow-500 hover:from-yellow-300 hover:to-yellow-400 text-gray-900 font-bold py-2 px-3 rounded-full text-xs shadow-md transition-all hover:scale-105">Drive</a>
                        <a v-if="subArtLib.path && subArtLib.path !== 'N/A'" :href="subArtLib.path" target="_blank" class="flex-1 text-center bg-gradient-to-r from-blue-400 to-blue-500 hover:from-blue-300 hover:to-blue-400 text-white font-bold py-2 px-3 rounded-full text-xs shadow-md transition-all hover:scale-105">Local</a>
                    </div>
                </div>
            </div>
        </div>

        <div class="flex flex-col xl:flex-row mt-12 px-4 xl:px-0">
            <h1 class="text-white flex text-2xl lg:text-3xl font-bold w-full xl:w-[70%] xl:ml-12 border-l-4 border-green-400 pl-4 mb-4 xl:mb-0">
                Recommended Courses
            </h1>
            <h1 class="hidden xl:block w-[25%] mx-5">
                <span class="text-white text-2xl lg:text-3xl font-bold border-l-4 border-green-400 pl-4">Recently Updated</span>
            </h1>
        </div>

        <div class="flex flex-col xl:flex-row bottom-0">
            <DataCards
                imgSrc="course"
                :cardDetailData="cardDetailData"
                :ArtLibrary="ArtLibrary"
                :filteredArtLibraryData="AllArtLibrary"
            />
            <DataRecentList imgSrc="course" :recent="recent" />
        </div>

        <Transition>
            <div v-if="showModal" @click="closeModal" class="fixed inset-0 flex items-center justify-center z-50 bg-opacity-50 bg-black transition-all ease-in-out duration-500">
                <div class="bg-white p-4 rounded shadow">
                    <img :src="`/img/course/${ArtLibrary.image_path}`" class="w-full h-auto">
                </div>
            </div>
        </Transition>

        <!-- Scroll buttons -->
        <ScrollButtons />
    </Layout>
</template>

<script setup>
    import { ref } from "vue";
    import Layout from "@/Shared/Layout.vue";
    import Nav from "@/Shared/Nav.vue";
    import DataCards from "@/Shared/DataCards.vue";
    import DataRecentList from "@/Shared/DataRecentList.vue";
    import DetailBackButton from "../../Shared/DetailBackButton.vue";
    import ScrollButtons from "@/Shared/ScrollButtons.vue";

    const props = defineProps({
        'AllArtLibrary': Object,
        'ArtLibrary': Object,
        'SubArtLibrary': Object,
        'recent': Object,
    });

    const cardDetailData = [
        {
            id: 'original',
            title: 'Original',
        },
        {
            id: 'year',
            title: 'Released Year',
        },
        {
            id: 'lang',
            title: 'Language',
        },
        {
            id: 'video',
            title: 'Video',
        },
        {
            id: 'status',
            title: 'Status',
        },
        {
            id: 'desc',
            title: 'Description'
        }
    ];

    const showModal = ref(false)
    const openModal = () => {
        showModal.value = true;
    };
    const closeModal = () => {
      showModal.value = false
    };
</script>

<style scoped>
    .v-enter-active,
    .v-leave-active {
        transition: opacity 0.1s, transform 0.1s;
    }

    .v-enter-from,
    .v-leave-to {
        opacity: 0;
        transform: scale(0.5);
    }

    .v-enter-to,
    .v-leave-from {
        opacity: 1;
        transform: scale(1);
    }
</style>
