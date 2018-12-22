<template>
  <div class="box" v-if="form && cities && products && packages">
    <new :urlList="'contracts'">
      <div class="box-header with-border">
        <h3 class="box-title">Basic Information</h3>
      </div>
      <el-form ref="form" :model="form" label-width="120px">
        <el-form-item label="Representative">
          <el-input v-model="form.representative"></el-input>
        </el-form-item>
        <el-row>
          <div class="col-sm-6 col-sm-12 col-xs-12">
            <el-form-item label="Phone 1">
              <el-input v-model="form.phone1"></el-input>
            </el-form-item>
          </div>
          <div class="col-sm-6 col-sm-12 col-xs-12">
            <el-form-item label="Phone 2">
              <el-input v-model="form.phone2"></el-input>
            </el-form-item>
          </div>
        </el-row>
        <el-row>
           <div class="col-sm-3 col-sm-12 col-xs-12">
            <el-form-item label="City">
              <el-select
                v-model="form.city"
                style="width: 100%;"
                filterable
                remote
                placeholder="Please enter city"
                :remote-method="remoteMethod"
                :loading="loading">
                  <el-option
                    v-for="item in cities"
                    :key="item._id"
                    :label="item.name"
                    :value="item.name">
                  </el-option>
                </el-select>
              </el-form-item>
            </div>
           <div class="col-sm-3 col-sm-12 col-xs-12">
            <el-form-item label="Group">
              <el-input v-model="form.group"></el-input>
            </el-form-item>
          </div>
           <div class="col-sm-3 col-sm-12 col-xs-12">
            <el-form-item label="Client">
              <el-select v-if="form.city != ''"
                v-model="client"
                value-key="name"
                style="width: 100%;"
                filterable
                remote
                placeholder="Please enter a keyword"
                :remote-method="remoteMethod"
                :loading="loading">
                <el-option
                  v-for="item in cities.filter(x => x.name == form.city)[0].colleges"
                  :key="item.key"
                  :label="item.name"
                  :value="item">
                </el-option>
              </el-select>
            </el-form-item>
          </div>
          <div class="col-sm-3 col-sm-12 col-xs-12">
            <el-form-item label="Client Label">
              <el-input v-model="form.label"></el-input>
            </el-form-item>
          </div>
        </el-row>
        <el-row>
          <div class="col-sm-3 col-sm-12 col-xs-12">
            <el-form-item label="Auto Code">
              <span>{{form.code}}</span>
            </el-form-item>
          </div>
          <div class="col-sm-3 col-sm-12 col-xs-12">
            <el-form-item label="Numerator">
              <el-input v-model="form.numerator"></el-input>
            </el-form-item>
          </div>
        </el-row>
        <el-col>
          <el-form-item label="Access Token">
            <span>{{form.access_token}}</span>
          </el-form-item>
        </el-col>
        <hr>
        <div class="box-header with-border">
          <h3 class="box-title">Package & Products</h3>
        </div>
        <el-row>
          <div class="col-sm-6 col-sm-12 col-xs-12">
            <el-form-item label="Package">
              <el-select v-model="packagesCurrent" value-key="name" placeholder="please select your zone">
                <el-option v-for="(item, index) of packages" :key="index" :label="item.name" :value="item"></el-option>
              </el-select>
              <button type="button" class="el-button el-button--warning is-plain" @click="addPackage"><span>Add</span></button>
            </el-form-item>
            <div class="row" v-if="form.packages && form.packages.length > 0">
              <div class="card-width bor-right bor-bottom" v-for="pack of form.packages" :key="pack._id"><div class="devin-table-dash centered pad-box"><i class="palette-Pink-500 bg ti-close"></i> <div class="palette-Pink-500 text value">{{pack.name}}</div> <div class="label">{{pack.price_first}}</div></div></div>
            </div>
          </div>
          <div class="col-sm-6 col-sm-12 col-xs-12">
            <el-form-item label="Product">
              <el-select v-model="productsCurrent" value-key="name" placeholder="Select">
                <el-option-group
                  v-for="item of products" 
                  :key="item._id" 
                  :label="item._id">
                  <el-option
                    v-for="product of item.products"
                    :key="product._id"
                    :label="product.name" :value="product">
                  </el-option>
                </el-option-group>
              </el-select>
              <button type="button" class="el-button el-button--warning is-plain" @click="addProduct"><span>Add</span></button>
            </el-form-item>
            <div class="row" v-if="form.products && form.products.length > 0">
              <div class="card-width bor-right bor-bottom" v-for="product of form.products" :key="product._id"><div class="devin-table-dash centered pad-box"><i class="palette-Deep-Purple-500 bg ti-close"></i> <div class="palette-Deep-Purple-500 text value">{{product.name}}</div> <div class="label">{{product.costHire}}</div></div></div>
            </div>
          </div> 
        </el-row>
        <hr>
        <div class="box-header with-border">
          <h3 class="box-title">Unexpected Fee</h3>
        </div>
        <el-row>
          <div class="col-sm-5 col-sm-12 col-xs-12">
            <el-form-item label="Content">
              <el-input v-model="costs.content"></el-input>
            </el-form-item>
          </div>
          <div class="col-sm-3 col-sm-12 col-xs-12">
            <el-form-item label="Fee">
            <el-input v-model="costs.fee"></el-input>
            </el-form-item>
          </div>
          <div class="col-sm-3 col-sm-12 col-xs-12">
            <button type="button" class="el-button el-button--warning is-plain" @click="addCosts"><span>Add Fee</span></button>
          </div>
        </el-row>
        <div class="row">
          <div class="col-xs-12 table-responsive">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th>STT</th>
                  <th>Content</th>
                  <th>Fee</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) of this.form.costs" :key="index">
                  <td>1</td>
                  <td>{{item.content}}</td>
                  <td>{{item.fee}}</td>
                </tr>
              </tbody>
            </table>
          </div>
         </div>
        <div class="box-header with-border">
          <h3 class="box-title">Date & Assign </h3>
        </div>
        <el-form-item label="Taken Date" v-if="takenDate">
          <el-col :span="5">
            <el-date-picker
              v-model="takenDate.start_time"
              type="datetime"
              placeholder="Select date and time">
            </el-date-picker>
          </el-col>
           <button type="button" class="el-button el-button--warning is-plain" @click="addDate"><span>Add</span></button>
        </el-form-item>
        <div class="section section-info">
          <p class="info-time">Schedule</p>
          <div class="info-block" v-for="(item, index) of this.form.taken_dates" :key="index">
            <dl>
              <dt><i class="ti-stats-up"></i></dt>
              <dt v-if="item.start_time">{{item.start_time}}</dt> 
              <dt>{{item.address}}</dt>
              <dd v-for="(ems, inx) of item.employees" :key="inx">{{ems.name}} - {{ems.role}}</dd>
              <dd><el-button :plain="true" @click="editTaken(index)">Edit Date Taken</el-button></dd>
            </dl>
          </div>
        </div>
        <hr>
        <div class="box-header with-border">
          <h3 class="box-title">Payment</h3>
        </div>
        <el-form-item label="Deposit">
          <el-input v-model="form.deposit"></el-input>
        </el-form-item>
        <el-form-item label="Note">
          <el-input type="textarea" v-model="form.note"></el-input>
        </el-form-item>
        <el-form-item label="Auto Total">
          <span :class="{ 'grey-text' :checked}">{{form.total}}</span><br>
          <el-checkbox v-model="checked">Manual Total</el-checkbox>
        </el-form-item>
        <el-form-item label="Input Total">
          <el-input :disabled="!checked" v-model="form.total"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="success" @click="onSubmit">Update</el-button>
          <el-button>Cancel</el-button>
        </el-form-item>
      </el-form>
      <el-dialog title="Edit Schedule" :visible.sync="dialogFormVisible">
        <el-form :model="form">
          <el-form-item label="Date Taken" :label-width="formLabelWidth">
             <el-date-picker
              v-model="form.taken_dates[this.currentEdit].start_time"
              type="datetime"
              placeholder="Select date and time">
            </el-date-picker>
          </el-form-item>
          <el-row>
            <el-col :span="8">
              <el-form-item label="Employee" :label-width="formLabelWidth">
                <el-select v-model="currentEmp" value-key="name" placeholder="Please select a zone">
                  <el-option  v-for="(item, index) of ems" :key="index" :label="item.name" :value="item"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="Role" :label-width="formLabelWidth">
                <el-select v-model="currentEmp.role" placeholder="Select Role">
                  <el-option v-for="(item, index) of departments" :key="index" :label="item" :value="item"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="Note" :label-width="formLabelWidth">
                <el-input type="textarea" v-model="currentEmp.note" ></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="24">
              <el-button type="success" style="width: 100%" @click="addEmp">ADD EMPLOYEE</el-button>
            </el-col><br>
          </el-row>
         <div class="wrap">
            <div class="box-header with-border">
              <h3 class="box-title">List Employee</h3>
            </div>
            <div class="row">
              <div class="employee-item col-md-3" v-for="(em, index) of listEmp" :key="index">
                <div class="em-av">
                  <h1 class="text-white"><img alt="" class="img-circle palette-Grey-900 bg" :src="em.avatar | takeImage"></h1>
                </div>
                <div class="em-meta">
                  <h3>{{em.name}}</h3>
                  <p>{{em.role}}</p>
                </div>
                <button class="btn-remove" @click="removeEms(em)"><i class="el-dialog__close el-icon el-icon-close"></i></button>
              </div>
            </div>
           </div>
          <div class="box-header with-border">
            <h3 class="box-title">Plan</h3>
          </div>
          <el-row>
            <el-col :span="20">
              <div class="box-body">
                <h4>Fixed time picker</h4>
                <el-time-select
                  placeholder="Start time"
                  v-model="currentPlan.startTime"
                  :picker-options="{
                    start: '07:00',
                    step: '00:15',
                    end: '20:30'
                  }">
                </el-time-select>
                <el-time-select
                  placeholder="End time"
                  v-model="currentPlan.endTime"
                  :picker-options="{
                    start: '07:00',
                    step: '00:15',
                    end: '20:30',
                    minTime: startTime
                  }">
                </el-time-select>
              </div>
            </el-col>
            <el-col :span="8">
              <el-form-item label="Clothes" :label-width="formLabelWidth">
                <el-select v-model="currentPlan.clothes" multiple placeholder="Please select a zone">
                  <el-option label="Zone No.1" value="shanghai"></el-option>
                  <el-option label="Zone No.2" value="beijing"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="Location" :label-width="formLabelWidth">
                <el-input v-model="currentPlan.location" ></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="18">
              <el-form-item>
                <el-input v-model="currentPlan.note" type="textarea" placeholder="Note...."></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="1">
              <el-form-item>
              </el-form-item>
            </el-col>
            <el-col :span="4">
              <el-button type="success" @click="addPlan">ADD PLAN</el-button>
            </el-col>
          </el-row>
          <el-table :data="listPlans">
            <el-table-column property="startTime" label="startTime" width="150"></el-table-column>
            <el-table-column property="endTime" label="endTime" width="200"></el-table-column>
            <el-table-column property="clothes" label="clothes"></el-table-column>
            <el-table-column property="location" label="location"></el-table-column>
            <el-table-column property="note" label="note"></el-table-column>
            <el-table-column
              fixed="right"
              label="Actions"
              width="120">
              <template scope="scope">
                <el-button type="text" size="small" @click="removePlan(scope)">Remove</el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-form>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">Cancel</el-button>
          <el-button type="primary" @click="dialogFormVisible = false">Confirm</el-button>
        </span>
      </el-dialog>
    </new>
  </div>
</template>
<script>
import New from '../../partials/New.vue'

export default {
  name: 'ContractDetails',
  components: {
    'new': New
  },
  data () {
    return {
      listPlans: [],
      currentPlan: {
        clothes: []
      },
      cities: null,
      products: [],
      productsCurrent: null,
      packages: [],
      packagesCurrent: null,
      currentEdit: 0,
      ems: null,
      currentEmp: {
        note: '',
        role: '',
        _id: '',
        name: ''
      },
      listEmp: [],
      departments: ['Leader', 'Support', 'Portrait', 'Assistant', 'Decorator'],
      dialogFormVisible: false,
      checked: false,
      contract: null,
      takenDate: {
        address: '',
        start_time: '',
        note: '',
        employees: []
      },
      address: null,
      form: null,
      costs: {
        content: '',
        fee: 0
      },
      list: [],
      loading: false
    }
  },
  created () {
    this.$http.get(`api/contracts/${this.$route.params.id}`).then(res => {
      this.contract = res.body
      this.form = this.contract
    })
    this.$http.get(`api/packages`).then(res => {
      this.packages = res.body
    })
    this.$http.get('api/products').then(res => {
      this.products = res.body
    })
    this.$http.get(`api/cities`).then(res => {
      this.cities = res.body
    })
    this.$http.get(`api/employees`).then(res => {
      this.ems = res.body
    })
  },
  mounted () {
  },
  methods: {
    onSubmit () {
      this.$http.put(`api/contracts/${this.$route.params.id}`, this.form).then(res => {
        if (res.status === 200) {
          this.$router.push({path: '/contracts'})
        }
      })
    },
    addPackage () {
      this.form.packages.push(this.packagesCurrent)
    },
    addProduct () {
      this.form.products.push(this.productsCurrent)
    },
    addCosts () {
      this.form.costs.push(this.costs)
    },
    addDate () {
      this.takenDate.start_time = this.$options.filters.formatDate(this.takenDate.start_time)
      this.form.taken_dates.push(this.takenDate)
    },
    addEmp () {
      this.listEmp.push(this.currentEmp)
      this.form.taken_dates[this.currentEdit].employees = this.listEmp
    },
    addPlan: () => {
      console.log(this.listPlans)
      this.listPlans.push(this.currentPlan)
      this.currentPlan = {}
      this.form.taken_dates[this.currentEdit].plan = this.listPlans
    },
    removePlan (scope) {
      this.listPlans = this.listPlans.filter(x => x.startTime !== scope.row.startTime)
      this.form.taken_dates[this.currentEdit].plan = this.listPlans
    },
    removeEms (em) {
      this.listEmp = this.listEmp.filter(x => x._id !== em._id)
      this.form.taken_dates[this.currentEdit].employees = this.listEmp
    },
    editTaken (index) {
      this.dialogFormVisible = true
      this.currentEdit = index
      console.log(12, this.form.taken_dates[this.currentEdit])
      this.listPlans = this.form.taken_dates[this.currentEdit].plan
    }
  }
}
</script>
<style lang="scss" scoped>
.card-width {
  width: 150px;
  float: left;
  margin-right: 20px;
}
.devin-table-dash .value {
  font-size: 20px;
}
.info-block {
  padding: 10px;
  margin: 0 10px;
  width: 250px;
}
.grey-text {
  color: #f0f0f0;
}
.img-circle {
  height: 30px !important;
  width: 30px !important;
  object-fit: cover;
}
.employee-item {
  position: relative;
  background-color: #D3DCE6;
  border-radius: 5px;
  max-height: 80px;
  margin: 10px;
  .em-meta {
    margin-left: 10px;
    float: left;
    h3 {
      font-size: 10px;
      margin-bottom: 0px;
      font-weight: 600;
    }
    p {
      font-size: 12px;
    }
  }
  .em-av {
    float: left;
    img {
      display: table-cell;
    }
  }
  .btn-remove {
    position: absolute;
    top: -11px;
    background: wheat;
    border-radius: 50%;
    right: -11px;
    &:hover {
      background: #f0f0f0;
    }
  }
}
</style>
