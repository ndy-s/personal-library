<template>
    <div class="bg-gray-200">
        <nav class="h-14 lg:h-16 text-white bg-custom-black-pearl bg-opacity-[80%] flex fixed z-30 w-full top-0 items-center shadow">
            <div class="container mx-auto px-2 sm:px-4">
                <div class="flex justify-between lg:justify-around items-center gap-2 lg:gap-4">
                    <!-- Logo -->
                    <Link href="/" class="flex flex-col items-center flex-shrink-0">
                        <h1 class="text-lg sm:text-xl lg:text-3xl font-bold text-gradient bg-gradient-to-r from-green-200 to-green-400 bg-clip-text text-transparent">
                            Personal Library
                        </h1>
                        <small class="text-green-400 text-[10px] sm:text-xs lg:text-sm hidden sm:block">Manage your digital tutorials, guides, and books</small>
                    </Link>

                    <!-- Search Bar - Modernized -->
                    <div v-if="ArtLibrary" class="hidden lg:flex ml-4 xl:ml-12 flex-1 max-w-[60%] items-center bg-gray-800/50 backdrop-blur-md rounded-full border border-white/10 shadow-lg px-2 py-1 focus-within:ring-2 focus-within:ring-green-400/50 transition-all duration-300">
                        
                        <!-- Filter Dropdown -->
                        <div class="relative border-r border-white/10 pr-2 mr-2">
                             <select
                                :value="typeCat"
                                @change="$emit('update:typeCat', $event.target.value)"
                                class="appearance-none bg-transparent text-gray-300 text-sm font-semibold py-2 px-4 pr-8 rounded-full focus:outline-none cursor-pointer hover:text-white transition-colors"
                                title="Category Option"
                            >
                                <option class="bg-gray-800" value="" selected>All</option>
                                <option
                                    class="bg-gray-800"
                                    v-for="(type, index) in types"
                                    :key="index"
                                    :value="Object.values(type).toString()"
                                >{{ Object.values(type).toString() }}</option>
                            </select>
                            <!-- Custom Chevron -->
                            <div class="pointer-events-none absolute inset-y-0 right-2 flex items-center text-gray-400">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
                            </div>
                        </div>

                        <!-- Search Input -->
                        <div class="relative flex-1">
                             <!-- Search Icon -->
                            <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                                <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                            </div>
                            
                            <input
                                type="search"
                                :value="search"
                                @input="$emit('update:search', $event.target.value)"
                                class="block w-full pl-10 pr-10 py-2 bg-transparent text-white placeholder-gray-400 text-sm font-medium focus:outline-none rounded-full"
                                placeholder="Search library..."
                                required
                            >
                            
                            <!-- Clear Button -->
                            <button
                                v-show="search != undefined && search != ''"
                                @click="$emit('update:clear')"
                                class="absolute inset-y-0 right-0 pr-3 flex items-center text-gray-400 hover:text-white transition-colors"
                                title="Clear"
                            >
                                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                            </button>
                        </div>
                    </div>

                    <div class="flex items-center gap-2">
                        <!-- Mobile Menu Button -->
                        <button
                            v-if="ArtLibrary"
                            @click="mobileMenuOpen = !mobileMenuOpen"
                            class="lg:hidden p-2 text-green-400 hover:text-white focus:outline-none"
                            title="Toggle Menu"
                        >
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path v-if="!mobileMenuOpen" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                                <path v-else stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                            </svg>
                        </button>

                        <!-- Profile Dropdown -->
                        <div class="relative">
                            <button
                                class="rounded-full overflow-hidden border-2 border-green-400 w-8 h-8 lg:w-10 lg:h-10 flex justify-center items-center | hover:animate-spin hover:border-gray-200 focus:animate-spin focus:border-gray-200 ease-in-out"
                                @click="dropdownOpen = true"
                            >
                                <img src="/icon.jpg" alt="Logo">
                            </button>
                            <div v-show="dropdownOpen" class="fixed inset-0 w-full h-screen z-20 bg-black opacity-25" @click="dropdownOpen = false"></div>
                            <div v-show="dropdownOpen" class="absolute z-30 mt-2 right-0" :class="{'hidden': !dropdownOpen }">
                                <div class="bg-custom-prussian-blue rounded-lg shadow-lg py-2 w-60">
                                    <Link href="/art-library-management" class="block text-gray-200 font-semibold px-4 py-2 | hover:bg-gray-700 hover:text-white">Library Management</Link>
                                    <Link href="/art-course-management" class="block text-gray-200 font-semibold px-4 py-2 | hover:bg-gray-700 hover:text-white">Course Management</Link>
                                    <Link href="/art-tutorial-book-management" class="block text-gray-200 font-semibold px-4 py-2 | hover:bg-gray-700 hover:text-white">Tutorial Book Management</Link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </nav>

        <!-- Mobile Search Menu -->
        <div 
            v-if="ArtLibrary && mobileMenuOpen" 
            class="lg:hidden fixed top-14 left-0 right-0 z-20 bg-custom-black-pearl bg-opacity-95 border-b border-green-400 p-4"
        >
            <div class="flex flex-col gap-3">
                <div class="relative">
                    <select
                        :value="typeCat"
                        @change="$emit('update:typeCat', $event.target.value)"
                        class="w-full py-2.5 px-4 text-sm font-semibold text-white bg-gray-800 border border-white/20 rounded-lg focus:ring-2 focus:ring-green-400/50 focus:outline-none appearance-none"
                        title="Category Option"
                    >
                        <option class="text-left bg-gray-800" value="" selected>All Categories</option>
                        <option
                            class="text-left bg-gray-800"
                            v-for="(type, index) in types"
                            :key="index"
                            :value="Object.values(type).toString()"
                        >{{ Object.values(type).toString() }}</option>
                    </select>
                     <!-- Custom Chevron -->
                    <div class="pointer-events-none absolute inset-y-0 right-3 flex items-center text-gray-400">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
                    </div>
                </div>

                <div class="relative w-full">
                    <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                        <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                    </div>
                    <input
                        type="search"
                        :value="search"
                        @input="$emit('update:search', $event.target.value)"
                        class="block w-full pl-10 pr-10 py-2.5 text-sm text-white bg-gray-800 border border-white/20 rounded-lg placeholder:text-gray-400 focus:ring-2 focus:ring-green-400/50 focus:outline-none"
                        placeholder="Search library..."
                        required
                    >
                    <button
                        v-show="search != undefined && search != ''"
                        @click="$emit('update:clear')"
                        class="absolute inset-y-0 right-0 pr-3 flex items-center text-gray-400 hover:text-white"
                        title="Clear"
                    >
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile menu overlay -->
        <div 
            v-if="mobileMenuOpen" 
            class="lg:hidden fixed inset-0 z-10 bg-black opacity-25" 
            @click="mobileMenuOpen = false"
        ></div>
    </div>
</template>

<script setup>
    import { ref } from "vue";

    const props = defineProps({
        ArtLibrary: Object,
        types: Object,
        search: String,
        typeCat: String,
    });

    const dropdownOpen = ref(false);
    const mobileMenuOpen = ref(false);
</script>

