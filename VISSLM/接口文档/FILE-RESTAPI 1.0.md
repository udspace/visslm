#文件管理RestAPI接口说明


此接口为VISSLM提供, 接口实现形式为 HTTP Restful

\*返回值格式默认是"application/json", 本文中使用伪json示意。

URI格式说明

	http://location:port/path?param=value#flag


##获取文件字节数组Base64字符串
---------


URI

		http://<server>/alm/rest/file/GetFileData/{fileUrl}

**方法:GET**

| 参数 | 类型 | 是否必填 | 说明 |
| --- | :-- | :-- | :-- |
| fileUrl | string | 是 | 文件链接或文件码 |
| appName | string | 是 | 应用名称 |
| appToken | string | 是 | 应用许可 |

返回值

		{"ErrorCode":0,"ErrorMsg":"成功","Data":"字节数组Base64字符串"}
***


##获取文件属性
---------


URI

		http://<server>/alm/rest/file/GetFileInfo/{fileUrl}

**方法:GET**

| 参数 | 类型 | 是否必填 | 说明 |
| --- | :-- | :-- | :-- |
| fileUrl | string | 是 | 文件链接或文件码 |
| appName | string | 是 | 应用名称 |
| appToken | string | 是 | 应用许可 |
| {token} | string | 是 | utoken/apitoken/... |

返回值

		{"ErrorCode":0,"ErrorMsg":"成功","Data":{"FileName":"aa.docx","FileSize":1000,"FileSizeShow":"1000 字节","SecretLevel":-1,"SecretLevelShow":"非密","MD5Code":"AAAAAAAAAAAAAAAAAAAAAAA","DownloadCount":0,"PreviewCount":0,"CreateTime":"2019-10-09","CreateBy":"user","CreateByShow":"演示用户","LastModifyTime":"2019-10-09","LastModifyBy":"user","LastModifyByShow":"演示用户"}}
***


##下载文件
---------


URI

		http://<server>/alm/rest/file/DownloadFile/{fileUrl}

**方法:GET**

| 参数 | 类型 | 是否必填 | 说明 |
| --- | :-- | :-- | :-- |
| fileUrl | string | 是 | 文件链接或文件码 |
| {token} | string | 是 | utoken/apitoken/apptoken/... |

返回值

		文件流
		{"ErrorCode":0,"ErrorMsg":"成功","Data":null}
***


##获取文件/文件夹信息
---------


URI
		http://<server>/alm/rest/file/DataArea/{fileArea}/{filePath}

**方法:GET**

| 参数 | 类型 | 是否必填 | 说明 |
| --- | :-- | :-- | :-- |
| fileArea | string | 是 | 文件区块（目前只支持项目区：project） |
| filePath | string | 否 | 文件相对路径（为空时，取根目录信息），eg：/Test1/001 |
| pathType | int | 否 | 路径类型，0（默认）：相对路径；1：文件链接或文件码 |
| proId | string | 否 | 项目id，获取项目区文件必需传项目id |
| {token} | string | 是 | utoken/apitoken/apptoken/... |

返回值
		{"ErrorCode":0,"ErrorMsg":"成功","Data":{"FileAttributes":{"FileName":"aa.docx","FileSize":1000,"FileSizeShow":"1000 字节","SecretLevel":-1,"SecretLevelShow":"非密","MD5Code":"AAAAAAAAAAAAAAAAAAAAAAA","DownloadCount":0,"PreviewCount":0,"CreateTime":"2019-10-09","CreateBy":"user","CreateByShow":"演示用户","LastModifyTime":"2019-10-09","LastModifyBy":"user","LastModifyByShow":"演示用户"},"Childrens":[{"FileName":"test.doc","FileCode":"*****",...}]}}
***


##创建文件/文件夹
---------


URI
		http://<server>/alm/rest/file/DataArea/{fileArea}/{filePath}

**方法:POST**

| 参数 | 类型 | 是否必填 | 说明 |
| --- | :-- | :-- | :-- |
| fileArea | string | 是 | 文件区块（目前只支持项目区：project） |
| filePath | string | 否 | 文件相对路径（为空时，取根目录信息），eg：/Test1/001 |
| pathType | int | 否 | 路径类型，0（默认）：相对路径；1：文件链接或文件码 |
| proId | string | 否 | 项目id，获取项目区文件必需传项目id |
| files | HttpFileCollectionBase | 否 | 客户端上载文件集合,创建文件是必传 | 
| rootCode | string | 否 | 上传文件夹code，当文件夹分批次上传时，第一次上传空字符串，剩下的次数必须传递第一次上传成功返回的文件夹code（RootCode） |
| fileBlock | string | 否 | 文件区块，project-项目（默认），personal-个人，site-站点，other-其他 |
| fileOtherInfo | string | 否 | 文件/文件夹上传重复处理信息（暂时无效） |
| fileTreeStr | string | 否 | 文件树，如果分批次上传，每一次上传结束返回有FileTree信息，则需要在之后的接口传递该信息 |
| bigUpload | bool | 否 | 是否为超大文件上传 |
| bigUploadEnd | bool | 否 | 是否超大文件上传结束 |
| c_fileCode | string | 否 | 超大文件code |
| autoCreateDirectory | int | 否 | 默认为0：若无该目录则创建，1：若无该目录则返回为空（仅pathType=0时有效） |
| dealNuptial | int | 否 | 处理重名文件/文件夹；0：跳过，1：覆盖，2：保留
| keepTime | int | 否 | 文件存放时间，默认为0：永久文件，》0：临时文件（失效min） |
| {token} | string | 是 | utoken/apitoken/apptoken/... ||

返回值
		{"ErrorCode":0,"ErrorMsg":"成功","Data":{"RelativePath（相对路径）":["filecode:1111.txt"],"AbsolutePath（绝对路径）":["filecode:1111.txt"],"RootCode（文件夹code）":["****"],"FilesCode"（文件code）:["****"],"BigFileCode"（超大文件code）:[],"FileTree"（文件树）:[],"N_BigFileCode"（文件夹内超大文件code）:[]}
***


##更新/写入文件
---------


URI
		http://<server>/alm/rest/file/DataArea/{fileArea}/{filePath}

**方法:PUT**

| 参数 | 类型 | 是否必填 | 说明 |
| --- | :-- | :-- | :-- |
| fileArea | string | 是 | 文件区块（目前只支持项目区：project） |
| filePath | string | 否 | 文件相对路径（为空时，取根目录信息），eg：/Test1/001 |
| pathType | int | 否 | 路径类型，0（默认）：相对路径；1：文件链接或文件码 |
| proId | string | 否 | 项目id，获取项目区文件必需传项目id |
| files | HttpFileCollectionBase | 是 | 客户端上载文件集合 |
| operateType | int | 否 | 文件操作类型，默认为0：更新，1：写入 |
| {token} | string | 是 | utoken/apitoken/apptoken/... |

返回值
		{"ErrorCode":0,"ErrorMsg":"成功","Data":{}
***


##更新/写入文件
---------


URI
		http://<server>/alm/rest/file/DataArea/{fileArea}/{filePath}

**方法:DELETE**

| 参数 | 类型 | 是否必填 | 说明 |
| --- | :-- | :-- | :-- |
| fileArea | string | 是 | 文件区块（目前只支持项目区：project） |
| filePath | string | 否 | 文件相对路径（为空时，取根目录信息），eg：/Test1/001 |
| pathType | int | 否 | 路径类型，0（默认）：相对路径；1：文件链接或文件码 |
| proId | string | 否 | 项目id，获取项目区文件必需传项目id |
| isPhysicalDelection | bool | 否 | 是否物理删除，默认false |
| {token} | string | 是 | utoken/apitoken/apptoken/... |

返回值
		{"ErrorCode":0,"ErrorMsg":"成功","Data":{}
***


		


