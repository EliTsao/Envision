<template>
  <v-layout row wrap> 
    <v-flex xl12 lg12 md12 sm12 xs12 order-sm2 order-xs2 order-lg1 order-md1 order-xl1>
      <v-card>
        <v-layout align-center justify-center style="
          height: 300px; 
          background-image: url('https://raw.githubusercontent.com/AAIT-SUSE/forum/master/src/assets/img/profile-bg.jpg');
          background-repeat: no-repeat;
          background-size: conver; 
          z-index: -99
        ">
          <v-flex xs3>
            <v-img
              :src="'https://api.adorable.io/avatars/165/' + userAvatar"
              height="125px"
              width="125px"
              contain
              style="margin-left:60px;border-radius:100%"
            ></v-img>
          </v-flex>
          <v-flex xs9>
            <v-card-title primary-title class="white--text">
              <div>
                <div class="headline">{{ username }}
                </div>
                <div>
                  <v-icon></v-icon>
                  {{ userSignature }}
                </div>
                <br>
                <div>
                  <v-tooltip right>
                    <v-chip color="white" slot="activator">
                      <v-avatar>
                        <v-icon color="primary">local_offer</v-icon>
                      </v-avatar>
                      {{ userCredits }} 积分
                    </v-chip>
                    <span>
                      积分可以通过以下途径获得: <br>
                      <li>热心帮助他人</li>
                      <li>对社区做出贡献</li>
                      <li>积极参加活动</li>
                      <li>参与其他活动、赛事等</li>
                      <br>
                      使用积分可以提出"直答"问题，或在礼品店兑换礼品
                    </span>
                  </v-tooltip>
                </div>
              </div>
              <div>
                <v-timeline dense v-if=" mini == true ">
                  <v-timeline-item
                    v-for="item in items"
                    :key="item.index"
                    color="red lighten-2"
                    small
                    left
                    fill-dot
                  >
                    <span>{{ item.time }}&nbsp;&nbsp;&nbsp;{{ item.assignment }}</span>                
                  </v-timeline-item>
                </v-timeline> 
              </div>
            </v-card-title>
          </v-flex>
        </v-layout>
      </v-card> 
    </v-flex>
    <v-flex xl12 lg12 md12 sm12 xs12 order-sm2 order-xs2 order-lg1 order-md1 order-xl1>
      <div>  
        <v-tabs
          slot="extension" v-model="tab" color="warning" grow>
          <v-tab to="/person/articlelist">我的文章</v-tab>
          <v-tab to="/person/postlist">我的帖子</v-tab>
          <v-tab to="/person/questions">我的直答</v-tab>
          <v-tab to="/person/attention">我的关注</v-tab>
          <v-tab to="/person/study">我的学习</v-tab>
        </v-tabs>
        <v-tabs-items>
          <v-tab-item>
            <router-view></router-view>
          </v-tab-item>
        </v-tabs-items>
      </div>
    </v-flex>
  </v-layout>
</template>

<script>
  import storage from '@/plugins/storage';

export default {

  data() {
    return {
      username: '',
      userCredits: '666',
      userSignature: 'Envision 组员',
      userAvatar: '',
      tab: null,
    }
  },
  methods: {
    GetPersonInfo:function() {
      let self = this;
      self.username = storage.state.username;
      self.userAvatar = storage.state.avatar;
      let pid = storage.state.uid;
      axios.get(`http://127.0.0.1:8000/api/UserViewSet/${pid}`
      )
      .then(function (response) {
        console.log(response);
        self.userSignature = response.data.results.user_description;
      })
      .catch(function (error) {
        console.log(error);
      });
    }
  },
  mounted() {
    this.GetPersonInfo();
  }
}
</script>
<style scoped>
</style>