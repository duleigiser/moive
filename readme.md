# imooc 第一季 mongodb + node + jade 电影网站

        jade,bower,mongodb使用

# schemas 学习

        最大的问题在于mongodb的使用。vscode对于node的调试很强大。。

## mongodb 安装,作为window服务启动。

        学习了mongodb基本的操作INSERT,UPDATE,GROUP,MapReduce;
           

# insert

    db.person.insert({"name":"jack"});

# find()

    db.person.find({"name":"jack"})

## find 高级版
	
### $gt;$lt;$gte;$lte;$ne;
	 
    db.person.find({"age":{$lt:22}})
    
### $or;$in;$nin;
>
    db.user.find({$or:[{"name":"jack"},{"name":"joe"}]})
    db.user.find({"address.province":{$in:["anhui","jiangsu"]}})

# update() 

  第一个参数查找条件，第二个为更新的值
  
    db.person.update({},{});
    
## 高级版 $inc;$set;
###   $inc 
      对于value增加;没有对应的key会创建
      db.user.update({ "name": "jack" }, { $inc: { "age": 30 } });
## upsert

    对于第一个参数没有查到的时候；在第三个参数设为true时候新加一条

## 批量更新
    
    如果匹配多条时候 默认只更新第一条；第4个参数为true时候批量更新

# remove

	db.person.remove();

# 聚合

## count

    db.user.count()
    db.user.count({"age":20})

## distinct 

    db.user.distinct("age");

## group

- key 分组key
- initial 每组都分享这个"初始化函数"
- $reduce 第一个参数 当前文档对象，第二个是上一次funciton操作的累积对象，第一次为initial的value；
   
		db.person.group({
			"key":{"age":true},
			"initial":{"person":[]},
				"$reduce":function(cur,out){
				out.person.push(cur.name)
			}
		})

- condition	过滤条件	
- finalize 每一组执行完毕会执行这个方法

		db.person.group({
			"key":{"age":true},
			"initial":{"person":[]},
			"$reduce":function(cur,out){
				out.person.push(cur.name)
			},
			"finalize":function(out){
				out.count = out.person.length;
			},
			"condition":{"age":{$lt:25}}
		})

## mapReduce

- map 这个称为映射函数；里面会调用emit(key,value),集合会按照你指定的key进行映射分组
- reduce 这个称为简化函数，会对map分组后的数据进行简化；
- mapReduce 这个就是最后执行的函数，参数为map，reduce和一些参数。



mongod --dbpath=D:\MongoDB\db
# 删除数据库
- show dbs 参看数据库数量
- db 返回所在的数据库位置
- show collections //在次检查是否正确
- db.dropDatabase();


db.movies.insert({"_id":1,"doctor":'javan',"country":'china',"title":'钢铁侠',"year":2014,"poster":'http://r3.ykimg.com/05160000530EEB63675839160D0B79D5',"language":'chinese',flash:'http://player.youku.com/player.php/sid/XNjA1Njc0NTUy/v.swf',"summary":"中国"})       