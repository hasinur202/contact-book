<template>
    <div class="modal" :class="openmodal">
  <div class="modal-background"></div>
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Update {{ list.name }}'s Details</p>
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
      <button @click="update" class="button is-success">Update</button>
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
            list:'',
            errors:{},
        }
    },


    methods:{
        close(){
            this.$emit('closeRequest')
            //Object.assign(this.$data, this.$options.data.apply(this))
            //here is a bug,that's why we use router refresh
            this.$router.go(0)
        },
        update(){
            axios.patch(`/phonebook/${this.list.id}`,this.$data.list).then((response)=>this.close())
            .catch((error)=>this.errors = error.response.data.errors)
        }
    }
}
</script>