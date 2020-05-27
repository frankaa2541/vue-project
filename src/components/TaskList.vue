<template>
    <div class="container">
        <div class="task-zone">
            <div class="drop-zone" @drop="OnDrop($event, 'todo')" @dragenter.prevent @dragover.prevent>
                <h1>To-Do</h1>
                <div class="drag-el" draggable @dragstart="onStart($event,task)" v-for="task in todoList" :key="task.id">
                    {{task.title}}</div>
            </div>
            <div class="drop-zone" @drop="OnDrop($event, 'doing')" @dragenter.prevent @dragover.prevent>
                <h1>Doing</h1>
               <div class="drag-el" draggable @dragstart="onStart($event,task)" v-for="task in doinglist" :key="task.id">
                    {{task.title}}</div>
            </div>
            <div class="drop-zone" @drop="OnDrop($event, 'done')" @dragenter.prevent @dragover.prevent>
                <h1>Done</h1>
               <div class="drag-el" draggable @dragstart="onStart($event,task)" v-for="task in doneList" :key="task.id">
                    {{task.title}}</div>
            </div>
        </div>
    </div>
</template>>

<script>
export default {
    name: 'TaskList',
    data(){
        return{
            tasks:[
                {
                    id:1,
                    title:'Item A',
                    status:'todo'
                },
                {
                    id:2,
                    title:'Item B',
                    status:'todo'
                },
                {
                    id:3,
                    title:'Item C',
                    status:'doing'
                },
                {
                    id:4,
                    title:'Item D',
                    status:'done'
                }
            ]
        }
    },
    computed:{
        todoList(){          


            return this.tasks.filter(task => task.status == "todo")
        },
        doinglist(){
           return this.tasks.filter(task => task.status == "doing") 
        },
        doneList(){
            return this.tasks.filter(task => task.status == "done")
        }
    },
    methods:{
        onStart(e,task){
            e.dataTransfer.dropEffect = "move"
            e.dataTransfer.effectAllowed = "move"
            e.dataTransfer.setData('taskId',task.id)
            
        },
        OnDrop(e,newStatus){
        const taskId = e.dataTransfer.getData('taskId')
        const task = this.tasks.find(task => task.id == taskId)
        task.status = newStatus   
        }
           
    }
}
</script>>


<style scoped>
    .container{ /*กรอบใหญ่สุดจะไม่มีเส้น*/
        margin: 30px 0;
        border: 1px solid black;
    }
    .task-zone{
        display: flex; /*ขยับช่องให้เป็นในแนวนอน*/
        justify-content: space-around;  /* ช่องว่างรอบกรอบ*/
    }
    .drop-zone{
        border: 1px solid black;
        width: 250px; /*กว้าง*/
        height: 400px; /*ยาว*/
        padding: 10px 0; /*padding คือช่องว่างภายในกรอบ 0 คือ ช่องว่างรอบๆ*/
    }
    .drag-el{
        border: 1px solid black;
        width: 200px;
        height: 40px;
        margin: 5px auto;
        padding-top: 20px;

    }
</style>
