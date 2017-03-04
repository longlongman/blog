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
<p>以上对数据点用线性的方法打分，并根据得分进行分类的做法，就是我们的perceptron（感知机，名字叫得这么高大上...），在我们使用感知机进行学习的时候，最重要的就是得到一条线，将我们的两个类别的点分开，也就是说我们要找到一个合适的<img src="http://latex.codecogs.com/gif.latex?W"/>向量，由它在平面形成的线正好分开了两个类别。</p>
<p>既然要找到一个好的<img src="http://latex.codecogs.com/gif.latex?W"/>向量，那么问题就来了，怎么找，这里我们就要说PLA了。</p>
<p>我们在这里用一个二维的例子来说明PLA是怎样运作的。假设我们有这样一些数据，我们将它们画在一个平面上，如下图。</p>
<img src="https://raw.githubusercontent.com/longlongman/blog/gh-pages/images/NTU/NTU_PLA_3.JPG">
<p>首先让我们思考一下有几种方法来找到这条线，一种是看天吃饭，我一条条的去试，这条不好，我们就下一条，但是通常我们的线有无数条，这样的方法明显不合理，而且这种方法能不能停机都是问题，更别说寻找到我们想要的那条线了，另一种是先天不足，后天弥补的方法，我先随机一条线，这条线通常是不能做好我们的分类的，但是我们想办法一步步去调整这根线，最后让它满足我们的要求，看起来这个方法合理多了，但是如何去调整这根线，以及这个方法最后是不是能停机都需要讨论，但是“先天不足，后天弥补”就是我们PLA最核心的算法思想。</p>
<img src="https://raw.githubusercontent.com/longlongman/blog/gh-pages/images/NTU/NTU_PLA_4.JPG">
<p>上面是林轩田老师用形象的方法告诉我们要怎么更新我们的<img src="http://latex.codecogs.com/gif.latex?W"/>向量。如果我们一开始的<img src="http://latex.codecogs.com/gif.latex?W"/>向量是不好的，那么我们必定会找到至少一个点，我们在这个点上犯了错，所谓的犯错在这里就是最后算出来分数的符号所对应的分类和我们真正的分类是不一样的。在这里的二元分类问题中我们可能会犯两种错误，一种是我们的<img src="http://latex.codecogs.com/gif.latex?y"/>是+1而最后我们给出的符号却是负的，则说明我们的<img src="http://latex.codecogs.com/gif.latex?W"/>向量与<img src="http://latex.codecogs.com/gif.latex?X"/>向量之间的夹角过大，我们用<img src="http://latex.codecogs.com/gif.latex?W+yX"/>来更新<img src="http://latex.codecogs.com/gif.latex?W"/>向量可以缩小<img src="http://latex.codecogs.com/gif.latex?W"/>向量和<img src="http://latex.codecogs.com/gif.latex?X"/>向量之间的角度，也许就可以修正我们在这一点上的错误了。另外一种错误则是我们的<img src="http://latex.codecogs.com/gif.latex?y"/>是-1，而预测却给了+1，处理方法是类似的。我们一直做这样的动作，知道我们发现已经没有点犯错了，就说明我们找到了我们想要的那条线了。到这里我们对PLA已经有了一个比较清晰的认识了，不过这里面还有一些细节问题，我会放到后面的来写。</p>
<h2>PART THREE: Type of Learning</h2>
<p>这一部分是关于机器学习到底有几种类别的讲解，主要是感性的认识，对所学东西有一个简单的了解。这一部分的写作主要是参考了马孔多在<a href="https://www.douban.com/note/319684224/" target="_blank">豆瓣</a>上的笔记，有兴趣的朋友也可以去看看。</p>
<p>用不同的分类标准来讨论我们的机器学习，可以使我们从各个方面对它有一个清楚的认识。我们主要会用一下几个标准来进行分类：
<ul>
	<li>一、根据输出空间分类</li>
	<li>二、根据数据标签的情况分类</li>
	<li>三、根据不同的协议来分类</li>
	<li>四、根据输入空间来分类</li>
</ul></p>
<p>
	<h3>根据输出空间分类</h3>
	<p>输出空间按大类来分主要有两类，一种输出<b>离散</b>的值，一种输出<b>连续</b>的值，一般我们把输出离散值的机器学习问题叫做<b>分类问题</b>，而那些输出连续值的，我们则称它们为<b>回归问题</b>。</p>
	<p>在分类问题中，又跟据所分类别的数量又可以分为<b>二值分类</b>和<b>多值分类</b>，就是你是要将东西分成两类还是多类。不同于分类问题，在解决回归问题时我们最后通常需要给出一个可以在一个<b>连续区间乃至正负无穷之间变化</b>的实数，这种问题我们生活中经常遇到，比如房价预测和天气预报就是典型的回归分析的应用。</p>
	<h3>根据数据标签情况分类</h3>
	<p>这里可以分为<b>有监督学习</b>、<b>无监督学习</b>、<b>半监督学习</b>、<b>增强学习</b>，这里有一句话总结得非常精炼，有监督学习有所有的标签，无监督学习没有标签，半监督学习只有少量标签，而增强学习则是只有隐式的标签。</p>
	<h3> 根据不同的协议来分类</h3>
	<p>主要分<b>批量学习</b>、<b>在线学习</b>、<b>主动学习</b>，其中所谓的批量指的是利用已有的训练数据来学习，是一个希望一次学到位的策略，在线学习则是通过序列化地接受数据来学习，是希望慢慢累积知识，逐渐提高性能的方法，最后的主动学习是开始只有少量的标签，通过有策略地“问问题”来提高性能。</p>
	<h3>通过输入空间来分类</h3>
	<p>可以分为<b>具体特征</b>、<b>原始特征</b>、<b>抽象特征</b>。具体特征中通常包含人类的智慧，由我们人类来选择事物的特征，然后再让机器学习；原始特征则是不由我们人类来做选择特征，而是我们输入原始的数据，机器自动将其转化为具体特征；抽象特征通常没有任何真实意义，更需要认为地进行特征转化、抽取和再组织。总结：具体特征具有丰富的自然含义；原始特征有简单的自然含义；抽象特征没有自然含义。原始特征、抽象特征都需要再处理，此过程成为特征工程，是机器学习、数据挖掘中及其重要的一步。离散特征一般只需要简单选取就够了。</p>
</p>