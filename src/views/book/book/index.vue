<template>
	<el-card>
		<el-form :inline="true" :model="state.queryForm" @keyup.enter="getDataList()">
					<el-form-item>
			  <el-input v-model="state.queryForm.bookName" placeholder="书名"></el-input>
			</el-form-item>
			<el-form-item>
			  <el-input v-model="state.queryForm.author" placeholder="作者"></el-input>
			</el-form-item>
			<el-form-item>
			  <el-input v-model="state.queryForm.issueYear" placeholder="出版年份"></el-input>
			</el-form-item>
			<el-form-item>
			  <el-input v-model="state.queryForm.isbn" placeholder="ISBN"></el-input>
			</el-form-item>
			<el-form-item>
			  <el-input v-model="state.queryForm.bookDesc" placeholder="描述"></el-input>
			</el-form-item>
			<el-form-item>
				<el-button @click="getDataList()">查询</el-button>
			</el-form-item>
			<el-form-item>
				<el-button  type="primary" @click="addOrUpdateHandle()">新增</el-button>
			</el-form-item>
			<el-form-item>
				<el-button  type="danger" @click="deleteBatchHandle()">删除</el-button>
			</el-form-item>
		</el-form>
		<el-table v-loading="state.dataListLoading" :data="state.dataList" border style="width: 100%" @selection-change="selectionChangeHandle">
			<el-table-column type="selection" header-align="center" align="center" width="50"></el-table-column>
			<el-table-column prop="id" label="id" header-align="center" align="center"></el-table-column>
			<el-table-column prop="bookName" label="书名" header-align="center" align="center"></el-table-column>
			<el-table-column prop="author" label="作者" header-align="center" align="center"></el-table-column>
			<el-table-column prop="issueYear" label="出版年份" header-align="center" align="center"></el-table-column>
			<el-table-column prop="isbn" label="ISBN" header-align="center" align="center"></el-table-column>
			<el-table-column prop="createTime" label="创建时间" header-align="center" align="center"></el-table-column>
			<el-table-column prop="logo" label="封面" header-align="center" align="center"></el-table-column>
			<el-table-column prop="bookDesc" label="描述" header-align="center" align="center"></el-table-column>
			<el-table-column prop="bookContent" label="图书内容" header-align="center" align="center"></el-table-column>
			<el-table-column label="操作" fixed="right" header-align="center" align="center" width="150">
				<template #default="scope">
					<el-button  type="primary" link @click="addOrUpdateHandle(scope.row.id)">修改</el-button>
					<el-button  type="primary" link @click="deleteBatchHandle(scope.row.id)">删除</el-button>
				</template>
			</el-table-column>
		</el-table>
		<el-pagination
			:current-page="state.page"
			:page-sizes="state.pageSizes"
			:page-size="state.limit"
			:total="state.total"
			layout="total, sizes, prev, pager, next, jumper"
			@size-change="sizeChangeHandle"
			@current-change="currentChangeHandle"
		>
		</el-pagination>

		<!-- 弹窗, 新增 / 修改 -->
		<add-or-update ref="addOrUpdateRef" @refreshDataList="getDataList"></add-or-update>
	</el-card>
</template>
<style>
  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
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
<script setup lang="ts" name="BookBookIndex">
import { useCrud } from '@/hooks'
import { reactive, ref } from 'vue'
import AddOrUpdate from './add-or-update.vue'
import { IHooksOptions } from '@/hooks/interface'

const state: IHooksOptions = reactive({
	dataListUrl: '/book/book/page',
	deleteUrl: '/book/book',
	queryForm: {
		bookName: '', 
		author: '', 
		issueYear: '', 
		isbn: '', 
		bookDesc: ''
	}
})

const addOrUpdateRef = ref()
const addOrUpdateHandle = (id?: number) => {
	addOrUpdateRef.value.init(id)
}

const { getDataList, selectionChangeHandle, sizeChangeHandle, currentChangeHandle, deleteBatchHandle } = useCrud(state)
</script>
