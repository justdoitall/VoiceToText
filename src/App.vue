<template>
  <!-- Voice Record Title + Button -->
  <div class="hello">
    <h1>Audio</h1>
    <div>
      <button class="button1" @click="recordAudio()">Start</button>
      <button class="button2" @click="stop">Stop</button>
      <div class = "player" id="audio"></div>
      <div class="textget">{{tryf}}</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      recorder: null,
      /*myBlob: null,*/
      createAudio: null,
      tryf: null,
    }
  },
  methods: {
    recordAudio() {
      let device = navigator.mediaDevices.getUserMedia({ audio: true });
      let items = [];
      device.then((stream) => {
        this.recorder = new MediaRecorder(stream)
        this.recorder.start();
        this.recorder.ondataavailable = (e) => {
          items.push(e.data);
          if (this.recorder.state === "inactive") {
            let myBlob = new Blob(items, { type: "audio/ogg" });
            let lookAudio = document.getElementById("audio");
            let createAudio = document.createElement("audio");
            createAudio.setAttribute("controls", "controls");
            lookAudio.appendChild(createAudio);
            createAudio.src =
                URL.createObjectURL(myBlob) +
                '" type="audio/ogg" />';
            const form= new FormData();
            form.append('ogg', new File([myBlob], '111.ogg'));
            console.log(form)
            axios.post('http://demo.telminov.ru:8091/stt/', form, { headers:{
                  'Authorization': `Token c3dd999bd9992918837b6a11cd0c2e02f340829d`,
                    /*'content-type': 'multipart/form-data'*/},
              /*Username: 'user',
              Password: 'C3X7nbpNhkdv',*/
              /*ogg: '://Users//Я//Downloads//8366'*/
            })
                .then ((response) => {
                  if (!(response.data.result))
                      this.tryf = "Попробуйте еще раз"
                  else
                  this.tryf = response.data.result

                })
                .catch ((error) =>
                    console.error(error))
          }
        };
      });
    },
    stop () {
      this.recorder.stop()
    },
    async sendreq() {
      /*axios({
        method: 'POST',
        url: 'http://demo.telminov.ru:8091/stt/',
        data: {
          username: 'user',
        password: 'C3X7nbpNhkdv',
          /!*ogg: '://Users//Я//Downloads//8366'*!/
        }
      });*/
      /*await new Response(this.myBlob).arrayBuffer();*/
      /*axios({
        method: 'post',
        url: 'http://demo.telminov.ru:8091/stt/',
        params: {
          _token: 'c3dd999bd9992918837b6a11cd0c2e02f340829d',
        },
        data: {
          ogg: '://Users//Я//Downloads//8366',
        },
        headers: {
          "Content-type": "application/json; charset=UTF-8"
        }
      })
          .then(function (response) {
            console.log('Ответ сервера успешно получен!');
            console.log(response.data);
          })
          .catch(function (error) {
            console.log(error);
          })*/
      /*const url = 'http://demo.telminov.ru:8091/stt/'
      axios.post(url,
        this.new_person, {
        dataType: 'json',
        'Access-Control-Allow-Origin': 'localhost:8080',
        'Access-Control-Allow-Methods': 'GET,PUT,POST,DELETE'
      })
          .then(function (response) {
            console.log('Ответ сервера успешно получен!');
            console.log(response.data);
          })
          .catch(function (error) {
            console.log(error);
          })*//*Authorization: 'c3dd999bd9992918837b6a11cd0c2e02f340829d',*/


      /*axios.post('http://demo.telminov.ru:8091/stt/',{ogg: this.createAudio}, { headers:
            {'Authorization': `Token c3dd999bd9992918837b6a11cd0c2e02f340829d`,
              'content-type': 'multipart/form-data'}
        /!*Username: 'user',
        Password: 'C3X7nbpNhkdv',*!/
        /!*ogg: '://Users//Я//Downloads//8366'*!/
      })
          .then ((response) => {
        this.tryf = response
      })
      .catch ((error) =>
          console.error(error))*/
    }
  },
};
</script>
<style>
#app {
  display: flex;
  justify-content: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.hello {
  font-size: 1em;
  position: absolute;
  top: 25%
}

.button1{
  position: relative;
  height: 2em;
  width: 4em;
  margin: 10px;
  border-radius: 20% 10%;
  transition: all 1.2s;
  background-color: #ffffff;
  font-size: 120%;

}

.button2{
  position: relative;
  height: 2em;
  width: 4em;
  margin: 10px ;
  border-radius: 20% 10%;
  transition: all 1.2s;
  background-color: #85f635;
  font-size: 120%;
}

.textget {
  position: relative;
  height: 1em;
  margin-top: 30px;
  font-size: 25px;
}

.player {
  position: relative;
  margin-top: 30px;
}
</style>