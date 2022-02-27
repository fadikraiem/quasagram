<template>
  <q-page class="constrain q-pa-md">

    <div class="row q-col-gutter-lg">
      <div class="col-12 col-sm-8">
        <template v-if="!loadingPosts && posts.length">
        <q-card v-for="post in posts" :key="post.id " class="card-post" flat bordered>
      <q-item >
        <q-item-section avatar>
          <q-avatar>
            <img src="https://cdn.quasar.dev/img/boy-avatar.png">
          </q-avatar>
        </q-item-section>

        <q-item-section>
          <q-item-label class="text-bold">fadi_kraiem</q-item-label>
          <q-item-label caption>
            {{post.location}}
          </q-item-label>
        </q-item-section>
      </q-item>

      <q-separator />
          <q-img
            :src="post.imageUrl"
          />  
          
      <q-card-section>
        <div>{{post.caption}}</div>
        <div class="text-caption text-grey">{{ niceDate }}</div>
      </q-card-section>
    </q-card>
        </template>
        <template v-else-if="!loadingPosts && !posts.length">
        <h5 class="text-center text-grey"> No posts yet. </h5>
        </template>
        <template v-else>
             <q-card >
      <q-item>
        <q-item-section avatar>
          <q-skeleton type="QAvatar" size="40px" />
        </q-item-section>

        <q-item-section>
          <q-item-label>
            <q-skeleton type="text" />
          </q-item-label>
          <q-item-label caption>
            <q-skeleton type="text" />
          </q-item-label>
        </q-item-section>
      </q-item>

      <q-skeleton height="200px" square />

      <q-card-actions align="right" class="q-gutter-md">
        <q-skeleton type="QBtn" />
        <q-skeleton type="QBtn" />
      </q-card-actions>
    </q-card>
        </template>
      </div>
      <div class="col-4 large-screen-only" >
        <q-item class="fixed">
        <q-item-section avatar>
          <q-avatar size= "48px">
            <img src="https://cdn.quasar.dev/img/boy-avatar.png">
          </q-avatar>
        </q-item-section>

        <q-item-section>
          <q-item-label class="text-bold">fadi_kraiem</q-item-label>
          <q-item-label caption>
            Fadi Kraiem
          </q-item-label>
        </q-item-section>
      </q-item>

      </div>
    </div>
   
  </q-page>
</template>

<script>
import { date } from 'quasar'

export default {
  name: 'PageHome',
  data() {
    return{
      vard : date,
     posts: [
      
     ],
     loadingPosts: false,
    }
  },
  methods: { 
    getPosts(){
       this.loadingPosts = true
 
       
        this.$axios.get(`${process.env.API}/posts`).then(response => {
      this.posts = response.data
      this.loadingPosts = false
      }).catch(err => {
        this.$q.dialog({
          title: 'Error',
          message: 'Could not find your location. '
        })
        this.loadingPosts = false
      })

    
    
    }
  },
  computed: {
    niceDate() {
      return  date.formatDate(1645643971634, 'MMMM D h:mmA')
    }
  },
  created() {
    this.getPosts()
  }
}
</script>
<style lang="sass">
   .card-post
     .q-img 
       min-height: 200px
</style>
