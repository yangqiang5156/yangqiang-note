* lodash 常用方法

```js
//模拟数据格式
var  data =  {
        "顺义区": [
            {
                "address": "顺义区",
                "appGkStation": {
                    "deviceidApp": "shunyi",
                    "showType": 1
                },
                "city": "北京",
                "deviceid": "shunyi",
                "district": "顺义区",
                "dname": "顺义",
                "dtype": "national",
                "installtime": 1420041600000,
                "lat": 40.127,
                "lon": 116.655,
                "province": "北京市",
                "stationRank": {
                    "deviceidR": "shunyi",
                    "typeR": 0
                }
            },
            {
                "address": "顺义区",
                "appGkStation": {
                    "deviceidApp": "sybeixiaoying",
                    "showType": 1
                },
                "city": "北京",
                "deviceid": "sybeixiaoying",
                "district": "顺义区",
                "dname": "顺义北小营",
                "dtype": "city",
                "installtime": 1420041600000,
                "lat": 40.238133347826,
                "lon": 116.73722463008,
                "province": "北京市",
                "stationRank": {
                    "deviceidR": "sybeixiaoying",
                    "typeR": 0
                }
            }
        ],
       
        "东城区": [
            {
                "address": "东城区",
                "appGkStation": {
                    "deviceidApp": "dongsi",
                    "showType": 1
                },
                "city": "北京",
                "deviceid": "dongsi",
                "district": "东城区",
                "dname": "东四",
                "dtype": "national",
                "installtime": 1420041600000,
                "lat": 39.929,
                "lon": 116.417,
                "province": "北京市",
                "stationRank": {
                    "deviceidR": "dongsi",
                    "typeR": 1
                }
            }
        ],
        "昌平区": [
            {
                "address": "昌平区",
                "appGkStation": {
                    "deviceidApp": "badaling",
                    "showType": 1
                },
                "city": "北京",
                "deviceid": "badaling",
                "district": "昌平区",
                "dname": "八达岭",
                "dtype": "city",
                "installtime": 1420041600000,
                "lat": 40.365,
                "lon": 115.988,
                "province": "北京市",
                "stationRank": {
                    "deviceidR": "badaling",
                    "typeR": 0
                }
            }
        ]
}

//_.pick  创建一个从 object 中选中的属性的对象。
var newData = _.pick(data, ['顺义区','昌平区']); //新建了一个只包含key 为顺义区和昌平区的对象

```