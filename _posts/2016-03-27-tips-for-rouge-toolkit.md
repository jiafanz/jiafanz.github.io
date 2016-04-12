**ROUGE Toolkit**[^1]是一个用于评价自动摘要或机器翻译效果的工具箱，也就是将不同系统生成的摘要（翻译）与人工生成的标准摘要（翻译）作对比。由于原作者并没有提供详细的使用文档，开始入门这个工具时一点头绪也没有。。。好在发现了这篇文章*Basics of Setting up ROUGE Toolkit for Evaluation of Summarization Tasks*[^2]，提供了有用的入门指导。

但是，由于ROUGE只接受英文作为输入数据，所以对于中文文本无法进行计算。解决的方法是：
对中文进行分词，将分词结果作为输入；
查看了源代码，发现源代码对中文进行了过滤，类似于下面这样的正则表达匹配语句，

```sh
$$tokenizedText=~s/[^A-Za-z0–9\-]/ /g; #过滤所有非字母数字的字符
if($mx_tokens[$i]=~/^[a-z0–9\$]/o){
    #只有匹配到字母和数字才进行操作
}
```

将这些语句注释掉，重新运行ROUGE-1.5.5.pl，问题解决~

## References:
[^1] Lin C Y. Rouge: A package for automatic evaluation of summaries[C]//Text summarization branches out: Proceedings of the ACL-04 workshop. 2004, 8.
[^2] http://kavita-ganesan.com/rouge-howto
