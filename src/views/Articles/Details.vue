<template>
  <div class="details">
    <Header :light-index="1"></Header>
    <div class="toc-fixed">
<!--      <div class="action" :class="toc.length > 0?'':'noContents'">-->
<!--        <mu-tooltip placement="top" content="点赞">-->
<!--          <mu-button fab color="primary">-->
<!--            <mu-icon value="thumb_up"></mu-icon>-->
<!--          </mu-button>-->
<!--        </mu-tooltip>-->
<!--        <mu-tooltip placement="top" content="收藏">-->
<!--          <mu-button fab color="purple500">-->
<!--            <mu-icon value="grade"></mu-icon>-->
<!--          </mu-button>-->
<!--        </mu-tooltip>-->
<!--        <mu-tooltip placement="top" content="评论">-->
<!--          <mu-button fab color="red">-->
<!--            <mu-icon value="chat"></mu-icon>-->
<!--          </mu-button>-->
<!--        </mu-tooltip>-->
<!--      </div>-->
    </div>
    <div class="content">
      <div class="right">
        <RightConfig></RightConfig>
      </div>
      <div class="left">
        <div class="left-box">
          <mu-card class="card">
            <mu-card-title :title="info.title" :sub-title="info.introduction"></mu-card-title>
            <mu-card-media>
              <img v-lazy="info.cover" style="height: 100%;" alt="">
            </mu-card-media>
            <mu-card-actions class="sub-title">
              <mu-button class="cursor-default" flat color="warning">字数（1000）</mu-button>
              <mu-button class="cursor-default" flat color="secondary">阅读大约2分钟</mu-button>
              <mu-button class="cursor-default" flat color="info">查看（1000）</mu-button>
              <mu-button class="cursor-default" flat color="error">评论（1000）</mu-button>
              <mu-button class="cursor-default" flat color="primary">点赞（1000）</mu-button>
              <mu-button class="cursor-default" flat color="#9e9e9e">2022-08-22 20:30</mu-button>
            </mu-card-actions>
            <mavon-editor
              v-model="content"
              defaultOpen="preview"
              :toolbarsFlag="false"
              :subfield="false"
              :navigation="true"
              codeStyle="tomorrow-night-eighties" />
            <mu-card-actions>
              <mu-button class="cursor-default" flat color="primary">
                <mu-icon left value="dns"></mu-icon>分类
              </mu-button>
              <mu-button class="cursor-default" flat>
                <mu-icon left value="loyalty"></mu-icon>Vue
              </mu-button>
              <mu-button class="cursor-default" flat>
                <mu-icon left value="loyalty"></mu-icon>React
              </mu-button>
            </mu-card-actions>
          </mu-card>
          <div class="action-list">
            <mu-tooltip placement="top" content="点赞">
              <mu-button fab color="primary">
                <mu-icon value="thumb_up"></mu-icon>
              </mu-button>
            </mu-tooltip>
            <mu-tooltip placement="top" content="收藏">
              <mu-button fab color="purple500">
                <mu-icon value="grade"></mu-icon>
              </mu-button>
            </mu-tooltip>
            <mu-tooltip placement="top" content="评论">
              <mu-button fab color="red">
                <mu-icon value="chat"></mu-icon>
              </mu-button>
            </mu-tooltip>
          </div>
<!--          评论-->
          <mu-card class="card" id="comment">
            <Comment @comment="comment" :commentSuccess="commentSuccess"></Comment>
          </mu-card>
<!--          评论列表-->
          <mu-card v-if="commentList.length > 0" class="card">
            <mu-card-title title="评论（3）"></mu-card-title>
            <mu-divider></mu-divider>
            <CommentList :list="commentList" :articleId="info._id" :articleTitle="info.title"></CommentList>
          </mu-card>
          <prev-next :prev="prev" :next="next"></prev-next>
        </div>
      </div>
    </div>
    <Footer></Footer>
  </div>
</template>

<script>
import Header from "@/components/Header";
import Footer from "@/components/Footer";
import RightConfig from "@/components/RightConfig";
import Comment from "@/components/Comment";
import CommentList from "@/components/CommentList"
import PrevNext from "@/components/PrevNext";
import {markdown} from "@/utils/markdown";
import Clipboard from 'clipboard'

export default {
  name: "articlesDetails",
  components: {
    Header,
    Footer,
    RightConfig,
    Comment,
    CommentList,
    PrevNext,
  },
  data(){
    return {
      info:{
        _id:'1',
        title:'使用jspdf+canvas2html将网页保存为pdf文件',
        introduction:'1234567890',
        cover:require('@/assets/photo.png')
      },
      toc:[],
      content:'#### how to use mavonEditor in nuxt.js',
      commentSuccess: false,
      commentList: [
        {
          targetReplayId: "6084ce48e268db458626591a",
          targetReplayContent: "good",
          currentReplayContent: "这篇文章写得不错",
          commentTime: 1623048202,
          auditTime: 0,
          auditStatus: "3",
          _id: "60bdc00ac4b76ef12cd151aa",
          avatar: "http://www.nevergiveupt.top/user_avatar.png",
          email: "13412345678@163.com",
          nickName: "Never",
          articleId: "601134b4c4ae0128013d322d",
          articleTitle: "测试评论文章",
        },
        {
          targetReplayId: "",
          targetReplayContent: "",
          currentReplayContent: "good",
          commentTime: 1619316296,
          auditTime: 1619316309,
          auditStatus: "1",
          _id: "6084ce48e268db458626591a",
          avatar:
            "http://img.nevergiveupt.top/78e79747e0658b0d1766c8928d784653.png",
          email: "1916579055@qq.com",
          nickName: "永不放弃",
          articleId: "601134b4c4ae0128013d322d",
          articleTitle: "测试评论文章",
        },
        {
          targetReplayId: "",
          targetReplayContent: "",
          currentReplayContent: "好，不错",
          commentTime: 1611745373,
          auditTime: 1612108800,
          auditStatus: "1",
          _id: "6011485dc4ae0128013d3246",
          avatar:
            "http://img.nevergiveupt.top/78e79747e0658b0d1766c8928d784653.png",
          email: "1916579055@qq.com",
          nickName: "永不放弃",
          articleId: "601134b4c4ae0128013d322d",
          articleTitle: "测试评论文章",
        },
      ],
      prev: {
        categories: "技术",
        collect: 0,
        comment: 0,
        content:
          "### 1.toRefs↵把一个响应式对象转换成普通对象，该普通对象的每个 property 都是一个 ref↵↵`应用`: ",
        cover: "http://nevergiveupt.top/vue/vue_composition_api.jpeg",
        createTime: 1611739740,
        introduction:
          "toRefs把一个响应式对象转换成普通对象，该普通对象的每个 property 都是一个 ref ，和响应式对象 property 一一对应。",
        isCollect: true,
        isComment: true,
        isLike: true,
        isReward: false,
        like: 0,
        publishStatus: 1,
        sort: 0,
        status: 1,
        tags: ["Vue"],
        title: "Vue3.x-toRefs & shallowReactive & shallowRef & shallowReadonly",
        updateTime: 1611739813,
        views: 5,
        _id: "6011325cc4ae0128013d3210",
      },
      next: {
        categories: "技术",
        collect: 0,
        comment: 0,
        content:
          "### 1.注册GitHub账号并创建一个OAuth Apps↵↵登录GitHub账号然后右上角找到你的头像点击",
        cover: "http://nevergiveupt.top/egg/github_signin.png",
        createTime: 1612341189,
        introduction:
          "『登录鉴权』 是一个常见的业务场景，包括『账号密码登录方式』和『第三方统一登录』。其中，后者我们经常使用到，如 Google， GitHub，QQ 统一登录，它们都是基于 OAuth 规范。",
        isCollect: true,
        isComment: true,
        isLike: true,
        isReward: true,
        like: 1,
        publishStatus: 1,
        sort: 0,
        status: 1,
        tags: ["Node.js", "Egg"],
        title: "使用Egg通过GitHub来实现用户登录",
        updateTime: 1612341807,
        views: 6,
        _id: "601a5fc5e268db458626523d",
      },
    }
  },
  mounted() {
    this.content = markdown(this.mavonEditor, '在前端开发中， html 转 pdf 是最常见的需求，实现这块需求的开发[html2canvas](http://html2canvas.hertzen.com/)和 [jspdf](http://mozilla.github.io/pdf.js/getting_started/) 是最常用的两个插件，插件都是现成的。\\n### 1.安装\\n### 2.使用 \\n ```js \\n console.log(123); \\n```')
    this.$nextTick(()=>{
      let clipboard = new Clipboard('.copy-btn')
      clipboard.on('success',()=>{
        this.$toast.success('复制成功')
      })
      clipboard.on('error',()=>{
        this.$toast.success('复制失败')
      })
    })
    this.commentList = this.listToTree(this.commentList)
  },
  methods:{
    comment(data){
      console.log('评论数据',data)
      this.commentSuccess = true
    },
    listToTree(list) {
      let info = list.reduce(
        (map, node) => ((map[node._id] = node), (node.children = []), map),
        {}
      );
      return list.filter((node) => {
        info[node.targetReplayId] &&
        info[node.targetReplayId].children.push(node);
        return !node.targetReplayId;
      });
    },
  }
}
</script>

<style lang="less" scoped>
.details {
  padding-top: 64px;
}

.toc-fixed {
  position: fixed;
  width: 20%;
  right: 20px;
  top: 80px;
  .toc {
    width: 100%;
    max-height: 400px;
    overflow-y: auto;
    word-break: break-all; //强制英文单词断行
    padding: 0.2rem 0 0.2rem 0.2rem;
    .title {
      font-size: 0.4rem;
      margin-bottom: 10px;
    }
    a {
      display: inline-block;
      color: #2196f3;
      font-size: 0.32rem;
      cursor: pointer;
      padding: 5px 0;
      &:hover {
        color: #00e676;
      }
    }
  }
}

.action-list {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin: 0.42667rem 0;
}
.action {
  margin-top: 0.42667rem;
  display: flex;
  justify-content: space-around;
}
.noContents {
  flex-direction: column;
  align-items: center;
  height: 400px;
}

.content {
  padding-bottom: 0.53333rem;
  display: flex;
  .left {
    flex: 9;
    margin-top: 16px;
    .card {
      border-radius: 5px;
      margin-bottom: 0.48rem;
      .article-detail {
        width: 100%;
        padding: 0.42667rem 0.42667rem 0.42667rem 0.69333rem;
        box-sizing: border-box;
        word-break: break-all;
      }
      .sub-title {
        display: flex;
        flex-wrap: wrap;
      }
      .text {
        padding: 0 0.42667rem;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 3;
      }
      .chip {
        margin-right: 0.26667rem;
      }
      .cover {
        flex: 1;
        border-radius: 0;
        padding: 0.42667rem;
        .cover-img {
          object-fit: cover;
          width: 100%;
          height: 4.26667rem;
          vertical-align: middle;
        }
      }
      .card-box {
        flex: 2;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
      }
    }
  }
  .right {
    flex: 3;
    display: flex;
    justify-content: center;
  }
}
@media screen and (min-width: 750px) {
  .cover{
    display: block;
  }
  .left{
    margin-top: 16px;
  }
  .right{
    display: block;
  }
  .toc-fixed{
    display: block;
  }
  .left-box{
    width: 70%;
  }
}
@media screen and (max-width: 750px) {
  .cover{
    display: none;
  }
  .left{
    margin-top: 0;
  }
  .right{
    display: none;
  }
  .toc-fixed{
    display: none;
  }
  .left-box{
    width: 100%;
  }
}
</style>