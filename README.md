# opencsv
Read and write csv file.

```C++

#include <assert.h>
#include "opencsv.h"

int main()
{
    std::string buffer1;
    std::string buffer2;

    //make csv
    {
        OpenCSV csv = { "ID", "name", "salary" };
        csv = { "1", "Jack", "100000" };
        csv = { "2", "Tom", "80000" };
        csv = { std::to_string(3), "Lucy", "50000" };

        csv >> buffer1;
    }
    
    buffer2 = "ID,name,salary\n"
            "1,Jack,100000\n"
            "2,Tom,80000\n"
            "3,Lucy,50000\n";

    assert(buffer1 == buffer2);

    //parse csv
    {
        OpenCSV csv;
        csv << buffer2;
        buffer1 = "ID,name,salary\n";
        for (size_t i = 1; i < csv.size(); ++i)
        {
            auto& line = csv[i];
            buffer1.append(line["ID"] + "," + line["name"] + "," + line["salary"] + "\n");
        }
    }
    assert(buffer1 == buffer2);

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

## build and run
#### Environment：
Windows、linux

### Source file list:
src/opencsv.h 
src/opencsv.cpp 


```
cd ./opencsv
mkdir build
cd build
cmake ..
make
./test
```