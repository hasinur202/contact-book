<template>
<div class="modal" :class="openmodal">
  <div class="modal-background"></div>
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Add Contact</p>
      <button @click="close" class="delete" aria-label="close"></button>
    </header>
    <section class="modal-card-body">
        <div class="field">
            <label class="label">Name</label>
            <div class="control">
                <input v-model="list.name" class="input" :class="{ 'is-danger':errors.name }" type="text" placeholder="Name">
            </div>
            <small v-if="errors.name" class="has-text-danger">{{ errors.name[0] }}</small>
        </div>
        <div class="field">
            <label class="label">Phone</label>
            <div class="control">
                <input v-model="list.phone" class="input" :class="{ 'is-danger':errors.phone }" type="number" placeholder="Phone">
            </div>
            <small v-if="errors.phone" class="has-text-danger">{{ errors.phone[0] }}</small>
        </div>
        <div class="field">
            <label class="label">Email</label>
            <div class="control">
                <input v-model="list.email" class="input" :class="{ 'is-danger':errors.email }" type="email" placeholder="Email">
            </div>
            <small v-if="errors.email" class="has-text-danger">{{ errors.email[0] }}</small>
        </div>

    </section>
    <footer class="modal-card-foot">
      <button @click="save" class="button is-success">Save</button>
      <button @click="close" class="button">Cancel</button>
    </footer>
  </div>
</div>
</template>

<script>
export default {

    props:['openmodal'],
    data(){
        return {
            list:{
                name:'',
                phone:'',
                email:'',
            },
            errors:{},
    
        }
    },
    
    methods:{
        close(){
            this.$emit('closeRequest')
            Object.assign(this.$data, this.$options.data())
        },
      
        
        save(){
            axios.post('/phonebook',this.$data.list).then((response)=>{
                this.close()
                
                this.$parent.lists.push(response.data)
                this.$parent.lists.sort(function(a,b){
                    if(a.name > b.name){
                        return +1;
                    }else if(a.name < b.name){
                        return -1;
                    }
                })
                //after 4 hours i can solve this problem, problem is i cann't add new after only once
                Object.assign(this.$data, this.$options.data())
                })
            .catch((error)=>this.errors = error.response.data.errors)
        },
        
      
    }
}
</script>