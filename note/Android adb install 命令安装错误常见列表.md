### Android adb install 命令安装错误常见列表

------

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

| 错误信息                                    | 说明                     |
| --------------------------------------- | ---------------------- |
| INSTALL_FAILED_ALREADY_EXISTS           | 要安装的APP已经存在（相同包名，相同版本） |
| INSTALL_FAILED_INVALID_APK              | 无效的APK（ 安装包损坏）         |
| INSTALL_FAILED_INVALID_URI              | 无效的链接（链接中含有非英文字符）      |
| INSTALL_FAILED_INSUFFICIENT_STORAGE     | 没有足够的存储空间              |
| INSTALL_FAILED_DUPLICATE_PACKAGE        | 已存在同包名程序（签名不一致）        |
| INSTALL_FAILED_NO_SHARED_USER           | 要求的共享用户不存在             |
| INSTALL_FAILED_UPDATE_INCOMPATIBLE      | 版本不能更新                 |
| INSTALL_FAILED_SHARED_USER_INCOMPATIBLE | 需求的共享用户签名错误            |
| INSTALL_FAILED_MISSING_SHARED_LIBRARY   | 需求的共享库已丢失              |
| INSTALL_FAILED_REPLACE_COULDNT_DELETE   | 需求的共享库无效               |
| INSTALL_FAILED_DEXOPT                   | dex优化验证失败              |
| INSTALL_FAILED_OLDER_SDK                | 系统版本过旧（minSdkVersion）  |
| INSTALL_FAILED_CONFLICTING_PROVIDER     | 存在同包名的内容提供者            |
| INSTALL_FAILED_NEWER_SDK                | 系统版本过新（maxSdkVersion）  |
| INSTALL_FAILED_TEST_ONLY                | 调用者不被允许测试的测试程序         |
| INSTALL_FAILED_CPU_ABI_INCOMPATIBLE     | 包含的本机代码不兼容             |
| CPU_ABIINSTALL_FAILED_MISSING_FEATURE   | 使用了一个无效的特性             |
| INSTALL_FAILED_CONTAINER_ERROR          | SD卡访问失败                |
| INSTALL_FAILED_INVALID_INSTALL_LOCATION | 无效的安装路径                |
| INSTALL_FAILED_MEDIA_UNAVAILABLE        | SD卡不存在                 |
| INSTALL_FAILED_INTERNAL_ERROR           | 系统问题导致安装失败             |
| DEFAULT                                 | 未知错误                   |

