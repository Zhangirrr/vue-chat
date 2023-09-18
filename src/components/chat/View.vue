<template>
  <div class="wrapper">
    <div class="messageList">
      <div 
        v-for="message in chat.history"
        :class="{messageWrapper: true, fromMe: login === message.from, notFromMe: login !== message.from}"
        :key="message.id">
        <div class="from">
          <!-- <p>Zhangir</p> -->
          <p> {{ message.from }}</p>
        </div>
        <div class="text">
          {{ message.text }}
        </div>
        <div class="timestamp">
          13:55
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'View.vue',
  data: () => ({

  }),
  props: {
    chat: {
      type: Object,
      default: {
        history: []
      }
    },
    login: {
      type: String,
      default: ''
    }
  }
}
</script>

<style lang="less" scoped>
  .wrapper {
    width: 96.5%;
    height: 90%;
    .messageList {
      width: 100%;
      height: auto;
      display: flex;
      flex-direction: column;
      justify-content: flex-end;
      align-items: center;
      overflow-y: auto;
      .messageWrapper {
        max-width: 60%;
        height: auto;
        min-height: 40px;
        border-radius: 15px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        padding: 20px;
        margin-bottom: 20px;
        * {
          width: 100%;
        }
        .text {
          font-size: 1em;
        }
        .from {
          font-size: 1.1em;
          text-align: left;
        }
        .timestamp {
          font-size: .9em;
          color: rgba(120, 113, 113, 0.9);
          text-align: right;
        }
      }
      .fromMe {
        background-color: rgba(50, 192, 50, .8);
        text-align: right;
        align-self: flex-end;

          p {
            margin: 0;
            padding: 0;
            font-weight: 900;
          }
      }
      .notFromMe {
        background-color: #c9c9c9;
        text-align: left;
        align-self: flex-start;

          p {
            margin: 0;
            padding: 0;
            font-weight: 900;
          }
      }
    }
  }
</style>