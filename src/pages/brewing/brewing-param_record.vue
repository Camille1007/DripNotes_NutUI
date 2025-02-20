<template>
  <view class="param-record-page">
    <!-- 表单容器 -->
    <nut-form>
      <!-- 咖啡豆信息模块 -->
      <nut-cell-group title="咖啡豆信息">
        <nut-form-item label="产地" required>
          <nut-input
            v-model="formData.origin"
            placeholder="例：埃塞俄比亚耶加雪菲"
            clearable
          />
        </nut-form-item>

        <nut-cell
          title="烘焙度"
          :desc="formData.roastLevel"
          @click="showRoastPicker = true"
        />

        <nut-picker
          v-model:visible="showRoastPicker"
          title="选择烘焙度"
          :columns="roastOptions"
          @confirm="handleRoastConfirm"
        />
      </nut-cell-group>

      <!-- 研磨参数模块 -->
      <nut-cell-group title="研磨参数">
        <nut-cell
          title="磨豆机"
          :desc="selectedGrinder || '点击选择'"
          @click="showGrinderPicker = true"
        />

        <nut-picker
          v-model:visible="showGrinderPicker"
          title="选择磨豆机"
          :columns="grinderOptions"
          @confirm="handleGrinderConfirm"
        >
          <nut-input
            v-model="customGrinder"
            placeholder="手动输入型号"
            class="custom-input"
          />
        </nut-picker>

        <nut-cell title="研磨刻度">
          <nut-input-number
            v-model="formData.grindSize"
            :min="0"
            :max="30"
            step="1"
          />
        </nut-cell>
      </nut-cell-group>

      <!-- 冲煮参数模块 -->
      <nut-cell-group title="冲煮参数">
        <nut-cell title="粉水比">
          <view class="ratio-input">
            <nut-input-number v-model="formData.coffeeWeight"  /> :
            <nut-input-number
              :model-value="formData.coffeeWeight  * 15"
              disabled
            />
          </view>
        </nut-cell>

        <nut-cell
          title="冲煮地点"
          :desc="formData.location.join('  - ')"
          @click="showLocationPicker = true"
        />

        <nut-picker
          v-model:visible="showLocationPicker"
          title="选择地点"
          @confirm="handleLocationConfirm"
        />
      </nut-cell-group>

      <!-- 感官评价 -->
      <nut-cell-group title="感官评价">
        <nut-cell title="综合评分">
          <nut-rate
            v-model="formData.rating"
            :count="5"
            spacing="20"
          />
        </nut-cell>

        <nut-cell title="风味标签">
          <nut-tag
            v-for="tag in flavorTags"
            :key="tag"
            type="primary"
            class="tag-margin"
          >
            {{ tag }}
          </nut-tag>
        </nut-cell>
      </nut-cell-group>
    </nut-form>

    <!-- 提交按钮 -->
    <nut-button
      type="primary"
      block
      class="submit-btn"
      @click="handleSubmit"
    >
      保存冲煮记录
    </nut-button>
  </view>
</template>

<script setup>
import { ref, reactive } from 'vue'

// 表单数据
const formData = reactive({
  origin: '',
  roastLevel: '中焙',
  grindSize: 20,
  coffeeWeight: 15,
  location: ['广东省', '深圳市', '南山区'],
  rating: 4
})

// 选择器控制
const showRoastPicker = ref(false)
const showGrinderPicker = ref(false)
const showLocationPicker = ref(false)

// 预设选项
const roastOptions = [['浅焙', '中焙', '深焙']]
const grinderOptions = [[
  { text: 'C40 MK4', value: 'c40' },
  { text: '泰摩 X-Lite', value: 'x-lite' },
  { text: '1ZPresso K-Max', value: 'kmax' }
]]
const locationData = reactive({
  province: [
    { id: 1, name: '北京' },
    { id: 2, name: '广西' },
    { id: 3, name: '江西' },
    { id: 4, name: '四川' }
  ],
  city: [
    { id: 7, name: '朝阳区' },
    { id: 8, name: '崇文区' },
    { id: 9, name: '昌平区' },
    { id: 6, name: '石景山区' }
  ],
  country: [
    { id: 3, name: '八里庄街道' },
    { id: 9, name: '北苑' },
    { id: 4, name: '常营乡' }
  ],
  town: []
})
const flavorTags = ['柑橘', '焦糖', '花香', '坚果']

// 事件处理
const handleRoastConfirm = ({ selectedValue }) => {
  formData.roastLevel  = selectedValue[0]
}

const handleGrinderConfirm = ({ selectedValue }) => {
  formData.grinder  = selectedValue[0]
}

const handleLocationConfirm = ({ selectedValue }) => {
  formData.location  = selectedValue
}

const handleSubmit = () => {
  console.log(' 提交数据：', JSON.stringify(formData))
  // 调用保存接口...
}
</script>

<style lang="scss" scoped>
// 引入页面级 SCSS 文件
//@import '../../assets/styles/pages/param-record.scss'
</style>
