<template>
  <form class="insurance-form stack-large" @submit.prevent="onSubmit">
    <h2 class="label-wrapper">
      <label class="label__lg">12月27日徒步投保信息收集</label>
    </h2>
    <p class="hint">
      自愿选择是否参保，如需参保，请填写信息；信息仅用于购买保险，会全程做好保密
    </p>

    <div class="form-item">
      <label for="cn-name"><span class="required">*</span>01 中文名</label>
      <input
        id="cn-name"
        type="text"
        autocomplete="off"
        placeholder="请输入"
        v-model.trim="form.name" />
      <div class="error" v-if="errors.name">{{ errors.name }}</div>
    </div>

    <div class="form-item">
      <label for="phone"><span class="required">*</span>02 手机号码</label>
      <input
        id="phone"
        type="tel"
        inputmode="numeric"
        autocomplete="off"
        placeholder="请输入"
        v-model.trim="form.phone" />
      <div class="error" v-if="errors.phone">{{ errors.phone }}</div>
    </div>

    <div class="form-item">
      <label for="id-no"><span class="required">*</span>03 身份证号码</label>
      <input
        id="id-no"
        type="text"
        autocomplete="off"
        placeholder="请输入"
        v-model.trim="form.idNo" />
      <div class="error" v-if="errors.idNo">{{ errors.idNo }}</div>
    </div>

    <button type="submit" class="btn btn__primary btn__lg submit-btn">提交</button>
  </form>
  
</template>

<script>
export default {
  name: "InsuranceForm",
  data() {
    return {
      form: {
        name: "",
        phone: "",
        idNo: "",
      },
      errors: {
        name: "",
        phone: "",
        idNo: "",
      },
    };
  },
  methods: {
    validate() {
      const nextErrors = { name: "", phone: "", idNo: "" };

      if (!this.form.name) {
        nextErrors.name = "请输入中文名";
      }

      const phoneRegex = /^1[3-9]\d{9}$/;
      if (!this.form.phone) {
        nextErrors.phone = "请输入手机号";
      } else if (!phoneRegex.test(this.form.phone)) {
        nextErrors.phone = "手机号格式不正确";
      }

      // 简单身份证校验（15位或18位，末位可为X）
      const idRegex = /^(\d{15}|\d{17}[\dXx])$/;
      if (!this.form.idNo) {
        nextErrors.idNo = "请输入身份证号码";
      } else if (!idRegex.test(this.form.idNo)) {
        nextErrors.idNo = "身份证号码格式不正确";
      }

      this.errors = nextErrors;
      return !nextErrors.name && !nextErrors.phone && !nextErrors.idNo;
    },
    onSubmit() {
      if (!this.validate()) {
        return;
      }
      this.$emit("form-submitted", { ...this.form });
    },
  },
};
</script>

<style scoped>
.insurance-form {
  margin-top: 1rem;
}
.hint {
  color: #666;
  font-size: 0.95rem;
  line-height: 1.6;
}
.form-item {
  display: block;
}
.form-item + .form-item {
  margin-top: 1.25rem;
}
label {
  font-weight: 600;
  display: block;
  margin-bottom: 0.4rem;
}
.required {
  color: #e53935;
  margin-right: 0.25rem;
}
input {
  display: inline-block;
  margin-top: 0.2rem;
  width: 100%;
  min-height: 4.4rem;
  padding: 0.4rem 0.8rem;
  border: 2px solid #565656;
}
.error {
  color: #e53935;
  margin-top: 0.25rem;
  font-size: 0.9rem;
}
.submit-btn {
  margin-top: 1rem;
}
</style>

