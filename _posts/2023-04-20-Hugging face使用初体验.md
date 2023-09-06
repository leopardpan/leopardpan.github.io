---
layout: post
title: "Hugging face使用初体验"
date: 2023-04-20
description: "Hugging face使用初体验"
tag: 工具箱
---
## 1. 什么是Hugging face

Hugging Face Hub和 Github 类似，都是Hub(社区)。Hugging Face可以说的上是机器学习界的Github。Hugging Face为用户提供了以下主要功能：

* [模型仓库（Model Repository）](https://huggingface.co/docs/hub/repositories-getting-started)：Git仓库可以让你管理代码版本、开源代码。而模型仓库可以让你管理模型版本、开源模型等。使用方式与Github类似。
* [模型（Models）](https://huggingface.co/docs/hub/models)：Hugging Face为不同的机器学习任务提供了许多[预训练好的机器学习模型](https://huggingface.co/models)供大家使用，这些模型就存储在模型仓库中。
* [数据集（Dataset）](https://huggingface.co/docs/hub/datasets)：Hugging Face上有许多公开数据集。

## 2. Hugging face的使用

hugging face在NLP领域最出名，其提供的模型大多都是基于Transformer的。

你可以用它做以下事情：

* 直接使用预训练模型进行推理
* 提供了大量预训练模型可供使用
* 使用预训练模型进行迁移学习

### Transformers安装

安装Transformers非常简单，直接安装即可。

```shell
pip install transformers
1
```

### 使用Transformers进行推理

如果你的任务是一个比较常见的，大概率可以直接使用Transformer提供的Pipeline解决。

比如说这，[Pipeline——transformers/v4.21.0/en/main_classes/pipelines](https://huggingface.co/docs/transformers/v4.21.0/en/main_classes/pipelines)API解决，其使用方式非常简单，可以说是直接用即可。

```python
from transformers import pipeline

translator = pipeline("translation_en_to_fr")
print(translator("How old are you?"))
```

### 如何搜索模型

首先，我们需要到来到官网的[模型模块](https://huggingface.co/models)。

其主要包含三部分：

1. **Filter** : 用于筛选你想要的模型
2. **模型列表** : 展示了可使用的模型。不带前缀的是官方提供的模型，例如gpt2，而带前缀的是第三方提供的模型。
3. **搜索框** ：你可以通过搜索框按名字搜索模型。

点进去某一个模型，主要包括以下内容：

1. **模型介绍（Model Card）** : 我们可以通过该文档查看该模型都提供了哪些功能，模型的表现等。
2. **模型文件（Files and versions)** : 从该模块可以下载模型文件，一般包含多种框架的（TF、Pytorch等）模型文件和配置文件等，可以用于离线加载。
3. **测试模型(Hosted inference API)** : 可以直接通过该模块测试自己的模型。同时Hugging Face也提供了Http API可以调用，这样就不需要本地部署了。详情请参考：https://huggingface.co/docs/api-inference/index
4. **使用该模型的应用（Spaces using …）** ：这里展示了使用该模型的应用，可以点进去玩一玩。
5. **代码样例（Use in Transformers）** ：你可以通过该模块直接查看该模型的使用方式，直接拷贝代码到项目里就可以用了。

## 部署

### 使用API

![20230420143243](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230420143243.png)

[bert-base-chinese](https://huggingface.co/bert-base-chinese)


```python
import requests

API_URL = "https://api-inference.huggingface.co/models/bert-base-chinese"
headers = {"Authorization": "Bearer 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"}

def query(payload):
	response = requests.post(API_URL, headers=headers, json=payload)
	return response.json()
	
output = query({
	"inputs": "The answer to the universe is [MASK].",
})
```

### 使用space

这里有个文字转成语音的模型，可以直接duplicate this space，他会在自己的空间生成一个Gradio的APP,然后就可以直接使用了。

[LINGUISTIC_ACCENTS](https://huggingface.co/spaces/Chandrabhan01/LINGUISTIC_ACCENTS)

也可以看一下这个repository，里面的file，发现非常简单。

![20230420144746](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230420144746.png)

其中的app.py如下：（这段代码是一个文本转语音的应用程序，支持50多种语言。它使用了Python中的gTTS库和Gradio库。用户可以在文本框中输入要转换为语音的文本，选择目标语言和口音，然后按下提交按钮即可。应用程序还提供了一个选项卡式界面，用户可以在其中将英语句子翻译成其他语言并进行语音合成。）

```python
import os
from gtts import gTTS
import gradio as gr
from translate import Translator


# os.environ['TF_CPP_MIN_LOG_LEVEL']='3'

# auth_token = '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'   # For accessing HuggingFace Facebook Model. 

class Languages:
    """ Languages currently supported by the application. """
    # Language code
    # lang (string, optional) – The language (IETF language tag) to read the text in. Default is en.
    lang = {'Afrikaans': 'af', 'Arabic': 'ar', 'Bulgarian': 'bg', 'Bengali': 'bn', 'Bosnian': 'bs',
            'Catalan': 'ca', 'Czech': 'cs', 'Danish': 'da', 'German': 'de', 'Greek': 'el', 'English': 'en',
            'Spanish': 'es', 'Estonian': 'et', 'Finnish': 'fi', 'French': 'fr', 'Gujarati': 'gu', 'Hindi': 'hi',
            'Croatian': 'hr', 'Hungarian': 'hu', 'Indonesian': 'id', 'Icelandic': 'is', 'Italian': 'it',
            'Hebrew': 'iw', 'Japanese': 'ja', 'Javanese': 'jw', 'Khmer': 'km', 'Kannada': 'kn', 'Korean': 'ko',
            'Latin': 'la', 'Latvian': 'lv', 'Malayalam': 'ml', 'Marathi': 'mr', 'Malay': 'ms',
            'Myanmar (Burmese)': 'my', 'Nepali': 'ne', 'Dutch': 'nl', 'Norwegian': 'no',
            'Polish': 'pl', 'Portuguese': 'pt', 'Romanian': 'ro', 'Russian': 'ru', 'Sinhala': 'si',
            'Slovak': 'sk', 'Albanian': 'sq', 'Serbian': 'sr', 'Sundanese': 'su', 'Swedish': 'sv',
            'Swahili': 'sw', 'Tamil': 'ta', 'Telugu': 'te', 'Thai': 'th', 'Filipino': 'tl', 'Turkish': 'tr',
            'Ukrainian': 'uk', 'Urdu': 'ur', 'Vietnamese': 'vi', 'Chinese (Simplified)': 'zh-CN',
            'Chinese (Mandarin/Taiwan)': 'zh-TW',
            'Chinese (Mandarin)': 'zh'}


class TLD:
    """ Depending on the top-level domain, gTTS can speak in different accents. """

    tld = {'English(Australia)': 'com.au', 'English (United Kingdom)': 'co.uk',
           'English (United States)': 'us', 'English (Canada)': 'ca', 'English (India)': 'co.in',
           'English (Ireland)': 'ie', 'English (South Africa)': 'co.za', 'French (Canada)': 'ca',
           'French (France)': 'fr', 'Portuguese (Brazil)': 'com.br', 'Portuguese (Portugal)': 'pt',
           'Spanish (Mexico)': 'com.mx', 'Spanish (Spain)': 'es', 'Spanish (United States)': 'us'}


class TTSLayer():
    """ Layer on top of gTTS - providing text to speech for """

    def __init__(self, text, tld, lang) -> None:
        """ [Constructor takes in text, the top-level domain and the language in which the text is : ] """
        self.text = text
        self.tld = tld
        self.lang = lang

    def tts(self):
        """ [Converts the text to speech.] """
        tts = gTTS(text=self.text, tld=TLD.tld[self.tld], lang=Languages.lang[self.lang])
        tts.save('tts.mp3')
        with open('tts.mp3') as fp:
            return fp.name


langs = Languages()
top_level_domain = TLD()

""" [Utiility Functions] """


def T2TConversion(text, dest):
    """ [(Utility Function) : Converts sentence from english to another language ] """
    translator = Translator(to_lang=langs.lang[dest])
    return translator.translate(text)


def convert_text(Text, Language, Accent):
    """ [(Utility Function) : Performs Text-To-Speech provided language and accent.] """
    tts = TTSLayer(Text, Accent, Language)
    return tts.tts()


class GRadioInterface:
    """ [Class for managing UI for the application.] """

    def __init__(self, function) -> None:
        """ [Interface for packaging GRadio Application] """

        # Necessary for interface
        self._function = function
        self._inputs = [
            gr.TextArea(label='The Text to be Converted to Audio'),
            gr.Dropdown([key for key, _ in langs.lang.items()], label='Languages Available', ),
            gr.Dropdown([key for key, _ in top_level_domain.tld.items()])]

        self.outputs = gr.Audio()

        # Necessary for descriptive content
        self.title = 'A Text-To-Speech Converter for Low Resource Languages'
        self.description = 'Support over 50 languages !'
        self.article = 'How does it work ? Just write a sentence (in target language) in the space provided and select the target language and accent and press submit. That is it. Wait and Enjoy.'

    def start(self):
        """ [Launching the interface in a tabbed manner.] """

        it_1 = gr.Interface(fn=self._function, inputs=self._inputs, outputs=self.outputs,
                            title=self.title,
                            description=self.description,
                            article=self.article)

        it_2 = gr.Interface(fn=T2TConversion,
                            inputs=[
                                gr.Text(label='Write a sentence in English'),
                                gr.Dropdown([key for key, _ in langs.lang.items()])],
                            outputs=gr.Text(label='The Converted Text'),
                            title='Translation from English',
                            description='Write a sentence in english and convert to other languages for speech synthesis',
                            article='What if you do not have a sentence in a particular language? Just write the sentence in english and let us do the magic.')

        demo = gr.TabbedInterface([it_1, it_2], ['Speech Synthesis', 'Sentence Translation'])
        demo.launch()


demo_app = GRadioInterface(function=convert_text)
demo_app.start()
```

直接扒拉到本地，然后运行，就可以看到和space里面一样的效果了。

![20230420144941](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230420144941.png)