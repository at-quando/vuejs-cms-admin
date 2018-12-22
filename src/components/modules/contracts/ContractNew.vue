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
          <el-col :span="8">
            <el-form-item label="Phone 1">
              <el-input v-model="form.phone1"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="Phone 2">
              <el-input v-model="form.phone2"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="5">
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
          </el-col>
          <el-col :span="5">
            <el-form-item label="Group">
              <el-input v-model="form.group"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
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
          </el-col>
          <el-col :span="5">
            <el-form-item label="Client Label">
              <el-input v-model="form.label"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="5">
            <el-form-item label="Auto Code">
              <span>{{form.code = client.key + form.group + form.label}}</span>
            </el-form-item>
          </el-col>
          <el-col :span="5">
            <el-form-item label="Numerator">
              <el-input v-model="form.numerator"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-col>
          <el-form-item label="Access Token">
            <span>{{form.access_token}}</span>
          </el-form-item>
        </el-col>
        <hr>
        <div class="box-header with-border">
          <h3 class="box-title">Payment</h3>
        </div>
        <el-form-item label="Deposit">
          <el-input v-model="form.deposit"></el-input>
        </el-form-item>
        <el-form-item label="Basic Package">
          <el-select v-model="packages" placeholder="please select your zone">
            <el-option label="Zone one" value="shanghai"></el-option>
            <el-option label="Zone two" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="Taken Date">
          <el-col :span="11">
            <el-date-picker
              v-model="takenDate"
              type="datetime"
              placeholder="Select date and time">
            </el-date-picker>
          </el-col>
        </el-form-item>
        <el-form-item label="Addresss">
          <el-input v-model="address"></el-input>
        </el-form-item>
        <el-form-item label="Note">
          <el-input type="textarea" v-model="form.note"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="success" @click="onSubmit">Create</el-button>
          <el-button>Cancel</el-button>
        </el-form-item>
      </el-form>
    </new>
  </div>
</template>
<script>
import New from '../../partials/New.vue'

export default {
  name: 'ContractNew',
  components: {
    'new': New
  },
  data () {
    return {
      packages: null,
      takenDate: null,
      address: null,
      client: {},
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
        products: [],
        costs: [],
        total: ''
      },
      cities: [],
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
  mounted () {
    this.list = this.states.map(item => {
      return { value: item, label: item }
    })
  },
  created () {
    this.$http.get(`api/cities`).then(res => {
      this.cities = res.body
    })
  },
  methods: {
    onSubmit () {
      const objTaken = {
        start_time: this.$options.filters.formatDate(this.takenDate),
        note: this.form.note,
        address: this.address
      }
      this.form.taken_dates.push(objTaken)
      this.form.packages.push(this.packages)
      this.$http.post('api/contracts', this.form)
        .then(res => {
          if (response.status === 201) {
            console.log(12, res.body)
          }
        })
        .catch(function (e) {
          if (e.status === 401) {
            console.log(33342)
          }
        })
    },
    remoteMethod (query) {
      if (query !== '') {
        this.loading = true
        setTimeout(() => {
          this.loading = false
          this.options4 = this.list.filter(item => {
            return item.label.toLowerCase()
              .indexOf(query.toLowerCase()) > -1
          })
        }, 200)
      } else {
        this.options4 = []
      }
    }
  },
  watch: {
    client: (val) => {
      this.form.client = client.name
    }
  }
}
</script>
<style lang="scss">
</style>