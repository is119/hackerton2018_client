<template>
  <v-app id="app">
    <v-toolbar color="black" dark fixed app>
      <v-icon>call_to_action</v-icon>
      <v-toolbar-title align-center>현황판</v-toolbar-title>
    </v-toolbar>
    <v-navigation-drawer
      v-model="drawer"
      permanent
      fixed
      app
    >
      <v-list dense>
        <v-list-tile>
        <v-list-tile-title class="title">
          <h4>[ 통합응급의료관리시스템 ] </h4>
        </v-list-tile-title>
        </v-list-tile>
        <v-list-tile>
        <v-list-tile-sub-title>
          <h4 style="text-align:center">Powered by Hyperleder Fabric</h4>
        </v-list-tile-sub-title>
        </v-list-tile>
        <v-spacer></v-spacer>
        <v-list-tile :to="{path:'/'}">
          <v-list-tile-action>
            <v-icon>dashboard</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>대시보드</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
        <v-divider></v-divider>
        <v-list-tile :to="{path:'/currentbeds'}">
          <v-list-tile-action>
            <v-icon>schedule</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>구급실 현황</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
        <v-list-tile :to="{path:'/currentcalls'}">
          <v-list-tile-action>
            <v-icon>schedule</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>구급차 출동 상황</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
        <v-list-tile :to="{path:'/currentrequests'}">
          <v-list-tile-action>
            <v-icon>dashboard</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>응급구조 요청 현황</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
        <v-list-tile color="red" @click="showModal=true">
          <v-list-tile-action >
            <v-icon>dashboard</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>응급구조 요청</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
        <v-divider></v-divider>
        <v-spacer></v-spacer>
        <v-list-tile :to="{path:'/settings'}">
        <v-list-tile-action>
          <v-icon>settings</v-icon>
        </v-list-tile-action>
        <v-list-tile-content>
          <v-list-tile-title>환경 설정</v-list-tile-title>
        </v-list-tile-content>
      </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    
    <v-content>
        <v-container fluid>
            <v-fade-transition mode="out-in">
                <router-view/>
            </v-fade-transition>
        </v-container>
        <v-dialog v-model="showModal" max-width="500px">
        <v-card>
          <v-card-title>
            응급구조 요청
          </v-card-title>
          <v-card-text>
            <v-form>
              <v-text-field
        v-model="requester"
        label="요청자 ID"
        disabled></v-text-field>
        <v-text-field
        v-model="lang"
        :rules="[v=>!!v || '위도값은 필수입니다']"
        label="위도"
        required></v-text-field>
        <v-text-field
        v-model="long"
        :rules="[v=>!!v || '경도값은 필수입니다']"
        label="경도"
        required></v-text-field>
        <v-text-field
        v-model="major"
        label="특이사항"
        ></v-text-field>
        <v-divider></v-divider>
        <v-btn
        :disabled="!valid"
        @click="submit"
        >
        요청하기
        </v-btn>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" flat @click.stop="showModal=false">닫기</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-content>
    <v-footer color="black" app>
      <span class="white--text">&copy; Information Security Hackerton 2018</span>
    </v-footer>
  </v-app>
</template>

<script>

  export default {
    data: () => ({
      drawer: null,
      showModal:false,
      valid: true,
      requester :'1',
      lang : 0,
      long : 0,
      major : ''
    }),
    props: {
      source: String
    },
    components : {
    },

    methods: {
      submit () {
        if (this.$refs.form.validate()) {
          // Native form submission is not yet supported
          axios.post(CONF.request, {
            requester: this.requester,
            x: this.lang,
            y: this.long,
          })
        }
      },
      clear () {
        this.$refs.form.reset()
      }
    }
  }
  
</script>
<style scoped>
@import url(//fonts.googleapis.com/earlyaccess/nanumpenscript.css);
@import url(//fonts.googleapis.com/earlyaccess/nanumgothic.css);

.ng{font-family: 'Nanum Gothic', sans-serif;}
#title{text-align: center;}
</style>