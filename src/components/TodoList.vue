<template>
    <section>
        <transition-group name ="list" tag="ul">
            <li v-for="(item,index) in propsdata" :key = "item['t_key']" class="shadow">
                <i class="checkBtn fas fa-check" aria-hidden="true"></i>
                <span v-if="edit_num === index && edit_state === true"><input type="text" v-model="edit_text" @keyup.enter="updateTodo(index)"></span>
                <span v-else>{{item['item']}}</span>
                <span class="removeBtn" type="button" @click="removeTodo(index)">
                    <i class="far fa-trash-alt" aria-hidden="true"></i>
                </span>
                <span class="editBtn" type="button" @click="updateTodo(index)">
                    <i class ="fas fa-edit" aria-hidden="true"></i>
                </span>

            </li>
        </transition-group>
    </section>
</template>


<script>
export default {
    props:['propsdata'],
    methods: {
        removeTodo(index){
            this.$emit("removeTodo", index)
        },
        updateTodo(index) {
            if( this.edit_state === true) {
                // edit num => 인덱스올리고
                // edit_text => 고칠 내용 올리고 
                // edit_state = false로 만듬
                this.$emit('updateTodo', this.edit_num, this.edit_text)
            } 
            else {
                this.edit_text = this.propsdata[index]['item'];
            }
            this.edit_num = index;
            
            this.edit_state = !this.edit_state
        },
    },
    data() {
        return {
            edit_num : -1,
            edit_state: false,
            edit_text: '',
        }
    },
}
</script>

<style scoped>
    ul{
        list-style-type: none;
        padding-left: 0px;
        margin-top: 0;
        text-align: right;
    }

    li {
        display: flex;
        min-height: 50px;
        height: 50px;
        margin: 0.5rem 0;
        padding: 0 0.9rem;
        background: white;
        border-radius: 5px;
        align-items: center;
    }

    .checkBtn {
        line-height: 45px;
        color: #62acde;
        margin-right: 5px;
    }

    .removeBtn {
        margin-left: auto;
        color: #de4343;
    }
    .list-enter-active, .list-leave-active {
        transition: all 1s;
    }
    .list-enter, .list-leave-to {
        opacity: 0;
        transform: translateY(30px);
    }
    .far {
        margin-right: 10px;
    }
</style>