<template>
  <section class="table">
    <el-data-table
      :url="url"
      :columns="columns"
      :dataPath="dataPath"
      :totalPath="totalPath"
      :hasDelete="true"
      :hasView="true"
      :searchForm="searchForm"
      :operationAttrs="operationAttrs"
      :extraButtons="extraButtons"
      :paginationLayout="pager"
    >
      <el-table-column
        label="状态"
        prop="oncar"
      >
        <template slot-scope="scope">
          <span
            v-if="scope.row.oncar===true"
            class="green"
          >上架</span>
          <span v-else>下架</span>
        </template>
      </el-table-column>
    </el-data-table>
  </section>
</template>

<script>
import { formatDate } from '../const/filter.js';
import { typeDir } from '../const/dir.js';
export default {
  data() {
    return {
      url: "/mock/11/demo",
      dataPath: "rows",
      totalPath:"total",
      pager:"total,sizes,prev,pager,next,jumper",
      operationAttrs:{
          width:'300px',
          fixed:'right'
      },
      searchForm: [{
        $id: 'name',
        $type: 'input',
        label: '组件名称',
        $default: '',
        $el: {
          placeholder: '请输入'
        }
      },
      {
        $id: 'type',
        $type: 'select',
        label: '分类',
        $el: {
          placeholder: '请选择'
        },
        $options:Object.keys(typeDir).map(i=>{
            return {
                label:typeDir[i],
                value:i
            }
        })
      },
        {
        $id: 'oncar',
        $type: 'select',
        label: '状态',
        $el: {
          placeholder: '请选择'
        },
        $options:[
            {
                label:'上架',
                value:true
            },
            {
                label:'下架',
                value:false
            }
        ]
      }
      ],
      extraButtons:[
           {type: '', text:'下架', atClick: row => {
               console.log(row)
               this.$alert('下架')
               return romise.resolve()
           }, show: row => row.oncar===true},
           {type: '', text:'上架', atClick: row => {
               console.log(row)
               this.$alert('上架')
               return romise.resolve()
           }, show: row => row.oncar===false},
      ],
      columns: [
        {
          label: "id",
          prop: "id",
          type: "selection"
        },
        {
          label: "组件名称",
          prop: "name"
        },
        {
          label: "分类",
          prop: "type",
          formatter(row, column, cellValue, index){
              console.log(cellValue);
              return typeDir[cellValue]||"";
          }
        },
        {
          label: "版本",
          prop: "version"
        },
        {
          label: "开发语言",
          prop: "language"
        },
        {
          label: "最后更新时间",
          prop: "time",
          formatter(row, column, cellValue, index) {
            console.log(cellValue)
            return formatDate(cellValue, "YYYY-MM-DD");
          }
        }
      ]
    }
  },

}
</script>

<style lang="css" scoped>
.green {
  color: #67c23a;
}
</style>