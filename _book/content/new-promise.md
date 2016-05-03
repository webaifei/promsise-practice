# 怎么使用？


> Promise 是es6提供的原生的对象， 用来管理js中异步操作难搞的回调。

1. 使用new来创建一个Promise 实例

 ```
   var promise = new Promise( function ( resolve, reject ){
      //some code of the asynchronouse operation

      if(xxx){
        resolve(data);
      }else{
        reject( new Error('no data'));
      }
     });
 ```

2. 一个ajax的实例

```
  function getJson(url){
    var promise = new Promise( function (resolve, reject){
      var ajax = new XMLHttpRequest();
      ajax.open('GET', url);
      ajax.onreadystatechange = handle;
      ajax.responseType = 'json';
      ajax.setRequestHeader('Accept', 'application/json');
      ajax.send();

      function handle(){
        if( this.readyState != 4 ){
          return;
        }
        if( this.status == 200 || this.status == 302 ){
          resolve( this.response );
        }else{
          reject( new Error( this.statusText ) )
        }
      }
      });
      return promise;
  }
```

3. 下一步就是给我们的成功和失败添加回调了

  如果是在jq或者是zepto中， 我们可能这么写

  ```
    $.ajax({
      url:xxx,
      data:data,
      success:fn,
      error: fn
      })
  ```

  现在我们可以这么写：

  ```
    getJson('XXX')
      .then( function (res){
        //success
      }, function (err){
        //error
      }).then(function (res){
        //success
      }, function (){
        //error    
      })
      ...好多   

  ```
