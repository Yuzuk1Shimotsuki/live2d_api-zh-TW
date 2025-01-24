# Live2D API

Live2D 看板娘插件 (https://www.fghrsh.net/post/123.html) 使用的後端 API

### 特性

- 原生 PHP 開發，無需偽靜態，開箱即用  
- 支援 模型、皮膚 的 順序切換 和 隨機切換  
- 支援 單模型 單皮膚 切換、多組皮膚 遞歸窮舉  
- 支援 同分組 多個模型 或 多個路徑 的 加載切換  

## 使用

### 環境需求

- PHP 版本 >= 5.2  
- 依賴 PHP 擴展：json  

### 目錄結構

```shell
│  model_list.json              // 模型列表
│
├─model                         // 模型路徑
│  └─GroupName                  // 模組分組
│      └─ModelName              // 模型名稱
│
├─add                           // 更新皮膚列表
├─get                           // 獲取模型配置
├─rand                          // 隨機切換模型
├─rand_textures                 // 隨機切換皮膚
├─switch                        // 順序切換模型
├─switch_textures               // 順序切換皮膚
└─tools
        modelList.php           // 列出模型列表
        modelTextures.php       // 列出皮膚列表
        name-to-lower.php       // 檔案名格式化
```

### 添加模型

- 單模型 單皮膚 切換  
    - 單次加載只輸出一個皮膚  
    - 皮膚放在 `textures` 資料夾，系統自動識別  

```shell
│  index.json
│  model.moc
│  textures.cache       // 皮膚列表緩存，系統自動生成
│
├─motions
│      idle_01.mtn
│      idle_02.mtn
│      idle_03.mtn
│
└─textures
        default-costume.png
        school-costume.png
        winter-costume.png
```

- 單模型 多組皮膚 遞歸窮舉  
    - 多組皮膚 組合模型、窮舉組合  
    - 皮膚資料夾按 `texture_XX` 命名  
    - 添加 `textures_order.json` 列出組合  

```shell
│  index.json
│  model.moc
│  textures.cache
│  textures_order.json
│
├─motions
│      idle_01.mtn
│      idle_02.mtn
│      idle_03.mtn
│
├─texture_00
│      00.png
│
├─texture_01
│      00.png
│      01.png
│      02.png
│
├─texture_02
│      00.png
│      01.png
│      02.png
│
└─texture_03
       00.png
       01.png
```

**textures_order.json**

```json
[
    ["texture_00"],
    ["texture_01","texture_02"],
    ["texture_03"]
]
```

**textures.cache**

```json
[
    ["texture_00/00.png","texture_01/00.png","texture_02/00.png","texture_03/00.png"],
    ["texture_00/00.png","texture_01/00.png","texture_02/00.png","texture_03/01.png"],
    ["texture_00/00.png","texture_01/01.png","texture_02/01.png","texture_03/00.png"],
    ["texture_00/00.png","texture_01/01.png","texture_02/01.png","texture_03/01.png"],
    ["texture_00/00.png","texture_01/02.png","texture_02/02.png","texture_03/00.png"],
    ["texture_00/00.png","texture_01/02.png","texture_02/02.png","texture_03/01.png"]
]
```

- 同分組 多個模型 或 多個路徑 切換  
    - 修改 `model_list.json` 添加多個模型  

```shell
│
├─model
│  ├─Group1
│  │  ├─Model1
│  │  │      index.json
│  │  │
│  │  └─Model2
│  │          index.json
│  │
│  ├─Group2
│  │  └─Model1
│  │          index.json
│  │
│  └─GroupName
│     └─ModelName
│          │  index.json
│          │  model.moc
│          │
│          ├─motions
│          └─textures
│
```

**model_list.json**

```json
{
    "models": [
        "GroupName/ModelName",
        [
            "Group1/Model1",
            "Group1/Model2",
            "Group2/Model1"
        ]
    ],
    "messages": [
        "Example 1",
        "Example 2"
    ]
}
```

### 接口用法

- `/add/` - 檢測 新增皮膚 並更新 緩存列表  
- `/get/?id=1-23` - 獲取 分組 1 的 第 23 號 皮膚  
- `/rand/?id=1` - 根據 上一分組 隨機切換  
- `/switch/?id=1` - 根據 上一分組 順序切換  
- `/rand_textures/?id=1-23` - 根據 上一皮膚 隨機切換 同分組其他皮膚  
- `/switch_textures/?id=1-23` - 根據 上一皮膚 順序切換 同分組其他皮膚  

## 版權聲明

> (>▽<) 都看到這裡了，點個 Star 吧 ~

**API 內所有模型版權均屬於原作者，僅供研究學習，不得用於商業用途**  

MIT © FGHRSH
