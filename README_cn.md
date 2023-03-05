# 简单易用的OpenCSV使用教程

跨平台多线程设计！
使用C++分析大数据的时候，数据按CSV格式导出，可以用Excel分析数据。
OpenCSV非常简单易用。

**OpenLinyou项目设计跨平台服务器框架，在VS或者XCode上写代码，无需任何改动就可以编译运行在Linux上，甚至是安卓和iOS.**
OpenLinyou：https://github.com/openlinyou

## 跨平台支持
Windows、linux、Mac、iOS、Android等跨平台设计

## 编译和执行
请安装cmake工具，用cmake可以构建出VS或者XCode工程，就可以在vs或者xcode上编译运行。
源代码：https://github.com/openlinyou/opencsv
```
git clone https://github.com/openlinyou/opencsv
cd ./opencsv
mkdir build
cd build
cmake ..
#如果是win32，在该目录出现opencsv.sln，点击它就可以启动vs写代码调试
make
./test
```

## 全部源文件
+ src/opencsv.h
+ src/opencsv.cpp

## 1.生成csv
```C++
#include <assert.h>
#include "opencsv.h"
int main()
{
    //make csv
    OpenCSV csv = { "ID", "name", "salary" };
    csv = { "1", "Jack", "100000" };
    csv = { "2", "Tom", "80000" };
    csv = { std::to_string(3), "Lucy", "50000" };

    std::string buffer1;
    csv >> buffer1;
    
    std::string buffer2 = "ID,name,salary\n"
            "1,Jack,100000\n"
            "2,Tom,80000\n"
            "3,Lucy,50000\n";

    assert(buffer1 == buffer2);
    return 0;
}
```

## 2.加载csv
```C++

#include <assert.h>
#include "opencsv.h"
int main()
{
    std::string buffer2 = "ID,name,salary\n"
            "1,Jack,100000\n"
            "2,Tom,80000\n"
            "3,Lucy,50000\n";

    OpenCSV csv;
    csv << buffer2;

    std::string buffer1 = "ID,name,salary\n";
    for (size_t i = 1; i < csv.size(); ++i)
    {
        auto& line = csv[i];
        buffer1.append(line["ID"] + "," + line["name"] + "," + line["salary"] + "\n");
    }
    assert(buffer1 == buffer2);
    return 0;
}
```

## 3.生成和加载csv文件
```C++

#include <assert.h>
#include "opencsv.h"

int main()
{
    std::string buffer1;
    std::string buffer2;
    buffer2 = "ID,name,salary\n"
            "1,Jack,100000\n"
            "2,Tom,80000\n"
            "3,Lucy,50000\n";

    std::string filePath = "./test.csv";
    //make csv file
    {
        OpenCSV csv;
        csv << buffer2;
        assert(csv[0][0] == "ID");
        assert(csv[0][2] == "salary");
        csv[1]["salary"] = std::to_string(10000);
        csv[2]["salary"] = "8000";
        csv[3]["salary"] = "5000";
        csv >> filePath;
    }
    buffer2 = "ID,name,salary\n"
        "1,Jack,10000\n"
        "2,Tom,8000\n"
        "3,Lucy,5000\n";

    //read csv file
    {
        OpenCSV csv;
        csv << filePath;
        buffer1.clear();
        csv >> buffer1;
    }
    assert(buffer1 == buffer2);

    return 0;
}

```

