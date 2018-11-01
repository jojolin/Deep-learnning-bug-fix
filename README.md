# Deep-learnning-bug-fix
深度学习过程中碰到的一些问题集锦

## Tencent_AILab_ChineseEmbedding.tar.gz 词向量数据处理
由于部数据出错和unicode error无法通过以下方式加载
```
from gensim.models import KeyedVectors
KeyedVectors.load_word2vec_format(...)
```
可通过临时修改代码直接跳过  ~/.conda/envs/nlp/lib/python3.6/site-packages/gensim/models/utils_any2vec.py

```
     ...
                if line == b'':
                    #raise EOFError("unexpected end of input; is count incorrect or file otherwise damaged?")
                    print('empty line, continue, line_no: %s' % line_no)
                    continue

                try:
                    parts = utils.to_unicode(line.rstrip(), encoding=encoding, errors=unicode_errors).split(" ")
                except:
                    print('to unicode error, skip, line: %s, line_no: %s' % (line, line_no))
                    continue

                if len(parts) != vector_size + 1:
                    #raise ValueError("invalid vector on line %s (is this really the text format?)" % line_no)
                    print('invalid len of parts: %s' % parts)
                    print('skip, line: %s, line_no: %s' % (line, line_no))
                    continue

      ...
    
File:      ~/.conda/envs/nlp/lib/python3.6/site-packages/gensim/models/utils_any2vec.py
```
