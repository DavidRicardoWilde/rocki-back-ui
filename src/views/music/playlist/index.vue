<template>
  <div class="app-container">
    <el-form :model="queryParams" ref="queryForm" :inline="true" v-show="showSearch" label-width="68px">
      <el-form-item label="PlaylistId" prop="id">
        <el-input
          v-model="queryParams.id"
          placeholder="Input Id"
          clearable
          size="small"
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="title" prop="title">
        <el-input
          v-model="queryParams.title"
          placeholder="Input playlist title"
          clearable
          size="small"
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <!--      <el-form-item label="create time" prop="releaseDate">-->
      <!--        <el-date-picker clearable size="small" style="width: 200px"-->
      <!--          v-model="queryParams.releaseDate"-->
      <!--          type="date"-->
      <!--          value-format="yyyy-MM-dd"-->
      <!--          placeholder="switch create time">-->
      <!--        </el-date-picker>-->
      <!--      </el-form-item>-->
      <!--      <el-form-item label="更新时间" prop="uploadDate">-->
      <!--        <el-date-picker clearable size="small" style="width: 200px"-->
      <!--          v-model="queryParams.uploadDate"-->
      <!--          type="date"-->
      <!--          value-format="yyyy-MM-dd"-->
      <!--          placeholder="选择更新时间">-->
      <!--        </el-date-picker>-->
      <!--      </el-form-item>-->
      <el-form-item label="Track count" prop="tracksCount">
        <el-input
          v-model="queryParams.tracksCount"
          placeholder="Input track count"
          clearable
          size="small"
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <el-form-item label="Number of followers" prop="followers">
        <el-input
          v-model="queryParams.followers"
          placeholder="Enter the number"
          clearable
          size="small"
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <!--      <el-form-item label="这首歌获利总数" prop="rocksEarned">-->
      <!--        <el-input-->
      <!--          v-model="queryParams.rocksEarned"-->
      <!--          placeholder="请输入这首歌获利总数"-->
      <!--          clearable-->
      <!--          size="small"-->
      <!--          @keyup.enter.native="handleQuery"-->
      <!--        />-->
      <!--      </el-form-item>-->
      <!--      <el-form-item label="所有歌曲总时长" prop="tracksLengthTotal">-->
      <!--        <el-input-->
      <!--          v-model="queryParams.tracksLengthTotal"-->
      <!--          placeholder="请输入所有歌曲总时长"-->
      <!--          clearable-->
      <!--          size="small"-->
      <!--          @keyup.enter.native="handleQuery"-->
      <!--        />-->
      <!--      </el-form-item>-->
      <el-form-item label="Playlist cover" prop="image">
        <el-input
          v-model="queryParams.image"
          placeholder="Enter cover url"
          clearable
          size="small"
          @keyup.enter.native="handleQuery"
        />
      </el-form-item>
      <!--      <el-form-item label="喜爱人数" prop="likes">-->
      <!--        <el-input-->
      <!--          v-model="queryParams.likes"-->
      <!--          placeholder="请输入喜爱人数"-->
      <!--          clearable-->
      <!--          size="small"-->
      <!--          @keyup.enter.native="handleQuery"-->
      <!--        />-->
      <!--      </el-form-item>-->
      <el-form-item>
        <el-button type="cyan" icon="el-icon-search" size="mini" @click="handleQuery">Search</el-button>
        <el-button icon="el-icon-refresh" size="mini" @click="resetQuery">Reset</el-button>
      </el-form-item>
    </el-form>

    <el-row :gutter="10" class="mb8">
      <!--      <el-col :span="1.5">-->
      <!--        <el-button-->
      <!--          type="primary"-->
      <!--          icon="el-icon-plus"-->
      <!--          size="mini"-->
      <!--          @click="handleAdd"-->
      <!--          v-hasPermi="['music:playlist:add']"-->
      <!--        >Add</el-button>-->
      <!--      </el-col>-->
      <el-col :span="1.5">
        <el-button
          type="success"
          icon="el-icon-edit"
          size="mini"
          :disabled="single"
          @click="handleUpdate"
          v-hasPermi="['music:playlist:edit']"
        >Modify</el-button>
      </el-col>
      <el-col :span="1.5">
        <el-button
          type="danger"
          icon="el-icon-delete"
          size="mini"
          :disabled="multiple"
          @click="handleDelete"
          v-hasPermi="['music:playlist:remove']"
        >Delete</el-button>
      </el-col>
      <!--      <el-col :span="1.5">-->
      <!--        <el-button-->
      <!--          type="warning"-->
      <!--          icon="el-icon-download"-->
      <!--          size="mini"-->
      <!--          @click="handleExport"-->
      <!--          v-hasPermi="['music:playlist:export']"-->
      <!--        >导出</el-button>-->
      <!--      </el-col>-->
      <right-toolbar :showSearch.sync="showSearch" @queryTable="getList"></right-toolbar>
    </el-row>

    <el-table v-loading="loading" :data="playlistList" @selection-change="handleSelectionChange">
      <el-table-column type="selection" width="55" align="center" />
      <el-table-column label="Playlist id" align="center" prop="id" />
      <el-table-column label="User id" align="center" prop="userId" />
      <el-table-column label="Playlist title" align="center" prop="title" />
      <!--      <el-table-column label="创建时间" align="center" prop="releaseDate" width="180">-->
      <!--        <template slot-scope="scope">-->
      <!--          <span>{{ parseTime(scope.row.releaseDate, '{y}-{m}-{d}') }}</span>-->
      <!--        </template>-->
      <!--      </el-table-column>-->
      <!--      <el-table-column label="更新时间" align="center" prop="uploadDate" width="180">-->
      <!--        <template slot-scope="scope">-->
      <!--          <span>{{ parseTime(scope.row.uploadDate, '{y}-{m}-{d}') }}</span>-->
      <!--        </template>-->
      <!--      </el-table-column>-->
      <el-table-column label="Track count" align="center" prop="tracksCount" />
      <el-table-column label="Number of followers" align="center" prop="followers" />
      <!--      <el-table-column label="这首歌获利总数" align="center" prop="rocksEarned" />-->
      <!--      <el-table-column label="所有歌曲总时长" align="center" prop="tracksLengthTotal" />-->
      <el-table-column label="Playlist cover" align="center" prop="image" />
      <!--      <el-table-column label="喜爱人数" align="center" prop="likes" />-->
      <el-table-column label="Operation" align="center" class-name="small-padding fixed-width">
        <template slot-scope="scope">
          <el-button
            size="mini"
            type="text"
            icon="el-icon-edit"
            @click="handleUpdate(scope.row)"
            v-hasPermi="['music:playlist:edit']"
          >Modify</el-button>
          <el-button
            size="mini"
            type="text"
            icon="el-icon-delete"
            @click="handleDelete(scope.row)"
            v-hasPermi="['music:playlist:remove']"
          >Delete</el-button>
        </template>
      </el-table-column>
    </el-table>

    <pagination
      v-show="total>0"
      :total="total"
      :page.sync="queryParams.pageNum"
      :limit.sync="queryParams.pageSize"
      @pagination="getList"
    />

    <!-- 添加或修改用户歌单对话框 -->
    <el-dialog :title="title" :visible.sync="open" width="500px" append-to-body>
      <el-form ref="form" :model="form" :rules="rules" label-width="80px">
        <el-form-item label="User id" prop="userId">
          <el-input v-model="form.userId" placeholder="Input user id" />
        </el-form-item>
        <el-form-item label="Playlist title" prop="title">
          <el-input v-model="form.title" placeholder="Input title" />
        </el-form-item>
        <!--        <el-form-item label="创建时间" prop="releaseDate">-->
        <!--          <el-date-picker clearable size="small" style="width: 200px"-->
        <!--            v-model="form.releaseDate"-->
        <!--            type="date"-->
        <!--            value-format="yyyy-MM-dd"-->
        <!--            placeholder="选择创建时间">-->
        <!--          </el-date-picker>-->
        <!--        </el-form-item>-->
        <!--        <el-form-item label="更新时间" prop="uploadDate">-->
        <!--          <el-date-picker clearable size="small" style="width: 200px"-->
        <!--            v-model="form.uploadDate"-->
        <!--            type="date"-->
        <!--            value-format="yyyy-MM-dd"-->
        <!--            placeholder="选择更新时间">-->
        <!--          </el-date-picker>-->
        <!--        </el-form-item>-->
        <el-form-item label="Track count" prop="tracksCount">
          <el-input v-model="form.tracksCount" placeholder="Input count" />
        </el-form-item>
        <el-form-item label="Number of followers" prop="followers">
          <el-input v-model="form.followers" placeholder="Input number" />
        </el-form-item>
        <!--        <el-form-item label="这首歌获利总数" prop="rocksEarned">-->
        <!--          <el-input v-model="form.rocksEarned" placeholder="请输入这首歌获利总数" />-->
        <!--        </el-form-item>-->
        <!--        <el-form-item label="所有歌曲总时长" prop="tracksLengthTotal">-->
        <!--          <el-input v-model="form.tracksLengthTotal" placeholder="请输入所有歌曲总时长" />-->
        <!--        </el-form-item>-->
        <el-form-item label="Playlist cover" prop="image">
          <el-input v-model="form.image" placeholder="Input url" />
        </el-form-item>
        <!--        <el-form-item label="喜爱人数" prop="likes">-->
        <!--          <el-input v-model="form.likes" placeholder="请输入喜爱人数" />-->
        <!--        </el-form-item>-->
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submitForm">Confirm</el-button>
        <el-button @click="cancel">Cancel</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { listPlaylist, getPlaylist, delPlaylist, addPlaylist, updatePlaylist, exportPlaylist } from "@/api/music/playlist";

export default {
  name: "Playlist",
  data() {
    return {
      // 遮罩层
      loading: true,
      // 选中数组
      ids: [],
      // 非单个禁用
      single: true,
      // 非多个禁用
      multiple: true,
      // 显示搜索条件
      showSearch: true,
      // 总条数
      total: 0,
      // 用户歌单表格数据
      playlistList: [],
      // 弹出层标题
      title: "",
      // 是否显示弹出层
      open: false,
      // 查询参数
      queryParams: {
        pageNum: 1,
        pageSize: 10,
        id: null,
        title: null,
        releaseDate: null,
        uploadDate: null,
        tracksCount: null,
        followers: null,
        rocksEarned: null,
        tracksLengthTotal: null,
        image: null,
        likes: null
      },
      // 表单参数
      form: {},
      // 表单校验
      rules: {
        id: [
          {required: true, message: "User id cannot be null", trigger: "blur"}
        ],
        title: [
          {required: true, message: "Playlist title cannot be null", trigger: "blur"}
        ],
        // releaseDate: [
        //   { required: true, message: "创建时间不能为空", trigger: "blur" }
        // ],
        // uploadDate: [
        //   { required: true, message: "更新时间不能为空", trigger: "blur" }
        // ],
        tracksCount: [
          {required: true, message: "Track count cannot be null", trigger: "blur"}
        ],
        followers: [
          {required: true, message: "The number of followers cannot be null", trigger: "blur"}
        ],
        // rocksEarned: [
        //   { required: true, message: "这首歌获利总数不能为空", trigger: "blur" }
        // ],
        // tracksLengthTotal: [
        //   { required: true, message: "所有歌曲总时长不能为空", trigger: "blur" }
        // ],
        image: [
          {required: true, message: "Url cannot be null", trigger: "blur"}
        ]
        // likes: [
        //   { required: true, message: "喜爱人数不能为空", trigger: "blur" }
        // ]
      }
    };
  },
  created() {
    this.getList();
  },
  methods: {
    /** 查询用户歌单列表 */
    getList() {
      this.loading = true;
      listPlaylist(this.queryParams).then(response => {
        this.playlistList = response.rows;
        this.total = response.total;
        this.loading = false;
      });
    },
    // 取消按钮
    cancel() {
      this.open = false;
      this.reset();
    },
    // 表单重置
    reset() {
      this.form = {
        id: null,
        userId: null,
        title: null,
        releaseDate: null,
        uploadDate: null,
        tracksCount: null,
        followers: null,
        rocksEarned: null,
        tracksLengthTotal: null,
        image: null,
        likes: null
      };
      this.resetForm("form");
    },
    /** 搜索按钮操作 */
    handleQuery() {
      this.queryParams.pageNum = 1;
      this.getList();
    },
    /** 重置按钮操作 */
    resetQuery() {
      this.resetForm("queryForm");
      this.handleQuery();
    },
    // 多选框选中数据
    handleSelectionChange(selection) {
      this.ids = selection.map(item => item.id)
      this.single = selection.length !== 1
      this.multiple = !selection.length
    },
    /** 新增按钮操作 */
    // handleAdd() {
    //   this.reset();
    //   this.open = true;
    //   this.title = "添加用户歌单";
    // },
    /** 修改按钮操作 */
    handleUpdate(row) {
      this.reset();
      const id = row.id || this.ids
      getPlaylist(id).then(response => {
        this.form = response.data;
        this.open = true;
        this.title = "Modify playlist";
      });
    },
    /** 提交按钮 */
    submitForm() {
      this.$refs["form"].validate(valid => {
        if (valid) {
          if (this.form.id != null) {
            updatePlaylist(this.form).then(response => {
              if (response.code === 200) {
                this.msgSuccess("Success");
                this.open = false;
                this.getList();
              }
            });
          } else {
            addPlaylist(this.form).then(response => {
              if (response.code === 200) {
                this.msgSuccess("Success");
                this.open = false;
                this.getList();
              }
            });
          }
        }
      });
    },
    /** 删除按钮操作 */
    handleDelete(row) {
      const ids = row.id || this.ids;
      this.$confirm('Are you sure to delete the playlist which id is "' + ids + '"?', "WARNING", {
        confirmButtonText: "Confirm",
        cancelButtonText: "Cancel",
        type: "warning"
      }).then(function () {
        return delPlaylist(ids);
      }).then(() => {
        this.getList();
        this.msgSuccess("Success");
      }).catch(function () {
      });
    }
    /** 导出按钮操作 */
    //   handleExport() {
    //     const queryParams = this.queryParams;
    //     this.$confirm('是否确认导出所有用户歌单数据项?', "警告", {
    //         confirmButtonText: "确定",
    //         cancelButtonText: "取消",
    //         type: "warning"
    //       }).then(function() {
    //         return exportPlaylist(queryParams);
    //       }).then(response => {
    //         this.download(response.msg);
    //       }).catch(function() {});
    //   }
    // }
  }
};
</script>
