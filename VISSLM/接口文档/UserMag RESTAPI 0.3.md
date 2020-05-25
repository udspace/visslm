#用户管理接口说明 V0.3


此接口为VISSLM提供, 接口实现形式为 HTTP Restful

\*返回值格式默认是"application/json", 本文中使用伪json示意。

URI格式说明

	http://location:port/path?param=value#flag

##获取用户信息
----------
在VISSLM中的获取用户

URI

		http://<server>/alm/rest/user/management/{userName}/

**方法:GET**

参数：说明
{userName}：用户名称
AppName：应用名称
AppToken：应用许可  

返回值

		{
			ErrorCode:"0", 
			ErrorMsg:"描述...",
			Data:{
				Name:"登录名称",//（必须项）
				NickName:"显示名称",//（必须项）
				Sex:"性别", // （必须项）
				......
			}
		}  ErrorCode返回0表示添加成功，返回1表示返回失败

***


##添加用户信息
----------
在VISSLM中的添加用户，可添加多个

URI

		http://<server>/alm/rest/user/management

**方法:POST**

参数：说明
AppName：应用名称
AppToken：应用许可  

Content

		[
			{
				Name:"登录名称",//（必须项）
				NickName:"显示名称",//（必须项）
				Sex:"性别", // （必须项）
				UserType:"", // （必须项）
				Mailbox:"邮箱"，//（必须项）
				PermitType:"许可类型", // （必须项）
				Address:"地址",
				Phone:"电话",
				IDType:"", // 身份证:军官证:护照
				IDNumber:"", 
				AutoGraph:"",
				AttributeExtension:{"属性1":"值1","属性2":"值2","属性3":"值3"....}
			},
			{}
		]

返回值

		{
			"ErrorCode":0,
			"ErrorMsg":"描述..."，
			"Data":{
				"SuccessAdd":[]，
				"FailAdd":[]
			}
		}  ErrorCode返回0表示添加成功，返回1表示返回失败
		 
***

##更新用户信息
----------

在VISSLM中的更新用户

URI

		http://<server>/alm/rest//user/management/{userName}/

**方法:PUT**

参数：说明
{userName}：用户名称
AppName：应用名称
AppToken：应用许可  

Content

		{
			NickName:"显示名称",
			Sex:"性别", 
			UserType:"", 
			Mailbox:"邮箱"，
			PermitType:"许可类型", 
			Address:"地址",
			Phone:"电话",
			IDType:"", 
			IDNumber:"", 
			AutoGraph:"",
			AttributeExtension:"{'属性1':'值1','属性2':'值2','属性3':'值3'....}"
		}

返回值

		{
			"ErrorCode":0,
			"ErrorMsg":"描述..."
		}  ErrorCode返回0表示添加成功，返回1表示返回失败


***

##删除用户信息
----------

在VISSLM中的删除用户

URI

		http://<server>/alm/rest/user/management/{userName}/

**方法:DELETE**

{userName}：用户名称
AppName：应用名称
AppToken：应用许可 


返回值

		{
			"ErrorCode":0,
			"ErrorMsg":"描述..."
		}  ErrorCode返回0表示添加成功，返回1表示返回失败


***

##恢复用户信息
----------

在VISSLM中的恢复用户

URI

		http://<server>/alm/rest/user/management/{userName}/restore

**方法:POST**

{userName}：用户名称
AppName：应用名称
AppToken：应用许可 


返回值

		{
			"ErrorCode":0,
			"ErrorMsg":"描述..."
		}  ErrorCode返回0表示添加成功，返回1表示返回失败


***

##查询所有用户信息
----------

查询VISSLM中所有的用户

URI

		http://<server>/alm/rest/user/management/AllUsers?pageSize={pageSize}&pageIndex={pageIndex}&searchWords={searchWords}

**方法:GET**

AppName：应用名称
AppToken：应用许可 


| pageSize (可选)| 每页显示的数量|
| pageIndex (可选)|当前页码 |
| searchWords (可选) |模糊查询字符|

返回值

			{
			"ErrorCode":0,
			"ErrorMsg":"成功",
			"Data":[
						{
							"Uid": 5,
							"Name": "user",
							"NickName": "演示用户",
							"Phone": "12345678910",
							"Address": "",
							"Mailbox": "user@email.com",
							"Sex": "1 ",
							"LastModifyDate": "2020-04-17T11:00:32.75",
							"CreateBy": "",
							"CreateDate": "2016-03-31T15:23:20.647",
							"Sort": 0,
							"IDType": "",
							"IDNumber": "",
							"Autograph": "/DataFiles/Personal/UserAutograph/UserCustom/7a00fafed1d340da80d3eba9513bf3a5.png",
							"AttributeExtension": null,
							"SecretType": null,
							"PermitType": 0,
							"PermitExtension1": null,
							"PermitExtension2": null,
							"PermitExtension3": null,
							"DomainAccount": null
						},
						{}
					]
		}

