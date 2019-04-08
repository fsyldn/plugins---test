
// import vuescroll from 'vuescroll'

// Vue.use(vuescroll)

<template>
  <div class="pr-wrap">
    <div class="wrap-part first">
      <div style="height:100px">s
        dsddd
      </div>
      <vue-scroll
        ref="vs"
        :ops="ops"
        @load-before-deactivate="handleLBD"
        @load-start="handleLoadStart"
      >
        <template v-for="(item, index) in amount">
          <div
            class="rl-child"
            :key="index"
            :class="getClass(index)"
          >
            <div>dsddd</div>
          </div>
        </template>
        <div
          slot="load-beforeDeactive"
          v-if="noData"
        >
        我是有底线的啊
        </div>
      </vue-scroll>
    </div>

  </div>
</template>

<script>
export default {
  props: {
    lang: {
      default: 'en'
    }
  },
  mounted () {
    console.log(this.$refs.op)
  },
  data () {
    const config = {}
    const ops = {
      vuescroll: {
        mode: 'slide',
        pullRefresh: {
          enable: false
        },
        pushLoad: {
          enable: true,
          auto: true,
          autoLoadDistance: 10
        }
      }
    }
    ops.vuescroll.pullRefresh.tips = {
      deactive: '下拉刷新',
      active: '释放刷新',
      start: '刷新中...',
      beforeDeactive: '刷新成功!'
    }
    ops.vuescroll.pushLoad.tips = {
      deactive: '上拉加载',
      active: '释放加载',
      start: '加载中...',
      beforeDeactive: '加载成功!'
    }
    // config.animateTip = '您也可以通过slot来自定义不同的刷新/加载动画。'
    // config.animateAddr =
    //     'https://vuescrolljs.yvescoding.org/zh/guide/slot.html#用法-2'

    return {
      ops,
      config,
      itemAmount: 3,
      refresh: 1,
      noData: false
    }
  },
  computed: {
    amount () {
      function getRandom () {
        let str = '#'
        for (let i = 0; i < 6; i++) {
          str += Math.floor(Math.random() * 16).toString(16)
        }
        return str
      }
      return (
        this.refresh &&
        Array.apply(null, {
          length: this.itemAmount
        }).map(item => {
          return getRandom()
        })
      )
    }
  },
  methods: {
    getClass (index) {
      return ['child' + ((index % 7) + 1)]
    },

    handleLoadStart (vm, dom, done) {
      setTimeout(() => {
        const random = Math.floor(Math.random() * 2) + 1
        if (random == 1) {
          this.noData = true
        } else {
          this.noData = false
        }
        done()
      }, 600)
    },
    handleLBD (vm, loadDom, done) {
      setTimeout(() => {
        if (!this.noData) {
          this.itemAmount += 2
        }
        done()
      }, 500)
    },
    trigger () {
      this.$refs['vs'].triggerRefreshOrLoad('load')
    }
  }
}
</script>

<style lang="less" scoped>
@media (max-width: 719px) {
  .pr-wrap {
    flex-wrap: wrap;
    .wrap-part {
      width: 100% !important;
      &.first {
        height: 100%;
      }
      &.second {
        height: 40%;
      }
    }
  }
}
.pr-wrap {
  display: flex;
  height: 100%;
  justify-content: center;
  table {
    margin: 0;
  }
  .rl-child {
    width: 100%;
    height: 200px;
  }
  .wrap-part {
    height: 100%;
    &.first {
      width: 40%;
    }
    &.second {
      width: 30%;
    }
    .parent {
      .rl-child {
      }
    }
    table {
      display: table;
      width: 100%;
    }
  }
}
.child1 {
  background-color: #43d2c6;
}
.child2 {
  background-color: #589be5;
}
.child3 {
  background-color: #f3b500;
}
.child4 {
  background-color: #ff705a;
}
.child5 {
  background-color: #fe7a9c;
}
.child6 {
  background-color: #7a85ee;
}
.child7 {
  background-color: #57cc71;
}
.animate-tip {
  text-align: center;
}
</style>
