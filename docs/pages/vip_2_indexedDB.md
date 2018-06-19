# IndexedDB 教程

IndexedDB 是一个基于 JavaScript 的面向对象的事务型数据库。有了 `LocalStorage` 和 `Cookies`，为什么还要推出 `indexedDB` 呢？其实对于在浏览器里存储数据，可以使用 `cookies` 或 `LocalStorage`，但它们都是比较简单的技术，而 `IndexedDB` 提供了类似数据库风格的数据存储和使用方式。

## LocalStorage 与 IndexedDB 区别也适用场景

`LocalStorage` 是用 `key-value` 键值模式存储数据，它存储的数据都是字符串形式。如果你想让 `LocalStorage` 存储对象，你需要借助 `JSON.stringify()`能将对象变成字符串形式，再用 `JSON.parse()`将字符串还原成对象,就是专门为小数量数据设计的，所以它的 api 设计为同步的。

`IndexedDB` 很适合存储大量数据，它的 API 是异步调用的。`IndexedDB` 使用索引存储数据，各种数据库操作放在事务中执行。`IndexedDB` 甚至还支持简单的数据类型。`IndexedDB` 比 `localstorage` 强大得多，但它的 API 也相对复杂。对于简单的数据，你应该继续使用 `localstorage`，但当你希望存储大量数据时，`IndexedDB` 会明显的更适合，`IndexedDB` 能提供你更为复杂的查询数据的方式。

## indexedDB 的特性

- 对象仓库

有了数据库后我们自然希望创建一个表用来存储数据，但 indexedDB 中没有表的概念，而是 objectStore，一个数据库中可以包含多个 objectStore，objectStore 是一个灵活的数据结构，可以存放多种类型数据。也就是说一个 objectStore 相当于一张表，里面存储的每条数据和一个键相关联。我们可以使用每条记录中的某个指定字段作为键值（keyPath），也可以使用自动生成的递增数字作为键值（keyGenerator），也可以不指定。选择键的类型不同，objectStore 可以存储的数据结构也有差异。

- 事务性

在 indexedDB 中，每一个对数据库操作是在一个事务的上下文中执行的。事务范围一次影响一个或多个 object stores，你通过传入一个 object store 名字的数组到创建事务范围的函数来定义。例如：db.transaction(storeName, 'readwrite')，创建事务的第二个参数是事务模式。当请求一个事务时,必须决定是按照只读还是读写模式请求访问。

- 基于请求

对 indexedDB 数据库的每次操作，描述为通过一个请求打开数据库,访问一个 object store，再继续。IndexedDB API 天生是基于请求的,这也是 API 异步本性指示。对于你在数据库执行的每次操作,你必须首先为这个操作创建一个请求。当请求完成,你可以响应由请求结果产生的事件和错误。

- 异步

在 IndexedDB 大部分操作并不是我们常用的调用方法，返回结果的模式，而是请求—响应的模式，所谓异步 API 是指并不是这条指令执行完毕，我们就可以使用 request.result 来获取 indexedDB 对象了，就像使用 ajax 一样，语句执行完并不代表已经获取到了对象，所以我们一般在其回调函数中处理。

## 几个概念

- [IDBFactory：数据库工厂，负责打开或者创建数据库](https://developer.mozilla.org/en-US/docs/Web/API/IDBFactory)

  - IDBFactory.open 方法发送一个打开或者创建一个数据库的请求
  - IDBFactory.deleteDatabase 方法： 发送一个删除数据库的请求

- [IDBDatabase: 数据库](https://developer.mozilla.org/en-US/docs/Web/API/IDBDatabase)

  - IDBDatabase.close 方法关闭数据库。
  - IDBDatabase.createObjectStore 方法创建 store，相当于表
  - IDBDatabase.transaction 开启一个事务。

- [IDBIndex：数据库表的索引](https://developer.mozilla.org/en-US/docs/Web/API/IDBIndex)

- [**IDBObjectStore**：数据库表](https://developer.mozilla.org/en-US/docs/Web/API/IDBObjectStore)

- [IDBTransaction：事务](https://developer.mozilla.org/en-US/docs/Web/API/IDBTransaction)

- [**IDBRequest**：机会是所有 indexedDB 操作的返回值，indexedDB 操作请求](https://developer.mozilla.org/en-US/docs/Web/API/IDBRequest)

  - IDBRequest.result 结果
  - IDBRequest.onerror 异常事件
  - IDBRequest.onsuccess 成功的事件

## 快速入门 demo

- 打开数据库实例。

```js
var db; // 全局的indexedDB数据库实例。

//1. 获取IDBFactory接口实例（文档地址： https://developer.mozilla.org/en-US/docs/Web/API/IDBFactory）
var indexedDB =
  window.indexedDB ||
  window.webkitIndexedDB ||
  window.mozIndexedDB ||
  window.msIndexedDB;

if (!indexedDB) {
  console.log('你的浏览器不支持IndexedDB');
}

// 2. 通过IDBFactory接口的open方法打开一个indexedDB的数据库实例
// 第一个参数： 数据库的名字，第二个参数：数据库的版本。返回值是一个：IDBRequest实例,此实例有onerror和onsuccess事件。
var IDBOpenDBRequest = indexedDB.open('demoDB', 1);

// 3. 对打开数据库的事件进行处理

// 打开数据库成功后，自动调用onsuccess事件回调。
IDBOpenDBRequest.onsuccess = function(e) {};

// 打开数据库失败
IDBOpenDBRequest.onerror = function(e) {
  console.log(e.currentTarget.error.message);
};

// 第一次打开成功后或者版本有变化自动执行以下事件：一般用于初始化数据库。
IDBOpenDBRequest.onupgradeneeded = function(e) {
  db = e.target.result; // 获取到 demoDB对应的 IDBDatabase实例,也就是我们的数据库。

  if (!db.objectStoreNames.contains(personStore)) {
    //如果表格不存在，创建一个新的表格（keyPath，主键 ； autoIncrement,是否自增），会返回一个对象（objectStore）
    // objectStore就相当于数据库中的一张表。IDBObjectStore类型。
    var objectStore = db.createObjectStore(personStore, {
      keyPath: 'id',
      autoIncrement: true
    });

    //指定可以被索引的字段，unique字段是否唯一。类型： IDBIndex
    objectStore.createIndex('name', 'name', {
      unique: true
    });
    objectStore.createIndex('phone', 'phone', {
      unique: false
    });
  }
  console.log('数据库版本更改为： ' + dbVersion);
};
```

- 数据库的 objectStore 添加

indexedDB 的增删改查的操作需要放到一个事务中进行（推荐）

```js
// 创建一个事务，类型：IDBTransaction，文档地址： https://developer.mozilla.org/en-US/docs/Web/API/IDBTransaction
var transaction = db.transaction(personStore, 'readwrite');

// 通过事务来获取IDBObjectStore
var store = transaction.objectStore(personStore);

// 往store表中添加数据
var addPersonRequest = store.add({
  name: '老马',
  phone: '189111833',
  address: 'aicoder.com'
});

// 监听添加成功事件
addPersonRequest.onsuccess = function(e) {
  console.log(e.target.result); // 打印添加成功数据的 主键（id）
};

// 监听失败事件
addPersonRequest.onerror = function(e) {
  console.log(e.target.error);
};
```

- 数据库的 objectStore 修改

```js
// 创建一个事务，类型：IDBTransaction，文档地址： https://developer.mozilla.org/en-US/docs/Web/API/IDBTransaction
var transaction = db.transaction(personStore, 'readwrite');

// 通过事务来获取IDBObjectStore
var store = transaction.objectStore(personStore);
var person = {
  id: 6,
  name: 'lama',
  phone: '515154084',
  address: 'aicoder.com'
};

// 修改或者添加数据。 第一参数是要修改的数据，第二个参数是主键（可省略)
var updatePersonRequest = store.get(6);

// 监听添加成功事件
updatePersonRequest.onsuccess = function(e) {
  // var p = e.target.result;  // 要修改的原对象
  store.put(person);
};

// 监听失败事件
updatePersonRequest.onerror = function(e) {
  console.log(e.target.error);
};
```

- 数据库的 objectStore 删除

```js
// 创建一个事务，类型：IDBTransaction，文档地址： https://developer.mozilla.org/en-US/docs/Web/API/IDBTransaction
var transaction = db.transaction(personStore, 'readwrite');

// 通过事务来获取IDBObjectStore
var store = transaction.objectStore(personStore);
store.delete(6).onsuccess = function(e) {
  console.log(删除成功！)
};
```

- 根据 id 获取数据

```js
// 创建一个事务，类型：IDBTransaction，文档地址： https://developer.mozilla.org/en-US/docs/Web/API/IDBTransaction
var transaction = db.transaction(personStore, 'readwrite');

// 通过事务来获取IDBObjectStore
var store = transaction.objectStore(personStore);
store.get(6).onsuccess = function(e) {
  console.log(删除成功！)
};
```

- 数据库的 objectStore 游标查询

```js
var trans = db.transaction(personStore, 'readwrite');
var store = trans.objectStore(personStore);
var cursorRequest = store.openCursor();
cursorRequest.onsuccess = function(e) {
  var cursor = e.target.result;
  if (cursor) {
    var html = template('tbTmpl', cursor.value);
    document.getElementById('tbd').innerHTML += html;
    cursor.continue(); // 游标继续往下 搜索，重复触发 onsuccess方法，如果到最后返回null
  }
};
```

## 完整案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="./lib/art_template.js"></script>
</head>
<body>
  <table>
    <tr>
      <td>
        <label for="name">用户名</label>
      </td>
      <td>
        <input type="text" name="name" id="name">
      </td>
    </tr>
    <tr>
      <td>
        <label for="phone">电话</label>
      </td>
      <td>
        <input type="text" name="phone" id="phone">
      </td>
    </tr>
    <tr>
      <td>
        <label for="address">地址</label>
      </td>
      <td>
        <input type="text" name="address" id="address">
      </td>
    </tr>
  </table>
  <input type="button" value="添加用户" id="btnAdd" onclick="addPerson()">
  <table>
    <thead>
      <tr>
        <th>id</th>
        <th>name</th>
        <th>address</th>
        <th>phone</th>
        <th>编辑</th>
      </tr>
    </thead>
    <tbody id="tbd">
    </tbody>
  </table>
  <script id="tbTmpl" type="text/html">
    <tr>
      <td>{{id}}</td>
      <td>{{name}}</td>
      <td>{{phone}}</td>
      <td>{{address}}</td>
      <td><a href="#">修改</a>
      <a href="#" onclick="delById({{id}})">删除</a></td>
    </tr>
  </script>
  <script>
    var db, dbName = 'demoDb', dbVersion = 1, personStore = 'person';
    // 创建indexedDB对象，兼容各种浏览器
    var indexedDB = window.indexedDB || window.webkitIndexedDB || window.mozIndexedDB || window.msIndexedDB;
    if (!indexedDB) {
      console.log("你的浏览器不支持IndexedDB");
    }

    openIndexedDB(loadTableData);

    // 配合游标遍历表中数据，并配合art-template生成html
    function loadTableData() {
      document.getElementById('tbd').innerHTML = "";
      var trans = db.transaction(personStore, 'readwrite');
      var store = trans.objectStore(personStore);
      var cursorRequest = store.openCursor();
      cursorRequest.onsuccess = function (e) {
        var cursor = e.target.result;
        if (cursor) {
          var html = template('tbTmpl', cursor.value);
          document.getElementById('tbd').innerHTML += html;
          cursor.continue(); // 游标继续往下 搜索，重复触发 onsuccess方法，如果到到返回null
        }
      }
    }

    function delById(id) {
      if (!db || !id) {
        return;
      }
      // 创建一个事务
      var transaction = db.transaction(personStore, 'readwrite');

      // 通过事务来获取store
      var store = transaction.objectStore(personStore);

      // 删除请求
      var delPersonRequest = store.delete(id);
      delPersonRequest.onsuccess = function (e) {
        loadTableData(); // 删除成功后，重新加载数据
      }
      delPersonRequest.onerror = function (e) {
        console.log(e.target.error);
      }
    }

    // 添加用户
    function addPerson() {
      if (!db) {
        return;
      }
      var pName = document.getElementById('name').value;
      var pPhone = document.getElementById('phone').value;
      var pAddress = document.getElementById('address').value;
      // 创建一个事务
      var transaction = db.transaction(personStore, 'readwrite');

      // 通过事务来获取store
      var store = transaction.objectStore(personStore);

      var addPersonRequest = store.add({ name: pName, phone: pPhone, address: pAddress });
      addPersonRequest.onsuccess = function (e) {
        console.log(e.target);
        loadTableData(); // 添加成功后重新加载数据
      }
      addPersonRequest.onerror = function (e) {
        console.log(e.target.error);
      }
    }

    // 打开数据库
    function openIndexedDB(callback) {
      // 打开一个数据库
      var request = indexedDB.open(dbName, dbVersion);

      // 打开失败
      request.onerror = function (e) {
        console.log(e.currentTarget.error.message);
      };

      // 打开成功！
      request.onsuccess = function (e) {
        db = e.target.result;
        console.log('成功打开DB');
        callback();
      };

      // 打开成功后，如果版本有变化自动执行以下事件
      request.onupgradeneeded = function (e) {
        var db = e.target.result;
        if (!db.objectStoreNames.contains(personStore)) {
          console.log("我需要创建一个新的存储对象");
          //如果表格不存在，创建一个新的表格（keyPath，主键 ； autoIncrement,是否自增），会返回一个对象（objectStore）
          var objectStore = db.createObjectStore(personStore, {
            keyPath: "id",
            autoIncrement: true
          });

          //指定可以被索引的字段，unique字段是否唯一, 指定索引可以加快搜索效率。
          objectStore.createIndex("name", "name", {
            unique: true
          });
          objectStore.createIndex("phone", "phone", {
            unique: false
          });
        }
        console.log('数据库版本更改为： ' + dbVersion);
      };
    }
  </script>
</body>
</html>
```

另外一个封装的案例

```html
<!DOCTYPE HTML>
<html>
<head>
  <title>aicoder.com</title>
</head>
<body>
  <script type="text/javascript">
    function openDB(name, version) {
      var version = version || 1;
      var request = window.indexedDB.open(name, version);
      request.onerror = function (e) {
        console.log(e.currentTarget.error.message);
      };
      request.onsuccess = function (e) {
        myDB.db = e.target.result;
      };
      request.onupgradeneeded = function (e) {
        var db = e.target.result;
        if (!db.objectStoreNames.contains('students')) {
          var store = db.createObjectStore('students', { keyPath: 'id' });
          store.createIndex('nameIndex', 'name', { unique: true });
          store.createIndex('ageIndex', 'age', { unique: false });
        }
        console.log('DB version changed to ' + version);
      };
    }

    function closeDB(db) {
      db.close();
    }

    function deleteDB(name) {
      indexedDB.deleteDatabase(name);
    }

    function addData(db, storeName) {
      var transaction = db.transaction(storeName, 'readwrite');
      var store = transaction.objectStore(storeName);

      for (var i = 0; i < students.length; i++) {
        store.add(students[i]);
      }
    }

    function getDataByKey(db, storeName, value) {
      var transaction = db.transaction(storeName, 'readwrite');
      var store = transaction.objectStore(storeName);
      var request = store.get(value);
      request.onsuccess = function (e) {
        var student = e.target.result;
        console.log(student.name);
      };
    }

    function updateDataByKey(db, storeName, value) {
      var transaction = db.transaction(storeName, 'readwrite');
      var store = transaction.objectStore(storeName);
      var request = store.get(value);
      request.onsuccess = function (e) {
        var student = e.target.result;
        student.age = 35;
        store.put(student);
      };
    }

    function deleteDataByKey(db, storeName, value) {
      var transaction = db.transaction(storeName, 'readwrite');
      var store = transaction.objectStore(storeName);
      store.delete(value);
    }

    function clearObjectStore(db, storeName) {
      var transaction = db.transaction(storeName, 'readwrite');
      var store = transaction.objectStore(storeName);
      store.clear();
    }

    function deleteObjectStore(db, storeName) {
      var transaction = db.transaction(storeName, 'versionchange');
      db.deleteObjectStore(storeName);
    }

    function fetchStoreByCursor(db, storeName) {
      var transaction = db.transaction(storeName);
      var store = transaction.objectStore(storeName);
      var request = store.openCursor();
      request.onsuccess = function (e) {
        var cursor = e.target.result;
        if (cursor) {
          console.log(cursor.key);
          var currentStudent = cursor.value;
          console.log(currentStudent.name);
          cursor.continue();
        }
      };
    }

    function getDataByIndex(db, storeName) {
      var transaction = db.transaction(storeName);
      var store = transaction.objectStore(storeName);
      var index = store.index("ageIndex");
      index.get(26).onsuccess = function (e) {
        var student = e.target.result;
        console.log(student.id);
      }
    }

    function getMultipleData(db, storeName) {
      var transaction = db.transaction(storeName);
      var store = transaction.objectStore(storeName);
      var index = store.index("nameIndex");
      var request = index.openCursor(null, IDBCursor.prev);
      request.onsuccess = function (e) {
        var cursor = e.target.result;
        if (cursor) {
          var student = cursor.value;
          console.log(student.name);
          cursor.continue();
        }
      }
    }

    var myDB = {
      name: 'test',
      version: 1,
      db: null
    };

    var students = [{
      id: 1001,
      name: "Byron",
      age: 24
    }, {
      id: 1002,
      name: "Frank",
      age: 30
    }, {
      id: 1003,
      name: "Aaron",
      age: 26
    }, {
      id: 1004,
      name: "Casper",
      age: 26
    }];
  </script>
</body>
</html>
```

## 参考

1.  [HTML5 本地存储——IndexedDB](https://www.cnblogs.com/dolphinX/p/3416889.html)
1.  [MDN](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
1.  [indexedDB 标准](https://www.w3.org/TR/IndexedDB/#database-connection)
