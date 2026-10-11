# 连接和使用 OpenAI 给出的 API

!!! warning "提示"

    本实验尚未正式完成，仅供参考.

    如果要完成这个实验，可以使用自己的 API（需要花钱）；也可以从助教（钉钉联系刘泓健或者邮箱 liu_h._jian@outlook.com，请使用浙大邮箱发送邮件并说明姓名班级）处获取一个余额不多的 API.

## 实验目的

1. 更进一步理解 Python 面向对象编程的特性；
2. 学习 openai 库的基本用法，了解如何作为开发者使用商业大模型；
3. 理解如何通过网络收发信息，了解报文的基本结构；
4. 了解大语言模型的初步知识，并对其能力获得直观的认识.

## 实验环境

- Python 3.x 环境
- openai 库（>= 1.0）

## 实验步骤

### 环境配置和安装

参考[环境配置指南](../00env.md)中的指导安装 openai 库. 在 Thonny 的工具提供的包管理器中可能会无法搜索到这个库，此时可以点击工具 -> 打开系统 Shell，然后输入

```powershell
python -m pip install openai
```

完成安装. 在环境配置指南中对这个过程有详尽的解释.

### 商用大模型基础知识和准备工作

首先，我们需要明确一个问题：在接下来的代码中，需要做的事情就是**通过网络**向所需的大模型发送请求，然后从中得到回应. 因此，要做的第一步是明确怎么发送消息. 我们将使用 openai 库中的接口完成这些操作. 因此，我们引入库中的 `#!python OpenAI` 对象：

```python
from openai import OpenAI
```

然后，正如我们访问网页时需要一个网址，在访问大模型的时候我们也需要告诉计算机我们需要访问的东西是什么. 在这里，我们使用的是 CloseAI 提供的服务，其对应的网址是

```
https://api.openai-proxy.org/v1
```

这里的 `v1` 是因为 API 是版本 1.0 所使用，所以加上的. 参照官方给出的说明，在使用 openai 的 API 时均需要加上这个后缀. 接下来，因为我们访问的模型是商用大模型，所以访问的过程中需要进行验证. 也就是说，网站需要确保是一个“已注册账号”在访问这个服务. 读者自己获得或者从助教处获得的秘钥应当形如：

```
sk-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

其中 `X` 应该是一串复杂的字符串. 有了这些信息，我们可以创建一个 OpenAI 类型的对象：

```python
client = OpenAI(
    base_url='https://api.openai-proxy.org/v1',
    api_key='sk-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX',
)
```

记得将其中的秘钥换成自己获得或助教提供的秘钥. 如此，我们就完成了所有的准备工作.

### 与大模型建立初步交流

在完成了上述步骤之后，我们已经获得了商用大模型的使用权. 接下来，我们需要向其发送信息. 发送的信息当然需要遵循大家在网络上收发信息的一般规律——我们称之为某种“协议”. 当然，openai 给我们提供了一个相对轻松的方法，我们只需要填写一部分信息，它就能给大模型发去格式正确的报文.

在准备工作当中，我们已经建立了 `#!python OpenAI` 类型的对象 `#!python client`，这个对象在创建的时候已经准备好了进行连接和通过认证的所有材料. 所以，接下来我们要进行各种操作，绕不开的就是 `#!python client` 这个对象. 这个对象有一个成员 `#!python chat`，它是一个 `#!python Chat` 类的对象，而它又有一个名叫 `#!python completions` 的 `#!python Completions` 类的对象. 这个 `#!python Completions` 类有一个方法称作 `#!python create`，它可以根据参数创建报文——是不是看的有点晕？但是作为代码，它告诉你的就是我们应该写下：

```python
client.chat.completions.create(
    ...
)
```

省略号中的内容就是我们接下来需要考虑的东西. 这个方法的类型说明如下：

??? info "点击展开冗长的类型说明"

    下面部分摘引自 [openai 包的源码](https://github.com/openai/openai-python/blob/89d49335a02ac231925e5a514659c93322f29526/src/openai/resources/chat/completions.py#L65C1-L104C25)：

    ```python
    def create(
        self,
        *,
        messages: Iterable[ChatCompletionMessageParam],
        model: Union[str, ChatModel],
        audio: Optional[ChatCompletionAudioParam] | NotGiven = NOT_GIVEN,
        frequency_penalty: Optional[float] | NotGiven = NOT_GIVEN,
        function_call: completion_create_params.FunctionCall | NotGiven = NOT_GIVEN,
        functions: Iterable[completion_create_params.Function] | NotGiven = NOT_GIVEN,
        logit_bias: Optional[Dict[str, int]] | NotGiven = NOT_GIVEN,
        logprobs: Optional[bool] | NotGiven = NOT_GIVEN,
        max_completion_tokens: Optional[int] | NotGiven = NOT_GIVEN,
        max_tokens: Optional[int] | NotGiven = NOT_GIVEN,
        metadata: Optional[Dict[str, str]] | NotGiven = NOT_GIVEN,
        modalities: Optional[List[ChatCompletionModality]] | NotGiven = NOT_GIVEN,
        n: Optional[int] | NotGiven = NOT_GIVEN,
        parallel_tool_calls: bool | NotGiven = NOT_GIVEN,
        prediction: Optional[ChatCompletionPredictionContentParam] | NotGiven = NOT_GIVEN,
        presence_penalty: Optional[float] | NotGiven = NOT_GIVEN,
        reasoning_effort: ChatCompletionReasoningEffort | NotGiven = NOT_GIVEN,
        response_format: completion_create_params.ResponseFormat | NotGiven = NOT_GIVEN,
        seed: Optional[int] | NotGiven = NOT_GIVEN,
        service_tier: Optional[Literal["auto", "default"]] | NotGiven = NOT_GIVEN,
        stop: Union[Optional[str], List[str]] | NotGiven = NOT_GIVEN,
        store: Optional[bool] | NotGiven = NOT_GIVEN,
        stream: Optional[Literal[False]] | NotGiven = NOT_GIVEN,
        stream_options: Optional[ChatCompletionStreamOptionsParam] | NotGiven = NOT_GIVEN,
        temperature: Optional[float] | NotGiven = NOT_GIVEN,
        tool_choice: ChatCompletionToolChoiceOptionParam | NotGiven = NOT_GIVEN,
        tools: Iterable[ChatCompletionToolParam] | NotGiven = NOT_GIVEN,
        top_logprobs: Optional[int] | NotGiven = NOT_GIVEN,
        top_p: Optional[float] | NotGiven = NOT_GIVEN,
        user: str | NotGiven = NOT_GIVEN,
        # Use the following arguments if you need to pass additional parameters to the API that aren't available via kwargs.
        # The extra values given here take precedence over values defined on the client or passed to this method.
        extra_headers: Headers | None = None,
        extra_query: Query | None = None,
        extra_body: Body | None = None,
        timeout: float | httpx.Timeout | None | NotGiven = NOT_GIVEN,
    ) -> ChatCompletion
    ```

!!! 类型说明及其规范

    在理解一个函数的功能时，其类型往往能给我们提供充足的信息. 一个函数或者方法在定义时，可以按照下面的方式说明类型：

    ```python
    def func(
        arg1: type1,
        arg2: type2 = default2,
        ...
    ) -> returntype
    ```

    其中 `type1` 是 `arg1` 的类型，`type2` 是 `arg2` 的类型，等等. 当然，其它东西并未受到影响，例如 `default2` 就是 `arg2` 的默认值. 另外，`returntype` 是函数返回值的类型.

    此外，类型之间可以进行组合. 在上面的说明中，我们看到了形如 `#!python type1 | type2` 的形式，它意味着这里可以填写 `type1` 或者 `type2` 类型的对象. 有些类型则被称作泛型（generics），它们可以被理解成一个接受一个类型，并返回另一个类型的“函数”. 例如，我们通常说的“整数构成的列表”，它就可以被写成 `#!python list[int]`. 关于 Python 中类型的更多知识，读者可以自行查阅官方文档.

在阅读这样的类型说明的时候，首先我们应该关注的是那些没有给出默认值的参数. 在这里，一共有两个参数没有给出默认值，分别是 `#!python messages` 和 `#!python model`. 顾名思义，`#!python model` 就是我们需要访问的模型——同一公司可能会给出不同版本的可访问模型，`#!python messages` 就是我们需要发送的信息.

在 `#!python model` 一栏中，我们可以填入需要使用的模型名称. 在这里，笔者使用 `gpt-4o` 为例. 不同的模型具备不同程度的能力，能够接受的信息，即接受的 `#!python messages` 的类型也有所不同. 下面我们先发送一个简单的文本信息，所有的大语言模型都可以理解它. `#!python messages` 的类型标注则是所有其中放了 `#!python ChatCompletionMessageParam` 的对象的可迭代对象（Iterables）.

至于一个信息当中应该包含哪些东西，我们就应该仔细阅读[模型官方给出的文档](https://platform.openai.com/docs/api-reference/chat/create#chat-create-messages)了. 在这里，我们发送用户信息（user message）. 官方文档告诉我们，它有两个必备的参数，一个是 `#!python content`， 一个是 `#!python role`，如果要发送用户信息，那么 `#!python role` 就应该是 `user`. 所以，我们以字典的形式创建信息：

```python
message = {
    "role": "user",
    "content": "Say hi!"
}
```

万事具备，只欠发送. 当然，我们需要发送的信息是一个可迭代对象，所以需要把 `#!python message` 套进一个列表里边：

```python
chat_completion = client.chat.completions.create(
    messages=[message],
    model="gpt-4o",
)
```
好了，现在打印出回收的信息吧：

```python
print(chat_completion)
```

咦，怎么得到了这么一大串？

```
ChatCompletion(id='chatcmpl-AhAja2ma5qOdsxmyS4ad9l0hufHz1', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='Hello! How can I assist you today?', refusal=None, role='assistant', audio=None, function_call=None, tool_calls=None))], created=1734852730, model='gpt-4o-2024-08-06', object='chat.completion', service_tier=None, system_fingerprint='fp_d28bcae782', usage=CompletionUsage(completion_tokens=10, prompt_tokens=10, total_tokens=20, completion_tokens_details=CompletionTokensDetails(accepted_prediction_tokens=0, audio_tokens=0, reasoning_tokens=0, rejected_prediction_tokens=0), prompt_tokens_details=PromptTokensDetails(audio_tokens=0, cached_tokens=0)))
```

!!! note "类型不相容警告"

    上面的代码执行时会报一个警告（warning）：

    ```
    List item 0 has incompatible type "dict[str, str]"; expected "ChatCompletionDeveloperMessageParam | ChatCompletionSystemMessageParam | ChatCompletionUserMessageParam | ChatCompletionAssistantMessageParam | ChatCompletionToolMessageParam | ChatCompletionFunctionMessageParam"  [list-item]
    ```

    这里的警告是一个类型不相容（incompatible type）警告. 我们已经知道，Python 并不强制要求类型符合标注，类型标注只不过是一个查错的手段. 因此，这样的警告不影响程序正常执行，虽然它违反了类型标注.

### 解析报文

到此为止，我们的代码如下：

```python
from openai import OpenAI

client = OpenAI(
    base_url='https://api.openai-proxy.org/v1',
    api_key='sk-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX',
)

message = {
    "role": "user",
    "content": "Say hi!"
}

chat_completion = client.chat.completions.create(
    messages=[message],
    model="gpt-4o",
)

print(chat_completion)
```

虽然得到了一大串信息，但是我们不难看出里边还是有一些我们真正需要的信息的. 模型发回的真正内容是：

Hello! How can I assist you today?

它被嵌在了一大堆信息当中. 其中最关键的是开头部分，它告诉我们这是一个 `#!python ChatCompletion` 类型的对象，这与 `#!python create` 方法的标注吻合. 这个类定义如下：

??? info "点击展开冗长的类定义"

    下面部分摘引自 [openai 包的源码](https://github.com/openai/openai-python/blob/89d49335a02ac231925e5a514659c93322f29526/src/openai/types/chat/chat_completion.py#L43C1-L77C55)：

    ```python
    class ChatCompletion(BaseModel):
        id: str
        """A unique identifier for the chat completion."""

        choices: List[Choice]
        """A list of chat completion choices.

        Can be more than one if `n` is greater than 1.
        """

        created: int
        """The Unix timestamp (in seconds) of when the chat completion was created."""

        model: str
        """The model used for the chat completion."""

        object: Literal["chat.completion"]
        """The object type, which is always `chat.completion`."""

        service_tier: Optional[Literal["scale", "default"]] = None
        """The service tier used for processing the request.

        This field is only included if the `service_tier` parameter is specified in the
        request.
        """

        system_fingerprint: Optional[str] = None
        """This fingerprint represents the backend configuration that the model runs with.

        Can be used in conjunction with the `seed` request parameter to understand when
        backend changes have been made that might impact determinism.
        """

        usage: Optional[CompletionUsage] = None
        """Usage statistics for the completion request."""
    ```

!!! note "带标注的赋值语句（annotated assignment statements）"

    读者可能会好奇，为什么在上面的类定义当中，只使用了类型标注（`#!python id: str`）就完成了对变量的定义，实际上，这种东西被称作[带标注的赋值语句](https://docs.python.org/3.13/reference/simple_stmts.html#annotated-assignment-statements). 虽然称作赋值语句，但是其中可以只有一个冒号和类型表达式，就能定义一个变量而不需要对它进行真正意义上的“赋值”操作.

仔细观察打印出来的内容，就会发现它实际上相当于把每个变量都打印了出来. 于是我们要做的就只剩下从中提取出有用的信息了. 因此，我们将打印改成：

```python
print(chat_completion.choices[0].message.content)
```

就能正确得到返回信息了. 这一步实际上丢失了很多看上去不太必要的信息，这些信息在调试之类的过程中可能是有用的，但是比较复杂，故不赘述.

### 编写命令行程序

完成了这些操作，我们就能构建一个可以完成对话的程序了. 它会读取用户输入，然后发送给大模型，然后获得输出：

```python
from openai import OpenAI

client = OpenAI(
    base_url='https://api.openai-proxy.org/v1',
    api_key='sk-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX',
)

while True:
    input_line = input()

    if input_line == "":
        break

    message = {
        "role": "user",
        "content": input_line
    }

    chat_completion = client.chat.completions.create(
        messages=[message],
        model="gpt-4o",
    )

    print(chat_completion.choices[0].message.content)
```

这里的设置是输入空行退出. 在这段代码的基础上，读者可以尝试进行更多的交互，或者对交互体验进行优化. 当然，也可以尝试给 gpt-4o 发送图片等等.

## 提交方式

我们希望你提交以下内容：

1. 实验代码，`py` 类型的文件，可以有多个.
2. 实验结果，即与大语言模型交流过程的截图.
3. 实验报告，包含对自己的代码的解释、结果分析，以及**过程中遇上的报错和任何遇到的好玩的问题，以及在查资料的时候发现的有趣的知识**，以 `pdf` 类型文件提交.

一般地，只需要提交一个 `zip` 文件即可. 你也可以尝试：

1. 修改代码中保存文件的路径，进而整理文件结构，代码、运行结果和文档分开.
2. 你也可以将你的尝试写成博客，发布到网上，然后提交可公开访问的链接.

作为一门编程语言类的课程，我们鼓励同学进行多种多样的尝试，因此，基于原始代码和功能之上的任何改动都是允许且被鼓励的. 但是，为了避免无意义的内卷，只需要完成基础功能、认真写完报告就能获得最高的分数，因此也不必勉强自己，尽情享受完成代码、修改代码的过程吧！

## 参考资料

- [OpenAI 的官方 API 参考](https://platform.openai.com/docs/api-reference/)
- [Python openai 库的实现](https://github.com/openai/openai-python/)
- [CloseAI 官网](https://platform.closeai-asia.com/)
- [Python 关于类型注解的提案](https://peps.python.org/pep-0484/)
