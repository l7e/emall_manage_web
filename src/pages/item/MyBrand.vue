<template>
  <div>
    <v-layout>
      <v-flex xs2 class="px-3 pb-2">
        <v-btn color="info">新增</v-btn>
      </v-flex>
      <v-spacer/>
      <v-flex xs4 class="px-2 pb-2">
        <v-text-field label="搜索" hide-details append-icon="search" v-model="search"/>
      </v-flex>
    </v-layout>
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img :src="props.item.image" alt=""></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="text-xs-center">
          <v-btn color="success" small>编辑</v-btn>
          <v-btn color="error" small>删除</v-btn>
        </td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
  import BrandForm from './BrandForm'

  export default {
    name: "brand",
    components: {
      BrandForm
    },
    data() {
      return {
        headers: [
          {text: 'id', value: 'id', align: 'center', sortable: true},
          {text: 'name', value: 'name', align: 'center', sortable: false},
          {text: 'image', value: 'image', align: 'center', sortable: false},
          {text: 'letter', value: 'letter', align: 'center', sortable: true},
          {text: '操作', align: 'center', sortable: false},
        ],

        search: '',

        brands: [],

        pagination: {},

        totalBrands: 0,

        loading: false,
      }
    },
    created() {
      this.brands = [
        {id: 1, name: '小米', image: 11, letter: '1'},
        {id: 1, name: '小米', image: 11, letter: '1'},
        {id: 1, name: '小米', image: 11, letter: '2'},
        {id: 1, name: '小米', image: 11, letter: '2'},
      ];

      this.totalBrands = 15;

      this.loadBrands();
    },
    methods: {
      loadBrands() {
        this.$http.get('/brand/page', {
          params: {
            keyword: this.search,
            page: this.pagination.page,  //当前页
            rows: this.pagination.rowsPerPage, //每页大小
            sortBy: this.pagination.sortBy,  //排序字段
            descending: this.pagination.descending, //是否降序
          }
        })
      }
    },
    watch: {
      search() {
        this.loadBrands();
      },
      pagination: {
        deep: true,
        handler() {
          this.loadBrands();
        }
      }
    }
  }
</script>

<style scoped>

</style>
