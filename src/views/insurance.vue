<template>
	<div>
		<div class="container">
			<div class="handle-box">
				<el-date-picker
					v-model="startDateValue"
					type="date"
					placeholder="起始时间"
					size='default'
      			/>
				<el-date-picker
					v-model="closeDateValue"
					type="date"
					placeholder="结束时间"
					size='default'
      			/>	  
				<el-input v-model="query.borrower" placeholder="保险公司" class="handle-input mr10"></el-input>
				<el-input v-model="query.lender" placeholder="货品项目" class="handle-input mr10"></el-input>
				<el-button type="primary" :icon="Search" @click="handleSearch">搜索</el-button>
				<el-button type="primary" :icon="Plus">新增</el-button>
			</div>
			<el-table :data="tableData" border class="table" ref="multipleTable" header-cell-class-name="table-header">
				<el-table-column prop="id" label="序号" width="55" align="center"></el-table-column>
				<el-table-column prop="insuranceId" label="保险单号"></el-table-column>
				<el-table-column prop="client" label="保险客户"></el-table-column>
				<el-table-column prop="company" label="保险公司"></el-table-column>
				<el-table-column prop="orderId" label="受保订单号"></el-table-column>
                <el-table-column prop="cargo" label="保险货品项目"></el-table-column>
                <el-table-column prop="amount" label="金额"></el-table-column>
				<el-table-column label="操作" width="320" align="center">
					<template #default="scope">
						<el-button text :icon="View" @click="handleDetail()" v-permiss="15">详情</el-button>
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
interface TableItem {
	id: number;
	insuranceId: string;
	client: string;
	company: string;
	orderId: string;
	cargo: string;
    amount: string;
}

const query = reactive({
	beginDate: '',
	closeDate: '',
	borrower: '',
	lender: '',
	pageIndex: 1,
	pageSize: 10
});
const tableData = ref<TableItem[]>([]);
const pageTotal = ref(0);
//日期查询
const startDateValue = ref('');
const closeDateValue = ref('');
// 获取表格数据
const getData = (url : string) => {
	fetchData(url).then(res => {
		tableData.value = res.data.list;
		pageTotal.value = res.data.pageTotal || 50;
	});
	console.log(tableData.value)
};
getData('/insuranceTable.json');

// 查询操作
const handleSearch = () => {
	query.pageIndex = 1;
	getData('/insuranceTable.json');
};
// 分页导航
const handlePageChange = (val: number) => {
	query.pageIndex = val;
	getData('/insuranceTable.json');
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
	router.push('insuranceDetail');
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
