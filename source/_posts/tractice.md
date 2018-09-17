title: 华清实习
author: songxingguo
tags: []
categories:
  - 实习
  - ''
date: 2018-09-16 19:01:00
---
## 第一周

**学习大纲：**

- HTML 标签 
- CSS 样式
- DOM 操作

<!-- more -->

### 第一天

- #### 笔记

  ![第一天-01](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-01.png)
  ![第一天-02](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-02.png)
  ![第一天-03](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-03.png)
  ![第一天-04](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-04.png)
  ![第一天-05](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-05.png)
  ![第一天-06](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-06.png)
  ![第一天-07](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-07.png)
  ![第一天-08](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-08.png)
  ![第一天-09](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-09.png)
  ![第一天-10](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-10.png)
  ![第一天-11](http://p9myzkds7.bkt.clouddn.com/tractice-huaqing/1/%E7%AC%AC%E4%B8%80%E5%A4%A9-11.png)

- #### 练习

  ##### [HTML 基础](https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/HTML%E5%9F%BA%E7%A1%80.html)
  <iframe src="https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/HTML%E5%9F%BA%E7%A1%80.html"  width="100%" height="300" frameborder="0" align="middle" ></iframe>

  **代码：**

  ```html
  <!DOCTYPE html>
  <html>
      <head>
          <meta charset="UTF-8">
          <title></title>

          <style  type="text/css">
            div {
              font-size: 14px; 
              color: red; 
              width: 40px; 
              height: 50px; 
              background-color: green;
            }
          </style>
      </head>
      <body>
          <!--
              作者：songxingguo
              时间：2018-09-10
              描述  hn 设置 n 级标题 
          -->

          <!-- 块级元素：独占一行，可设宽高 -->

          <div> 行内元素 h1~h6、div、p、ul、ol、li</div>

          这是标题标签：
          <h1>这是标题一</h1>
          <h2>这是标题二</h2>
          <h3>这是标题三</h3>

          <div>这是块标记</div>

          <p>段落标记</p>

          无序列表：
          <ul>
              <li>列表项1</li>
              <li>列表项2</li>
              <li>列表项3</li>
          </ul>

          有序列表：
          <ol>
              <li>列表项1</li>
              <li>列表项2</li>
              <li>列表项3</li>
          </ol>

          <span>天气</span>
          <span>真凉</span>

          <br />
          <span>真凉</span>

          <strong>加粗的字体</strong>

          <a href="https://www.baidu.com">点击 链接进行跳转</a>

          <br />

          <img src="./img/picture2.jpg" />

          <div  style="font-size: 14px; color: red; width: 40px; height: 50px; background-color: green;">这是内联样式</div>

          <br />

          <div>这是内部样式</div>
      </body>
  </html>
  ```
  
### 第二天

   - #### 练习
   
     ##### [第一个例子](https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/example.html)
     
     <iframe src="https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/example.html"  width="100%" height="300" frameborder="0" align="middle" ></iframe>
     
    **代码：**
    
    ```html
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8">
            <title>第一个例子</title>
            <style type="text/css">

               #root {
                 width: 70%;
               }
               .outer {
                  width: 100%;
                  margin-bottom: 5px;
               }
               .title {
                  display: inline-block;
                  font-size: 18px;
                  color: #FFFFFF;
                  margin-left: 20px;
               }
               .content {
                 display: inline-block;
                 margin-left: 200px;
               }
               .inner {
                  width: 100px;
                  height: 30px;
                  margin: 10px;
                  border: 2px solid #C0A4B5;
                  font-size: 14px;
                  text-align: center;
                  line-height: 30px;
                  color: #fff;
                  background-color: #AC6F93;
                  display: inline-block;
               }
               div.inner:hover {
                  border: 2px solid #fff;
               }
               #root div.outer:nth-child(1) {
                 background-color: #994576;
               }
               #root div.inner:nth-child(1) {
                 background-color: #AC6F93;
               }
               #root div.outer:nth-child(2) {
                 background-color: #CA365A;
               }
               #root div.inner:nth-child(2) {
                 background-color: #AC6F93;
               }
               #root div.outer:nth-child(3) {
                 background-color: #734E8B;
               }
               #root div.inner:nth-child(3) {
                 background-color: #AC6F93;
               }
               #root div.outer:nth-child(4) {
                 background-color: #5D6B90;
               }
               #root div.inner:nth-child(4) {
                 background-color: #AC6F93;
               }
            </style>
        </head>
        <body>
            <div id="root">
                <div class="outer">
                    <span class="title">爱逛</span>
                    <div class="content">
                        <div class="inner">型男毛呢</div>
                        <div class="inner">潮流女包</div>
                        <div class="inner">品质钟表</div>
                        <div class="inner">珠宝首饰</div>
                        <div class="inner">奢品特惠</div>
                    </div>
                </div>
                <div class="outer">
                    <span class="title">爱逛</span>
                    <div class="content">
                        <div class="inner">型男毛呢</div>
                        <div class="inner">潮流女包</div>
                        <div class="inner">品质钟表</div>
                        <div class="inner">珠宝首饰</div>
                        <div class="inner">奢品特惠</div>
                    </div>
                </div>
                <div class="outer">
                    <span class="title">爱逛</span>
                    <div class="content">
                        <div class="inner">型男毛呢</div>
                        <div class="inner">潮流女包</div>
                        <div class="inner">品质钟表</div>
                        <div class="inner">珠宝首饰</div>
                        <div class="inner">奢品特惠</div>
                    </div>
                </div>
                <div class="outer">
                    <span class="title">爱逛</span>
                    <div class="content">
                        <div class="inner">型男毛呢</div>
                        <div class="inner">潮流女包</div>
                        <div class="inner">品质钟表</div>
                        <div class="inner">珠宝首饰</div>
                        <div class="inner">奢品特惠</div>
                    </div>
                </div>
            </div>
        </body>
    </html>
    ```
   ##### [京东导航](https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/JD%E5%AF%BC%E8%88%AA.html)
   
    <iframe src="https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/JD%E5%AF%BC%E8%88%AA.html"  width="100%" height="300" frameborder="0" align="middle" ></iframe>

    **代码：**
    
    ```html
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8">
            <title>京东导航</title>
            <style type="text/css">
                * {
                    padding: 0;
                    margin: 0;
                }
                .nav {
                    border: 1px solid #D8D8D8;
                    width: 210px;
                    margin: 0 auto;
                    box-shadow: 0 -12px 10px rgba(0,0,0,.2);
                }
                img {
                    padding: 30px;
                }
                article {
                    text-align: center;
                    border-bottom: 1px solid #EDEDED;
                }
                nav {
                    margin: 10px 0;
                }
                ul {
                    list-style: none;
                }
                li{
                    height: 30px;	
                    line-height: 30px;
                    text-align: left;	
                    padding-left: 20px;
                    margin: 0 1px;
                }
                li:hover {
                    background-color: #D8D8D8;
                }
                a {
                    text-decoration: none;
                    font-size: 14px;
                    color: #626262;
                }
                a:hover {
                    color: #FD0045;
                }
                .menu_line {
                    font-size: 12px;
                }
            </style>
        </head>
        <body>
            <div id="root">
              <div class="nav">
                  <article>
                    <img src="../img/jd.jpg" width="150px" height="150px"/>
                  </article>
                  <nav>
                    <ul>
                        <li>
                           <a href="#">家用电器</a> 
                        </li>
                        <li>
                           <a href="#">手机</a>
                           <span class="menu_line">/</span>
                           <a href="#">运营商</a>
                           <span class="menu_line">/</span>
                           <a href="#">数码</a>
                        </li>
                        <li>
                           <a href="#">电脑</a>
                           <span class="menu_line">/</span>
                           <a href="#">办公</a>
                        </li>
                        <li>
                           <a href="#">家居</a>
                           <span class="menu_line">/</span>
                           <a href="#">家具</a>
                           <span class="menu_line">/</span>
                           <a href="#">家装</a>
                           <span class="menu_line">/</span>
                           <a href="#">厨具</a>
                        </li>
                        <li>
                           <a href="#">男装</a> 
                           <span class="menu_line">/</span>
                           <a href="#">女装</a>
                           <span class="menu_line">/</span>
                           <a href="#">童装</a>
                           <span class="menu_line">/</span>
                           <a href="#">内衣</a>
                        </li>
                        <li>
                           <a href="#">美妆</a>
                           <span class="menu_line">/</span>
                           <a href="#">个人清洁</a>
                           <span class="menu_line">/</span>
                           <a href="#">宠物</a>
                        </li>
                        <li>
                           <a href="#">女鞋</a> 
                           <span class="menu_line">/</span>
                           <a href="#">箱包</a> 
                           <span class="menu_line">/</span>
                           <a href="#">钟表</a>
                           <span class="menu_line">/</span>
                           <a href="#">珠宝</a>
                        </li>
                        <li>
                           <a href="#">男鞋</a>
                           <span class="menu_line">/</span>
                           <a href="#">运动</a>
                           <span class="menu_line">/</span>
                           <a href="#">户外</a>
                        </li>
                        <li>
                           <a href="#">房产</a>
                           <span class="menu_line">/</span>
                           <a href="#">汽车</a> 
                           <span class="menu_line">/</span>
                           <a href="#">汽车用品</a>
                        </li>
                        <li>
                           <a href="#">母婴</a>
                           <span class="menu_line">/</span>
                           <a href="#">玩具乐器</a>
                        </li>
                    </ul>
                  </nav>
              </div>
            </div>
        </body>
    </html>
    ```
   ##### [表单](https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E8%A1%A8%E5%8D%95.html)
  <iframe src="https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E8%A1%A8%E5%8D%95.html"  width="100%" height="300" frameborder="0" align="middle" ></iframe>
  
  **代码：**
  
  ```html
  <!DOCTYPE html>
  <html>
      <head>
          <meta charset="UTF-8">
          <title>表单</title>
          <style type="text/css">
              .inputArea {
                  margin: 10px 0;
                  font-size: 14px;
              }

              .inputArea label {
                  margin-right: 20px;
              }
              .text {
                  width: 150px;
                  height: 30px;
              }
              .select {
                  width: 80px;
                  height: 30px;
              }
              .textArea {
                  width: 200px;
                  height: 100px;
                  vertical-align: top;
              }
              .botton {
                  width: 100px;
                  height: 40px;
                  color: #fff;
                  background-color: #D753B8;
                  border: none;
                  text-align: center;
                  line-height: 40px;
                  font-size: 16px;
              }
              .ml50 {
                  margin-left: 50px;
              }
          </style>
      </head>
      <body>
          <div id="root">
              <form>
                  <div class="inputArea">
                      <label for="file">头像</label>
                      <input type="file" />
                  </div>
                  <div class="inputArea">
                      <label for="name">昵称</label>
                      <input type="text" class="text"/>
                  </div>
                  <div class="inputArea">
                      <label for="edu">学历</label>
                      <select class="select">
                          <option>一年级</option>
                          <option>二年级</option>
                          <option>三 年级</option>
                      </select>
                  </div>
                  <div class="inputArea">
                      <label for="sex">性别</label>
                      <input type="radio" name="sex" />男
                      <input type="radio" name="sex" />女
                  </div>
                  <div class="inputArea">
                      <label for="hobby">爱好</label>
                      <input type="checkbox" />篮球
                      <input type="checkbox" />音乐
                      <input type="checkbox" />钢琴
                      <input type="checkbox" />吉他
                  </div>
                  <div class="inputArea">
                      <label for="autograph">签名</label>
                      <textarea class="textArea"></textarea>
                  </div>

                  <input type="botton" class="botton ml50" value="保存">
              </form>
          </div>
      </body>
  </html>
  ```
  
### 第三天

-  #### 练习

  ##### [改变文字大小](https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E6%94%B9%E5%8F%98%E6%96%87%E5%AD%97%E5%A4%A7%E5%B0%8F.html)
  
  <iframe src="https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E6%94%B9%E5%8F%98%E6%96%87%E5%AD%97%E5%A4%A7%E5%B0%8F.html"  width="100%" height="300" frameborder="0" align="middle" ></iframe>
  
  **代码：**[改变文字大小源码](https://github.com/songxingguo/HuaQing/blob/master/HTML%E5%9F%BA%E7%A1%80/%E6%94%B9%E5%8F%98%E6%96%87%E5%AD%97%E5%A4%A7%E5%B0%8F.html)
  
  ##### [聊天窗口](https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E8%81%8A%E5%A4%A9%E7%AA%97%E5%8F%A3.html)
  
  <iframe src="https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E8%81%8A%E5%A4%A9%E7%AA%97%E5%8F%A3.html"  width="100%" height="300" frameborder="0" align="middle" ></iframe>
  
  **代码：**[聊天窗口源码](https://github.com/songxingguo/HuaQing/blob/master/HTML%E5%9F%BA%E7%A1%80/%E8%81%8A%E5%A4%A9%E7%AA%97%E5%8F%A3.html)
   
### 第四天

- #### 练习

  ##### [幻灯片](https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E5%B9%BB%E7%81%AF%E7%89%87.html)

    <iframe src="https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E5%B9%BB%E7%81%AF%E7%89%87.html"  width="100%" height="300" frameborder="0" align="middle" ></iframe>

    **代码：**[幻灯片源码](https://github.com/songxingguo/HuaQing/blob/master/HTML%E5%9F%BA%E7%A1%80/%E5%B9%BB%E7%81%AF%E7%89%87.html)
    
   ##### [猜图游戏](https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E7%8C%9C%E5%9B%BE%E6%B8%B8%E6%88%8F.html)
  
  <iframe src="https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E7%8C%9C%E5%9B%BE%E6%B8%B8%E6%88%8F.html"  width="100%" height="300" frameborder="0" align="middle" ></iframe>
  
  **代码：**[猜图游戏源码](https://github.com/songxingguo/HuaQing/blob/master/HTML%E5%9F%BA%E7%A1%80/%E7%8C%9C%E5%9B%BE%E6%B8%B8%E6%88%8F.html)
  
### 第五天

- #### 练习

  ##### [轮播图](https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E8%BD%AE%E6%92%AD%E5%9B%BE.html)

    <iframe src="https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E8%BD%AE%E6%92%AD%E5%9B%BE.html"  width="100%" height="300" frameborder="0" align="middle" ></iframe>

    **代码：**[轮播图源码](https://github.com/songxingguo/HuaQing/blob/master/HTML%E5%9F%BA%E7%A1%80/%E8%BD%AE%E6%92%AD%E5%9B%BE.html)

## 第二周

**学习大纲：**

  - CSS3 动画
  - Flex 布局
  - 小程序开发工具，布局知识
  - 动态数据绑定
  - LeanCloud
  
### 第一天

- #### 练习

  ##### [扭曲的导航](https://songxingguo.github.io/HuaQing/CSS3%E5%8A%A8%E7%94%BB/%E6%89%AD%E6%9B%B2%E7%9A%84%E5%AF%BC%E8%88%AA.html)

     <iframe src="https://songxingguo.github.io/HuaQing/CSS3%E5%8A%A8%E7%94%BB/%E6%89%AD%E6%9B%B2%E7%9A%84%E5%AF%BC%E8%88%AA.html"  width="100%" height="300" frameborder="0" align="middle" ></iframe>

     **代码：**[扭曲的导航源码](https://github.com/songxingguo/HuaQing/blob/master/CSS3%E5%8A%A8%E7%94%BB/%E6%89%AD%E6%9B%B2%E7%9A%84%E5%AF%BC%E8%88%AA.html)








