<!--
 * @Author: your name
 * @Date: 2019-12-06 15:51:53
 * @LastEditTime : 2019-12-29 18:08:19
 * @LastEditors  : Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \express-project\admin\src\views\CategoriesEdit.vue
 -->
<!--  -->
<template>
  <div>
    <h1>{{id ? '编辑' : '新建'}}物品</h1>
    <el-form @submit.native.prevent="save">
      <el-form-item label="名称">
        <el-input v-model="model.name"></el-input>
      </el-form-item>
      <el-form-item label="图标">
        <el-upload
          class="avatar-uploader"
          :action="$httpsBaseUrl + '/upload'"
          :headers="getAuthHeaders()"
          :show-file-list="false"
          :on-success="afterUpload"
        >
          <img v-if="model.icon" :src="model.icon" class="avatar">
          <i v-else class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" native-type="submit">保存</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script lang='ts'>
import { Vue, Component, Prop } from 'vue-property-decorator';
import { editItem, addItem, getItemInfo } from '@/api/item';

@Component
// 导入的其他文件 例如：import moduleName from 'modulePath';
export default class ItemsEdit extends Vue {
  public model: GoodsModel = {
    name: '',
    icon: '',
  };
  @Prop({type: String}) public id!: string;
  public created() {
    this.id && this.fetch();
  }
  // 通过id将新增和编辑页面作区分
  public async save() {
    let res;
    if (this.id) {
      res = await editItem(this.id, this.model);
    } else {
      res = await addItem(this.model);
    }
    this.$router.push('/items/list');
    this.$message({
      type: 'success',
      message: '保存成功',
    });
  }
  public async fetch() {
    const res: any = await getItemInfo(this.id);
    this.model = res.data;
  }
  public afterUpload(res: any) {
    this.model.icon = res.url;
  }
}
</script>

<style scoped>
.avatar-uploader>>>.el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader>>>.el-upload:hover {
  border-color: #409EFF;
}
.avatar-uploader-icon {
  font-size: 28px;
  color: #8c939d;
  width: 178px;
  height: 178px;
  line-height: 178px;
  text-align: center;
}
.avatar {
  width: 178px;
  height: 178px;
  display: block;
}
</style>
