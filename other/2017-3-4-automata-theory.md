---
layout: post
title: Automata Theory, Languages, and Computation
date: 2017-3-12 22:00:00
category: "Automata"
---
<h2>自动机理论笔记</h2>
<p>这是笔者在大二下学期的自动机理论的学习笔记，欢迎大家过来一起交流，有不足或者错误希望大家指正。由于学校的学时有限，我学习的章节也不会太全，就是说我不会写完书中所有内容。最后强调一下，这不是一个教程，只是一个学习心得，推荐有一定基础的同学阅读。</p>

<h2>第二章：有穷自动机</h2>
<p>这一章是我们这门课的基础，对于这一章我们要厘清概念，重点区分各种有穷自动机，如：确定型有穷自动机、非确定型有穷自动机，另外这一章的重点还在于怎样构造出一个有穷自动机接受符合一定规则的串，一般来讲关于构造自动机，并没有绝对有效的方法，需要我们不断练习总结出自己的经验，在这里我会试着将自己对于一些典型题目的理解写下来，也许对你有用。</p>
<p>这一章书上是以一个银行验证的例子开始的，嗯，又复杂又难懂，我建议先不看它，等我们学完了这一章回过头再来看也不迟。跳过这一节后，我们首先会学习到确定型有穷自动机，英文缩写DFA。我们在这里先给出DFA的公理化定义。</p>
<p>一个确定型有穷自动机包括：
	<ul>
		<li>1.一个有穷的状态集合，记作
			<img src="http://latex.codecogs.com/gif.latex?Q"/></li>
		<li>2.一个有穷的输入符号集合，记作
			<img src="http://latex.codecogs.com/gif.latex?\Sigma"/></li>
		<li>3.一个转移函数，以一个状态和一个输入符号作为变量，返回一个状态，记作
			<img src="http://latex.codecogs.com/gif.latex?\delta"/></li>
		<li>4.一个初始状态，是我们有穷集合
			<img src="http://latex.codecogs.com/gif.latex?Q"/>中的状态之一</li>
		<li>5.一个终结状态或一个终结状态的集合
			<img src="http://latex.codecogs.com/gif.latex?F"/></li>
	</ul>
</p>
<p>由于只要我们给出以上这五个部分，一个确定型有穷自动机就完全的清楚了，所以我们通常也用一个五元组记录一个DFA，我们结合下面这幅图举个例子,帮助记忆。</p>
<img src="https://raw.githubusercontent.com/longlongman/blog/gh-pages/images/AUTO/DFA.jpg"/>
<p>上图其实是DFA的图形表达形式，比较直观，便于理解。在这个例子中，S,A,B,C,D,E,F都是我们的状态，由它们组成的集合就是我们上面的<img src="http://latex.codecogs.com/gif.latex?Q"/>；在图上还有一些小写字母标注在从一个状态指向另一个状态的箭头上，它们是我们的输入符号，a,b就是我们<img src="http://latex.codecogs.com/gif.latex?\Sigma"/>中的元素；而箭头本身就是我们的转移函数<img src="http://latex.codecogs.com/gif.latex?\delta"/>，它表示在一个状态下输入一个符号后，状态怎样变化，例如在图上有从A到C的一个箭头，并且箭头上标注了一个a，这说明当我们当前状态是A时，如果输入了一个a符号，那么状态会迁移到C上；在众多状态中有一个没有起始状态的大箭头指向的状态S就是我们的初始状态；而有两个环圈起的状态C,D,E,F组成了我们的终结状态集合。</p>
<p>看到这里我们不禁会想定义了这样一个数学机器到底可以做什么事情呢？事实上的确定型有穷自动机是我们整个自动机理论学习中能力最弱的自动机，但是它却已经十分有用了，在串识别中经常可以看到它的身影。例如：常用的UNIX正则表达式，是从理论上的严格定义的正则表达式的基础之上扩展而来的，而理论上的正则表达式则可以证明是与我们的DFA等价的，也就是说由正则式定义的每一个串都可以用一个DFA来识别接受，等到知道什么叫做正则式的语言和DFA的语言后，就会发现这里的两个东西是等价的，其实就是说正则式和DFA的语言是相同的。</p>
<p>既然我们知道DFA可以识别处理一个串，但是它是怎样去识别的呢，这里我们给出两种对于处理方法的定义，两者没有实质上的差别，第一种好理解，但是第二种更加简洁，同时还引入一个扩展转移函数的概念。</p>
<ul>
	<li>一、假设一个输入符号的串可以表示为a1,a2,...,an，q0是开始状态，当输入了一个符号a1后查询状态转移函数<img src="http://latex.codecogs.com/gif.latex?\delta"/>，得到下一个状态，这里记作q1，之后再从q1开始，接受符号，改变状态，如此迭代的做下去，直到做到an，如果最后得到的状态qn是终结状态集中的一个元素，那么这个串就被接受了。</li>
	<li>二、一个符号串w也可以被表示为xa的形式，其中a是一个输入符号，而x是去掉a后的符号子串，关于接受的过程可以很简洁的用<img src="http://latex.codecogs.com/gif.latex?\widehat{\delta&space;}\left&space;(&space;q,w&space;\right&space;)=\delta&space;\left&space;(&space;\widehat{\delta&space;}\left&space;(&space;q,x&space;\right&space;),a&space;\right&space;)"/>递归表示，其中的<img src="http://latex.codecogs.com/gif.latex?\widehat{\delta&space;}"/>是我们将转移函数由输入单个符号扩展到输入一个符号串时引入的扩展转移函数，其本质就是不停地递归调用<img src="http://latex.codecogs.com/gif.latex?\delta"/>。</li>
</ul>