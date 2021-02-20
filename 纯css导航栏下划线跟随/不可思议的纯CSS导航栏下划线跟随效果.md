## 不可思议的纯CSS导航栏下划线跟随效果

> 这是一个项目中常用的功能, 但是这个需求把我们平常用的效果美化,并且强调用纯CSS实现

![image-20210220120249176](.\image-20210220120249176.png)

~~~html
<ul>
  <li>不可思议的CSS</li>
  <li>导航栏</li>
  <li>光标小下划线跟随</li>
  <li>PURE CSS</li>
  <li>Nav Underline</li>
</ul>
~~~

1. 先有一个基本的样式

   ~~~css
   ul,li {
   	list-style: none;
   }
   ul {
       display: flex;
       position: absolute;
       top: 50%;
       left: 50%;
       transform: translate(-50%, -50%);
   }
   
   li {
       position: relative;
       padding: 20px;
       font-size: 24px;
       color: #000;
       line-height: 1;
       cursor: pointer;
       border-bottom: 1px solid #000;
   }
   ~~~

   ![image-20210220121257272](D:\workSpace\learn\CSS\纯css导航栏下划线跟随\image-20210220121257272.png)

2. 要实现跟着鼠标跟随,用css实现 所以最后我们应该伪元素实现 这里选择before

   ~~~css
   li {
       position: relative;
       padding: 20px;
       font-size: 24px;
       color: #000;
       line-height: 1;
       cursor: pointer;
   }
   li::before {
       content: "";
       position: absolute;
       top: 0;
       //这里left:100% 即在li的最右端作起点
       left: 100%;
       //因为有一个由短变长的过程  我们先设置长度为0
       width: 0;
       height: 100%;
       border-bottom: 2px solid #000;
       transition: 0.2s all linear;
   }
   ~~~

   这里面用到了transition,  transition之前我也没常用

   参考 https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions

   ### **transition** 

   * 是==transition-property==, ==transition-duration==,==transition-timing-function== 和 ==transition-delay==的一个简写属性.

     ~~~css
     /* Apply to 1 property */
     /* 属性名 | 持续时间 */
     transition: margin-right 4s;
     
     /* 属性名 | 动画时间4s |  延迟1s开始*/
     transition: margin-right 4s 1s;
     
     /* 属性名 | 动画时间4s | 过渡动画 */
     transition: margin-right 4s ease-in-out;
     ~~~

     

   * **transition-property**  基本属性都可以用,  根据自己需要使用在合适属性上  比如 上面例子的 all:代表全部属性 , margin-right 就只对margin-right有效

   * **transition-duration** 属性以秒或毫秒为单位制定过渡动画所需的时间. **默认值 0s**, 表示不出现过渡动画.

     * 可以指定多个时长,每个时长被应用到transition-property指定的对应属性上.

     * 如果指定的时长个数小于属性个数,那么时长列表会重复.

     * 如果时长列表更长,那么该列表会被裁剪

       ~~~css
       /* 所有属性过渡动画时间为6s */
       transition-duration: 6s;  
       /* 所有属性过渡动画时间为120ms */
       transition-duration:120ms;
       /* 对应transition-property上的两个属性值的过渡动画时间 */
       transition-duration: 1s, 15s;
       ~~~

   * **transition-timing-function** 属性变化过程中中间值是怎样计算的.
     * 初始值: ease
     * 常用的 : ease-in, ease-out,ease-in-out,linear,step-start,step-end,steps(4, end)

   > 通过对transiton的学习,  我现在知道 上面代码中 transition: 0.2s all linear;这句的意思是 作用用所有属性通过0.2s的过渡时间线性变化到最后的样式

3. 滑动到的时候有样式变化,所以在伪类上添加样式

   ~~~css
   li:hover::before {
       /*滑动的时候 长度从0变为100%*/
       width: 100%;
       top: 0;
       /* left 属性 从100%变为0, 就有一个从右到左的变化动画*/
       left: 0;
       /*延迟0.1s开始执行 属性变化*/
       transition-delay: 0.1s;
       border-bottom-color: #000;
       z-index: -1;
   }
   ~~~

4. 最难的跟随动画,从左滑的时候 下横线从左到右,  鼠标从右往左的时候下横线也从右往左

   > 当前默认就是从右往左的动画过渡样式, 这里就借助了 ~,  选中所有同级元素 把所有的元素的left:0 

   ~~~css
   li:hover ~ li::before {
       left: 0;
   }
   /*即这里选中所有的 li:hover 后面的li::before*/
   ~~~

   这里就是第一个元素还有点问题