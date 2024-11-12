# js实现根据文件名排序功能

## 一、实现功能
通过js实现对文件根据文件名排序，文件名根据首字母排序,按照数字 - 特殊字符 - 大写英文 - 小写英文 - 中文的顺序排序
## 二、实现思路
### 1.首先根据文件名将所有文件名进行分类，共计分为四类：
因为文件名大致可以按照类型分为：数字，特殊字符，英文，中文四种类型，我们在此处用四个数组分别维护四种类型的文件名
```javascript
let enList = [];// 英文数组
let cnList = [];//中文数组
let elList = [];//数字
let numList = [];//其他字符
```
### 2.对每一个数据按照既定的规则进行排序
确定完四种类型的文件名数组后，需要对每一种类型的文件名规定一种排序规则
- 中文数组通过localeComparet对比字符串来进行排序
- 其他数组根据字符编码排序
### 3.对排序后的四个数组拼接在一起
对排序后的数组按照既定的规则拼接在一起
```javascript
let result = [...numList, ...elList, ...enList, ...cnList];
```
完整代码实例：
```javascript
  data() {
    return {
      arr: ["测试第一单元第五章", "阿里","测试第一单元第一章","####","4第二","aaa三", "第3"],
    };
  },
  mounted() {
    this.pySort(this.arr);
  },
  methods: {
        pySort(arr, empty) {
          if (!String.prototype.localeCompare) return null;
          var result = [];
          let enList = [];// 英文数组
          let cnList = [];//中文数组
          let elList = [];//数字
          let numList = [];//其他字符
          this.arr.forEach((item) => {
            let first = item.slice(0, 1);
            if (this.isChar(first)) {
              enList.push(item);
            } else if (this.isChinese(first)) {
              cnList.push(item);
            } else if (this.isNumber(first)) {
              numList.push(item);
            } else {
              elList.push(item);
            }
          });
          enList.sort((a, b) => {
            return b.charCodeAt(0) - a.charCodeAt(0);
          });
          cnList.sort((a, b) => {
            return a.slice(0, 1).localeCompare(b.slice(0, 1), "zh-Hans-CN", {
              sensitivity: "accent",
            });
          });
          numList.sort((a, b) => {
            return a.charCodeAt(0) - b.charCodeAt(0);
          });
          elList.sort((a, b) => {
            return a.charCodeAt(0) - b.charCodeAt(0);
          });
          result = [...numList, ...elList, ...enList, ...cnList];
          return result;
        },
        isChinese(temp) {
          var re = /[^\u4E00-\u9FA5]/;
          if (re.test(temp)) {
            return false;
          }
          return true;
        },
        isChar(char) {
          var reg = /[A-Za-z]/;
          if (!reg.test(char)) {
            return false;
          }
          return true;
        },
        isNumber(num) {
          var re = /^([0-9]+\.?[0-9]*|-[0-9]+\.?[0-9]*)$/;
          if (!re.test(num)) {
            return false;
          } else {
            return true;
          }
        },
      },
```
