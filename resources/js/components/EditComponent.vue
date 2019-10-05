<template>
    <div class="modal" tabindex="-1" role="dialog" id="editModal">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Edit Task</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-success alert-dismissible fade show" v-if="success.length > 0">
                        <button type="button" class="close" data-dismiss="alert">&times;</button>
                        <span>{{success}}</span>
                    </div>
                    <label for="title">Title : </label>
                    <input type="text" name="title" id="title" class="form-control" v-model="task.title"/>
                    <ul v-if="error.title" class="list-unstyled">
                        <li v-for="(err, index) in error.title" :key="index" class="error">{{err}}</li>
                    </ul>
                    <label for="title">Description : </label>
                    <textarea name="description" id="description" class="form-control" v-model="task.description"></textarea>
                    <ul v-if="error.description" class="list-unstyled">
                        <li v-for="(err, index) in error.description" :key="index" class="error">{{err}}</li>
                    </ul>
                    <label for="status">Status : </label>
                    <select name="status" class="form-control" v-model="task.status">
                        <option value="Created">Created</option>
                        <option value="Working">Working</option>
                        <option value="Finished">Finished</option>
                        <option value="Cancelled">Cancelled</option>
                    </select>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary" @click="updateTask">Update</button>
                </div>
            </div>
        </div>
    </div>
</template>
<script type="text/javascript">
export default {
    props:['task'],
    data(){
        return {
            success:'',
            error:[],
            title: '',
            description: '',
            baseURL: process.env.MIX_BASE_URL
        }
    },
    methods:{
        updateTask(){
            axios.post(this.baseURL+"tasks/"+this.task.id,{
                'title':this.task.title,
                'description':this.task.description,
                'status':this.task.status,
                '_method':'PUT'
            })
            .then(data => {
                this.$emit('taskupdated',data);
                this.success =  "Task updated successfully";
            })
            .catch(error => {
                this.error = error.response.data.errors;
            })
            this.title = '';
            this.description = '';
        }
    }
}
</script>
<style scoped type="text/css">

</style>