<template>
  <div class="home">
    <h1>HOME</h1>
    <FilterNav @filterValue="current=$event" :current="current"></FilterNav>
    <div v-for="project in filteredproject" :key="project.id">
      <SingleProject :project="project" @delete="deleteproject" @complete="completeProject"></SingleProject>
    </div>
  </div>
  
</template>

<script>
import FilterNav from '../components/FilterNav'
import SingleProject from '../components/SingleProject'


export default {
  name: 'HomeView',
  components: {
    FilterNav,
    SingleProject,
    
  },
  data(){
    return {
      projects:[],
      current:"all"
    }
  },
  computed:{
    filteredproject(){
      if(this.current === "complete"){
        return this.projects.filter((p)=>{
          return p.complete
        })
      }
      if(this.current === "ongoing"){
        return this.projects.filter((p)=>{
          return !p.complete
      })
      }
      return this.projects;
    }
  },
  methods:{
    deleteproject(id){
      this.projects=this.projects.filter(project=>{
        return project.id!=id; 
      })
    },  
    completeProject(id){
      let findProject=this.projects.find(project=>{
        return project.id===id;
      }); 
      findProject.complete= !findProject.complete;
    }
  },
  mounted(){
    fetch('http://localhost:3000/project')
    .then((response)=>{
      return response.json()
    })
    .then((datas)=>{
      this.projects=datas;
    })
    .catch(()=>{

    });
  }
}
</script>
