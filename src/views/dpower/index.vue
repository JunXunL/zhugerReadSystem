<template>
  <!-- 新权限管理 -->
  <div class="power">
    <myNav :bread="routerData" />

    <div class="table-page">
      <p class="name">
        <span>姓名：</span>
        <span>窦瑞环</span>
        <span>手机号：</span>
        <span>15175331451</span>
      </p>
      <div class="group_power">
        <span>所属组：</span>
        <el-select v-model="value" placeholder="请选择" size="mini">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          ></el-option>
        </el-select>
      </div>
      <div class="powerList" v-for="item in managementList">
          <power :powertitle="item"/>
      </div>
        
    </div>   
  </div>
</template>
<script>
import power from "@/components/dpower"
export default {
    components:{power},
  data() {
    return {
      routerData: {
        content: []
      },
      options: [
        {
          value: "选项1",
          label: "产品/设计"
        },
        {
          value: "选项2",
          label: "研发"
        },
        {
          value: "选项3",
          label: "测试"
        },
        {
          value: "选项4",
          label: "运营"
        },
        {
          value: "选项5",
          label: "内容"
        },
        {
          value: "选项6",
          label: "超级管理员"
        }
      ],
      value: "产品/设计",
     managementList:[
         {
             title:"书籍管理",
             cityOptions:["上下架","书籍详情","书籍关卡编辑","新增书籍"]
         },
         {
             title:"书单管理",
             cityOptions:["上下架","书单详情","新增书单"]
         },
         {
             title:"同伴管理",
             cityOptions:["组合","图鉴","上下架",'同伴详情',"新增同伴","删除同伴"]
         },
         {
             title:"其它权限",
             cityOptions:["兑换码","参数设置","用户管理","消息","用户","数据"]
         }
     ]
    };
  },
  created() {
    this.fetchData();
  },
  methods: {
    fetchData() {},
    
  }
};
</script>
<style scoped>
/deep/ .el-form--inline .el-form-item__content {
  width: 50%;
}
.power {
  font-size: 13px;
}
.table-page{
  height: 660px;
  overflow: hidden;
}


.table-page .name {
  line-height: 32px;
  margin-top: 30px;
  margin-left: 20px;
}
.table-page .name span {
  display: inline-block;
  padding-right: 20px;
  text-align: center;
  font-size: 13px;
}
.group_power {
  margin-top: 30px;
  margin-left: 20px;
}
.user_power {
  margin-top: 30px;
}
.powerList{
  margin-left: 20px;
}
</style>