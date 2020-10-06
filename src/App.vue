<template>
    <div id="app">
        {{ user.username }} - {{ fullName }}
        <strong>Followers: </strong> {{followers}}
        <button @click="followerUser">
            Follow
        </button>
    </div>
</template>

<script>

export default {
    name: 'App',
    data() {
        return {
            followers: 0,
            user: {
                id: 1,
                username: '_MitchellRomney',
                firstName: 'Mitchell',
                lastName: 'Romney',
                email: 'mitchellRomney@theearthisquare.com',
                isAdmin: true
            }
        }
    },
    // watch a data point when it changed
    watch: {
        followers(newFollowerCount, oldFollowerCount) {
            if (oldFollowerCount < newFollowerCount) {
                console.log(`${this.user.username} has gained a follower!`)
            }
        }
    },
    computed: {
        fullName() {
            return `${this.user.firstName} ${this.user.lastName}`;
        }
    },
    methods: {
        followerUser() {
            this.followers++
        }
    },
    // loud on the first time
    mounted() {
        this.followerUser()
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    display: flex;
    flex-direction: column;
    text-align: center;
}
</style>
