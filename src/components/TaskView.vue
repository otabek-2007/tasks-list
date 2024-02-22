<template>
    <div class="container">
        <div class="header">
            <div class="header-box">
                <div class="header-items">
                    <h3>Welcome to VueDo</h3>
                    <div class="add-box">
                        <input v-model="text" placeholder="Add thing to do" type="text">
                        <button @click="add">Add</button>
                    </div>
                    <h4>Completed {{ to_do_list.filter((i) => i.checked).length }} of {{ to_do_list.length }} tasks</h4>
                </div>
            </div>
        </div>
        <!-- <small>
            <pre>{{ to_do_list.length }}</pre>
        </small> -->
        <div class="show-tasks">
            <div class="tasks-list">
                <template v-for="(item, i) in to_do_list">
                    <div class="tasks" :key="i">
                        <div class="task">
                            <input v-model="item.checked" type="checkbox" @click="check(item.id)">
                            <input :class="{ line: item.checked }" type="text" :disabled="item.disable" v-model="item.name">
                            <button v-if="item.disable" @click="editItem(item.id)" class="edit">edit</button>
                            <button v-else @click="saveEdit(item.id, item.name)" class="edit">save</button>
                            <button @click="deleteItem(item.id)" class="delete">delet</button>
                        </div>
                    </div>
                </template>
            </div>
        </div>
    </div>
</template>
<script>
export default {


    data() {
        return {
            text: "",
            checked: [],
            to_do_list: []
        }
    },
    mounted() {
        this.to_do_list = JSON.parse(this.$cookie.get('to_do_list')) ?? [];
    },
    methods: {
        editItem(id) {

            let editItem = this.to_do_list.find((item) => item.id === id)
            editItem.disable = false
            
            editItem.checked = false
        },
        check(id) {
            let item = this.to_do_list.find((item) => item.id === id)
            item.checked = !item.checked
            // for ( const item in this.to_do_list) {
            //     if(item){
            //         console.log(this.to_do_list[i]);
            //         console.log(item.checked == true);
            //         this.checked.push(this.to_do_list[i])
            //     }
            // }
            this.$cookie.set('to_do_list', JSON.stringify(this.to_do_list), 7);
        },
        saveEdit(id, text) {
            let editItem = this.to_do_list.find((item) => item.id === id)
            editItem.checked = false
            editItem.disable = true
            editItem.name = text
            this.$cookie.set('to_do_list', JSON.stringify(this.to_do_list), 7);

            // console.log( text);

        },
        deleteItem(id) {
            this.to_do_list.splice(this.to_do_list.findIndex((el) => el.id = id), 1)
            this.$cookie.set('to_do_list', JSON.stringify(this.to_do_list), 7);
        },
        add() {
            if (!this.text) return;

            const id = this.to_do_list.length == 0 ? 0 : Math.max.apply(null, this.to_do_list.map(i => i.id))

            const obj = {
                id: id + 1,
                name: this.text,
                checked: false,
                disable: true
            }

            this.to_do_list.push(obj);
            this.text = ""

            this.$cookie.set('to_do_list', JSON.stringify(this.to_do_list), 7);
        },
    }
}
</script>
<style lang="scss" scoped>
.container {
    display: flex;
    justify-content: center;
    width: 100%;
    height: 100%;
    position: absolute;
    flex-direction: column;
    left: 0px;
    top: 0px;

    .header {
        height: 220px;
        width: 100%;
        display: flex;
        justify-content: center;
        background-color: green;

        padding: 20px 0px;

        .header-box {
            width: 50%;
            height: 100%;

            .header-items {
                width: 100%;
                height: 100%;
                text-align: center;

                h3 {
                    font-size: 40px;
                    font-weight: bold;
                    font-family: sans-serif;
                    color: white;
                }

                h4 {
                    font-size: 15px;
                    font-weight: bold;
                    font-family: sans-serif;
                    color: white;
                }

                .add-box {
                    input {
                        outline: none;
                        border: 0px;
                        padding: 0px 10px;
                        width: 500px;
                        height: 40px;
                    }

                    button {
                        border: 0px;
                        width: 200px;
                        font-size: 12px;
                        padding: 13px;
                    }

                }
            }
        }
    }

    .show-tasks {
        height: 100vh;
        padding: 20px 0px;
        width: 100%;
        background-color: rgb(131, 215, 131);
        display: flex;
        justify-content: center;

        .tasks-list {
            width: 60%;
            height: 100%;

            .tasks {
                width: 100%;

                // height: 100%;
                display: flex;
                justify-content: center;
                flex-direction: column;
                align-items: center;

                .task {
                    margin-top: 20px;
                    padding: 10px 20px;
                    display: flex;
                    box-shadow: 10px 10px 10px green;
                    border-top-left-radius: 30px;
                    border-bottom-right-radius: 30px;
                    justify-content: space-around;
                    background-color: rgb(155, 234, 155);

                    .line {
                        text-decoration: line-through;
                        color: red;

                    }

                    input {
                        padding: 10px;
                        border: 0px;
                        outline: none;
                        background-color: rgb(131, 215, 131);

                        // background-color: green;
                    }

                    button {
                        border: 0px;
                        cursor: pointer;
                        margin: 5px;
                        color: aliceblue;
                        padding: 5px;
                    }

                    .edit {
                        background-color: blue;
                    }

                    .delete {
                        background-color: red;
                    }
                }
            }
        }
    }
}
</style>