<template>
<div class="nav_bar">
<div class="search_bar">
    <input type="text" class="search_input">
    <img v-on:click="createChatButtonHandler" class="search_button" src="../../assets/icons8-добавить-50.png" alt="add"/>
</div>
<div class="list_chats">
    <div 
    v-on:click="chooseChatButtonHandler(chat._id)" 
    class="chat" 
    v-for="chat in chatData" 
    v-bind:key="chat._id">
        <div class="chat_image" :style="{'backgroundColor': changeColor()}">
            {{ chat.label[0].toUpperCase() }}
        </div>
        <div class="chat_content">
            <div class="title">
                {{ chat.label }}
            </div>
            <div class="last_message">
                <div class="from">
                    {{ chat.history.at(-1).from }}: &nbsp;
                </div>
                <div class="text">
                    {{ chat.history.at(-1).text.slice(0, 17) }}...
                </div>
            </div>
        </div>
    </div>
</div>
</div>
</template>

<script>
import axios from 'axios'
import EventBus from '@/EventBus';

export default {
name: 'chatList',
props: {
    login: {
        type: String,
        default: ''
    }
},     
data: () => ({
    serverUrl: 'http://195.49.210.34/',
    chatData: [],
}),

async mounted () {
    //     const response = await this.sendRequest(`chat/user/${user.login}`, 'GET')
    //     this.chatData = response.payload;

    // })
    EventBus.$on('userDataReady', async (user) =>{
        await this.updateChatList(user.login)
        EventBus.$on('newChat', async (chatData) => {
            await this.updateChatList(user.login)
            
        })
    })

},

methods: {
    updateChatList: async function (login) {
        const response = await this.sendRequest(`chat/user/${login}`, 'GET')
        this.chatData = response.payload;
    },
    sendRequest: async function(path, method, body) {
        const url = `${this.serverUrl}${path}`
        let response

        if(method === 'GET') {
            return (await axios.get(url)).data
        } else {
            return (await axios.post(url,body)).data
        }
        return response.data
    },

    changeColor: function() {
        return `rgb(${Math.floor(Math.random() * 255)}, ${Math.floor(Math.random() * 255)}, ${Math.floor(Math.random() * 255)})`
    },

    createChatButtonHandler: function() {
        this.$emit('openModalCreateChat')
    },
    chooseChatButtonHandler: function(chat_id) {
        this.$emit('chooseChat', chat_id)
    }
}
}
</script>

<style lang="less" scoped>
// * {
//     border: 1px solid red;
// }
.nav_bar {
width: 30vw;
height: 100vh;
box-shadow: 0 0 3px;
justify-content: space-between;
height: 100vh;
display: flex;
flex-direction: column;
// margin-right: 10px;
    .search_bar {
        display: flex;
        width: 100%;
        justify-content: space-around;
        align-items: center;
        margin-bottom: 8px;
        .search_input {
            padding-left: 30px;
            background-image: url(../../assets/icons8-magnifying-glass-50.png);
            background-repeat: no-repeat;
            background-size: contain;
            border-radius: 50px;
            width: 70%;
            height: 70%;
            background-color: #f1f1f1;
            border: none;
        }
        .search_button {
            width: 7%;
            cursor: pointer;
        }
    }
    .list_chats {
    display: flex;
    flex-direction: column;
    gap: 5px;
    overflow-y: auto;
    // margin-left: 10px;
    .chat {
        box-shadow: 0 0 3px;
        display: flex;
        gap: 25px;
        border-radius: 12px;
        margin-top: 2px;
        // color: white;
        &:hover {
            background-color: rgba(163, 161, 161, 0.1);
        }
        .chat_image {
            // label {
            width: 50px;
            background-color: cornflowerblue;
            border-radius: 100%;
            display: grid;
            justify-content: center;
            align-content: center;
        }
        .chat_content {
            display: flex;
            flex-direction: column;
            margin-top: 10px;
            .title {
                font-size: 1.5em;
            }
            .last_message {
                display: flex;
                .text {
                    display: flex;
                }
            }
        }   
    }
}

::-webkit-scrollbar {
        width: 5px;
        height: 8px;
        background-color: #f3faf7;
}

::-webkit-scrollbar-thumb {
    background-color: rgba(192, 191, 191, 0.466);
    border-radius: 9em;
    box-shadow: inset 1px 1px 10px rgba(163, 161, 161, 0.1);

}
::-webkit-scrollbar-thumb:hover {
    background-color: #253861;
}
}

</style >