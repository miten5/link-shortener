<template>
    <div class="flex w-full h-screen bg-gradient-to-r from-cyan-500 to-blue-500">
        <div class="m-auto">
            <h2 class="text-4xl mb-4 text-white font-bold text-center">Link shortener</h2>
            <div class="w-96 p-5 bg-white rounded shadow shadow-slate-800">
                <div>
                    <div class="relative">
                        <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                            <svg class="w-5 h-5 text-gray-500 dark:text-gray-400" fill="none" stroke="currentColor" stroke-width="1.5" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
                                <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    d="M13.19 8.688a4.5 4.5 0 011.242 7.244l-4.5 4.5a4.5 4.5 0 01-6.364-6.364l1.757-1.757m13.35-.622l1.757-1.757a4.5 4.5 0 00-6.364-6.364l-4.5 4.5a4.5 4.5 0 001.242 7.244"
                                ></path>
                            </svg>
                        </div>
                        <input
                            type="text"
                            class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                            placeholder="Enter full URL here"
                            v-model="url"
                        />
                        <span v-if="shortenedUrl">
                            <button
                                @click="reset"
                                class="text-white absolute right-2.5 bottom-2.5 bg-red-700 hover:bg-red-800 focus:ring-4 focus:outline-none focus:ring-red-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-800"
                            >
                                Reset
                            </button>
                        </span>
                        <span v-else>
                            <button
                                @click="shortenUrl"
                                class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
                            >
                                Short link
                            </button>
                        </span>
                    </div>
                </div>

                <div v-if="shortenedUrl">
                    <input type="text" v-model="shortenedUrl" readonly class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg p-2.5 w-96" />

                    <button @click="copyShortenedUrl" class="text-white bg-slate-700 hover:bg-slate-800 focus:ring-4 focus:ring-slate-300 font-medium rounded-lg text-sm px-5 py-2.5 ml-2 mb-2">Copy</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import Clipboard from "clipboard";

export default {
    data() {
        return {
            url: "",
            shortenedUrl: "",
        };
    },
    methods: {
        async shortenUrl() {
            const config = {
                headers: {
                    Authorization: `Bearer ${import.meta.env.VITE_BEARER_TOKEN}`,
                    "Content-Type": "application/json",
                },
            };
            const requestBody = {
                long_url: this.url,
            };
            try {
                const response = await axios.post(import.meta.env.VITE_API_URL, requestBody, config);
                console.log(response.data.link);
                this.shortenedUrl = response.data.link;
            } catch (error) {
                console.error(error);
            }
        },
        copyShortenedUrl() {
            Clipboard.copy(this.shortenedUrl);
        },
        reset() {
            this.url = "";
            this.shortenedUrl = "";
        },
    },
};
</script>
