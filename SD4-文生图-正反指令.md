# SD4-文生图-正反指令

## 通过命令：.\webui-user.bat 运行

## .\web-user.bat 启动

![1719653221424](images/SD3-跑通一个小例子/1719653221424.png)

## 累加命令测试

### Tree

#### Code: tree

![1719658615140](images/SD4-text-to-image详细教程/1719658615140.png)

#### Code: a green tree

![1719658682304](images/SD4-text-to-image详细教程/1719658682304.png)

#### Code: a green tree without leaves

![1719658762764](images/SD4-text-to-image详细教程/1719658762764.png)

#### Code: a green tree without leaves and trunk

![1719658815611](images/SD4-text-to-image详细教程/1719658815611.png)

![1719658909768](images/SD4-text-to-image详细教程/1719658909768.png)

![1719658987190](images/SD4-text-to-image详细教程/1719658987190.png)

由此可见，SD模型对于一些训练数据集中不常见的，或者没有理解的物体是很难自主设计出来的。

Code: a green tree without leaves located in the sea

![1719659094891](images/SD4-text-to-image详细教程/1719659094891.png)

但是，SD模型对于一些训练数据集中少见的，或者奇特的但是能够理解的物体是可以自主设计出来的。

![1719659166149](images/SD4-text-to-image详细教程/1719659166149.png)

![1719659210293](images/SD4-text-to-image详细教程/1719659210293.png)

![1719659304191](images/SD4-text-to-image详细教程/1719659304191.png)![1719659301057](images/SD4-text-to-image详细教程/1719659301057.png)

## 增加生成目标对象的数量

### Code: two green trees without leaves located in the sea

![1719659352445](images/SD4-text-to-image详细教程/1719659352445.png)

![1719662454991](images/SD4-text-to-image详细教程/1719662454991.png)

![1719662508114](images/SD4-text-to-image详细教程/1719662508114.png)

![1719662527527](images/SD4-text-to-image详细教程/1719662527527.png)

可见，V1.5版本的模型对数量的理解能力不大。

---

## 添加反向命令：Negative Prompts

### 输入命令：

![1719662858067](images/SD4-text-to-image详细教程/1719662858067.png)

#### 正命令：a red tree without leaves located in the sea

#### 反命令：green leaves

![1719662784340](images/SD4-text-to-image详细教程/1719662784340.png)

![1719662938610](images/SD4-text-to-image详细教程/1719662938610.png)

![1719662973175](images/SD4-text-to-image详细教程/1719662973175.png)

![1719662997732](images/SD4-text-to-image详细教程/1719662997732.png)

![1719663050937](images/SD4-text-to-image详细教程/1719663050937.png)

通过多次尝试，V1.5版模型能够生成一定数量的，符合正反指令要求的图片。
