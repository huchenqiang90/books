<template>
  <div class="container">
    <el-row :gutter="20">
      <el-col :span="24">
        <el-input
          placeholder="请输入会员姓名或学号查询"
          icon="search"
          v-model="query"
          @click="searchRecords"
        >
        </el-input>
      </el-col>
      <el-col :span="6">
      </el-col>
    </el-row>
    <el-table :data="records" stripe style="margin-top: 20px; width: 100%">
      <el-table-column type="expand">
        <template scope="props">
          <el-form label-position="left" inline class="demo-table-expand">
            <el-form-item label="会员名">
              <span>{{ props.row.memberName }}</span>
            </el-form-item>
            <el-form-item label="学号">
              <span>{{ props.row.memberNum }}</span>
            </el-form-item>
            <el-form-item label="联系方式">
              <span>{{ props.row.memberTel }}</span>
            </el-form-item>
            <el-form-item label="地址">
              <span>{{ props.row.memberAddress }}</span>
            </el-form-item>
            <el-form-item label="会员类型">
              <span>{{ props.row.memberRank }}</span>
            </el-form-item>
            <el-form-item label="创建时间">
              <span>{{ props.row.memberCreateTime }}</span>
            </el-form-item>
            <el-form-item label="书籍名">
              <span>{{ props.row.bookTitle }}</span>
            </el-form-item>
            <el-form-item label="ISBN 码">
              <span>{{ props.row.bookIsbn }}</span>
            </el-form-item>
            <el-form-item label="封面">
              <img :src="props.row.bookImg" :alt="props.row.bookTitle">
            </el-form-item>
            <el-form-item label="归还时间">
              <span>{{ props.row.returnTime }}</span>
            </el-form-item>
          </el-form>
        </template>
      </el-table-column>
      <el-table-column prop="memberNum" label="会员学号">
      </el-table-column>
      <el-table-column prop="memberName" label="会员名称">
      </el-table-column>
      <el-table-column prop="bookTitle" label="书籍名称">
      </el-table-column>
      <el-table-column prop="borrowTime" label="借阅时间">
      </el-table-column>
      <el-table-column prop="returnTime" label="状态" width="100" :filters="[{ text: '已还', value: '已还' }, { text: '未还', value: '未还' }, { text: '超期', value: '超期'}]" :filter-method="filterReturn">
        <template scope="scope">
          <el-tag :type="scope.row.tag === '超期' ? 'danger' : 'success'" close-transition>{{scope.row.tag}}</el-tag>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
/**
 * @file 借阅记录页
 * @author ltaoo<litaowork@aliyun.com>
 */
import {
  mapGetters,
} from 'vuex';
import {
  FETCH_RECORDS,
} from '@/common/constants';

export default {
  name: 'Records',
  data () {
    return {
      query: null,
    };
  },
  computed: mapGetters([
    'records',
  ]),
  mounted () {
    this.$store.dispatch(FETCH_RECORDS);
  },
  methods: {
    searchRecords () {
      const params = {
        name: this.query,
      };
      this.$store.dispatch(FETCH_RECORDS, params);
    },
    filterReturn (value, row) {
      return row.tag === value;
    },
  },
};
</script>
