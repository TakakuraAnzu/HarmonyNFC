# HarmonyNFC配网问题点整理
#### 1.FeatureAbility.startAbility启动半屏ability或全屏ability时无响应
>启动半屏ability需带参数
>`RequestParams.flag=276826112`
>
>启动全屏ability时需带参数
>`RequestParams.flag=286435456`

#### 2.FeatureAbility.startAbilityForResult无返回
>使用FeatureAbility.startAbilityForResult启动另一ability时，因加载时序问题，需要在onShow方法之后使用FeatureAbility.finishWithResult才能接收返回数据

#### 3.FeatureAbility.startAbilityForResult无法启动ability
>使用FeatureAbility.startAbilityForResult启动ability时，不能设置问题1中提到的flag参数

#### 4.FeatureAbility.startAbilityForResult不能返回自定义数据
>EMUI11与HarmonyOS中FeatureAbility.finishWithResult中使用的参数不一致
>
>EMUI11写法
```
let result = {
  code: 200,
  result: {}
}
FeatureAbility.finishWithResult(result)
```
>HarmonyOS写法
```
let result = {}
FeatureAbility.finishWithResult(200, result)
```

#### 5.搜索不到so库
>第一次从线上下载hap时,hap文件写入是异步操作，需要在使用so库的ability中延迟加载so库，并且so库需配置在入口module中，其他module不能配置

#### 6.多个FA不能使用同一PA
>FA与PA需要一一对应

#### 7.同一项目不同module下使用相同packagename且classname相同
>可以使用相同packagename，但相同packagename下不能使用相同classname，否则可能出现闪退等不可预见的错误

#### 8.华为账号登录不能正确加载
>同一项目下有且只有一个module可以配置
>`implementation(group: 'com.huawei.hms', name: 'jsb-ohos-adapter', version: '5.3.0.303', ext: 'har')`

#### 9.ability无法获取wifi列表
>需要在entry module加入权限
```
"reqPermissions": [
      {
        "name": "com.huawei.hilink.framework.permission.BIND_AI_LIFE_CORE_SERVICE"
      }
]
```

#### 10.权限不生效
>需要分别在entry module和使用该权限的module中配置权限声明

#### 11.找不到入口FA
>需要在项目每个module中配置mainability字段
```
"module": {
  "mainAbility": "com.xxx.xxx.MainAbility"
}
```

#### 12.EMUI11第一次使用FeatureAbility.callAbility调用Ability实现的PA时返回null
>1.结果加入null判断，若null再次调用FeatureAbility.callAbility则可以获取正确结果
>2.使用Internal Ability实现的PA

