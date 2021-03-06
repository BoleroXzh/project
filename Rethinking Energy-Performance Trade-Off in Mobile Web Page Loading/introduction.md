网络浏览是智能手机和其他移动设备（如平板电脑）的核心应用程序之一。 然而，网页浏览，特别是网页加载，具有高能耗。 由于能源是智能手机最关心的问题，因此希望提高网页浏览的能效，特别是网页加载。 在本文中，我们试图在不影响用户体验的情况下降低智能手机上网页加载的能耗。 特别是，我们的目标不是增加页面加载时间。

为了实现这一目标，我们研究了浏览器内部和系统行为，以了解如何将能量用于加载网页，并找出提高能效的机会。 尽管许多浏览器制造商都在努力提高移动设备的能效，但我们的调查结果表明，目前的移动浏览器尚未完全针对网页加载进行能源优化。 首先，网络资源处理是积极进行的，不管网络条件如何，都存在能源效率低下的风险。 其次，内容绘画率不必要地高，消耗大量能量而没有带来用户可感知的好处。 最后，随着新兴的ARM big.LITTLE架构[3]，现代CPU的节电能力未得到充分利用。 从根本上说，网页加载过度优化性能，但不是能源成本。

我们认为，对于移动设备上的网页加载，必须重新考虑能源性能的权衡。基于我们的研究结果，我们制定了节能网页加载的新设计原则。基于这些原则，我们开发了三种新技术，每种技术解决上述能效问题之一。首先，我们建议使用网络感知资源处理（NRP）技术来有效折衷性能，以适应不断变化的网络条件进行能源减少。我们使用自适应资源缓冲技术来动态控制资源下载速度，从而在不增加页面加载时间的情况下提高能源效率。其次，我们提出了自适应内容绘制（ACP）技术，以避免不必要的内容涂料以减少能源开销。我们研究节能和页面加载时间增加之间的权衡，以确保用户体验不会受到影响。最后，为了更好地利用big.LITTLE架构，我们提出了应用辅助调度（AAS）技术，以利用浏览器的内部知识来制定更好的调度决策。具体来说，我们采用基于QoS反馈的自适应线程调度，只要满足相关QoS要求，浏览器就可以让线程在小内核上运行以节约能源。

我们通过修改Chromium浏览器在商业智能手机上实施了所有这三项技术。总体而言，这些技术大大降低了网页加载的能源成本。使用美国Alexa排名前100的网站进行的实验评估表明，在使用WiFi的big.LITTLE智能手机上，我们修订后的Chromium浏览器能够实现平均24.4％的系统节能，同时减少0.38％的页面加载时间，而默认的Chromium浏览器。在使用3G时，平均系统节能为22.5％，平均页面加载时间为0.41％。在另一款没有big.LITTLE支持的智能手机上，我们的技术在使用WiFi时平均可以减少11.7％的能耗。我们还进行了用户研究，以确认我们的技术不会影响用户的感知体验。此外，所有参与者都表明他们的意图（或兴趣）总是使用我们的技术（72％）或电池电量不足（28％）。

尽管我们的实施基于Chromium，但我们提出的技术也可以应用于其他移动浏览器。 Chromium的前两个能源效率问题与Web内容下载，处理和绘画的一般程序有关。 因此，其他移动浏览器通常会遇到相同的问题，但具有不同的重要级别。 除Chromium之外，我们还将Firefox的ACP和AAS技术应用到Firefox，即使没有对Firefox内部的深入了解，也导致平均节省大量的能源（10.5％），页面加载时间略微增加（1.69％） ，与默认的Firefox相比。



本文的主要贡献是：
1.我们表明，在流行的移动网络浏览器（包括Chrome，Firefox，Opera Mobile和UC Browser）上加载的网页过度优化了性能，但不是为了节能，并找出了降低能源成本的机会。
2.我们推导出设计原则并提出技术来提高智能手机上网页加载的能效。
3.我们通过修改Chromium浏览器来实施我们的技术，展示我们的技术在实际移动浏览器中的适用性。 我们还在Firefox浏览器中实施了建议的ACP和AAS，表明我们的发现可以应用于其他移动浏览器。
4.我们进行全面的实验和用户研究，以显示我们的技术有效降低移动网页加载的能耗，同时不会影响用户体验。

本文的其余部分安排如下。 第2节介绍了Chromium浏览器和big.LITTLE体系结构作为背景。 第3节报告了能源效率低下问题，并提出了降低能源成本的一般准则。 第4节描述了我们的新技术。 第5部分显示了实施细节，第6部分显示了评估结果。 第7节讨论节能技术和未来工作的更多方面，第8节调查相关工作，第9节结束。