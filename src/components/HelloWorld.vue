<script setup lang="ts">
import { ref } from 'vue';

const columns = [
	{
		title: 'ip',
		dataIndex: 'ip',
		align: 'center',
	},
	{
		title: 'weight',
		dataIndex: 'weight',
		align: 'center',
		sorter: (a: DataItem, b: DataItem) => a.weight - b.weight,
	},
	{
		title: 'color',
		dataIndex: 'color',
		align: 'center',
		filters: [
			{
				// text: 'London',
				value: 'London',
			},
			{
				// text: 'New York',
				value: 'New York',
			},
		],
		onFilter: (value: string, record: DataItem) => record.color.indexOf(value) === 0,
	},
	{
		title: '操作',
		dataIndex: 'operation',
		align: 'center',
	},
];
interface DataItem {
	key: string;
	ip: string;
	weight: number;
	color: string;
}
const data: DataItem[] = [
	{
		key: '1',
		ip: '123.333.33',
		weight: 321,
		color: 'red',
	},
	{
		key: '2',
		ip: '123.333.33',
		weight: 32,
		color: 'blue',
	},
	{
		key: '3',
		ip: '123.333.33',
		weight: 44,
		color: 'blue',
	},
	{
		key: '4',
		ip: '123.333.33',
		weight: 55,
		color: 'green',
	},
];
const dataSource = ref<DataItem[]>([]);

dataSource.value = data;

const editKey = ref();
const editValue = ref();

function edit(key: string) {
	editKey.value = key;
	editValue.value = dataSource.value.filter((item) => key === item.key)[0].weight;
}

function save(key: string) {
	const submitKey = dataSource.value.filter((item) => key === item.key)[0].key;
	console.log('提交数据：', editValue.value, '数据key:', submitKey);
	editKey.value = 0;
}

function cancel() {
	editKey.value = 0;
}
</script>

<template>
	<a-table style="width: 1000px" :columns="columns" :data-source="dataSource" bordered>
		<template #bodyCell="{ column, text, record }">
			<template v-if="['weight'].includes(column.dataIndex)">
				<div>
					<a-input v-if="editKey == record.key" v-model:value="editValue" style="margin: -5px 0" />
					<template v-else>
						<span> {{ text }}</span>
					</template>
				</div>
			</template>
			<template v-else-if="column.dataIndex === 'operation'">
				<div class="editable-row-operations">
					<span v-if="editKey">
						<a-typography-link @click="save(record.key)">保存</a-typography-link>
						<a-popconfirm title="确认取消?" @confirm="cancel()">
							<a style="margin-left: 8px">取消</a>
						</a-popconfirm>
					</span>
					<span v-else>
						<a @click="edit(record.key)">编辑</a>
					</span>
				</div>
			</template>
		</template>
	</a-table>
</template>

<style scoped></style>
