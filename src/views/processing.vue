<template>
	<div>
		<div class="container">
			<div class="handle-box">
				<el-date-picker
					v-model="startDateValue"
					type="date"
					placeholder="申请时间"
					size='default'
      			/>
				<el-input v-model="query.secondParty" placeholder="承接方" class="handle-input mr10"></el-input>
                <el-select v-model="query.state" placeholder="状态" class="handle-select mr10">
					<el-option key="1" label="待承接" value="待承接"></el-option>
					<el-option key="2" label="加工中" value="加工中"></el-option>
                    <el-option key="2" label="已完成" value="已完成"></el-option>
				</el-select>
				<el-button type="primary" :icon="Search" @click="handleSearch">搜索</el-button>
				<el-button type="primary" :icon="Plus">新增</el-button>
			</div>
			<el-table :data="tableData" border class="table" ref="multipleTable" header-cell-class-name="table-header">
				<el-table-column prop="id" label="ID" width="55" align="center"></el-table-column>
				<el-table-column prop="processingId" label="加工单号"></el-table-column>
				<el-table-column prop="orderId" label="订单编号"></el-table-column>
				<el-table-column prop="sellerId" label="商家编号"></el-table-column>
                <el-table-column prop="submitTime" label="申请时间"></el-table-column>
				<el-table-column prop="quantity" label="加工数目"></el-table-column>
                <el-table-column prop="secondParty" label="承接方"></el-table-column>
                <el-table-column prop="demand" label="加工需求"></el-table-column>
				<el-table-column label="状态" align="center">
					<template #default="scope">
						<el-tag
							:type="scope.row.state === '已完成' ? 'success' : scope.row.state === '待承接' ? 'warning'  : ''">
							{{ scope.row.state }}
						</el-tag>
					</template>
				</el-table-column>
				<el-table-column label="操作" width="320" align="center">
					<template #default="scope">
						<el-button text :icon="View" @click="handleDetail()" v-permiss="15">详情</el-button>
						<el-button text :icon="Edit" @click="handleEdit(scope.$index, scope.row)" v-permiss="15">
							编辑
						</el-button>
						<el-button text :icon="Delete" class="red" @click="handleDelete(scope.$index)" v-permiss="16">
							删除
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div class="pagination">
				<el-pagination
					background
					layout="total, prev, pager, next"
					:current-page="query.pageIndex"
					:page-size="query.pageSize"
					:total="pageTotal"
					@current-change="handlePageChange"
				></el-pagination>
			</div>
		</div>

		<!-- 编辑弹出框 -->
		<el-dialog title="编辑" v-model="editVisible" width="30%">
			<el-form label-width="70px">
				<el-form-item label="用户名">
					<el-input v-model="form.name"></el-input>
				</el-form-item>
				<el-form-item label="地址">
					<el-input v-model="form.address"></el-input>
				</el-form-item>
			</el-form>
			<template #footer>
				<span class="dialog-footer">
					<el-button @click="editVisible = false">取 消</el-button>
					<el-button type="primary" @click="saveEdit">确 定</el-button>
				</span>
			</template>
		</el-dialog>
	</div>
</template>

<script setup lang="ts" name="basetable">
import { ref, reactive } from 'vue';
import { ElMessage, ElMessageBox } from 'element-plus';
import { Delete, Edit, Search, Plus, View } from '@element-plus/icons-vue';
import { fetchData } from '../api/index';
import router from '../router/index'
import { useProcessingStore } from '../store/processingInfo';
interface TableItem {
	id: number;
	processingId: string;
    orderId: string;
    sellerId: string;
    submitTime: string;
	quantity: number;
    secondParty: string;
    demand: string;
	state: string;
}
const query = reactive({
	reqDate: '',
	secondParty: '',
	state: '',
	pageIndex: 1,
	pageSize: 10
});
const tableData = ref<TableItem[]>([]);
const pageTotal = ref(0);
//日期查询
const startDateValue = ref('');
const closeDateValue = ref('');
// 获取表格数据
let processingData = useProcessingStore();
tableData.value = processingData.getAll;
// 查询操作
const handleSearch = () => {
	query.pageIndex = 1;
};
// 分页导航
const handlePageChange = (val: number) => {
	query.pageIndex = val;
};

// 删除操作
const handleDelete = (index: number) => {
	// 二次确认删除
	ElMessageBox.confirm('确定要删除吗？', '提示', {
		type: 'warning'
	})
		.then(() => {
			ElMessage.success('删除成功');
			tableData.value.splice(index, 1);
		})
		.catch(() => {});
};

function handleDetail(this : any) 
{
	router.push('processingDetail');
};
// 表格编辑时弹窗和保存
const editVisible = ref(false);
let form = reactive({
	name: '',
	address: ''
});
let idx: number = -1;
const handleEdit = (index: number, row: any) => {
	//idx = index;
	//form.name = row.name;
	//form.address = row.address;
	//editVisible.value = true;
};
const saveEdit = () => {
	//editVisible.value = false;
	//ElMessage.success(`修改第 ${idx + 1} 行成功`);
	//tableData.value[idx].name = form.name;
	//tableData.value[idx].address = form.address;
};
</script>

<style scoped>
.handle-box {
	margin-bottom: 20px;
}

.handle-select {
	width: 120px;
}

.handle-input {
	width: 300px;
}
.table {
	width: 100%;
	font-size: 14px;
}
.red {
	color: #F56C6C;
}
.mr10 {
	margin-right: 10px;
}
.table-td-thumb {
	display: block;
	margin: auto;
	width: 40px;
	height: 40px;
}
</style>
