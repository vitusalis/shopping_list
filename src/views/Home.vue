<template>
        <div class="home">
            <header>
            <h1>SHOPPING LIST</h1>
        </header>
        <div id="app">
            <div class="form">
            <!-- NEW ITEM -->
                <input
                    id="newItem"
                    class="form-control"
                    type="text"
                    placeholder="ex: leite, ovos"
                    v-model="item.name"
                    @keyup.enter="change_focus('newItemAmmount')"
                />
            <div class="flex center">
                <input
                    id="newItemAmmount"
                    class="form-control"
                    type="number"
                    v-model="item.ammount"
                    placeholder="1"
                    @keyup.enter="addItem(item)"
                />
                <button id="addItem" class="btn btn-primary" @click="addItem(item)">
                    Adicionar
                </button>
            </div>

            </div>

            <!-- EMPTY LIST -->
            <div class="flex center faded" v-if="! computed_list || !computed_list.length">
                <h3>Nada na lista</h3>
            </div>

            <!-- LIST -->
            <ul v-else class="list-group">
                <!-- Completed -->
                <li v-for="item in computed_list" :key="item.name" class="list-group-item flex">
                    <input type="checkbox" v-model="item.completed" @change="set_list()"/>
                    <p :class="[item.completed ? 'crossed-off':'']">
                        {{item.name}} ({{item.ammount}})
                    </p>
                    <button
                        class="btn btn-primary bg-danger delete"
                        @click="removeItem(item)"
                    > &times; </button>
                </li>
                <!-- Incomplete -->
            </ul>
            <div class="actions">
                    <button id="showHidden" class="btn btn-info btn-full" @click="hide_completed = !hide_completed">
                            <span v-if="hide_completed">Esconder</span>
                            <span v-else>Mostrar</span>
                            intens completos
                    </button>
                    <button id="showHidden" class="btn btn-info btn-full" @click="clear_list">
                            Resetar lista
                    </button>
            </div>
        </div>
        </div>
</template>

<script>
export default {
    data(){
        return{
            shopping_list:[],
            item:{
                name:"",
                ammount:undefined
            },
            hide_completed:false

        }
    },
    methods:{
        set_list(){
            localStorage.setItem('shopping_list', JSON.stringify(this.shopping_list))
            },
        get_list(){
            const ls_list = JSON.parse(localStorage.getItem('shopping_list'))
            this.shopping_list = ls_list ? ls_list : [] 
            },
        addItem(item){
            if (!this.item.ammount)
                this.item.ammount = 1
            this.shopping_list = [{...item}, ...this.shopping_list]
            this.set_list();
            this.item.name = ""
            this.item.ammount = undefined
            this.change_focus('newItem')
            },
        removeItem(item){
            this.shopping_list = this.shopping_list.filter(i => i!= item)
            this.set_list();
            },
        clear_list(){
            this.shopping_list = this.shopping_list.map(item=> {return{...item, completed:false}})
            this.set_list();
        },
        change_focus(el_id){
            document.getElementById(el_id).focus();
            }
    },
    created(){
        this.get_list()
    },
    computed:{
        computed_list(){
            return this.hide_completed? this.shopping_list : this.shopping_list.filter(item => !item.completed);
        }
    }
    

}
</script>

<style>


</style>