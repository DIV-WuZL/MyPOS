<template>
  <div id='pos'>
    <el-row>
      <el-col :span=7 id="mOrder">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width:100%">
              <el-table-column prop='goodsName' label='商品名称'></el-table-column>
              <el-table-column prop='count' label='数量' width='50'></el-table-column>
              <el-table-column prop='price' label='单价' width='50'></el-table-column>
              <el-table-column label='操作' width='100' fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size='small' @click="delSingleGoods(scope.row)>删除</el-button>
                  <el-button type="text" size='small' @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="total">
              <b> 数量：</b> <span>{{totalCount}}</span>
              &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
              <b> 金额：</b> <span>￥{{totalMoney}}</span>
            </div>
            <div class="bottomBtn"><hr>
              <el-button type="danger" @click="delAllGoods">删除</el-button>
              <el-button type="warning">挂单</el-button>
              <el-button type="success" @click="checkOut">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">
            挂单
          </el-tab-pane>
          <el-tab-pane label="外卖">
            外卖
          </el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="17">
        <div class="likeGoods">
          <div class="likeTitle">推荐商品</div>
          <div class="likeGoods-list">
            <ul>
              <li v-for="goods in likeGoods" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="likePrice">￥{{goods.price}}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goodsType">
          <el-tabs>
            <el-tab-pane label="主食">
              <div>
                <ul class="mainFoods" >
                  <li v-for="goods in mainFoods" @click="addOrderList(goods)">
                    <span class="foodsImg">
                      <img :src="goods.goodsImg" alt="404">
                    </span>
                    <span class="foodsName">{{goods.goodsName}}</span>
                    <span class="foodsPrice">￥{{goods.price}}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <ul class="mainFoods" >
                  <li v-for="goods in smallFoods" @click="addOrderList(goods)">
                    <span class="foodsImg">
                      <img :src="goods.goodsImg" alt="404">
                    </span>
                    <span class="foodsName">{{goods.goodsName}}</span>
                    <span class="foodsPrice">￥{{goods.price}}</span>
                  </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class="mainFoods" >
                  <li v-for="goods in drinks" @click="addOrderList(goods)">
                    <span class="foodsImg">
                      <img :src="goods.goodsImg" alt="404">
                    </span>
                    <span class="foodsName">{{goods.goodsName}}</span>
                    <span class="foodsPrice">￥{{goods.price}}</span>
                  </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class="mainFoods" >
                  <li v-for="goods in packageFoods" @click="addOrderList(goods)">
                    <span class="foodsImg">
                      <img :src="goods.goodsImg" alt="404">
                    </span>
                    <span class="foodsName">{{goods.goodsName}}</span>
                    <span class="foodsPrice">￥{{goods.price}}</span>
                  </li>
              </ul>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  name:'pos',
  data() {
    return {
      tableData:[],
      likeGoods:[],
      mainFoods:[],
      smallFoods:[],
      drinks:[],
      packageFoods:[],
      totalMoney:0,
      totalCount:0
    }
  },
  created() {
    axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
    .then(response=>{
      // console.log(response)
      this.likeGoods = response.data;
    })
    .catch(error=>{
      console.log('[ERROR] 没有网络了~')
    })

    axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
    .then(response=>{
      // console.log(response)
      this.mainFoods = response.data[0];
      this.smallFoods = response.data[1];
      this.drinks = response.data[2];
      this.packageFoods = response.data[3];
    })
    .catch(error=>{
      console.log('[ERROR] 网络已断开~')
    })
  },
  mounted () {
    var orderHeight = document.body.clientHeight;
    document.getElementById('mOrder').style.height = orderHeight+'px';
  },
  methods: {
    addOrderList(goods){
      this.totalMoney = 0;
      this.totalCount = 0;
      //商品是否存在订单中
      let isHave = false;
      for(let i=0;i<this.tableData.length;i++){
        if(this.tableData[i].goodsId==goods.goodsId){
          isHave = true;
        }
      };
      //业务逻辑
      if(isHave){
        let arr = this.tableData.filter(o=>o.goodsId == goods.goodsId);
        arr[0].count ++;
      }else{
        let newGoods = {
          goodsId:goods.goodsId,
          goodsName:goods.goodsName,
          price:goods.price,
          count:1
        };
        this.tableData.push(newGoods);
      };
      this.getAllMoney();
    },
    delSingleGoods(goods){
      this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId);
      this.getAllMoney();
    },
    delAllGoods(){
      this.tableData=[];
      this.totalCount=0;
      this.totalMoney=0;
    },
    // 模拟结账
    checkOut(){
      if(this.totalCount != 0){
        this.tableData=[];
        this.totalCount=0;
        this.totalMoney=0;
        this.$message({
          // 由element-ui提供的message方法
          message:'^_^   结账成功,请留意订单!',
          type:'success'
        })
      }else{
        this.$message.error('#_#   没有账单需要结账！')
      }
    },
    getAllMoney(){
      this.totalCount = 0;
      this.totalMoney = 0;
      if(this.tableData){
        //计算金额与数量
        this.tableData.forEach((element) => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney+(element.price * element.count);
        })
      }
    }
  },
}
</script>

<style scoped>
#mOrder{
  background-color: #F9FAFC;
  border-right: 1px solid #C0CCDA;
}
.likeTitle{
  height: 22px;
  border-bottom: 1px solid #ddd;
  background-color: #F9FAFC;
  padding: 9px;
  text-align: left;
}
.likeGoods-list ul li{
  list-style: none;
  float: left;
  border: 1px solid skyblue;
  border-radius: 20px;
  padding: 10px;
  margin: 10px;
  background-color: seashell;
  cursor: pointer;
}
.likePrice{
  color: #f30;
}
.goodsType{
  clear: both;
}
.goodsType .mainFoods li{
  list-style: none;
  width: 24%;
  border: 1px solid chocolate;
  /* border-radius: 20px; */
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  margin: 2px;
  float: left;
  cursor: pointer;
}
.goodsType .mainFoods li span{
  display: block;
  float: left;
}
.foodsImg{
  width: 40%
}
.foodsName{
  font-size: 16px;
  padding-left: 10px;
  color: brown;
}
.foodsPrice{
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.total{
  background-color: #fff;
  padding: 10px;
}
.total span{
  color: #f30;
  font-weight: bold;
  font-size: 22px;
}
</style>