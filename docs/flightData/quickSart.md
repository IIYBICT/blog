# 接口文档
## 查询机场信息（Query Airport Info）

### 基本信息

- **接口名称**：查询机场信息
- **请求服务器**：`https://api.deteam.cn`
- **请求地址**：`/flightData/airport/query/info`
- **请求方法**：[GET， POST]
- **接口描述**：每分钟请求10次

### 功能描述

通过提供机场的 ICAO 编码和授权密钥，获取对应机场的详细信息。

### 请求参数说明

| 参数名     | 类型   | 是否必填 | 示例值      | 说明                                                                       |
| ---------- | ------ | -------- | ----------- | -------------------------------------------------------------------------- |
| `icaoCode` | string | 否       | `ZBAA`      | 机场的 ICAO 四字编码，可通过路径或查询字符串传入                           |
| `key`      | string | 是       | `abc123xyz` | 授权密钥，用于身份验证，前往[授权系统](https://warrant.deteam.cn) 免费获取 |

> 注意：如果同时通过路径和查询字符串传递 `icaoCode`，将优先使用路径中的值。

### 请求头

```http
Content-Type: application/json
```


### 成功响应示例（HTTP 200）

```json
{
  "code": 0,
  "message": "success",
  "data": {
    "id": 15818,
    "name": "南宁吴圩国际机场",
    "icao": "ZGNN",
    "primaryId": 0,
    "latitude": 22.61,
    "longitude": 108.17333333,
    "elevation": 420,
    "transitionAltitude": 10270,
    "transitionLevel": 11800,
    "speedLimit": 250,
    "speedLimitAltitude": 10000,
    "runways": [
      {
        "id": 39439,
        "airportId": 15818,
        "ident": "05",
        "trueHeading": 46.66,
        "length": 10499,
        "width": 148,
        "surface": "ASP",
        "latitude": 22.59861389,
        "longitude": 108.16415,
        "elevation": 417
      },
      {
        "id": 39440,
        "airportId": 15818,
        "ident": "23",
        "trueHeading": 226.67,
        "length": 10499,
        "width": 148,
        "surface": "ASP",
        "latitude": 22.61838333,
        "longitude": 108.18684722,
        "elevation": 415
      }
    ]
  }
}
```


---

## 📌 字段详细说明

### 🔹 根对象（Top-level）

| 字段名    | 类型   | 必填 | 描述               |
| --------- | ------ | ---- | ------------------ |
| `code`    | int    | 是   | 状态码，0 表示成功 |
| `message` | string | 是   | 响应描述信息       |
| `data`    | object | 是   | 机场数据主体       |

---

### 🛫 `data`参数

| 字段名               | 类型     | 必填 | 示例值               | 描述                                 |
| -------------------- | -------- | ---- | -------------------- | ------------------------------------ |
| `name`               | string   | 是   | `"南宁吴圩国际机场"` | 机场名称                             |
| `icao`               | string   | 是   | `"ZGNN"`             | ICAO 四字代码                        |
| `latitude`           | float64  | 是   | `22.61`              | 机场纬度坐标                         |
| `longitude`          | float64  | 是   | `108.17333333`       | 机场经度坐标                         |
| `elevation`          | int64    | 是   | `420`                | 海拔高度（单位：米）                 |
| `transitionAltitude` | int64    | 是   | `10270`              | 过渡高度（单位：英尺）               |
| `transitionLevel`    | int64    | 是   | `11800`              | 过渡高度层（单位：英尺）             |
| `speedLimit`         | int64    | 是   | `250`                | 速度限制（单位：节）                 |
| `speedLimitAltitude` | int64    | 是   | `10000`              | 速度限制适用的最大高度（单位：英尺） |
| `runways`            | []Runway | 是   | 数组                 | 跑道列表                             |

---

### 🛬 `Runway`参数

| 字段名        | 类型    | 必填 | 示例值        | 描述                           |
| ------------- | ------- | ---- | ------------- | ------------------------------ |
| `ident`       | string  | 是   | `"05"`        | 跑道编号（如：05/23）          |
| `trueHeading` | float64 | 是   | `46.66`       | 真航向（True Heading）         |
| `length`      | int64   | 是   | `10499`       | 跑道长度（单位：米）           |
| `width`       | int64   | 是   | `148`         | 跑道宽度（单位：米）           |
| `surface`     | string  | 是   | `"ASP"`       | 道面材质（如 ASP=沥青）        |
| `latitude`    | float64 | 是   | `22.59861389` | 跑道中心点纬度                 |
| `longitude`   | float64 | 是   | `108.16415`   | 跑道中心点经度                 |
| `elevation`   | int64   | 是   | `417`         | 跑道中心点海拔高度（单位：米） |


### 常见错误码说明

| 状态码 | 描述                                   |
| ------ | -------------------------------------- |
| 403    | 授权失败，可能是未传 key 或 key 已过期 |
| 500    | 内部服务器错误，如数据查询失败         |

### 使用场景示例

#### 示例

```bash
GET /flightData/airport/query/info?icaoCode=ZBAA&key=abc123xyz
```


### 用户注意事项

1. 每个 `key` 都有有效期，请确保使用的是有效授权密钥。
2. 若频繁调用接口，请注意控制频率，避免触发限流机制。
3. 如果返回 403 错误，请检查 `key` 是否正确或联系管理员重新申请授权。

---

如果你还需要加入权限说明、调用频率限制、或配合其他接口的使用流程图，也可以继续告诉我。