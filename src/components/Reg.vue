<template>
    <div class="wrapper">
        <input v-model="regData.login" type="text" placeholder="Login" class="login">
        <input v-model="regData.password" type="text" placeholder="Password" class="password">
        <input v-model="regData.tryPassword" type="text" placeholder="try password" class="tryPassword">
        <button class="approve" v-on:click="registrationClickHandler">Зарегистрироваться</button>
    </div>
</template>

<script>


    export default {
        name: 'Reg',        
        data: () => ({
            serverUrl: 'http://195.49.210.34/',
            regData: {
                login: '',
                password: '',
                tryPassword: ''
            },
            // response: ''
        }), 
    methods: {
        registrationClickHandler: async function () {
            if(!this.regData.login || !this.regData.password || !this.regData.tryPassword) return alert('заполните поля')
            const request_body = {
                userData: {
                    login: this.regData.login,
                    password: this.regData.password,
                    tryPassword: this.regData.tryPassword
                }
            }
            const response = await this.sendRequest('user/registration', 'POST', request_body)
            console.log({response});
            if(response.info.status == "Error") return alert(response.payload)

            else if(response.info.status == "OK")
            this.$router.push({name: 'home', params: {id: response.payload._id}})

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

<style lang="less" scoped>
.wrapper {
    display: grid;
    justify-content: center;
    gap: 15px;
    border-radius: 8px;
    box-shadow: 0 0 3px;
    padding: 15px;
    input {
    outline: none;
    border: 1px solid rgba(140, 145, 150, .6);
    border-radius: 3px;
    &:hover {
    border: 1px solid rgba(140, 145, 150, 1);
    }
    }
}
</style>