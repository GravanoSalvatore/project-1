<template lang="">
  

    <main>
    
       
      <div class="container marketing">
    
        
        
        <!-- START THE FEATURETTES -->
    
       
    
        <div class="row featurette"
        v-for="item in paginatedArticles"
       :key="item"
        >
          
          <div class="col-md-8">
    
            <img class="bd-placeholder-img bd-placeholder-img-lg featurette-image img-fluid mx-auto" 
            :src='item.urlToImage'
            width="800" 
            height="500" 
        
            role="img" aria-label="Placeholder: 500x500" 
            preserveAspectRatio="xMidYMid slice" 
            focusable="false">
            <!-- <title>Placeholder</title>
            <rect width="100%" height="100%" fill="#eee"/> -->
           
        
    
          </div>
       
       
    
          <div class="col-md-4">
            <h2 class="featurette-heading"> <a :href="item.url" target="_blank">{{item.title}}</a><span class="text-muted"></span></h2>
            <p class="lead"><strong>{{item.description}}</strong></p>
            <u class="text-success"> {{item.source.name}}</u><br/>
                    <u><strong style="font-family:gotic">author({{item.author}})</strong></u><br/>
                    <u> {{item.publishedAt}}</u>
          </div>
    
       </div>
       <div class="pagination">
                <button class="prev-bt" @click="prevPage" :disabled="currentPage === 1">Previous</button>
                <span class="page-numbers">{{ currentPage }} / {{ totalPages }}</span>
                <button class="next-bt" @click="nextPage" :disabled="currentPage === totalPages">Next</button>
              </div>
        <!-- /END THE FEATURETTES -->
    
      </div><!-- /.container -->
    
    
      
      
    </main>
    
    
    </template>
    <script>
     import searchGrid from '@/views/Select-3-grid.vue'
    import search2 from '@/views/Select-3.vue'
    export default {
      components:{
     searchGrid,
    search2
      },
      data() {
        return {
          articles: [],       // Array to store articles
          currentPage: 1,    // Current page number
          totalPages: 1,     // Total number of pages
          rows: 1,          // Number of articles per page
        };
      },
      computed: {
        paginatedArticles() {
          // Calculate the range of articles to display on the current page
          const start = (this.currentPage - 1) * this.rows;
          const end = start + this.rows;
          return this.articles.slice(start, end);
        },
      },
      methods: {
        async getData() {
          // Fetch articles from the News API
          const apiKey = 'd205e0353aed4e42b97d11c1a88207f0'
         // const apiKey = '1fb27fc9978d48ecadb4bdc77705325e';
          const pageSize = 100;
          
          try {
            const response = await fetch(
              `https://newsapi.org/v2/top-headlines?category=science&language=en&apiKey=${apiKey}&pageSize=${pageSize}`
            );
            const data = await response.json();
            return data.articles;
          } catch (error) {
            console.error('Error fetching news:', error);
            return [];
          }
        },
        async fetchNews() {
          // Fetch news and update component data
          const articles = await this.getData();
          this.articles = articles;
          this.totalPages = Math.ceil(articles.length / this.rows);
        },
        nextPage() {
          // Go to the next page
          if (this.currentPage < this.totalPages) {
            this.currentPage++;
          }
        },
        prevPage() {
          // Go to the previous page
          if (this.currentPage > 1) {
            this.currentPage--;
          }
        },
      },
      mounted() {
        // Fetch news data when the component is mounted
        this.fetchNews();
      },
    };
    </script>
    <style lang="scss" scoped>
    u{
      text-decoration: none;
    }
    img{
        margin: 10px;
    }
        .next-bt,.prev-bt{
              margin: 13px;
              background-color:white;
              color: rgb(248, 2, 2);
              box-shadow: 0 0 20px 0 rgb(0 0 0 / 50%);
              border-radius: 2px;
              padding: 7px;
              font-weight: bold;
              
            }
            .next-bt:hover,.prev-bt:hover{
              background-image: linear-gradient(to right,#040d1d, #053684);
              color: white;
              
              
          
            } 
            a{
    text-decoration: none;
    color: black;
}
a:hover{
    text-decoration: underline;
}
span{
  margin-top: 20px;
}
    </style>