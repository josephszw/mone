<!--
  Copyright 2020 Xiaomi

     Licensed under the Apache License, Version 2.0 (the "License");
     you may not use this file except in compliance with the License.
     You may obtain a copy of the License at

         http://www.apache.org/licenses/LICENSE-2.0

     Unless required by applicable law or agreed to in writing, software
     distributed under the License is distributed on an "AS IS" BASIS,
     WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     See the License for the specific language governing permissions and
     limitations under the License.
  -->

<template>
  <el-dropdown placement="bottom" size="small" @command="handleChange">
    <el-button class="d2-mr btn-text can-hover" type="text">
      <d2-icon name="font" style="font-size: 16px;"/>
    </el-button>
    <el-dropdown-menu slot="dropdown">
      <el-dropdown-item command="default">
        <d2-icon :name="iconName('default')" class="d2-mr-5"/>默认
      </el-dropdown-item>
      <el-dropdown-item command="medium">
        <d2-icon :name="iconName('medium')" class="d2-mr-5"/>中
      </el-dropdown-item>
      <el-dropdown-item command="small">
        <d2-icon :name="iconName('small')" class="d2-mr-5"/>小
      </el-dropdown-item>
      <el-dropdown-item command="mini">
        <d2-icon :name="iconName('mini')" class="d2-mr-5"/>最小
      </el-dropdown-item>
    </el-dropdown-menu>
  </el-dropdown>
</template>

<script>
import { mapState, mapMutations, mapActions } from 'vuex'
export default {
  name: 'd2-header-size',
  computed: {
    ...mapState('d2admin/size', [
      'value'
    ])
  },
  watch: {
    // 注意 这里是关键
    // 因为需要访问 this.$ELEMENT 所以只能在这里使用这种方式
    value: {
      handler (val) {
        if (this.$ELEMENT.size !== val) {
          // 设置 element 全局尺寸
          this.$ELEMENT.size = val
          // 清空缓存设置
          this.pageKeepAliveClean()
          // 刷新此页面
          const { path, query } = this.$route
          this.$router.replace({
            path: '/redirect/' + JSON.stringify({ path, query })
          })
        }
      },
      immediate: true
    }
  },
  methods: {
    ...mapMutations({
      pageKeepAliveClean: 'd2admin/page/keepAliveClean'
    }),
    ...mapActions({
      sizeSet: 'd2admin/size/set'
    }),
    handleChange (value) {
      this.sizeSet(value)
    },
    iconName (name) {
      return name === this.value ? 'dot-circle-o' : 'circle-o'
    }
  }
}
</script>
