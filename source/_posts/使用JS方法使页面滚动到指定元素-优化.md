---
title: 使用JS方法使页面滚动到指定元素+优化
date: 2018-05-10 15:51:33
tags: JavaScript
categories: 前端
---

当页面最上部有顶部菜单是，使用锚点跳转的方法很容易挡住想要呈现的内容<escape><!-- more --></escape>（如字被挡了一半），为避免出现这样的问题，故滚动到指定元素使用用JS的方法来实现
### 1  初版（第一版）
```javascript
//1 获取所有的a标签
let aTags=document.querySelectorAll("nav.menu ul li a") 
    //console.log(aTags)
//2 遍历a标签并点击标签滚动到指定元素位置
for(let i=0;i<=aTags.length;i++){
    aTags[i].onclick=function(x){
        x.preventDefault();  //阻止a标签默认的跳转
        //console.log(x.currentTarget);
        let a=x.currentTarget;
        let href=a.getAttribute("href"); //找到href中的内容，如果href中时一个锚点则返回#siteSkill
        //console.log(href);
        let element=document.querySelector(href); //找到内容中的锚点对应ID的标签，如对应的锚点名为#siteSkill，则返回<section class=​"skills" id=​"siteWorks">​…​</section>​
        //console.log(element);
        let top=element.offsetTop;     //获取元素到页面最顶点的高度（不会随着页面滚动变化的高度）
        //console.log(top);
        window.scrollTo(0,top-80);
    }
}
```
这样能准确的达到想要的地方并且也不会内容也不会被挡住，但是，也存在一些缺点，比如跳转太生硬，中间没有过渡，影响用户体验。

### 2  优化（第二版）

```javascript
//1 获取所有的a标签
let aTags=document.querySelectorAll("nav.menu ul li a") 
//console.log(aTags)
//2 遍历a标签并点击标签跳到指定元素位置
for(let i=0;i<=aTags.length;i++){
    aTags[i].onclick=function(x){
        x.preventDefault();  //阻止a标签默认的跳
        let a=x.currentTarget;
        let href=a.getAttribute("href"); //找到href中的内容，如果href中时一个锚点则返回#siteSkill
        let element=document.querySelector(href); //找到内容中的锚点对应ID的标签，如对应的锚点名为#siteSkill，则返回<section class=​"skills" id=​"siteWorks">​…​</section>​
        let top=element.offsetTop;    
        let n=25;  //动的次数
        let t=500/n;   //多久动一次
        let currentTop=window.scrollY; //所在的位置
        let targetTop=top-80;  //目标位置
        var s=(targetTop-currentTop)/n;   //每次动的距离
        let i=0;
        let id=setInterval(()=>{
            if(i===n){
                window.clearInterval(id);
                return;
            }             //当i=n时停止动画
            i=i+1
            window.scrollTo(0,currentTop+s*i)
            
        },t)
    }
}
```
优化后有跳转动画，但是依然还有缺点，比如：定义的是时间一致，所以跳转到距离TOP不同位置的地方速度不一致。看起来依然生硬不自然

### 3  继续优化（第三版 引入tween.js库）
```javascript
//1 引入tween.js库
<script src='https://cdnjs.cloudflare.com/ajax/libs/tween.js/17.2.0/Tween.min.js'></script>  
<script> 

//2 获取所有的a标签 let aTags=document.querySelectorAll("nav.menu ul li a"); 
function animate(time){
    requestAnimationFrame(animate);
    TWEEN.update(time);
}
requestAnimationFrame(animate);

//3 遍历a标签并在点击标签时滚动到指定元素的位置
for(let i=0;i<=aTags.length;i++){
    aTags[i].onclick=function(x){
        x.preventDefault();  //阻止a标签默认的跳
        let a=x.currentTarget;
        let href=a.getAttribute("href"); //找到href中的内容，如果href中时一个锚点则返回#siteSkill
        let element=document.querySelector(href); //找到内容中的锚点对应ID的标签，如对应的锚点名为#siteSkill，则返回<section class=​"skills" id=​"siteWorks">​…​</section>​
        let top=element.offsetTop;   
        let currentTop=window.scrollY; //所在的位置
        let targetTop=top-80;  //目标位
        let s=targetTop-currentTop;     //所在到目标的高度差
        let t=Math.abs((s/100)*200)    //Math.abs方法保证时间为正值不为负数。ps:Math的首字母需要大写！！！
        var coords={y:currentTop};     //y为所在位置
        if(t>=500){t=500}     //如果时间最大为500，不超过500
        var tween=new TWEEN.Tween(coords)
            .to({y:targetTop},t)   //y为到达目标位置，时间
            .easing(TWEEN.Easing.Quadratic.In)
            .onUpdate(function(){
                window.scroll(0,coords.y)
            })
            .start();
    }
}

</script>
```