# YKSwiftNetworking

[![CI Status](https://img.shields.io/travis/com/{username}/YKSwiftNetworking.svg?style=flat)](https://travis-ci.com/541278903/YKSwiftNetworking)
[![Version](https://img.shields.io/cocoapods/v/YKSwiftNetworking.svg?style=flat)](https://cocoapods.org/pods/YKSwiftNetworking)
[![License](https://img.shields.io/cocoapods/l/YKSwiftNetworking.svg?style=flat)](https://cocoapods.org/pods/YKSwiftNetworking)
[![Platform](https://img.shields.io/cocoapods/p/YKSwiftNetworking.svg?style=flat)](https://cocoapods.org/pods/YKSwiftNetworking)

基于 Alamofire 的 Swift 网络请求封装库，支持链式调用和 RxSwift 集成。

## 核心功能

### 基础网络请求
- 支持 GET/POST 请求
- 链式编程风格
- 可配置默认请求头和参数
- 支持 URL 前缀配置
- 统一的响应处理机制

### 请求配置
1. 请求参数设置
   - 动态参数添加
   - 自定义请求头
   - 请求体（Body）配置
   - 请求编码方式设置

2. 文件传输
   - 文件上传（支持图片等多媒体文件）
   - 文件下载（支持自定义下载路径）

3. 进度监控
   - 上传/下载进度回调
   - 实时进度展示

### 高级特性
- RxSwift 支持
- 请求模拟数据支持（Mock Data）
- 灵活的请求协议配置

## 技术特点
- 基于 Alamofire 封装
- 支持 CocoaPods 集成
- 链式调用 API 设计
- 完善的类型支持

## 主要 API

### 初始化方式
1. 基础初始化
```swift
YKSwiftNetworking.init()
```

2. 高级初始化
```swift
YKSwiftNetworking.init(
    defaultHeader: [String: String],
    defaultParams: [String: Any],
    handleResponse: (YKSwiftNetworkResponse, YKSwiftNetworkRequest) -> Error?
)
```

### 请求配置 API
- `.method(.post)` - 设置请求方法
  ```swift
  networking.method(.post)
  ```

- `.url("https://api.example.com/users")` - 设置请求 URL
  ```swift
  networking.url("https://api.example.com/users")
  ```

- `.params(["page": 1, "limit": 20])` - 设置请求参数
  ```swift
  networking.params(["page": 1, "limit": 20])
  ```

- `.header(["Authorization": "Bearer token"])` - 设置请求头
  ```swift
  networking.header(["Authorization": "Bearer token"])
  ```

- `.progress { progress in print("进度: \(progress)") }` - 设置进度回调
  ```swift
  networking.progress { progress in
      print("进度: \(progress)")
  }
  ```

- `.encoding(.json)` - 设置请求编码
  ```swift
  networking.encoding(.json)
  ```

- `.mockData(MockResponse())` - 设置模拟数据
  ```swift
  networking.mockData(MockResponse())
  ```

- `.httpBody(jsonData)` - 设置请求体
  ```swift
  networking.httpBody(jsonData)
  ```

- `.uploadData(imageData, name: "avatar")` - 配置上传请求
  ```swift
  networking.uploadData(imageData, name: "avatar")
  ```

- `.downloadDestPath("/Downloads/file.pdf")` - 配置下载路径
  ```swift
  networking.downloadDestPath("/Downloads/file.pdf")
  ```

### 上传请求示例
```swift
// 单图片上传
let imageData = UIImage(named: "avatar")?.jpegData(compressionQuality: 0.8)
YKSwiftNetworking()
    .url("https://api.example.com/upload/avatar")
    .method(.post)
    .uploadData(imageData, name: "avatar", mimeType: "image/jpeg")
    .progress { progress in
        print("上传进度：\(progress * 100)%")
    }
    .request { response in
        switch response {
        case .success(let data):
            print("上传成功：\(data)")
        case .failure(let error):
            print("上传失败：\(error)")
        }
    }

// 多图片上传
let images = [
    ("photo1", UIImage(named: "photo1")?.jpegData(compressionQuality: 0.8)),
    ("photo2", UIImage(named: "photo2")?.jpegData(compressionQuality: 0.8))
]

YKSwiftNetworking()
    .url("https://api.example.com/upload/photos")
    .method(.post)
    .params(["albumId": "123"]) // 附加参数
    .header(["Authorization": "Bearer token123"])
    .uploadMultiData(images.compactMap { name, data in
        data.map { (name, $0, "image/jpeg") }
    })
    .progress { progress in
        print("批量上传进度：\(progress * 100)%")
    }
    .request { response in
        switch response {
        case .success(let data):
            print("批量上传成功：\(data)")
        case .failure(let error):
            print("批量上传失败：\(error)")
        }
    }

// 文件上传（如 PDF）
let fileURL = Bundle.main.url(forResource: "document", withExtension: "pdf")!
let fileData = try! Data(contentsOf: fileURL)

YKSwiftNetworking()
    .url("https://api.example.com/upload/document")
    .method(.post)
    .uploadData(fileData, name: "document", fileName: "document.pdf", mimeType: "application/pdf")
    .header([
        "Content-Type": "multipart/form-data",
        "Authorization": "Bearer token123"
    ])
    .progress { progress in
        print("文件上传进度：\(progress * 100)%")
    }
    .request { response in
        switch response {
        case .success(let data):
            print("文件上传成功：\(data)")
        case .failure(let error):
            print("文件上传失败：\(error)")
        }
    }
```



### RxSwift 支持
- 提供 RxSwift 扩展
- 支持响应式请求处理





