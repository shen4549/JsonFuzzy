# fuzzy 工具箱
## 一 文件目录

1.factory.py   #其他工具类

2.fuzz_interface.py   #接口文件，上层只需调用该类方法

3.core_files   #工具内部处程序

4.log   #日志文件夹

5.result   #fuzzy结果文件夹

## 二 如何调用
fuzz_interface.py接口类提供两个函数接口：
```

 def fuzz_data(self, template_dict, fuzz_type):
        """实时返回数据
        :param template_dict: 用户传入json数据，该函数会fuzz全部key对应的value
        :param fuzz_type: 生成特殊字符串选项，一共有七个选项，["X", "H", "S", "T", "C", "L", "R"],
                         分别代表[xss注入, http header注入, SQL注入, 模板注入, RCE注入, LFI攻击
                         , 随机串] 如果都不选，代表都选择
        :return: dict, fuzz后的数据
       """
  def fuzz_file(self, template_dict, fuzz_type, case_num, file_name):
        """批量生成数据保存到文件
        :param template_dict:
        :param fuzz_type:
        :param case_num: 生成数据量
        :param file_name: 保存文件名
        """

```
调用例子如下：
```
    from xxx.fuzz_interface import FuzzApi
    fuzzer = FuzzyApi()
    json_data = {
        "name": "小明"
    }
    result_data = fuzzer.fuzz_data(json_data, fuzz_type="")
    print(result_data)
    返回结果如下：
    {'name': 'SELECT 1,2,IF(SUBSTR(@@version,1,1)<5,BENCHMARK(2000000,SHA1(0xDE7EC71F1)),SLEEP(1))/*\'XOR(IF(SUBSTR(@@version,1,1)<5,BENCHMARK(2000000,SHA1(0xDE7EC71F1)),SLEEP(1)))OR\'|"XOR(IF(SUBSTR(@@version,1,1)<5,BENCHMARK(2000000,SHA1(0xDE7EC71F1)),SLEEP(1)))OR"*/ FROM some_table WHERE\\u0000\\u0002\\u0000\\u0000= \\u5c0f\\u660e'}
    
```