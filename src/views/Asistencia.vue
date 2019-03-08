<template>
    <div>
        <span>{{ asistencia.firstname }} {{ asistencia.lastname }}</span>
        <v-form v-model="valid">
            <v-container>
                <v-layout>
                    <v-flex xs12 md4>
                        <v-text-field
                                v-model="asistencia.firstname"
                                :rules="nameRules"
                                :counter="10"
                                label="First name"
                                required></v-text-field>
                    </v-flex>

                    <v-flex xs12 md4>
                        <v-text-field
                                v-model="asistencia.lastname"
                                :rules="nameRules"
                                :counter="10"
                                label="Last name"
                                required></v-text-field>
                    </v-flex>

                    <v-flex xs12 md4>
                        <v-text-field
                                @keyup.enter="presionarEnter"
                                v-model="asistencia.email"
                                :rules="emailRules"
                                label="E-mail"
                                required
                        ></v-text-field>
                    </v-flex>
                </v-layout>
            </v-container>

            <v-btn @click.prevent="save" color="success">Guardar</v-btn>
        </v-form>

        <!--<table class="table">
            <tr><td>nombre</td></tr>
            <tr v-for="asis in asistencias">
                <td>{{ asis.name }}</td>
            </tr>
        </table>-->
        <!--<v-data-table :headers="headers" :items="asistencias" class="elevation-1">
            <template v-slot:items="props">
                <td>{{ props.item.name }}</td>
            </template>
        </v-data-table>-->

        <pre>{{ $data.asistencia }}</pre>
        <pre>{{ asistencias }}</pre>
    </div>
</template>

<script>
    import {db} from './../firebase'
    export default {
        name: "asistencia",
        data: () => ({
            valid: false,
            asistencia: {
                firstname: '',
                lastname: '',
                email: '',
            },
            asistencias: [],
            nameRules: [
                v => !!v || 'Name is required',
                v => v.length <= 10 || 'Name must be less than 10 characters'
            ],
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /.+@.+/.test(v) || 'E-mail must be valid'
            ],
            headers: [
                {
                    text: 'Nombres'
                },
            ]
        }),
        created(){
          this.show()
        },
        methods: {
            show() {
                this.asistencias = db.collection('asistance').get().then(function(results) {
                    if(results.empty) {
                        console.log("No documents found!");   
                  } else {
                        // go through all results
                        console.log(results);
                        results.forEach(function (doc) {
                        console.log("Document data:", doc.data());
                    });

                    // or if you only want the first result you can also do something like this:
                    console.log("Document data:", results.docs[0].data());
                  }
                }).catch(function(error) {
                    console.log("Error getting documents:", error);
                });

                console.log(db.collection('asistance').get());
            },
            presionarEnter(){
                alert('presiono enter');
            },
            save(){
                alert(this.asistencia.firstname)
                alert(this.asistencia.lastname)
                let vm = this;
                db.collection('asistance').add({
                    name: vm.asistencia.firstname,
                });
                this.reset()
            },
            reset(){
                this.asistencia.firstname=''
                this.asistencia.lastname=''
                this.asistencia.email=''
            }
        }
    }
</script>

<style scoped>

</style>