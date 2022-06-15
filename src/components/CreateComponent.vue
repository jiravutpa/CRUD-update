<template>
  <div class="vld-parent">
    <loading
      v-model:active="isLoading"
      :can-cancel="true"
      :is-full-page="false"
    />
    <div class="row justify-content-center">
      <div class="col-md-6">
        <h1>Create</h1>
        <form @submit.prevent="handleSubmitForm">
          <div class="form-group">
            <label for="name">Brand (ID)</label>
            <select v-model="user.brandId" class="form-select">
              <option
                v-for="(item, index) in optionBrands"
                :key="index"
                :value="item"
              >
                {{ item.brandName }}
              </option>
            </select>
          </div>
          <div class="form-group">
            <label for="name">Model Code</label>
            <input
              type="text"
              class="form-control"
              v-model="user.modelCode"
              required
            />
          </div>
          <div class="form-group">
            <label for="name">Name</label>
            <input
              type="text"
              class="form-control"
              v-model="user.name"
              required
            />
          </div>
          <div class="form-group">
            <label for="email">Model Year</label>
            <input
              type="number"
              class="form-control"
              v-model="user.modelYear"
              required
            />
          </div>
          <!-- <div class="form-group" >
          <label for="fuel">Fuel</label><br>
          <div v-for="(itemf, index) in optionFuel" :key="index" :value="itemf">
            <input class="form-check-input" type="radio" name="fuel" key="index.id" value="itemf.name" v-model="user.fuel" required>
            <label class="form-check-label" for="fuel">{{itemf.name}}</label>&nbsp;
          </div>
        </div> -->
          <div class="form-group">
            <label for="fuel">Fuel</label><br />
            <div
              v-for="(itemf, index) in optionFuel"
              :key="index"
              :value="itemf"
            >
              <!-- <select v-model="user.fuel" class="form-select">
            <option v-for="(itemf, index) in optionFuel" :key="index" :value="itemf" >{{itemf.name}}</option> -->
              <input
                type="radio"
                name="fuel"
                :key="index"
                :value="itemf"
                v-model="user.fuel"
              />
              <a>{{ itemf.name }}</a>
              <!-- </select> -->
            </div>
          </div>
          <br />
          <div class="form-group">
            <button class="btn btn-success btn-block">Create</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import Loading from "vue-loading-overlay";
import "vue-loading-overlay/dist/vue-loading.css";

// import Multiselect from "vue-multiselect";

export default {
  //   components: { Multiselect },
  created() {
    this.op();
  },
  data() {
    return {
      isLoading: false,
      optionBrands: [],
      optionFuel: [
        {
          id: "D",
          name: "ดีเซล",
        },
        {
          id: "S",
          name: "เบนซิน",
        },
      ],

      user: {
        brandId: "",
        modelCode: "",
        name: "",
        modelYear: "",
        fuel: "",
      },
    };
  },
  components: {
    Loading,
  },
  methods: {
    logModel() {
      console.log(this.user.brandId);
    },
    handleSubmitForm() {
      this.isLoading = true;
      setTimeout(() => {
        this.isLoading = false;
      }, 1000);
      // console.log(this.user.brandId.brandId);
      // console.log(this.user.fuel.id);
      let apiURL =
        "https://dms-backend-dev-dxvb7izyka-as.a.run.app/api/vehicle-model/store";
      axios
        .post(apiURL, {
          brandId: this.user.brandId.brandId,
          modelCode: this.user.modelCode,
          name: this.user.name,
          modelYear: this.user.modelYear,
          fuel: this.user.fuel.id,
        })
        .then(() => {
          this.$router.push("/view");
          this.user = {
            brandId: "",
            modelCode: "",
            name: "",
            modelYear: "",
            fuel: "",
          };
        })
        .catch((error) => {
          console.log(error);
        });
    },
    op() {
      axios
        .get(
          "https://dms-backend-dev-dxvb7izyka-as.a.run.app/api/vehicle-model/brand",
          {
            params: {},
          }
        )
        .then((res) => {
          const arr = [];
          res.data.forEach((value) => {
            arr.push(value);
            console.log(value);
          });
          this.optionBrands = arr;

          console.log(arr);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
