<template>
  <div class="main-content">
    <!-- 列表页 -->
    <div v-if="showFlag">
      <el-form :inline="true" :model="searchForm" class="form-content">
        <el-row  :gutter="20" class="slt" :style="{justifyContent:contents.searchBoxPosition=='1'?'flex-start':contents.searchBoxPosition=='2'?'center':'flex-end'}">
		<el-form-item class="select" label="收费月份" prop="shoufeiyuefen">
		  <el-select  @change="shoufeiyuefenChange" clearable v-model="searchForm.shoufeiyuefen" placeholder="请选择收费月份">
		    <el-option
			v-for="(item,index) in shoufeiyuefenOptions"
			v-bind:key="index"
			:label="item"
			:value="item">
		    </el-option>
		  </el-select>
		</el-form-item>
		<el-form-item class="select" label="用户账号" prop="yonghuzhanghao">
		  <el-select  @change="yonghuzhanghaoChange" clearable v-model="searchForm.yonghuzhanghao" placeholder="请选择用户账号">
		    <el-option
			v-for="(item,index) in yonghuzhanghaoOptions"
			v-bind:key="index"
			:label="item"
			:value="item">
		    </el-option>
		  </el-select>
		</el-form-item>
          <el-form-item>
            <el-button v-if="contents.searchBtnIcon == 1 && contents.searchBtnIconPosition == 1" icon="el-icon-search" type="success" @click="search()">{{ contents.searchBtnFont == 1?'查询':'' }}</el-button>
            <el-button v-if="contents.searchBtnIcon == 1 && contents.searchBtnIconPosition == 2" type="success" @click="search()">{{ contents.searchBtnFont == 1?'查询':'' }}<i class="el-icon-search el-icon--right"/></el-button>
            <el-button v-if="contents.searchBtnIcon == 0" type="success" @click="search()">{{ contents.searchBtnFont == 1?'查询':'' }}</el-button>
          </el-form-item>
        </el-row>

        <el-row class="ad" :style="{justifyContent:contents.btnAdAllBoxPosition=='1'?'flex-start':contents.btnAdAllBoxPosition=='2'?'center':'flex-end'}">
          <el-form-item>
            <el-button
              v-if="isAuth('wuyeshoufei','新增') && contents.btnAdAllIcon == 1 && contents.btnAdAllIconPosition == 1"
              type="success"
              icon="el-icon-plus"
              @click="addOrUpdateHandler()"
            >{{ contents.btnAdAllFont == 1?'新增':'' }}</el-button>
            <el-button
              v-if="isAuth('wuyeshoufei','新增') && contents.btnAdAllIcon == 1 && contents.btnAdAllIconPosition == 2"
              type="success"
              @click="addOrUpdateHandler()"
            >{{ contents.btnAdAllFont == 1?'新增':'' }}<i class="el-icon-plus el-icon--right" /></el-button>
            <el-button
              v-if="isAuth('wuyeshoufei','新增') && contents.btnAdAllIcon == 0"
              type="success"
              @click="addOrUpdateHandler()"
            >{{ contents.btnAdAllFont == 1?'新增':'' }}</el-button>
            <el-button
              v-if="isAuth('wuyeshoufei','删除') && contents.btnAdAllIcon == 1 && contents.btnAdAllIconPosition == 1 && contents.tableSelection"
              :disabled="dataListSelections.length <= 0"
              type="danger"
              icon="el-icon-delete"
              @click="deleteHandler()"
            >{{ contents.btnAdAllFont == 1?'删除':'' }}</el-button>
            <el-button
              v-if="isAuth('wuyeshoufei','删除') && contents.btnAdAllIcon == 1 && contents.btnAdAllIconPosition == 2 && contents.tableSelection"
              :disabled="dataListSelections.length <= 0"
              type="danger"
              @click="deleteHandler()"
            >{{ contents.btnAdAllFont == 1?'删除':'' }}<i class="el-icon-delete el-icon--right" /></el-button>
            <el-button
              v-if="isAuth('wuyeshoufei','删除') && contents.btnAdAllIcon == 0 && contents.tableSelection"
              :disabled="dataListSelections.length <= 0"
              type="danger"
              @click="deleteHandler()"
            >{{ contents.btnAdAllFont == 1?'删除':'' }}</el-button>




            <el-button
              v-if="isAuth('wuyeshoufei','报表') && contents.btnAdAllIcon == 1 && contents.btnAdAllIconPosition == 1"
              type="warning"
              icon="el-icon-s-data"
              @click="chartDialog()"
            >{{ contents.btnAdAllFont == 1?'统计报表':'' }}</el-button>
            <el-button
              v-if="isAuth('wuyeshoufei','报表') && contents.btnAdAllIcon == 1 && contents.btnAdAllIconPosition == 2"
              type="warning"
              @click="chartDialog()"
            >{{ contents.btnAdAllFont == 1?'统计报表':'' }}<i class="el-icon-s-data el-icon--right" /></el-button>
            <el-button
              v-if="isAuth('wuyeshoufei','报表') && contents.btnAdAllIcon == 0"
              type="warning"
              @click="chartDialog()"
            >{{ contents.btnAdAllFont == 1?'统计报表':'' }}</el-button>

          </el-form-item>
        </el-row>
      </el-form>
      <div class="table-content">
        <el-table class="tables" :size="contents.tableSize" :show-header="contents.tableShowHeader"
            :header-row-style="headerRowStyle" :header-cell-style="headerCellStyle"
            :border="contents.tableBorder"
            :fit="contents.tableFit"
            :stripe="contents.tableStripe"
            :style="{width: '100%',fontSize:contents.tableContentFontSize,color:contents.tableContentFontColor}"
            v-if="isAuth('wuyeshoufei','查看')"
            :data="dataList"
            v-loading="dataListLoading"
            @selection-change="selectionChangeHandler">
            <el-table-column  v-if="contents.tableSelection"
                type="selection"
                :header-align="contents.tableAlign"
                align="center"
                width="50">
            </el-table-column>
            <el-table-column label="索引" :align="contents.tableAlign"  v-if="contents.tableIndex" type="index" width="50" />
                <el-table-column  :sortable="contents.tableSortable" :align="contents.tableAlign" 
                    prop="shoufeiyuefen"
                   :header-align="contents.tableAlign"
		    label="收费月份">
		     <template slot-scope="scope">
                       {{scope.row.shoufeiyuefen}}
                     </template>
                </el-table-column>
                <el-table-column  :sortable="contents.tableSortable" :align="contents.tableAlign" 
                    prop="yonghuzhanghao"
                   :header-align="contents.tableAlign"
		    label="用户账号">
		     <template slot-scope="scope">
                       {{scope.row.yonghuzhanghao}}
                     </template>
                </el-table-column>
                <el-table-column  :sortable="contents.tableSortable" :align="contents.tableAlign" 
                    prop="yonghuxingming"
                   :header-align="contents.tableAlign"
		    label="用户姓名">
		     <template slot-scope="scope">
                       {{scope.row.yonghuxingming}}
                     </template>
                </el-table-column>
                <el-table-column  :sortable="contents.tableSortable" :align="contents.tableAlign" 
                    prop="lianxifangshi"
                   :header-align="contents.tableAlign"
		    label="联系方式">
		     <template slot-scope="scope">
                       {{scope.row.lianxifangshi}}
                     </template>
                </el-table-column>
                <el-table-column  :sortable="contents.tableSortable" :align="contents.tableAlign" 
                    prop="wuyefei"
                   :header-align="contents.tableAlign"
		    label="物业费">
		     <template slot-scope="scope">
                       {{scope.row.wuyefei}}
                     </template>
                </el-table-column>
                <el-table-column  :sortable="contents.tableSortable" :align="contents.tableAlign" 
                    prop="lvhuayanghu"
                   :header-align="contents.tableAlign"
		    label="绿化养护">
		     <template slot-scope="scope">
                       {{scope.row.lvhuayanghu}}
                     </template>
                </el-table-column>
                <el-table-column  :sortable="contents.tableSortable" :align="contents.tableAlign" 
                    prop="qingjieweisheng"
                   :header-align="contents.tableAlign"
		    label="清洁卫生">
		     <template slot-scope="scope">
                       {{scope.row.qingjieweisheng}}
                     </template>
                </el-table-column>
                <el-table-column  :sortable="contents.tableSortable" :align="contents.tableAlign" 
                    prop="qitashoufei"
                   :header-align="contents.tableAlign"
		    label="其它收费">
		     <template slot-scope="scope">
                       {{scope.row.qitashoufei}}
                     </template>
                </el-table-column>
                <el-table-column  :sortable="contents.tableSortable" :align="contents.tableAlign" 
                    prop="zongjine"
                   :header-align="contents.tableAlign"
		    label="总金额">
		     <template slot-scope="scope">
                       {{scope.row.zongjine}}
                     </template>
                </el-table-column>
                <el-table-column
                  :sortable="contents.tableSortable" :align="contents.tableAlign" 
                  prop="ispay"
                 :header-align="contents.tableAlign"
                  label="是否支付">
                  <template slot-scope="scope">
                    <span style="margin-right:10px">{{scope.row.ispay=='已支付'?'已支付':'未支付'}}</span>
                    <el-button v-if="scope.row.ispay!='已支付' && isAuth('wuyeshoufei','支付') " type="text" icon="el-icon-edit" size="small" @click="payHandler(scope.row)">支付</el-button>
                  </template>
                </el-table-column>
            <el-table-column width="300" :align="contents.tableAlign" 
               :header-align="contents.tableAlign"
                label="操作">
                <template slot-scope="scope">
                <el-button v-if="isAuth('wuyeshoufei','查看') && contents.tableBtnIcon == 1 && contents.tableBtnIconPosition == 1" type="success" icon="el-icon-tickets" size="mini" @click="addOrUpdateHandler(scope.row.id,'info')">{{ contents.tableBtnFont == 1?'详情':'' }}</el-button>
                <el-button v-if="isAuth('wuyeshoufei','查看') && contents.tableBtnIcon == 1 && contents.tableBtnIconPosition == 2" type="success" size="mini" @click="addOrUpdateHandler(scope.row.id,'info')">{{ contents.tableBtnFont == 1?'详情':'' }}<i class="el-icon-tickets el-icon--right" /></el-button>
                <el-button v-if="isAuth('wuyeshoufei','查看') && contents.tableBtnIcon == 0" type="success" size="mini" @click="addOrUpdateHandler(scope.row.id,'info')">{{ contents.tableBtnFont == 1?'详情':'' }}</el-button>
                <el-button v-if=" isAuth('wuyeshoufei','修改') && contents.tableBtnIcon == 1 && contents.tableBtnIconPosition == 1" type="primary" icon="el-icon-edit" size="mini" @click="addOrUpdateHandler(scope.row.id)">{{ contents.tableBtnFont == 1?'修改':'' }}</el-button>
                <el-button v-if=" isAuth('wuyeshoufei','修改') && contents.tableBtnIcon == 1 && contents.tableBtnIconPosition == 2" type="primary" size="mini" @click="addOrUpdateHandler(scope.row.id)">{{ contents.tableBtnFont == 1?'修改':'' }}<i class="el-icon-edit el-icon--right" /></el-button>
                <el-button v-if=" isAuth('wuyeshoufei','修改') && contents.tableBtnIcon == 0" type="primary" size="mini" @click="addOrUpdateHandler(scope.row.id)">{{ contents.tableBtnFont == 1?'修改':'' }}</el-button>




                <el-button v-if="isAuth('wuyeshoufei','删除') && contents.tableBtnIcon == 1 && contents.tableBtnIconPosition == 1" type="danger" icon="el-icon-delete" size="mini" @click="deleteHandler(scope.row.id)">{{ contents.tableBtnFont == 1?'删除':'' }}</el-button>
                <el-button v-if="isAuth('wuyeshoufei','删除') && contents.tableBtnIcon == 1 && contents.tableBtnIconPosition == 2" type="danger" size="mini" @click="deleteHandler(scope.row.id)">{{ contents.tableBtnFont == 1?'删除':'' }}<i class="el-icon-delete el-icon--right" /></el-button>
                <el-button v-if="isAuth('wuyeshoufei','删除') && contents.tableBtnIcon == 0" type="danger" size="mini" @click="deleteHandler(scope.row.id)">{{ contents.tableBtnFont == 1?'删除':'' }}</el-button>
                </template>
            </el-table-column>
        </el-table>
        <el-pagination
          clsss="pages"
          :layout="layouts"
          @size-change="sizeChangeHandle"
          @current-change="currentChangeHandle"
          :current-page="pageIndex"
          :page-sizes="[10, 20, 50, 100]"
          :page-size="Number(contents.pageEachNum)"
          :total="totalPage"
          :small="contents.pageStyle"
          class="pagination-content"
          :background="contents.pageBtnBG"
          :style="{textAlign:contents.pagePosition==1?'left':contents.pagePosition==2?'center':'right'}"
        ></el-pagination>
      </div>
    </div>
    <!-- 添加/修改页面  将父组件的search方法传递给子组件-->
    <add-or-update v-if="addOrUpdateFlag" :parent="this" ref="addOrUpdate"></add-or-update>





    <el-dialog
      title="统计报表"
      :visible.sync="chartVisiable"
      width="800">
        <div id="zongjineChart" style="width:100%;height:600px;"></div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="chartDialog">返回</el-button>
      </span>
    </el-dialog>
  </div>
</template>
<script>
import axios from 'axios'
import AddOrUpdate from "./add-or-update";
export default {
  data() {
    return {
      yonghuzhanghaoOptions: [],
      searchForm: {
        key: ""
      },
      form:{},
      dataList: [],
      pageIndex: 1,
      pageSize: 10,
      totalPage: 0,
      dataListLoading: false,
      dataListSelections: [],
      showFlag: true,
      sfshVisiable: false,
      shForm: {},
      chartVisiable: false,
      addOrUpdateFlag:false,
      contents:{"searchBtnFontColor":"rgba(0, 0, 0, 1)","pagePosition":"2","inputFontSize":"14px","inputBorderRadius":"4px","tableBtnDelFontColor":"rgba(173, 215, 230, 1)","tableBtnIconPosition":"1","searchBtnHeight":"30px","tableBgColor":"rgba(255, 255, 255, 1)","inputIconColor":"#C0C4CC","searchBtnBorderRadius":"4px","tableStripe":false,"btnAdAllWarnFontColor":"#333","tableBtnDelBgColor":"#fff","searchBtnIcon":"1","tableSize":"medium","searchBtnBorderStyle":"solid","text":{"padding":"0","boxShadow":"0 0 0px rgba(0,0,0,.1)","margin":"20% auto 0","borderColor":"rgba(0,0,0,.3)","backgroundColor":"rgba(247, 247, 247, 0)","color":"#333","borderRadius":"6px","borderWidth":"0","width":"auto","lineHeight":"auto","fontSize":"50px","borderStyle":"solid"},"tableSelection":true,"searchBtnBorderWidth":"0 0 2px 0","tableContentFontSize":"14px","searchBtnBgColor":"rgba(173, 215, 230, 1)","inputTitleSize":"14px","btnAdAllBorderColor":"rgba(173, 215, 230, 1)","pageJumper":true,"btnAdAllIconPosition":"1","searchBoxPosition":"1","tableBtnDetailFontColor":"rgba(173, 215, 230, 1)","tableBtnHeight":"40px","pagePager":true,"searchBtnBorderColor":"rgba(0, 0, 0, 1)","tableHeaderFontColor":"rgba(0, 0, 0, 1)","inputTitle":"1","tableBtnBorderRadius":"40px","btnAdAllFont":"1","btnAdAllDelFontColor":"#333","tableBtnIcon":"1","btnAdAllHeight":"40px","btnAdAllWarnBgColor":"#fff","btnAdAllBorderWidth":"3px","tableStripeFontColor":"#606266","tableBtnBorderStyle":"solid","inputHeight":"30px","btnAdAllBorderRadius":"40px","btnAdAllDelBgColor":"#fff","pagePrevNext":true,"btnAdAllAddBgColor":"#fff","searchBtnFont":"1","tableIndex":true,"btnAdAllIcon":"1","tableSortable":false,"pageSizes":true,"tableFit":true,"pageBtnBG":false,"searchBtnFontSize":"14px","tableBtnEditBgColor":"#fff","box":{"padding":"10px","boxShadow":"0 0 6px rgba(0,0,0,0)","flag":"2","backgroundImage":"","background":"#fff"},"inputBorderWidth":"0 0 2px 0","inputFontPosition":"1","inputFontColor":"rgba(0, 0, 0, 1)","pageEachNum":10,"tableHeaderBgColor":"rgba(173, 215, 230, 1)","inputTitleColor":"#333","btnAdAllBoxPosition":"1","tableBtnDetailBgColor":"#fff","inputIcon":"0","searchBtnIconPosition":"1","btnAdAllFontSize":"14px","inputBorderStyle":"solid","tableHoverFontColor":"#333","inputBgColor":"rgba(173, 215, 230, 1)","pageStyle":true,"pageTotal":true,"btnAdAllAddFontColor":"#333","tableBtnFont":"1","tableContentFontColor":"#606266","inputBorderColor":"rgba(0, 0, 0, 1)","tableShowHeader":true,"tableHoverBgColor":"rgba(255, 255, 255, 1)","tableBtnFontSize":"14px","tableBtnBorderColor":"rgba(173, 215, 230, 1)","inputIconPosition":"1","tableBorder":true,"btnAdAllBorderStyle":"solid","tableBtnBorderWidth":"3px","tableStripeBgColor":"#F5F7FA","tableBtnEditFontColor":"rgba(173, 215, 230, 1)","tableAlign":"center"},
      layouts: '',


    };
  },
  created() {
    this.init();
    this.getDataList();
    this.contentStyleChange()
  },
  mounted() {

  },
  filters: {
    htmlfilter: function (val) {
      return val.replace(/<[^>]*>/g).replace(/undefined/g,'');
    }
  },
  components: {
    AddOrUpdate,
  },
  methods: {

    contentStyleChange() {
      this.contentSearchStyleChange()
      this.contentBtnAdAllStyleChange()
      this.contentSearchBtnStyleChange()
      this.contentTableBtnStyleChange()
      this.contentPageStyleChange()
    },
    contentSearchStyleChange() {
      this.$nextTick(()=>{
        document.querySelectorAll('.form-content .slt .el-input__inner').forEach(el=>{
          let textAlign = 'left'
          if(this.contents.inputFontPosition == 2) textAlign = 'center'
          if(this.contents.inputFontPosition == 3) textAlign = 'right'
          el.style.textAlign = textAlign
          el.style.height = this.contents.inputHeight
          el.style.lineHeight = this.contents.inputHeight
          el.style.color = this.contents.inputFontColor
          el.style.fontSize = this.contents.inputFontSize
          el.style.borderWidth = this.contents.inputBorderWidth
          el.style.borderStyle = this.contents.inputBorderStyle
          el.style.borderColor = this.contents.inputBorderColor
          el.style.borderRadius = this.contents.inputBorderRadius
          el.style.backgroundColor = this.contents.inputBgColor
        })
        if(this.contents.inputTitle) {
          document.querySelectorAll('.form-content .slt .el-form-item__label').forEach(el=>{
            el.style.color = this.contents.inputTitleColor
            el.style.fontSize = this.contents.inputTitleSize
            el.style.lineHeight = this.contents.inputHeight
          })
        }
        setTimeout(()=>{
          document.querySelectorAll('.form-content .slt .el-input__prefix').forEach(el=>{
            el.style.color = this.contents.inputIconColor
            el.style.lineHeight = this.contents.inputHeight
          })
          document.querySelectorAll('.form-content .slt .el-input__suffix').forEach(el=>{
            el.style.color = this.contents.inputIconColor
            el.style.lineHeight = this.contents.inputHeight
          })
          document.querySelectorAll('.form-content .slt .el-input__icon').forEach(el=>{
            el.style.lineHeight = this.contents.inputHeight
          })
        },10)

      })
    },
    // 搜索按钮
    contentSearchBtnStyleChange() {
      this.$nextTick(()=>{
        document.querySelectorAll('.form-content .slt .el-button--success').forEach(el=>{
          el.style.height = this.contents.searchBtnHeight
          el.style.color = this.contents.searchBtnFontColor
          el.style.fontSize = this.contents.searchBtnFontSize
          el.style.borderWidth = this.contents.searchBtnBorderWidth
          el.style.borderStyle = this.contents.searchBtnBorderStyle
          el.style.borderColor = this.contents.searchBtnBorderColor
          el.style.borderRadius = this.contents.searchBtnBorderRadius
          el.style.backgroundColor = this.contents.searchBtnBgColor
        })
      })
    },
    // 新增、批量删除
    contentBtnAdAllStyleChange() {
      this.$nextTick(()=>{
        document.querySelectorAll('.form-content .ad .el-button--success').forEach(el=>{
          el.style.height = this.contents.btnAdAllHeight
          el.style.color = this.contents.btnAdAllAddFontColor
          el.style.fontSize = this.contents.btnAdAllFontSize
          el.style.borderWidth = this.contents.btnAdAllBorderWidth
          el.style.borderStyle = this.contents.btnAdAllBorderStyle
          el.style.borderColor = this.contents.btnAdAllBorderColor
          el.style.borderRadius = this.contents.btnAdAllBorderRadius
          el.style.backgroundColor = this.contents.btnAdAllAddBgColor
        })
        document.querySelectorAll('.form-content .ad .el-button--danger').forEach(el=>{
          el.style.height = this.contents.btnAdAllHeight
          el.style.color = this.contents.btnAdAllDelFontColor
          el.style.fontSize = this.contents.btnAdAllFontSize
          el.style.borderWidth = this.contents.btnAdAllBorderWidth
          el.style.borderStyle = this.contents.btnAdAllBorderStyle
          el.style.borderColor = this.contents.btnAdAllBorderColor
          el.style.borderRadius = this.contents.btnAdAllBorderRadius
          el.style.backgroundColor = this.contents.btnAdAllDelBgColor
        })
        document.querySelectorAll('.form-content .ad .el-button--warning').forEach(el=>{
          el.style.height = this.contents.btnAdAllHeight
          el.style.color = this.contents.btnAdAllWarnFontColor
          el.style.fontSize = this.contents.btnAdAllFontSize
          el.style.borderWidth = this.contents.btnAdAllBorderWidth
          el.style.borderStyle = this.contents.btnAdAllBorderStyle
          el.style.borderColor = this.contents.btnAdAllBorderColor
          el.style.borderRadius = this.contents.btnAdAllBorderRadius
          el.style.backgroundColor = this.contents.btnAdAllWarnBgColor
        })
      })
    },
    // 表格
    // rowStyle({ row, rowIndex}) {
    //   if (rowIndex % 2 == 1) {
    //     if(this.contents.tableStripe) {
    //       return {color:this.contents.tableStripeFontColor}
    //     }
    //   } else {
    //     return ''
    //   }
    // },
    // cellStyle({ row, rowIndex}){
    //   if (rowIndex % 2 == 1) {
    //     if(this.contents.tableStripe) {
    //       return {backgroundColor:this.contents.tableStripeBgColor}
    //     }
    //   } else {
    //     return ''
    //   }
    // },
    headerRowStyle({ row, rowIndex}){
      return {color: this.contents.tableHeaderFontColor}
    },
    headerCellStyle({ row, rowIndex}){
      return {backgroundColor: this.contents.tableHeaderBgColor}
    },
    // 表格按钮
    contentTableBtnStyleChange(){
      // this.$nextTick(()=>{
      //   setTimeout(()=>{
      //     document.querySelectorAll('.table-content .tables .el-table__body .el-button--success').forEach(el=>{
      //       el.style.height = this.contents.tableBtnHeight
      //       el.style.color = this.contents.tableBtnDetailFontColor
      //       el.style.fontSize = this.contents.tableBtnFontSize
      //       el.style.borderWidth = this.contents.tableBtnBorderWidth
      //       el.style.borderStyle = this.contents.tableBtnBorderStyle
      //       el.style.borderColor = this.contents.tableBtnBorderColor
      //       el.style.borderRadius = this.contents.tableBtnBorderRadius
      //       el.style.backgroundColor = this.contents.tableBtnDetailBgColor
      //     })
      //     document.querySelectorAll('.table-content .tables .el-table__body .el-button--primary').forEach(el=>{
      //       el.style.height = this.contents.tableBtnHeight
      //       el.style.color = this.contents.tableBtnEditFontColor
      //       el.style.fontSize = this.contents.tableBtnFontSize
      //       el.style.borderWidth = this.contents.tableBtnBorderWidth
      //       el.style.borderStyle = this.contents.tableBtnBorderStyle
      //       el.style.borderColor = this.contents.tableBtnBorderColor
      //       el.style.borderRadius = this.contents.tableBtnBorderRadius
      //       el.style.backgroundColor = this.contents.tableBtnEditBgColor
      //     })
      //     document.querySelectorAll('.table-content .tables .el-table__body .el-button--danger').forEach(el=>{
      //       el.style.height = this.contents.tableBtnHeight
      //       el.style.color = this.contents.tableBtnDelFontColor
      //       el.style.fontSize = this.contents.tableBtnFontSize
      //       el.style.borderWidth = this.contents.tableBtnBorderWidth
      //       el.style.borderStyle = this.contents.tableBtnBorderStyle
      //       el.style.borderColor = this.contents.tableBtnBorderColor
      //       el.style.borderRadius = this.contents.tableBtnBorderRadius
      //       el.style.backgroundColor = this.contents.tableBtnDelBgColor
      //     })

      //   }, 50)
      // })
    },
    // 分页
    contentPageStyleChange(){
      let arr = []

      if(this.contents.pageTotal) arr.push('total')
      if(this.contents.pageSizes) arr.push('sizes')
      if(this.contents.pagePrevNext){
        arr.push('prev')
        if(this.contents.pagePager) arr.push('pager')
        arr.push('next')
      }
      if(this.contents.pageJumper) arr.push('jumper')
      this.layouts = arr.join()
      this.contents.pageEachNum = 10
    },

    payHandler(row){
      this.$storage.set('paytable','wuyeshoufei');
      this.$storage.set('payObject',row);
      this.$router.push('pay');
    },
    chartDialog() {
      this.chartVisiable = !this.chartVisiable;
      this.$nextTick(()=>{
        // shoufeiyuefen shoufeiyuefen
        //  zongjine
        // xcolumn ycolumn shoufeiyuefen zongjine
        var zongjineChart = this.$echarts.init(document.getElementById("zongjineChart"),'macarons');
        this.$http({
            url: `wuyeshoufei/value/shoufeiyuefen/zongjine`,
            method: "get"
        }).then(({ data }) => {
            if (data && data.code === 0) {
                let res = data.data;
                let xAxis = [];
                let yAxis = [];
                let pArray = []
                for(let i=0;i<res.length;i++){
                    xAxis.push(res[i].shoufeiyuefen);
                    yAxis.push(parseFloat((res[i].total)).toFixed(2));
                    pArray.push({
                    value: parseFloat((res[i].total)),
                    name: res[i].shoufeiyuefen
                    })
                    var option = {};
                    option = {
                        tooltip: {
                          trigger: 'item',
                          formatter: '{b} : {c}'
                        },
                        title: {
                            text: '物业收费',
                            left: 'center'
                        },
                        xAxis: {
                            type: 'category',
                            data: xAxis
                        },
                        yAxis: {
                            type: 'value'
                        },
                        series: [{
                            data: yAxis,
                            type: 'bar'
                        }]
                    };
                    // 使用刚指定的配置项和数据显示图表。
                    zongjineChart.setOption(option);
                    //根据窗口的大小变动图表
                    window.onresize = function() {
                        zongjineChart.resize();
                    };
                }
            }
        });
      })
    },
    init () {
          this.shoufeiyuefenOptions = "1月份,2月份,3月份,4月份,5月份,6月份,7月份,8月份,9月份,10月份,11月份,12月份".split(',')
          this.$http({
            url: `option/yonghu/yonghuzhanghao`,
            method: "get"
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.yonghuzhanghaoOptions = data.data;
            } else {
              this.$message.error(data.msg);
            }
          });
    },
    search() {
      this.pageIndex = 1;
      this.getDataList();
    },

    // 获取数据列表
    getDataList() {
      this.dataListLoading = true;
      let params = {
        page: this.pageIndex,
        limit: this.pageSize,
        sort: 'id',
      }
          if(this.searchForm.shoufeiyuefen!='' && this.searchForm.shoufeiyuefen!=undefined){
            params['shoufeiyuefen'] = this.searchForm.shoufeiyuefen
          }
          if(this.searchForm.yonghuzhanghao!='' && this.searchForm.yonghuzhanghao!=undefined){
            params['yonghuzhanghao'] = this.searchForm.yonghuzhanghao
          }
      this.$http({
        url: "wuyeshoufei/page",
        method: "get",
        params: params
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.dataList = data.data.list;
          this.totalPage = data.data.total;
        } else {
          this.dataList = [];
          this.totalPage = 0;
        }
        this.dataListLoading = false;
      });
    },
    // 每页数
    sizeChangeHandle(val) {
      this.pageSize = val;
      this.pageIndex = 1;
      this.getDataList();
    },
    // 当前页
    currentChangeHandle(val) {
      this.pageIndex = val;
      this.getDataList();
    },
    // 多选
    selectionChangeHandler(val) {
      this.dataListSelections = val;
    },
    // 添加/修改
    addOrUpdateHandler(id,type) {
      this.showFlag = false;
      this.addOrUpdateFlag = true;
      this.crossAddOrUpdateFlag = false;
      if(type!='info'){
        type = 'else';
      }
      this.$nextTick(() => {
        this.$refs.addOrUpdate.init(id,type);
      });
    },
    // 查看评论
    // 下载
    download(file){
      window.open(`${file}`)
    },
    // 删除
    deleteHandler(id) {
      var ids = id
        ? [Number(id)]
        : this.dataListSelections.map(item => {
            return Number(item.id);
          });
      this.$confirm(`确定进行[${id ? "删除" : "批量删除"}]操作?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(() => {
        this.$http({
          url: "wuyeshoufei/delete",
          method: "post",
          data: ids
        }).then(({ data }) => {
          if (data && data.code === 0) {
            this.$message({
              message: "操作成功",
              type: "success",
              duration: 1500,
              onClose: () => {
                this.search();
              }
            });
          } else {
            this.$message.error(data.msg);
          }
        });
      });
    },


  }

};
</script>
<style lang="scss" scoped>
  .slt {
    margin: 0 !important;
    display: flex;
  }

  .ad {
    margin: 0 !important;
    display: flex;
  }

  .pages {
    & /deep/ el-pagination__sizes{
      & /deep/ el-input__inner {
        height: 22px;
        line-height: 22px;
      }
    }
  }
  

  .el-button+.el-button {
    margin:0;
  } 

  .tables {
	& /deep/ .el-button--success {
		height: 40px;
		color: rgba(173, 215, 230, 1);
		font-size: 14px;
		border-width: 3px;
		border-style: solid;
		border-color: rgba(173, 215, 230, 1);
		border-radius: 40px;
		background-color: #fff;
	}
	
	& /deep/ .el-button--primary {
		height: 40px;
		color: rgba(173, 215, 230, 1);
		font-size: 14px;
		border-width: 3px;
		border-style: solid;
		border-color: rgba(173, 215, 230, 1);
		border-radius: 40px;
		background-color: #fff;
	}
	
	& /deep/ .el-button--danger {
		height: 40px;
		color: rgba(173, 215, 230, 1);
		font-size: 14px;
		border-width: 3px;
		border-style: solid;
		border-color: rgba(173, 215, 230, 1);
		border-radius: 40px;
		background-color: #fff;
	}

    & /deep/ .el-button {
      margin: 4px;
    }
  }
	.form-content {
		background: transparent;
	}
	.table-content {
		background: transparent;
	}
	
	.tables /deep/ .el-table__body tr {
				background-color: rgba(255, 255, 255, 1) !important;
				color: #606266 !important;
	 }
	.tables /deep/ .el-table__body tr.el-table__row--striped td {
	    background: transparent;
	}
	.tables /deep/ .el-table__body tr.el-table__row--striped {
		background-color: #F5F7FA !important;
		color: #606266 !important;
	}
	
	 .tables /deep/ .el-table__body tr:hover>td {
	   	   background-color: rgba(255, 255, 255, 1) !important;
	   	   	   color: #333 !important;
	   	 }
	 
</style>
