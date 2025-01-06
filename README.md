# ad_sdk_doc
AD SDK Document


---

## 1. **SDK 1.0.0 接入文档**

### 1.1 **介绍**

> 这是一个用于广告展示的 SDK，支持中间弹窗广告、右下角悬浮广告、视频启动广告，自定义容器展示广告等。通过 SDK，开发者可以轻松在应用中集成广告展示功能。

---

### 1.2 **开发环境要求**

为确保广告SDK在您的项目中能够顺利集成和运行，以下是SDK所需的开发环境和配置要求。

---

## 1. **开发环境要求**

### 1.1 **Android Studio**

推荐使用 **Android Studio** 作为开发环境。确保使用支持当前SDK版本的 Android Studio。

- **最低版本**: Android Studio 4.0及以上  
- **推荐版本**: Android Studio Koala | 2024.1.1 或更高版本

---

### 1.2 **JDK (Java Development Kit)**

广告SDK要求使用 **JDK 8 或更高版本**。建议使用与 Android Studio 兼容的版本。

- **最低版本**: JDK 8  
- **推荐版本**: JDK 17（但确保与 Android Studio 的版本兼容）

---

### 1.3 **Android SDK**

确保您已经安装了所需版本的 **Android SDK**。

- **Android SDK版本**: 需要支持的 Android 版本应至少为 **Android 7.0 (API 24)** 或更高。

---

### 1.4 **Gradle**

确保项目使用适当版本的 **Gradle** 构建工具，并且和 SDK 的版本兼容。

- **Gradle版本**: 推荐使用 **8.7** 或更高版本。  
- **Gradle插件版本**: 需要使用 **Android Gradle Plugin 8.5.0 或更高版本**。

---

### 1.5 **Kotlin（Java项目直接忽略这个）**

确保项目使用适当版本的 **Kotlin** 构建工具，并且与 SDK 的版本兼容。

- **Kotlin版本要求**：需要确保您的项目使用的 Kotlin 版本至少为 **Kotlin 1.3.50** 或更高。

---

### 权限说明（无需再添加）

1. **授予运行时权限** (`android.permission.GRANT_RUNTIME_PERMISSIONS`)：允许应用在运行时请求权限。
2. **悬浮窗权限** (`android.permission.SYSTEM_ALERT_WINDOW`)：允许应用创建系统级弹窗或悬浮窗。
3. **联网权限** (`android.permission.INTERNET`)：允许应用访问互联网进行数据通信。
4. **网络状态权限** (`android.permission.ACCESS_NETWORK_STATE`)：允许应用检查设备的网络连接状态。
5. **查询所有应用权限** (`android.permission.QUERY_ALL_PACKAGES`)：允许应用查询设备上所有已安装的应用。需要在 Android 11 及以上版本中提供合理的用途说明。
6. **开机广播权限** (`android.permission.RECEIVE_BOOT_COMPLETED`)：允许应用在设备启动完成后执行任务，通常用于在系统启动时启动服务。

---

### 1.3 **集成步骤**

#### 1.3.1 **添加 JitPack 仓库**

开发者需要在 `build.gradle` 文件中添加 JitPack 仓库。

```gradle
repositories {
    mavenCentral()
    maven { url "https://jitpack.io" }
}
```

---

#### 1.3.2 **添加依赖**

在 `build.gradle` 中的 `dependencies` 部分，添加你的 SDK 依赖：

```gradle
dependencies {
    implementation 'com.github.codeZeng95:ad-sdk:1.0.0-SNAPSHOP'  // JitPack 地址
}
```

---

#### 1.3.3 **初始化 SDK**

开发者只需要在 `Application` 类中进行一次 SDK 的初始化。例如：

```kotlin
class MyApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        AdSdk.init(this)
    }
}
```

---

### 1.4 **使用示例**

提供一些典型的使用示例，展示如何在不同的场景中展示广告，通过后台配置图片或者视频广告。

#### 1.4.1 **弹窗广告**

屏幕息屏亮屏会触发中间广告弹窗，其他场景也可自行显示中间弹窗，在需要显示的地方调用：

```kotlin
AdSdk.showCenterPopAd(context:Context)
```

---

#### 1.4.2 **悬浮广告**

SDK内部控制，后台可配置相关字段。(目前会在打开第三方视频应用的时候会触发显示)

---

#### 1.4.3 **绑定外部View展示广告**

通过传入父控件容器和广告ID来绑定广告并展示：

```kotlin
AdSdk.bindAD(rlContainer, AdIds.AD_ID_VIEW) {
    //按需重写相应回调
}
```

---



### 1.4.3 **广告点击**

在使用 `AdSdk.bindAD()` 方法时，确保为传入的父控件容器设置能够获取焦点的属性。可以通过如下方式在布局文件中设置父容器的焦点属性：

```xml
<RelativeLayout
    android:focusableInTouchMode="true"
    android:focusable="true"
    android:id="@+id/rl_container"
    android:layout_width="@dimen/qb_px_350"
    android:layout_height="@dimen/qb_px_200"
    android:layout_centerHorizontal="true"
    android:layout_marginTop="@dimen/qb_px_20"
/>
```

这个配置确保了 `RelativeLayout` 容器能够在遥控模式下获取焦点，进而能够响应广告点击事件。

---




### 1.5 **事件监听**

SDK 中支持的事件，如何监听这些事件。例如：

```kotlin
AdSdk.bindAD(rlContainer, AdIds.AD_ID_VIEW) {
    onAdDataFetchStart {
        //开始拉取广告数据
    }
    onAdDataFetchSuccess {
        //广告数据获取成功
    }
}
```

---

以下是其他相关回调：

```kotlin
interface AdLoadCallback {

    fun onAdDataFetchStart() {
        // 开始拉取广告数据
    }

    fun onAdDataFetchSuccess() {
        // 广告数据获取成功
    }

    fun onAdDataFetchFailed(errorMsg: String?) {
        // 广告数据获取失败
    }

    fun onNoAdData() {
        // 暂无广告数据
    }

    fun onAdDataLoading() {
        // 广告数据获取中
    }

    fun onAdLoading() {
        // 广告加载中
    }

    fun onAdLoadSuccess() {
        // 广告加载成功
    }

    fun onAdLoadFailed(errorMsg: String) {
        // 广告加载失败
    }

    fun onAdClosed() {
        // 广告关闭
    }

    fun onAdClicked(adId:String,adUrl:String?=null) {
        // 广告被点击
    }

    fun onAdCountdownFinished() {
        // 广告倒计时结束
    }
}
```

---

### 1.6 **配置选项**

所有广告配置的选项，例如广告展示的频率、显示时长等。每个选项的默认值和如何修改配置都可以通过后端接口配置。

---

### 1.7 **错误处理**

可以直接在上面的回调进行对应的错误处理。

---

### 1.8 **卸载与清理**

```kotlin
override fun onTerminate() {
    super.onTerminate()
    AdSdk.destroy(this)
}
```

---

### 1.9 **常见问题**

- **问题**：无法弹出悬浮窗。  
  **解决**：悬浮窗权限是自动获取的，无需用户手动打开，前提是确保应用已具备系统权限。

---

### 1.10 **混淆配置说明**

**无需手动配置混淆规则**。SDK 已经为您处理了所有必要的混淆规则，因此在开发过程中无需特别考虑混淆问题。SDK 会自动处理相关类和方法的混淆，确保广告功能的正常运行。只需专注于广告展示和相关配置，无需修改 `proguard-rules.pro` 或其他混淆设置。

---

### 1.11 **联系方式**

---

## 2. **附录**

- **版本历史**：  
  - 版本 1.0.0-SNAPSHOP：首次发布  

- **API 文档**：  
  - API 参考文档请访问 [SDK API 文档链接]

---

## 3. **示例代码**

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        val rlContainer = findViewById<RelativeLayout>(R.id.rl_container)
        
        AdSdk.bindAD(rlContainer, AdIds.AD_ID_VIEW) {
            onAdDataFetchStart {
                //开始拉取广告数据
            }
            onAdDataFetchSuccess {
                //广告数据获取成功
            }
        }
    }
}
```

---
