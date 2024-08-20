<script setup>
import { computed, ref } from 'vue';
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'

const data = [
  {
    "id": 1,
    "name": "珍珠奶茶",
    "description": "香濃奶茶搭配QQ珍珠",
    "price": 50
  },
  {
    "id": 2,
    "name": "冬瓜檸檬",
    "description": "清新冬瓜配上新鮮檸檬",
    "price": 45
  },
  {
    "id": 3,
    "name": "翡翠檸檬",
    "description": "綠茶與檸檬的完美結合",
    "price": 55
  },
  {
    "id": 4,
    "name": "四季春茶",
    "description": "香醇四季春茶，回甘無比",
    "price": 45
  },
  {
    "id": 5,
    "name": "阿薩姆奶茶",
    "description": "阿薩姆紅茶搭配香醇鮮奶",
    "price": 50
  },
  {
    "id": 6,
    "name": "檸檬冰茶",
    "description": "檸檬與冰茶的清新組合",
    "price": 45
  },
  {
    "id": 7,
    "name": "芒果綠茶",
    "description": "芒果與綠茶的獨特風味",
    "price": 55
  },
  {
    "id": 8,
    "name": "抹茶拿鐵",
    "description": "抹茶與鮮奶的絕配",
    "price": 60
  }
]

const orderItem =ref([])
const orderCheckItem =ref([])


const sendItem = (item)=>{
orderItem.value.push({
  id:new Date().getTime(),
  name:item.name,
  description:item.description,
  price:item.price,
  quantity: 1
})
}

const deleteItem =()=>{
 
  const index = orderItem.value.findIndex((item)=>item.id === orderItem.id )
  orderItem.value.splice(index,1)
}


    const itemSubtotal = (item) => {
  return item.price * item.quantity;
};
const sum = computed(() => {
  return orderItem.value.reduce((pre, next) => {
    return pre + next.price * next.quantity;
  }, 0);
});

const checkSum = computed(() => {
  return orderCheckItem.value.reduce((pre, next) => {
    return pre + next.price * next.quantity;
  }, 0);
});


const checkItem =(item)=>{
 
if (orderItem.value.length === 0) {
    turn.value = false;  // 修改为 turn.value
  } else {
    orderItem.value.forEach(item => {
    orderCheckItem.value.push(item);
  });
    turn.value = true;  // 修改为 turn.value
  }
  orderItem.value = [];
  

}
let  turn = ref(false)
let  turnOrder = false

</script>

<template>
<div id="root">
  <div class="container mt-5">
    <div class="row"  >
      <div class="col-md-4" >
        <div class="list-group"  >
          <a  v-for="(item,key) in data" :key="item.id"    href="#" class="list-group-item list-group-item-action" @click="sendItem(item)" >
            <div class="d-flex w-100 justify-content-between"  >
              <h5 class="mb-1">{{item.name}}</h5>
              <small>{{ item.price }}</small>
            </div>
            <p class="mb-1">{{item.description}}</p>
          </a>                    
        </div>
      </div>
      <div class="col-md-8">
        <table class="table">
          <thead>
            <tr>
              <th scope="col" width="50">操作</th>
              <th scope="col">品項</th>
              <th scope="col">描述</th>
              <th scope="col" width="90">數量</th>
              <th scope="col">單價</th>
              <th scope="col">小計</th>
            </tr>
          </thead>
          
          <tbody>
            <tr v-for="item in orderItem" :key="orderItem.id">
              <td><button type="button" class="btn btn-sm" @click="deleteItem(item)">x</button></td>
              <td>{{item.name}}</td>
              <td><small>{{ item.description }}</small></td>
              <td>
                <select class="form-select"    v-model="item.quantity"  >
                  <option v-for="n in 10" :key="n" :value="n">{{ n }}</option>
                </select>
              </td>
              <td>{{item.price}}</td>
              <td>{{ itemSubtotal(item) }}</td>
            </tr>
        
          </tbody>
        </table>
        <div class="text-end mb-3">
          <h5>總計: <span>{{sum}}</span></h5>
        </div>
        <textarea
          class="form-control mb-3"
          rows="3"
          placeholder="備註"
        ></textarea>
        <div class="text-end">
          <button class="btn btn-primary" @click="checkItem(orderItem)" >送出</button>
        </div>
      </div>
    </div>
    <hr />
    <div class="row justify-content-center" v-if="turn" >
      <div class="col-8">
        <div class="card">
          <div class="card-body">
            <div class="card-title">
              <h5>訂單</h5>
              <table class="table">
                <thead>
                  <tr>
                    <th scope="col">品項</th>
                    <th scope="col">數量</th>
                    <th scope="col">小計</th>
                  </tr>
                </thead>
                <tbody  v-for="item2 in orderCheckItem" >
                  <tr>
                    <td>{{item2.name}}</td>
                    <td>{{ item2.quantity }}</td>
                    <td>{{item2.price}}</td>
                  </tr>
               
                </tbody>
              </table>
              <div class="text-end">備註: <span>都不要香菜</span></div>
              <div class="text-end">
                <h5>總計: <span>{{ checkSum }}</span></h5>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<style scoped>

</style>
