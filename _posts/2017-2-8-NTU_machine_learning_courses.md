---
layout: post
title: 台大机器学习视频教程（机器学习基石）笔记
date: 2017-2-8 21:21:00
category: "courses"
---
<h2>视频课程总体评价</h2>
<p>最近看了一小段由台湾大学林轩田教授主讲的机器学习的课程，名字是“机器学习基石”，个人感觉教学质量确实比较好，对新手入门来讲是十分好的教程，不想其他的书籍或者视频，这门课会涉及到理论，但这部分从来不是给你摆公式，而是能够引导你思考，一步步推导，讲得也比较清楚，十分喜欢这一点，有兴趣的可以去coursera看看，上面有视频。</p>

<h2>PART ONE: Course Introduce</h2>
<p>这一部分主要是课程的介绍，是一些关于机器学习的大方向的问题，由What、Why、When、How、How to do better这几个方面组成，讲课的过程中实践与理论都会讨论到。</p>
<p>首先第一节课开始讲什么是机器学习，也就是What的部分。我们类比人技能习得的过程，通常是通过观察，然后学习，找到观察对象的某种规律，最后我们使用这种规律，习得某种技能。机器学习这个名字其实很直白，就是让计算机模拟人学习的过程，最后计算机也可以习得技能，变得更加智能。不过这里计算机并不具备人类的感官，它只能理解数据，所以机器学习就是计算机通过使用机器学习的算法分析大量数据，最后从数据中获得有用的技能。</p>
<p>然后是为什么要使用机器学习？Why？在现实生活中，有很多我们难以用十分准确的语言定义的事物，例如：我们如何定义一棵树，怎样的事物才是一棵树，这个问题就比较难以准确定义，你也许可以罗列关于树的几十个特征，用这些特征去定义什么是树，什么不是，换句话来说就是给树建立一个模型，这个方法既耗时又很笨，还要你知道关于树的专业知识，最后准确性也值得质疑，毕竟树和树也是不尽相同的。然而对于这种问题，机器学习通常是十分在行的，这时候机器学习的重要性与优势就显现出来了。</p>
<p>什么时候能够使用机器学习？When？有三个关键：
<ul>
	<li>你的问题存在着一些被隐藏的模式是可以被学习的。例如：如果一个问题完全是随机的，那么谁都没有办法通过学习预测到下一次的结果。</li>
	<li>问题难以用程序定义和描述，例如：定义树的例子。</li>
	<li>我们必须有数据，机器学习的基础就是对数据进行分析，没有数据就没有机器学习。</li>
</ul></p>
<p>最后我认为比较重要的地方是关于机器学习流程的理解，熟悉这部分对后面的学习十分有益处。先附上一张示意图。</p>
<img src="https://raw.githubusercontent.com/longlongman/blog/gh-pages/images/NTU/NTU_1.JPG">
<p>解释图中各个字母所代表的意义，<img src="http://latex.codecogs.com/gif.latex?X"/>代表的是我们的数据对象的集合，它通常是由许多向量组成，每个向量是我们的一个数据实例，<img src="http://latex.codecogs.com/gif.latex?Y"/>则是我们最后希望得到的结果的集合，例如分类问题里，我们的<img src="http://latex.codecogs.com/gif.latex?Y"/>就是我们通过计算后得到的<img src="http://latex.codecogs.com/gif.latex?X"/>的类别的集合，如果我们相信<img src="http://latex.codecogs.com/gif.latex?X"/>和<img src="http://latex.codecogs.com/gif.latex?Y"/>之间是有关系的，那么我们可以认为存在一个函数<img src="http://latex.codecogs.com/gif.latex?f"/>可以通过<img src="http://latex.codecogs.com/gif.latex?X"/>计算得到<img src="http://latex.codecogs.com/gif.latex?Y"/>，我们写作<img src="http://latex.codecogs.com/gif.latex?f:X \rightarrow Y"/>，这里要强调的是对于这个函数<img src="http://latex.codecogs.com/gif.latex?f"/>我们是不知道的，事实上我们的目标就是去逼近这个函数，所以这个<img src="http://latex.codecogs.com/gif.latex?f"/>也叫做目标函数。而<img src="http://latex.codecogs.com/gif.latex?D"/>则是我们的<img src="http://latex.codecogs.com/gif.latex?X"/>和<img src="http://latex.codecogs.com/gif.latex?Y"/>的笛卡儿积的一个子集，我们称之为训练取样。<img src="http://latex.codecogs.com/gif.latex?A"/>则是我们的学习算法，是我们机器学习的核心，其实这部分要做的工作就是通过计算我们输入机器的<img src="http://latex.codecogs.com/gif.latex?D"/>从我们的假设集合（hypothesis，简单认为是一个包含了许多函数的集合）里，选择一个函数<img src="http://latex.codecogs.com/gif.latex?g"/>作为学习的结果，要求<img src="http://latex.codecogs.com/gif.latex?g"/>要和<img src="http://latex.codecogs.com/gif.latex?f"/>足够相似，这样我们就说我们有真的学到东西。</p>
<h2>PART TWO: Perceptron Learning Algorithm(PLA)</h2>
<p>这一部分介绍了一个基础的学习算法，叫做Perceptron Learning Algorithm，翻译成中文即感知机学习算法。这里用二元分类举例，算法的目的是能够做出一个超平面（其实超平面很好理解，如果进行分类的数据是二维的，即数据都可以在一个二维平面上画下来，超平面就是这个平面上的一条线，同理如果分类数据是三维的，那么超平面就是三维空间里的一个平面，以此类推，n维空间里的超平面是一个n-1维的图形）将我们的数据分为两个部分， 分别对应我们二元分类的两种结果。</p>
<p>具体的模型如下图所示：</p>
<img src="https://raw.githubusercontent.com/longlongman/blog/gh-pages/images/NTU/NTU_PLA.JPG">
<p>这里老师是用了一个简单的例子——银行是否要向用户发放贷款，来介绍了我们的perceptron感知机模型。首先这里的问题显然是一个二元分类问题，可以归结为我们将用户划分为还贷能力良好和还贷能力不好的两类，对还贷能力良好的我们自然对其发放贷款，还贷能力不好的我们则要拒绝其带款请求。那怎么来评判一个人的还贷能力是好还是坏呢？最容易想到的是给这个人的还贷能力打分，如果分数超过一定数值（threhold，阈值）那么就是好，反之就是不好了，按照这样的思想，现在的重点应该是构建这个打分系统和选取这个阈值。</p>
<p>要给一个用户打分，我们势必是要知道用户的一些信息，在本例中我们知道用户的年龄、年收入、工作时间以及现在的负债状况等，于是我们用一个行向量来描述我们的用户，向量的每一维都对应着我们收集到的用户信息，每一个用户我们都描述成类似<img src="http://latex.codecogs.com/gif.latex?X=(x1,x2,...,xd)"/>的向量。这个向量里每一项都会影响到我们最后对用户的打分，但是很明显每一项的重要性是不相同的，有些项对问题十分重要，我们的贷款例子中，年收入和目前的负债就是十分重要的两项，为了表征这样的关系我们引进一个叫权重<img src="http://latex.codecogs.com/gif.latex?W"/>的量，这同样是一个向量，维数和<img src="http://latex.codecogs.com/gif.latex?X"/>一样。<img src="http://latex.codecogs.com/gif.latex?W"/>中每一项的值代表着对应<img src="http://latex.codecogs.com/gif.latex?X"/>中某项的重要性。</p>
<p>打分的过程实际就是让<img src="http://latex.codecogs.com/gif.latex?wi"/>和<img src="http://latex.codecogs.com/gif.latex?xi"/>相乘再求和。<p><img src="http://latex.codecogs.com/gif.latex?\sum_{i=1}^{d}wixi"/></p>如果计算出来的值大于我们的阈值（threhold）我们就说这个用户的还贷能力良好，反之则是不好。如果抛弃例子的情景，模型如下。<p><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/04228fc42b76b9ebcb067208e6129c3ccb735903"></p>在经过一点简单的变形，使得式子看起来简洁。<img src="https://raw.githubusercontent.com/longlongman/blog/gh-pages/images/NTU/NTU_PLA_2.JPG">
</p>