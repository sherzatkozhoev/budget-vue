<template>
  <el-card>
    <el-form :model="formData" ref="budgetForm" :rules="rules" lable-position="top">
      <el-form-item label="Type" prop="type">
        <el-select class="form-select" v-model="formData.type" placeholder="Choose type...">
          <el-option lable="Income" value="INCOME" />
          <el-option lable="Expense" value="EXPENSE" />
        </el-select>
      </el-form-item>
      <el-form-item label="Comments" prop="comment">
        <el-input v-model="formData.comment" />
      </el-form-item>
      <el-form-item label="Value" prop="value">
        <el-input v-model.number="formData.value" />
      </el-form-item>
      <el-button @click="onSubmit" type="primary">Submit</el-button>
    </el-form>
  </el-card>
</template>

<script>
export default {
  name: 'AppForm',
  data: () => {
    const checkValue = (rule, value, callback) => {
      if (value === 0) {
        callback(new Error('Value must not be zero'));
      } else {
        callback();
      }
    };

    return {
      formData: {
        type: 'INCOME',
        comment: '',
        value: 0
      },
      rules: {
        type: [
          { required: true, message: 'Please select type', trigger: 'blur' }
        ],
        comment: [
          { required: true, message: 'Please input comment', trigger: 'change' }
        ],
        value: [
          { required: true, message: 'Please input value', trigger: 'change' },
          { type: 'number', message: 'Value must be a number', trigger: 'change' },
          { validator: checkValue, trigger: 'change' }
        ]
      }
    }
  },
  methods: {
    onSubmit () {
      this.$refs.budgetForm.validate(valid => {
        if (valid) {
          this.updateValue();
          this.$emit('submitForm', { ...this.formData });
          this.$refs.budgetForm.resetFields();
        }
      });
    },
    updateValue () {
      const { value, type } = this.formData;

      this.formData.value = type === 'INCOME' ? Math.abs(value) : -Math.abs(value);
    }
  }
}
</script>

<style scoped lang="scss">
.form-select {
  width: 100%;
}
</style>
