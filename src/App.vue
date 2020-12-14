<template>
  <div id="app">
    <h1>Tomonky todoApp</h1>
    <div>
      <label for="title">Todo：</label>
      <input type="text" v-model="title" placeholder="What's your plan today?"/>
    </div>
    <div>
      <label for="body">Content：</label>
      <textarea v-model="body" placeholder="describe the specific"></textarea>
    </div>
    <div>
      <input type="submit" value="追加" v-on:click="addList" />
    </div>
    <ul>
      <li v-for="(list, i) in lists" v-bind:key="i">
        <label v-bind:class="{ done: list.isChecked }">
          <input type="checkbox" v-model="list.isChecked" v-on:change="saveTodos" />
          id: {{ i }}
          Todo: {{list.title}}<br />
          Content: {{ list.body }}
        </label>
        <button v-on:click="deleteList(i)">削除</button>
      </li>
    </ul>
    <!-- ***データ格納確認用***
     <pre>
      {{ $data }}
    </pre> -->
  </div>
</template>

<script>
  export default {
    data: () => ({
        lists: [
          {title:'mytodo', body: 'contentcontent', isChecked: true},
          {title: 'hogehoge', body: 'wooooow', isChecked: false}
        ],
        title: '',
        body: ''
      }),
      methods: {
        addList: function() {
          if(this.title === '' || this.body === '') return
          this.lists.push({
            title: this.title,
            body: this.body,
            isChecked: false
          });
          this.title = '';
          this.body = '';
          this.saveTodos();
        },
        deleteList: function(i){
          this.lists.splice(i,1);
          this.saveTodos();
        },
        saveTodos: function() {
          // localStorage.title = this.lists.title;
          // localStorage.body = this.lists.body;
          localStorage.setItem('lists', JSON.stringify(this.lists));
          console.log('now I am pretending I did more stuff.....');
        },
        loadTodos: function() {
          if(!this.lists){
            this.lists= []
            } else { this.lists = JSON.parse(localStorage.getItem('lists'))
            }
          }
        },
      mounted: function(){
        this.loadTodos();
      }
      };
</script>

<style>
.done {
  text-decoration: line-through;
}

#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

ul {
  display: inline-flex;
  justify-content: left;
  flex-direction: column;
  height: 55cdvh;
  margin-top: 50px;
  overflow-y: scroll;
}

li {
  height: 80px;
  margin: 20px 0;
}


</style>
