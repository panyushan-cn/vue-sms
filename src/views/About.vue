<template>
    <div>
        <div style="margin-bottom: 16px">
            <el-row style="margin: 18px 0 0 18px ">
                <el-breadcrumb separator-class="el-icon-arrow-right">
                    <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
                    <el-breadcrumb-item>用户管理</el-breadcrumb-item>
                    <el-breadcrumb-item>用户信息</el-breadcrumb-item>
                </el-breadcrumb>
            </el-row>
            <a-button class="editable-add-btn" @click="handleAdd">
                添加用户信息
            </a-button>
            <span style="margin-right: 24px"></span>
            <a-button type="primary" :disabled="!hasSelected" :loading="loading" @click="start">
                重置页面
            </a-button>
            <span style="margin-left: 8px">
                <template v-if="hasSelected">
                    {{ `Selected ${selectedRowKeys.length} items` }}
                </template>
            </span>
        </div>
        <a-table
            :row-selection="{ selectedRowKeys: selectedRowKeys, onChange: onSelectChange }"
            :columns="columns"
            :data-source="data"
            rowKey='id'
            bordered
        >
            <div
                slot="filterDropdown"
                slot-scope="{ setSelectedKeys, selectedKeys, confirm, clearFilters, column }"
                style="padding: 8px"
            >
                <a-input
                    v-ant-ref="c => (searchInput = c)"
                    :placeholder="`搜索 ${column.dataIndex}`"
                    :value="selectedKeys[0]"
                    style="width: 188px; margin-bottom: 8px; display: block;"
                    @change="e => setSelectedKeys(e.target.value ? [e.target.value] : [])"
                    @pressEnter="() => handleSearch(selectedKeys, confirm, column.dataIndex)"
                />
                <a-button
                    type="primary"
                    icon="search"
                    size="small"
                    style="width: 90px; margin-right: 8px"
                    @click="() => handleSearch(selectedKeys, confirm, column.dataIndex)"
                >
                    搜索
                </a-button>
                <a-button size="small" style="width: 90px" @click="() => handleReset(clearFilters)">
                    重置
                </a-button>
            </div>
            <a-icon
                slot="filterIcon"
                slot-scope="filtered"
                type="search"
                :style="{ color: filtered ? '#108ee9' : undefined }"
            />
            <template slot="customRender" slot-scope="text, record, index, column">
            <span v-if="searchText && searchedColumn === column.dataIndex">
                <template
                    v-for="(fragment, i) in text.toString().split(new RegExp(`(?<=${searchText})|(?=${searchText})`, 'i'))"
                >
                <mark
                    v-if="fragment.toLowerCase() === searchText.toLowerCase()"
                    :key="i"
                    class="highlight"
                >{{ fragment }}</mark>
                <template v-else>{{ fragment }}</template>
                </template>
                </span>
                <template v-else>{{ text }}</template>
            </template>
            <template slot="operation_delete" slot-scope="text, record">
                <a-popconfirm
                    v-if="data.length"
                    title="确定要删除吗?"
                    @confirm="() => onDelete(record.id)"
                >
                    <a href="javascript:;">删除</a>
                </a-popconfirm>
            </template>
            <template
                v-for="col in ['id', 'username', 'name', 'phone', 'email', 'enabled']"
                :slot="col"
                slot-scope="text, record, index"
            >
                <div :key="col">
                    <a-input
                        v-if="record.editable"
                        style="margin: -5px 0"
                        :value="text"
                        @change="e => handleChange(e.target.value, record.id, col)"
                    />
                    <template v-else>
                        {{ text }}
                    </template>
                </div>
            </template>
            <template slot="operation_edit" slot-scope="text, record, index">
                <div class="editable-row-operations">
                    <span v-if="record.editable">
                        <a @click="() => save(record.id)">Save</a>
                        <a-popconfirm title="Sure to cancel?" @confirm="() => cancel(record.id)">
                            <a>Cancel</a>
                        </a-popconfirm>
                    </span>
                    <span v-else>
                        <a :disabled="editingKey !== ''" @click="() => edit(record.id)">编辑</a>
                     </span>
                </div>
            </template>
        </a-table>
    </div>
</template>
<script>

const data = [];
for (let i = 0; i < 100; i++) {
    data.push({
        id: i.toString(),
        username: "a",
        name: `Edrward ${i}`,
        phone: 11,
        email: 32,
        enabled: 1,
    });
}
const roles =[];
//import BulkRegistration from './BulkRegistration'

export default {
    name: 'UserProfile',
    //components: {BulkRegistration},
    data() {
        this.cacheData = data.map(item => ({...item}));
        return {
            data,
            searchText: '',
            searchInput: null,
            searchedColumn: '',
            editingKey: '',
            roles,
            columns: [
                {
                    title: 'id',
                    dataIndex: 'id',
                    key: 'id',
                    scopedSlots: {
                        filterDropdown: 'filterDropdown',
                        filterIcon: 'filterIcon',
                        customRender: 'id',
                    },
                    onFilter: (value, record) =>
                        record.id
                            .toString()
                            .toLowerCase()
                            .includes(value.toLowerCase()),
                    onFilterDropdownVisibleChange: visible => {
                        if (visible) {
                            setTimeout(() => {
                                this.searchInput.focus();
                            }, 0);
                        }
                    },
                },
                {
                    title: '用户名',
                    dataIndex: 'username',
                    key: 'username',
                    scopedSlots: {
                        filterDropdown: 'filterDropdown',
                        filterIcon: 'filterIcon',
                        customRender: 'username',
                    },
                    onFilter: (value, record) =>
                        record.username
                            .toString()
                            .toLowerCase()
                            .includes(value.toLowerCase()),
                    onFilterDropdownVisibleChange: visible => {
                        if (visible) {
                            setTimeout(() => {
                                this.searchInput.focus();
                            });
                        }
                    },
                },
                {
                    title: '真实姓名',
                    dataIndex: 'name',
                    key: 'name',
                    scopedSlots: {
                        filterDropdown: 'filterDropdown',
                        filterIcon: 'filterIcon',
                        customRender: 'name',
                    },
                    onFilter: (value, record) =>
                        record.name
                            .toString()
                            .toLowerCase()
                            .includes(value.toLowerCase()),
                    onFilterDropdownVisibleChange: visible => {
                        if (visible) {
                            setTimeout(() => {
                                this.searchInput.focus();
                            });
                        }
                    },
                },
                {
                    title: '手机号',
                    dataIndex: 'phone',
                    key: 'phone',
                    scopedSlots: {
                        filterDropdown: 'filterDropdown',
                        filterIcon: 'filterIcon',
                        customRender: 'phone',
                    },
                    onFilter: (value, record) =>
                        record.phone
                            .toString()
                            .toLowerCase()
                            .includes(value.toLowerCase()),
                    onFilterDropdownVisibleChange: visible => {
                        if (visible) {
                            setTimeout(() => {
                                this.searchInput.focus();
                            });
                        }
                    },
                },
                {
                    title: '邮箱',
                    dataIndex: 'email',
                    key: 'email',
                    scopedSlots: {
                        filterDropdown: 'filterDropdown',
                        filterIcon: 'filterIcon',
                        customRender: 'email',
                    },
                    onFilter: (value, record) =>
                        record.email
                            .toString()
                            .toLowerCase()
                            .includes(value.toLowerCase()),
                    onFilterDropdownVisibleChange: visible => {
                        if (visible) {
                            setTimeout(() => {
                                this.searchInput.focus();
                            });
                        }
                    },
                },
                {
                    title: '状态',
                    dataIndex: 'enabled',
                    key: 'enabled',
                    scopedSlots: {
                        filterDropdown: 'filterDropdown',
                        filterIcon: 'filterIcon',
                        customRender: 'enabled',
                    },
                    onFilter: (value, record) =>
                        record.enabled
                            .toString()
                            .toLowerCase()
                            .includes(value.toLowerCase()),
                    onFilterDropdownVisibleChange: visible => {
                        if (visible) {
                            setTimeout(() => {
                                this.searchInput.focus();
                            });
                        }
                    },
                },
                {
                    title: '删除',
                    dataIndex: 'operation_delete',
                    scopedSlots: {customRender: 'operation_delete'},
                },
                {
                    title: '修改',
                    dataIndex: 'operation_edit',
                    scopedSlots: {customRender: 'operation_edit'},
                },
            ],
            selectedRowKeys: [], // Check here to configure the default column
            loading: false,
        };
    },
    computed: {
        hasSelected() {
            return this.selectedRowKeys.length > 0;
        },
    },
    mounted () {

    },
    methods: {
        start() {
            this.loading = true;
            // ajax request after empty completing
            setTimeout(() => {
                this.loading = false;
                this.selectedRowKeys = [];
            }, 500);
        },
        onSelectChange(selectedRowKeys) {
            console.log('selectedRowKeys changed: ', selectedRowKeys);
            this.selectedRowKeys = selectedRowKeys;
        },
        onDelete(key) {
            const data = [...this.data];
            this.data = data.filter(item => item.id !== key);
        },
        handleSearch(selectedKeys, confirm, dataIndex) {
            confirm();
            this.searchText = selectedKeys[0];
            this.searchedColumn = dataIndex;
        },

        handleReset(clearFilters) {
            clearFilters();
            this.searchText = '';
        },
        handleAdd() {
            const {count, data} = this;
            const newData = {
                key: count,
                id: `Edward King ${count}`,
                username: 32,
                name: `London, Park Lane no. ${count}`,
            };
            this.data = [...data, newData];
            this.count = count + 1;
        },
        handleChange(value, key, column) {
            const newData = [...this.data];
            const target = newData.filter(item => key === item.id)[0];
            console.log(column)
            if (target) {
                target[column] = value;
                this.data = newData;
            }
        },
        edit(key) {
            const newData = [...this.data];
            const target = newData.filter(item => key === item.id)[0];
            this.editingKey = key;
            if (target) {
                target.editable = true;
                this.data = newData;
            }
        },
        save(key) {
            const newData = [...this.data];
            const newCacheData = [...this.cacheData];
            const target = newData.filter(item => key === item.id)[0];
            const targetCache = newCacheData.filter(item => key === item.id)[0];
            if (target && targetCache) {
                delete target.editable;
                this.data = newData;
                Object.assign(targetCache, target);
                this.cacheData = newCacheData;
            }
            this.editingKey = '';

        },
        cancel(key) {

            const newData = [...this.data];
            const target = newData.filter(item => key === item.id)[0];
            this.editingKey = '';
            if (target) {
                Object.assign(target, this.cacheData.filter(item => key === item.id)[0]);
                delete target.editable;
                this.data = newData;
            }
        },
    },
};
</script>

<style scoped>
.highlight {
    background-color: rgb(255, 192, 105);
    padding: 0;
}

.editable-row-operations a {
    margin-right: 8px;
}
</style>

