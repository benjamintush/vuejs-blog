<template>
  <div id="show-blogs">
    <h1>All Blog Articles</h1>
    <input type="text" v-model="search" placeholder="search blogs" />
    <div v-for="blog in filteredBlogs" class="single-blog" :key="blog.id">
      <router-link :to="'/blog/'+blog.id"><h2>{{ blog.title | to-uppercase }}</h2></router-link>
      <!-- <h2 v-rainbow><router-link to="'/blog/'+{{blog.id}}">{{ blog.title | to-uppercase }}</router-link></h2> -->
      <article>{{ blog.content | snippet }}</article>
    </div>
  </div>
</template>

<script>
  import searchMixin from "../mixins/searchMixin";
  export default {
    data () {
      return {
        blogs: [],
        search: ''
      } 
    },
    methods: {
    },
    created () {
      this.$http.get('https://vue-blog-ed03d.firebaseio.com/posts.json').then(function (data) {
        return data.json()
      }).then(function (data) {
        var blogsArray = []
        for (let key in data){
          data[key].id = key;
          blogsArray.push(data[key]);
        }
        this.blogs = blogsArray;
      })
    },
    computed: {
    },
    filters: {
      toUppercase (value) {
        return value.toUpperCase();  
      }
    },
    directives: {
      'rainbow': {
        bind(el, binding, vnode){
          el.style.color = '#' + Math.random().toString(16).slice(2, 8);
        }
      }
    },
    mixins: [searchMixin]
  }
</script>

<style>
  #show-blogs{
    max-width: 800px;
    margin: 0 auto;
  }
  .single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
  }
</style>
