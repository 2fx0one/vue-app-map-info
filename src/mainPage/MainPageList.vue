<template>
  <div :style="{width: '100%', height: fullHeight + 'px' }">
    <!--<cube-index-list-->
    <!--:data="cityData"-->
    <!--:title="title"-->
    <!--@select="selectItem"-->
    <!--&gt;</cube-index-list>-->
    <div>
      <cube-checkbox-group
        v-model="checkList"
        :options="options"
        :horizontal="true"
        shape="square"
        :hollow-style="true"
        @click="clickCheckBox">
      </cube-checkbox-group>
    </div>

    <div :style="{width: '100%', height: fullHeight-50 + 'px' }">
      <cube-scroll>
        <cube-swipe
          @item-click="onItemClick"
          @btn-click="onBtnClick"
          :data="caseListData">
        </cube-swipe>
      </cube-scroll>
    </div>
  </div>
</template>

<script>
  import {caseList, getDict} from "@/api/case";

  export default {
    name: "MainPageList",
    created() {


      // getDict('CASE_SOURCE').then(response => {
      //   console.log(response)
      // })
      caseList({}).then(response => {
        console.log(response)
        this.caseListData = response.map(v => ({
          item: {
            target: v,
            text: v.caseName,
            value: v.id
          },
          btns: [
            // {
            //   action: 'clear',
            //   text: '不再关注',
            //   color: '#c8c7cd'
            // },
            {
              action: 'delete',
              text: '编辑',
              color: '#ff3a32'
            }
          ]
        }))
      }).catch(err => console.log(err))
      console.log(this.fullHeight);

      // let swipeData = [];
      // for (let i = 0; i < 100; i++) {
      //   swipeData.push({
      //     item: {
      //       text: '测试' + i,
      //       value: i
      //     },
      //     btns: [
      //       // {
      //       //   action: 'clear',
      //       //   text: '不再关注',
      //       //   color: '#c8c7cd'
      //       // },
      //       {
      //         action: 'delete',
      //         text: '编辑',
      //         color: '#ff3a32'
      //       }
      //     ]
      //   })
      // }
      // this.swipeData = swipeData;
    },
    data() {
      return {
        fullHeight: document.documentElement.clientHeight - 56,
        checkList: ['2'],
        options: [
          {
            label: '经营户',
            value: '1',
          },
          {
            label: '案件',
            value: '2',
          },
          {
            label: '情报',
            value: '3',
          }
        ],
        caseListData: [{
          item: {
            text: '测试1',
            value: 1
          },
          btns: [
            {
              action: 'clear',
              text: '不再关注',
              color: '#c8c7cd'
            },
            {
              action: 'delete',
              text: '删除',
              color: '#ff3a32'
            }
          ]
        }],
      }
    },
    methods: {
      clickCheckBox() {
        console.log(this.checkList)
      },
      onItemClick(item) {
        console.log('click item:', item)
        console.log('click createTime:', item.target.createTime)
        console.log('click gisLongitude:', item.target.gisLongitude)
        console.log('click gisLatitudes:', item.target.gisLatitudes)
      },
      onBtnClick(btn, index) {
        if (btn.action === 'delete') {
          this.$createActionSheet({
            title: '确认要删除吗',
            active: 0,
            data: [
              {content: '删除'}
            ],
            onSelect: () => {
              this.swipeData.splice(index, 1)
            }
          }).show()
        }
      }
    },
    // computed: {
    //   options() {
    //     return {
    //       // scrollbar: this.scrollbarObj,
    //       startY: this.startY
    //     }
    //   }
    // }

  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .swipe-wrapper {
    height 500px;
  }

  .cube-swipe-item-inner {
    text-align: left;
    line-height: 30px
  }

  .scroll-list-wrap {
    height: 500px;
    border: 1px solid rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    transform: rotate(0deg); // fix 子元素超出边框圆角部分不隐藏的问题
    overflow: hidden;
  }
</style>
