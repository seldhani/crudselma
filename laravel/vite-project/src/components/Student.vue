<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-10">
        <h3 class="text-center text-dark mt-2">CRUD Tutor Cairocoders <3</h3>
      </div>
      <br>
    </div>
    <div class="row">
      <div class="col-md-4">
        <div class="card"></div>
        <br>
        <div class="card text-center">
          <div class="card-body card-bordered">
            <form @submit.prevent="save">
              <div class="form-floating">
                <label>Student name</label>
                <input type="text" v-model="student.name" class="form-control" placeholder="Student name" style="height: 100px">
              </div>
              <div class="form-floating">
                <label>Student Address</label>
                <input type="text" v-model="student.address" class="form-control" placeholder="Student Address" style="height: 100px">
              </div>
              <div class="form-floating">
                <label>Phone</label>
                <input type="text" v-model="student.phone" class="form-control" placeholder="Phone" style="height: 100px">
              </div>
              <br>
              <button type="submit" class="btn btn-outline-primary">Save</button>
            </form>
          </div>
        </div>
      </div>
      <div class="col-md-8">
        <h2></h2>
        <div class="mb-2">
          <label><input type="checkbox" v-model="selectedColumns" value="id"> ID</label>
          <label><input type="checkbox" v-model="selectedColumns" value="name"> Student Name</label>
          <label><input type="checkbox" v-model="selectedColumns" value="address"> Address</label>
          <label><input type="checkbox" v-model="selectedColumns" value="phone"> Phone</label>
        </div>
        <button @click="generatePDF" class="btn btn-outline-secondary mb-2">Download PDF</button>
        <table class="table table-striped table-dark table-hover">
          <thead>
            <tr>
              <th scope="col">ID</th>
              <th scope="col">Student Name</th>
              <th scope="col">Address</th>
              <th scope="col">Phone</th>
              <th scope="col">Option</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="student in result" :key="student.id">
              <td>{{ student.id }}</td>
              <td>{{ student.name }}</td>
              <td>{{ student.address }}</td>
              <td>{{ student.phone }}</td>
              <td>
                <button type="button" class="btn btn-outline-light" @click="edit(student)">Edit</button>
                <button type="button" class="btn btn-outline-danger" @click="remove(student)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import jsPDF from 'jspdf';
import 'jspdf-autotable';

export default {
  name: 'Student',
  data() {
    return {
      result: [],
      student: {
        id: '',
        name: '',
        address: '',
        phone: ''
      },
      selectedColumns: ['id', 'name', 'address', 'phone']
    }
  },
  created() {
    this.StudentLoad();
  },
  mounted() {
    console.log("mounted() called.......");
  },
  methods: {
    StudentLoad() {
      var page = "http://127.0.0.1:8000/api/student";
      axios.get(page)
        .then(
          ({ data }) => {
            console.log(data);
            this.result = data;
          }
        );
    },
    save() {
      if (this.student.id == '') {
        this.saveData();
      } else {
        this.updateData();
      }
    },
    saveData() {
      axios.post("http://127.0.0.1:8000/api/student", this.student)
        .then(
          ({ data }) => {
            alert("Data Saved");
            this.StudentLoad();
            this.student.name = '';
            this.student.address = '';
            this.student.phone = '';
            this.student.id = '';
          }
        )
    },
    edit(student) {
      this.student = student;
    },
    updateData() {
      var editrecords = 'http://127.0.0.1:8000/api/student/' + this.student.id;
      axios.put(editrecords, this.student)
        .then(
          ({ data }) => {
            this.student.name = '';
            this.student.address = '';
            this.student.phone = '';
            this.student.id = '';
            alert("Data Updated");
            this.StudentLoad();
          }
        );
    },
    remove(student) {
      var url = `http://127.0.0.1:8000/api/student/${student.id}`;
      axios.delete(url)
        .then(() => {
          alert("Data Deleted");
          this.StudentLoad();
        });
    },
    generatePDF() {
      const doc = new jsPDF();
      let y = 10;
      doc.text("Student List", 10, y);
      y += 10;
      const columns = this.selectedColumns.map(column => {
        switch (column) {
          case 'id':
            return 'ID';
          case 'name':
            return 'Name';
          case 'address':
            return 'Address';
          case 'phone':
            return 'Phone';
          default:
            return '';
        }
      });
      const data = this.result.map(student => {
        return this.selectedColumns.map(column => student[column]);
      });
      doc.autoTable({
        startY: y,
        head: [columns],
        body: data
      });
      doc.save('students.pdf');
    }
  }
}
</script>