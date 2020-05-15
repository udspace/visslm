# WebServices接口说明文档 V1


此接口为VISSLM提供, 接口实现形式为 WebService，使用SOAP标准消息协议通讯，用WSDL文件进行说明，并通过UDDI进行注册

 

WebService地址

    http://location:port/almws/VALMService.svc

## 通用参数

所有接口的通用参数


 LoginUser：用户信息

| 参数 | 说明 |
| --- | :--- |
| AppName | 访问服务的应用名称 |
| UToken | 用户登陆后令牌 |
| UserName | 用户登录名 |
| UserPassword | 用户密码 |

> UToken与(UserName、UserPassword)二选一，同时存在时使用用户名与密码验证



## 新增对象数据【AddNode】

#### 参数说明


| 参数 | 类型 | 说明 |
| --- | :--- | :--- |
| propertyList | 字典 | 对象属性字典 |
| parentId | 字符串 | 父节点对象编号,0:默认为项目编号 |
| insertAfterId | 字符串 | 相对对象编号，与insertType一起用于确定新增对象在父对象的排序位置，-1（当前父对象最后，默认值），0（当前父对象最前面） |
| insertType | 字符串 | 与相对对象前后关系，top：之前，bottom（默认值）：之后 |
| nodeType | 字符串 | 对象类型 |
| projectID | 字符串 | 项目编号 |
| component | 字符串 | 组件编号，默认为0 |
| userInfo | LoginUser | 用户信息 |

#### 返回说明

    返回字典类型，失败时返回ReturnMsg：failed，ErrorMsg：错误说明，成功时返回Uid:新增对象编号
## 查询对象数据【SearchNode】
#### 参数说明

| 参数 | 类型 | 说明 |
| --- | :--- | :--- |
| searchConditionList | SearchCondition列表 | 查询条件 |
| returnProperties | 字符串列表 | 返回对象属性 |
| userInfo | LoginUser | 用户信息 | 


### 【SearchCondition】


#### 查询条件

 | 参数 | 说明 | 样例 |
 | --- | :--- | :--- |
 | property | 对象属性 | ProjectStatus,ProjectName,_valm_Uid |
 | condition | 比较运算符 | >、<、=、like,in |
 | PropertyVal | 属性值 | 1 |
 | conector | 连接符 | And或者Or  |

#### 排序条件

 | 参数 | 说明 | 样例 |
 | --- | :-- | :-- |
 | property | [OrderBy] | |
 | condition | 排序条件 | asc或者desc |
 | PropertyVal | 属性值 | _valm_Sort |
 | conector | 空白 | 空白 |


#### 分页大小条件

 | 参数 | 说明 | 样例 |
 | --- | :-- | :-- |
 | property | [PageSize] | [PageSize] |
 | condition | 空白 | 空白 |
 | PropertyVal | 每页条数 | 例如：100 |
 | conector | 空白 | 空白 |


#### 页数条件

 | 参数 | 说明 | 样例 | 
 | --- | :-- | :-- |
 | property | [PageIndex]| |
 | condition | 空白| |
 | PropertyVal | 页数 | 默认1 |
 | conector |空白 |空白|

#### 获取特定版本数据

 | 参数 | 说明 | 样例 |
 | --- | :-- | :-- |
 | property | [VersionNum]| [VersionNum]|
 | condition | 空白| |
 | PropertyVal | 对应版本号 |   |
 | conector |空白 |空白|

#### 获取受影响对象数据

 | 参数 | 说明 | 样例 |
 | --- | :-- | :-- |
 | property | [IsImpacted]| [IsImpacted]|
 | condition | 空白| |
 | PropertyVal | True | True  |
 | conector |空白 |空白|

### 【returnProperties】特殊属性说明

 * [VersionNum]: 返回版本号
 * [Attachment]: 返回对象附件，返回附件信息为JSON字符串，Ex: [{“Name”:“bbs.txt”,”Description”:“dd”,”DownloadLink”:”../测试项目2\\Task\\20140627182220_bbs.txt”}]
 * [ExInfos@key]: 返回隐藏字段信息，key为存值时设置的键，返回key对应的值，Ex: [ExInfos@almSpe]
### 返回说明
    返回字典列表，成功时根据returnProperties设置的字段属性返回XML格式数据，失败时返回ReturnMsg：failed，ErrorMsg：错误说明

## 更新对象数据【ModifyNode】

#### 参数说明
| 参数 | 类型 |说明|
| --- | :-- |:-- |
| uid | 字符串 |对象编号 |
| propertyList | 字典 |更新属性字典 |
| userInfo | LoginUser |用户信息 | 
### 返回说明
    返回ReturnMsg：failed/success，ErrorMsg：错误说明

## 删除对象数据【DelNode】

#### 参数说明
| 参数 | 类型 |说明|
| --- | :-- |:-- |
| nodeId | 字符串 |对象编号 |
| delSelf | bool |是否删除本身 |
| userInfo | LoginUser |用户信息 | 

### 返回说明
    返回ReturnMsg：failed/success，ErrorMsg：错误说明

## 移动对象数据【MoveNode】

#### 参数说明
| 参数 | 类型 |说明|
| --- | :-- |:-- |
| currentNodeId | 字符串 |对象编号 |
| parentNodeId | 字符串 |移动后父对象编号 |
| moveAfterId | 字符串 |相对对象编号，与insertType一起用于确定新增对象在父对象的排序位置，-1（当前父对象最后，默认值），0（当前父对象最前面）  |
| moveType | 字符串 |与相对对象前后关系，top：之前，bottom（默认值）：之后  |
| userInfo | LoginUser |用户信息 | 

### 返回说明
    返回ReturnMsg：failed/success，ErrorMsg：错误说明

## 获取用户信息【GetUserByUserName】

#### 参数说明
| 参数 | 类型 |说明|
| --- | :-- |:-- |
| userName | 字符串 |用户登录名 | 
### 返回说明
	返回字典列表，成功：以XML格式返回用户的详细信息，失败：返回400状态码

## 获取用户登录态【UserVerify】

#### 参数说明
| 参数 | 类型 |说明|
| --- | :-- |:-- |
| userInfo | LoginUser |用户信息 | 
### 返回说明
    返回ReturnMsg：failed/success，ErrorMsg：错误说明，成功时UToken：用户登录态


## 获取服务器时间【GetNowTime】

#### 参数说明
| 参数 | 类型 |说明|
| --- | :-- |:-- |
| userInfo | LoginUser |用户信息 | 
### 返回说明
    返回ReturnMsg：failed/success，ErrorMsg：错误说明，成功时ServiceTime：服务器时间

## 查询对象类型定义【SearchMemberItem】

#### 参数说明
| 参数 | 类型 |说明|
| --- | :-- |:-- |
| searchConditionList | SearchCondition列表 |查询条件 |
| returnProperties | 字符串列表 |返回属性 |
| userInfo | LoginUser |用户信息 | 

##### property支持属性

| 参数 | 说明 |支持条件|
| --- | :-- |:-- |
| MemberName | 属性说明 |= |
| NodeType | 对象类型 | = |
| HideMember | 属性名称 |= |

##### returnProperties支持属性

| 参数 | 说明 |
| --- | :-- |
| Uid | 编号 |
| MemberName | 属性描述 | 
| HideMember | 属性名称 |
| AttrType | 属性类型 |
| NodeType | 对象类型 |
 

### 返回说明
    返回ReturnMsg：failed/success，ErrorMsg：错误说明，成功时ServiceTime：服务器时间

##查询数据类型定义【SearchNodeType】

#### 参数说明

| 参数 | 类型 | 说明 |
| --- | :--- | :--- |
| searchConditionList | SearchCondition列表 | 查询条件 |
| returnProperties | 字符串列表 | 返回数据类型属性 |
| userInfo | LoginUser | 用户信息 | 

##### property支持属性

| 参数 | 说明 |支持条件|
| --- | :-- |:-- |
| Uid | 编号 |= |
| NodeConfigType | 类型名称 | = |
| ShowNodeTypeName | 类型显示名 |= |

##### returnProperties支持属性

| 参数 | 说明 |
| --- | :-- |
| Uid | 编号 |
| NodeConfigType | 类型名称 | 
| ShowNodeTypeName | 类型显示名 |

### 返回说明
    返回字典列表，成功时根据returnProperties设置的字段属性返回XML格式数据，失败时返回ReturnMsg：failed，ErrorMsg：错误说明
