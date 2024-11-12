# 主页

<center><font  color= #518FC1 size=6 class="ml3">“莫愁千里路，自有到来风”</font></center>


<div id="rcorners2" >

<div id="rcorners1" class="date-display">
    <p class="p1"></p>
</div>

<style>
    .date-display {
        color: #4351AF;
    }
</style>
<script defer>
    function format(newDate) {
        const day = newDate.getDay();
        const y = newDate.getFullYear();
        const m = newDate.getMonth() + 1 < 10 ? `0${newDate.getMonth() + 1}` : newDate.getMonth() + 1;
        const d = newDate.getDate() < 10 ? `0${newDate.getDate()}` : newDate.getDate();
        const h = newDate.getHours() < 10 ? `0${newDate.getHours()}` : newDate.getHours();
        const min = newDate.getMinutes() < 10 ? `0${newDate.getMinutes()}` : newDate.getMinutes();
        const s = newDate.getSeconds() < 10 ? `0${newDate.getSeconds()}` : newDate.getSeconds();
        const dict = {1: "一", 2: "二", 3: "三", 4: "四", 5: "五", 6: "六", 0: "天"};
        
        return `${y}年${m}月${d}日 ${h}:${min}:${s} 星期${dict[day]}`;
    }

    const timerId = setInterval(() => {
        const newDate = new Date();
        const p1 = document.querySelector(".p1");
        if (p1) {
            p1.textContent = format(newDate);
        }
    }, 1000);
</script>
</div>

<div class="grid cards" markdown>

-   :octicons-bookmark-16:{ .lg .middle } __推荐文章__

    ---

    - [了解我](about.md)

-   :simple-materialformkdocs:{ .lg .middle } __开发教程__

    ---

    - [简易的低代码平台](technology/web/lowCode/first.md)


-   :material-format-font:{ .lg .middle } __源码学习__

    ---

    - [vue源码学习](sourceCode/vue.md)

-   :simple-aboutdotme:{ .lg .middle } __其他分享__

    ---

    - [:octicons-arrow-right-24: 了解我](about.md)[^see-how-much-I-love-you]

</div>



[^Knowing-that-loving-you-has-no-ending]:风吹哪页读哪页
[^see-how-much-I-love-you]:Which page is blown by the wind and which page to read

<body>
<font color="#B9B9B9">
  <p style="text-align: center; ">
      <span>本站已经运行</span>
      <span id='box1'></span>
</p>
  <div id="box1"></div>
  <script>
    function timingTime(){
      let start = '2024-11-11 00:00:00'
      let startTime = new Date(start).getTime()
      let currentTime = new Date().getTime()
      let difference = currentTime - startTime
      let m =  Math.floor(difference / (1000))
      let mm = m % 60  // 秒
      let f = Math.floor(m / 60)
      let ff = f % 60 // 分钟
      let s = Math.floor(f/ 60) // 小时
      let ss = s % 24
      let day = Math.floor(s  / 24 ) // 天数
      return day + "天" + ss + "时" + ff + "分" + mm +'秒'
    }
    setInterval(()=>{
      document.getElementById('box1').innerHTML = timingTime()
    },1000)
  </script>
  </font>
</body>



