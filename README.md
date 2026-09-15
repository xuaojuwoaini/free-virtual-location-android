# 一款完全免费的虚拟定位软件（安卓版）

> 摇杆控制位置 · 配速自由调整 · 悬浮窗摇杆 · 读取手机真实 GPS · 多地图源切换
> **完全免费、无广告、无内购、无加固、不联网上传任何数据**

![高德中文地图](docs/screenshots/01-高德中文地图.png)

## 功能特性

- **摇杆控制位置**：摇杆方向＝行进方向，离圆心越远速度越快（实际速度 = 配速 × 力度）；默认松手后继续移动，也可勾选"松手即停"。
- **配速自由调整**：预设 步行 1.4 / 跑步 3.0 / 骑行 6.0 / 驾车 15 / 高铁 80 / 飞机 250（m/s），滑杆无级 0.2~60，输入框可填 0.05~600，回车即生效，自动记住。
- **悬浮窗摇杆**：摇杆可以变成系统悬浮窗，盖在微信 / 地图 / 游戏上照样能用；拖动横条换位置，松手自动吸附左右边缘。
- **读取手机真实定位**：点「获取定位」持续读取 GPS / 网络定位，显示精度与来源；一键「设为起点」；还能开启「跟随定位」让模拟位置跟着真实定位走。地图上蓝点＝真实位置，绿图标＝模拟位置。
- **地图选点与轨迹**：单击地图即设为目标点，会按当前配速自动走过去并停下；实时绘制移动轨迹；可收藏常用位置。
- **多地图源**：自动测速选最快 / OSM 官方 / OSM 德国镜像 / Esri 世界街道图 / **高德地图（中文，自动 GCJ-02 坐标纠偏）**。个别区域没数据时会自动识别占位图并提示更换图源。
- **首启使用说明图**：第一次打开弹出图文说明（可滚动），确认后才进主界面；随时可在应用内重新查看。
- **真实位置注入**：以无 ROOT 方式向系统写入模拟位置提供者（gps + network），其它 App 读取到的就是设定的位置。

## 下载安装

### 国内可直接下载（GitHub 打不开时用这两个）

| 加速源 | 下载地址 |
| --- | --- |
| jsDelivr CDN（最快） | https://cdn.jsdelivr.net/gh/xuaojuwoaini/free-virtual-location-android@main/dist/MockLocation-1.5.apk |
| ghproxy 加速 | https://ghproxy.net/https://github.com/xuaojuwoaini/free-virtual-location-android/releases/download/v1.5/MockLocation-1.5.apk |

### 官方地址

| 方式 | 说明 |
| --- | --- |
| 直链 | [dist/MockLocation-1.5.apk](dist/MockLocation-1.5.apk) |
| Releases | 见本仓库 [Releases](https://github.com/xuaojuwoaini/free-virtual-location-android/releases) 页面 |

**文件信息**：`MockLocation-1.5.apk`，301,534 字节
**SHA-256**：`94D85452F76618CA3C5E7099666CCE47A83CE3A666F91849F645E34219F5FFEF`

> 下载后可用上面的 SHA-256 校验文件完整性；小米/华为等机型安装"未知来源应用"时按提示允许即可。

> 注意：`dist/` 里的 APK 使用开发者签名；如果你自己重新构建，会生成新的签名，
> 两者不能互相覆盖安装（需要先卸载旧版本）。建议二选一：直接用 dist 里的，或自己构建后一直用它。

安装后需要做两步（否则模拟不生效）：

1. 开启开发者选项：「设置 → 关于手机 → 连续点击版本号 7 次」，然后进入「设置 → 系统 → 开发者选项 → **选择模拟位置信息应用**」→ 选中「**模拟定位**」。
2. 回到 App 点绿色「**开始模拟**」；状态显示「等待授权」就是第 1 步没选上。

更详细的图文说明见 [docs/使用说明.md](docs/使用说明.md)。

## 截图

| 真实 GPS 定位 | 悬浮窗摇杆 |
| --- | --- |
| ![真实GPS](docs/screenshots/03-真实GPS定位.png) | ![悬浮窗摇杆](docs/screenshots/04-悬浮窗摇杆.png) |

| 首启使用说明图 |
| --- |
| ![说明图](docs/screenshots/02-首启说明图.png) |

## 从源码构建

不依赖 Gradle，不需要 Android Studio，一条命令出一个签名的 APK（只需 JDK 11+ 和 Android SDK）。

```powershell
# 需要 JDK（含 javac/keytool）和 Android SDK（build-tools + 任意一个 platform）
powershell -ExecutionPolicy Bypass -File build.ps1

# 也可以显式指定路径
powershell -ExecutionPolicy Bypass -File build.ps1 -SdkRoot "C:\Android\sdk" -JavaHome "C:\Program Files\Java\jdk-17"
```

产物：`build/MockLocation-1.5.apk`（v1 + v2 + v3 签名）。

> 首次构建会自动生成签名密钥库 `keystore/mockloc.jks`（口令 `mockloc123`）。
> **本仓库不包含签名密钥**（已在 `.gitignore` 中排除）——请自行备份生成好的 keystore，
> 因为安卓要求"覆盖升级必须使用同一个签名"。

跑单元测试（纯 Java，无需安卓环境）：

```powershell
javac -encoding UTF-8 -d testout app\java\com\codex\mocklocation\Geo.java `
      app\java\com\codex\mocklocation\MotionEngine.java tests\EngineTest.java
java -cp testout EngineTest
```

重新生成首启说明图 / 位置图标（改文字后重跑即可）：

```powershell
powershell -ExecutionPolicy Bypass -File tools\gen_guide.ps1
powershell -ExecutionPolicy Bypass -File tools\gen_marker.ps1
```

## 项目结构

| 路径 | 作用 |
| --- | --- |
| `app/AndroidManifest.xml` | 清单（声明 `ACCESS_MOCK_LOCATION` 才会出现在"选择模拟位置信息应用"列表里） |
| `app/java/.../MainActivity.java` | 主界面：遥测、摇杆、配速、地图工具栏、图源选择、GPS 面板、权限 |
| `app/java/.../MockService.java` | 前台服务：5Hz 写入模拟位置、托管悬浮窗、后台跟随真实定位 |
| `app/java/.../MotionEngine.java` | 运动推算核心（摇杆向量 + 配速 → 经纬度），纯 Java 可单测 |
| `app/java/.../Geo.java` | 距离/方位/目标点/墨卡托投影 + GCJ-02 ↔ WGS-84 纠偏 |
| `app/java/.../MapView.java` | 地图绘制：瓦片、祖先瓦片顶替、轨迹缓存、位置图标、真实位置蓝点、离线网格 |
| `app/java/.../TileLoader.java` | 图源定义与并行探测、占位瓦片识别、失败冷却重试、内存缓存 |
| `app/java/.../RealGps.java` | 手机真实定位读取（单例，GPS + 网络，无监听自动停止） |
| `app/java/.../OverlayJoystick.java` | 悬浮窗摇杆（拖动、吸附边缘、速度显示） |
| `app/java/.../GuideActivity.java` | 首启说明图页面 |
| `app/res/` | 布局、配色、图标、说明图、位置图标 |
| `build.ps1` | 一键构建脚本（aapt2 → javac → d8 → zipalign → apksigner） |
| `tests/EngineTest.java` | 26 项核心算法单元测试 |
| `tools/` | 说明图 / 位置图标生成脚本 |
| `dist/` | 编译好的 APK |

## 实现要点

1. **模拟位置注入**：`LocationManager.addTestProvider()` + `setTestProviderLocation()`，同时写入 `gps` 与 `network` 两个 provider，5Hz 刷新；无需 ROOT，只要在开发者选项里选为模拟位置应用。
2. **前台服务**：`foregroundServiceType="location"`，通知栏显示实时坐标与速度并带"停止"按钮，持有 `PARTIAL_WAKE_LOCK` 保证长时间运行。
3. **摇杆 → 位置**：速度 = 配速 × min(1, 偏离圆心比例)，方向 = atan2(dx, -dy)；位置用球面公式按真实时间推进，掉帧不会造成速度失真。
4. **地图容错与提速**：多图源并行探测选最快；瓦片没到先用祖先层级放大顶替（先粗后细），冷启动 3 秒即可铺满；轨迹缓存在瓦片像素坐标系，平移只做矩阵变换。
5. **坐标系处理**：高德等 GCJ-02 图源自动做双向纠偏（实测北京偏移 554.9 米），保证地图与设定坐标一致。
6. **无网络可用**：瓦片全部失败时改画经纬网格 + 距离圈，位置依然可见。

## 已知限制

- 无 ROOT 方案的固有限制：部分 App（银行、部分游戏反作弊）会检测并忽略模拟位置。
- 地图瓦片来自公开服务，个别地区/时段可能限速；加载不出来会自动切网格底图，可点 ↻ 重试或换图源。
- 悬浮窗需要「显示在其他应用上层」权限；部分国产系统还要求"后台弹出界面"权限。
- 室内 GPS 精度下降（±米数变大）属于系统定位本身的特性。

## 免责声明

本软件面向**开发调试、应用兼容性测试、位置相关功能测试**等合法用途。
请遵守所在地法律法规以及相关服务条款，**不得用于考勤作弊、欺诈、伪造行程等违法违规用途**，使用本软件产生的一切后果由使用者自行承担。

## 开源协议

[MIT](LICENSE)

## 地图数据来源

- © OpenStreetMap contributors（ODbL）
- © Esri World Street Map
- © 高德地图（仅供个人学习测试使用，请勿用于商业用途）
