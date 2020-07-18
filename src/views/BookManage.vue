<template>
    <div>
        <el-table
                :data="tableData"
                border
                style="width: 40%"
        >
            <el-table-column
                    fixed
                    prop="id"
                    label="ID"
                    width="150">
            </el-table-column>
            <el-table-column
                    prop="name"
                    label="书目"
                    width="120">
            </el-table-column>
            <el-table-column
                    prop="author"
                    label="作者"
                    width="120">
            </el-table-column>

            <el-table-column
                    fixed="right"
                    label="操作"
                    width="100">
                <template slot-scope="scope">
                    <el-button @click="edit(scope.row)" type="text" size="small">修改</el-button>
                    <el-button @click="deleteBook(scope.row)" type="text" size="small">删除</el-button>
                </template>
            </el-table-column>
        </el-table>

        <el-pagination
                background
                layout="prev, pager, next"
                :page-size="5"
                :total="total"
                @current-change="page">
        </el-pagination>
    </div>

</template>

<script>
    export default {
        methods: {

            deleteBook(row){
                const _this=this
                axios.delete('http://localhost:8181/book/deleteById/'+row.id).then(function (resp) {
                    alert('删除成功')
                    window.location.reload()
                })
            },
            edit(row) {
                // console.log(row);
                this.$router.push({
                    path:'/bookUpdate',
                    query:{
                        id:row.id
                    }
                })
            },
            page(currentPage){
                const _this=this
                // alert(currentPage)
                axios.get('http://localhost:8181/book/findAll/'+(currentPage-1)+'/5').then(function (resp) {
                    // console.log(resp)
                    _this.tableData=resp.data.content
                    _this.total=resp.data.totalElements
                })
            }
        },
        created() {
            const _this=this
            axios.get('http://localhost:8181/book/findAll/0/5').then(function (resp) {
                // console.log(resp)
                _this.tableData=resp.data.content
                _this.total=resp.data.totalElements
            })
        },

        data() {
            return {
                total: null,
                tableData: null
            }
        }
    }
</script>