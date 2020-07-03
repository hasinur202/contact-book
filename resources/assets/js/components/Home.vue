<template>
<div>
    <nav class="panel column is-offset-2 is-8">
    <p class="panel-heading">
        Vuejs Phonebook
        <button class="button is-link is-outlined" @click="openAdd">
        Add New
        </button>
        <span class="is-pulled-right" v-if="loading">
            <i class="fa fa-refresh fa-spin fa-2x fa-fw"></i>
        </span>
    </p>

    <div class="panel-block">
        <p class="control has-icons-left">
        <input class="input" type="text" placeholder="Search" v-model="searchQuery">
        <span class="icon is-left">
            <i class="fa fa-search" aria-hidden="true"></i>
        </span>
        </p>
    </div>

    <a class="panel-block is-active" v-for="item, key in temp">
        <span class="column is-9">
            {{ item.name }}
        </span>

        <span class="has-text-danger panel-icon column is-1">
        <i @click="del(key,item.id)" class="fa fa-trash" aria-hidden="true"></i>
        </span>

        <span class="has-text-info panel-icon column is-1">
        <i @click="openEdit(key)" class="fa fa-edit" aria-hidden="true"></i>
        </span>

        <span class="has-text-primary panel-icon column is-1">
        <i @click="openShow(key)" class="fa fa-eye" aria-hidden="true"></i>
        </span>

    </a>
    </nav>

    <Add @closeRequest='closeAdd' :openmodal='addActive'></Add>
    <Edit @closeRequest='closeAdd' :openmodal='editActive'></Edit>
    <Show @closeRequest='closeAdd' :openmodal='showActive'></Show>

</div>

</template>

<script>
let Add = require('./Add.vue');
let Edit = require('./Edit.vue');
let Show = require('./Show.vue');
export default {
    components:{Add, Edit, Show},
    data(){
        return {
            addActive: '',
            showActive: '',
            editActive:'',
            lists:{},
            errors:{},
            loading:false,
            searchQuery:'',
            temp:''
        }
    },

    watch:{
        searchQuery(){
            if(this.searchQuery.length > 0){
                this.temp = this.lists.filter((item) => {
                    return Object.keys(item).some((key)=>{
                        let string = String(item[key])
                        return string.toLowerCase().indexOf(this.searchQuery.toLowerCase())>-1
                    })
                });
            }else{
                this.temp = this.lists
            }
        }
    },

    mounted(){
        axios.post('/getData').
        then((response)=> this.lists = this.temp = response.data)
        .catch((error)=>this.errors = error.response.data.errors)
    },

    methods:{
        openAdd(){
            this.addActive = 'is-active';
        },

        closeAdd(){
            this.addActive = this.showActive = this.editActive = ''
        },

        openEdit(key){
            this.$children[1].list = this.temp[key];
            this.editActive = 'is-active';
        },
        openShow(key){
            this.$children[2].list = this.temp[key];
            this.showActive = 'is-active';
        },
        del(key,id){
            if(confirm("Are you sure ?")){
                this.loading = !this.loading;
                axios.delete(`/phonebook/${id}`).
                then((response)=> {this.lists.splice(key, 1);this.loading = !this.loading;})
                .catch((error)=>this.errors = error.response.data.errors)
            }
        }
    }
}
</script>