<template>
  <div>
    <template v-if="!this.advancedConfig_STATUS">
      <!-- 左侧列表 -->
      <div class="nav">
        <!-- 导航列表 -->
        <div class="nav_list">
          <ul>
            <li v-for="(item,index) of equipNoArr.value" :key="index" @click="getEquipMsgObj(item,index)" :class="{active:currentEquipMsgObjIndex==index}">{{item.equip_nm}}</li>
          </ul>
        </div>
      </div>
      <!-- 右侧内容块 -->
      <div class="content">
        <div class="content_list">
          <ul @change="getChangeValue($event)">
            <!-- <ul> -->
            <li>
              <span>设备号：</span>
              <b>{{currentEquipMsgObj.equip_no}}</b>
              <div class="clear"></div>
            </li>
            <li>
              <span>设备名称： </span>
              <input type="text" v-model="currentEquipMsgObj.equip_nm" listName="equip_nm">
              <div class="clear"></div>
            </li>
            <li>
              <span>关联页面：</span>
              <input type="text" v-model="currentEquipMsgObj.related_pic" listName="related_pic">
              <div class="clear"></div>
            </li>
            <li>
              <span>关联视频：</span>
              <select v-model="currentEquipMsgObj.related_video" listName="related_video">
                <option v-for="(item,index) of QueryTableData[1].value" :key="index">{{item.related_video}}</option>
              </select>
              <div class="clear"></div>
            </li>
            <li>
              <span>资产编号：</span>
              <select v-model="currentEquipMsgObj.ZiChanID" listName="ZiChanID">
                <option v-for="(item,index) of QueryTableData[2].value" :key="index">{{item.ZiChanName}}</option>
              </select>
              <div class="clear"></div>
            </li>
            <li>
              <span>预案号：</span>
              <select v-model="currentEquipMsgObj.PlanNo" listName="PlanNo">
                <option v-for="(item,index) of QueryTableData[3].value" :key="index">{{item.PlanNo}}</option>
              </select>
              <div class="clear"></div>
            </li>
          </ul>
        </div>
        <div class="content_alarm">
          <!-- <ul @change="getChangeValue($event)"> -->
          <ul>
            <li>
              <span>显示报警：</span>
              <input type="checkbox" :checked="alarmSchemes(currentEquipMsgObj.alarm_scheme,1)" listName="alarm_scheme">
              <div class="clear"></div>
            </li>
            <li>
              <span>记录报警：</span>
              <input type="checkbox" :checked="alarmSchemes(currentEquipMsgObj.alarm_scheme,2)" listName="alarm_scheme">
              <div class="clear"></div>
            </li>
            <li>
              <!-- TODO:不确定判断字段 -->
              <span>Eamil报警</span>
              <input type="checkbox">
              <div class="clear"></div>
            </li>
          </ul>
        </div>
      </div>
      <!-- 底部按钮块 -->
      <div class="foot_btn">
        <span>配置更多属性，请单击 </span>
        <button class="advancedConfig" @click="advancedConfig()">高级配置</button>
      </div>
    </template>
    <!-- 高级配置内容块 -->
    <template v-if="this.advancedConfig_STATUS">
      <div class="detailedConfig">
        <div class="content_list">
          <ul @change="getChangeValue($event)">
            <li>
              <span>设备属性：</span>
              <input type="text" v-model="currentEquipMsgObj.detail" listName="detail">
              <div class="clear"></div>
            </li>
            <li>
              <span>通讯刷新周期：</span>
              <input type="text" v-model="currentEquipMsgObj.acc_cyc" listName="acc_cyc">
              <div class="clear"></div>
            </li>
            <li>
              <span>通故障处理意见：</span>
              <input type="text" v-model="currentEquipMsgObj.proc_advice" listName="proc_advice">
              <div class="clear"></div>
            </li>
            <li>
              <span>故障提示：</span>
              <input type="text" v-model="currentEquipMsgObj.out_of_contact" listName="out_of_contact">
              <div class="clear"></div>
            </li>
            <li>
              <span>故障恢复提示：</span>
              <input type="text" v-model="currentEquipMsgObj.contacted" listName="contacted">
              <div class="clear"></div>
            </li>
            <li>
              <span>报警声音文件：</span>
              <input type="text" v-model="currentEquipMsgObj.event_wav" listName="event_wav">
              <div class="clear"></div>
            </li>
            <li>
              <span>驱动文件：</span>
              <input type="text" v-model="currentEquipMsgObj.communication_drv" listName="communication_drv">
              <div class="clear"></div>
            </li>
  
            <li>
              <span>通讯端口：</span>
              <input type="text" v-model="currentEquipMsgObj.local_addr" listName="local_addr">
              <div class="clear"></div>
            </li>
  
            <li>
              <span>设备地址：</span>
              <input type="text" v-model="currentEquipMsgObj.equip_addr" listName="equip_addr">
              <div class="clear"></div>
            </li>
  
            <li>
              <span>通讯参数：</span>
              <input type="text" v-model="currentEquipMsgObj.communication_param" listName="communication_param">
              <div class="clear"></div>
            </li>
            <li>
              <span>通讯时间参数：</span>
              <input type="text" v-model="currentEquipMsgObj.communication_time_param" listName="communication_time_param">
              <div class="clear"></div>
            </li>
            <li>
              <span>报警升级周期(分钟)：</span>
              <input type="text" v-model="currentEquipMsgObj.AlarmRiseCycle" listName="AlarmRiseCycle">
              <div class="clear"></div>
            </li>
            <li>
              <span>模板设备号：</span>
              <input type="text" v-model="currentEquipMsgObj.raw_equip_no" listName="raw_equip_no">
              <div class="clear"></div>
            </li>
            <li>
              <span>附表名称：</span>
              <input type="text" v-model="currentEquipMsgObj.tabname" listName="tabname">
              <div class="clear"></div>
            </li>
            <li>
              <span>属性：</span>
              <input type="text" v-model="currentEquipMsgObj.attrib" listName="attrib">
              <div class="clear"></div>
            </li>
            <li>
              <span>安全时段：</span>
              <input type="text" v-model="currentEquipMsgObj.SafeTime" listName="SafeTime">
              <div class="clear"></div>
            </li>
          </ul>
        </div>
      </div>
      <div class="foot_btn">
        <button class="advancedConfig" @click="advancedConfig()">返回</button>
      </div>
    </template>
  </div>
</template>

<script type="es6">
export default {
  props: ["equipNoArrA", "curractiveIndex", "queryTableData"],
  data() {
    return {
      advancedConfig_STATUS: false,
      // 当前被选中的设备号
      currentEquipNo: '',
      // 当前被选中的设备信息
      currentEquipMsg: [],

      // 当前被选中的设备详细信息index
      currentEquipMsgObjIndex: '',
      // 当前被选中的设备详细信息
      currentEquipMsgObj: {},

      // 提交上去的jsonChange
      temJsonChange: []
    }
  },
  methods: {
    updataCheckbox(e, item) {
      this.currentEquipMsgObj[item] = e.target.checked
    },
    getChangeValue(e) {

      if (!this.currentEquipMsgObj.equip_no) {
        return
      }
      let temObj
      if (e.target.type === "checkbox") {
        temObj = {
          id: this.currentEquipMsgObj.equip_no,
          listName: e.target.getAttribute('listName'),
          vlaue: `'${e.target.checked}'`
        }
      } else {
        if (!e.target.value) {
          return
        }
        temObj = {
          id: this.currentEquipMsgObj.equip_no,
          listName: e.target.getAttribute('listName'),
          vlaue: `'${e.target.value}'`
        }
      }
      this.temJsonChange.push(temObj)
      // console.log(temObj);
      this.$emit("getJsonChangeArr", temObj)
    },
    advancedConfig() {
      this.advancedConfig_STATUS = !this.advancedConfig_STATUS
    },
    // 点击二级设备列表获取当前详细信息
    getEquipMsgObj(item, index) {
      this.currentEquipMsgObjIndex = index
      this.currentEquipMsgObj = item
    },
    // 是否显示报警
    alarmSchemes(item, index) {
      if ((item & index) > 0) {
        return true
      } else {
        return false
      }
    },
  },
  computed: {
    equipNoArr() {
      return this.equipNoArrA
    },
    QueryTableData() {
      return this.queryTableData
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../../../style/mixin.scss';

@mixin ul-li {
  ul {
    list-style: none;
    padding: 0;
    margin: 0;
    white-space: nowrap;
    li {
      line-height: 30px;
      padding: 0 12px;
      cursor: default;
      transition: all 300ms linear 0s;
      /* 此代码可以提取为公用 */
      &:nth-child(odd) {
        background: rgba(0, 0, 0, .13);
        border-top: 1px solid rgba(255, 255, 225, .15);
        border-bottom: 1px solid rgba(255, 255, 225, .15);
      }
      &:nth-child(even) {
        background-color: transparent;
      }
      &:hover {
        background: rgba(255, 255, 255, .2);
      }
    }
    .active {
      background: rgba(255, 255, 255, .36)!important;
    }
  }
}

@mixin content-list($span-min-width, $input_select-width, $content_list-height) {
  .content_list {
    overflow: auto;
    height: $content_list-height; // height: 100%;
    @include ul-li;
    span {
      float: left;
      display: block;
      min-width: $span-min-width;
      text-align: right;
    }
    input[type="text"],
    select {
      background: rgba(255, 255, 255, .2);
      border: 1px solid rgba(255, 255, 255, .4);
      color: #FFF;
      margin: 0px;
      padding: 6px 12px;
      display: block;
      transition: all 300ms linear 0s;
      float: left;
      width: $input_select-width;
      display: block;
      top: 2px!important;
    }
  }
  .content_alarm {
    border: 1px solid rgba(255, 255, 225, .15);
    overflow: auto;
    height: 87px;
    @include ul-li;
    span {
      float: left;
      display: block;
      min-width: $span-min-width;
      text-align: right;
    }
  }
}

//  左侧导航  
.nav {
  @include absoluteWH(auto, auto);
  @include trbl(0, 66%, 35px, 0);
  overflow-y: auto;
  display: block;
  border: 1px solid rgba(255, 255, 225, .15);
  /*  导航条件选择快 */
  .nav_source {
    background: rgba(255, 255, 255, .2);
    border: 1px solid rgba(255, 255, 255, .4);
    color: #FFF;
    transition: all 300ms linear 0s;
    padding: 5px 10px;
    outline: 0;
    width: 100%;
    option {
      background: rgba(0, 0, 0, .9);
      border-color: rgba(0, 0, 0, .4);
      color: #FFF;
    }
  } // 导航列表
  .nav_list {
    height: auto;
    overflow: auto;
    @include ul-li;
  }
}

//  右侧内容块 
.setparm {
  .content {
    @include absoluteWH(auto, auto);
    @include trbl(0, 0, 35px, 34%); // display: block;
    // bottom: 35px!important;
    border: 1px solid rgba(255, 255, 225, .15);
    @include content-list(82px, 185px, 196px);
  }
}

.detailedConfig {
  @include absoluteWH(auto, auto);
  @include trbl(0, 0, 35px, 0);
  @include content-list(182px, auto, 100%);
}

//  底部按钮样式
// 此段代码可以共用 
.foot_btn {
  position: absolute;
  left: 13px;
  bottom: 0;
  right: 13px;
  margin: 3px 0;
  text-align: right;
  .advancedConfig {
    border: none;
    border-radius: 2px;
    padding: 6px 12px;
    box-shadow: inset 0 -2px 0 rgba(0, 0, 0, .05);
    color: #fff;
    font-family: "Microsoft YaHei";
    background-color: rgba(50, 219, 1, 0.3);
    &:hover {
      background-color: rgba(50, 219, 1, 0.6);
      color: #fff;
      box-shadow: 0 0 2rem 0.5rem rgba(50, 219, 1, 0.3);
    }
  }
}
</style>
