# Promise-polyfill

#### 项目介绍

- Promise-polyfill

#### IE下测试

- 基础函数

```javascript
  function getData(status) {
    return new Promise(function (resolve, reject) {
      setTimeout(function () {
        var num = Math.random();
        if (num > 0.5 || status) {
          resolve({
            status: 1,
            num: num
          });
        } else {
          reject({
            status: 2,
            num: num
          });
        }
      }, 500);
    });
  }
```

- 基本用法

```javascript
  getData()
    .then(function (res) {
      console.log(res);
    })
    .catch(function (err) {
      console.error(err);
    })
    .finally(function () {
      console.log('finally');
    });
```

- Promise.all

```javascript
  var promiseAll = Promise.all([
    getData(true),
    getData(true),
    (function () {
      return new Promise(function (resolve, reject) {
        setTimeout(function () {
          resolve(1);
        }, 2000);
      });
    }())
  ]);

  promiseAll
    .then(function (res) {
      console.log(res); // 2秒后 [{…}, {…}, 1]
    })
    .catch(function (err) {
      console.error(err);
    });
```

- Promise.race

```javascript
  var promiseRace = Promise.race([
    getData(true),
    getData(true),
    (function () {
      return new Promise(function (resolve, reject) {
        setTimeout(function () {
          reject(2);
        }, 200);
      });
    }())
  ]);
  promiseRace
    .then(function (res) {
      console.log(res);
    })
    .catch(function (err) {
      console.error(err); // 200毫秒后
    });
```

- Promise.resolve

```javascript
  Promise.resolve([1, 2])
    .then(function (res) {
      console.log(res); // [1, 2]
    });
  
  Promise.resolve([1, 2])
    .then(function (res) {
      console.log(abc); // abc is not defined
    })
    .catch(function (err) {
      console.log(err); // err msg
    });
```

- Promise.reject

```javascript
  Promise.reject('reject status msg')
    .catch(function (err) {
      console.log(err);   // 'reject status msg'
    });
```

