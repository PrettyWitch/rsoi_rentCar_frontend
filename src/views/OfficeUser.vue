<template>
  <el-table
      :data="tableData"
      style="width: 100%" height="500px">
    <el-table-column
        fixed
        prop="id"
        label="id"
        width="150"
    >
    </el-table-column>

    <el-table-column
        prop="officeUid"
        label="officeUid"
        width="150"
    >
    </el-table-column>

    <el-table-column
        prop="location"
        label="location"
        style="width: 50%"
    >
    </el-table-column>

    fixed="right"
    <el-table-column

        label="operation"
        width="150">
      <template slot-scope="scope">
        <el-button @click="find(scope.row)" type="text" size="medium">view car</el-button>
<!--        <el-button @click="del(scope.row)" type="text" size="medium">删除</el-button>-->
      </template>
    </el-table-column>

  </el-table>
</template>

<script>
export default {
    data() {
    return {
      tableData: [{
        id: '',
        officeUid:'',
        location: '',
      }]
    }
  },
    created() {
    const _this = this

    axios.get('http://localhost:10010/office/offices/findall','',{
      headers:{
        'Authorization': 'Bearer ' + localStorage.getItem('Cartoken')
      }
    }).then(function (resp) {
      _this.tableData = resp.data
      // _this.total = resp.data.length
      // console.log(resp);
    })
  },
  methods: {
    find(row) {

      this.$router.push({
        path:'/user/office/cars',
        query:{
          officeUid:row.officeUid
        }
      })
      console.log(row);
    },
    // del(row){
    //   const _this = this
    //   axios.delete('http://192.168.3.131:10010/office/office/delete/' + row.id).then(function (resp){
    //     console.log(resp)
    //     if (resp.status === 200){
    //       _this.$alert('success', 'Delete Car', {
    //         confirmButtonText: 'ok',
    //         callback: action => {
    //           window.location.reload()
    //         }
    //       });
    //     }
    //   })
    //
    // }
  },


}
</script>