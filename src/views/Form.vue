<template>
  <div class="form">
    <div class="content-container" id="form-container">
      <div class="title">
        <p class="NotoSansCJKtc-Regular">立即連署，要求十大零售通路，減少塑膠包裝！</p>
      </div>
      <div class="subtitle">
        <p class="NotoSansCJKtc-Regular">綠色和平會把你的訴求，轉達給各企業。</p>
      </div>
      <div class="marts-icon">
        <el-row :gutter="10">
          <el-col :xs="{span: 8}" :sm="{span: 8}" :md="{span: 4, offset: 1}">
            <img src="@/assets/img/marts/a-mart.png" width="100%" alt />
          </el-col>
          <el-col :xs="{span: 8}" :sm="{span: 8}" :md="{span: 4}">
            <img src="@/assets/img/marts/family.png" width="100%" alt />
          </el-col>
          <el-col :xs="{span: 8}" :sm="{span: 8}" :md="{span: 4}">
            <img src="@/assets/img/marts/px-mart.png" width="100%" alt />
          </el-col>
          <el-col :xs="{span: 8}" :sm="{span: 8}" :md="{span: 4}">
            <img src="@/assets/img/marts/costco.png" width="100%" alt />
          </el-col>
          <el-col :xs="{span: 8}" :sm="{span: 8}" :md="{span: 4}">
            <img src="@/assets/img/marts/wellcome.png" width="100%" alt />
          </el-col>
          <el-col :xs="{span: 8}" :sm="{span: 8}" :md="{span: 4, offset: 1}">
            <img src="@/assets/img/marts/simple-mart.png" width="100%" alt />
          </el-col>
          <el-col :xs="{span: 8}" :sm="{span: 8}" :md="{span: 4}">
            <img src="@/assets/img/marts/rt-mart.png" width="100%" alt />
          </el-col>
          <el-col :xs="{span: 8}" :sm="{span: 8}" :md="{span: 4}">
            <img src="@/assets/img/marts/711.png" width="100%" alt />
          </el-col>
          <el-col :xs="{span: 8}" :sm="{span: 8}" :md="{span: 4}">
            <img src="@/assets/img/marts/carrefour.png" width="100%" alt />
          </el-col>
        </el-row>
      </div>
      <div class="counting-bar-container">
        <div class="counting-bar-out">
          <el-progress :stroke-width="6" :percentage="percent" :show-text="false" :color="barColor"></el-progress>
        </div>
        <div class="counting-text">
          <p>
            連署人數：
            <span class="inlight">{{total.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}}</span>; 目標：
            <span class="inlight">{{goal.toLocaleString()}}</span>
          </p>
        </div>
      </div>
      <div class="form-inputs">
        <el-form
          :model="ruleForm"
          :rules="rules"
          ref="ruleForm"
          label-width="120px"
          label-position="top"
          class="NotoSansCJKtc-Regular"
        >
          <el-form-item label="電子信箱" prop="email" required>
            <el-input
              autocomplete="on"
              placeholder="greenpeace@gmail.com"
              v-model="ruleForm.email"
              v-on:blur="checkMail()"
              name="email"
            ></el-input>
            <span class="email-suggestion">
              您是否想輸入：
              <span id="emailSuggestion"></span>
            </span>
          </el-form-item>
          <el-row :gutter="30">
            <el-col :span="10">
              <el-form-item label="姓氏" prop="lastName" required>
                <el-input
                  autocomplete="on"
                  v-model="ruleForm.lastName"
                  placeholder="王"
                  name="last-name"
                ></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="14">
              <el-form-item label="名字" prop="firstName" required>
                <el-input
                  autocomplete="on"
                  v-model="ruleForm.firstName"
                  placeholder="小明"
                  name="first-name"
                ></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :gutter="30">
            <el-col :xs="24" :md="24" :xl="17">
              <el-form-item label="電話（0912345678 或 02-23612351）" prop="phoneNumber" v-if="isDDPage">
                <el-input
                  @change="watchPhone()"
                  autocomplete="on"
                  v-model="ruleForm.phoneNumber"
                  placeholder="0912345678"
                  name="phone"
                ></el-input>
              </el-form-item>
              <el-form-item label="電話（0912345678 或 02-23612351）" prop="phoneNumber" required v-else>
                <el-input
                  @change="watchPhone()"
                  autocomplete="on"
                  v-model="ruleForm.phoneNumber"
                  placeholder="0912345678"
                  name="phone"
                ></el-input>
              </el-form-item>
            </el-col>
            <el-col :xs="12" :md="12" :xl="7">
              <el-form-item label="出生年份" prop="yearOfBirth" required>
                <el-date-picker type="year" v-model="ruleForm.yearOfBirth" style="width: 100%;"></el-date-picker>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :gutter="15">
            <el-col
              :xs="{span: 3, offset: 1}"
              :sm="{span: 3, offset: 1}"
              :md="{span: 3, offset: 1}"
              :xl="{span: 1, offset: 1}"
            >
              <el-form-item label class="check-box">
                <el-checkbox v-model="ruleForm.moreInfo"></el-checkbox>
              </el-form-item>
            </el-col>
            <el-col :xs="20" :sm="20" :md="20" :xl="21">
              <p class="checkbox-text">
                我要即時收到最新專案訊息，知道更多參與和協助的方法！綠色和平尊重並保障您的個人隱私資料，您隨時可取消訂閱，請參考
                <a
                  href="https://www.greenpeace.org/taiwan/policies/privacy-and-cookies/?ref=2020-oceans_sanctuaries_form_homepage"
                  target="_blank"
                  title="隱私保護政策"
                >隱私保護政策</a>。
              </p>
            </el-col>
          </el-row>
          <div class="submit-btn-container">
            <el-form-item>
              <el-button
                round
                class="submit-btn NotoSansCJKtc-Regular"
                @click="submitForm('ruleForm')"
              >立即連署</el-button>
            </el-form-item>
          </div>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";
const qs = require("qs");
var Mailcheck = require("mailcheck");

export default {
  name: "Form",
  props: {
    msg: String,
  },
  data() {
    var validateName = (rule, value, callback) => {
      let nameReg = new RegExp(/^[\u4e00-\u9fa5_a-zA-Z_ ]{1,40}$/);
      if (value === "") {
        callback(new Error("姓名格式不正確，請不要輸入數字或符號"));
      } else if (!nameReg.test(value)) {
        callback(new Error("姓名格式不正確，請不要輸入數字或符號"));
      } else {
        callback();
      }
    };
    return {
      total: 0,
      percent: 0,
      goal: 0,
      barColor: "#eb9062",
      ruleForm: {
        email: "",
        lastName: "",
        firstName: "",
        phoneNumber: "",
        yearOfBirth: "",
        moreInfo: true,
      },
      rules: {
        email: [
          {
            type: "email",
            required: true,
            message: "請輸入電子郵件",
            trigger: "blur",
          },
        ],
        lastName: [
          {
            validator: validateName,
            message: "姓名格式不正確",
            trigger: "blur",
          },
        ],
        firstName: [
          {
            validator: validateName,
            message: "姓名格式不正確，請不要輸入數字或符號",
            trigger: "blur",
          },
        ],
        // phoneNumber: [
        //   {
        //     validator: validatePhone,
        //     message: "電話格式不正確",
        //     trigger: "blur",
        //   },
        // ],
        yearOfBirth: [
          {
            type: "date",
            required: true,
            message: "請選擇出生年份",
            trigger: "blur",
          },
        ],
      },
    };
  },
  computed: {
    isDDPage: function () {
      return this.$route.query.utm_source === "dd"
    }
  },
  created() {
    this.getPetitionNumber();
    if (this.isDDPage) {
      console.log("dd page");
      delete this.rules.phoneNumber;
    } else {
      console.log("normal page");
      this.rules.phoneNumber = [
        {
          validator: this.validatePhone,
          message: "電話格式不正確",
          trigger: "blur",
        },
      ];
    }
  },
  methods: {
    submitForm(formName) {
      // console.log((this.ruleForm.moreInfo ? "Y" : "N"));
      this.$refs[formName].validate((valid) => {
        if (!valid) {
          console.log("error submit!!");
          return false;
        }
        this.yahooADTracking();
        // this.$emit("thankYou");
        this.postForm();
      });
    },
    validatePhone(rule, value, callback) {
      let re_phone = new RegExp(/0\d{1,2}-\d{6,8}/);
      let re_mobile = new RegExp(/((?=(09))[0-9]{10})$/);
      if (value === "") {
        callback(new Error("電話格式不正確"));
      } else if (!(re_phone.test(value) || re_mobile.test(value))) {
        callback(new Error("電話格式不正確"));
      } else {
        callback();
      }
    },
    checkMail() {
      //email suggestion, email correctness
      let domains = [
        "me.com",
        "outlook.com",
        "netvigator.com",
        "cloud.com",
        "live.hk",
        "msn.com",
        "gmail.com",
        "hotmail.com",
        "ymail.com",
        "yahoo.com",
        "yahoo.com.tw",
        "yahoo.com.hk",
      ];
      let topLevelDomains = ["com", "net", "org"];

      Mailcheck.run({
        email: $("[name='email']").val(),
        domains: domains, // optional
        topLevelDomains: topLevelDomains, // optional
        suggested: (suggestion) => {
          $("#emailSuggestion").html(suggestion.full);
          $(".email-suggestion").show();

          $(".email-suggestion").click(function () {
            $("[name='email']").val($("#emailSuggestion").html());
            $(".email-suggestion").hide();
          });
        },
        empty: () => {
          this.emailSuggestion = null;
        },
      });
    },
    async getPetitionNumber() {
      let numSignupTarget = document.querySelector(
        'input[name="numSignupTarget"]'
      )
        ? parseInt(
            document.querySelector('input[name="numSignupTarget"]').value,
            10
          )
        : 0;
      let numResponses = document.querySelector('input[name="numResponses"]')
        ? parseInt(
            document.querySelector('input[name="numResponses"]').value,
            10
          )
        : 0;

      // use the default values if something wrong
      if (isNaN(numResponses) || numResponses < 191854) numResponses += 191854;
      if (isNaN(numSignupTarget) || numSignupTarget < 200000)
        numSignupTarget = 200000;
      if (numResponses > numSignupTarget)
        numSignupTarget = Math.ceil(numResponses / 10000) * 10000;

      this.total = numResponses;
      this.percent = (numResponses / numSignupTarget) * 100;
      this.goal = numSignupTarget;
    },
    async postForm() {
      let $ = (selector) => document.querySelector(selector),
        $all = (selector) => document.querySelectorAll(selector);

      this.$emit("displayCover");
      
      if (this.isDDPage && !this.ruleForm.phoneNumber) {
        this.ruleForm.phoneNumber = ""
      }
      // modify the original form
      $('#mc-form [name="Email"]').value = this.ruleForm.email;
      $('#mc-form [name="LastName"]').value = this.ruleForm.lastName;
      $('#mc-form [name="FirstName"]').value = this.ruleForm.firstName;
      $('#mc-form [name="MobilePhone"]').value = this.ruleForm.phoneNumber;
      $('#mc-form [name="OptIn"]').value = this.ruleForm.moreInfo;
      $('#mc-form [name="Birthdate"]').value = dayjs(
        this.ruleForm.yearOfBirth
      ).format("YYYY-MM-DD");
      //console.log("optin:", $('#mc-form [name="OptIn"]').value);
      // collect values from form
      let formData = new FormData();
      $all("#mc-form input").forEach(function (el) {
        let v = null;
        if (el.type === "checkbox") {
          v = $('#mc-form [name="OptIn"]').value;
        } else {
          v = el.value;
        }

        formData.append(el.name, v);
        //console.log('use', el.name, v)
      });

      return fetch($("#mc-form").action, {
        method: "POST",
        body: formData,
      })
        .then((response) => response.json())
        .then((response) => {
          if (response) {
            //console.log('response', response)
            this.$emit("removeCover");
            this.$emit("thankYou");
          }
        })
        .catch((error) => {
          this.$emit("removeCover");
        });
    },
    watchPhone() {
      if (this.isDDPage) {
        if (!this.ruleForm.phoneNumber) {
          delete this.rules.phoneNumber;
        } else {
          this.rules.phoneNumber = [
            {
              validator: this.validatePhone,
              message: "電話格式不正確",
              trigger: "blur",
            },
          ];
        }
      }
    },
    yahooADTracking() {
      //console.log('yahooADTracking')
      window.dotq = window.dotq || [];
      window.dotq.push({
        projectId: "10000",
        properties: {
          pixelId: "10094925",
          qstrings: {
            et: "custom",
            ea: "submit",
          },
        },
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.form {
  p {
    margin: unset;
  }
  position: relative;
  width: 100%;
  height: auto;
  border-radius: 1px;
  box-shadow: 10px -5px 15px 0 rgba(214, 207, 207, 0.5);
  background-color: #f9f9f9;
  .counting-bar-container {
    padding: 0 4% 0;
    .counting-bar-out {
      border: 1px solid black;
      padding: 2px;
      background-color: white;
      border-radius: 20px;
      width: 100%;
      .counting-bar-in {
        border-radius: 20px;
        background-color: #eb9062;
        width: 80%;
        height: 10px;
      }
    }
    .counting-text {
      color: #644837;
      line-height: 1.4;
      padding-top: 5px;
    }
  }
  .content-container {
    background-color: #f9f9f9;
    padding: 5% 8% 0;
    .title {
      color: #eb9062;
      font-weight: bold;
      letter-spacing: 3pt;
      font-size: 1.4rem;
    }
    .subtitle {
      padding-top: 10px;
      letter-spacing: 3pt;
      color: #644837;
    }
    .form-inputs {
      color: #644837;
      padding: 10px;
      letter-spacing: 1pt;
      width: 92%;
      .check-box {
        margin-bottom: 10px;
      }
      .checkbox-text {
        font-size: 0.8rem;
        // padding-top: 10px;
      }
      .submit-btn-container {
        width: 100%;
        text-align: center;
        padding-top: 20px;
        padding-bottom: 10px;
        .submit-btn {
          background-color: #eb9062;
          color: white;
          letter-spacing: 3pt;
          font-size: 1.2rem;
          border-radius: 50px;
        }
      }
    }
  }
}
@media (min-width: 992px) and (max-width: 1919px) {
  .form {
    .content-container {
      .counting-bar-container {
        padding: 0 5.5% 0;
      }
      .title {
        color: #eb9062;
        font-weight: bold;
        letter-spacing: 3pt;
        font-size: 1.2rem;
      }
      .subtitle {
        padding-top: 10px;
        letter-spacing: 3pt;
      }
      .form-inputs {
        padding: 10px;
        .checkbox-text {
          font-size: 0.8rem;
          padding-top: unset;
        }
        .submit-btn-container {
          padding-top: 10px;
          padding-bottom: 0px;
          .submit-btn {
            font-size: 1rem;
          }
        }
      }
    }
  }
}
@media (max-width: 991px) and (min-width: 768px) {
  .form {
    .content-container {
      .counting-bar-container {
        padding: 0 3.5% 0 !important;
      }
      .title {
        font-size: 2.5rem !important;
        line-height: 1.6;
      }
      .subtitle {
        font-size: 2rem !important;
        padding-top: 10px;
        letter-spacing: 3pt;
      }
      .form-inputs {
        font-size: 1.6rem;
        .submit-btn-container {
          .submit-btn {
            font-size: 1.6rem;
          }
        }
      }
    }
  }
}
@media (max-width: 991px) {
  .form {
    font-size: 1.4rem;
    margin-top: -50vh;
    padding-top: 52vh;
    .content-container {
      .counting-bar-container {
        padding: 0 6% 0;
      }
      .title {
        font-size: 1.8rem;
        line-height: 1.6;
      }
      .subtitle {
        font-size: 1.4rem;
        padding-top: 10px;
        letter-spacing: 3pt;
      }
      .form-inputs {
        padding: 10px;
        .submit-btn-container {
          padding-top: 40px;
          padding-bottom: 10px;
        }
      }
    }
  }
}
</style>
