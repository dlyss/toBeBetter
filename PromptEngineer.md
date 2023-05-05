# 提示工程简介
提示工程应用于开发和优化提示词（Prompt），帮助用户有效地将语言模型用于各种应用场景和研究领域。
提示工程（Prompt Engineering）就是探讨如何设计出最佳提示词，用于指导语言模型帮助我们高效完成某项任务。
掌握提示工程相关技能将有助于更好地了解大型语言模型的能力和局限性。开发人员可通过提示工程设计和研发出强大的技术，
实现和大语言模型或其他生态工具的高效接轨。
可以通过简单的提示词（Prompts）获得大量结果，但结果的质量与您提供的信息数量和完善度有关。一个提示词可以包含您传递到
模型的_指令_或_问题_等信息，也可以包含其他详细信息，如_上下文_、_输入_或_示例_等。您可以通过这些元素来更好地指导模型，
并因此获得更好的结果。
# 模型设置
<strong>Temperature</strong>：temperature 的参数值越小，模型就会返回越确定的一个结果。如果调高该参数值，大语言模型可能会返回更随机的结果，
也就是说这可能会带来更多样化或更具创造性的产出。QA等任务适合调低参数值，诗歌等适合调高参数值。
<strong>Top_p</strong>：top_p（与 temperature 一起称为核采样的技术），可以用来控制模型返回结果的真实性。如果你需要准确和事实的答案，
就把参数值调低。如果你想要更多样化的答案，就把参数值调高一些。
 <strong>一般建议修改一个参数即可</strong>
# 提示词格式
## 零样本提示(zero-shot prompting)
`<问题>?`
   
`<指令>`
## 小样本提示(Few-shot prompting)
```Q: <问题>?
   
A: <答案>
   
Q: <问题>?
   
A: <答案>
   
Q: <问题>?
   
A: <答案>
   
Q: <问题>?
   
A:
```   
## 提示词要素
``` 
指令：想要模型执行的特定任务或指令。

上下文：包含外部信息或额外的上下文信息，引导语言模型更好地响应。

输入数据：用户输入的内容或问题。

输出指示：指定输出的类型或格式
```
<strong>指令示例</strong>：写入、分类、总结、翻译、排序等
可以使用分割符###，分隔指令和上下文
 ```
  ### 指令 ###
将以下文本翻译成西班牙语：
文本：“hello！
  ```
<strong>示例</strong>

```
提取以下文本中的地名。
所需格式：
地点：<逗号分隔的公司名称列表>
输入：“虽然这些发展对研究人员来说是令人鼓舞的，但仍有许多谜团。里斯本未知的香帕利莫德中心的神经免疫学家
Henrique Veiga-Fernandes说：“我们经常在大脑和我们在周围看到的效果之间有一个黑匣子。
”“如果我们想在治疗背景下使用它，我们实际上需要了解机制。””
```
# 提示词示例
## 代码生成
```
/*
询问用户的姓名并说“ Hello”
*/
```
输出结果
```
let name = prompt("What is your name?");
console.log(`Hello, ${name}!`);
```
```
"""
Table departments, columns = [DepartmentId, DepartmentName]
Table students, columns = [DepartmentId, StudentId, StudentName]
Create a MySQL query for all students in the Computer Science Department
"""
```
输出结果
```
SELECT StudentId, StudentName 
FROM students 
WHERE DepartmentId IN (SELECT DepartmentId FROM departments WHERE DepartmentName = 'Computer Science');
```
   
