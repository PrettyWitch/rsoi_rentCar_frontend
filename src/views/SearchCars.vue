<!--3.获取有关办公室中所有计算机的信息。[G]GET /办公室/ {officeUid} /汽车-->
<!--4.获取有关汽车及其在特定办公室中的可用性的信息。[G]GET /办公室/ {officeUid} /汽车/ {carUid}-->
<!--5.获取有关汽车及其在所有办公室的可用性的信息。[G]GET /办公室/汽车/ {carUid}-->
<template>
  <div>
    <h4>Office info:</h4>
    <!--  :span-method="arraySpanMethod"  -->
    <el-table
        :data="tableData"
        style="width: 100%"
    >
      <el-table-column
          prop="office.officeUid"
          label="officeUid"
          style="width: 20%"
      >
      </el-table-column>

      <el-table-column
          prop="office.location"
          label="location"
          style="width: 20%"
      >
      </el-table-column>

    </el-table>

    <h4>Cars info for this office:</h4>
    <el-table
        :data="tableData"
        style="width: 100%"
        height="500px"
    >

      <el-table-column
          prop="availableCars.car.carUid"
          label="carUid"
          width="100"
      >
      </el-table-column>

      <el-table-column
          prop="availableCars.car.brand"
          label="brand"
          width="100"
      >
      </el-table-column>

      <el-table-column
          prop="availableCars.car.model"
          label="model"
          width="100"
      >
      </el-table-column>

      <el-table-column
          prop="availableCars.car.power"
          label="power"
          width="100"
      >
      </el-table-column>

      <el-table-column
          prop="availableCars.car.type"
          label="type"
          width="150"
      >
      </el-table-column>

      <el-table-column
          prop="availableCars.registrationNumber"
          label="registrationNumber"
          width="200"
      >
      </el-table-column>

      <el-table-column
          prop="availableCars.availabilitySchedules"
          label="availabilitySchedules"
          style="width: 20%"
      >
      </el-table-column>

      <el-table-column
          fixed="right"
          label="operation"
          width="150"
      >
        <template slot-scope="scope">
          <el-button
              @click="view(scope.row)"
              type="text"
              size="small"
          >view
          </el-button>
          <el-button
              @click="booking(scope.row)"
              type="text"
              size="small"
          >booking
          </el-button>
<!--          <el-button-->
<!--              @click="del(scope.row)"-->
<!--              type="text"-->
<!--              size="small"-->
<!--          >delete-->
<!--          </el-button>-->
        </template>
      </el-table-column>
    </el-table>
    <el-dialog
        v-dialogDrag
        title="booking"
        :visible.sync="dialogFormVisible"
        center
        width="600px"
        class="dialogHearderBlue"
    >

      <!-- <div
          slot="footer"
          class="dialog-footer"
      >
          <el-button
              type="primary"
              size="mini"
              @click="booking()"
          >
              保存
          </el-button>
          <el-button
              size="mini"
              @click="dialogFormVisible = false"
          >
              取消
          </el-button>
      </div> -->
    </el-dialog>
  </div>
</template>


<script>
export default {
  data() {
    return {
      carUid: "",
      ruleForm: {
        id: "",
        carUid: "",
        taken_from_office: "",
        model: "",
        booking_period: "",
        type: "",
      },
      rules: {
        taken_from_office: [
          {
            required: true,
            message: "please enter taken_from_office",
            trigger: "blur",
          },
          {
            min: 1,
            max: 10,
            message: "3 to 8 characters in length",
            trigger: "blur",
          },
        ],
        return_to_office: [
          {
            required: true,
            message: "please enter return_to_office",
            trigger: "blur",
          },
          {
            min: 1,
            max: 5,
            message: "3 to 8 characters in length",
            trigger: "blur",
          },
        ],
        booking_period: [
          {required: true, message: "please enter power", trigger: "blur"},
        ],
        type: [
          {
            required: true,
            message: "please select type",
            trigger: "change",
          },
        ],
      },
      dialogFormVisible: false,
      // total:null,
      offices: [],
      tableData: [
        {
          office: {
            officeUid: "",
            location: "",
          },
          availableCars: {
            car: {
              carUid: "",
              brand: "",
              model: "",
              power: "",
              type: "",
            },
            registrationNumber: "",
            availabilitySchedules: "",
          },
        },
      ],
    };
  },
  created() {
    axios
        .post(
            "http://localhost:10010/booking/booking",
            {
              car_uid: 1,
              taken_from_office: 90,
              return_to_office: 86,
              booking_period: "2021-06-19:2021-06-20",
              payment_data: {
                status: "NEW",
              },
            },
            {
              headers: {
                "Content-Type": "application/json;charset=utf-8",
                "Access-control-Allow-Origin": "Origin",
                "Access-Control-Allow-Headers":
                    "Access-Control-Allow-Headers",
              },
            }
        )
        .then((resp) => {
          console.log("pay", resp);
          this.offices = resp.data;
          // _this.total = resp.data.length
        });
    axios.get("http://localhost:10010/office/offices/findall").then((resp) => {
      this.offices = resp.data;
      // _this.total = resp.data.length
      console.log(resp);
    });

    if (typeof this.$route.query.carUid == "undefined") {
      //3.获取有关办公室中所有计算机的信息。[G]
      //GET / offices / {officeUid} / cars
      const _this = this;
      axios
          .get(
              "http://localhost:10010/office/offices/" +
              this.$route.query.officeUid +
              "/cars", '', {
                headers: {
                  'Authorization': 'Bearer ' + localStorage.getItem('Cartoken')
                }
              }
          )
          .then(function (resp) {
            _this.tableData = resp.data;
            console.log(resp);
          });
    } else {
      // 4.获取有关汽车及其在特定办公室中的可用性的信息。[G]
      // GET / offices / {officeUid} / cars / {carUid}
      const _this = this;
      axios
          .get(
              "http://localhost:10010/office/offices/" +
              this.$route.query.officeUid +
              "/cars/" +
              this.$route.query.carUid
          )
          .then(function (resp) {
            _this.tableData = resp.data;
            console.log(resp);
          });
    }
  },
  methods: {
    submitForm(formName) {
      const _this = this;
      console.log(this.ruleForm);
      let time = this.ruleForm.booking_period[0] + ':' + this.ruleForm.booking_period[1]
      console.log(this.ruleForm);
      this.$refs[formName].validate((valid) => {
        if (valid) {
          axios
              .post(
                  "http://localhost:10010/booking/booking",
                  {
                    car_uid: parseInt(this.ruleForm.carUid),
                    taken_from_office: parseInt(this.tableData[0].office.location),
                    return_to_office: this.ruleForm.return_to_office,
                    booking_period: time,
                    payment_data: {
                      status: "NEW",
                    },
                  },
                  {
                    headers: {
                      "Content-Type":
                          "application/json;charset=utf-8",
                      "Access-control-Allow-Origin": "Origin",
                      "Access-Control-Allow-Headers":
                          "Access-Control-Allow-Headers",
                    },
                  }
              )
              .then((resp) => {
                console.log("pay", resp);
                this.offices = resp.data;
                // _this.total = resp.data.length
              });
        } else {
          return false;
        }
      });
    },
    resetForm(formName) {
      const _this = this;
      // axios
      //     .get(
      //         "http://192.168.3.131:10010/car/car/" + this.$route.query.carUid
      //     )
      //     .then(function (resp) {
      //         _this.ruleForm = resp.data;
      //     });
      // this.$refs[formName].resetFields();
    },
    //方法要重写
    booking(row) {
      // this.dialogFormVisible = true;
      this.ruleForm.carUid = row.availableCars.car.id;
      this.$router.push({
        path: '/user/carBooking',
        query: {
          carUid: row.availableCars.car.id,
          taken_from_office: row.office.officeUid
        }
      })
      console.log("row", row);
    },

    view(row) {
      const officeUid2 = this.$route.query.officeUid;
      console.log("carUid:", row.availableCars.car.carUid);
      this.$router.push({
        path: "/user/office/cars",
        query: {
          carUid: row.availableCars.car.carUid,
          officeUid: officeUid2,
        },
      });
      console.log("row:", row);
      location.reload();
    },
    // arraySpanMethod: function (row){
    //   if (row > 1){
    //     return 'hidden-row';
    //   }
    // }
    // /12.从办公室删除车辆。[A] [M] [G] DELETE /offices/{officeUid}/car/{carUid}
    del(row) {
      const _this = this;
      axios
          .delete(
              "http://localhost:10010/office/offices/" +
              this.$route.query.officeUid +
              "/car/" +
              row.availableCars.car.carUid
          )
          .then(function (resp) {
            console.log(resp);
            if (resp.status === 204) {
              _this.$alert("success", "Delete Car", {
                confirmButtonText: "ok",
                callback: (action) => {
                  window.location.reload();
                },
              });
            }
          });
    },
  },
};
</script>


