# 和风天气查询 Skill

基于和风天气API的气象数据查询服务，提供天气预报、空气质量、生活指数、气象预警、天文信息等多种气象数据查询功能。

## 功能特性

- **基础天气**：实时天气、3-30天预报、逐小时预报、历史天气
- **空气质量**：实时AQI、逐小时/逐日预报、监测站数据、历史数据
- **生活指数**：16种指数（运动、洗车、穿衣、感冒、紫外线等）
- **气象预警**：台风、暴雨、高温等灾害预警
- **天文数据**：日出日落、月相数据（未来60天）
- **分钟级降水**：未来2小时5分钟级预报
- **格点天气**：高分辨率数值模式数据
- **地理信息**：热门城市、POI搜索

## 安装

### 1. 克隆仓库

```bash
git clone <repository-url>
cd hefeng-weather-skill
```

### 2. 安装依赖

```bash
pip install httpx pyjwt python-dotenv cryptography
```

### 3. 配置API

#### 方式一：代码配置（推荐）

```python
from skill import configure

# API KEY方式（推荐）
configure(
    api_host="your-domain.qweatherapi.com",
    api_key="your_api_key_here"
)

# 或 JWT方式
configure(
    api_host="your-domain.qweatherapi.com",
    project_id="your_project_id",
    key_id="your_key_id",
    private_key_path="./ed25519-private.pem"
)
```

#### 方式二：手动创建配置文件

创建 `~/.config/qweather/.env`：

```bash
mkdir -p ~/.config/qweather
cat > ~/.config/qweather/.env << 'EOF'
HEFENG_API_HOST=your-domain.qweatherapi.com
HEFENG_API_KEY=your_api_key_here
EOF
```

#### 方式三：环境变量

```bash
export HEFENG_API_HOST=your-domain.qweatherapi.com
export HEFENG_API_KEY=your_api_key_here
```

## 快速开始

```python
from skill import (
    get_weather_now,
    get_weather,
    get_air_quality,
    get_indices,
    get_warning
)

# 查询实时天气
result = get_weather_now(city="北京")
print(f"当前温度: {result['now']['temp']}°C")

# 查询3天预报
result = get_weather(city="上海", days="3d")
for day in result['daily']:
    print(f"{day['fxDate']}: {day['textDay']}, {day['tempMin']}~{day['tempMax']}°C")

# 查询空气质量
result = get_air_quality(city="广州")
print(f"AQI: {result['indexes'][0]['aqi']}")

# 查询生活指数
result = get_indices(city="深圳", days="1d")
for index in result['daily']:
    print(f"{index['name']}: {index['category']}")

# 查询气象预警
result = get_warning(city="杭州")
if result.get('warning'):
    for w in result['warning']:
        print(f"{w['title']}: {w['text']}")
```

## API参考

### 配置函数

#### `configure(api_host, api_key, ...)`

配置API认证信息。

**参数：**
- `api_host` (str): API主机地址
- `api_key` (str, optional): API KEY
- `project_id` (str, optional): JWT项目ID
- `key_id` (str, optional): JWT密钥ID
- `private_key_path` (str, optional): 私钥文件路径
- `private_key` (str, optional): 私钥内容
- `save_to_file` (bool): 是否保存到配置文件，默认True

**返回：** `{"success": bool, "message": str, "config_file": str}`

---

### 基础天气工具

#### `get_weather_now(city, location, lang, unit)`
获取实时天气数据。

**参数：**
- `city` (str, optional): 城市名称
- `location` (str, optional): LocationID或经纬度
- `lang` (str): 语言，默认"zh"
- `unit` (str): 单位，"m"公制或"i"英制

#### `get_weather(city, days)`
获取3-30天天气预报。

**参数：**
- `city` (str): 城市名称
- `days` (str): 预报天数，"3d"/"7d"/"10d"/"15d"/"30d"

#### `get_hourly_weather(hours, city, location, lang, unit)`
获取逐小时天气预报。

**参数：**
- `hours` (str): 预报时长，"24h"/"72h"/"168h"
- `city`/`location`: 城市或位置

#### `get_weather_history(city, location, days, lang, unit)`
获取历史天气数据（最近10天）。

---

### 空气质量工具

#### `get_air_quality(city)`
获取实时空气质量。

#### `get_air_quality_hourly(location, hours, lang)`
逐小时空气质量预报。

#### `get_air_quality_daily(location, days, lang)`
逐日空气质量预报（最多15天）。

#### `get_air_quality_stations(station_id, lang)`
监测站污染物数据。

#### `get_air_quality_history(city, days, lang)`
历史空气质量数据。

---

### 生活指数工具

#### `get_indices(city, days, index_types)`
获取生活指数预报。

**指数类型：**
- 1-运动, 2-洗车, 3-穿衣, 4-感冒, 5-紫外线
- 6-旅游, 7-花粉过敏, 8-舒适度, 9-交通, 10-防晒
- 11-化妆, 12-空调, 13-晾晒, 14-钓鱼, 15-太阳镜, 16-空气污染

---

### 预警工具

#### `get_warning(city)`
获取气象灾害预警信息。

---

### 天文工具

#### `get_astronomy_sun(location, date, lang)`
获取日出日落时间。

**参数：**
- `location`: 城市名、LocationID或经纬度
- `date`: 日期，格式"yyyyMMdd"，支持今天到未来60天

#### `get_astronomy_moon(location, date, lang)`
获取月升月落和月相数据。

---

### 分钟级预报

#### `get_minutely_5m(location, lang)`
获取未来2小时5分钟级降水预报。

---

### 格点天气

#### `get_grid_weather_now(location, lang, unit)`
格点实时天气（3-5公里分辨率）。

#### `get_grid_weather_daily(location, days, lang, unit)`
格点每日预报。

#### `get_grid_weather_hourly(location, hours, lang, unit)`
格点逐小时预报。

---

### 地理信息

#### `get_top_cities(number, city_type, lang)`
获取热门城市列表。

**参数：**
- `number`: 返回数量，1-100
- `city_type`: "cn"/"world"/"overseas"

#### `search_poi(location, keyword, poi_type, ...)`
搜索兴趣点。

**POI类型：**
- `"scenic"`: 景点
- `"TSTA"`: 潮汐站点

#### `search_poi_range(location, poi_type, radius, ...)`
范围搜索POI。

## 获取和风天气API

1. 访问 [和风天气开发者平台](https://dev.qweather.com/)
2. 注册账号并创建项目
3. 获取API KEY或配置JWT认证
4. 获取API主机地址（商业版为自定义域名）

## 依赖

- Python >= 3.11
- httpx >= 0.25.0
- pyjwt >= 2.8.0
- python-dotenv >= 1.0.0
- cryptography >= 41.0.0

## 许可证

MIT License
