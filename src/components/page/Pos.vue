<template>
    <div class="pos">
        <el-row class="row" >
            <el-col :span='7' class="bill-block">
                <el-tabs v-model="activeName" value="finishBill" type="card">
                    <el-tab-pane label="收账" name="finishBill">
                        <el-table border :data="tableData" stripe style="width: 100%">
                            <el-table-column align="center" prop="goodsName" label="商品名称" width="180"></el-table-column>
                            <el-table-column align="center" prop="price" label="价格" width="160"></el-table-column>
                            <el-table-column align="center" prop="count" label="数量"></el-table-column>
                            <el-table-column align="center" fixed="right" label="操作" width="100">
                                <template slot-scope="scope">
                                    <el-button type="text" @click="addOrderList(scope.row)" size="small">添加</el-button>
                                    <el-button type="text" @click="deleteOrder(scope.row)" size="small">删除</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                    </el-tab-pane>
                    <el-tab-pane label="挂单" name="waitBill">配置管理</el-tab-pane>
                    <el-tab-pane label="外卖" name="TakeOutBill">角色管理</el-tab-pane>
                </el-tabs>
                <el-row type="flex" style="padding:16px;" justify="space-around">
                    <el-col :span='12'>
                        数量：{{totalCount}}
                    </el-col>
                    <el-col :span='12'>
                        金额：{{totalMoney}}
                    </el-col>
                </el-row>
                <el-row type="flex" class="action-group" justify="center">
                    <el-col :push=2 col-6>
                        <el-button style="width:120px;" size="medium" type="warning">挂单</el-button>
                    </el-col>
                    <el-col col-6>
                        <el-button style="width:120px;" @click="checkOut" size="medium" type="danger">删除</el-button>
                    </el-col>
                    <el-col :pull=2 col-6>
                        <el-button style="width:120px;" @click="checkOut" size="medium" type="success">结账</el-button>
                    </el-col>
                </el-row>
            </el-col>
            <el-col :span='17'><div class="often-goods">
                 <div class="title">常用商品</div>
                    <div class="often-goods-list">
                        <ul>
                            <li @click="addOrderList(item)" v-for="item in oftenGoods">
                                <span>{{item.goodsName}}</span>
                                <span class="o-price">{{item.price}}元</span>
                            </li>
                        </ul>
                    </div>
                </div>
                <div class="goods-type">
                    <el-tabs class="good-tab" v-model="activeKind" value="stapleFood">
                        <el-tab-pane name="stapleFood" label="主食">
                            <ul class='cookList'>
                                <li @click="addOrderList(goods)" v-for="goods in type0Goods">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane name="snack" label="小食">
                            <ul class='cookList'>
                                <li @click="addOrderList(goods)" v-for="goods in type1Goods">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane name="drinks" label="饮料">
                            <ul class='cookList'>
                                <li @click="addOrderList(goods)" v-for="goods in type2Goods">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane name="setMeal" label="套餐">
                            <ul class='cookList'>
                                <li @click="addOrderList(goods)" v-for="goods in type3Goods">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
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
import axios from "axios";
export default {
  name: "pos",
  data() {
    return {
      activeName: "finishBill",
      activeKind: "stapleFood",
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalCount: 0,
      totalMoney: 0
    };
  },
  created() {
    //常用商品
    axios
      .get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(response => {
        console.log(response);
        this.oftenGoods = response.data;
      })
      .catch(error => {
        console.log(error);
        alert("网络错误，不能访问");
      });
    //读取分类商品列表
    axios
      .get("http://jspang.com/DemoApi/typeGoods.php")
      .then(response => {
        console.log(response);
        //this.oftenGoods=response.data;
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      })
      .catch(error => {
        console.log(error);
        alert("网络错误，不能访问");
      });
  },
  methods: {
    //   添加订单列表
    addOrderList(good) {
      let isHave = false;
      // 判断是否已存在
      for (let item of this.tableData) {
        if (item.goodsId === good.goodsId) {
          isHave = true;
          break;
        }
      }
      // 添加
      if (isHave) {
        let arr = this.tableData.filter(o => o.goodsId == good.goodsId);
        arr[0].count++;
      } else {
        this.tableData.push({
          goodsId: good.goodsId,
          goodsName: good.goodsName,
          price: good.price,
          count: 1
        });
      }
      this.getTotalData();
    },
    //  删除订单列表
    deleteOrder(good) {
      console.log(good);
      this.tableData = this.tableData.filter(o => o.goodsId != good.goodsId);
      this.getTotalData();
    },
    // 删除全部订单
    deleteAllGoods() {
      this.tableData = [];
      this.getTotalData();
    },
    // 统计金额
    getTotalData() {
      this.totalCount = 0;
      this.totalMoney = 0;
      this.tableData.forEach(item => {
        this.totalCount += item.count;
        this.totalMoney += item.count * item.price;
      });
    },
    // 结账
    checkOut() {
      if (this.totalCount != 0) {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
        this.$message({
          message: "感谢你做出的贡献",
          type: "success"
        });
      } else {
        this.$message({
          message: "不能进行空操作",
          type: "warning"
        });
      }
    }
  }
};
</script>

<style scoped>
.pos {
  height: 100%;
}
.row {
  height: 100%;
}
.el-col {
  height: 100%;
}
/* 左边栏 */
.bill-block {
  background-color: #fff;
}
.action-group {
  margin-top: 30px;
}
/* 右边栏（上部） */
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
}
.often-goods-list ul:after {
  content: "";
  display: block;
  visibility: hidden;
  clear: both;
  height: 0;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
  cursor: pointer;
}
.o-price {
  color: #58b7ff;
}
/* 右边栏（下部） */
.cookList li {
  cursor: pointer;
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.goods-type {
  padding-left: 40px;
}
.good-tab .el-tabs__nav .el-tabs__item {
  width: 100px !important;
}
</style>