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
                      <template>
                        <el-button type="text" size="small" >删除</el-button>
                        <el-button type="text" size="small" >增加</el-button>
                     </template>
                    </el-table-column>
                  </el-table>

                  <div class="totalDiv">
                    <small>数量：</small>
                    <strong>0</strong> &nbsp;&nbsp;&nbsp;&nbsp;
                    <small>总计：</small>
                    <strong>0</strong> 元
                  </div>

                  <div class="order-btn">
                    <el-button type="warning">挂单</el-button>
                    <el-button type="danger">删除</el-button>
                    <el-button type="success"> 结账</el-button>
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
                      <li v-for="item in hotGoods" :key="item.goodsId">
                        <span>{{ item.goodsName }}</span>
                        <span class="hot-price">￥{{ item.price }}元</span>
                      </li>
                    </ul>
                </div>
              </div>
              <div class="goods-type">
                <el-tabs class="tabs">
                  <el-tab-pane label="主食">
                    <ul class='cookList'>
                      <li v-for="item in type0Goods" :key="item.goodsId">
                        <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                        <span class="foodName">{{ item.goodsName }}</span>
                        <span class="foodPrice">￥{{ item.price }}元</span>
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
      tableData: [{    
          goodsName: '可口可乐',
          price: 8,
          count:1
        }, {
          
          goodsName: '香辣鸡腿堡',
          price: 15,
          count:1
        }, {
         
          goodsName: '爱心薯条',
          price: 8,
          count:1
        }, {
         
          goodsName: '甜筒',
          price: 8,
          count:1
      }],
      hotGoods: [],
      type0Goods:[]
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
          console.log(data)
          this.type0Goods = data.type0Goods
        }
      })
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
