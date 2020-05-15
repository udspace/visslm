# 项目添加RestAPI接口说明


此接口为VISSLM提供, 接口实现形式为 HTTP Restful

\*返回值格式默认是"application/json", 本文中使用伪json示意。

URI格式说明

	http://location:port/path?param=value#flag


##添加项目信息
---------


URI

		http://<server>/alm/rest/Project/AddProject
**方法:POST**

|参数 | 说明 |
| --- | :-- |
| ParentId | （默认1）父节点ID(指定添加到父节点的子数据) |
| RoleData |  角色(角色名称)和用户数据对象数据 |
| Property |  *项目字段数据(对应project类型的字段信息,注意字段的合法性) |
| InsertAfterId |  添加数据的位置(默认-1表示最后) |
| Project_Template |  项目模板(模板项目ID) |
| UToken |  * (是参数不是post值)用户Token |

Content

	{
		"ParentId":"1",
		"RoleData":
		[
			{
			"RoleName":"项目成员",
			"UserList":"user,PM"
			}
		],
		" Property":
		{
			"_valm_Name":"测试项目添加",
			"Key":"dltysof.ds1"
		},
		"InsertAfterId":"-1",
		"Project_Template":"0"

	}

返回值

	{
		"ErrorCode": 0,
		"ErrorMsg": "添加成功",
		"Data": [
			{
				"_valm_LastModifyTime": "2020/01/14 19:51:00",
				"_valm_Name": "测试项目添加",
				"_valm_ParentId": "1",
				"_valm_Uid": "2748744"
			}
		]
    }
	
返回Code

|参数 | 说明 |
| --- | :-- |
| 0 | 表示添加成功 |
| -10 |  错误(查看消息) |
| 30000 |  字段不能为空 |
| 30001 |  字段数据类型错误 |

***


				
				
