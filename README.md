# 闭包的理解


之前有写过闭包，作用域，this方面的文章，但现在想想当时写的真是废话太多了，以至于绕来绕去的，让新手反而更难理解了，所以就有了此篇文章，也好和闭包，作用域，this告一段落。

　   

    第一个问题：什么是闭包？

    我不想回答这个问题，但是我可以告诉你的是闭包可以解决函数外部无法访问函数内部变量的问题。下面是一段没有使用闭包的代码：

    　　function fn(){

    　　　　var a = 10;

    　　}

         alert(a);

         //报错了，因为a没有定义，虽然函数fn里面定义了a但是，但是它只能在函数fn中使用。也就是作用域的问题。

　  再看‘闭包可以解决函数外部无法访问函数内部变量的问题’这段话，好像有点意思，那么究竟闭包是怎么做的，看下面代码。

    　　function fn(){

            //定义了一个变量name

    　　　var name = '追梦子';

            //我现在想在外部访问这个变量name怎么办呢？哈：不是有return，我把它返回出去，我再用个变量接收一下不就可以了，哈哈哈哈~~~~~

             return name;

          }

          var name = fn();//接收fn返回的name值。

          alert(name);//追梦子；

     ·······这里的闭包就是利用函数的return。除了通过return还可以通过其他的几种方法如下：

　　

　　方法1：

    　　　　

    function fn(){
    　　var a = 0;
    　　b = a;
    }
    alert(b)

　　这里利用了js的一个特性，如果在函数中没有用var定义变量，那么这个变量属于全局的，但这种方法多少有些不好。

　　

　　方法2：

    　　

    var f = null;
    function fn(){
    　　var a = 0;
    　　f = function(){
    　　　　a++;
    　　　　f.a = a;
    　　};
    }
    fn();
    f();
    alert(f.a);//1
    f();
    alert(f.a);//2

 

    但好像也没有那样神奇对吧？其实闭包还有一个很重要的特性。来看一个例子。

    　　var lis= document.getElementsByTagName['li']; 

    　　//假如这段代码中的lis.length = 5;

          for(var i=0;i<lis.length;i++){

    　　　　lis[i].onclick = function(){

    　　　　　　alert(i);

    　　　　};

         }

　　最终结果是不管单击哪个li元素都是弹5。不信你试试。为什么呢。看下面分析。

    　　　　

    　　for(var i=0;i<lis.length;i++){

          }

          // i = 5对吧

    　　lis[0].onclick = function(){

    　　　　alert(i); 

    　　};

    　　lis[1].onclick = function(){

    　　　　alert(i); 

    　　};

    　　lis[2].onclick = function(){

    　　　　alert(i);

    　　};

    　　lis[3].onclick = function(){

    　　　　alert(i);

    　　};

    　　lis[4].onclick = function(){

    　　　　alert(i);

    　　};

    为什么会这样呢，因为你for循环只是给li绑定事件，但是里面的函数代码并不会执行啊，这个执行是在你点击的时候才执行的好吧？但是此时的i已经是5了，所以所有的都打印出5来了。

　　如果想解决这个问题我们可以使用闭包，闭包的特点不只是让函数外部访问函数内部变量这么简单，还有一个大的特点就是通过闭包我们可以让函数中的变量持久保持。来看。
  
  //解决循环的问题
  方案1：作用域的自我执行
  var lis= document.getElementsByTagName['li'];
    　　//假如这段代码中的lis.length = 5;      
          for(var i=0;i<lis.length;i++){

    　　　　lis[i].onclick = （function(){

    　　　　　　alert(i);//0,1,2,3,4,5

    　　　　})(i);

 }
 

  

    　　function fn(){

    　　  var num = 0;

    　　　return function(){

    　　　　num+=1;

               alert(num);　　　

             };　　

          }

          var f = fn();

          f(); //1

          f(); //2

     如果你是初学者可能没觉得这有什么。OK，让你看个东西。

    　　function fn(){

            var num = 5;

            num+=1;

            alert(num);

         }　　

        fn(); //6

        fn(); //6

     为什么呢？因为函数一旦调用里面的内容就会被销毁，下一次调用又是一个新的函数，和上一个调用的不相关了，不过有个特殊的情况，看这：

    　　

    　　function fn(){

    　　  var num = 0;

    　　　return function(){

    　　　　num+=1;

               alert(num);　　　

             };　　

          }

          var f = fn();

          f(); //1

          f(); //2

        

    这段代码很简单，不要被它欺骗了，我们首页定义了一个fn函数，里面有个num默认为0，接着返回了一个匿名函数（也就是没有名字的函数）。我们在外部用f接收这个返回的函数。这个匿名函数干的事情就是把num加1，还有我们用来调试的alert。

　 这里之所以执行玩这个函数num没有被销毁是因为那个匿名函数的问题，因为这个匿名函数用到了这个num，所以没有被销毁，一直保持在内存中，因此我们f()时num可以一直加。

   这里你可以看不懂了，之所以有这种感觉是因为js回收机制你不懂，强烈建议你看我之前的再次讲解js中的回收机制是怎么一回事。这篇文章。

 

   关于闭包的知识就到这里了，如果你想看关于闭包的案例可以看这篇：从闭包案例中学习闭包的作用，会不会由你。

   而外：关于在for循环中绑定事件打印变量i是最后一次。

　　

   而外说一句：这里并不是说return就是闭包，这里只是强调return的重要性，如果你还是一个新手建议你多看一些初级文章，在来看这篇文章，希望你会有新收获。写这篇文章一开始我也说了它的目的是回顾一下当初我没有理解的地方，当初已经理解的这篇文章并没有过多的去讲。

　
 

     这篇文章并不算是一篇入门的教程，这篇文章主要是总结之前没有理解的地方，或者是以一种更加简单明了的方式写的，当然是按照我自己的理解来的，不一定你能理解，sorry，好了一切就从这里结束吧。
