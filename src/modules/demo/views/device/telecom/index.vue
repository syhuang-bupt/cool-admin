<template>
	<cl-crud ref="Crud">
		<cl-row>
			<!-- 刷新按钮 -->
			<cl-refresh-btn />
			<!-- 删除按钮 -->
			<cl-multi-delete-btn />
			<!-- 产品型号 -->
			<cl-filter label="产品型号">
				<cl-select :options="options.productName" prop="productName" :width="120" />
			</cl-filter>
			<cl-flex1 />
			<!-- 关键字搜索 -->
			<cl-search-key
				field="deviceName"
				:field-list="[
					{
						label: '设备名称',
						value: 'deviceName'
					},
					{
						label: '所属单位',
						value: 'unit'
					},
					{
						label: '地址',
						value: 'address'
					}
				]"
			/>
		</cl-row>

		<cl-row>
			<!-- 数据表格 -->
			<cl-table ref="Table" />
		</cl-row>

		<cl-row>
			<cl-flex1 />
			<!-- 分页控件 -->
			<cl-pagination />
		</cl-row>

		<!-- 新增、编辑 -->
		<cl-upsert ref="Upsert" />
	</cl-crud>
</template>

<script lang="ts" name="user-list" setup>
import { useCrud, useTable, useUpsert } from '@cool-vue/crud';
import { useCool } from '/@/cool';
import { reactive } from 'vue';

const { service } = useCool();
console.log(service, '接口');

// 数据选项
const options = reactive({
	productName: [
		{
			value: 'ZGDX3000(WYH)',
			label: 'ZGDX3000(WYH)'
		},
		{
			value: 'ZGDX3000S(WYH)',
			label: 'ZGDX3000S(WYH)'
		},
		{
			value: 'ZGDX5000(WYH)',
			label: 'ZGDX5000(WYH)'
		},
		{
			value: 'ZGDX9000(WYH)',
			label: 'ZGDX9000(WYH)'
		},
		{
			value: 'ZGDX9000_4G(WYH)',
			label: 'ZGDX9000_4G(WYH)'
		},
		{
			value: 'ZGDX-A100(WYH)',
			label: 'ZGDX-A100(WYH)'
		},
		{
			value: 'ZGDX3000S-2-无电池(WYH)',
			label: 'ZGDX3000S-2-无电池(WYH)'
		},
		{
			value: 'ZGDX5000-无电池(WYH)',
			label: 'ZGDX5000-无电池(WYH)'
		},
		{
			value: 'ZGDX9000-无电池(WYH)',
			label: 'ZGDX9000-无电池(WYH)'
		}
	],
	netStatus: [
		{
			label: '在线',
			value: 1,
			type: 'success'
		},
		{
			label: '离线',
			value: 2,
			type: 'danger'
		}
	]
});

// cl-table
const Table = useTable({
	columns: [
		{
			type: 'selection',
			width: 60
		},
		{
			label: '设备名称',
			prop: 'deviceName',
			minWidth: 120
		},
		{
			label: '产品型号',
			prop: 'productName',
			minWidth: 120
			// dict: options.productName
		},
		{
			label: '所属单位',
			prop: 'unit',
			minWidth: 100
		},
		{
			label: '地址',
			prop: 'address',
			minWidth: 150
		},
		{
			label: '当前状态',
			prop: 'netStatus',
			minWidth: 80,
			dict: options.netStatus
		},
		{
			label: '创建时间',
			prop: 'createTime',
			sortable: 'desc',
			minWidth: 120
		},
		{
			label: '操作',
			type: 'op',
			buttons: ['edit', 'delete']
		}
	]
});

// cl-upsert
const Upsert = useUpsert({
	dialog: {
		width: '600px'
	},
	items: [
		{
			prop: 'deviceName',
			label: '设备名称',
			component: { name: 'el-input' },
			required: true
		},
		{
			prop: 'address',
			label: '安装位置',
			component: { name: 'el-input' }
		},
		{
			prop: 'scene',
			label: '所属场景',
			component: { name: 'el-input' }
		},
		{
			prop: 'unit',
			label: '所属单位',
			component: { name: 'el-input' }
		},
		{
			prop: 'cityId',
			label: '归属部门',
			component: { name: 'el-tree-select' },
			required: true
		},
		{
			prop: 'longitude',
			label: '经度',
			span: 12,
			component: { name: 'el-input' }
		},
		{
			prop: 'latitude',
			label: '纬度',
			span: 12,
			component: { name: 'el-input' }
		}
	]
});

// cl-crud
const Crud = useCrud(
	{
		service: service.device.info,
		permission: {
			add: true,
			update: true,
			delete: true
		}
	},
	app => {
		app.refresh();
	}
);
</script>
