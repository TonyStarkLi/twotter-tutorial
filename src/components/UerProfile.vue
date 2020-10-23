<template>
    <div class="user-profile">
        <div class="user-profile_user-panel">
            <h1 class="user-profile_username">
                @{{user.username}}
            </h1>
            <div class="user-profile_admin-badge" v-if="user.isAdmin">
                admin
            </div>
            <div class="user-profile_follower-count">
                <strong>Followers: </strong> {{ followers }}
            </div>
            <form class="user-profile_create-twoot" @submit.prevent="createNewTwoot">
                <label for="newTwoot"><strong>New Twoot</strong></label>
                <textarea id="newTwoot" rows="4" v-model="newTwootContent"></textarea>

                <div class="user-profile_create-twoot-type">
                    <label for="newTwootType"><strong>Type: </strong></label>
                    <select id="newTwootType" v-model="selectedTwootType">
                        <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                            {{ option.name }}
                        </option>
                    </select>
                </div>

                <button>
                    Twoot!
                </button>
            </form>
        </div>
        <div class="user-profile_twoots-wrapper">
            <TwootItem
                    v-for="twoot in user.twoots"
                    :key="twoot.id" :username="user.username"
                    :twoot="twoot"
                    @favourite="toggleFavourite"
            />
        </div>
    </div>
</template>


<script>

    import TwootItem from "./TwootItem";
    export default {
        name: 'UserProfile',
        components: {TwootItem},
        data() {
            return {
                newTwootContent: '',
                selectedTwootType: 'instant',
                twootTypes: [
                    { value: 'draft', name: 'Draft' },
                    { value: 'instant', name: 'Instant Twoot'}
                ],
                followers: 0,
                user: {
                    id: 1,
                    username: '_MitchellRomney',
                    firstName: 'Mitchell',
                    lastName: 'Romney',
                    email: 'mitchellRomney@theearthisquare.com',
                    isAdmin: true,
                    twoots: [
                        { id: 1, content: "Twotter is Amazing!" },
                        { id: 2, content: "Don't forget to subscriber to The Earth is Square!" }
                    ]
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
            },
            toggleFavourite(id) {
                console.log(`Favourate Tweet #${id}`)
            },
            createNewTwoot() {
                if (this.newTwootContent && this.selectedTwootType !== 'draft') {
                    this.user.twoots.unshift({
                        id: this.user.twoots.length + 1,
                        content: this.newTwootContent
                    })
                }
                this.newTwootContent = ''
            }
        },
        // loud on the first time
        mounted() {
            this.followerUser()
        }
    }
</script>

<style>
    .user-profile {
        display: grid;
        grid-template-columns: 1fr 3fr;
        width: 100%;
        padding: 50px 5%;
    }
    .user-profile_user-panel {
        display: flex;
        flex-direction: column;
        margin-right: 50px;
        padding: 20px;
        background-color: white;
        border-radius: 5px;
        border: 1px solid #DFE3E8;
    }
    h1 {
        margin: 0;
    }
    .user-profile_admin-badge {
        background: rebeccapurple;
        color: white;
        border-radius: 5px;
        margin-right: auto;
        padding: 0 10px;
        font-weight: bold;
    }
    .user-profile_create-twoot {
        padding-top: 20px;
        display: flex;
        flex-direction: column;
    }
</style>