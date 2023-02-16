<template>
    <v-layout>
    <v-flex>
        <form action="post"  enctype="multipart/form-data" @submit.prevent="submitFiles()" >
        
            <v-file-input  ref="myfile" v-model="files" show-size counter chips multiple label="Arquivo Geral" name="image"></v-file-input>
    
    
      <v-btn color="primary" type="submit">test</v-btn>
    
        </form>
    </v-flex>
</v-layout>
</template>

<script>
export default {
    data () {
      return {
        files: null,
       data: [],
      }
    },
    
    methods: {
      async  submitFiles() {
        try {
                const formData = new FormData()
                formData.append('file', this.files[0])
              const res =  await this.$axios.post(`/upload/`, formData)
              this.data = res.data
              console.log(this.data)
            } catch (e) {
                // if (e.response.status === 403) {
                //     alert("Token Exception")
                //     this.$router.push('/login');
                // } else if (e.response.status === 401) {
                //     alert("Go to Login")
                //     this.$router.push('/login');
                // }
                console.log(e)
            }
}
    },
  }
</script>