
### how to use json in c++
#### 1. jsoncpp
> jsoncpp is a library for c++ developer to use json
#### 2. install jsoncpp on linux
 [http://www.centoscn.com/image-text/install/2016/0505/7173.html](http://www.centoscn.com/image-text/install/2016/0505/7173.html)
 
#### 3. json format
- usually json is like this:
```
json_obj 
{
    "json_key1":"json_value1",
    "json_key2":"json_value2",
    "json_key3":"json_value3"
}
```
- json object can contain json objects
```
student 
{
    "name":"lihua",
    "gender":"female",
    "grades": {
       "math":"90",
       "cs":"98",
       "english":"99"
    }
}
```
#### 4. three objects in json.h
- include head file #include <json/json.h>
- Json::Value  表示各种类型的对象
- Json::Reader 将json字符串解析为json value，主要函数有parse
```
  Json:: Reader reader;
  Json:: Value json_obj;
  reader.parse(reinterpret_cast<char*>(ptr), json_obj); //parse the json string ptr into json value json_obj
```
- Json::Writer 将json value转化为json字符串，Json::FastWriter和Json::StyleWriter，分别输出不带格式的json和带格式的json

#### 5. coding with json
- add or modify 
JRoot["stringdata"] = Json::Value("msg");
JRoot["intdata"] = Json::Value(10);
- delete
JValue.removeMember("toberemove");
- how to deal with different data type
the Json::Value can be converted into different data type: 
```
  json_obj["key1"].asInt();
  json_obj["key2"].asDouble();
  json_obj["key3"].asString();
```
#### 6. data type conversion
- if you got the error like this "Value is not convertible to double"

maybe the json value can't be converted into double or other data type, but you can convert json value into string first, then convert the string into whatever data type you want. 

- for example, data in json value with many decimal places, it can't convert into a double 
- or you got the json value is a double, you want to convert it into int. you can't convert it using asInt() directly.
- apparently, we may got some decimal loss
```
  atof(json_obj["key4"].asString().c_str()); // convert a double with many decimal places into string then into double
  atof(json_obj["key5"].asString().c_str()); // convert a double into string then into int
```
#### 7. 四舍五入
```
double d = 5.8;
int a = static_cast<int> (d);  // a = 5
```
- the simplest way is:
```
int b = static_cast<int> (d+0.5); // a = 6
```

