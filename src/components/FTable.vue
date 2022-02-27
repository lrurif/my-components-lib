<script>
export default {
  // 过滤式组件
  name: "vTable",
  data() {
    return {};
  },
  props: {
    visibleKeys: {
      type: Array,
      default: () => {
        return [];
      },
    },
    description: {
      type: String,
      default: "暂无数据",
    },
  },
  methods: {
    clearSelection() {
      this.$refs.table.clearSelection();
    },
    toggleRowSelection(...args) {
      this.$refs.table.toggleRowSelection(...args);
    },
    // 以此类推，可直接在此组件实例上执行这些方法
  },
  render() {
    const vNodes = this.$slots.default;
    let filterNode = vNodes;
    if (this.visibleKeys.length > 0) {
      filterNode = vNodes.filter((item) =>
        this.visibleKeys.includes(item.componentOptions.propsData.prop)
      );
    }
    return (
      <el-table
        ref="table"
        height="calc(100% - 1px)"
        header-row-class-name={
          "tableHeader" || this.$attrs["header-row-class-name"]
        }
        {...{ attrs: this.$attrs, on: this.$listeners }}
      >
        {filterNode}(
        <div slot="empty">
          <el-empty description={this.description} />
        </div>
        )
      </el-table>
    );
  },
};
</script>
<style  scoped>
::v-deep(.tableHeader){
	background-color: #fafafaff!important;
	color: #4b4c4e;
	
}
.tableHeader > th {
		background-color: #fafafaff !important;
		font-weight: bold;
		font-size: 14px;
	}
.el-table__cell .el-button {
		margin-left: 10px;
	}
</style>
