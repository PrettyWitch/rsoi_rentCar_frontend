<template>
    <div>
        <el-form
            style="width: 100%"
            :model="ruleForm"
            :rules="rules"
            ref="ruleForm"
            label-width="150px"
            class="demo-ruleForm"
        >

            <!-- <el-form-item label="id">
      <el-input v-model="ruleForm.id" readonly></el-input>
    </el-form-item> -->

            <el-form-item label="carUid">
                <el-input
                    v-model="ruleForm.carUid"
                    readonly
                ></el-input>
            </el-form-item>

            <el-form-item label="taken_from_office">
                <el-input
                    v-model="ruleForm.taken_from_office"
                    readonly
                ></el-input>
            </el-form-item>
            <el-form-item
                label="return_to_office"
                prop="return_to_office"
            >
                <el-select
                    v-model="ruleForm.return_to_office"
                    placeholder="please select"
                >
                    <el-option
                        v-for="item in offices"
                        :key="item.id"
                        :label="item.location"
                        :value="item.id"
                    />
                </el-select>
                <!-- <el-input v-model="ruleForm.return_to_office"></el-input> -->
            </el-form-item>
            <el-form-item
                label="booking_period"
                prop="booking_period"
            >
                <el-date-picker
                    v-model="ruleForm.booking_period"
                    type="daterange"
                    range-separator="to"
                    start-placeholder="start date"
                    end-placeholder="end date"
                    format="yyyy-MM-dd"
                    value-format="yyyy-MM-dd"
                >
                </el-date-picker>
                <!-- <el-input v-model="ruleForm.booking_period"></el-input> -->
            </el-form-item>

            <el-form-item>
                <el-button
                    type="primary"
                    @click="submitForm('ruleForm')"
                >Booking</el-button>
                <el-button @click="resetForm('ruleForm')">reset</el-button>
            </el-form-item>
        </el-form>
        <el-dialog
        :show-close="false"
            title="booking"
            :visible.sync="dialogFormVisible"
            center
            width="600px"
            class="dialogHearderBlue"
        >
            <el-form
                style="width: 100%"
                :model="booking"
                ref="ruleForm1"
                label-width="150px"
                class="demo-ruleForm"
            >
 <el-form-item label="car">
   <div style="padding-left:20px">
<p>carUid: {{booking.car.carUid}}</p>
<p>brand: {{booking.car.brand}}</p>
<p>id: {{booking.car.id}}</p>
<p>model: {{booking.car.model}}</p>
<p>power: {{booking.car.power}}</p>
<p>type: {{booking.car.type}}</p>
   </div>


                </el-form-item>
                <el-form-item label="userUid">
                    <el-input
                        v-model="booking.userUid"
                        readonly
                    ></el-input>
                </el-form-item>
                <el-form-item label="bookingUid">
                    <el-input
                        v-model="booking.bookingUid"
                        readonly
                    ></el-input>
                </el-form-item>
                <el-form-item label="paymentUid">
                    <el-input
                        v-model="booking.paymentUid"
                        readonly
                    ></el-input>
                </el-form-item>
                <el-form-item label="bookingPeriod">
                    <el-input
                        v-model="booking.bookingPeriod"
                        readonly
                    ></el-input>
                </el-form-item>
                <el-form-item label="bookingStatus">
                    <el-input
                        v-model="booking.bookingStatus"
                        readonly
                    ></el-input>
                </el-form-item>
                <el-form-item label="takeFromOffice">
                    <el-input
                        v-model="booking.takeFromOffice"
                        readonly
                    ></el-input>
                </el-form-item>
                <el-form-item label="returnToOffice">
                    <el-input
                        v-model="booking.returnToOffice"
                        readonly
                    ></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button
                        type="primary"
                        @click="confirm('ruleForm1')"
                    >confirm booking</el-button>
                    <el-button @click="cancel('ruleForm1')">cancel booking</el-button>
                </el-form-item>
            </el-form>
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
            dialogFormVisible: false,
            booking: {
                id: 3,
                car: {
                    id: 1,
                    carUid: 1,
                    brand: "1",
                    model: "1",
                    power: "1",
                    type: "MIVIVAN",
                },
                bookingUid: 5,
                userUid: 1,
                paymentUid: 20,
                bookingPeriod: "2021-06-23:2021-06-24",
                bookingStatus: "NEW",
                takeFromOffice: 1,
                returnToOffice: 1,
            },
            offices: [
                {
                    id: 1,
                    location: "moscow",
                },
            ],
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
            carUid: "",
            ruleForm: {
                return_to_office: "",
                location: "",
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
                ],
                return_to_office: [
                    {
                        required: true,
                        message: "please enter return_to_office",
                        trigger: "blur",
                    },
                ],
                booking_period: [
                    { required: true, message: "please enter power", trigger: "blur" },
                ],
                type: [
                    {
                        required: true,
                        message: "please select type",
                        trigger: "change",
                    },
                ],
            },
        };
    },
    methods: {
        submitForm(formName) {
            const _this = this;
            let time =
                this.ruleForm.booking_period[0] +
                ":" +
                this.ruleForm.booking_period[1];
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    axios
                        .post(
                            "http://localhost:10010/booking/booking",
                            {
                                car_uid: parseInt(this.ruleForm.carUid),
                                taken_from_office: parseInt(
                                    this.ruleForm.taken_from_office
                                ),
                                return_to_office:
                                    this.ruleForm.return_to_office,
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
                            this.$message.success("booking succeed！");
                            console.log("pay", resp);
                            this.booking = resp.data;
                            this.dialogFormVisible=true
                            // _this.total = resp.data.length
                        });
                } else {
                    return false;
                }
            });
        },
        confirm(formName){
           this.$refs[formName].validate((valid) => {
                if (valid) {
                    axios
                        .patch(
                            `http://localhost:10010/booking/booking/${this.booking.bookingUid}/finish`,'',
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
                            this.$message.success("Confirm the booking is successful！");
                            console.log("pay", resp);
                            this.dialogFormVisible=false
                            this.$router.push('/user')
                            // _this.total = resp.data.length
                        });
                } else {
                    return false;
                }
            });
        },
        cancel(){
                    axios
                        .delete(
                            `http://localhost:10010/booking/booking/${this.booking.bookingUid}`,'',
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
                            this.$message.success("cancel booking！");
                            this.dialogFormVisible=false
                            this.$router.push('/user')
                            // _this.total = resp.data.length
                        });
        },
        resetForm(formName) {
            const _this = this;
            axios
                .get(
                    "http://localhost:10010/car/cars/" + this.$route.query.carUid
                )
                .then(function (resp) {
                    _this.ruleForm = resp.data;
                });
            // this.$refs[formName].resetFields();
        },
    },
    created() {
        this.ruleForm.carUid = this.$route.query.carUid;
        this.ruleForm.taken_from_office = this.$route.query.taken_from_office;
        const _this = this;
        console.log(this.$route.query.carUid);
        axios.get("http://localhost:10010/office/offices/findall").then((resp) => {
            this.offices = resp.data;
            // _this.total = resp.data.length
            console.log(resp);
        });
        // axios.get('http://192.168.3.131:10010/car/car/' + this.$route.query.carUid).then(function (resp){
        //   _this.ruleForm = resp.data
        // })
    },
};
</script>