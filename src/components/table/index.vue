<template>
    <div style="position: relative">
        <div v-if="columnsFixed.length > 0" :style="{width: columnsFixedWidth+'px'}" style="position: absolute; top: 0;z-index: 1;overflow: hidden">
            <table-body
                :on-change="handleTableChange"
                :columns="columnsFixed"
                :data-source="dataSource"
                :bordered = "bordered"
                :stripe = "stripe"
                :size="size"
                :sort-field="sortField"
                :sort-type="sortType"
            />
        </div>
        <div class="dd-table-container">
            <table-body
                :on-change="handleTableChange"
                :columns="columns"
                :data-source="dataSource"
                :bordered = "bordered"
                :stripe = "stripe"
                :size="size"
                :sort-field="sortField"
                :sort-type="sortType"
            />
        </div>
    </div>
</template>
<style>
    .dd-table-container {
        box-shadow: 0 0 5px 0 rgba(0,0,0,0.15);
        overflow: auto;
    }
</style>
<script>
    import tableBody from './table.vue';
    export default{
        props: {
            columns: Array,
            dataSource: Array,
            bordered: {
                type: Boolean,
                default: false
            },
            size: {
                type: String,
                default: 'default'
            },
            stripe: {
                type: Boolean,
                default: false
            },
            onChange: Function
        },
        data() {
            return {
                sortField: undefined,
                sortType: undefined
            }
        },
        computed: {
            columnsFixed() {
                if (!this.columns) {
                    return [];
                }

                return this.columns.filter(i => i.fixed);
            },
            columnsFixedWidth() {
                if (!this.columns) {
                    return undefined;
                }

                return this.columnsFixed.reduce((a, b) => a + b.width, 0) + 1;
            }
        },
        methods: {
            expand(row) {
                this.$set(row, 'expand', !row.expand);
            },
            handleTableChange(data) {
                this.sortField = data.sortField;
                this.sortType = data.sortType;
                this.onChange && this.onChange(data);
            }
        },
        components: {
            tableBody
        }
    }
</script>
