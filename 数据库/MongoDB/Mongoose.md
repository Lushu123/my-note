### Mongoose的使用

#### 1、连接数据库

```JavaScript
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/test');
const conn = mongoose.connection;
conn.on('connected',function () {
    console.log('数据库连接成功！')
});
```

#### 2、得到特定集合的model

```javascript
//描述文档结构
const userSchema = mongoose.Schema({//指定文档的结构：属性名/属性名的类型，是否时必须的，默认值
    username:{type:String,require:true},
    password:{type:String,require:true},
    type:{type:String,require:true},
});
//定义model(与集合对应，可以操作集合)
const UserModel = mongoose.model('user',userSchema);//集合名：users（是一个函数）
```

#### 3、CRUD

- 通过**Model实例**的save()添加数据

  ```JavaScript
  function testSave() {
      const userModel = new UserModel({
          username:'Tom',
          password:MD5('123'),
          type:'老板'
      });
      userModel.save(function (error, doc) {
          console.log('save()',error,doc)
      });
  }
  testSave();
  ```

- 通过**Mode**l的find()/findOne()查询多个或一个数据

  ```JavaScript
  //查询所有
  function testFind() {
      UserModel.find(function (error, datas) {
          console.log('find()',error,datas)
      })
  }
  //查询指定的数据
  function testFindOne() {
      UserModel.findOne({username:'Bob'},function (error, data) {
          console.log('find()',error,data)
      })
  }
  testFind();
  testFindOne();
  ```

- 通过**Mode**l的findByIdAndUpdate()更新某个数据

  ```JavaScript
  function testFindByIdAndUpdate() {
      UserModel.findOneAndUpdate({_id:'5de8b224b7062a377cb788f5'},{username:'Jack'},function (error, doc) {
          console.log('update',error,doc)
      })
  }
  // testFindByIdAndUpdate();
  ```

- 通过**Model**的remove()删除某个数据

  ```JavaScript
  function testRemove() {
      UserModel.remove({_id:'5de8b224b7062a377cb788f5'},function (error, doc) {
          console.log('remove',error,doc)
      })
  }
  testRemove()
  ```

  