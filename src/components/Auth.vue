<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <div class="register_window">
            <div class="input_window">
                <input v-model="authData.login" type="text" placeholder="Login" class="login">
                <input v-model="authData.password" type="text" placeholder="Password" class="password">
                <!-- {{ authData.login }} -->
                <!-- {{ authData.password }} -->
                <button class="approve" v-on:click="buttonClickHandler">Войти</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
        name: 'Auth',        
        data: () => ({
            serverUrl: 'http://195.49.210.34/',
            authData: {
                login: '',
                password: ''
            },
            // response: ''
        }), 
    methods: {
        buttonClickHandler: async function () {
            if(!this.authData.login || !this.authData.password) return alert('заполните поля')
            const request_body = {
                userData: {
                    login: this.authData.login,
                    password: this.authData.password
                }
            }
            const response = await this.sendRequest('user/authorization', 'POST', request_body)

            if(response.info.status === 'OK' && response.payload.isAuth) {
                this.$router.push({name: 'home' })
                this.$router.push({name: 'home', params: {id: response.payload.userData[0]._id}})
            } else {
                alert('Логин или пароль некорректен')
            }


            // console.log({response});
        },
        sendRequest: async function (path, method, body) {
            const request_config = {
                method,
                headers: {
                    'Content-Type': 'application/json;charset=utf-8'
                },
                body: null
            }

            if(method !== 'GET') request_config.body = JSON.stringify(body)
            console.log(JSON.stringify(body));

            // fetch(`${this.serverUrl}${path}`, request_config)
            // .then(response => {
            //     // console.log({response});
            //     requestData = response.json()
            //     .then(jsonRes => {
            //         this.response = jsonRes;
            //         // console.log(requestData);
            //     })
            //     .catch (error => {
            //         console.log({error});
            //     })
            // })
            // .catch (error => {
            //     console.log({error})
            // })
                let data = await fetch(`${this.serverUrl}${path}`, request_config)
                return await data.json()

            // console.log({response});
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
h3 {
    margin: 40px 0 0;
}
.register_window {
    border-radius: 8px;
    padding: 15px;
    box-shadow: 0 0 3px;
}
.input_window{
    display: grid;
    justify-content: center;
    gap: 15px;
}
button{
    width: 100px;
}

a {
    color: #42b983;
}
</style>
