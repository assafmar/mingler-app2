<template>
  <transition name="fade">
  <section>
    <div class="page" v-if="currUser">
      <div class="marvel-device nexus5">
        <div class="top-bar"></div>
        <div class="sleep"></div>
        <div class="volume"></div>
        <div class="camera"></div>
        <div class="screen">
          <div class="screen-container">
            <div class="status-bar">
              <div class="time"></div>
              <div class="battery">
                <i class="zmdi zmdi-battery"></i>
              </div>
              <div class="network">
                <i class="zmdi zmdi-network"></i>
              </div>
              <div class="wifi">
                <i class="zmdi zmdi-wifi-alt-2"></i>
              </div>
              <div class="star">
                <i class="zmdi zmdi-star"></i>
              </div>
            </div>
            <div class="chat">
              <div class="chat-container">
                <div class="user-bar">
                  <div class="back">
                    <i class="zmdi zmdi-arrow-left"></i>
                  </div>
                  <div v-if="chatUser" class="avatar">
                        <img :src="chatUser.photos[0]" />
                  </div>
                  <div class="name">
                      <span> {{this.$store.state.user.chatUser.name}}</span>
                      <!--<span>me: {{this.$store.state.user.currUser.name}}</span>-->
                      <span v-if="chatUser.socket" class="status">online</span>
                  </div>
                    <!--<div class="userMe">-->
                      <!--<div  v-if="currUser" class="avatar">
                          <img :src="currUser.photos[0]" />
                      </div>
                      <div class="name">
                        <span> {{this.$store.state.user.currUser.id}}-{{this.$store.state.user.currUser.name}}</span>
                        <span class="status">online</span>
                      </div>-->
                    <!--</div>-->
                  <div class="actions more">
                    <i class="zmdi zmdi-more-vert"></i>
                  </div>
                  <div class="actions attachment">
                    <i class="zmdi zmdi-attachment-alt"></i>
                  </div>
                  <div class="actions">
                  <!--<button class="btn-go-home" @click.prevent="moveToBrowse" >Back</button>-->
                  <!--<button class="btn-go-home" @click.prevent="getMyHistory" >get hist</button>-->
                    <i class="zmdi zmdi-phone"></i>
                  </div>
                </div>
                <div class="conversation">
                  <div class="conversation-container">
<!--{{msgs}}-->
                    <div  v-for="msg in msgs" v-if="msg.from === currUser.id || msg.from === chatUser.id " class="message" :class="msgClass(msg)">
                      <div class = " msg-from" >{{msg.fromName}}</div>
                      <!--<div v-if="msg.status" class = " msg-txt" >status: {{msg.status}}</div>-->
                      <!--<div class = " msg-txt" >{{msg.txt}}/st:{{msg.status}}</div>-->
                      <div class = " msg-txt" >{{msg.txt}}</div>
                      <span class="metadata">
                        <span class="time">{{msg.atFormated}}</span>
                        <span class="tick" v-if="msg.from === currUser.id" :class="vvClass(msg)" >
                          <svg v-if="msg.status ==='read'" xmlns="http://www.w3.org/2000/svg" width="16"
                               height="15" id="msg-dblcheck" x="2047" y="2061"><path d="M15.01 3.316l-.478-.372a.365.365 0 0 0-.51.063L8.666 9.88a.32.32 0 0 1-.484.032l-.358-.325a.32.32 0 0 0-.484.032l-.378.48a.418.418 0 0 0 .036.54l1.32 1.267a.32.32 0 0 0 .484-.034l6.272-8.048a.366.366 0 0 0-.064-.512zm-4.1 0l-.478-.372a.365.365 0 0 0-.51.063L4.566 9.88a.32.32 0 0 1-.484.032L1.892 7.77a.366.366 0 0 0-.516.005l-.423.433a.364.364 0 0 0 .006.514l3.255 3.185a.32.32 0 0 0 .484-.033l6.272-8.048a.365.365 0 0 0-.063-.51z" fill="#4fc3f7"/></svg>
                          <svg v-else xmlns="http://www.w3.org/2000/svg" width="16" height="15" 
                                id="msg-dblcheck-ack" x="2063" y="2076"><path d="M15.01 3.316l-.478-.372a.365.365 0 0 0-.51.063L8.666 9.88a.32.32 0 0 1-.484.032l-.358-.325a.32.32 0 0 0-.484.032l-.378.48a.418.418 0 0 0 .036.54l1.32 1.267a.32.32 0 0 0 .484-.034l6.272-8.048a.366.366 0 0 0-.064-.512zm-4.1 0l-.478-.372a.365.365 0 0 0-.51.063L4.566 9.88a.32.32 0 0 1-.484.032L1.892 7.77a.366.366 0 0 0-.516.005l-.423.433a.364.364 0 0 0 .006.514l3.255 3.185a.32.32 0 0 0 .484-.033l6.272-8.048a.365.365 0 0 0-.063-.51z" fill= "#92a58c"/></svg>
                        </span>
                      </span>
                    </div>

                  </div>
                  <form class="conversation-compose">
                    <div class="emoji">
                      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" id="smiley" x="3147" y="3209">
                        <path fill-rule="evenodd" clip-rule="evenodd" d="M9.153 11.603c.795 0 1.44-.88 1.44-1.962s-.645-1.96-1.44-1.96c-.795 0-1.44.88-1.44 1.96s.645 1.965 1.44 1.965zM5.95 12.965c-.027-.307-.132 5.218 6.062 5.55 6.066-.25 6.066-5.55 6.066-5.55-6.078 1.416-12.13 0-12.13 0zm11.362 1.108s-.67 1.96-5.05 1.96c-3.506 0-5.39-1.165-5.608-1.96 0 0 5.912 1.055 10.658 0zM11.804 1.01C5.61 1.01.978 6.034.978 12.23s4.826 10.76 11.02 10.76S23.02 18.424 23.02 12.23c0-6.197-5.02-11.22-11.216-11.22zM12 21.355c-5.273 0-9.38-3.886-9.38-9.16 0-5.272 3.94-9.547 9.214-9.547a9.548 9.548 0 0 1 9.548 9.548c0 5.272-4.11 9.16-9.382 9.16zm3.108-9.75c.795 0 1.44-.88 1.44-1.963s-.645-1.96-1.44-1.96c-.795 0-1.44.878-1.44 1.96s.645 1.963 1.44 1.963z" fill="#7d8489" />
                      </svg>
                    </div>
                    <input v-model="newMsg.txt" class="input-msg" name="input" placeholder="Type a message" 
                      autocomplete="off" @focus="typing()" autofocus></input>
                    <div class="photo">
                      <i class="zmdi zmdi-camera"></i>
                    </div>
                    <button @click.prevent="send" class="send">
                      <div class="circle">
                        <i class="zmdi zmdi-mail-send">&gt;</i>
                      </div>
                    </button>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
 

  </section>
</transition>
</template>

<script>
import msgService from '../services/msg.service';
import moment from "moment";
import { GET_MATCHED } from '../store/store'

// import $ from "jquery";

export default {
  name: 'chat',
  data() {
    return {
      // nickName: null,
      // msgs: msgService.getMsgs(),
      // onlineUsers:[]
      // chatUser: {} ,
      // currUser:{}
      msgs:['333']
    }
  },
  created() {
    this.newMsg = this.createEmptyMsg();
    // this.msgs = msgService.getMsgs();
    console.log('chat.created.1:');
    // this.getMyHistory() ;
    console.log('chat.created.2:');
    this.currUser = this.$store.getters.fetchCurrUser;
        console.log('chat.created.3:');
    this.chatUser = this.$store.getters.fetchChatUser;
    this.msgs = msgService.getMsgs();
    this.getOurHistory() ;
    this.$store.dispatch({ type: GET_MATCHED });
    this.scrollToBottom();

  },
    watch: {
    msgs: function (newMsgs) {
      console.log('CHAT.watch-msgs');
      this.scrollToBottom();

    }
  },

  computed:{
      msgs() {
          var msgs = msgService.getMsgs();
          return msgs;
      },
      currUser() {
          return this.$store.getters.fetchCurrUser;
      },
      chatUser() {
          console.log('chat.computed.chatUser:', this.$store.getters.fetchChatUser.id);
          var chatUser = this.$store.getters.fetchChatUser;
          if (typeof chatUser === 'undefined')moveToBrowse();
          return chatUser;
      }
  },
   beforeDestroy() {
      this.userIsMovingOutOfChat(this.currUser);
  },
  methods: {
      scrollToBottom(){
          var that = this;
          setTimeout(function() {
              
              // while (container.scrollTop < container.scrollHeight) {
              //       text += "The number is " + i;
              //       i++;
              //   }
              
              var container =  document.getElementsByClassName("conversation-container")[0];
              container.scrollTop = container.scrollHeight;
          }, 300);

      },
    // markMsgsAsRead(msgs){
    //     var msg = Object.assign({}, this.newMsg);
    //     msg.type1= 'markMsgsAsRead';
    //     msg.msgs = msgs;
    //     console.log('Chat.markMsgsAsRead:', msg);
    //     msgService.send(msg);

    // },
 //========================== 
    userIsMovingOutOfChat(){
        var msg = Object.assign({}, this.newMsg);
        msg.type1= 'userIsMovingOutOfChat';
        console.log('Chat.userIsMovingOutOfChat:', msg);
        msgService.send(msg);

    },
 //========================== 
    createEmptyMsg() {
        // console.log('Chat.createEmptyMsg, txt:');
      return {txt: '', processed: false, from: this.currUser.id,
                fromName:this.currUser.name,fromSocket:this.currUser.socket,
                  to:this.chatUser.id ,toName:this.chatUser.name,status:'created',
                  type1: 'sendMsgToUser'};
    },
 //===========================
    getOurHistory() {
       
        var msg = Object.assign({}, this.newMsg);
        msg.type1 = 'getOurHistory';
        console.log('Chat.getOurHistory:', msg);
        msgService.send(msg);
        this.newMsg = this.createEmptyMsg();
    },
 //===========================
    vvClass(msg) { 
      return (msg.status === 'read') ? 'sent' : 'vv';
    },
 //===========================
    msgClass(msg) { 
      if (msg.from==='server') return 'server';
      console.log('Chat: msgClass')
      return (msg.from !== this.currUser.id) ? 'received' : 'sent';
    },
 //========================== 
    moveToBrowse() {
      // console.log('Chat: move to browse ')
      this.$router.push('Browse')
    },
 //========================== 
    typing() {
      var msg = {txt: '', processed: false, from: this.currUser.id,fromName:this.currUser.name, 
                  to:this.chatUser.id ,toName:this.chatUser.name, type1: 'typing'};
      console.log('typing:' ,msg);
      msgService.send(msg);
    },
 //==========================
    initUser() {
      var msg = Object.assign({}, this.newMsg);
      msg.type1 = 'initUser';
      // console.log('chat initUser:', msg);
      msgService.send(msg);
      this.newMsg = this.createEmptyMsg();

    },
 //==========================
    send() {
        var msg = this.newMsg
      if(!msg.txt || msg.txt ==='') return;
      console.log('chat send:', this.newMsg);
        msg.atFormated= moment(msg.at).format('HH:mm');

      msgService.send(msg);
      this.newMsg = this.createEmptyMsg();    
      this.scrollToBottom()
        }
  }

}





</script>

<style scoped>
.msg-from{
  color:green;
  font-size:12px;
}
*, *:before, *:after {
  box-sizing: inherit;
}

html {
  box-sizing: border-box;
  height: 100%;
  margin: 0;
  padding: 0;
}

body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-family: "Roboto", sans-serif;
  margin: 0;
  padding: 0;
  height: 100%;
}
img{
  height:100%;
}
.btn-go-home{
    background-color: #4CAF50; /* Green */
    border-radius: 9px;
    border: none;
    color: white;
    padding: 10px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    transition-duration: 0.4s;
    cursor: pointer;
    background-color: white; 
    color: black; 
    border: 2px solid #4CAF50;
}
.btn-go-home:hover {
    background-color: #008CBA;
    color: white;
}
.page {
  position:relative;
  top:-1px;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.marvel-device .screen {
  text-align: left;
}

.screen-container {
  height: 100%;
}

/* Status Bar */

.status-bar {
  height: 25px;
  background: #004e45;
  color: #fff;
  font-size: 14px;
  padding: 0 8px;
}

.status-bar:after {
  content: "";
  display: table;
  clear: both;
}

.status-bar div {
  float: right;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  margin: 0 0 0 8px;
  font-weight: 600;
}

/* Chat */

.chat {
  height: calc(100% - 69px);
}

.chat-container {
  height: 100%;
}

/* User Bar */

.user-bar {
  height: 55px;
  background: #005e54;
  color: #fff;
  padding: 0 8px;
  font-size: 24px;
  position: relative;
  z-index: 1;
}

.user-bar:after {
  content: "";
  display: table;
  clear: both;
}

.user-bar div {
  float: left;
  transform: translateY(-50%);
  position: relative;
  top: 50%;
}

.user-bar .actions {
  float: right;
  margin: 0 0 0 20px;
}

.user-bar .actions.more {
  margin: 0 12px 0 32px;
}

.user-bar .actions.attachment {
  margin: 0 0 0 30px;
}

.user-bar .actions.attachment i {
  display: block;
  transform: rotate(-45deg);
}

.user-bar .avatar {
  margin: 0 0 0 5px;
  width:36px;
  height: 36px;
  border-radius: 50%;
  overflow:hidden;
  display:flex;
    justify-content: center;

  
}
.user-bar .avatar img {
  box-shadow: 0 1px 0 rgba(255, 255, 255, 0.1);
  height: 100%;
  max-width: none;
}

.user-bar .name {
  
  font-size: 17px;
  font-weight: 600;
  text-overflow: ellipsis;
  letter-spacing: 0.3px;
  margin: 0 0 0 8px;
  overflow: hidden;
  white-space: nowrap;
  width: 80%;
}

.user-bar .status {
  display: block;
  font-size: 13px;
  font-weight: 400;
  letter-spacing: 0;
}

/* Conversation */

.conversation {
  height: calc(100% - 12px);
  position: relative;
  background: #efe7dd url("https://cloud.githubusercontent.com/assets/398893/15136779/4e765036-1639-11e6-9201-67e728e86f39.jpg") repeat;
  z-index: 0;
}

.conversation ::-webkit-scrollbar {
  transition: all .5s;
  width: 5px;
  height: 1px;
  z-index: 10;
}

.conversation ::-webkit-scrollbar-track {
  background: transparent;
}

.conversation ::-webkit-scrollbar-thumb {
  background: #b3ada7;
}

.conversation .conversation-container {
  height: calc(100% - 68px);
  box-shadow: inset 0 10px 10px -10px #000000;
  overflow-x: hidden;
  padding: 0 16px;
  margin-bottom: 5px;
}

.conversation .conversation-container:after {
  content: "";
  display: table;
  clear: both;
}

/* Messages */

.message {
  color: #000;
  clear: both;
  line-height: 18px;
  font-size: 15px;
  padding: 8px;
  position: relative;
  margin: 8px 0;
  max-width: 85%;
  word-wrap: break-word;
  z-index: -1;
}

.message:after {
  position: absolute;
  content: "";
  width: 0;
  height: 0;
  border-style: solid;
}

.metadata {
  display: inline-block;
  float: right;
  padding: 0 0 0 7px;
  position: relative;
  bottom: -4px;
}

.metadata .time {
  color: rgba(0, 0, 0, .45);
  font-size: 11px;
  display: inline-block;
}

.metadata .tick {
  display: inline-block;
  margin-left: 2px;
  position: relative;
  top: 4px;
  height: 16px;
  width: 16px;
}

.metadata .tick svg {
  position: absolute;
  transition: .5s ease-in-out;
}

.metadata .tick svg:first-child {
  -webkit-backface-visibility: hidden;
          backface-visibility: hidden;
  -webkit-transform: perspective(800px) rotateY(180deg);
          transform: perspective(800px) rotateY(180deg);
}

.metadata .tick svg:last-child {
  -webkit-backface-visibility: hidden;
          backface-visibility: hidden;
  -webkit-transform: perspective(800px) rotateY(0deg);
          transform: perspective(800px) rotateY(0deg);
}

.metadata .tick-animation svg:first-child {
  -webkit-transform: perspective(800px) rotateY(0);
          transform: perspective(800px) rotateY(0);
}

.metadata .tick-animation svg:last-child {
  -webkit-transform: perspective(800px) rotateY(-179.9deg);
          transform: perspective(800px) rotateY(-179.9deg);
}

.message:first-child {
  margin: 16px 0 8px;
}

.message.received {
  background: #fff;
  border-radius: 0px 5px 5px 5px;
  float: left;
}

.message.received .metadata {
  padding: 0 0 0 16px;
}

.message.received:after {
  border-width: 0px 10px 10px 0;
  border-color: transparent #fff transparent transparent;
  top: 0;
  left: -10px;
}

.message.sent {
  background: #e1ffc7;
  border-radius: 5px 0px 5px 5px;
  float: right;
}

.message.sent:after {
  border-width: 0px 0 10px 10px;
  border-color: transparent transparent transparent #e1ffc7;
  top: 0;
  right: -10px;
}
.message.server {
  background: lightgreen;
  border-radius: 25px;
  text-align: center;
  margin:auto;
}
.sent:after {
  border-style:none;
}


/* Compose */

.conversation-compose {
  display: flex;
  flex-direction: row;
  align-items: flex-end;
  overflow: hidden;
  height: 50px;
  width: 100%;
  z-index: 2;
}

.conversation-compose div,
.conversation-compose input {
  background: #fff;
  height: 100%;
}

.conversation-compose .emoji {
  display: flex;
  align-items: center;
  justify-content: center;
  background: white;
  border-radius: 5px 0 0 5px;
  flex: 0 0 auto;
  margin-left: 8px;
  width: 48px;
}

.conversation-compose .input-msg {
  border: 0;
  flex: 1 1 auto;
  font-size: 16px;
  margin: 0;
  outline: none;
  min-width: 50px;
}

.conversation-compose .photo {
  flex: 0 0 auto;
  border-radius: 0 0 5px 0;
  text-align: center;
  position: relative;
  width: 48px;
}

.conversation-compose .photo:after {
  border-width: 0px 0 10px 10px;
  border-color: transparent transparent transparent #fff;
  border-style: solid;
  position: absolute;
  width: 0;
  height: 0;
  content: "";
  top: 0;
  right: -10px;
}

.conversation-compose .photo i {
  display: block;
  color: #7d8488;
  font-size: 24px;
  transform: translate(-50%, -50%);
  position: relative;
  top: 50%;
  left: 50%;
}

.conversation-compose .send {
  background: transparent;
  border: 0;
  cursor: pointer;
  flex: 0 0 auto;
  margin-left: 8px;
  margin-right: 8px;
  padding: 0;
  position: relative;
  outline: none;
}

.conversation-compose .send .circle {
  background: #008a7c;
  border-radius: 50%;
  color: #fff;
  position: relative;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.conversation-compose .send .circle i {
  font-size: 24px;
  margin-left: 5px;
}

/* Small Screens */

/*@media (max-width: 768px) {*/
  .marvel-device.nexus5 {
    /*border-radius: 0;
    flex: none;
    padding: 0;
    max-width: none;
    overflow: hidden;
    height: 100%;
    width: 100%;*/
  }

  .marvel-device > .screen .chat {
    visibility: visible;
  }

  .marvel-device {
    visibility: hidden;
  }

  .marvel-device .status-bar {
    display: none;
  }
  .screen-container {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }

  .conversation {
    height: calc(100vh - 55px);
  }
  .conversation .conversation-container {
    height: calc(100vh - 200px);
  }
/*}*/



.fade-enter-active, .fade-leave-active {
    transition: opacity 1s
}

.fade-enter,.fade-leave-to{
  opacity: 0
}

</style>

