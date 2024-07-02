<template>
  <v-container class="">
    <v-responsive class="mx-auto" max-width="900">
      <v-row>

        <v-col cols="12" xs="6" sm="6">

          <v-text-field label="IdInstance" placeholder="XXX" v-model="idInstance" type="text"></v-text-field>
          <v-text-field label="ApiToken Instance" placeholder="XXX" v-model="apiTokenInstance"
            type="text"></v-text-field>


          <v-btn color="#5865f2" block class="ma-1" @click="initGetMethod('setSettings')">get Settings</v-btn>
          <v-btn color="#5865f2" block class="ma-1" @click="initGetMethod('getStateInstance')">get State
            Instance</v-btn>


          <v-container>
            <v-card> <v-text-field label="Chat ID" placeholder="Chat ID" v-model="message.chatId"
                type="text"></v-text-field>
              <v-textarea block label="Message" model-value="" v-model="message.message" name="input-7-2"
                variant="filled" auto-grow></v-textarea>
              <div class="text-center">
                <v-btn color="#5865f2" class="ma-1 center" @click="initPostMethod('sendMessage', this.message)">Send
                  Message
                </v-btn>
              </div>
            </v-card>
          </v-container>



          <v-container>
            <v-card> <v-text-field label="Chat ID" placeholder="Chat ID" v-model="fileLink.chatId"
                type="text"></v-text-field>
              <v-text-field label="File Name" placeholder="File Name" v-model="fileLink.fileName"
                type="text"></v-text-field>
              <v-text-field label="Link" placeholder="Link" v-model="fileLink.urlFile" type="text"></v-text-field>
              <div class="text-center">
                <v-btn color="#5865f2" class="ma-1 center" @click="initPostMethod('sendFileByUrl', this.fileLink)">Send
                  File
                </v-btn>
              </div>
            </v-card>
          </v-container>

        </v-col>



        <v-col cols="12" xs="6" sm="6">
          <div class="is-error-msg" v-if="isError">
            Error
          </div>
          <v-container fluid>
            <v-textarea label="Response" class="response-area" :class="{ error: isError }" readonly v-model="response"
              name="input-7-1" variant="filled" auto-grow></v-textarea>
          </v-container>
        </v-col>

      </v-row>
    </v-responsive>
  </v-container>
</template>


<script lang="js">

export default {
  data() {
    return {
      idInstance: "1103953379",
      apiTokenInstance: "5ca35bd9dda44eaebcafbae9bde5e56e156d890211af43ff83",
      apiUrl: "https://1103.api.green-api.com",
      mediaUrl: "https://1103.media.green-api.com",
      message: {
        chatId: "",
        message: ""
      },
      fileLink: {
        chatId: "",
        urlFile: "",
        fileName: ""
      },
      response: "",
      isError: false
    }
  },
  computed: {
    ins_url() {
      return `${this.apiUrl}/waInstance${this.idInstance}`
    },
  },
  methods: {
    // функция для методов get
    initGetMethod(methodName) {
      const vue = this;
      vue.isError = false;
      try {
        fetch(this.getLinkByMethod(methodName))
          .then(resp => {
            console.log(resp);
            if (!resp.ok) {
              this.responseError(resp.statusText)
              throw new Error(resp.statusText);
            }
            return resp.text()
          })
          .then(
            response_context => {
              vue.response = response_context;
            }
          )
      } catch (e) {
        vue.responseError(e)
      }

    },
    // функция для методов post
    initPostMethod(methodName, body) {
      const vue = this;
      vue.isError = false;
      try {
        fetch(this.getLinkByMethod(methodName), {
          method: "POST",
          body: JSON.stringify(body),
        }).then(resp => {
          if (!resp.ok) {
            this.responseError(resp.statusText);
            console.log({ resp });
            throw new Error(resp.statusText);
          }
          return resp.text()
        }).then(
          response_context => {
            vue.response = response_context;
          }
        )
      } catch (e) {
        vue.responseError(e)
      }
    },
    // сконструировать url для запроса
    getLinkByMethod(method) {
      return `${this.ins_url}/${method}/${this.apiTokenInstance}`
    },
    // если есть ошибка, показать это
    responseError(errMsg) {
      this.response = errMsg
      this.isError = true;
    }
  }
}

</script>

<style>
.response-area.error {
  border: 1px solid #ff5b5b;
}

.is-error-msg {
  color: #ff5b5b;
}
</style>