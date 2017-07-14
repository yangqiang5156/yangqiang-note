* js数组常用方法

```js
var data= [{
    "lon": 116.73722839355469,
    "so2": 10,
    "status": 2,
    "o3": 2,
    "street": "顺义区",
    "most": "pm25",
    "type": "gk",
    "id": "sybeixiaoying",
    "time": "2017-04-07 23:00:00",
    "co": 1,
    "no2": 99,
    "name": "顺义北小营",
    "aqi": 189,
    "dtype": "市控",
    "pm10": 198,
    "pm25": 142,
    "lat": 40.23813247680664
},
{
    "lon": 116.161034,
    "so2": -1,
    "status": 3,
    "o3": -1,
    "street": "首钢区域",
    "most": "pm10",
    "type": "atlas",
    "id": "A1YQ520B02AE0514",
    "time": "2017-04-11 15:00:00",
    "co": -1,
    "no2": -1,
    "name": "SJS031首钢制氧厂",
    "aqi": 200,
    "dtype": "区控",
    "pm10": 205,
    "pm25": 71,
    "lat": 39.915047
},
{
    "lon": 116.68277740478516,
    "so2": 2,
    "status": 1,
    "o3": 108,
    "street": "通州区",
    "most": "pm25",
    "type": "gk",
    "id": "tzdongguan",
    "time": "2017-04-11 14:00:00",
    "co": 0.20000000298023224,
    "no2": 12,
    "name": "通州东关",
    "aqi": 118,
    "dtype": "市控",
    "pm10": -1,
    "pm25": 89,
    "lat": 39.914886474609375
}]

//----------------按照字段排序---------------------
//按照aqi排序
function sortAqi(a,b){
    return a.aqi-b.aqi
}
var sortData = data.sort(sortAqi);  //返回按照aqi 升序排列的3个数组对象，如果要降序，return b.aqi-a.aqi;
//sortData.slice(0,2)  返回排序后的前2个

//----------------按照条件筛选----------
//排除dtype 为区控 的数组对象；
var filterData = data.filter(function(item){
	if(item.dtype =='区控'){
		return false;
	}else{
		return item;
	}	
})//结果返回dtype不为区控的剩余2个对象数组

//----------------根据条件改变满足条件的对象属性值--------------

//状态status为3的对象属性改变值
var newData = data.map(function(item){
    if(item.status==3){
	 	item.aqi=-1;
		item.pm25=-1;
	 	item.pm10 = -1;
	 	item.so2 = -1;
	 	item.no2 = -1;
	 	item.co = -1;
	 	item.o3 = -1;
	 	item.most = '无';
	 }
	 return item;   	
})  // 返回按条件修改后的3个对象数组


```