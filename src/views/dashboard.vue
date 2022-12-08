<template>
    <div>
        <p>dashboard</p>
        <button @click="logOut()">Log out</button>
        <div>
          <label for="oldLink">Enter Link</label><br>
          <input type="text" v-model="oldLink" required><br>
        </div>
        <div>
          <label for="newLink">127.0.0.1:5173/p/</label><br>
          <input type="text" v-model="newLink" required><br>
        </div>
        <button @click="addLink(oldLink, newLink)">Short Your Link</button>
        <div v-for="link in links" :key="link">
            <p>{{link.newLink}}</p>
            <p>{{link.oldLink}}</p>
            <p>{{link.viewCount}}</p>
            <button @click="deleteLink(link.id)">Delete</button>
        </div>
        <!-- <p>Hi {{this.currentUser.email}}</p> -->
    </div>
</template>

<script>
import axios from 'axios'
  
export default {
  data() {
    return {
      currentUser: "",
      links: []
    }
  },
  methods: {
    async logOut() {    
        try {
            const res = await axios.get('http://localhost:3000/logout')
            console.log(res)
            localStorage.removeItem("userToken")
            this.$router.push("/")
        }
        catch (e) {
            console.log(e)
        }
    },
    async addLink(oldLink, newLink) {
      try{
        const res = await axios.post('http://localhost:3000/link',{
          oldLink: oldLink,
          newLink: "127.0.0.1:5173/p/" + newLink,
          uid: localStorage.getItem("userToken")
        })
      }
      catch(err){
        console.log(err)
      }
    },
    async getLinks(){
        try{
          const res = await axios.get('http://localhost:3000/links',{
            params: {uid: localStorage.getItem('userToken')}
          }) 
          .then((response)=>{
            const links = response.data
            this.links.push(...response.data)
            console.log('berhasil')
            console.log(response)  
          })
                
        }
        catch(err){
          console.log(err)
        }
    },
    async deleteLink(id){
      try {
        const res = await axios.delete(`http://localhost:3000/${id}`)
        .then((response)=>{
          const index = this.links.findIndex((k)=>{
            return k.id == id;
          })
          if(index == -1) return;
          this.links.splice(index,1)
        })
      }
      catch(err){
        console.log(err)
      }
    }
      
    },
    mounted() {
    this.getLinks()
      },
    
}
</script>