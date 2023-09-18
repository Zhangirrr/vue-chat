<template>
<div class="home">
    <div @click="closeModalCreateChat" class="createChatModalWrapper" v-if="createChat"></div>
    <Create v-if="createChat" class="create_class" @closeModalCreateChat="closeModalCreateChat"/>

    <div class="chat_page">
        <List :login="user.login" @chooseChat="chooseChat" @openModalCreateChat="openModalCreateChat" class="chatList"/>
        <div class="chat_view">            
            <MainChat 
            :chat="currentChat" class="view"
            :login="user.login"
            />

            <SendMessage
            :chat_id="currentChat._id"
            :login_user="user.login"
            class="send_message"
            />     
        </div>
    </div>
</div>
</template>


<script>
import MainChat from '@/components/chat/View.vue';
import Create from '@/components/chat/Create.vue';
import List from '@/components/chat/List.vue';
import SendMessage from '@/components/chat/SendMessage.vue';
import axios from 'axios';
import EventBus from '@/EventBus';

export default {
    name: 'home',
    data: () => ({
        createChat: false,
        serverUrl: 'http://195.49.210.34/',
        user: {
            login: ''
        },
        currentChat: {
            _id:'',
        },
        intervalId: '',
        webSocket: null
    }),  
    components: {
    MainChat,
    List,
    SendMessage,
    Create
    },
    async mounted () {
        const userId = this.$route.params.id
        const response = await this.sendRequest(`user/${[userId]}`, 'GET')

        this.user = response.payload[0];

        EventBus.$emit('userDataReady', this.user)
        console.log(this.user);
        this.webSocket = new WebSocket(`ws://195.49.210.34/ws?login=${this.user.login}`)
        
        this.webSocket.onopen = async (event) => {
            console.log({event});
            this.webSocket.onmessage = async (msg) => {
                const serverData = JSON.parse(msg.data)
                console.log({serverData});
                if(serverData.type === 'newMessage') {
                    this.updateChatData(serverData.payload.chatId)
                }else if(serverData.type === 'newChat') {
                    EventBus.$emit('newChat', serverData.payload)

                }

            }
            this.webSocket.onclose = async () => {
            console.log('connection closed');
            }
        }
        this.webSocket.onerror = async (error) => {
            console.log({error});
        }

    },  
methods: {
    openModalCreateChat: function () {
        this.createChat = true
    },
    closeModalCreateChat: function () {
        this.createChat = false
    },
    sendRequest: async function(path, method, body) {
        const url = `${this.serverUrl}${path}`
        let response

        if(method === 'GET') {
            return (await axios.get(url)).data
        } else {
            return (await axios.post(url,body)).data
        }
    },
    chooseChat: async function (chat_id) {

        // if(this.intervalId) {
            // clearInterval(this.intervalId)
        // }
        this.currentChat = (await this.sendRequest(`chat/${chat_id}`, 'GET')).payload[0]
        // this.intervalId = setInterval(() => this.updateChatData(chat_id), 1000)
    },
        updateChatData: async function (chat_id) {
            const oldChatDataHistory = this.currentChat.history
            this.currentChat = (await this.sendRequest(`chat/${chat_id}`, 'GET')).payload[0]
            const newChatData = this.currentChat.history

            if(oldChatDataHistory.length !== this.currentChat.history.length){
                this.notifyMe(this.currentChat.history.at(-1)?.text)
            }
    },
        notifyMe: function(text) {  
            if (!("Notification" in window)) {
                alert("This browser does not support desktop notification");
            } else if (Notification.permission === "granted") {
                const notification = new Notification(text);
            } else if (Notification.permission !== "denied") {
                Notification.requestPermission().then((permission) => {
                    if (permission === "granted") {
                        const notification = new Notification(text);
                    }
                });
            }
        }
}
}

</script>

<style scoped lang="less">
.home {
    width: 100vw;
    height: 100vh;
    position: relative;
    margin: 10px;

        .createChatModalWrapper {
            position: fixed;
            width: 100vw;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            top: 0;
            left: 0;
            background-color: gray;
            opacity: .6;
        }
        .create_class {
    position: fixed;
    width: 50vw;
    height: 50vh;
    left: calc(50% - 25vw);
    top: calc(50% - 25vh);
}

    .chat_page {
        display: grid;
        grid-template-columns: 3fr 10fr;
        grid-gap: 10px;
        .chat_view {
            display: grid;
            grid-template-rows: 9fr 1fr;
            grid-gap: 10px;
            .send_message {
                // position: relative;
                // position: fixed;
                // bottom: 0;
            }
        }
    }
}

</style>