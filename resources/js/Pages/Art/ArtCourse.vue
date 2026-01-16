<template>
    <Layout>
        <template #nav>
            <Nav
                :ArtLibrary="ArtLibrary"
                :types="types"
                v-model:search="search"
                @update:search="val => search = val"
                v-model:typeCat="typeCat"
                @update:typeCat="(val) => {
                    typeCat = val;
                    onChange();
                }"
                @update:clear="search = ''"
            />
        </template>

        <div class="mt-24">
            <div class="flex flex-col xl:flex-row justify-between items-end xl:items-center px-4 xl:px-0 xl:ml-12 xl:mr-0 mb-5">
                <!-- Main Header Group (Left) -->
                <div class="flex justify-between items-center w-full xl:w-[70%] xl:mr-4">
                    <h1 class="font-bold text-xl lg:text-3xl text-gray-200">
                        Art Course <span class="text-xs lg:text-sm text-gray-400 align-top">(Total {{ localCourseTotal ?? 0 }})</span>
                    </h1>
                    <div class="flex items-center gap-3">
                        <button class="bg-gradient-to-r from-gray-700 to-gray-600 hover:from-gray-600 hover:to-gray-500 text-white px-4 py-1.5 rounded-full text-xs font-bold shadow-md transition-all active:scale-95 border border-white/10" @click="$inertia.visit('/art-course-management')">Manage</button>
                        <Link href="/" class="text-gray-400 hover:text-white transition bg-gray-800/80 p-2 rounded-full hover:bg-gray-700 border border-white/10">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6"/>
                            </svg>
                        </Link>
                    </div>
                </div>

                <!-- Recent Header (Right - Desktop Only) -->
                <div class="hidden xl:block w-[25%] border-l-4 border-green-500/50 pl-4">
                    <span class="text-white text-xl lg:text-3xl font-bold tracking-wide">Recently Updated</span>
                </div>
            </div>

            <div class="flex">
                <DataCards
                    imgSrc="course"
                    :cardDetailData="cardDetailData"
                    :ArtLibrary="ArtLibrary"
                    :filteredArtLibraryData="filteredArtLibraryData"
                    @update:typeCat="(val) => {
                        typeCat = val;
                        onChange();
                    }"
                />
                <DataRecentList
                    imgSrc="course"
                    :recent="recent"
                    @update:typeCat="(val) => {
                        typeCat = val;
                        onChange();
                    }"
                />
            </div>
        </div>

        <!-- Scroll buttons -->
        <ScrollButtons />
    </Layout>
</template>

<script setup>
    import { onMounted, onUnmounted, ref, computed, watch } from "vue";
    import { router } from "@inertiajs/vue3";
    import { throttle } from "lodash/function";

    import Layout from "@/Shared/Layout.vue";
    import Nav from "@/Shared/Nav.vue";
    import DataCards from "@/Shared/DataCards.vue";
    import DataRecentList from "@/Shared/DataRecentList.vue";
    import ScrollButtons from "@/Shared/ScrollButtons.vue";

    let props = defineProps({
        courseTotal: Number,
        ArtLibrary: Object,
        types: Object,
        filters: Object,
        recent: Object,
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
    
    // Local total count to preserve value across pagination
    const localCourseTotal = ref(props.courseTotal);
    
    watch(() => props.ArtLibrary, (newData) => {
        if (newData.current_page === 1) {
            localCourseTotal.value = props.courseTotal;
        }
    });

    let filteredData = [];
    const filteredArtLibraryData = computed(() => {
        if (props.ArtLibrary.current_page === 1) {
            filteredData = props.ArtLibrary.data;
        } else {
            filteredData.push(...props.ArtLibrary.data)
        }

        return filteredData;
    });

    const search = ref(props.filters.search);
    let typeCat = "";

    watch(search, debounce(function (value) {
        router.post(props.ArtLibrary.first_page_url, { search: value, type: typeCat }, {
            replace: true,
            preserveScroll: true,
        });

        if (search.value === "") {
            window.addEventListener("scroll", handleScroll);
            filteredData.splice(40, filteredData.length);
        }
    }, 300));

    function debounce(func, wait) {
        let timeout;
        return function executedFunction(...args) {
            const later = () => {
                clearTimeout(timeout);
                func(...args);
            };
            clearTimeout(timeout);
            timeout = setTimeout(later, wait);
        };
    }

    function onChange() {
        router.post(props.ArtLibrary.first_page_url, { search: search.value, type: typeCat }, {
            replace: true,
            preserveScroll: true,
        });

        if (typeCat === "") {
            window.addEventListener("scroll", handleScroll);
            filteredData.splice(40, filteredData.length);
        }
    }

    const loading = ref(false);
    const handleScroll = throttle(() => {
        let pixelsFromBottom = document.documentElement.offsetHeight - document.documentElement.scrollTop - window.innerHeight;

        if (pixelsFromBottom < 1000 && !loading.value) {
            loading.value = true;

            if (props.ArtLibrary && props.ArtLibrary.next_page_url) {
                router.post(props.ArtLibrary.next_page_url, {search: search.value, type: typeCat}, {
                    preserveState: true,
                    preserveScroll: true,
                    onSuccess: () => { loading.value = false },
                    onError: error => {
                        console.error("Failed to load more data:", error);
                        loading.value = false;
                    }
                });
            } else {
                loading.value = false;
            }
        }
    }, 200);

    onMounted(() => {
        window.addEventListener("scroll", handleScroll);
    });

    onUnmounted(() => {
        window.removeEventListener("scroll", handleScroll);
    });
</script>
