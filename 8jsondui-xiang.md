## JSON序列化



```
var lbp = {"name":"lbp",age:20,hobby:{"sport":"ping-pang","food":"delicious"}};
JSON.stringify(lbp)                //"{"name":"lbp","age":20,"hobby":{"sport":"ping-pang","food":"delicious"}}"


1、将输出美化
JSON.stringify(lbp,null," ")    

/*
"{
 "name": "lbp",
 "age": 20,
 "hobby": {
  "sport": "ping-pang",
  "food": "delicious"
 }
}"
*/

2、选择需要输出的属性
JSON.stringify(lbp,["age"])


3、还可以传入一个函数，这样对象的每个键值对都会被函数先处理
function convert(key,value){ 
       if(typeof value === "string"){ 
          return value.toUpperCase(); 
       }
       return value;   
}  
var lbp = {"name":"lbp",age:20,hobby:{"sport":"ping-pang","food":"delicious"}};
JSON.stringify(lbp,convert)
//"{"name":"LBP","age":20,"hobby":{"sport":"PING-PANG","food":"DELICIOUS"}}"


4、如果我们需要精确地控制如何序列化对象，可以给我们的对象定义一个 toJSON() 的方法。直接返回JSON需要显示的数据
 var lbp = {"name":"lbp",
             age:20,
             hobby:{
                  "sport":"ping-pang",
                  "food":"delicious"
             },
             toJSON:function(){ 
               return {"name":"杭城小刘","age":22 } 
              }
            }; 
JSON.stringify(lbp);
//"{"name":"杭城小刘","age":22}" 


5、JSON.parse() 函数可以接收一个函数用来转换解析出的属性
	var obj = JSON.parse('{"name":"杭城小刘","age":"20"}', function(key, value) {
			if(key === "name") {
				return value + "同学";
			}
			return value;
		});
		console.log(JSON.stringify(obj));
		//{"name":"杭城小刘同学","age":"20"}

```

## 



