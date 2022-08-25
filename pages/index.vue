<template>

  <div>
    <!-- Create button -->
    <el-button type="primary" style="margin-bottom: 10px" @click="handleCreate">Create Blog</el-button>

    <el-table :data="tableData" style="width: 100%">
      <el-table-column label="No." width="180">
        <template slot-scope="scope">
          <span>{{ scope.$index + 1 }}.</span>
        </template>
      </el-table-column>
      <el-table-column prop="title" label="Title" width="180">
      </el-table-column>
      <el-table-column prop="description" label="Description" width="180">
      </el-table-column>
      <el-table-column prop="tags" label="Tags">
        <template slot-scope="scope">
          <el-tag size="mini" v-for="item in scope.row.tags" :key="item" :type="'success'">{{ item }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column label="Action">
        <template slot-scope="scope">
          <el-button size="mini" type="text" @click="handleEdit(scope.$index, scope.row)">Edit</el-button>
          <el-button size="mini" type="text" @click="handleDelete(scope.$index, scope.row)">Delete</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <!-- <Home /> -->
</template>

<script>
export default {
  name: 'IndexPage',
  async fetch() {
    await this.getData();
  },
  data() {
    return {
      tableData: [],
    }
  },
  methods: {
    async getData() {
      const { data } = await this.$axios.get('/blog');
      this.tableData = data;
    },
    handleCreate() {
      this.$router.push('/form');
    },
    handleEdit(index, row) {
      this.$router.push(`/form/${row.id}`);
    },
    handleDelete(index, row) {
      // confirmation
      this.$confirm('Are you sure to delete this record?', 'Confirm', {
        confirmButtonText: 'Delete',
        cancelButtonText: 'Cancel',
        type: 'warning'
      }).then(async () => {
        const { data } = await this.$axios.delete(`/blog/${row.id}`);
        this.$message({
          type: 'success',
          message: 'Delete successfully!',
        });
        this.tableData.splice(index, 1);
      }).catch(() => {
        this.$message({
          type: 'info',
          message: 'Delete canceled!',
        });
      });
    },
  }
}
</script>
