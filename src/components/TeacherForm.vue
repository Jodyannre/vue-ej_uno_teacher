<template>
    <section>
        <h3>Añadir profesor</h3>
        <div> <label>Nombre:</label> <input type="text" v-model="teacher.teacherName"></div>
        <div> <label>Apellido:</label> <input type="text" v-model="teacher.surname"></div>
        <div> <label>DNI:</label> <input type="text" v-model="teacher.dni"></div>
        <div> <label>Materias:</label> <input type="text" v-model="subject"> <button v-on:click="handleSubject()">Agregar</button></div>
        <div>
            <ul>
                <li v-for="(elm,index) in teacher.subjects" :key="index">{{elm}}</li>
            </ul>
        </div>
        <input type="checkbox" v-model="teacher.doc"> Documentación entregada
        <button v-show="!edit" @click="handleAddTeacher()">Agregar</button>  
        <button v-show="edit" @click="handleEditTeacher()">Guardar</button>
    </section>

    <section>
        <h3>Lista de profesores</h3>
        <table>
            <tr>
                <th>Nombre</th>
                <th>Apellido</th>
                <th>DNI</th>
                <th>Materias</th>
                <th>Documentación entregada</th>
                <th>Acciones</th>
            </tr>
            <tr v-for="(elm, index) in teachers" :key="index">
                <th>{{elm.teacherName}}</th>
                <th>{{elm.surname}}</th>
                <th>{{elm.dni}}</th>
                <th>
                    <ul>
                        <li v-for="(subject,index) in elm.subjects" :key="index">{{subject}}</li>
                    </ul>
                </th>
                <th v-if="elm.doc"> Documentación entregada </th>
                <th v-else> Documentación no entregada </th>
                <th>
                    <button @click="activateEdit(index)">Editar</button>
                </th>
            </tr>
        </table>
    </section>
</template>
  
<script lang="ts" setup>
    import {Ref, ref} from 'vue';
    interface ITeacher {
        teacherName: string;
        surname: string;
        dni: string;
        subjects: string[];
        doc: boolean;
    }

    let teacher: Ref<ITeacher> = ref({
        teacherName: '',
        surname: '',
        dni: '',
        subjects: [],
        doc: false
    });

    let teachers: Ref<Array<ITeacher>> = ref([]);

    let subject: Ref<string> = ref('');

    let edit: Ref<boolean> = ref(false);

    let index: Ref<number> = ref(0);

    const handleSubject = () => {
        teacher.value.subjects.push(subject.value);
        subject.value = '';
    }

    const handleAddTeacher = () => {
        teachers.value.push({
            teacherName: teacher.value.teacherName,
            surname: teacher.value.surname,
            dni: teacher.value.dni,
            subjects: teacher.value.subjects,
            doc: teacher.value.doc
        });
        cleanTeacher();
    }

    const handleEditTeacher = () => {
        teachers.value[index.value] = {
            teacherName: teacher.value.teacherName,
            surname: teacher.value.surname,
            dni: teacher.value.dni,
            subjects: teacher.value.subjects,
            doc: teacher.value.doc
        };
        cleanTeacher();
        edit.value = false;
    }

    const activateEdit = (inx: number) => {
        teacher.value = {
            teacherName: teachers.value[inx].teacherName,
            surname: teachers.value[inx].surname,
            dni: teachers.value[inx].dni,
            subjects: teachers.value[inx].subjects,
            doc: teachers.value[inx].doc
        };
        edit.value = true;
        index.value = inx;
    }

    const cleanTeacher = () => {
        teacher.value = {
            teacherName: '',
            surname: '',
            dni: '',
            subjects: [],
            doc: false
        };
    }

</script>
  
<style>
</style>
  