
# lightgbm

## 介绍
- github地址：https://github.com/Microsoft/LightGBM
- 说明文档：[Welcome to LightGBM’s documentation! — LightGBM documentation](https://lightgbm.readthedocs.io/en/latest/)
- 中文说明文档：[欢迎来到 LightGBM 的中文文档! — LightGBM 中文文档 - ApacheCN](http://lightgbm.apachecn.org/cn/latest/)

## 简单安装与使用

编译安装：
``` shell
git clone --recursive https://github.com/Microsoft/LightGBM ; cd LightGBM
mkdir build ; cd build
cmake ..
make -j8
```

使用：
``` shell
cd examples/binary_classification/
# 当前目录生成 LightGBM_model.txt
../../lightgbm config=train.conf
# 当前目录生成 LightGBM_predict_result.txt
../../lightgbm config=predict.conf
```
## 配置参数说明

查看 predict.conf 文件
- task: type=enum, options=train, predict, convert_model
- data: type=string, 输入数据路径
- input_model: type=string, 模型路径，对于predict用于预测，对于train用于继续训练
- output_result: default=LightGBM_predict_result.txt, type=string, 输出文件名

查看 train.conf 文件
- boosting_type/boosting: type=enum, options=gbdt, rf, dart, goss ??
- application/objective/app: type=enum, options=regression, binary, multiclass, lamdbarank 等等，任务类型
- output_model:  default=LightGBM_model.txt, type=string，模型输出名称

其他参数查看文档

## 使用样例

kaggle地址：[Porto Seguro’s Safe Driver Prediction \| Kaggle](https://www.kaggle.com/c/porto-seguro-safe-driver-prediction)
参考：https://blog.csdn.net/qq_37195507/article/details/78553581

### 赛题说明：
这是Kaggle在9月30日开启的一个新的比赛，举办者是巴西最大的汽车与住房保险公司之一：Porto Seguro。该比赛要求参赛者根据汽车保单持有人的数据建立机器学习模型，分析该持有人是否会在次年提出索赔。

### 特征说明：
```
Index(['id', 'target', 'ps_ind_01', 'ps_ind_02_cat', 'ps_ind_03',
       'ps_ind_04_cat', 'ps_ind_05_cat', 'ps_ind_06_bin', 'ps_ind_07_bin',
       'ps_ind_08_bin', 'ps_ind_09_bin', 'ps_ind_10_bin', 'ps_ind_11_bin',
       'ps_ind_12_bin', 'ps_ind_13_bin', 'ps_ind_14', 'ps_ind_15',
       'ps_ind_16_bin', 'ps_ind_17_bin', 'ps_ind_18_bin', 'ps_reg_01',
       'ps_reg_02', 'ps_reg_03', 'ps_car_01_cat', 'ps_car_02_cat',
       'ps_car_03_cat', 'ps_car_04_cat', 'ps_car_05_cat', 'ps_car_06_cat',
       'ps_car_07_cat', 'ps_car_08_cat', 'ps_car_09_cat', 'ps_car_10_cat',
       'ps_car_11_cat', 'ps_car_11', 'ps_car_12', 'ps_car_13', 'ps_car_14',
       'ps_car_15', 'ps_calc_01', 'ps_calc_02', 'ps_calc_03', 'ps_calc_04',
       'ps_calc_05', 'ps_calc_06', 'ps_calc_07', 'ps_calc_08', 'ps_calc_09',
       'ps_calc_10', 'ps_calc_11', 'ps_calc_12', 'ps_calc_13', 'ps_calc_14',
       'ps_calc_15_bin', 'ps_calc_16_bin', 'ps_calc_17_bin', 'ps_calc_18_bin',
       'ps_calc_19_bin', 'ps_calc_20_bin'],
      dtype='object')
```
- 除去作为唯一标识的ID列以及target列之外，特征列共有57列。
- 特征列的列名结构是统一的，共有被下划线分割开来的四层词缀：第一层词缀是所有特征列都有的ps，不需要关注；第二层词缀有ind、reg、car、calc四个类别；第三层词缀是单纯的编号；而第四层词缀有bin、cat两个种类，需要注意的是，不是所有的特征列都具有第四层词缀的。
- 根据举办方给出的数据说明，第四层词缀标注的是特征的变量类型，cat为多分类变量，bin为二分类变量，无词缀则属于连续或顺序变量；而第二层词缀则是变量名称，Ind是与司机个人相关的特征，reg是地区相关的特征，car是汽车相关的特征，calc则是其他通过计算或估计得到的特征。第三层词缀虽然官方没有明说，但应该是每个变量名称下的变量编号。

### 解决样例

简单样例：[Simple Python LightGBM example \| Kaggle](https://www.kaggle.com/ezietsman/simple-python-lightgbm-example/code)
复杂样例：[Safe Driver Prediction top 1%, LightGBM (0.29132) \| Kaggle](https://www.kaggle.com/msahamed/safe-driver-prediction-top-1-lightgbm-0-29132/notebook)
