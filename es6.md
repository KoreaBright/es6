0. let  
1）let用来声明变量（类似var），let声明的变量只在自己声明的区块内有效  
2）用let声明的变量，一定要在变量声明之后使用，否则报错，但是如果声明了却没有赋值，那默认值仍然是undefined
   提示：用var声明变量的时候，变量在声明之前使用，值为undefined  
3）let声明的变量，会使同名的父级或全局变量在当前区块内无效（所以在有let的区块中，let定义变量之前都属于这个变量的死区，只要使用这个变量就报错）  
   示例：  
   <pre>
   var tmp = 123;  
   
   if (true) {
      tmp = 'abc'; // ReferenceError
      let tmp;
   }
   </pre>
   说明：var定义了全局变量tmp，if中let定义了局部变量tmp，let定义的变量导致var定义的变量失效，并且在if内变量没有声明就直接使用导致报错
   
   注意：let和const都有这样的特性，目的就是让大家先声明再使用变量  
4）在一个区域中，如果已经用let声明了变量，绝对不可以再通过let/var方式声明同名的变量，否则报错
   注意：与参数同名的变量也不可声明了  
   <pre>
    // 两种方式都定义变量a，报错
    function () {
      let a = 10;
      var a = 1;
    }  
    
    // 用let定义两次变量a，报错
    function () {
      let a = 10;
      let a = 1;
    }  
    
    function func(arg) {
      let arg; // let定义的变量与参数名重复，报错
    }
   </pre>  
   
0. 块级作用域问题  
1）es5中只有全局作用域和函数作用域，却没有块级作用域，这带来很多误解  
   <pre>
    示例1：内层变量覆盖外层变量  
    var tmp = new Date();  // 位置1  
    
    function f() {
      console.log(tmp);
      if (false) {
        var tmp = "hello world"; // 位置2
      }
    }

    f(); // undefined
    
    解析：位置1定义的变量在全局作用域中，位置2定义的变量在函数作用域中，
         由于函数作用域里面的变量在整个函数都是有效的，所以函数f中打印的tmp，实际上应该是函数内部定义的变量，
         只是没有声明就直接使用了，所以是undefined
   </pre>  
   
   <pre>
    示例2：用来计数的循环变量泄露为全局变量
    var s = 'hello';

    for (var i = 0; i < s.length; i++) {
      console.log(s[i]);
    }

    console.log(i); // 5
    解析：在for循环里面定义的变量i，实际上是在全局作用域里面的，所以在整个环境下都可以使用变量i
   </pre>  
2）es6中的块级作用域：let会使父级、全局作用域中的同名变量失效，所以实际上let是为js新增了块级作用域  
   <pre>
     示例1：外层代码块不受内层代码块的影响
     function f1() {
      let n = 5; // 位置1
      if (true) {
        let n = 10; // 位置2
      }
      console.log(n); // 5
     }
     解析：if环境中的变量n的值，不会影响到函数f1的作用域范围内的变量，所以打印值依然为位置1定义的n -- 5
   </pre>  
   
   <pre>
    示例2：允许块级作用域的任意嵌套 
    {{{{{let insane = 'Hello World'}}}}};
   </pre>   
   
   <pre>
    示例3：外层作用域无法访问内层作用域的同名变量
    {{{{
      {let insane = 'Hello World'}
      console.log(insane); // 报错
    }}}};
   </pre>  
   
   <pre>
    示例4：内层作用域可以定义外层作用域的同名变量  
    {{{{
      let insane = 'Hello World';
      {let insane = 'Hello World'}
    }}}};
   </pre>  
3）块级作用域内能否声明函数  
   --> es5规定，函数只能在顶层作用域和函数作用域中声明，不能在块级作用域中声明  
      （然而浏览器并没有遵守这个规定，还是支持块级作用域定义函数的不会报错，只是在严格模式下遵守了，还是会报错）  
   --> es6规定，函数可以在块级作用域中声明，但是在块级作用域之外无法引用  
       (然而es6指出，浏览器可以不遵循上面的规则)  
   所以，还是不要在块级作用域中定义函数
