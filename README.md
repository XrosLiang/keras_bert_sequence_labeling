本项目采用Keras和Keras-bert实现序列标注。

### 维护者

- jclian91

### 数据集

人民日报命名实体识别数据集（example.train和example.test），共三种标签：LOC, PER, ORG

### 模型结构

```
__________________________________________________________________________________________________
Layer (type)                    Output Shape         Param #     Connected to                     
==================================================================================================
input_3 (InputLayer)            (None, None)         0                                            
__________________________________________________________________________________________________
input_4 (InputLayer)            (None, None)         0                                            
__________________________________________________________________________________________________
model_5 (Model)                 multiple             101382144   input_3[0][0]                    
                                                                 input_4[0][0]                    
__________________________________________________________________________________________________
bidirectional_2 (Bidirectional) (None, None, 200)    695200      model_5[1][0]                    
__________________________________________________________________________________________________
crf_2 (CRF)                     (None, None, 7)      1470        bidirectional_2[0][0]            
==================================================================================================
Total params: 102,078,814
Trainable params: 102,078,814
Non-trainable params: 0
```

### 模型效果

模型参数：MAX_SEQ_LEN=128, BATCH_SIZE=32, EPOCH=10

运行model_evaluate.py,模型评估结果如下：

```
           precision    recall  f1-score   support

      LOC     0.9330    0.8986    0.9155      3658
      ORG     0.8881    0.8902    0.8891      2185
      PER     0.9692    0.9469    0.9579      1864

micro avg     0.9287    0.9079    0.9182      7707
macro avg     0.9291    0.9079    0.9183      7707
```

### 模型预测示例

运行model_predict.py，对新文本进行预测，结果如下：

```
{'entities': [{'end': 17, 'start': 16, 'type': 'LOC', 'word': '欧'},
              {'end': 50, 'start': 48, 'type': 'LOC', 'word': '英国'},
              {'end': 63, 'start': 62, 'type': 'LOC', 'word': '欧'},
              {'end': 72, 'start': 69, 'type': 'PER', 'word': '卡梅伦'},
              {'end': 78, 'start': 73, 'type': 'PER', 'word': '特雷莎·梅'},
              {'end': 86, 'start': 85, 'type': 'LOC', 'word': '欧'},
              {'end': 102, 'start': 95, 'type': 'PER', 'word': '鲍里斯·约翰逊'}],
 'string': '当2016年6月24日凌晨，“脱欧”公投的最后一张选票计算完毕，占投票总数52%的支持选票最终让英国开始了一段长达4年的“脱欧”进程，其间卡梅伦、特雷莎·梅相继离任，“脱欧”最终在第三位首相鲍里斯·约翰逊任内完成。'}
```

```
{'entities': [{'end': 6, 'start': 0, 'type': 'ORG', 'word': '台湾“立法院'},
              {'end': 30, 'start': 29, 'type': 'LOC', 'word': '台'},
              {'end': 38, 'start': 35, 'type': 'PER', 'word': '蔡英文'},
              {'end': 66, 'start': 64, 'type': 'LOC', 'word': '台湾'}],
 'string': '台湾“立法院”“莱猪（含莱克多巴胺的猪肉）”表决大战落幕，台当局领导人蔡英文24日晚在脸书发文宣称，“开放市场的决定，将会是未来台湾国际经贸走向世界的关键决定”。'}
```

```
{'entities': [{'end': 9, 'start': 7, 'type': 'LOC', 'word': '印度'},
              {'end': 14, 'start': 12, 'type': 'LOC', 'word': '南海'},
              {'end': 27, 'start': 25, 'type': 'LOC', 'word': '印度'},
              {'end': 30, 'start': 28, 'type': 'LOC', 'word': '越南'},
              {'end': 45, 'start': 43, 'type': 'LOC', 'word': '印度'},
              {'end': 49, 'start': 47, 'type': 'PER', 'word': '莫迪'},
              {'end': 53, 'start': 51, 'type': 'LOC', 'word': '南海'},
              {'end': 90, 'start': 88, 'type': 'LOC', 'word': '南海'}],
 'string': '最近一段时间，印度政府在南海问题上接连发声。在近期印度、越南两国举行的线上总理峰会上，印度总理莫迪声称南海行为准则“不应损害该地区其他国家或第三方的利益”，两国总理还强调了所谓南海“航行自由”的重要性。'}
```

### 代码说明

0. 将BERT中文预训练模型chinese_L-12_H-768_A-12放在chinese_L-12_H-768_A-12文件夹下
1. 运行load_data.py，生成类别标签，注意O标签为0;
2. 所需Python第三方模块参考requirements.txt文档
3. 自己需要分类的数据按照data/example.train和data/example.test的格式准备好
4. 调整模型参数，运行model_train.py进行模型训练
5. 运行model_evaluate.py进行模型评估
6. 运行model_predict.py对新文本进行预测