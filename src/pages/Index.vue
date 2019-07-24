<template>
  <q-page >
   <div class="q-pa-md" style="max-width: 350px">
      
      <q-spinner
        v-if="loading"
        color="primary"
        size="3em"
      />
    <q-list v-else bordered>

      <q-item 
      clickable
       v-ripple
       v-for="post in posts" 
       :key="post.data.id"
       @click="showimagedialog(post.data.preview.images[0].source.url)"
       >
        <q-item-section avatar>
          <q-avatar size="80px">
            <img :src="post.data.thumbnail">
          </q-avatar>
        </q-item-section>
        <q-item-section>{{post.data.title}}</q-item-section>
      </q-item>
    </q-list>

     <q-dialog full-width v-model="icon" >
      <q-card>
        <q-card-section class="row items-center justify-between">
          <div class="text-h6">image</div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>

        <q-card-section>
           <q-img
      :src="imageurl"
      spinner-color="blue"
    />
        </q-card-section>
      </q-card>
    </q-dialog>
   </div>
  </q-page>
</template>

<style>
</style>

<script>
export default {
  name: 'PageIndex',
  created() {
    this.$axios.get('https://www.reddit.com/r/awww.json?raw_json=1')
    .then(response=>{
      this.loading=false
      console.log(response);
      this.posts= response.data.data.children
    })
    .catch(err=>{
      this.loading=false
      console.log(err);  
    })
  },
  data(){
    return{
      icon:false,
      imageurl:'',
      posts:[ ],
      loading:true 
    }
  },
  methods: {
    showimagedialog(image){
      this.icon= true
      this.imageurl= image; 
    }
  },
}
</script>
