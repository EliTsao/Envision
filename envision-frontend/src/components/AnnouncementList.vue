<template>
  <div>
    <span>
      <v-btn icon :to="'/forum'">
        <v-icon>keyboard_arrow_left</v-icon>
      </v-btn>
      返回论坛首页
    </span>
    <v-card >
      <v-subheader>
        {{header}}
      </v-subheader>
      <v-list two-line>
        <div v-for="(item, index) in items" :key="index">
          <v-list-tile avatar ripple :to="/announcementdetail/ +item.announcement_id">
            <v-list-tile-content>
              <v-list-tile-title>{{ item.topic }}</v-list-tile-title>    
            </v-list-tile-content>
            <v-list-tile-action>
              <v-list-tile-action-text>{{ item.create_time }}</v-list-tile-action-text>
            </v-list-tile-action>
          </v-list-tile>
          <v-divider v-if="index + 1 < items.length" :key="`divider-${index}`">
          </v-divider>
        </div>
      </v-list>
    </v-card>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      header: '公告',
      items:[]
    }
  },
  methods:{
      AnnoucementListGet: function() {
      let self = this;
      axios.get('http://127.0.0.1:8000/api/AnnouncementViewSet/?ordering=-create_time'
      ).
      then(function(response) {
        self.items=response.data.results;
      }).
      catch(function(error) {
        console.log(error);
      });
    }
  },
  mounted() {
    this.AnnoucementListGet();
  }
}
</script>
