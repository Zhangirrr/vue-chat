<template>
    <div class="send_message_field">
        <div class="input_field">
            <div class="smile_png">
                <img src="../../assets/icons8-счастливый-50.png" alt="smile">
            </div>
            <!-- <div class="smile_png"> -->
            <!-- </div> -->
            <div class="input">
                <input v-on:keypress.enter="sendMessageHandler" v-model="text" type="text" placeholder="Message">
            </div>
            <div class="clip_png">
                <img src="../../assets/icons8-скрепка-48.png" alt="clip">
            </div>

            <div class="voice_png">
                <img src="../../assets/icons8-микрофон-50.png" alt="voice">
            </div>
            <div 
            class="send_message"
            @click="sendMessageHandler"
            >
                <p>
                    Send
                </p>                
            </div>
            
        </div>
    </div>
</template>

<script>
// import { async } from 'rxjs'
import axios from 'axios'

    export default {
        name: 'sendMessageField',        
        data: () => ({
            serverUrl: 'http://195.49.210.34/',
            chatData: {
            },
            text: ''
        }), 
        props: {
            chat_id: {
                type: String,
                default: ''
            },
            login_user:{
                type: String,
                default: ''
            }
        },
        methods: {
            sendRequest: async function(path, method, body) {
                const url = `${this.serverUrl}${path}`


                if(method === 'GET') {
                    return (await axios.get(url)).data
                } else {
                    return (await axios.post(url,body)).data
                }
            },

            sendMessageHandler: async function () {
                const sendData = {
                    login: this.login_user,
                    text: this.text,
                    chatId: this.chat_id
                }
                console.log(sendData);
                this.text = ''
                
                const response = await this.sendRequest('chat/send/text', 'POST', sendData)

                // if(response.info.status === 'Error') return alert(response.payload)
                console.log(response);

                // return alert(`Чат ${this.label}, успешно создан`) 
            }

            // if(method !== 'GET') request_config.body = JSON.stringify(body)
            // console.log(JSON.stringify(body));
        },
        mounted () {
            
        }
    }
</script>

<style lang="less" scoped>
    .send_message_field {
        display: flex;
        // background-color: black;
        .input_field {
            position: relative;
            left: 250px;
            display: grid;
            grid-gap: 15px;
            grid-template-columns: .1fr 6fr .1fr .1fr 1fr;
            grid-auto-columns: auto;
            align-content: center;
            // background-color: black;
            .send_message {
                display: grid;
                align-content: center;
                border-radius: 8px;
                background-color: cornflowerblue;
                cursor: pointer;
                p {
                    margin: 0;
                    padding: 0;
                    color: white;
                }
            }
            .input {
                input {
                    width: 500px;
                    border-radius: 15px;
                    height: 30px;
                }
            }
            .smile_png {
                display: grid;
                align-items: center;
                    img {
                        width: 25px;
                        height: 25px;
                        cursor: pointer;
                }
            }
            .clip_png {
                display: grid;
                align-items: center;
                    img {
                        width: 25px;
                        height: 25px;
                        cursor: pointer;
                    }
            }
            .voice_png {
                display: grid;
                align-items: center;
                    img {
                        width: 25px;
                        height: 25px;
                        cursor: pointer;
                    }
            }
        }
    }

</style >