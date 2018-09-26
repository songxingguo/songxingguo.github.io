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

    <iframe src="https://songxingguo.github.io/HuaQing/HTML%E5%9F%BA%E7%A1%80/%E8%BD%AE%E6%92%AD%E5%9B%BE.html"  width="100%" height="400" frameborder="0" align="middle" ></iframe>

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

  ##### [倾斜的导航](https://songxingguo.github.io/HuaQing/CSS3%E5%8A%A8%E7%94%BB/%E5%80%BE%E6%96%9C%E7%9A%84%E5%AF%BC%E8%88%AA.html)

     <iframe src="https://songxingguo.github.io/HuaQing/CSS3%E5%8A%A8%E7%94%BB/%E5%80%BE%E6%96%9C%E7%9A%84%E5%AF%BC%E8%88%AA.html"  width="100%" height="100" frameborder="0" align="middle" ></iframe>

     **代码：**[倾斜的导航源码](https://github.com/songxingguo/HuaQing/blob/master/CSS3%E5%8A%A8%E7%94%BB/%E5%80%BE%E6%96%9C%E7%9A%84%E5%AF%BC%E8%88%AA.html)
  
  ##### [风车练习](https://songxingguo.github.io/HuaQing/CSS3%E5%8A%A8%E7%94%BB/%E9%A3%8E%E8%BD%A6%E7%BB%83%E4%B9%A0.html)

     <iframe src="https://songxingguo.github.io/HuaQing/CSS3%E5%8A%A8%E7%94%BB/%E9%A3%8E%E8%BD%A6%E7%BB%83%E4%B9%A0.html"  width="100%" height="400" frameborder="0" align="middle" ></iframe>

     **代码：**[风车练习源码](https://github.com/songxingguo/HuaQing/blob/master/CSS3%E5%8A%A8%E7%94%BB/%E9%A3%8E%E8%BD%A6%E7%BB%83%E4%B9%A0.html)
  
  ##### [时钟练习](https://songxingguo.github.io/HuaQing/CSS3%E5%8A%A8%E7%94%BB/%E6%97%B6%E9%92%9F%E7%BB%83%E4%B9%A0.html)

     <iframe src="https://songxingguo.github.io/HuaQing/CSS3%E5%8A%A8%E7%94%BB/%E6%97%B6%E9%92%9F%E7%BB%83%E4%B9%A0.html"  width="100%" height="600" frameborder="0" align="middle" ></iframe>

     **代码：**[时钟练习源码](https://github.com/songxingguo/HuaQing/blob/master/CSS3%E5%8A%A8%E7%94%BB/%E6%97%B6%E9%92%9F%E7%BB%83%E4%B9%A0.html)
  
### 第二天

- #### 


### 第三天

- ####


### 第四天

- #### 学习总结

 ##### inline-block、float 和 Flex 的优劣

 inline-block、float 和 Flex 都可以进行水平排列元素，inline-block 会存在元素之间有间隙，float 属性则会脱离文档流，使得布局改变，并且需要清除浮动，Flex 是最容易实现的一种方式，也是最好的，但是它会有兼容的问题。总之，如果在项目开发的过程中如果需要兼容 IE8 及其以下的浏览就不建议使用 Flex 弹性盒模型。

 ##### 弹性盒模型

 - **启动弹性盒模型：**

   display: flex;
   
 - **轴**

   **flex-direction：**设置主轴的方向。
   
    - row：向右。
    - column：向下。
    - row-reverse：向左。
    - column-reverse：向上。
   
   交叉轴：主轴沿顺时针方向旋转 90° 就得到了交叉轴。
   
 - **父容器：**

   **justify-content：**设置项目沿着主轴排列的方式。

   - flex-start：起始端对齐。
   - flex-end：末尾段对齐。
   - center：居中对齐。
   - space-around：等边距均匀分布（即在项目周围添加间距）。
   - space-between： 等间距均匀分布（即在项目之间添加间距）。

   **align-items：**设置项目沿着交叉轴排列的方式。

   - flex-start：起始端对齐。
   - flex-end：末尾段对齐。
   - center：居中对齐。
   - baseline：基线对齐（即沿项目的第一行文字的基线对）。
   - stretch：如果项目未设置高度或设为auto，将占满整个容器的高度。
   
   **flex-wrap：**设置换行方式。
   
   - nowrap：不换行。
   - wrap：换行。
   - wrap-reverse：逆序换行（指沿着交叉轴的反方向换行）。
   
   **flex-flow：**轴向与换行组合设置（复合属性，`flex-flow = flex-direction + flex-wrap`）。
   
   **align-content：**多行沿交叉轴对齐方式（即行与行之间的对齐方式）。
   
   - flex-start：起始端对齐。
   - flex-end：末尾段对齐。
   - center：居中对齐。
   - space-around：等边距均匀分布。
   - space-between：等间距均匀分布。
   - stretch：拉伸对齐。
   
 - **子容器：**

   **flex：**沿主轴如何伸缩（复合属性，`flex = flex-grow + flex-shrink + flex-basis`）。
   
   **align-self：**单独设置项目如何沿交叉轴排列。
   
   - flex-start：起始端对齐。
   - flex-end：末尾段对齐。
   - center：居中对齐。
   - baseline：基线对齐。
   - stretch：拉伸对齐。
   
   **flex-basis：**设置基准大小（表示在不伸缩的情况下子容器的原始尺寸）。
   
   **flex-grow：**设置拉伸比例。
   
   **flex-shrink：**设置收缩比例。
   
   **order：**设置排列顺序。

### 第五天

- #### 学习总结

  ##### 字体图标

  1. 访问 [阿里图标库](http://www.iconfont.cn/)。

  2. 将阿里图标添加到我的项目中。

  3. 复制 Font class 代码，放入项目中。

  4. 为元素添加图标的 class属性。

  ##### 相对路径与绝对路径

  - 相对路径：以 `./` 或 `../` 开头。
  - 绝对路径：以 `/` 开头。

  ##### 小程序

  - **页面生命周期**

    ![页面生命周期](http://p9myzkds7.bkt.clouddn.com/practice/mina-lifecycle.png)

  - **数据更新**

    setData 函数用于将数据从逻辑层发送到视图层（异步），同时改变对应的 this.data 的值（同步）。

    ```js
    this.setData({
      num: this.data.num
    })
    ```
    参数以 key: value 的形式表示，将 this.data 中的 key 对应的值改变成 value。

    其中 key 可以以数据路径的形式给出，支持改变数组中的某一项或对象的某个属性，如 array[2].message，a.b.c.d，并且不需要在 this.data 中预先定义。

    注意：

    1. 直接修改 this.data 而不调用 this.setData 是无法改变页面的状态的，还会造成数据不一致。
    2. 仅支持设置可 JSON 化的数据。
    3. 单次设置的数据不能超过1024kB，请尽量避免一次设置过多的数据。
    4. 请不要把 data 中任何一项的 value 设为 undefined ，否则这一项将不被设置并可能遗留一些潜在问题。

  - **数据绑定**

    简单绑定

    数据绑定使用 Mustache 语法（双大括号）将变量包起来。

      ```
      <view> {{ message }} </view>
      ```
      ```js
      Page({
        data: {
          message: 'Hello MINA!'
        }
      })
      ```
    - 组件属性(需要在双引号之内)。

      ```
      <view id="item-{{id}}"> </view>
      ```

    - 控制属性(需要在双引号之内)。

      ```
      <view wx:if="{{condition}}"> </view>
      ```

    - 关键字(需要在双引号之内)。

      ```
      <checkbox checked="{{false}}"> </checkbox>
      ```
    运算
    
    可以在双大括号内进行简单的运算。
    
    - 三元运算

      ```
      <view hidden="{{flag ? true : false}}"> Hidden </view>
      ```
    - 算数运算

      ```
      <view> {{a + b}} + {{c}} + d </view>
      ```
    - 逻辑判断

      ```
      <view wx:if="{{length > 5}}"> </view>
      ```
    - 字符串运算

      ```
      <view>{{"hello" + name}}</view>
      ```
    - 数据路径运算

      ```
      <view>{{object.key}} {{array[0]}}</view>
      ```
    组合
    
    也可以在 Mustache 内直接进行组合，构成新的对象或者数组。
    
    - 数组
    
      ```
      <view wx:for="{{[zero, 1, 2, 3, 4]}}"> {{item}} </view>
      ```
    - 对象
    
      ```
      <template is="objectCombine" data="{{for: a, bar: b}}"></template>
      ```
      也可以用扩展运算符 ... 来将一个对象展开。
      
      ```
      <template is="objectCombine" data="{{...obj1, ...obj2, e: 5}}"></template>
      ```
      如果对象的 key 和 value 相同，也可以间接地表达。
      
      ```
       <template is="objectCombine" data="{{foo, bar}}"></template>
      ```
       ```
        Page({
          data: {
            foo: 'my-foo',
            bar: 'my-bar'
          }
        })
       ```
      最终组合成的对象是 `{foo: 'my-foo', bar:'my-bar'}`。
      
  - **列表渲染**
    
    在组件上使用 wx:for 控制属性绑定一个数组，即可使用数组中各项的数据重复渲染该组件。

    默认数组的当前项的下标变量名默认为 index，数组当前项的变量名默认为 item
    
    ```
    <view wx:for="{{array}}">
      {{index}}: {{item.message}}
    </view>
    ```
    使用 wx:for-item 可以指定数组当前元素的变量名，

    使用 wx:for-index 可以指定数组当前下标的变量名：
    
    ```
     <view wx:for="{{array}}" wx:for-index="idx" wx:for-item="itemName">
      {{idx}}: {{itemName.message}}
    </view>
    ```
  - **条件渲染**   
    
    在框架中，使用 wx:if="{{condition}}" 来判断是否需要渲染该代码块：

    ```
    <view wx:if="{{condition}}"> True </view>
    ```
    也可以用 wx:elif 和 wx:else 来添加一个 else 块：
    
    ```
    <view wx:if="{{length > 5}}"> 1 </view>
    <view wx:elif="{{length > 2}}"> 2 </view>
    <view wx:else> 3 </view>
    ```
  - **模块化**
   
    - module.exports：对外共享本模块的私有变量与函数。
    
    - require：引用其他 wxs 文件模块。
    
  - **页面跳转**
  
    wx.navigateTo(Object object)
    
    ```
    wx.navigateTo({
      url: 'test?id=1'
    })
    ```
    ```
    Page({
      onLoad: function(option){
        console.log(option.query)
      }
    })
    ```
## 第三周

### 第一天

#### 前后端未分离

前端负责页面布局+JS交互
PHP后端，在HTML页面中嵌入PHP代码绑定数据

> 弊端：开发周期较长、后期维护要求前端能够一些PHP语法

#### 前后端分离

前端负责：布局+JS交互+数据获取绑定
后端负责：编写数据接口

#### 前端已经完成，后端接口尚未完成

1. 前端跟后端商定接口及数据格式。
2. Mock模拟线上数据，进行线上数据的获取与绑定。
3. 将Mock地址切换为真实服务器地址。
4. 联调测试。

#### easy-mock的使用流程

1. 注册并登陆 [easy-mock](https://www.easy-mock.com/) 。
2. 点击easy-mock右下角的加号，新建项目，定义baseURL。
3. 在当前项目下新建接口：数据包、请求方式（get或post等）、接口附加地址、接口描述	
4. 复制并得到接口地址。
5. 在小程序中需要获取数据的页面（如：classify.js）中，
   通过如下方式请求easy-mock平台获取数据：
    
   ```js
	onLoad: function (options) {
	    var _This = this;
	    wx.request({
	      url: 'https://www.easy-mock.com/mock/5ba99795d7eb4275f939a560/api/student',
	      method:'get',
	      success:function(res){
	        console.log(res.data.data);
	        _This.setData({ //6，将请求到的数据设置给Page对象的data
	          sData: res.data.data
	        })
	      }
	    })
	  },
     ```
7. 将数据绑定并显示到classify.wxml中去。

 ```html
 <view wx:for="{{sData}}">
  姓名：{{item.name}},分数：{{item.score}}
</view>	
```
#### 在小程序中使用 LeanClond

1. 注册 [Leancloud](https://leancloud.cn) 账号，并新建一个leancloud应用。

2. 将leancloud应用提供给我们的【域名】配置到小程序的域名白名单之下。

3. 下载av-weapp-min.js核心文件（该文件中有leancloud为我们提供的操作leancloud数据库的一些方法）

4. 将av-weapp-min.js复制到小程序项目下的utils文件夹中。

5. 在小程序项目中的app.js中对leancloud核心方法进行初始化。
   
   ```js
	var Cloud = require('/utils/av-weapp-min.js'); //引入核心文件
	Cloud.init({ //基于核心文件提供的对象下的init方法，对该对象进行初始化
	  appId:'lw0PCyoRcDpekvo6cGcWog2V-gzGzoHsz',
	  appKey:'yHD6FoFGxSqUAf86g9sE1pRz'
	})
   ```
6. 初始化的时候，appId与appKey来自【leancloud应用】-【设置】-【应用key】。

7. 使用leancloud核心对象提供的方法存储数据。

#### 在小程序中使用 form表单

1. wxml结构中，要有一个完整的form表单组件

  ```html
  <form bindsubmit="handleSubmit" bindreset="formReset">
    <view class="section">
      <view class="section__title">图书分类</view>
      <input name="classify" placeholder="请输入图书分类" value='1'/>
    </view>

    <view class="btn-area">
      <button formType="submit">确定录入</button>
    </view>
  </form>
  ```
  >【注意】表单三要素：
  > 1. 为form标签绑定 bindsubmit 事件
  > 2. from内部所有的input都要设置一个独一无二的name
  > 3. 要为表单内部的button添加 formType="submit" 属性

2. 在js中通过handleSubmit事件，拿到表单里面的内容
   
   ```js
	handleSubmit:function(ev){
	    console.log(ev.detail.value); //获取表单内容
	  },
   ```
3. 调用Leancloud提供的存储方法，将【表单内容】存储到云端服务器
  
   ```js
	handleSubmit:function(ev){
	    var v = ev.detail.value; //（1），获取到表单中的值
	    console.log(v)
	    var Book = Cloud.Object.extend('Book'); //（2）创建表对象
	    var book = new Book(); //（3）基于表对象，实例化一条具体的数据对象
	    
	    for(var attr in v){  // (4)为数据对象设置数据
	      book.set(attr,v[attr]);
	    }
	    book.save().then(function (res) { //（5）执行save方法，提交数据并保存
	      console.log('objectId is ' + res.id);
	    }, function (error) {
	      console.error(error);
	    });
	 },
   ```
  
### 第二天

#### LeanCloud查询数据流程

1. 在需要查询数据的小程序页面js文件中，引入leanCloud核心模块
 
 ```js
  var Cloud = require('../../utils/av-weapp-min.js')
 ```
2. 通过LeanCloud提供的查询方法，查询数据库并获取数据

  ```js
  onLoad: function (options) {
      var _This = this; //修正this指向
      var query =new Cloud.Query('Book');//2，实例化一个查询对象
      // query.equalTo('classify','1') //3，为查询对象设置匹配条件，可省略，没有匹配条件会获取所有数据
      query.find().then(function(res){ //4，执行查询方法，获取查询结果
        console.log(res);
        // 5，处理查询结果数据包，提取有用的数据
        var arr=[];  //为了存储过滤后的数据
        var Len = res.length;
        for(var i=0;i<Len;i++){
          res[i].attributes.id = res[i].id;
          arr.push(res[i].attributes)
        }
        console.log(arr);
        _This.setData({
          bookData:arr
        })
      })
    },
   ```
3. 查询方法
 
   - **find(optionsopt) → {Promise}**
   
     查询任务状态和结果，任务结果为一个 JSON 对象，包括 status 表示任务状态， totalCount 表示总数， results 数组表示任务结果数组，previewCount 表示可以返回的结果总数，任务的开始和截止时间 startTime、endTime 等信息。
   
   - **limit(n) → {AV.Query}**
   
     设置要返回的结果数量的限制。默认限制为100，一次最多返回1000个结果。
   
   - **skip （n）→{ AV.Query }**
   
     设置在返回任何结果之前要跳过的结果数。这对于分页很有用。默认是跳过零结果。
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    