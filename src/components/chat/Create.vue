<template>
    <div class="wrapper">
        <img src="../../assets/icons8-macos-закрыть-50.png" class="close" v-on:click="close_modal_window">
        <input v-model="label" placeholder="Название..." type="text">
        <input v-model="description" placeholder="Описание..." type="text">
        <hr>
        <h3>Выберите участников чата.</h3>
        <div class="addMemberWrapper">
            <input list="usersList" v-model="selectMember" placeholder="Выберите участника..." type="text">
            <div v-on:click="add_member_button_handler" class="addMemberButton button">Добавить</div>
        </div>
        <div class="addedMembersList">
            Добавленные участники: {{ selectedMembers }}
        </div>
        <div v-on:click="create_chat_button_handler" class="submit button">Создать</div>
        <datalist id="usersList">
            <option v-for="user in usersList"
            :key="user._id" 
            :value="user.login"
            />
        </datalist>
    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'CreateChat',
        data: () => ( {
            serverUrl: 'http://195.49.210.34/',
            label: '',
            description: '',
            selectMember: '',
            selectedMembers: [],
            usersList: []   
        }),

        async mounted () {
            this.usersList = (await this.sendRequest('user', 'GET')).payload
            console.log(this.usersList);
        },  

        methods: {
            sendRequest: async function(path, method, body) {
                const url = `${this.serverUrl}${path}`
                // let response

                // method = method.toLowerCase()

                // return (await axios[method](url, body)).data

                if(method === 'GET') {
                    return (await axios.get(url)).data
                } else {
                    return (await axios.post(url,body)).data
                }
                // return response.data
            },

            create_chat_button_handler: async function() {
                if(!this.label) return alert("Введите название")
                if(this.selectedMembers.length <= 0) return alert("Добавьте участника")
                if(!this.description) return alert("Введите описание")

                const data_for_create_chat = {
                    userId: this.$route.params.id,
                    label: this.label,
                    members: this.selectedMembers,
                    description: this.description 
                }


                
                const response = await this.sendRequest('chat/create', 'POST', data_for_create_chat)

                if(response.info.status === 'Error') return alert(response.payload)
                console.log(response);

                return alert(`Чат ${this.label}, успешно создан`) 
            },
            add_member_button_handler: async function () {
                if(!this.selectMember) return alert("Выберите участника")
                if(this.selectedMembers.includes(this.selectMember)) return alert("Вы уже добавили данного пользователя")

                let selectedMemberIsExist = false
                let counter = 0

                for (const user of this.usersList){


                    if(user.login === this.selectMember){
                        selectedMemberIsExist = true
                    }
                    if(this.selectedMembers.includes(user.login)){
                        counter++
                        } 
                }
                if(!selectedMemberIsExist) return alert(`Пользователя с логином ${this.selectMember} не существует`)
                if(counter !== this.selectedMembers.length) return alert(`Пользователя с таким логином не существует`)

                this.selectedMembers.push(this.selectMember)

                this.selectMember = ''
            },
            close_modal_window: function () {
                this.$emit('closeModalCreateChat')
            }
        }
    }
</script>

<style scoped lang="less">
    .wrapper {
        position: relative;
        height: auto;
        display: flex;
        flex-direction: column;
        justify-content: space-evenly;
        align-items: center;
        position: relative;
        padding-top: 50px;
        border: 1px solid rgba(37, 47, 37, 1);
        padding: 20px;
        border-radius: 10px;
        background-color: white;
        .close {
            width: 2.5%;
            position: absolute;
            right: 5px;
            top: 5px;
            cursor: pointer;
            margin: 0;
        }
        .addMemberWrapper {
            width: 50%;
            display: flex;
            justify-content: space-evenly;
            align-items: center;
            input {
            width: 60%;
            }
        }
        .button {
            padding: 4px 16px;
            border: 1px solid rgba(37, 47, 37, 1);
            border-radius: 10px;
            cursor: pointer;
            &:hover {
                background-color: rgba(166, 166, 166, 0.785);
            }
        }
        input {
            width: 40%;
            padding: 8px 16px;
            outline: none;
            border: 1px solid rgba(37, 47, 37, 1);
            border-radius: 10px;
        }
        * {
            margin-top: 20px;
        }
        hr {
            width: 100%;
        }
        h3 {
            padding: 0;
            margin: 0;
        }
        }
</style>