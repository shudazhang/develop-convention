# BEM

## 1、什么是BEM命名规范    

BEM是指：块（block）、元素（element）、修饰符（modifier）的简写  

## 2、学好BEM需要掌握：  

>1、块（block）  
2、元素（element）    
3、修饰符（modifier）  
4、-（中划线）：仅作为连接字符使用，表示某个块或者子元素的多个单词之间的链接符号。    
5、__（双下划线）：用来链接块与块的子元素   
6、--（双中线）：用来链接块元素与修饰符  

## 3、BEM命名的好处
BEM命名的核心就是可以清晰的描述页面的结构,从其名字就可以知道某个标记的含义，于是通过查看class属性就可以知道元素之间的关联。

## 4、block、element、modifier之间的关系
>block: 独立的实体它本身就是有意义的  
如：header、container、aside

>element: block的一部分（一般为块的子元素）  
如：item

>modifier: 块或元素上的修饰使用它改变外观或行为的  
如：red、size、disabled  

## 5、BEM常见的几种情况
```
.block{}  
.block__element{}  
.block__element--modifier{}  

.block{}  
.block__element-name{}  
.block__element-name--modifier{}

.block-name{}  
.block-name__element{}  
.block-name__element--modifier{}

.block-name{}  
.block-name__element-name{}  
.block-name__element-name--modifier{}

.block--modifier{}  

```

## 6、BEM在实际中的使用
我们举一个项目列表的例子
```
<style>
    .project-list{}
    .project-list__item{}
    .project-list__item--red{}
    .project-list__item--green{}
</style>

<div class="project-list">
    <div class="project-list__item"></div>
    <div class="project-list__item--red"></div>
    <div class="project-list__item--green"></div>
</div>
```

## 7、在less或sass中使用BEM
```
.project{
    height:100px;
    &__item{
        height:50px;
        &--red{
            color: red;
        }
    }
}
```
编译后的结果：
```
.project {
  height: 100px;
}
.project__item {
  height: 50px;
}
.project__item--red {
  color: red;
}
```
## 8、个人感受
BEM命名规范给人的感觉就是好臃肿，类名好长，但还是建议在开发中遵循BEM命名规范，
它只是一种思想，方便我们开发中规范类名，使我们的代码具有更高的可读性，
BEM的难点并不是怎么使用，而是要不要使用，在什么地方使用，一定要灵活运用并不是所有地方都要遵循BEM的命名规范，
