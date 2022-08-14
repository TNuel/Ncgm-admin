<template>

  <div>
    <input type="text" v-model="search" placeholder="search "/>
       <div class="overflow-x-auto w-screen h-screen shadow-md bg-white">
          <table class="table-auto text-center w-full lg:text-left text-gray-50 dark:text-gray-400">
            <thead class="border-b border-purple-200 bg-gray-400 text-left">
              <tr class="bg-primary">
                <!-- <th scope="col" class="lg:py-3 lg:px-6"></th> -->
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">Id</th>
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">First Name</th>
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">Middle Name</th>
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">Surname</th>
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">Phone Number</th>
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">Gender</th>
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">Section</th>
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">State</th>
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">District</th>
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">Assembly</th>
                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">Status</th>

                <th scope="col" class="text-sm font-bold text-gray-900 px-6 py-4">Action</th>
              </tr>
            </thead>
            <tbody>
              <template v-if="state.isloading">
                <div class="bg-gray-400 text-white text-xl" > Loading...</div>
              </template>
              <template v-else>
                <tr
                v-for="user in state.users"
                :key="user"
                class="bg-gray-100 dark:bg-gray-900 text-xs lg:text-xl dark:border-gray-700"
              >
                <td
                  class="text-center text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{user.ID}}</td>
                <td
                  class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{ user.surname }}</td>
                <td
                  class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{ user.middlename }}</td>
                <td
                  class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{ user.firstname }}</td>
                <td
                  class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{ user.phone_number }}</td>
                <td
                  class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{ user.gender }}</td>
                <td
                  class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{ user.section }}</td>
                <td
                  class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{ user.state }}</td>
                <td
                  class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{ user.district }}</td>
                <td
                  class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{ user.assembly }}</td>
                <td
                  class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                >{{ user.status }}</td>

                <td class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap">
                  <button
                    class="font-medium hover:bg-purple-400 bg-secondary text-tertiary rounded-lg focus:outline-none py-2 px-3"
                    @click="updateStatus(user.ID)"
                  >Review</button>
                </td>
              </tr>
              </template>
            </tbody>
          </table>
        </div>
  </div>

</template>



<script>
import { reactive } from "vue";
import axios from "axios";


export default {

  setup() {
    
    const state = reactive({
      users: [],
      updateUser : false,
      status: "",
      isloading: true
    })
    


      const getUsers = async () => {
        state.isloading = true
      try {

        const response = await axios.get(`http://localhost:4000/api/users`);
         console.log(response);
        
        const Users = response.data;
        console.log(Users);
        state.users = Users

        state.isloading = false
      } catch (error) {}
    };

    
      const updateStatus = async (id) => {
        state.isloading = true
        try {
          const response = await axios.put(`http://localhost:4000/api/users/update/${id}`);
          console.log(response.data.status);
          const update = response;
          state.updateUser = update;
          state.status = response.data.status;
          
          getUsers();
          state.isloading = false
          
        
        }catch (error) {

        }
      };
      //  watch(updateStatus(),getUsers())
    return {
      state,
      getUsers,
      updateStatus,
      
    }
  },
   mounted() {
    // this.getItem();
    this.getUsers()
    // this.updateStatus()
  },
 
 
}
</script>

<style>

</style>