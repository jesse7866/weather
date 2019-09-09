# Weather

基于 [高德开发平台](https://lbs.amap.com/dev/id/newuser) 的 PHP 天气信息组件。

[![Build Status](https://travis-ci.org/jesse7866/weather.svg?branch=master)](https://travis-ci.org/jesse7866/weather)
![StyleCI build status](https://github.styleci.io/repos/206710094/shield)

## 安装

```sh
$ composer require jesse/weather -vvv
```

## 配置

在使用本扩展之前，你需要去 [高德开发平台](https://lbs.amap.com/dev/id/newuser) 注册账号，然后创建应用，获取应用的 API Key。

## 使用

```php
use Jesse\Weather\Weather;

$key = 'xxxxxx';

$weather = new Weather($key);

```

### 获取实时天气
```php
$response = $weather->getWeather('深圳');
```

实例：
```json
{
    "status": "1",
    "count": "1",
    "info": "OK",
    "infocode": "10000",
    "lives": [
        {
            "province": "广东",
            "city": "深圳市",
            "adcode": "440300",
            "weather": "多云",
            "temperature": "31",
            "winddirection": "东北",
            "windpower": "≤3",
            "humidity": "65",
            "reporttime": "2019-09-06 11:14:13"
        }
    ]
}
```

### 获取近期天气预报

```php
$response = $weather->getWeather('深圳', 'all');
```

实例：
```json
{
    "status":"1",
    "count":"1",
    "info":"OK",
    "infocode":"10000",
    "forecasts":[
        {
            "city":"深圳市",
            "adcode":"440300",
            "province":"广东",
            "reporttime":"2019-09-06 11:14:13",
            "casts":[
                {
                    "date":"2019-09-06",
                    "week":"5",
                    "dayweather":"雷阵雨",
                    "nightweather":"雷阵雨",
                    "daytemp":"33",
                    "nighttemp":"25",
                    "daywind":"无风向",
                    "nightwind":"无风向",
                    "daypower":"≤3",
                    "nightpower":"≤3"
                },
                {
                    "date":"2019-09-07",
                    "week":"6",
                    "dayweather":"雷阵雨",
                    "nightweather":"雷阵雨",
                    "daytemp":"34",
                    "nighttemp":"26",
                    "daywind":"无风向",
                    "nightwind":"无风向",
                    "daypower":"≤3",
                    "nightpower":"≤3"
                },
                {
                    "date":"2019-09-08",
                    "week":"7",
                    "dayweather":"雷阵雨",
                    "nightweather":"雷阵雨",
                    "daytemp":"33",
                    "nighttemp":"26",
                    "daywind":"无风向",
                    "nightwind":"无风向",
                    "daypower":"≤3",
                    "nightpower":"≤3"
                },
                {
                    "date":"2019-09-09",
                    "week":"1",
                    "dayweather":"阵雨",
                    "nightweather":"阵雨",
                    "daytemp":"32",
                    "nighttemp":"27",
                    "daywind":"无风向",
                    "nightwind":"无风向",
                    "daypower":"≤3",
                    "nightpower":"≤3"
                }
            ]
        }
    ]
}
```

### 获取 XML 格式返回值

第三个参数为返回值类型，可选 `json` 与 `xml`，默认 `json`

```php
$response = $weather->getWeather('深圳', 'all', 'xml');
```

实例：
```xml
<response>
  <status>1</status>
  <count>1</count>
  <info>OK</info>
  <infocode>10000</infocode>
  <lives type="list">
    <live>
      <province>广东</province>
      <city>深圳市</city>
      <adcode>440300</adcode>
      <weather>多云</weather>
      <temperature>31</temperature>
      <winddirection>东北</winddirection>
      <windpower>≤3</windpower>
      <humidity>65</humidity>
      <reporttime>2019-09-06 11:44:12</reporttime>
    </live>
  </lives>
</response>
```

### 参数说明

```
array|string getWeather(string $city, string $type = 'base', string $format = 'json')
```

>- `$city` - 城市名，比如：“深圳”；
>- `$type` - 返回内容类型：`base`: 返回实时天气 / `all`:返回预报天气；
>- `$format` - 输出的数据格式，默认为 `json` 格式，当 output 设置为 `xml` 时，输出的为 XML 格式的数据。

### 在 Laravel 中的使用

在 Laravel 中使用也是同样的安装方式，配置写在 `config/services.php` 中：

```php
    .
    .
    .
    'weather' => [
        'key' => env('WEATHER_API_KEY),
    ]
```

然后在 `.env` 中配置 `WEATHER_API_KEI` ：

```dotenv
WEATHER_API_KEY=xxxxxx
```

可以用两种方式来获取 `Jesse\Weather\Weather` 实例：

#### 方法参数注入

```php
	.
	.
	.
	public function edit(Weather $weather) 
	{
		$response = $weather->getWeather('深圳');
	}
	.
	.
	.
```

#### 服务名访问

```php
	.
	.
	.
	public function edit() 
	{
		$response = app('weather')->getWeather('深圳');
	}
	.
	.
	.
```

## 参考

- [高德开发平台天气接口](https://lbs.amap.com/api/webservice/guide/api/weatherinfo/)

## License

MIT