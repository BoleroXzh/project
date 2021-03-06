在本节中，我们通过回答以下问题来评估我们的实施：1）我们可以节省多少能量; 2）三种技术中的每一种节省了多少能量; 3）这些技术会影响页面加载时间和用户体验。

## 实验设置

#### 设备

我们对两款三星Galaxy S5智能手机进行了实验：S5-E和S5-S。 S5-E是SMG900H型号，采用三星Exynos 5422 SoC，具有4个CortexA15大内核和4个Cortex-A7小核心以及Linux内核3.10。 S5-S是采用四核Qualcomm Snapdragon 801 SoC和Linux内核3.4的SM-G900K型号。 两款智能手机都运行Android Kitkat 4.4.2和Chromium版本38.我们使用季风功率监视器工具[29]来测量智能手机的总系统能耗。

为了尽量减少测量噪音，我们删除所有不必要的应用程序和后台服务，禁用不相关的硬件组件（如相机，GPS和其他传感器），并关闭除WiFi之外的所有无线网络接口。 屏幕亮度设置为最低级别。

#### 试验台和网络条件

为了实现可重复的实验，为了避免不稳定蜂窝网络的噪声和Web服务器的不可预测的响应，我们构建了一个测试平台，以从本地模拟服务器加载网站。 Linux服务器使用网页重播[13]工具记录并重播网站的响应。 智能手机通过本地WiFi网络从服务器加载网页。 服务器使用dummynet网络仿真器模拟3G蜂窝网络[7]。 根据以前的3G蜂窝网络测量研究[40,26,1,27]，我们使用以下参数来模拟典型的蜂窝网络条件：2 Mbps下行链路带宽，1 Mbps上行链路带宽和120 ms往返时间（RTT）。

#### 数据集

2014年5月，根据Alexa [2]，我们使用美国排名前100位的网站评估了我们的技术。这100个网站包括多种网站，其中包括多种图像的复杂网站，以及大多数为文本内容的简单网站。 其中，88个网站采用移动设计。 默认情况下，我们在我们的实验中使用网站的主页。 对于需要用户登录的网站，我们在登录后记录并重播网站的响应，或从网站选择具有代表性的公共网页。 对于拥有如此琐碎主页的网站（如搜索引擎ask.com），我们选择一个不平凡的，具有代表性的二级网页。

#### 自动化工具

我们开发了一个工具来实现实验和数据收集的自动化。 该工具有两个模块，一个运行在智能手机上，另一个运行在运行季风电力监测软件的Windows PC上，以收集能耗数据。 在智能手机上，该工具是Chromium的内容外壳层（例如ShellManager类）上的模块。 它获取URL列表并控制Chromium逐一访问URL。 在访问每个URL之前，它会清除Chromium的缓存。 在加载网页期间，它收集有关页面加载的数据，例如页面加载时间。 实验完成后，它将所有收集的数据传输到PC。 在PC上，该工具控制Monsoon软件记录能耗数据。 该工具还使用电源轨迹上的合成下降沿精确地同步智能手机和PC的时间。 因此，我们可以将PC上收集的电源跟踪数据与智能手机上的页面加载数据对齐。

#### 页面加载时间

页面加载时间（PLT）度量自网页开始加载以来的时间，即导航开始事件，直到页面加载完成，即加载事件结束事件[24,23,22]。我们修改了Chromium以记录导航开始事件和加载事件结束事件的时间戳，因此我们的自动化工具可以计算每个单一网页的PLT。我们使用浏览器缓存和DNS缓存清除了冷页面加载时间。

除非另有说明，我们重复每个实验至少5次。通过在每个配置组合的页面加载时间值上应用修改的Z-评分[28]来检测并移除测量异常值。如果去除异常值后的剩余样本数量太小，我们会添加更多的实验。本节累积分布函数（CDF）图上的每个点都显示每个网站的平均值。为了估计变异，我们计算了每个平均值的95％置信区间，但因为大多数置信区间小于平均值的5％，所以这里不存在。

## 节约能源

#### 总节能

![Figure5](/image/Figure5.png)

我们首先评估所有三种技术共同实现的节能总量。 图5显示了结果。 在S5-E上，100个网站的平均总节能量为24.4％，范围从0.02％到66.5％不等。 62个网站节能15％-45％，7个网站节能50％以上。 这些结果表明我们的技术能够显着降低网页加载的能源成本。 我们还应该能够在Chrome浏览器和Opera Mobile等基于Chromium的网络浏览器上实现这种高节能。 最节能（66.5％）的网站是infusionsoft.com，它有一个沉重的幻灯片放映高分辨率图像与衰落过渡效应。 最少的节能网站是具有简单文本内容的网站，例如usps.com。

与S5-E智能手机相比，S5-S智能手机的节能量更小。 在S5-S上，平均节能量为11.7％，介于0.21％至57.1％之间。 66个网站节能5％30％，只有两个网站节能50％以上。 原因在于S5-S不支持big.LITTLE，因此无法从应用程序辅助调度中获益，这是本节后面将介绍的最节能的技术。

上述节能量是根据整个系统能源消耗（包括屏幕的能源成本）来衡量的，因为网络浏览器不会在屏幕关闭的情况下加载网页。 因此，为了评估屏幕功率的影响，我们使用单独的实验。 我们在屏幕打开和关闭的情况下运行实验两次。 然后将屏幕功率作为两种不同类型实验之间的系统功率差异进行测量。 两款智能手机使用相同类型的屏幕，屏幕功率为最低亮度级别186 mW。 剔除屏幕能源成本，S5-E的平均节能量从24.4％增加到26.6％，S5-S的平均节能从11.7％增加到13.0％。

#### 个别技术的节能

![Figure6](/image/Figure6.png)

接下来我们评估每种技术在多大程度上可以节省能源消耗。 图6显示了仅启用三种技术中的一种时的结果。 NRP，ACP和AAS分别代表节能资源缓冲，聚合内容绘制和应用辅助调度。

在S5-E上，最节能的技术是AAS，平均能耗降低19.5％。 其他两种技术也很有用，NRP和ACP的平均节能分别为10.2％和6.8％。 S5-S只能使用NRP和ACP，平均节能分别为6.8％和6.3％。 如果我们排除屏幕的能源成本，在S5-E上，NRP，ACP和AAS的平均节能分别为11.1％，7.4％和21.1％; 而在S5-S上，NRP和ACP的平均节能分别为7.6％和6.9％。 这些结果表明，所有这三种技术都能有效降低网页加载的能耗。

值得注意的是，对于一些网站来说，只启用其中一种技术可能会稍微增加系统能源成本。 例如，在S5-E上，如果只启用AAS技术，则四个网站的能源成本增加，从0.28％到3.0％不等。 S5-E和S5-S的最大能源成本增加分别为3.8％和5.6％。 然而，所有这三种技术（S5-S上的两项技术）都没有增加100个网站的能耗。

## 页面加载时间和用户体验

接下来，我们通过三种技术和每种技术的组合来评估PLT的增加程度，并使用速度指数[10]和用户研究来研究这些技术对用户感知体验的影响。

#### 总的页面加载时间增加

![Figure7](/image/Figure7.png)

我们的技术只需要额外的PLT。 图7显示了两款智能手机的结果（S5-E上启用了所有三项技术，S5-S上仅启用了两项技术）。 在S5-E上，平均PLT降低0.38％（或绝对PLT下降29 ms），范围从-8.11％至6.38％。 事实上，55个网站的PLT减少，93个网站的PLT增长不到3％。 就绝对PLT而言，94个网站的PLT增长不到0.2秒。 在S5-S上，平均PLT增加几乎为零，0.01％，从-4.1％到4.5％不等。 51个网站的PLT减少，94个网站的PLT增长低于3％。 就绝对PLT而言，平均PLT增加了6.7 ms，93个网站的PLT增长小于0.2秒。 我们认为，为了节省大量的能源，交易这个小的PLT是值得的。 特别是，我们确实能够减少100个网站中约一半的PLT。

#### 个别技术的页面加载时间增加

![Figure8](/image/Figure8.png)

![Table2](/image/Table2.png)

![Table3](/image/Table3.png)

我们还测量每种技术造成的PLT增加。 图8显示了结果。 正如人们所期望的那样，仅仅每种技术都会引入极少量的PLT。 在S5-E上，AAS引入平均PLT增幅最高，但增幅仅为0.23％，就绝对PLT而言甚至下降22.8 ms。 NRP和ACP的PLT增加分别为0.04％（-2.6毫秒）和-0.45％（-49.9毫秒）。 在S5-S上，NRP的PLT增加分别为0.76％（56.2 ms）和ACP的-0.25％（-24.5 ms）。 与其他两种技术相比，ACP对PLT的影响最小，因为它降低了两部手机的PLT。

表2和表3总结了S5-E和S5-S的平均节能量和PLT增加量。 值得注意的是，这三种技术不是完全独立的，因此总的节能量不等于个别技术的节能总和。

#### 对视觉完整进度的影响

![Figure9](/image/Figure9.png)

为了评估这些技术对网页视觉加载进度的影响，除了页面加载时间外，我们还测量了速度指数[10]。 该度量标准衡量页面内容的可视化填充速度，并按照以下公式计算：

$SpeedIndex=\int_0^{end}(1-\frac{VC}{100})$

结束时间是以毫秒为单位的结束时间，VC是视觉完成的百分比。 较低的速度索引更好，因为用户将看到较早绘制的网页内容，从而加快视觉加载进度。

我们使用遥测工具从Blink通过DevTools时间线公开的绘画事件中获取视觉效果。 与从视频捕捉中获得视觉效果相比，这种方法处理更新更好的页面[10,12]。 此外，它在我们的数据集中的网站上更可靠地工作，特别是带有动画的网站。

我们的技术平均增加了1.8％的指标，因此它们只会稍微延迟页面加载进度。 图9显示了与默认值相比，所有配置的速度索引增加。 我们报告了99个网站的结果，但conduit.com包含一个连续旋转的图像，而遥测工具报告我们的技术异常减少了42％。

#### 用户感知的体验

![Figure10](/image/Figure10.png)

为了进一步评估增加的PLT对用户感知体验的影响，我们开展了一项用户研究，以收集真实用户对我们在真实世界环境中的技术的反馈。因此，我们在我们大学招募了18名本科生，男性11人，女性7人，并在真实世界的WiFi网络上使用S5-E智能手机进行了两项测试。在第一个测试中，我们使用默认的Chromium和我们修改后的Chromium来加载从前100个网站中随机选择的10个网页。每个网站都使用默认的Chromium或我们修改过的Chromium浏览器加载。我们显示加载到用户的网页，但不让他们知道使用哪个Chromium版本。用户可以要求重复加载任何网页多次他/她想要的。然后，我们问他们，在页面加载速度和平滑度方面，我们修订后的Chromium是好于还是差于默认Chromium，使用七个等级（从3到-3更好，更好，更好，相同，更少更糟，更糟糕，分别更糟糕）。在第二项测试中，我们让用户以他们最喜欢的方式进行网页浏览，分别使用每个浏览器版本5分钟。用户也不知道使用哪个Chromium版本。此测试不仅包括页面加载，还包括页面阅读和用户交互（例如滚动）。然后，我们要求他们在页面加载速度和整体速度方面再次使用相同的七级比较两个浏览器。

图10显示了结果。对于第一次测试，18位用户中有4位（22％）告诉他们很难确定哪种浏览器更快。 10位（55％）用户甚至在我们修改后的浏览器上获得了更高的分数。为了保持平稳，大多数用户无法区分差异。页面加载速度和平滑度的平均得分几乎为零：0.04（标准差（stdev）1.30）和-0.02（标准差1.14）。在第二次实际使用测试中，我们得到了类似的结果：7位（39％）的用户告诉我们修改后的浏览器在加载页面时速度更快，5位（28％）用户告诉我们修改后的浏览器总体速度更快。页面加载速度和总体速度的平均得分也接近于零：-0.12（标准差1.45）和-0.18（标准差1.38）。这些结果表明，我们的技术对用户体验的影响最小，用户很难感受到这样的小影响。测试结束后，我们询问用户他们想如何使用我们的技术。其中13人（72％）总是使用我们的技术，其中5人（28％）会在电池电量不足时使用。

## 节能分析：案例分析

为了进一步调查我们提出的技术如何降低能耗，我们提供了infusionsoft.com作为案例研究的详细分析，因为它是每种技术最节能的网站。 实质上，我们的技术通过降低系统功耗和CPU利用率来降低总能耗。 我们检查在S5-E智能手机上获得的结果。

#### 系统功耗降低

![Figure11](/image/Figure11.png)

由于我们的技术在降低总消耗能量的同时引入了PLT的最小增加，我们实际减少的是系统功耗。 图11显示了加载infusionsoft.com（具有最高节能率的网站（66.5％））中的电源轨迹。 我们的技术能够显着降低系统功耗。 在图中5.0秒至17.0秒的时间段内，使用默认铬的平均系统功耗为5.28 W; 启用我们的技术后，平均系统功耗降至1.44 W.系统功耗的显着降低来自CPU利用率的降低，如下所示。

#### NRP的CPU利用率降低

![Figure12](/image/Figure12.png)

图12显示了使用默认的Chromium加载infusionsoft.com时主线程的CPU时间，并且仅在启用了NRP技术的情况下使用了我们修订的Chromium。当启用NRP时，Chrome_ChildIOThread线程的CPU时间从2.42秒显着减少到0.84秒，或减少了65.3％。这是因为NRP旨在减少浏览器进程和渲染器进程之间的IPC。 NRP还可以减少CompositorRasterWorker线程的CPU时间（从8.68秒1.83秒到78.9％），Compositor线程（从3.58秒到2.14秒或40.2％），Chrome_InProcGpuThread线程（从2.26秒到1.29秒或42.9秒％）和AsyncTransferThread线程（从2.94秒到1.86秒，或36.7％）。这是因为增加每个处理的数据大小减少了对网页图层更改的数量，从而减少了内容呈现的数量。这样可以显着减少渲染引擎和图形处理的CPU处理时间。

#### ACP的CPU利用率降低

![Figure13](/image/Figure13.png)

图13显示了仅启用ACP技术的结果。 当启用ACP时，以下四个线程的CPU时间显着减少：对于Compositor线程，从3.58秒到1.87秒（47.8％），对于Chrome_InProcGpuThread线程，从2.26秒到1.41秒（37.5％），对于CompositorRasterWorker 线程从8.68秒到6.25秒（28.0％）。 这些结果证明了ACP在降低图形处理流水线处理成本方面的有效性。

#### AAS的CPU利用率降低

![Figure14](/image/Figure14.png)

AAS显着降低了大型内核的工作负载，同时提高了用于节能处理的小型内核利用率 图14显示了加载infusionsoft.com时小型和大型集群的利用情况。 在默认的Chromium中，小核心和大核心的平均利用率分别为25.2％和40.9％。 使用AAS，小型核心的平均利用率增加到60.1％，而大型核心的平均利用率仅下降到6.1％。

## 在其他Web浏览器上节能

![Table4](/image/Table4.png)

为了演示其他Web浏览器的技术适用性，我们在Firefox的Android开源Web浏览器上实现了ACP和AAS技术。在前100个网站中，我们的实验是在72个使用HTTP协议的网站上进行的。这是因为Firefox建立到使用自签名加密证书的Web页面重放服务器的HTTPS连接是有问题的。我们在撰写本文时使用Firefox 38版本，这是最新版本。对于ACP，CompositorParent类中只有50行代码负责Firefox中的图形组成。 AAS的实施涉及线程管理模块的更改。由于NRP技术需要在Firefox的网络堆栈中大量实施全新的缓冲层，因此我们将其留作未来工作。

使用ACP和AAS技术，经过修订的Firefox网页浏览器可以节省不少重要的能源，同时最大限度地减少页面加载时间。在S5-E上，两种技术均可节省平均系统能量的10.5％，同时增加1.69％（180毫秒）的PLT。对于每种技术，ACP的平均系统节能为9.5％，AAS为3.3％。 ACP的PLT增幅为-0.46％，AAS为0.2％。结果如表4所示.AAS技术不能在Firefox上节省大量能源的原因是AAS技术试图将Compositor线程放到小内核中。但是，尽管线程比其他线程具有更高的CPU利用率，但CPU负载仍然很轻。因此，与Compositor线程以尽可能小的频率在大内核上运行的情况相比，将Compositor线程放到一个小内核上并不能节省太多能量。

## 快速网络节能

![Table5](/image/Table5.png)

我们还评估了我们的技术在诸如4G蜂窝和WiFi等快速网络上可以节省多少能量。 我们模拟了一个具有20 Mbps下行链路带宽，10 Mbps上行链路带宽和50 ms RTT的快速网络。 表5显示我们的技术在S5-E和S5-S上分别节省系统能量的21.8％和6.0％。 剔除屏幕能量，我们的技术分别在S5-E和S5-S上分别节省23.4％和6.5％。 PLT甚至下降了0.06％（S5-E）和0.92％（S5-S）。 PLT的减少可能来自两个原因：1）当网络速度更快时，NRP引入的额外延迟变得更小; 2）由于AAS更好地利用了小内核，因此它为渲染引擎留下了更多的大内核容量，从而使内容渲染速度更快。

## 热负荷下的节能

![Figure15](/image/Figure15.png)

我们评估我们的技术在使用缓存内容加载网页时的性能。 我们测试热网页加载，网页浏览器在冷负载后立即再次加载网页。 因此，许多资源仍然在Web浏览器缓存中，DNS查找结果仍然在OS的本地DNS缓存中。 我们使用具有2 Mbps下行链路，1 Mbps上行链路带宽和120 ms RTT的仿真WiFi网络。

我们的技术在降低热载系统能耗方面仍然有效。 S5-E上所有配置的结果如图15所示。平均系统能量减少了19.6％，平均页面加载时间减少了1.73％（或绝对页面加载时间为67毫秒）。 页面加载时间的减少与快速网络中的解释类似。

## 3G网络节能

![Figure16](/image/Figure16.png)

考虑到蜂窝网络接口的功耗特性与WiFi不同，我们评估了在3G网络上运行时我们的技术节能情况[36]。对于可重复的实验，网页从我们的网页重放服务器加载。服务器通过2 Mbps下载和1 Mbps上传节流带宽，同时不增加任何延迟。鉴于3G（HSPA +）网络足够快，服务器端的带宽有限。该延迟不受3G网络控制和确定。我们使用KT公司提供的3G网络，手机只有3G接口（其WiFi接口关闭）。

图16显示了S5-E上所有配置的结果。我们的技术平均系统节能22.5％，平均PLT增加0.41％（绝对页面加载时间27毫秒）。使用蜂窝网络接口时的平均系统节能仍然很高，但比使用WiFi网络接口（24.4％）低约2％。鉴于每个网页的下载数据量并不大（例如，在我们的数据集中平均为1.1 MB），CPU上的处理通常占据整个系统的功耗。测量的能量是包括网页加载时间期间的3G尾部能量的总系统能量，但不包括页面加载完成后的尾部能量。