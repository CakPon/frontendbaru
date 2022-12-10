<template>
  <div>
    <header
      class="w-full bg-green-800 border-gray-200 px-2 sm:px-4 py-2.5 dark:bg-gray-900"
    >
      <div
        class="w-full container flex flex-wrap items-center justify-between mx-auto"
      >
        <a
          href="https://www.google.com/search?q=jadwal+piala+dunia+2022&rlz=1C1VDKB_enID1028ID1028&oq=jadwal+piala+dunia+2022&aqs=chrome.0.69i59j0i131i433i512l5j0i3l2j0i512j0i131i433i512.4312j0j7&sourceid=chrome&ie=UTF-8"
          class="flex items-center"
        >
          <img
            src="https://flowbite.com/docs/images/logo.svg"
            class="h-6 mr-3 sm:h-9"
            alt="Flowbite Logo"
          />
          <span
            class="self-center text-xl font-semibold whitespace-nowrap dark:text-white"
            >masang maroko</span
          >
        </a>
        <button
          data-collapse-toggle="navbar-default"
          type="button"
          class="inline-flex items-center p-2 ml-3 text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
          aria-controls="navbar-default"
          aria-expanded="false"
        >
          <span class="sr-only">Open main menu</span>
          <svg
            class="w-6 h-6"
            aria-hidden="true"
            fill="currentColor"
            viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              fill-rule="evenodd"
              d="M3 5a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 10a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 15a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z"
              clip-rule="evenodd"
            ></path>
          </svg>
        </button>
        <div class="hidden w-full md:block md:w-auto" id="navbar-default">
          <ul
            class="flex flex-col p-4 mt-4 border border-gray-100 rounded-lg md:flex-row md:space-x-8 md:mt-0 md:text-sm md:font-medium md:border-0 dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700"
          >
            <li>
              <a
                href="/dashboard"
                class="block py-2 pl-3 pr-4 text-white bg-blue-700 rounded md:bg-transparent md:text-white md:p-0 dark:text-white"
                aria-current="page"
                >Dashboard</a
              >
            </li>
            <li>
              <a
                href="/"
                class="text-white bg-green-700 hover:bg-green-800 focus:outline-none focus:ring-4 focus:ring-green-300 font-medium rounded-full text-sm px-5 py-2.5 text-center mr-2 mb-2 dark:bg-green-600 dark:hover:bg-green-700 dark:focus:ring-green-800"
                >Log Out</a
              >
            </li>
          </ul>
        </div>
      </div>
    </header>
    <div class="h-screen flex bg-gray-200">
      <div
        class="bg-gray-200 border border-green-500 w-full max-w-5xl m-auto p-4 bg-white rounded-lg shadow-md sm:p-6 md:p-8 flex items-center flex justify-center"
      >
        <form class="space-y-6">
          <h1
            class="text-xl font-medium text-gray-900 dark:text-white flex justify-center"
          >
            Edit Link
          </h1>
          <div class="max-w-sm flex flex-col mx-auto gap-y-4">
            <label
              for="oldLink"
              class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
              >Enter Link</label
            >
            <input
              type="text"
              ref="oldLink"
              :placeholder="this.links.find(link => link.id == this.ids).oldLink"
              class="flex w-full border-2 border-green-500 bg-gray-50 text-gray-900 text-sm rounded-lg block p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white"
            />
            <label
              for="newLink"
              class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
              >127.0.0.1:5173/p/</label
            >
            <input
              type="text"
              ref="newLink"
              :placeholder="this.links.find(link => link.id == this.ids).id"
              class="mx-auto border-2 border-green-500 bg-gray-50 text-gray-900 text-sm rounded-lg block w-full p-2.5 dark:bg-gray-600 dark:border-gray-500 dark:placeholder-gray-400 dark:text-white"
              required
            />
            <div class="mx-auto">
        <button
            @click="editLink(this.ids)"
            class="text-white bg-green-700 hover:bg-green-800 focus:outline-none focus:ring-4 focus:ring-green-300 font-medium rounded-full text-sm px-5 py-2.5 text-center mr-2 mb-2 dark:bg-green-600 dark:hover:bg-green-700 dark:focus:ring-green-800 flex "
          >
            Edit Link
        </button>
            <button
            @click="$router.push('/dashboard')"
            class="text-white bg-green-700 hover:bg-green-800 focus:outline-none focus:ring-4 focus:ring-green-300 font-medium rounded-full text-sm px-5 py-2.5 text-center mr-2 mb-2 dark:bg-green-600 dark:hover:bg-green-700 dark:focus:ring-green-800 flex justify-center"
          >
            Back
          </button>
  

          </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            ids: this.$route.params.id,
            links: [],
        }
    },
    methods: {
        async editLink(id){
            try {
                const res = await axios.post(`http://localhost:3000/${id}`,{
                    oldLink: this.$refs.oldLink.value,
                    newLink: this.$refs.newLink.value,
                    uid: localStorage.getItem("userToken"),
                    viewCount: this.links.find(link => link.id == this.ids).viewCount
                })
                console.log("Update Link")
                this.deleteLink()
                this.$router.push("/edit/"+this.$refs.newLink.value)
            }
            catch(err) {
                console.log(err)
                this.getLinks()
            }
        },
        
        async deleteLink(){
            try {
                const res = await axios.delete(`http://localhost:3000/${this.ids}`)
                console.log("delete")
                this.getLinks()
            }
            catch(err){
                console.log(err)
                this.getLinks()
            }
        },

        async getLinks(){
            this.links = []
            try {
                const res = await axios.get('http://localhost:3000/links',{
                    params: {uid: localStorage.getItem('userToken')}
                }) 
                .then((response)=>{
                    const links = response.data
                    this.links.push(...response.data)
                    console.log('Get Link')
                    // console.log(response)  
                })
            }
            catch(err){
                console.log(err)
                this.getLinks()
            }
        },

    },
    created() {
        this.getLinks()    
    }
}
</script>