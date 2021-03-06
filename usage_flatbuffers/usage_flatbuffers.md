# flatbuffers

https://github.com/google/flatbuffers

### 命令(*mac平台*)

```
./flatc -j -b schema.fbs sample.json
```

### 数据结构(*sample.json*)
```
    {   
        p:[
            {
                id:1,
                name:"other",
            },
            ...
        ]
    }
```
### fbs文件结构(*schema.json*)
```
    table Person{
        id:long;
        name:string;
    }

    table PersonList{
        p:[Person];
    }

    root_type PersonList;
```
### JAVA

#### 生成
```
    FlatBufferBuilder fbb = new FlatBufferBuilder();
    //创建Person数据
    int[] list=new int[10];
    for(int i=0;i<list.length;i++){
        int a=fbb.createString("test"+i);
        Person.startPerson(fbb);
        Person.addId(fbb, i);
        Person.addName(fbb, a);
        list[i]=Person.endPerson(fbb);
    }
    //生成数据buffer
    int v=PersonList.createPVector(fbb, list);
    //创建PersonList对象
    int p=PersonList.createPersonList(fbb,v);
    //结束创建
    PersonList.finishPersonListBuffer(fbb, p);
```
#### 解析
```
    PersonList test=PersonList.getRootAsPersonList(fbb.dataBuffer());
```