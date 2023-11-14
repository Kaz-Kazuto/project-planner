<template>
     <div class="project" :class="{complete:project.complete}">
        <div class="flexing">
            <div>
                <h3 @click="showDetail=!showDetail">{{project.title}}</h3>
            </div>
            <div>
                <span class="material-symbols-outlined" @click="deleteProject">
                    delete
                </span>
                <span class="material-symbols-outlined">
                    edit
                </span>
                <span class="material-symbols-outlined" @click="completeProject">
                    done
                </span>
            </div>
        </div>
        
        <p v-if="showDetail">{{project.detail}}</p>
        {{project.complete}}  
     </div>
</template>

<script>
import HelloWorldVue from './HelloWorld.vue';
export default {
    props:['project'],
    data(){
        return {
            showDetail:false,
            api: 'http://localhost:3000/project/',
        }
    },
    methods:{
        deleteProject(){
            let deleteRoute=this.api+this.project.id;
            fetch(deleteRoute, {method:'DELETE'})
            .then(()=>{
                this.$emit("delete",this.project.id)
            })
            .catch((err)=>{
                console.log(err);
            });
        },
        completeProject(){
            let updateCompleteRoute=this.api+this.project.id;
            fetch(updateCompleteRoute, {
                method:'PATCH',
                headers:{
                    "Content-Type":"application/json"
                },
                body:JSON.stringify(
                    {
                        complete:!this.project.complete
                    }
                )
                })
                .then(()=>{
                this.$emit("complete",this.project.id)
            })
            .catch((err)=>{
                console.log(err);
            });

        }
    }
}

</script>

<style>
    .project{
        padding: 20px;
        background-color: #f2f2f2;
        cursor: pointer;
        margin: 10px;
        border-left: 6px solid red;
        border-radius: 10px;
    }

    h3{
        color: indigo;
        cursor: pointer;
    }

    h3:hover{
        color: blue;
    }

    .flexing{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    span{
        margin: 8px;
    }
    span:hover{
        cursor: pointer;
        color: red;
    }

    .complete{
        border-left-color: green;
    }
</style>