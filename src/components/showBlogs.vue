<template>
  <div v-theme:column="'narrow'" id="show-blogs">
      <h1>All Blog Articles</h1>
      <input type="text" v-model="search" placeholder="search blogs" />
      <div v-for="blog in filteredBlogs" class="single-blog">
        <router-link v-bind:to="'/blog/' + blog.id"><h2>{{ blog.title | to-uppercase }}</h2></router-link>
        <article>{{ blog.content | snippet }}</article>
      </div>
  </div>
</template>

<script>
import searchMixin from '../mixins/searchMixin';


export default { 
  
  data () {
    return {
      blogs: [],
      search: ''
    }
  },
  methods: {
    
  },
  created() {
      this.$http.get('https://vuejs-blog-4b7b1.firebaseio.com/post.json').then(function(data){
          return data.json();
      }).then(function(data){
        var blogsArray = [];
        for (let key in data){
          data[key].id = key
          blogsArray.push(data[key]);
        }
        this.blogs = blogsArray;
      })
  },
  computed: {
    
  },
  filters: {
    toUppercase(value){
      return value.toUpperCase();
    }
  },
  directives: {
    'rainbow': {
      bind(el, binding, vnode){
        el.style.color = "#" + Math.random().toString().slice(2,8);
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
  #show-blogs a{
    color: #444;
    text-decoration: none;
  }
  input[type="text"]{
    padding: 8px;
    width: 100%;
    box-sizing: border-box;
  }
</style>
