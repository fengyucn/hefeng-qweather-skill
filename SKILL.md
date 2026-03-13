# 和风天气查询 Skill

## 描述

基于和风天气API的气象数据查询服务，提供天气预报、空气质量、生活指数、气象预警、天文信息等多种气象数据查询功能。

## 位置

- **类型**: user
- **路径**: `/home/fengyu/.qoder/skills/hefeng-weather/`

## 工具

### 基础天气工具

#### get_weather
获取指定城市的天气预报（3-30天）

**参数:**
- `city` (string, required): 城市名称，如 '北京'、'上海'
- `days` (string, optional): 预报天数，支持 "3d"(默认)、"7d"、"10d"、"15d"、"30d"

**返回:** 包含天气预报的JSON数据

---

#### get_weather_now
获取实时天气数据

**参数:**
- `location` (string, optional): LocationID 或 "经度,纬度"
- `city` (string, optional): 城市名称（与location二选一）
- `lang` (string, optional): 语言，默认 "zh"
- `unit` (string, optional): 单位，"m" 公制（默认）或 "i" 英制

**返回:** 包含实时天气的JSON数据

---

#### get_hourly_weather
获取逐小时天气预报（24/72/168小时）

**参数:**
- `hours` (string, optional): 预报小时数，支持 "24h"(默认)、"72h"、"168h"
- `location` (string, optional): LocationID 或 "经度,纬度"
- `city` (string, optional): 城市名称（与location二选一）
- `lang` (string, optional): 语言，默认 "zh"
- `unit` (string, optional): 单位，默认 "m"

**返回:** 逐小时天气预报数据

---

#### get_weather_history
获取历史天气数据（最近10天）

**参数:**
- `location` (string, optional): LocationID 或 "经度,纬度"
- `city` (string, optional): 城市名称（与location二选一）
- `days` (integer, optional): 天数，1-10，默认 10
- `lang` (string, optional): 语言，默认 "zh"
- `unit` (string, optional): 单位，默认 "m"

**返回:** 历史天气数据字典

---

### 空气质量工具

#### get_air_quality
获取实时空气质量数据

**参数:**
- `city` (string, required): 城市名称

**返回:** 包含AQI、污染物浓度的JSON数据

---

#### get_air_quality_history
获取历史空气质量数据（最近10天）

**参数:**
- `city` (string, required): 城市名称
- `days` (integer, optional): 天数，1-10，默认 10
- `lang` (string, optional): 语言，默认 "zh"

**返回:** 历史空气质量数据字典

---

#### get_air_quality_hourly
获取逐小时空气质量预报

**参数:**
- `location` (string, required): 经纬度坐标，格式: "纬度,经度"
- `hours` (string, optional): 预报小时数，支持 "24h"(默认)、"72h"、"168h"
- `lang` (string, optional): 语言，默认 "zh"

**返回:** 逐小时空气质量预报数据

---

#### get_air_quality_daily
获取逐日空气质量预报（最多15天）

**参数:**
- `location` (string, required): 经纬度坐标，格式: "纬度,经度"
- `days` (string, optional): 预报天数，支持 "3d"(默认)、"7d"、"15d"
- `lang` (string, optional): 语言，默认 "zh"

**返回:** 逐日空气质量预报数据

---

#### get_air_quality_stations
获取空气质量监测站数据

**参数:**
- `station_id` (string, required): 监测站ID，如 "P58911"
- `lang` (string, optional): 语言，默认 "zh"

**返回:** 监测站污染物浓度数据

---

### 生活指数工具

#### get_indices
获取天气生活指数预报

**参数:**
- `city` (string, required): 城市名称
- `days` (string, optional): 预报天数，"1d" 或 "3d"，默认 "1d"
- `index_types` (string, optional): 指数类型ID，默认全部16种指数

**指数类型说明:**
- 1-运动指数, 2-洗车指数, 3-穿衣指数, 4-感冒指数, 5-紫外线指数
- 6-旅游指数, 7-花粉过敏指数, 8-舒适度指数, 9-交通指数, 10-防晒指数
- 11-化妆指数, 12-空调开启指数, 13-晾晒指数, 14-钓鱼指数, 15-太阳镜指数
- 16-空气污染扩散条件指数

**返回:** 生活指数预报数据

---

### 预警信息工具

#### get_warning
获取气象灾害预警信息

**参数:**
- `city` (string, required): 城市名称

**返回:** 包含台风、暴雨、高温等预警信息

---

### 天文数据工具

#### get_astronomy_sun
获取日出日落时间（全球任意地点，未来60天内）

**参数:**
- `location` (string, required): LocationID、经纬度或城市名
- `date` (string, required): 日期，格式 yyyyMMdd
- `lang` (string, optional): 语言，默认 "zh"

**返回:** 日出日落时间数据

---

#### get_astronomy_moon
获取月升月落和逐小时月相数据

**参数:**
- `location` (string, required): LocationID、经纬度或城市名
- `date` (string, required): 日期，格式 yyyyMMdd
- `lang` (string, optional): 语言，默认 "zh"

**返回:** 月升月落时间 + 24小时月相数据（含照明度）

---

### 分钟级预报工具

#### get_minutely_5m
获取未来2小时5分钟级降水预报

**参数:**
- `location` (string, required): 经纬度坐标或城市名
- `lang` (string, optional): 语言，默认 "zh"

**返回:** 分钟级降水预报数据

---

### 格点天气工具

#### get_grid_weather_now
获取格点实时天气数据（高分辨率数值模式）

**参数:**
- `location` (string, required): 经纬度坐标，如 "116.41,39.92"
- `lang` (string, optional): 语言，默认 "zh"
- `unit` (string, optional): 单位，默认 "m"

**返回:** 格点实时天气数据

---

#### get_grid_weather_daily
获取格点每日天气预报

**参数:**
- `location` (string, required): 经纬度坐标
- `days` (string, optional): 预报天数，"3d"(默认) 或 "7d"
- `lang` (string, optional): 语言，默认 "zh"
- `unit` (string, optional): 单位，默认 "m"

**返回:** 格点每日天气预报数据

---

#### get_grid_weather_hourly
获取格点逐小时天气预报

**参数:**
- `location` (string, required): 经纬度坐标
- `hours` (string, optional): 预报小时数，"24h"(默认) 或 "72h"
- `lang` (string, optional): 语言，默认 "zh"
- `unit` (string, optional): 单位，默认 "m"

**返回:** 格点逐小时天气预报数据

---

### 地理信息工具

#### get_top_cities
获取热门城市列表

**参数:**
- `number` (integer, optional): 返回数量，默认10，范围1-100
- `city_type` (string, optional): 城市类型，"cn"(默认)、"world"、"overseas"
- `lang` (string, optional): 语言，默认 "zh"

**返回:** 热门城市列表

---

#### search_poi
搜索兴趣点(POI)

**参数:**
- `location` (string, required): 经纬度、LocationID或城市名
- `keyword` (string, required): 搜索关键词
- `poi_type` (string, required): POI类型，"scenic"(景点) 或 "TSTA"(潮汐站点)
- `city` (string, optional): 限定搜索城市
- `radius` (integer, optional): 搜索半径(米)，默认5000，范围100-50000
- `page` (integer, optional): 页码，默认1
- `lang` (string, optional): 语言，默认 "zh"

**返回:** POI搜索结果

---

#### search_poi_range
在指定范围内搜索POI

**参数:**
- `location` (string, required): 中心点经纬度坐标
- `poi_type` (string, required): POI类型，"scenic" 或 "TSTA"
- `radius` (integer, optional): 搜索半径(公里)，默认5，范围1-50
- `city` (string, optional): 限定搜索城市
- `page` (integer, optional): 页码，默认1
- `lang` (string, optional): 语言，默认 "zh"

**返回:** 包含距离信息的POI搜索结果

---

## 环境变量

使用前需要配置以下环境变量（在`.env`文件中）：

### API KEY认证（推荐）
```bash
HEFENG_API_HOST=your-domain.qweatherapi.com
HEFENG_API_KEY=your_api_key_here
```

### JWT数字签名认证（备用）
```bash
HEFENG_API_HOST=your-domain.qweatherapi.com
HEFENG_PROJECT_ID=your_project_id
HEFENG_KEY_ID=your_key_id
HEFENG_PRIVATE_KEY_PATH=./ed25519-private.pem
```

## 使用示例

```python
# 查询北京实时天气
get_weather_now(city="北京")

# 查询上海3天天气预报
get_weather(city="上海", days="3d")

# 查询北京空气质量
get_air_quality(city="北京")

# 查询上海生活指数
get_indices(city="上海", days="1d")

# 查询日出日落时间
get_astronomy_sun(location="北京", date="20251029")

# 查询月相数据
get_astronomy_moon(location="上海", date="20251101")

# 查询分钟级降水预报
get_minutely_5m(location="116.38,39.91")
```

## 依赖

- Python >= 3.11
- httpx >= 0.25.0
- mcp >= 1.10.0
- pyjwt >= 2.8.0
- cryptography >= 41.0.0
- python-dotenv >= 1.0.0

## 安装

```bash
# 克隆仓库
git clone <repository-url>
cd hefeng-weather-skill

# 安装依赖
pip install -e .

# 配置环境变量
cp .env.example .env
# 编辑 .env 填入你的API配置
```
