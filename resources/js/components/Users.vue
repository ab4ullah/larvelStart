<template>
    <div class="container">
        <div class="row mt-5">
      <div class="col-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Users table</h3>

                <div class="card-tools">
                    <button class="btn btn-success" data-toggle="modal" data-target="#addnew"> Add user <i class="fas fa-user-plus fa-fw"></i>

 </button>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0" style="height: 300px;">
                <table class="table table-head-fixed">
                  <thead>
                    <tr>
                      <th>ID</th>
                      <th>Name</th>
                      <th>Email</th>
                      <th>Type</th>
                      <th>Registed</th>
                      <th>Modify</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for=" user in users" :key="user.id">
                      <td>{{user.id}}</td>
                      <td>{{user.name}}</td>
                      <td>{{user.email}}</td>
                      <td>{{user.type | uptext}}</td>
                      <td>{{user.created_at | mydate}}</td>
                      <td>
                          <a href="#"><i class="fas fa-user-edit"></i></a>
                          /
                          <a href="#"><i class="fas fa-user-slash red"></i></a>
                      </td>
                    </tr>


                  </tbody>
                </table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
          </div>
    <form @submit.prevent="createUser" >
            <div class="modal fade" id="addnew" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="addnew">Add new user</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
                </button>
            </div>
                <div class="modal-body">
                    <div class="form-group">
                        <input v-model="form.name" type="text" name="name"
                            placeholder="Enter your name"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                        <has-error :form="form" field="name"></has-error>
                    </div>

                    <div class="form-group">
                        <textarea v-model="form.bio" type="text" name="bio"
                            placeholder="Enter your bio"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
                        <has-error :form="form" field="bio"></has-error>
                    </div>

                    <div class="form-group">
                        <select name="type" v-model="form.type" type="type" id="type"

                            class="form-control" :class="{ 'is-invalid': form.errors.has('type') }">
                            <option value="">Select User Role</option>
                            <option value="admin">Admin</option>
                            <option value="user">user</option>
                            <option value="auther">auther</option>
                        </select>
                        <has-error :form="form" field="type"></has-error>
                    </div>

                    <div class="form-group">
                        <input v-model="form.email" type="email" name="email"
                            placeholder="Enter your Email"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                        <has-error :form="form" field="email"></has-error>
                    </div>


                    <div class="form-group">
                        <input v-model="form.password" type="password" name="password"
                            placeholder="Enter your password"
                            class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                        <has-error :form="form" field="password"></has-error>
                    </div>

            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                <button type="submit" class="btn btn-primary">Create</button>
            </div>

            </div>

  </div>


</div>
</form>
    </div>
</template>

<script>
    export default {

        data(){
            return{
                users : {},
                form : new Form({
                name: '',
                email: '',
                password:'',
                type:'',
                bio:'',
                photo:''
              })
            }
        },
        created() {
            this.loadusers();
        } ,

        methods :{
           createUser(){
               this.$Progress.start();
               this.form.post('api/user');
               $('#addnew').modal('hide')
               Toast.fire({
                icon: 'success',
                title: 'user created successfully'
                })
               this.$Progress.finish();

           },

           loadusers(){

               axios.get("api/user").then(({data}) => (this.users = data.data));

           }
        }
    }
</script>
