<template>
    <main>
        <nav class=" bg-slate-400 w-full h-12 text-black flex flex-row items-center justify-between">
            <div>
                <router-link class=" text-xl ml-10" to="/">Home</router-link>
                <router-link v-if="isAuthenticated" class=" text-xl ml-10" to="/profile">Profile</router-link>
            </div>
            <div v-if="isAuthenticated">
                <button class="text-md mx-5 text-white bg-blue-600 px-2 py-1 rounded-md font-bold"
                    @click="logoutUser">Logout</button>
            </div>
            <div v-else>
                <router-link class="text-md ml-5 text-white bg-blue-600 px-2 py-1 mr-5 rounded-md font-bold"
                    to="/login">Login</router-link>
            </div>
        </nav>
        <router-view />
    </main>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'

export default {
    computed: {
        ...mapGetters(['isAuthenticated'])
    },
    methods: {
        ...mapActions(['logoutUser', 'refreshToken']),
    },
    mounted() {
        try {
            if (this.refreshToken()) {
                this.refrestTokenInterval = setInterval(this.refreshToken, 4 * 60 * 1000)
            } else {
                throw error
            }
        } catch {
            console.error('Token refresh error!')
        }
    },
    beforeDestroy() {
        clearInterval(this.refreshTokenInterval)
    }
}
</script>

<style scoped>
@import '../src/assets/css/main.css';
</style>
