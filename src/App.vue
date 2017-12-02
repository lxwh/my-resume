<template>
  <div id="app">
    <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
    <ResumeEditor ref="resumeEditor" :markdown="currentMarkdown" :enableHtml="enableHtml"></ResumeEditor>
    <div>
      <div class="languageBtns">
        <button>中文</button>
        <button>英文</button>
      </div>
    </div>
  </div>
</template>

<script>
import StyleEditor from "./components/StyleEditor";
import ResumeEditor from "./components/ResumeEditor";
import "./assets/reset.css";

export default {
  name: "app",
  components: {
    StyleEditor,
    ResumeEditor
  },
  data() {
    return {
      interval: 10,
      currentStyle: "",
      enableHtml: false,
      fullStyle: [
        `/*
* 大家好，我是马贵龙，现就职于北京一家互联网公司，担任高级前端开发工程师一职。
* 让我们来点实际的，看看我能做些什么吧!!!
*/

/* 
* 让我们开始吧,我们先来让所有的变化都以动画的效果来显示
* 首先给所有元素加上过渡效果 
*/
* {
  transition: all .5s;
}
/* 
* 看起来似乎没什么变化，不要着急，接下来你就会看到
* 白色背景太单调了，而且也厌倦了白底黑字 
* 稍等，那就让我们来改变一下背景颜色与字体的颜色
*/
html {
  background: rgb(63, 82, 99);
  color: rgb(222,222,222); 
}

/* 文字离边框太近了,那就改一下呗*/
.styleEditor {
  padding: .5em;
  border: 1px solid;
  margin: .5em;
  overflow: auto;
  width: 49%; 
  height: 95vh;
}
/* 让代码高亮起来吧 */
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

/* 要不我们加点 3D 效果呗 */
html{
  perspective: 1000px;
}
.styleEditor {
  position: absolute;
  left: 0;
  top: 0;
  color: white;
  overflow: auto;
  background: rgb(48, 48, 48);
  border: 1px solid rgb(204, 204, 204);
  font-size: 13px;
  font-family: monospace;
  padding: 10px 10px 20px;
  box-shadow: rgba(0, 0, 0, 0.3) -4px 4px 2px 0px;
  white-space: pre-wrap;
  outline: 0px;
}

/*
* 现在看起来好多了
* 在这整个页面里写代码让人有些不舒服
*/
/* 接下来我给自己准备一个编辑器 */
.resumeEditor{
  position: absolute; 
  right: 0; 
  top: 0;
  padding: .5em;  
  margin: .5em;
  width: 49%; 
  height: 95vh;
  overflow: auto;
  background: rgb(48, 48, 48);
  border: 1px solid #ccc;
  font-size: 14px;
  font-family: monospace;
  box-shadow: -4px 4px 2px 0 rgba(0,0,0,0.3);
  white-space: pre-wrap;
  outline: 0;
}
/* 
* 现在我们差不多可以开始写我的简历了
* 我相信你来这不只是为了看这些漂亮的颜色
* 好了，我开始写简历了 
*/


`,
        `
/* 这个简历好像差点什么
 * 对了，这是 Markdown 格式的，我需要变成对 HR 更友好的格式
 * 简单，用开源工具翻译成 HTML 就行了
 */
`,
        `
/* 再对 HTML 加点样式 */
.resumeEditor{
  padding: 2em;
}
.resumeEditor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
}
.resumeEditor ul,
.resumeEditor ol{
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
/* 是不是感觉不够牛逼
* OK，现在让我们来换一下位置
* 让我们准备来再做一些改变
*/
.styleEditor{
  transform: translateX(102%);
}
.resumeEditor{
  transform: translateX(-102%);
}
/*
* 我们再来点效果吧
*/
.styleEditor {
  transform: translateX(98%) rotateY(-10deg);
  transform-origin: right center 0px;
  width: 50%;
}
.resumeEditor{
  transform: translateX(-98%) rotateY(10deg);
  transform-origin: left center 0px;
  width: 50%;
}
/*
* 最后非常感谢您愿意花3分钟看完我的简历！
* 如果你喜欢这个效果，Fork [我的项目](https://github.com/lxwh/my-resume)，打造你自己的简历！
*/
`
      ],
      currentMarkdown: "",
      fullMarkdown: `# 个人简历

基本信息
----

姓 名：马贵龙 
性 别：男
电 话：135-2265-4025 
邮 箱：mgl4025@163.com
博 客：www.github.com/lxwh/MyBlog 
住 址：北京市昌平区

求职意向
----

求职岗位：高级WEB 前端开发工程师 
期望薪资：面议
求职状态：在职，正在交接工作 
工作地点：北京

专业技能
----

* 精通 DIV+CSS、弹性盒 Flex 的页面布局、重构，能根据设计图,100%还原成 web 页面；
* 精通 HTML5、CSS3，掌握基于 HTML5 的 webApp 开发及移动端适配，熟练使用 CSS3 动画；
* 熟悉主流浏览器的兼容性以及相应的适配技术，完成页面的完整兼容和自适应；
* 熟悉 W3C 标准，明确 HTML 语义化，熟练进行前端性能优化设计；
* 熟悉 ECMA 标准，熟练掌握 DOM、BOM 操作，熟悉闭包原理、面向对象思想，理解原型、原型链的继承机制；
* 精通 AJAX、JSON，实现数据的按需交互，提高用户体验度；熟悉跨域、正则表达式；
* 精通 JavaScript，以及其类库 jQuery、Zepto 等，实现 DOM 操作及事件处理，熟悉动态绑定事件的原理，掌握各种动画的处理；
* 精通响应式 JS 框架 Bootstrap，熟悉其栅格系统原理，熟练使用各种组件；
* 熟练使用 Vue.js、Angular.js；熟悉 MV*模式，熟悉数据双向绑定原理，以及组件化、模块化；了解 React、Require.js 等主流框架；
* 掌握 CSS 类库 Sass、Less 技术；熟练使用 Grunt、Gulp 等前端构建化工具；熟练使用webpack，babel 等编译工具；
* 熟练使用 npm ，node 包管理工具和 Bower 前端包管理工具，熟练使用 Git、SVN，对代码进行管理、更新迭代与版本控制；
* 熟练使用 Linux 环境下常用命令，熟悉微信小程序开发流程，阅读过 API 文档；
* 熟练使用 HBuilder、VS Code、webstorm、Sublime、Eclipse、Photoshop 等开发工具；
* 了解 PHP、JAVA、Python、MySql 等后端服务器语言。

教育背景
----

2017.4-至今 中央财经大学 项目管理 本科

2011.9-2014.7 北京理工大学 电子信息科学与技术 专科

工作经历
----

**2017.4-至今  5人团队**

**联合永道（外派中国移动）   产品事业部   WEB前端开发工程师**

**工作职责：**

1、制定前端开发规范，负责前端的代码结构和实施；
2、负责公司前端产品的组件的设计、规划及开发，独立承担一个系统的前端开发任务；
3、负责前端 JS 的开发与调试，同时负责前端 HTML、CSS、JS 的性能调优；
4、负责前端的维护、优化、重构，并编写相应的技术文档。

**2016.9-2017.4  4人团队**

**科锐国际（外派联想）   研发部   WEB前端开发工程师**

**工作职责：**

1、负责项目的前端开发工作，实现高性能的交互、DOM 操作、数据渲染等；
2、根据产品需求，提供解决方案、编写基础控件、业务组件等；
3、与产品 UI 设计团队及需求工程师密切沟通和紧密配合，了解用户体验，实现设计师的设计想法；
4、运用 Photoshop 进行切图，HTML+CSS 进行静态页面的搭建精准还原设计稿；
5、与后端开发团队紧密配合，实现代码规范及良好的交互。

**2014.08-2016.09   3人团队**

**北京智业联创科技有限公司   研发部   WEB前端开发工程师**

**工作职责：**

1、根据工作安排完成代码编写，确保代码符合 W3C 规范；
2、负责公司现有项目和新项目的前端修改调试和开发工作；
3、与设计团队紧密合作，实现设计师的设计想法，使得项目的用户体验高；
4、与后端开发团队紧密配合，确保代码有效对接，优化网站前端性能；
5、整个网站页面通过标准校验，兼容各大主流浏览器；对项目进行测试、维护与更新。


项目经验
----

**项目名称：中国移动 MOA（移动端）**

**项目描述：**

该项目是集所有办公一体化的掌上 APP，轻松实现日常办公，提高办公效率，方便管理日常的工作任务，随时随地沟通、随时随地办公。

**项目职责：**

1、使用 XPATH 语言语法解析 DOM，获取 DOM 数据；
2、使用 Chalers 网络抓包工具，对 PC 网站数据进行抓取，实现数据的监控与调整，方便移动端开发；
3、使用 flex 弹性盒对各个页面布局，实现各个设备的适配；
4、使用原生 JS、Zepto.js 实现业务逻辑及组件的封装；
5、实现列表页的下拉刷新，上拉加载更多、筛选等功能的实现，并组件化，实现可复用性；

**项目名称：ThinkStack 存储管理（PC 端）**

**项目描述：**

ThinkStack 存储管理是关于云方向的系统硬件管理类项目，主要是为用户提供资源管理、 信息储存存储、硬盘内存管理、数据库管理等。

**项目职责：**

1、使用 Bootstrap 进行布局及静态页面的开发；
2、使用 css3 中 transform、transition 实现页面中的动画特效；
3、运用 AngularJs 实现增删改查、tab 切换、分页效果及表单验证等；
4、封装 Ajax 接口，编写业务组件以及处理 JSON 数据页面渲染；
5、使用 Git、GitBash、Xshell、WinSCP 对代码进行压缩、打包以及上传等。

**项目名称：客利客（PC 端）**

**项目描述：**

客利客，是目前中国最大的专业性保险商品网络超市，是“线上互动服务”与“现场个性 服务”相结合、“丰富商品集合”与“便捷下单购买”相结合、“金融商品选购”与“全程服务代办”相结合的专业的金融保险电子商务平台。

**项目职责：**

1、负责首页的静态页面布局，使用 Ajax 技术完成 JSON 数据的异步请求和页面数据渲染；
2、协同后台人员实现产品界面与各功能接口调试和开发，实现界面动态交互；
3、使用原生态 JS 代码和 jQuery 实现 banner 轮播效果和滚动条监听效果；
4、使用 CSS3 的 transition 和 scale,实现鼠标经过图片时动态放大缩小效果；
5、负责整个项目 CSS、JS 代码优化，解决主流浏览器兼容性问题。

**项目名称：独书在线（webApp）**

**项目描述：**

独书在线是一款功能强大的客户端阅读神器，引领人文阅读、严肃阅读趣味,提供上万部 优质文史、文学、艺术、思想、财经、职场、生活等诸多图书的在线阅读，极大的满足了爱好阅读的 用户的需求。

**项目职责：**

1、使用核心的 touch 事件，实现移动触屏滑动的效果即图片的轮播效果；
2、运用 flex 布局和@media 技术，实现响应式，从而适配不同的设备；
3、使用 CSS3 中 keyframes 来创建动画,逐步改变每个 CSS 样式；
4、数据渲染采用 JSONP 形式从服务器请求数据，应用 localstorage 将数据进行长期存储，避免用户无网络或退出时数据遗失；
5、使用 jQuery 实现章节切换功能，当点击上一章或下一章，完成翻页效果等。

**项目名称：云购（PC 端与移动端）**

**项目描述：**

云购是一个商城网站,用户只要注册登录就有机会获得一件奖品。主要负责实现注册页面、 登录页面、页面切换以及详情页的渲染等业务要求。

**PC 端技术要点：**

1、登录注册页面主要使用到的是正则表达式判断数据格式及 Ajax 的异步请求；
2、运用 div+css 完成静态页面布局，运用 CSS3 和 jQuery 进行数据渲染；
3、运用 Ajax+json+jsp 把需要加载的 dom 元素进行组织，动态加载网页元素；
4、运用 chrome-network 调试工具进行调试、抓包，bug 修复及维护；
5、使用 jQuery 和原生态 JS 代码实现 banner 效果、tab 切换效果和详情页，放大镜效果。

**移动端技术要点：**

1、使用 touchstart 事件代替 onclick 事件，onclick 会有 300ms 延迟，点击时会出现阴影，对用 户体验不好；
2、基于 HTML 5 和 CSS 3 相关技术进行多终端适配开发；
3、运用 Swiper 移动框架进行页面轮播效果的实现；
4、运用 Zepto.js 轻量级的 jq 框架实现页面的各种特效。

**项目名称：农交网（PC 端）**

**项目描述：**

农交网是一个专业的综合网上购物商城，在线销售家电、数码通讯、电脑、家居百货、服 装服饰、母婴、图书、在线旅游等数万个品牌千万种优质商品。便捷、诚信的服务，为您提供愉悦的 网上商城购物体验!

**项目职责：**

1、主要负责整个网站静态页面布局，使用 JavaScript 和 jQuery 实现动态效果；
2、使用 HTML5+CSS3 新特性，使页面更绚丽，提高用户体验度；
3、对网站进行模块化功能设计，优化代码，调试各种浏览器兼容问题；
4、与后端开发人员合作实现项目测试和上线。

**项目名称：每日优鲜（webApp）**

**项目描述：**

每日优鲜，专注优质生鲜的移动电商，完成在水果，肉类，水产，蔬菜，乳品，饮品，零食等品类的布局，为全国数百万客户提供 2 小时送货上门的极速配送服务。

**责任描述：**

1、主要负责移动端网页制作；
2、参与移动产品和项目制作，配合后台开发人员实现产品前端界面效果功能；
3、负责移动产品 HTML5、CSS3 的编写，AJAX 数据交互，解决移动端适配问题；
4、实现产品 UI 和交互方面的开发需求，确保产品具有优质的用户使用体验。

**主要技术：**

1、使用 jQuery 库进行动态事件绑定；
2、使用 touchslide.js 函数库实现手机端的 banner 轮播图效果；
3、使用 animate.min.css 运动框架实现图片运动效果；
4、使用 iScroll.js 实现页面的流畅滑动；
5、利用 zoom.js 实现手机的适屏问题；

其他项目
----


**项目名称：个人动态简历**

**技术要点：**

主要应用 Vue2 与 ES6 实现数据与页面结构分离，动态加载页面相关数据信息，另外采用 CSS3 及 HTML5 相关特性，实现动画效果，使用 webpack、Gulp 和 Git 进行代码压缩、打包及上传。

源码：https://github.com/lxwh/my-resume/

**项目名称：微信小程序 Demo**

**技术要点：**

主要应用 ES6 相关语法及小程序的开发流程和相关技术，实现仿豆瓣电影、仿 QQ音乐、仿微票小程序等。

源码：https://github.com/lxwh/WechatApp/

**项目名称：个人 Github 博客**

技术要点：使用 Markdown 实现的个人博客博文，及相关技术类文章。

源码：https://github.com/lxwh/MyBlog/

自我评价
----

1、对前端技术具有浓厚的兴趣，喜欢逛 CSDN，博客园等技术论坛；
2、具有良好的沟通能力、业务逻辑分析能力、解决问题能力强；
3、对前端编程有自己独到的见解，擅长使用原生态+JQ 的方式实现前端功能；
4、学习能力强，有了面向对象的编程思想后，能融会贯通其它的编程语言；
5、主攻前端开发，另涉及 Java、Python、Node、PHP、MySQL 等后台开发。
 
链接
----

1. [GitHub](https://github.com/lxwh/)
2. [我的博客](https://lxwh/github.io/MyBlog/)

`
    };
  },
  created() {
    this.makeResume();
  },

  methods: {
    makeResume: async function() {
      await this.progressivelyShowStyle(0);
      await this.progressivelyShowResume();
      await this.progressivelyShowStyle(1);
      await this.showHtml();
      await this.progressivelyShowStyle(2);
    },
    showHtml: function() {
      return new Promise((resolve, reject) => {
        this.enableHtml = true;
        resolve();
      });
    },
    progressivelyShowStyle(n) {
      return new Promise((resolve, reject) => {
        let interval = this.interval;
        let showStyle = async function() {
          let style = this.fullStyle[n];
          if (!style) {
            return;
          }
          // 计算前 n 个 style 的字符总数
          let length = this.fullStyle
            .filter((_, index) => index <= n)
            .map(item => item.length)
            .reduce((p, c) => p + c, 0);
          let prefixLength = length - style.length;
          if (this.currentStyle.length < length) {
            let l = this.currentStyle.length - prefixLength;
            let char = style.substring(l, l + 1) || " ";
            this.currentStyle += char;
            if (style.substring(l - 1, l) === "\n" && this.$refs.styleEditor) {
              this.$nextTick(() => {
                this.$refs.styleEditor.goBottom();
              });
            }
            setTimeout(showStyle, interval);
          } else {
            resolve();
          }
        }.bind(this);
        showStyle();
      });
    },
    progressivelyShowResume() {
      return new Promise((resolve, reject) => {
        let length = this.fullMarkdown.length;
        let interval = this.interval;
        let showResume = () => {
          if (this.currentMarkdown.length < length) {
            this.currentMarkdown = this.fullMarkdown.substring(
              0,
              this.currentMarkdown.length + 1
            );
            let lastChar = this.currentMarkdown[
              this.currentMarkdown.length - 1
            ];
            let prevChar = this.currentMarkdown[
              this.currentMarkdown.length - 2
            ];
            if (prevChar === "\n" && this.$refs.resumeEditor) {
              this.$nextTick(() => this.$refs.resumeEditor.goBottom());
            }
            setTimeout(showResume, interval);
          } else {
            resolve();
          }
        };
        showResume();
      });
    }
  }
};
</script>

<style scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

html {
  min-height: 100vh;
}
* {
  box-sizing: border-box;
}
.languageBtns {
  width: 100vm;
  position: fixed;
  bottom: 40px;
  left: 0;
  right: 0;
  display: flex;
}
.languageBtns button {
  background: none;
  border: none;
  color: #fff;
  justify-content: center;
}
</style>
