<template>
<!-- We are using TailwindCss as a CSS framework -->

<div class="flex flex-col">
  
    <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
      <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
        <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
          <table class="min-w-full divide-y divide-gray-200">
            <thead class="bg-gray-50">
              <tr>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  ID
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  First Name
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Last Name
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Email Address
                </th>
                <th scope="col" class="relative px-6 py-3">
                  <span class="sr-only">Edit</span>
                </th>
                <th scope="col" class="relative px-6 py-3">
                  <span class="sr-only">Delete</span>
                </th>
              </tr>
              <tr>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  <input @change="searchById" placeholder="search by ID" type="text" class="focus:ring-indigo-500 focus:border-indigo-500 flex-1 block w-full rounded-none rounded-r-md sm:text-sm border-gray-300"/> 
                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  <input @change="searchByFirstName" placeholder="search by first name" type="text" class="focus:ring-indigo-500 focus:border-indigo-500 flex-1 block w-full rounded-none rounded-r-md sm:text-sm border-gray-300"/> 

                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  <input @change="searchByLastName" placeholder="search by last name" type="text" class="focus:ring-indigo-500 focus:border-indigo-500 flex-1 block w-full rounded-none rounded-r-md sm:text-sm border-gray-300"/> 

                </th>
                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  <input @change="searchByEmail" placeholder="search by email" type="text" class="focus:ring-indigo-500 focus:border-indigo-500 flex-1 block w-full rounded-none rounded-r-md sm:text-sm border-gray-300"/> 
                </th>
                <th scope="col" class="relative px-6 py-3">
                  <span class="sr-only">Edit</span>
                </th>
                <th scope="col" class="relative px-6 py-3">
                  <span class="sr-only">Delete</span>
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(person,personIdx) in persons" :key="person.id" :class="personIdx % 2 === 0 ? 'bg-white' : 'bg-gray-50'">
                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">
                  {{ person.id }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                  <!-- the fetched data comes with only the name value we need to split it to extract the first and the last name   -->

                  {{ person.name.split(' ')[0] }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                  {{ person.name.split(' ')[1] }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                  {{ person.email }}
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">

                  <!-- We are initializing data (setting default data) for the inputs that will be updated / edited  -->

                  <button @click="(show=!show,
                  editId=person.id,newEmail=person.email,
                  newlastName=person.name.split(' ')[1],
                  newFirstName=person.name.split(' ')[0])" 
                class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full">
                    Edit user

                  </button>
                </td>
                <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                  <button @click="()=>deleteUser(person.id)" class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded-full">
                    delete user
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div>
<form v-on:submit.prevent="onSubmit" class="space-y-8 divide-y divide-gray-200">
    <div class="space-y-8 divide-y divide-gray-200">


      <div class="pt-8">
        <div>
          <h3 class="text-lg leading-6 font-medium text-gray-900">
            Add a new person 
          </h3>

        </div>
        <div class="mt-6 grid grid-cols-1 gap-y-6 gap-x-4 sm:grid-cols-6">
          <div class="sm:col-span-3">
            <label for="first-name" class="block text-sm font-medium text-gray-700">
              First name
            </label>
            <div class="mt-1">
              <input v-model="firstName" type="text" name="first-name" id="first-name" autocomplete="given-name" class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md" />
            </div>
          </div>

          <div class="sm:col-span-3">
            <label  for="last-name" class="block text-sm font-medium text-gray-700">
              Last name
            </label>
            <div class="mt-1">
              <input v-model="lastName" type="text" name="last-name" id="last-name" autocomplete="family-name" class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md" />
            </div>
          </div>

          <div class="sm:col-span-4">
            <label for="email" class="block text-sm font-medium text-gray-700">
              Email address
            </label>
            <div class="mt-1">
              <input v-model="email" id="email" name="email" type="email" autocomplete="email" class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md" />
            </div>
          </div>

        </div>
      </div>
    </div>
    <div class="pt-5">
      <div class="flex justify-end">

        <button   type="submit" class="ml-3 inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          Add
        </button>
      </div>
    </div>
  </form>

    </div>
        <!-- Tailwind modal to show/hide the edit section  -->
      <vue-tailwind-modal
        :showing="show"
        @close="show=false"
        :showClose="true"
        :backgroundClose="true"
      >
        
        <form v-on:submit.prevent="editUser" class="space-y-8 divide-y divide-gray-200">
    <div class="space-y-8 divide-y divide-gray-200">


      <div class="pt-8">
        <div>
          <h3 class="text-lg leading-6 font-medium text-gray-900">
            Edit person 
          </h3>

        </div>
        <div class="mt-6 grid grid-cols-1 gap-y-6 gap-x-4 sm:grid-cols-6">
          <div class="sm:col-span-3">
            <label for="first-name" class="block text-sm font-medium text-gray-700">
              First name
            </label>
            <div class="mt-1">
              <input value="aaaaaaa"  v-model="newFirstName" type="text" name="first-name" id="first-name" autocomplete="given-name" class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md" />
            </div>
          </div>

          <div class="sm:col-span-3">
            <label  for="last-name" class="block text-sm font-medium text-gray-700">
              Last name
            </label>
            <div class="mt-1">
              <input v-model="newlastName" type="text" name="last-name" id="last-name" autocomplete="family-name" class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md" />
            </div>
          </div>

          <div class="sm:col-span-4">
            <label for="email" class="block text-sm font-medium text-gray-700">
              Email address
            </label>
            <div class="mt-1">
              <input v-model="newEmail" id="email" name="email" type="email" autocomplete="email" class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md" />
            </div>
          </div>

        </div>
      </div>
    </div>
    <div class="pt-5">
      <div class="flex justify-end">

        <button   type="submit" class="ml-3 inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          Edit
        </button>
      </div>
    </div>
  </form>
      </vue-tailwind-modal>
  </div>

</template>

<script>
import axios from 'axios';
import VueTailwindModal from 'vue-tailwind-modal'

export default {


  name: 'DataListing',
  components: {
	VueTailwindModal,
  },
  data: function () {

  return {
    //show is a variable to toggle the modal's state
    show: false,
    // lastName,firstName and email are variables used to add a new person
    lastName:'',
    firstName:'',
    email:'',
    //     editId,newEmail,newlastName & newFirstName are variables used to edit an existing person
    editId:'',
    newEmail:'',
    newlastName:'',
    newFirstName:'',
    // persons is an array that lists the fetched data 
    persons: []
  }
},
  created(){
    // fetching the list of persons with axios
    axios.get('http://jsonplaceholder.typicode.com/users')
    .then(res=>res.data)
    .then(data=>this.persons=data)
    .catch(e=>console.log(e))
  
  },
  methods:{
    // a function to add a new person to the list 
    onSubmit(){
      this.persons.push({id:Math.floor(Math.random()*100),name:this.firstName+' '+this.lastName,email:this.email})
    },
    // a function to search a person by ID
    searchById(e){
      this.persons=this.persons.filter(el=>el.id==e.target.value)
    },
    // a function to search a person by first name
    searchByFirstName(e){
      this.persons=this.persons.filter(el=>el.name.split(' ')[0].includes(e.target.value))
    },
    // a function to search a person by last name
    searchByLastName(e){
      this.persons=this.persons.filter(el=>el.name.split(' ')[1].includes(e.target.value))
    },
    // a function to search a person by email
    searchByEmail(e){
      this.persons=this.persons.filter(el=>el.email.includes(e.target.value))
    },
    // a function to delete a person from array 
    deleteUser(index){
      this.persons=this.persons.filter(el=>el.id!=index)
    },
    // a function to edit a person from array 

    editUser(){
      this.persons=this.persons.map(el=>el.id==this.editId ? {...el,email:this.newEmail,name:this.newFirstName+' '+this.newlastName} : el)
      this.show=false
    }
  }

}

</script>


