<template>
    <Page>
        <ActionBar title="Welcome to NativeScript-Vue!"/>
        <GridLayout columns="*" rows="*">
            <Label class="message" :text="msg" col="0" row="0" @tap="hi" />
            <Button @tap="onTapScheduleNotification" class="btn" text="Schedule Notification"></Button>
        </GridLayout>
    </Page>
</template>

<script >

import { SocketIO } from 'nativescript-socketio';
import { LocalNotifications } from "nativescript-local-notifications";

  export default {

    mounted() {
        this.mySocket = new SocketIO('http://192.168.1.67:3000/');
        console.log(this.mySocket);
        this.mySocket.connect();
         this.mySocket.on('connect', function(){
            console.log('connectaaado');
        });
         this.mySocket.on("error", error => {
            console.log("Errorcito gay", error)
        });

    LocalNotifications.addOnMessageReceivedCallback(notificationData => {
        this.message = "Notification received: " + JSON.stringify(notificationData);
      });


    },
    methods:{
        hi() {
            console.log('Peeerrro');
            //socketIO.connect(server);
            this.mySocket.connect();
            console.log(this.mySocket.connected )
        },
        onTapScheduleNotification() {
        LocalNotifications.schedule(
            [{
              id: 1,
              title: 'Notificación local',
              subtitle: 'Aqui el subtitulo',
              body: 'Listo tu nuevo culito',
              bigTextStyle: false, // Allow more than 1 row of the 'body' text on Android, but setting this to true denies showing the 'image'
             // color: #,
              image: "https://images-na.ssl-images-amazon.com/images/I/61mx-VbrS0L.jpg",
              thumbnail: "https://2.bp.blogspot.com/-H_SZ3nAmNsI/VrJeARpbuSI/AAAAAAAABfc/szsV7_F609k/s200/emoji.jpg",
              forceShowWhenInForeground: true,
              channel: "vue-channel",
              ticker: "Special ticker text for Vue (Android only)",
              at: new Date(new Date().getTime() + (5 * 1000)), // 5 seconds from now
              actions: [
                {
                  id: "yes",
                  type: "button",
                  title: "Yes (and launch app)",
                  launch: true
                },
                {
                  id: "no",
                  type: "button",
                  title: "No",
                  launch: false
                }
              ]
            }])
            .then(() => {
              alert({
                title: "Notification scheduled",
                message: "ID: 1",
                okButtonText: "OK, thanks"
              });
            })
            .catch(error => console.log("doSchedule error: " + error));
      },
    },
    data() {
      return {
        mySocket : null,
        msg: 'Hello World!'
      }
    }
  }
</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }

    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20;
        color: #333333;
    }
</style>
