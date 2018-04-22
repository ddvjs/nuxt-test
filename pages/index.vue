<template>
  <section>

    <div class="admin-tabmenu__wrap">
      <ul class="admin-tabmenu__ul clearfix">
        <li :class="{
              'admin-tabmenu__li':1,
              'admin-tabmenu__linow':tab.isAllNav
            }"
            @click="tab.isAllNav = !tab.isAllNav"
        >
          全部菜单
        </li>
        <li :class="{
              'admin-tabmenu__li':1,
              'admin-tabmenu__linow':!tab.isAllNav&&item.id===tab.id
            }"
            :key="item.id"
            v-for="item in tab.lists"
            @click="onClickTab(item)"
        >
          {{item.title}}
          <div v-if="item.closable!==false" class="admin-tabmenu__close" @click.stop="onClickTabRemove(item)">
            <span :class="{'admin-tabmenu__span': item.id == tab.id}">X</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="admin-taballmenu__wrap" v-show="tab.isAllNav">
      main nav menu
    </div>

    <div class="admin-tabmain__wrap" v-show="!tab.isAllNav">
      <div class="admin-tabmain__navwarp">
        left nav menu
      </div>


      <ul class="admin-tabmain__ul">
        <li
          v-show="item.id===tab.id"
          class="admin-tabmain__li"
          :key="item.id"
          v-for="item in tab.lists"
        >
          <iframe
            @load="onIframe(item)"
            class="admin-tabmain__iframe"
            allowtransparency="true"
            frameborder="0"
            scrolling="no"
            :src="item.src"
            :ref="item.id+'Iframe'">
          </iframe>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
export default {
  head () {
    return {
      title: 'test',
      meta: [
        {'charset': 'utf-8'},
        {'name': 'viewport', 'content': 'width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no'},
        {'name': 'renderer', 'content': 'webkit'}
      ]
    }
  },
  data () {
    return {
      tab: {
        isAllNav: false,
        id: '',
        lastIndex: 0,
        lists: [
          {
            'id': 'a0',
            'title': 'test-1',
            'src': '/test-1',
            'closable': false
          },
          {
            'id': 'a1',
            'title': 'test-2',
            'src': '/test-2'
          },
          {
            'id': 'a2',
            'title': 'test-3',
            'src': '/test-3'
          }
        ],
        history: []
      }
    }
  },
  methods: {
    handleOpen (key, keyPath) {
      console.log(key, keyPath)
    },
    handleClose (key, keyPath) {
      console.log(key, keyPath)
    },
    toTabById (id) {
      console.log('toTabById', id)
      this.tab.id = id
    },
    onClickTab (tab) {
      this.tab.isAllNav = false
      console.log('onClickTab', tab)
      this.toTabById(tab.id)
      var idNow = tab.id
      removeArray(this.tab.history, item => item === idNow)
      this.tab.history.unshift(idNow)
    },
    onClickTabRemove (tab) {
      removeArray(this.tab.history, item => item === tab.id)
      removeArray(this.tab.lists, item => item.id === tab.id)
      this.toTabById(this.tab.history[0] || this.tab.lists[0].id)
      console.log('onClickTabRemove', tab)
      return false
    },
    onIframe (item) {
      console.log('onIframe', item)
    },
    createTabId () {
      return this.tab.lastIndex++
    },
    addClickTab (url) {
      var id = this.createTabId()
      this.tab.lists.push({
        id,
        'title': `id[${id}]`,
        'src': url
      })
      // this.toTabById(id)
    },
    init () {
       this.addClickTab('/test-4')
      // this.addClickTab('http://127.0.0.1:3000/admin/news/lists')
      // d.tab.init({
      //   addClickTab: url => this.addClickTab(url)
      // })
      return Promise.resolve()
    }
  },
  watch: {
    // '$route.path': {
    //   handler: 'changePage',
    //   deep: true
    // }
  },
  computed: {
    tabNow () {
      var item
      for (var i = 0; i < this.tab.lists.length; i++) {
        item = this.tab.lists[i]
        if (item.id === this.tab.id) {
          return item
        }
      }
      return null
    }
  },
  created () {
  },
  mounted () {
    this.init()
  }
}
function removeArray(array, fn) {
    var res = []
    array.forEach((item, index) => {
      if (fn(item)) {
        res.push.apply(res, array.splice(index, 1))
      }
    })
    return res
  }
</script>
<style type="text/css">
body{
  margin: 0;
  padding: 0;
}
</style>
<style type="text/css">
  
.admin-tab__iframe {
      width: 100%;
      height: 600px;
}
.admin-tabmenu__wrap {
      display: block;
}
.admin-tabmenu__ul {
      display: block;
      background: #0da3b9;
      width: 100%;
      height: 50px;
      margin: 0;
}
.admin-tabmenu__li {
      display: block;
      outline: 0;
      cursor: pointer;
      float: left;
      position: relative;
      z-index: 3;
      height: 22px;
      border-width: 1px 1px 0;
      /* border-style: solid; */
      width: 91px;
      text-align: center;
      line-height: normal;
      font-size: 14px;
      padding: 7px 10px 2px 8px;
      -moz-user-select: none;
      -webkit-user-select: none;
      -ms-user-select: none;
      user-select: none;
      margin: 0;
      border-bottom-width: 0!important;
      color: #fff;
      background: #3bbad5;
      margin-top: 18px;
      margin-right: 10px;
}
.admin-tabmenu__linow {
      background: #fff;
      color: #000;
}
.admin-tabmenu__span {
      color: #000;
}
.admin-tabmenu__close {
      display: block;
      color: #fff;
      float: right;
}
.admin-tabmain__wrap {
      clear: both;
      display: block;
      width: 100%;
      margin: 0;
}
.admin-tabmain__navwarp {
      float: left;
      width: 230px;
    /*  background: #f14;
      height: 55px;*/
}
.admin-tabmain__ul {
      float: left;
}
.admin-tabmain__li {
      display: block;
}
.admin-tabmain__iframe {
      width: 600px;
      height: 600px;
}
.el-submenu .el-menu-item {
    padding-left: 20px !important;
}

</style>