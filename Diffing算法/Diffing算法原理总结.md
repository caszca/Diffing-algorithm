```javascript
     /*
        经典面试题:
                1). react/vue中的key有什么作用？（key的内部原理是什么？）
                2). 为什么遍历列表时，key最好不要用index?

                1. 虚拟DOM中key的作用：
                        1). 简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用。
                        2). 详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, 
                                                    随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：
                                a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：
                                            (1).若虚拟DOM中内容没变, 直接使用之前的真实DOM
                                            (2).若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM
                                b. 旧虚拟DOM中未找到与新虚拟DOM相同的key
                                            根据数据创建新的真实DOM，随后渲染到到页面

                2. 用index作为key可能会引发的问题：
                            1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:
                                            会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。
                            2. 如果结构中还包含输入类的DOM：
                                            会产生错误DOM更新 ==> 界面有问题。

                            3. 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，
                                仅用于渲染列表用于展示，使用index作为key是没有问题的。

                3. 开发中如何选择key?:
                            1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
                            2.如果确定只是简单的展示数据，用index也是可以的。
                */
```

```javascript
 /* 
         一、使用索引值(index)作为key：
                                初次挂载组件：
                                        1.初始的数据：
                                                {id:'num_000',name:'小张',age:18},
                                                {id:'num_001',name:'小李',age:19},
                                        2.初始的虚拟DOM
                                                <li key=0>小张-18 <input type="text"/> </li>
                                                <li key=1>小李-19 <input type="text"/> </li>

                                更新：
                                        1.新的数据：
                                                {id:'num_000',name:'小王',age:20},
                                                {id:'num_001',name:'小张',age:18},
                                                {id:'num_002',name:'小李',age:19},
                                        2.新的虚拟DOM
                                                <li key=0>小王-20 <input type="text"/> </li>
                                                <li key=1>小张-18 <input type="text"/> </li>
                                                <li key=2>小李-19 <input type="text"/> </li>


 二、使用唯一标识(id)作为key：
                                初次挂载组件：
                                        1.初始的数据：
                                                {id:'num_000'',name:'小张',age:18},
                                                {id:'num_001'',name:'小李',age:19},
                                        2.初始的虚拟DOM
                                                <li key="num_000'">小张-18 <input type="text"/> </li>
                                                <li key="num_001'">小李-19 <input type="text"/> </li>

                                更新：
                                        1.新的数据：
                                                {id:'num_000',name:'小王',age:20},
                                                {id:'num_001',name:'小张',age:18},
                                                {id:'num_002',name:'小李',age:19},
                                        2.新的虚拟DOM
                                                <li key="num_000">小王-20 <input type="text"/> </li>
                                                <li key="num_001">小张-18 <input type="text"/> </li>
                                                <li key="num_002">小李-19 <input type="text"/> </li>

        */
```

```javascript
/* 

    针对该事例，因为无法放上图片，大家可以自取代码进行测试，这里只对index作为key值，为何小王后的input输入框
呈现的是小张先前输入框的内容，小张后的input输入框呈现的是小李的先前输入框的内容。
    大致如下：
    小王 <input>小张</input>
    小张 <input>小李</input>
    小李 <input></input>

  解：因为进行添加小王后，重新生成了小王、小张、小李的虚拟DOM，React的Diffing算法，会取相同的key值进行比
较（单个标签为最小比较单位），比较发现内容不同，但input标签未发生改变，则复用小张的input输入框，所以小王后
的input输入框里显示的是小张的信息，因为它就是同一个真实DOM，未重新生成渲染。
  至于小张后的输入框显示的是小李的先前输入框的内容，也是如上的原因。

  那么这里有些同学可能就会有疑惑——我不是在小张的input输入框里写入了数据了吗?它怎么在于小王的虚拟DOM比较时
是相同的呢？未理解问题的同学可以再思考一下问题，再看解答。

  解答：因为虚拟DOM没有value值，通俗点讲，也就是虚拟DOM是不知道用户的操作，它不知道你往里面写入数据，更不
知道数据是什么，value值是真实DOM所拥有。这样就解释了原因，因为我往input里输入的数据，虚拟DOM它不拥有，比较
时，它比较了双方都为input标签，type属性都是text。 

    */
```
