<template>
  <div class="pos">
     <div>
        <el-row >
            <el-col :span='7' id="order-list" class="pos-order">
              <el-tabs class="tabs">
                <el-tab-pane label="点餐">
                  <el-table :data="tableData" border style="width: 100%">
                    <el-table-column prop="goodsName" label="日期"></el-table-column>
                    <el-table-column prop="count" label="数量" width="50"></el-table-column>
                    <el-table-column prop="price" label="金额" width="70"></el-table-column>
                    <el-table-column label="操作" width="100" fixed="right">
                      <template slot-scope="scope">
                        <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                        <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                     </template>
                    </el-table-column>
                  </el-table>
 
                  <div class="totalDiv">
                    <small>数量：</small>
                    <strong>{{ totalCount }}</strong> &nbsp;&nbsp;&nbsp;&nbsp;
                    <small>总计：</small>
                    <strong>{{ totalMoney }}</strong> 元
                  </div>

                  <div class="order-btn">
                    <el-button type="warning">挂单</el-button>
                    <el-button type="danger" @click="delAllGoods">删除</el-button>
                    <el-button type="success" @click="checkout"> 结账</el-button>
                  </div>
                </el-tab-pane>
                <el-tab-pane label="外卖">
                  外卖
                </el-tab-pane>
                <el-tab-pane label="挂单">
                  挂单
                </el-tab-pane>
              </el-tabs>
            </el-col>
            <!--商品展示-->
            <el-col :span="17">
             <div class="hot-goods">
                <div class="title">常用商品</div>
                <div class="hot-goods-list">
                    <ul>
                      <li v-for="goods in hotGoods" :key="goods.goodsId" @click="addOrderList(goods)">
                        <span>{{ goods.goodsName }}</span>
                        <span class="hot-price">￥{{ goods.price }}元</span>
                      </li>
                    </ul>
                </div>
              </div>
              <div class="goods-type">
                <el-tabs class="tabs">
                  <el-tab-pane label="主食">
                    <ul class='cookList'>
                      <li v-for="goods in type0Goods" :key="goods.goodsId" @click="addOrderList(goods)">
                        <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                        <span class="foodName">{{ goods.goodsName }}</span>
                        <span class="foodPrice">￥{{ goods.price }}元</span>
                      </li>
                  </ul>
                  </el-tab-pane>
                  <el-tab-pane label="小食">
                    小食
                  </el-tab-pane> 
                  <el-tab-pane label="饮料">
                    饮料
                  </el-tab-pane> 
                  <el-tab-pane label="套餐">
                    套餐
                  </el-tab-pane>  
                </el-tabs>
              </div>
            </el-col>
        </el-row>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data () {
    return {
      tableData: [],
      hotGoods: [],
      type0Goods:[],
      totalMoney: 0, //订单总价格
      totalCount: 0  //订单商品总数量

    }
  },
  methods: {
    getHotGoods () {
      axios.get('/api/hotGoods.json').then(res => {
        if(res.status && res.data) {
          const data = res.data.data
          this.hotGoods = data.hotGoods
        }
      })
    },
    getTypeGoods () {
      axios.get('/api/typeGoods.json').then(res => {
        if(res.status && res.data) {
          const data = res.data.data
          this.type0Goods = data.type0Goods
        }
      })
    },
    addOrderList(goods) {

      this.totalMoney = 0; //汇总数量清0
      this.totalCount = 0;
      
      //判断是否这个商品已经存在于订单列表
      let isHave = false;
      for(let i = 0; i < this.tableData.length; i++) {
        if(this.tableData[i].goodsId === goods.goodsId) {
          isHave = true;
        }
      }
      //根据isHave的值判断订单列表中是否已经有此商品
      if(isHave) {
        let arr = this.tableData.filter( o => o.goodsId === goods.goodsId)
        arr[0].count++;
      }else {
        let data = { 
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        }
        this.tableData.push(data)
      }

      this.getAllMoney();
    },
    getAllMoney () {
      this.totalCount = 0;
      this.totalMoney = 0;
      if(this.tableData) {
        this.tableData.forEach(goods => {
        this.totalCount += goods.count
        this.totalMoney += goods.price * goods.count
      });
     }
    },
    delSingleGoods (goods) {
     this.tableData.forEach((ele, index) => {
       if(ele.goodsId === goods.goodsId) {
         this.tableData.splice(index, 1)
       }
     })
     this.getAllMoney();
    },
    delAllGoods () {
      this.tableData = []
      this.totalCount = 0
      this.totalMoney = 0
    },
    checkout () {
      if(this.totalCount !== 0) {
        this.delAllGoods()
        this.$message({
          message: '结账成功，欢迎下次光临！',
          type: 'success'
        });
      }else {
        this.$message({
          message: '不能空结，请先点单！',
          type: 'error'
        });
      }
      
    }
  },
  created () {
    this.getHotGoods()
    this.getTypeGoods()
  },
  mounted () {
    var orderHeight = document.body.clientHeight
    document.getElementById('order-list').style.height = orderHeight + 'px'
  }
}
</script>

<style scoped>
.pos{
  font-size: 12px;
}
.pos-order{
  background-color: #F9FAFC;
  border-right: 1px solid #C0CCDA;
}
.tabs{
  padding-left: 5px;
}
.totalDiv {
  height: auto;
  overflow: hidden;
  text-align: right;
  font-size: 16px;
  background-color: #fff;
  border-bottom: 1px solid #E5E9F2;
  padding: 10px;
}
.order-btn {
  margin-top: 10px;
  text-align: center;
}
.title{
  height: 20px;
  border-bottom:1px solid #D3DCE6;
  background-color: #F9FAFC;
  padding:10px;
}
.hot-goods-list ul li{
  list-style: none;
  float: left;
  border: 1px solid #E5E9F2;
  padding: 15px;
  margin: 10px;
  background-color: #fff;
  cursor: pointer;
}
.hot-price{
  color:#58B7FF; 
}
.goods-type {
  clear: both;
}
.cookList li{
  list-style: none;
  width: 20%;
  border:1px solid #E5E9F2;
  height: auto;
  overflow: hidden;
  background-color:#fff;
  padding: 2px;
  float:left;
  margin: 10px;
  cursor: pointer;
}
.cookList li span{
  display: block;
  float:left;
}
.foodImg{
  width: 40%;
}
.foodName{
  font-size: 16px;
  padding-left: 10px;
  color:brown;
  padding-top: 10px;
}
.foodPrice{
  font-size: 16px;
  padding-left: 10px;
  padding-top:10px;
}
  
</style>
