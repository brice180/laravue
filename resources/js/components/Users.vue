<template>
    <div class="container">
        <div class="row mt-5">
          <div class="col-md-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Lista de usuarios</h3>

                <div class="card-tools">
                  <button class="btn btn-success" data-toggle="modal" data-target="#agregaUser">Agregar Usuario
                      <i class="fas fa-user-plus"></i>
                  </button>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover text-nowrap">
                  <thead>
                    <tr>
                      <th>ID</th>
                      <th>Nombre</th>
                      <th>Email</th>
                      <th>Tipo</th>
                      <th>Fecha de Creación</th>
                      <th>Modificar</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="user of users" :key="user.id">
                      <td>{{ user.id}}</td>
                      <td>{{ user.name }}</td>
                      <td>{{ user.email }}</td>
                      <td><span class="tag tag-success">{{ user.tipo | upText }}</span></td>
                      <td>{{ user.created_at | miFecha }}</td>
                      <td>
                          <a href="#">
                              <i class="fa fa-edit blue"></i>
                          </a>
                            /
                          <a href="#">
                              <i class="fa fa-trash red"></i>
                          </a>
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
    <!-- Modal -->
      <div class="modal fade" id="agregaUser" tabindex="-1" role="dialog" aria-labelledby="agregaUserLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="agregaUserLabel">Agregar Nuevo Usuario</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            
            <form @submit.prevent="crearUsuario">

            <div class="modal-body">
                <div class="form-group">
                <input v-model="form.name" type="text" name="name" placeholder="Ingrese Nombre"
                  class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                <has-error :form="form" field="name"></has-error>
              </div>

              <div class="form-group">
                <input v-model="form.email" type="email" name="email" placeholder="Correo Electrónico"
                  class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                <has-error :form="form" field="email"></has-error>
              </div>

              <div class="form-group">
                <textarea v-model="form.bio" type="textarea" name="bio" id="bio" placeholder="Descripción (Opcional)"
                  class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }">
                </textarea>
                <has-error :form="form" field="bio"></has-error>
              </div>

              <div class="form-group">
                <select v-model="form.tipo" type="text" name="tipo" id="tipo" 
                  class="form-control" :class="{ 'is-invalid': form.errors.has('tipo') }">
                  <option value="">Seleccione Rol</option>
                  <option value="admin">Administrador</option>
                  <option value="usuario">Usuario</option>
                  <option value="alumno">Alumno</option>
                </select>
                <has-error :form="form" field="tipo"></has-error>
              </div>

              <div class="form-group">
                <input v-model="form.password" type="password" name="password" placeholder="Contraseña"
                  class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                <has-error :form="form" field="password"></has-error>
              </div>

            </div>

            <div class="modal-footer">
              <button type="button" class="btn btn-danger" data-dismiss="modal">Cerrar</button>
              <button type="submit" class="btn btn-primary">Crear Usuario</button>
            </div>

          </form>

          </div>
        </div>
      </div>

    </div>
</template>

<script>
    export default {
        data () {
          return {

            users: {},
            // Create a new form instance
            form: new Form({
              name: '', 
              email: '', 
              password: '',
              tipo: '', 
              bio: '',
              foto: '',  
              remember: false
            })
          }
        },

        methods: {
          
            cargarUsuarios(){
              this.$Progress.start()
              axios.get("api/user").then(({ data }) => (this.users = data.data));
              this.$Progress.finish()
            },

            crearUsuario () {
              // Submit the form via a POST request
              //this.$Progress.start()
              this.form.post('/api/user');
              $('#agregaUser').modal('hide');
              Toast.fire({
                icon: 'success',
                title: 'Usuario creado satisfactoriamente'
              })
              //this.$Progress.finish()
            },
            
        },

        created() {
            this.cargarUsuarios();
        },
    }
</script>
