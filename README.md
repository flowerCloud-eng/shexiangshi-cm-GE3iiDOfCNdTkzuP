[合集 \- 玩转Streamlit(14\)](https://github.com)[1\.什么是Streamlit10\-11](https://github.com/wang_yb/p/18458062)[2\.『玩转Streamlit』\-\-环境配置10\-17](https://github.com/wang_yb/p/18471660)[3\.『玩转Streamlit』\-\-架构和运行机制10\-22](https://github.com/wang_yb/p/18492213)[4\.『玩转Streamlit』\-\-多页应用10\-25](https://github.com/wang_yb/p/18502232)[5\.『玩转Streamlit』\-\-页面布局10\-31](https://github.com/wang_yb/p/18516928)[6\.『玩转Streamlit』\-\-登录认证机制11\-05](https://github.com/wang_yb/p/18527320)[7\.『玩转Streamlit』\-\-文本与标题组件11\-07](https://github.com/wang_yb/p/18531821):[milou云加速器官网](https://jiechuangmoxing.com)[8\.『玩转Streamlit』\-\-数据展示组件11\-13](https://github.com/wang_yb/p/18543687)[9\.『玩转Streamlit』\-\-图像与媒体组件11\-16](https://github.com/wang_yb/p/18549720)[10\.『玩转Streamlit』\-\-交互类组件11\-19](https://github.com/wang_yb/p/18554142)[11\.『玩转Streamlit』\-\-布局与容器组件11\-23](https://github.com/wang_yb/p/18564468)[12\.『玩转Streamlit』\-\-可编辑表格11\-29](https://github.com/wang_yb/p/18576592)[13\.『玩转Streamlit』\-\-表单Form12\-04](https://github.com/wang_yb/p/18585991)14\.『玩转Streamlit』\-\-片段Fragments12\-10收起
在 `Streamlit` 应用开发中，`Fragments`组件是一种用于更精细地控制页面元素更新和显示顺序的工具。


它允许开发者将内容分解成多个小的片段，这些片段可以按照特定的顺序或者逻辑进行更新，而不是一次性地更新整个页面或容器中的所有内容。


这为创建动态且交互性强的用户界面提供了更多的灵活性和控制力。


# 1\. 概要


`Fragments`有点像`Web2.0`时的`Ajax`技术，它能够把页面内容拆分成多个小片段，就像是把一幅完整的画分成了许多小拼图。


这样的好处是，`Fragments` 可以对更新操作进行细分，仅更新部分内容，提升页面响应速度。


从本质上说，它为开发者在构建动态、交互性强的应用界面时，提供了更高的灵活性和精准的内容控制能力。


`Fragments`组件的参数不多，使用时一般通过`st.fragment`装饰器来编写小片段，后续会通过示例来演示。




| **名称** | **类型** | **说明** |
| --- | --- | --- |
| func | 函数对象 | 将其转换为片段的函数，`@st.fragment`装饰器所装饰的函数 |
| run\_every | 整数、浮点数、时间间隔、字符串或None | **None**（默认值）：片段仅在用户触发事件时重新运行。**整数或浮点数**：指定以秒为单位的时间间隔，例如5表示每 5 秒自动重新运行片段。**字符串**：指定时间格式，如"1d"（1 天）、"1\.5 days"（1\.5 天）或"1h23s"（1 小时 23 秒），该格式被 Pandas 的Timedelta构造函数支持。**timedelta对象**（来自 Python 的内置datetime库）：如timedelta(days\=1\)表示每天自动重新运行片段。 |


# 2\. Fragments 和 Form 区别


`Fragments`和上一篇介绍的`Form`看起来很类似，都是将多个关联的组件组织起来，统一更新和管理。


实际上，它们的应用场景和工作方式区别很大，了解其中的区别，可以让我们更好的选择相应的组件。


## 2\.1\. 主要用途


从**用途**上来看，`Fragments`主要用于：


* **实现引导式内容展示**：用于创建引导性的应用界面，以逐步展示信息。例如，在一个数据分析应用的教程中，先使用`Fragments`展示数据加载的步骤，然后再展示数据分析方法的介绍。
* **优化页面更新性能**：在处理大量数据或复杂 UI 更新时，`Fragments`可以将更新操作拆分成多个小片段更新。每次只刷新必要的部分，提高应用的响应速度。比如，在一个实时数据监控应用中，使用`Fragments`可以分别更新不同数据图表的部分，而不是一次性更新整个页面的所有图表。
* **构建复杂交互逻辑**：对于具有复杂交互逻辑的应用，`Fragments`能够帮助组织和控制页面元素的显示与隐藏。例如，在一个多步骤的操作流程应用中，通过`Fragments`管理每个步骤中不同操作按钮和提示信息的显示和隐藏。


而`Form`则主要用于：


* **数据收集**：主要用于收集用户输入的数据。这可以是简单的文本信息，也可以是复杂的选择，如在一个产品配置表单中，用户通过下拉菜单选择产品型号、颜色等选项。
* **数据验证和提交**：表单通常包含数据验证机制，以确保用户输入的数据符合要求。并且，表单提供了**提交功能**，将收集到的用户数据发送到服务器或者进行本地处理。


## 2\.2\. 工作方式


从组件的**工作方式**来看，`Fragments`本身并不具有像表单那样固定的结构。


它更像是一个容器，可以容纳各种 `Streamlit` 组件，如文本、按钮、图表等。可以通过代码逻辑来控制这些组件在`Fragments`中的显示顺序和条件。


而**表单**具有比较明确的结构，通常包含`form`标签（在 HTML 层面）和一系列的输入组件，如`st.text_input`、`st.selectbox`等。


表单中的所有输入组件通常是相互关联的，它们共同构成了一个数据收集单元。


而且，表单可以通过`st.form_submit_button`来触发提交操作，并且可以使用`st.form`上下文管理器来确保表单内的组件数据在提交时能够正确地一起处理。


## 2\.3\. 数据处理


在**数据处理**方面，`Fragments`相对灵活，例如，在一个包含多个`Fragments`的应用中，


每个`fragment`可能有自己独立的按钮，点击按钮后的既可以更新当前`fragment`，也可以触发其他`fragment`的更新。


数据交互更多地体现在不同`fragment`之间的切换和内容更新上。


而`Form`在数据处理主要围绕用户输入的数据，在表单提交后，通常会对收集到的数据进行验证、清洗和存储等操作。


例如，将用户在表单中输入的注册信息发送到数据库进行存储，或者根据用户在表单中选择的查询条件从数据库中获取数据并显示。


表单内的交互主要集中在用户输入和提交操作上，以及根据输入数据的合法性给予用户相应的反馈，如提示输入错误信息等。


# 3\. Fragments使用示例


下面通过两个根据实际情况简化的示例来演示`Fragments`的使用场景。


## 3\.1\. 逐步显示信息


在这个示例中，我们创建了一个产品介绍页面，通过`Fragments`组件将信息分成三个步骤逐步显示。


首先显示欢迎信息，当用户点击 【**点击了解更多**】按钮后，显示产品功能信息，


再点击【**继续下一步**】按钮后，显示产品优势信息。



```
import streamlit as st

@st.fragment
def step1():
    st.write("欢迎来到我们的产品介绍页面。")
    if st.button("点击了解更多", key="step1"):
        step2()


@st.fragment
def step2():
    st.write("这是我们产品的主要功能：功能 1、功能 2、功能 3。")
    if st.button("继续下一步", key="step2"):
        step3()


@st.fragment
def step3():
    st.write("最后，这是我们产品的优势所在，如高效性、易用性等。")
    if st.button("刷新", key="step3"):
        st.rerun()


# 初始显示第一个片段
step1()

```

![](https://img2024.cnblogs.com/blog/83005/202412/83005-20241210114714756-847674151.gif)


## 3\.2\. 局部刷新页面


这个示例模拟一个数据展示大屏，大屏被分成两列，分别展示**销售数据**和**流量数据**。


每个数据区域都有自己的刷新按钮，点击按钮时，只会重新运行对应的 `fragment` 函数，从而实现该区域数据的局部刷新，不会影响其他部分。



```
import streamlit as st
import random


# 模拟获取一些数据
def get_sales_data():
    return random.randint(100, 1000)


def get_traffic_data():
    return random.randint(50, 500)


# 创建销售数据展示片段
@st.fragment
def sales_data_fragment():
    st.subheader("销售数据")
    # 获取并显示销售数据
    sales_data = get_sales_data()
    st.write(f"今日销售额: {sales_data}")
    # 销售数据的局部刷新
    if st.button("刷新销售数据", key="sales"):
        st.rerun(scope="fragment")


# 创建流量数据展示片段
@st.fragment
def traffic_data_fragment():
    st.subheader("流量数据")
    # 获取并显示流量数据
    traffic_data = get_traffic_data()
    st.write(f"今日流量: {traffic_data}")
    # 流量数据的局部刷新
    if st.button("刷新流量数据", key="traffic"):
        st.rerun(scope="fragment")


# 主函数，在大屏上排列各个片段
def main():
    # 将屏幕划分为两列，分别放置不同的片段
    col1, col2 = st.columns(2)
    with col1:
        sales_data_fragment()
    with col2:
        traffic_data_fragment()


if __name__ == "__main__":
    main()

```

![](https://img2024.cnblogs.com/blog/83005/202412/83005-20241210114714597-1754554110.gif)


# 4\. 总结


最后，总结下应该在什么时候可以考虑选择`Fragments`来构建我们的`streamlit`应用。


首先，需要**逐步显示信息**时，可以考虑使用`Fragments`。


例如在创建一个教程或者引导性的应用界面时，先显示一部分说明文字，然后在用户进行某个操作（如点击按钮）后再显示下一步的内容，而不是一次性将所有教程信息都呈现给用户，避免信息过载，影响用户的理解和操作体验。


其次，**优化页面更新性能**时，可以考虑使用`Fragments`。


比如，当应用中有大量数据或者复杂的 UI 更新时，`Fragments`组件可以将更新操作拆分成多个小的片段更新。这样，在每次更新时只刷新必要的部分，减少了页面重新渲染的工作量，从而提高应用的响应速度和性能。


还有**构建复杂交互逻辑**时，也可以考虑使用`Fragments`。


在构建具有复杂交互逻辑的应用时，`Fragments`组件能够帮助开发者更好地组织和控制页面元素的显示与隐藏。


