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
      hotGoods:[
        {
            goodsId:1,
            goodsName:'香辣鸡腿堡',
            price:18
        }, {
            goodsId:2,
            goodsName:'田园鸡腿堡',
            price:15
        }, {
            goodsId:3,
            goodsName:'和风汉堡',
            price:15
        }, {
            goodsId:4,
            goodsName:'快乐全家桶',
            price:80
        }, {
            goodsId:5,
            goodsName:'脆皮炸鸡腿',
            price:10
        }, {
            goodsId:6,
            goodsName:'魔法鸡块',
            price:20
        }, {
            goodsId:7,
            goodsName:'可乐大杯',
            price:10
        }, {
            goodsId:8,
            goodsName:'雪顶咖啡',
            price:18
        }, {
            goodsId:9,
            goodsName:'大块鸡米花',
            price:15
        }, {
            goodsId:20,
            goodsName:'香脆鸡柳',
            price:17
        }
      ],
      type0Goods:[
          {
              goodsId:1,
              goodsImg:"http://p0.meituan.net/xianfu/549f44bcf8326227d355aa3539ebd52f340133.png",
              goodsName:'香辣鸡腿堡',
              price:18
          }, {
              goodsId:2,
              goodsImg:"http://p1.meituan.net/xianfu/11d12a87c1a7aa58fa677c7b966b6594323357.png",
              goodsName:'田园鸡腿堡',
              price:15
          }, {
              goodsId:3,
              goodsImg:"http://p0.meituan.net/xianfu/549f44bcf8326227d355aa3539ebd52f340133.png",
              goodsName:'和风汉堡',
              price:15
          }, {
              goodsId:4,
               goodsImg:"http://p1.meituan.net/xianfu/11d12a87c1a7aa58fa677c7b966b6594323357.png",
              goodsName:'快乐全家桶',
              price:80
          }, {
              goodsId:5,
               goodsImg:"http://p0.meituan.net/xianfu/549f44bcf8326227d355aa3539ebd52f340133.png",
              goodsName:'脆皮炸鸡腿',
              price:10
          }, {
              goodsId:6,
               goodsImg:"http://p1.meituan.net/xianfu/11d12a87c1a7aa58fa677c7b966b6594323357.png",
              goodsName:'魔法鸡块',
              price:20
          }, {
              goodsId:7,
               goodsImg:"http://p0.meituan.net/xianfu/549f44bcf8326227d355aa3539ebd52f340133.png",
              goodsName:'可乐大杯',
              price:10
          }, {
              goodsId:8,
               goodsImg:"http://p1.meituan.net/xianfu/11d12a87c1a7aa58fa677c7b966b6594323357.png",
              goodsName:'雪顶咖啡',
              price:18
          }, {
              goodsId:9,
               goodsImg:"http://p0.meituan.net/xianfu/549f44bcf8326227d355aa3539ebd52f340133.png",
              goodsName:'大块鸡米花',
              price:15
          }, {
              goodsId:20,
               goodsImg:"http://p0.meituan.net/xianfu/549f44bcf8326227d355aa3539ebd52f340133.png",
              goodsName:'香脆鸡柳',
              price:17
          }
      ]
    }
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
