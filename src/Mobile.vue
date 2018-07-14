<template>
  <div id="app">
    <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
    <ResumeEditor ref="resumeEditor" :markdown="currentMarkdown" :enableHtml="enableHtml"></ResumeEditor>
  </div>
</template>

<script>
  import StyleEditor from './components/StyleEditor'
  import ResumeEditor from './components/ResumeEditor'
  import './assets/reset.css'

  export default {
    name: 'app',
    components: {
      StyleEditor,
      ResumeEditor
    },
    data() {
      return {
        interval: 5,
        currentStyle: '',
        enableHtml: false,
        fullStyle: [
          `/*
* Inspired by http://strml.net/
* 源码地址 https://github.com/sitexa/anires
* 大家好，我是黄珂。
* 我来改一个网页（一个大神的源码）。
*/

/* 给所有元素加上过渡效果 */
* {
  transition: all .2s;
}
/* 设置背景颜色 */
html {
  color: rgb(222,222,222);
  background: rgb(0,43,54);
}
/* 设置边框 */
.styleEditor {
  padding: .5em;
  border: 1px solid;
  overflow: auto;
  width: 90vw;
  margin: 2.5vh 5vw;
  height: 90vh;
}
/* 太高了 */
.styleEditor {
  height: 45vh;
}
/* 代码高亮 */
.token.selector{
  color: rgb(133,153,0);
}
.token.property{
  color: rgb(187,137,0);
}
.token.punctuation{
  color: yellow;
}
.token.function{
  color: rgb(42,161,152);
}

/* 加3D效果 */
html{
  perspective: 1000px;
}
.styleEditor {
  position: fixed; left: 0; top: 0;
  transform: rotateX(-10deg) translateZ(-50px) ;
}

/* 准备一个编辑器 */
.resumeEditor{
  position: fixed;
  top: 50%; left: 0;
  padding: .5em;  margin: 2.5vh;
  width: 95vw; height: 45vh;
  border: 1px solid;
  background: white; color: #222;
  overflow: auto;
}
/* 开始吹牛逼 */


`,
          `
/*将Markdown格式翻译成HTML
 *再对HTML加点样式
*/
`, `
.resumeEditor{
  padding: 2em;
}
.resumeEditor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
}
.resumeEditor ul,.resumeEditor ol{
  list-style: none;
}
.resumeEditor ul> li::before{
  content: '•';
  margin-right: .5em;
}
.resumeEditor ol {
  counter-reset: section;
}
.resumeEditor ol li::before {
  counter-increment: section;
  content: counters(section, ".") " ";
  margin-right: .5em;
}
.resumeEditor blockquote {
  margin: 1em;
  padding: .5em;
  background: #ddd;
}
`],
        currentMarkdown: '',
        fullMarkdown: `黄珂
====
技能
====

数据库设计
----
  - 结构化数据库设计
  - noSQL设计

  
后端开发
----
  - 用户管理
  - 单点登录
  - 第三方登录
  - 权限管理
  - 交易系统
  - 支付系统
  - 社区系统
  - 博客系统
  - 公众号开发
  - 小程序开发
  - API接口开发

前端开发
----
  - Web前端开发
  - 移动终端(Native App, Hybrid App)

产品设计
----
  - 智慧旅游项目
  - 运动健康云平台
  - 社区支持农业O2O项目
  - 省级环境监控平台
  - 高速公路异地处罚系统
  - 环保局办公自动化系统
  - 保险公司数据迁移项目
  - 啤酒厂供应链项目
  - 货运代理系统
  - 集装箱管理系统
  - 滞期费管理项目

技术及语言
----
  - Java: SpringMVC, SpringCloud, Hibernate, iBatis, spark, sql2o, HikariCP, freemarker, okHttp, retrofit, RxJava
  - Kotlin: ktor, exposed, anko
  - Node.js: express, angular, ionic, react, cordova, meteor, electron, axios
  - Swift: Vapor, ReactiveSwift
  - Golang: hugo, beego, gorm, sqlx, matcha
  - Python: tushare, pandas, numpy, matplotlib
  - DotNet and PHP
  - DB: SQLServer, Oracle, MySQL/MariaDB, MongoDB, graphQL, redis, memcached
  - WebServer: apache, nginx, tomcat, netty, jetty
  - OS: Ubuntu, CentOS, MacOS, Windows
  - Others: Docker, git, Xmind，Axure

以上我都不会
----
但是想学。

勾引方式
----
* QQ：1650713339
<img src ="static/null311cbdae174255c.jpg" style="width:280px;height:350px"></img>

`, thanksMarkdown: `
最后
----

* 大一就要结束了，学校没教多少干货，但是感觉自己学到的还挺多的，每天都过得很充实————打打游戏，吹吹逼，简单而精彩。
* 我是一个有梦想的有志青年，一直想找一个志同道合的人一起上分，王者大师什么的。有人问我每天起早贪黑这么累干嘛，给自己一点时间放松不行吗，我只想说，这不是游戏，是梦想。
* 最近想学盗号，改银行卡密码，挺难的。百度老师也不是很会，但我暑假一定学会，到时候想学的可以来问我，只限胸大腿长的女同学，保证教得明明白白。
* 我还会爬妹子图，爬各种资源。本来想组一个团队把资源打包，上传到百度网盘，然后靠招收代理，卖百度网盘创业发家致富的，但是不知道为什么没人肯加入我。
* 这一年其实挺快的不是吗。离开了一些人，但也认识了一些人，把那一群人的吹逼技巧教给了这一群人，那边认识了太久的那种腻的感觉升华成了淡淡的思念，这边初识的惊艳与窃喜也远去变成了如今的嫌弃与默契。我们得慢下来。
* 有什么好玩的都可以来滴滴我交流学习。
`
      }
    },
    created() {
      this.makeResume()
    },

    methods: {
      makeResume: async function () {
        await this.progressivelyShowStyle(0)
        await this.progressivelyShowResume()
        await this.progressivelyShowStyle(1)
        await this.showHtml()
        await this.progressivelyShowStyle(2)
      },
      showHtml: function () {
        return new Promise((resolve, reject) => {
          this.enableHtml = true
          this.$nextTick(() => {
            this.$refs.resumeEditor.goTop()
          })
          resolve()
        })
      },
      progressivelyShowStyle(n) {
        return new Promise((resolve, reject) => {
          let interval = this.interval
          let showStyle = (async function () {
            let style = this.fullStyle[n]
            if (!style) { return }
            // 计算前 n 个 style 的字符总数
            let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0)
            let prefixLength = length - style.length
            if (this.currentStyle.length < length) {
              let l = this.currentStyle.length - prefixLength
              let char = style.substring(l, l + 1) || ' '
              this.currentStyle += char
              if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
                this.$nextTick(() => {
                  this.$refs.styleEditor.goBottom()
                })
              }
              setTimeout(showStyle, interval)
            } else {
              resolve()
            }
          }).bind(this)
          showStyle()
        })
      },
      progressivelyShowResume() {
        return new Promise((resolve, reject) => {
          let length = this.fullMarkdown.length
          let interval = this.interval
          let showResume = () => {
            if (this.currentMarkdown.length < length) {
              this.currentMarkdown = this.fullMarkdown.substring(0, this.currentMarkdown.length + 1)
              let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
              let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
              if (prevChar === '\n' && this.$refs.resumeEditor) {
                this.$nextTick(() => this.$refs.resumeEditor.goBottom())
              }
              setTimeout(showResume, interval)
            } else {
              resolve()
            }
          }
          showResume()
        })
      }
    }
  }

</script>

<style scoped>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    min-height: 100vh; position: relative;
  }

  html {
    min-height: 100vh;
  }
  *{
    box-sizing: border-box;
  }

</style>
