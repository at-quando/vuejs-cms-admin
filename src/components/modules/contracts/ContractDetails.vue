<template>
  <div class="box">
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
              <el-input v-model="form.city"></el-input>
            </el-form-item>
          </div>
           <div class="col-sm-3 col-sm-12 col-xs-12">
            <el-form-item label="Group">
              <el-input v-model="form.group"></el-input>
            </el-form-item>
          </div>
           <div class="col-sm-3 col-sm-12 col-xs-12">
            <el-form-item label="Client">
              <el-select
                v-model="form.client"
                multiple
                style="width: 100%;"
                filterable
                remote
                placeholder="Please enter a keyword"
                :remote-method="remoteMethod"
                :loading="loading">
                <el-option
                  v-for="item in options4"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
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
          <h3 class="box-title">Package & Clothes</h3>
        </div>
        <el-row>
          <div class="col-sm-6 col-sm-12 col-xs-12">
            <el-form-item label="Package">
              <el-select v-model="packages" placeholder="please select your zone">
                <el-option label="Zone one" value="shanghai"></el-option>
                <el-option label="Zone two" value="beijing"></el-option>
              </el-select>
              <button type="button" class="el-button el-button--warning is-plain"><span>Add</span></button>
            </el-form-item>
            <div class="row">
              <div class="card-width bor-right bor-bottom"><div class="devin-table-dash centered pad-box"><i class="palette-Pink-500 bg ti-close"></i> <div class="palette-Pink-500 text value">Combo 1</div> <div class="label">149k</div></div></div>
              <div class="card-width bor-right bor-bottom"><div class="devin-table-dash centered pad-box"><i class="palette-Pink-500 bg ti-close"></i> <div class="palette-Pink-500 text value">648</div> <div class="label">Items Sold</div></div></div>
              <div class="card-width bor-right bor-bottom"><div class="devin-table-dash centered pad-box"><i class="palette-Pink-500 bg ti-close"></i> <div class="palette-Pink-500 text value">648</div> <div class="label">Items Sold</div></div></div>
            </div>
          </div>
          <div class="col-sm-6 col-sm-12 col-xs-12">
            <el-form-item label="Clothes">
              <el-select v-model="packages" placeholder="please select your zone">
                <el-option label="Zone one" value="shanghai"></el-option>
                <el-option label="Zone two" value="beijing"></el-option>
              </el-select>
              <button type="button" class="el-button el-button--warning is-plain"><span>Add</span></button>
            </el-form-item>
            <div class="row">
              <div class="card-width bor-right bor-bottom"><div class="devin-table-dash centered pad-box"><i class="palette-Deep-Purple-500 bg ti-close"></i> <div class="palette-Deep-Purple-500 text value">Combo 1</div> <div class="label">149k</div></div></div>
              <div class="card-width bor-right bor-bottom"><div class="devin-table-dash centered pad-box"><i class="palette-Deep-Purple-500 bg ti-close"></i> <div class="palette-Deep-Purple-500 text value">648</div> <div class="label">Items Sold</div></div></div>
              <div class="card-width bor-right bor-bottom"><div class="devin-table-dash centered pad-box"><i class="palette-Deep-Purple-500 bg ti-close"></i> <div class="palette-Deep-Purple-500 text value">648</div> <div class="label">Items Sold</div></div></div>
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
              <el-input v-model="form.label"></el-input>
            </el-form-item>
          </div>
          <div class="col-sm-3 col-sm-12 col-xs-12">
            <el-form-item label="Fee">
            <el-input v-model="form.label"></el-input>
            </el-form-item>
          </div>
          <div class="col-sm-3 col-sm-12 col-xs-12">
            <button type="button" class="el-button el-button--warning is-plain"><span>Add Fee</span></button>
          </div>
        </el-row>
        <div class="row">
          <div class="col-xs-12 table-responsive">
              <table class="table table-striped">
                  <thead>
                      <tr>
                          <th>Qty</th>
                          <th>Product</th>
                          <th>Serial #</th>
                          <th>Description</th>
                          <th>Subtotal</th>
                      </tr>
                  </thead>
                  <tbody>
                      <tr>
                          <td>1</td>
                          <td>Call of Duty</td>
                          <td>455-981-221</td>
                          <td>El snort testosterone trophy driving gloves handsome</td>
                          <td>$64.50</td>
                      </tr>
                      <tr>
                          <td>1</td>
                          <td>Need for Speed IV</td>
                          <td>247-925-726</td>
                          <td>Wes Anderson umami biodiesel</td>
                          <td>$50.00</td>
                      </tr>
                      <tr>
                          <td>1</td>
                          <td>Monsters DVD</td>
                          <td>735-845-642</td>
                          <td>Terry Richardson helvetica tousled street art master</td>
                          <td>$10.70</td>
                      </tr>
                      <tr>
                          <td>1</td>
                          <td>Grown Ups Blue Ray</td>
                          <td>422-568-642</td>
                          <td>Tousled lomo letterpress</td>
                          <td>$25.99</td>
                      </tr>
                  </tbody>
              </table>
          </div>
         </div>
        <div class="box-header with-border">
          <h3 class="box-title">Date & Assign </h3>
        </div>
        <el-form-item label="Taken Date">
          <el-col :span="5">
            <el-date-picker
              v-model="takenDate"
              type="datetime"
              placeholder="Select date and time">
            </el-date-picker>
          </el-col>
           <button type="button" class="el-button el-button--warning is-plain"><span>Add</span></button>
        </el-form-item>
        <div class="section section-info">
          <p class="info-time">Schedule</p>
          <div class="info-block">
            <dl>
              <dt><i class="ti-stats-up"></i></dt>
              <dt>13/11/2018</dt> 
              <dt>DHKT DANANG</dt>
              <dd>Nguyen Vinh Minh - Leader</dd>
              <dd>Duc Thai - Support</dd>
              <dd><el-button :plain="true" @click="dialogFormVisible = true">Edit Date Taken</el-button></dd>
            </dl>
          </div>
          <div class="info-block">
            <dl>
              <dt><i class="ti-stats-up"></i></dt>
              <dt>13/11/2018</dt> 
              <dd>Nguyen Vinh Minh - Leader</dd>
              <dd>Duc Thai - Support</dd>
            </dl>
          </div>
          <div class="info-block last">
            <dl>
              <dt>
                <i class="ti-stats-down"></i>
              </dt> 
              <dt>28.44</dt> 
              <dd>Market Cap</dd>
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
          <span :class="{ 'grey-text' :checked}">12312312</span><br>
          <el-checkbox v-model="checked">Manual Total</el-checkbox>
        </el-form-item>
        <el-form-item label="Input Total">
          <el-input :disabled="!checked"></el-input>
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
              v-model="takenDate"
              type="datetime"
              placeholder="Select date and time">
            </el-date-picker>
          </el-form-item>
          <el-row>
            <el-col :span="8">
              <el-form-item label="Employee" :label-width="formLabelWidth">
                <el-select v-model="form.region" placeholder="Please select a zone">
                  <el-option label="Zone No.1" value="shanghai"></el-option>
                  <el-option label="Zone No.2" value="beijing"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="Role" :label-width="formLabelWidth">
                <el-select v-model="form.region" placeholder="Please select a zone">
                  <el-option label="Zone No.1" value="shanghai"></el-option>
                  <el-option label="Zone No.2" value="beijing"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="Note" :label-width="formLabelWidth">
                <el-input type="textarea"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="24">
              <el-button type="success" style="width: 100%" @click="dialogFormVisible = false">ADD EMPLOYEE</el-button>
            </el-col><br>
          </el-row>
          <div class="wrap">
            <div class="box-header with-border">
              <h3 class="box-title">List Employee</h3>
            </div>
            <div class="row">
              <div class="employee-item col-md-6">
                <div class="col-md-2">
                  <h1 class="text-white"><img alt="profile" class="img-circle palette-Grey-900 bg" src="http://api.randomuser.me/portraits/thumb/men/46.jpg"></h1>
                </div>
                <div class="col-md-8">
                  <h3 style="font-size: 18px; margin-bottom: 0">D'Agustino Gavin</h3>
                  <p>Photographer</p>
                </div>
              </div>
              <div class="employee-item col-md-6">
                <div class="col-md-2">
                  <h1 class="text-white"><img alt="profile" class="img-circle palette-Grey-900 bg" src="http://api.randomuser.me/portraits/thumb/men/46.jpg"></h1>
                </div>
                <div class="col-md-8">
                  <h4>D'Agustino Gavin</h4>
                  <p>Photographer</p>
                </div>
              </div>
              <div class="employee-item col-md-6">
                <div class="col-md-2">
                  <h1 class="text-white"><img alt="profile" class="img-circle palette-Grey-900 bg" src="http://api.randomuser.me/portraits/thumb/men/46.jpg"></h1>
                </div>
                <div class="col-md-8">
                  <h4>D'Agustino Gavin</h4>
                  <p>Photographer</p>
                </div>
              </div>
              <div class="employee-item col-md-6">
                <div class="col-md-2">
                  <h1 class="text-white"><img alt="profile" class="img-circle palette-Grey-900 bg" src="http://api.randomuser.me/portraits/thumb/men/46.jpg"></h1>
                </div>
                <div class="col-md-8">
                  <h4>D'Agustino Gavin</h4>
                  <p>Photographer</p>
                </div>
              </div>
            </div>
          </div>
          <div class="box-header with-border">
            <h3 class="box-title">Plan</h3>
          </div>
          <el-row>
            <el-col :span="8">
              <el-form-item label="Date Taken" :label-width="formLabelWidth">
                <el-date-picker
                  v-model="takenDate"
                  type="datetime"
                  placeholder="Select date and time">
                </el-date-picker>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="Clothes" :label-width="formLabelWidth">
                <el-select v-model="form.region" placeholder="Please select a zone">
                  <el-option label="Zone No.1" value="shanghai"></el-option>
                  <el-option label="Zone No.2" value="beijing"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="Location" :label-width="formLabelWidth">
                <el-input></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="18">
              <el-form-item>
                <el-input type="textarea" placeholder="Note...."></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="1">
              <el-form-item>
              </el-form-item>
            </el-col>
            <el-col :span="4">
              <el-button type="success" @click="dialogFormVisible = false">ADD PLAN</el-button>
            </el-col>
          </el-row>
          <el-table :data="gridData">
            <el-table-column property="date" label="Date" width="150"></el-table-column>
            <el-table-column property="name" label="Name" width="200"></el-table-column>
            <el-table-column property="address" label="Address"></el-table-column>
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
      dialogFormVisible: false,
      checked: false,
      contract: null,
      packages: null,
      takenDate: null,
      address: null,
      gridData: [
        {
          date: '2016-05-02',
          name: 'John Smith',
          address: 'No.1518,  Jinshajiang Road, Putuo District'
        }, {
          date: '2016-05-04',
          name: 'John Smith',
          address: 'No.1518,  Jinshajiang Road, Putuo District'
        }, {
          date: '2016-05-01',
          name: 'John Smith',
          address: 'No.1518,  Jinshajiang Road, Putuo District'
        }, {
          date: '2016-05-03',
          name: 'John Smith',
          address: 'No.1518,  Jinshajiang Road, Putuo District'
        }
      ],
      form: {
        representative: '',
        phone1: '',
        phone2: '',
        city: '',
        group: '',
        client: '',
        label: '',
        code: '',
        numerator: '',
        members: [],
        deposit: '',
        note: '',
        access_token: '',
        taken_dates: [],
        packages: [],
        costs: [],
        total: ''
      },
      options4: [],
      list: [],
      loading: false,
      states: [
        'Alabama', 'Alaska', 'Arizona',
        'Arkansas', 'California', 'Colorado',
        'Connecticut', 'Delaware', 'Florida',
        'Georgia', 'Hawaii', 'Idaho', 'Illinois',
        'Indiana', 'Iowa', 'Kansas', 'Kentucky',
        'Louisiana', 'Maine', 'Maryland',
        'Massachusetts', 'Michigan', 'Minnesota',
        'Mississippi', 'Missouri', 'Montana',
        'Nebraska', 'Nevada', 'New Hampshire',
        'New Jersey', 'New Mexico', 'New York',
        'North Carolina', 'North Dakota', 'Ohio',
        'Oklahoma', 'Oregon', 'Pennsylvania',
        'Rhode Island', 'South Carolina',
        'South Dakota', 'Tennessee', 'Texas',
        'Utah', 'Vermont', 'Virginia',
        'Washington', 'West Virginia', 'Wisconsin',
        'Wyoming'
      ]
    }
  },
  created () {
    this.$http.get(`api/contracts/${this.$route.params.id}`).then(res => {
      this.contract = res.body
      this.form = this.contract
      console.log(this.form)
    })
  },
  methods: {
    onSubmit () {
      console.log(12)
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
</style>
