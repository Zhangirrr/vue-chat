<template>
    <div class="home">
        <div class="switcher">
            <button class="authSwitcher" v-on:click="switch_handler(false)">Авторизация</button>
            <button class="regSwitcher" v-on:click="switch_handler(true)">Регистрация</button>
        </div>
        
    <div class="forms">
        <Reg v-show="switcher" />
        <Auth v-show="!switcher" />
    </div>
    
        <!-- <h1>Welcome to My first Vue.js App</h1> -->
    </div>
</template>

<script>
    import Auth from '@/components/Auth.vue'
    import Reg from '@/components/Reg.vue'

    export default {
        name: 'Login',
        components: {
            Auth,
            Reg
        },
        data: () => ({
            switcher: false
        }),
        methods: {
            switch_handler: function (isSwitch) {
                this.switcher = isSwitch
            }
        }
    }
</script>

<style lang="less" scoped>
.home {
    width: 100vw;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    // gap: 15px;
    .switcher {
        width: 50%;
        height: 45px;
        padding: 8px 16px 8px 16px;
    & * {
        width: 25%;
        cursor: pointer;
        font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        font-size: 1.25em;
    }
    }
}
</style>
