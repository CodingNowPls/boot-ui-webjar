<template>
  <div>
    <el-dialog
      :close-on-click-modal="false"
      :modal-append-to-body="false"
      v-bind="$attrs"
      @close="onClose"
      @open="onOpen"
      v-on="$listeners"
    >
      <el-row :gutter="0">
        <el-form
          ref="elForm"
          :model="formData"
          :rules="rules"
          label-width="100px"
          size="small"
        >
          <el-col :span="24">
            <el-form-item
              label="选项名"
              prop="label"
            >
              <el-input
                v-model="formData.label"
                clearable
                placeholder="请输入选项名"
              />
            </el-form-item>
          </el-col>
          <el-col :span="24">
            <el-form-item
              label="选项值"
              prop="value"
            >
              <el-input
                v-model="formData.value"
                clearable
                placeholder="请输入选项值"
              >
                <el-select
                  slot="append"
                  v-model="dataType"
                  :style="{width: '100px'}"
                >
                  <el-option
                    v-for="(item, index) in dataTypeOptions"
                    :key="index"
                    :disabled="item.disabled"
                    :label="item.label"
                    :value="item.value"
                  />
                </el-select>
              </el-input>
            </el-form-item>
          </el-col>
        </el-form>
      </el-row>
      <div slot="footer">
        <el-button
          type="primary"
          @click="handleConfirm"
        >
          确定
        </el-button>
        <el-button @click="close">
          取消
        </el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
import { isNumberStr } from '@/utils/index'

export default {
  components: {},
  inheritAttrs: false,
  props: [],
  data() {
    return {
      id: 100,
      formData: {
        label: undefined,
        value: undefined
      },
      rules: {
        label: [
          {
            required: true,
            message: '请输入选项名',
            trigger: 'blur'
          }
        ],
        value: [
          {
            required: true,
            message: '请输入选项值',
            trigger: 'blur'
          }
        ]
      },
      dataType: 'string',
      dataTypeOptions: [
        {
          label: '字符串',
          value: 'string'
        },
        {
          label: '数字',
          value: 'number'
        }
      ]
    }
  },
  computed: {},
  watch: {
    // eslint-disable-next-line func-names
    'formData.value': function(val) {
      this.dataType = isNumberStr(val) ? 'number' : 'string'
    }
  },
  created() {
  },
  mounted() {
  },
  methods: {
    onOpen() {
      this.formData = {
        label: undefined,
        value: undefined
      }
    },
    onClose() {
    },
    close() {
      this.$emit('update:visible', false)
    },
    handleConfirm() {
      this.$refs.elForm.validate(valid => {
        if (!valid) return
        if (this.dataType === 'number') {
          this.formData.value = parseFloat(this.formData.value)
        }
        this.formData.id = this.id++
        this.$emit('commit', this.formData)
        this.close()
      })
    }
  }
}
</script>
