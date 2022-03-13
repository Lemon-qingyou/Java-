**《Java核心技术卷一》**P177

## 编写一个完美的equals方法的建议

* 1、显示参数命名为otherObject，稍后需要将它强制转换成另一个名为other的变量。

* 2、检测this与otherObject是否相等：
  `if （this == otherObject）return true；`

* 3、检测otherObject是否为null，如果为null，返回false。这项检测是很必要的。
  `if （otherObject == null) return false;`

* 4、比较this与otherObject的类。
              如果equals的语义可以在子类中改变，就使用getClass检测：
                  `if (getClass() != otherObject.getClass())  return false;`

  ​            如果所有的子类都有相同的相等性语义，可以使用instanceof检测：
  ​                `if (!(otherObject instanceof ClassName))  return false;`

* 5、将otherObject强制转换成为相应类型的变量：
        `ClassName other = （ClassName）otherObject`

* 6、现在根据相等性概念的要求比较字段。使用 == 比较基本类型字段，使用`Objects.equals`比较对象字段。
        如果所有的字段都匹配，就返回true；否则返回false。



**如果在子类中重新定义了equals，就要在其中包含一个`super.equals(other)`调用。**

 