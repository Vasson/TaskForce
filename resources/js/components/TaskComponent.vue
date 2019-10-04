<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-10">
                <a href="#addModal" class="btn btn-success" data-toggle="modal">Add</a>
                <table class="table table-striped table-hover">
                    <thead>
                        <tr>
                        <th scope="col">#</th>
                        <th scope="col">Task</th>
                        <th scope="col">Description</th>
                        <th scope="col">Status</th>
                        <th scope="col">Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(task, index) in tasks" :key="index">
                        <th scope="row">{{task.id}}</th>
                        <td>{{task.title}}</td>
                        <td>{{task.description}}</td>
                        <td>{{task.status}}</td>
                        <td>
                            <a href="#editModal" data-toggle="modal" class="btn btn-primary" @click="getRecord(task.id)">Edit</a> 
                            <a class="btn btn-danger text-white" @click="deleteRecord(task.id)">Delete</a></td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
        <div id="modal">
            <add-task-component @taskadded="refreshData"></add-task-component>
            <edit-task-component :task='editRec' @taskupdated="refreshData"></edit-task-component>
        </div>
    </div>
</template>
<script type="text/javascript">
export default {
    data(){
            return{
                tasks:{},
                editRec:{}
            }
        },
    methods:{
        refreshData(data){
            this.tasks = data.data
        },
        getRecord(id){
            axios.get(config.APP_URL+'/tasks/'+id+'/edit')
            .then(response => this.editRec = response.data)
            .catch(error => this.response.errors)
        },
        deleteRecord(id){
            const reply = confirm("Are you sure you want to delete record?");
            if(reply){
                axios.post(config.APP_URL+'/tasks/'+id,{
                    id:id,
                    _method:'DELETE'
                })
                .then(response => this.tasks = response.data)
                .catch(error => this.response.errors)
            }else{
                return
            }
                
        }
    },
    mounted() {
            axios.get(config.APP_URL+'/tasks')
            .then((response) => this.tasks = response.data)
            .catch((error) => console.log(error));
        }
    
}
</script>
<style type="text/css" scoped>
@import "../../css/custom.css";
</style>