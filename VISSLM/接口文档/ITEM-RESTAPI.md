# 数据对象管理RestAPI接口说明 0.3.2


此接口为VISSLM提供, 接口实现形式为 HTTP Restful

\*返回值格式默认是"application/json", 本文中使用伪json示意。

URI格式说明

	http://location:port/path?param=value#flag

## 通用参数

所有接口的通用参数

| 参数 | 说明 |
| --- | :-- |
| user | 用户登录名 |
| apiToken | 用户的API令牌 |
| xutoken | 长效用户令牌 |
| utoken | 短期用户令牌 |

## 数据查询
---------


URI

		http://<server>/alm/rest/items
**方法:GET**

|参数 | 说明 |
| --- | :-- |
| ReturnProperty | 返回属性，多个属性使用逗号分割 |
| q.{字段} | 条件字段，参数值为条件值 |
| VSearch | 支持VSearch 查询语句, select [字段] from [类型] where [条件] order by [字段] limit [偏移值，记录数] |

例子

		http://192.168.0.3/alm/rest/items/?q._valm_Name=对象名&user=sa&ApiToken=12aef578c34a47849c789c6127d60827
		or
		http://192.168.0.3/alm/rest/items/?VSearch=select _valm_Uid from Task where _valm_Name=对象名&user=sa&ApiToken=12aef578c34a47849c789c6127d60827

		返回
		{
			"ErrorCode":0,"ErrorMessage":null,"propList":[{"_valm_Uid":"17401"}]
		}
***


## 数据新建
---------


URI

		http://<server>/alm/rest/items
**方法:POST**

|参数 | 说明 |
| --- | :-- |
| nodeType | 数据节点类型，必填 |
| projectId | 项目Id，必填 |
| componentId | 组件Id，选填，如果未指定parentId，而又想创建到某个Component下，则需要填写 |
| parentId | 父节点Id，如不填，则平台自动寻找匹配集合作为父节点 |
| insertAfterId | 插入到指定节点之后，0为第一个节点，-1为最后一个，默认-1 |
| insertBeforeId | 插入到指定节点之前，如果指定了这个参数，则insertAfterId无效 |
| 消息体 | 属性字典 |



例子

		http://192.168.0.3/alm/rest/items/?nodeType='Requirement'&projectId=1000&user=sa&ApiToken=12aef578c34a47849c789c6127d60827

		消息体
		{
			"_valm_Name":"需求1"
		}

		返回
		{
			"ErrorCode":0,"ErrorMessage":null,"propList":[{"_valm_Uid":"17401"}]
		}
***



## 获取指定数据属性
---------

获取指定数据对象各字段

URI

		http://<server>/alm/rest/items/id/{id}
**方法:GET**

|参数 | 说明 |
| --- | :-- |
| {id} | 对象的 Uid |
| ReturnProperty | 返回属性，多个属性使用逗号分割 |


例子

		http://192.168.0.3/alm/rest/items/id/17401?&user=sa&ApiToken=12aef578c34a47849c789c6127d60827
		返回
		{
			"ErrorCode":0,"ErrorMessage":null,"prop":{"_valm_Uid":"17401"}
		}
***


## 修改指定数据属性
---------

修改指定数据对象各字段

URI

		http://<server>/alm/rest/items/id/{id}
**方法:POS **

|参数 | 说明 |
| --- | :-- |
| {id} | 对象的 Uid |

**消息体**

 属性字典，发送的请求包体,application/json,utf-8


例子

		http://192.168.0.3/alm/rest/items/id/17401?&user=sa&ApiToken=12aef578c34a47849c789c6127d60827
		body
		{
			"_valm_Name":"NewName"
		}

		返回
		{
			"ErrorCode":0,"ErrorMessage":""
		}
***	


## 删除数据
---------

删除数据

URI

		http://<server>/alm/rest/items/id/{id}
**方法:DELETE**

|参数 | 说明 |
| --- | :-- |
| {id} | 对象的 Uid |


例子

		http://192.168.0.3/alm/rest/items/id/17401?&user=sa&ApiToken=12aef578c34a47849c789c6127d60827
		body
		{
			"_valm_Name":"NewName"
		}

		返回
		{
			"ErrorCode":0,"ErrorMessage":""
		}
***	

## 获取对象附件清单
---------

获取指定数据对象的附件

URI

		http://<server>/alm/rest/items/id/{id}/attachment
**方法:GET**

|参数 | 说明 |
| --- | :-- |
| {id} | 对象的 Uid |



例子

		http://192.168.0.3/alm/rest/items/id/17401/attachment?&user=sa&ApiToken=12aef578c34a47849c789c6127d60827

		返回
		{
			"ErrorCode":0,
			"ErrorMessage":""
			"propList": [
				{
					"Uid":"8679",
					"Name":NewUploadFile1.txt",
					"resource":"http://192.168.0.3/alm/rest/items/attachment/8679",
					...,
				}
			]
		}
***	

## 上传附件
---------

上传指定数据对象的附件

URI

		http://<server>/alm/rest/items/id/{id}/attachment
**方法:POST**

|参数 | 说明 |
| --- | :-- |
| {id} | 对象的 Uid |
| fileName | 附件文件名 |
| Name | 附件显示名 |
| Description | 附件描述 |
| overwrite | 如果为True，则覆盖同名文件  |

**消息体**

附件文件流


例子

		http://192.168.0.3/alm/rest/items/id/17401/attachment?&user=sa&ApiToken=12aef578c34a47849c789c6127d60827&fileName=NewFile1.txt

		返回
		{
			"ErrorCode":0,
			"ErrorMessage":""
			"props": 
				{
					"Uid":"8679",
					"Name":NewUploadFile1.txt",
					"resource":"http://192.168.0.3/alm/rest/items/attachment/8679",
					...,
				}
			
		}
***	


## 删除附件
---------

删除附件

URI

		http://<server>/alm/rest/items/id/{id}/attachment
**方法:DELETE**

|参数 | 说明 |
| --- | :-- |
| {id} | 对象的 Uid |



例子

		http://192.168.0.3/alm/rest/items/attachment/8679?&user=sa&ApiToken=12aef578c34a47849c789c6127d60827

		返回
		{
			"ErrorCode":0,
			"ErrorMessage":""
		}
***	
