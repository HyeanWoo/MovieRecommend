<template>
  <v-hover v-slot:default="{ hover }">
    <v-card :elevation="hover ? 8 : 2">
      <v-layout align-center py-4 pl-4>
        <v-flex text-center>
          <v-container grid-list-lg pa-0>
            <v-layout column>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title class="headline">
                    {{ title }}
                  </v-list-item-title>
                  <v-list-item-subtitle>{{ genreSplit }}</v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
              <v-card-text>
                <v-layout justify-center>
                  <v-rating
                    :value="rating"
                    color="indigo"
                    background-color="indigo"
                    half-increments
                    dense
                    readonly
                  />
                  <div class="grey--text ml-4">{{ rating.toFixed(1) }}</div>
                </v-layout>
              </v-card-text>
              <v-card-text>
                <v-layout justify-center>
                  <v-icon color="black">mdi-eye</v-icon>
                  <div class="grey--text ml-4">{{ viewCnt }}</div>
                </v-layout>
              </v-card-text>
            </v-layout>
          </v-container>
        </v-flex>
      </v-layout>
      <v-btn color="primary" class="ma-2" dark @click="dialog = true">평점 작성</v-btn>
      <v-dialog v-model="dialog" persistent max-width="300">
        <v-card>
          <v-layout justify-center>
            <v-card-title class="headline">이 영화의 점수는?</v-card-title>
          </v-layout>
            <v-card-text>
                <v-layout justify-center>별점을 매겨주세요!</v-layout>
            </v-card-text>
          <v-layout justify-center>
          <v-rating
            v-model="newRating.rating"
            color="indigo"
            background-color="indigo"
          />
          </v-layout>
          <v-card-actions>
            <div class="flex-grow-1"></div>
            <v-btn color="green darken-1" text @click="submit()">완료</v-btn>
            <v-btn color="green darken-1" text @click="dialog = false">취소</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-card>
  </v-hover>
</template>

<script>
import axios from "axios";
import router from '@/router';

export default {
  data(){
    return {
      newRating:{
          userid:this.$session.get('id'),
          movieid:this.id,
          rating:"",
          timestamp:"123456789"
      },
      dialog:false
    }
  },
  props: {
    id: {
      type: Number,
      default: 0
    },
    title: {
      type: String,
      default: ""
    },
    genres: {
      type: String,
      default: ""
    },
    img: {
      type: String,
      default: ""
    },
    rating: {
      type: Number,
      default: 0.0
    },                                                                                                                                                                   
    viewCnt: {
      type: Number,
      default: 0
    },
    check:{
      type: Boolean,
      default : true,
    }
  },
  computed: {
    genreSplit() {
      var str = this.genres
      return str.replace(/\|/g," / ")
    }
  },
  components: {
    
  },
  methods:{
    submit(){
      axios.post(this.$store.state.server + "/api/ratings/", this.newRating);
      this.dialog = false;
    }
  }
};
</script>