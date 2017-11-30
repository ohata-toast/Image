## Contents > Image > Developer's Guide

## 사전 준비

- API 사용을 위해서는 AppKey와 SecretKey가 필요합니다. SecreKey는 “생성” 버튼을 클릭하여 생성해 주세요.

![[그림 1 API용 AppKey와 SecreKey 확인]](http://static.toastoven.net/prod_img2/img_11.png)
<center>[그림 1 API용 AppKey와 SecreKey 확인]</center>

## SecretKey 인증 처리

- SecreKey는 인증 처리에 필요하며, 모든 API 요청시에 Request Header에 Authorization 에 넣어서 요청해야 합니다.

[Request Header 공통]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

## 응답 성공 Sample

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
	}
}
```

## 응답 에러 Sample

```
{
	"header": {
		"isSuccessful": false,
		"resultCode": 404,
		"resultMessage": "Please check your API Url, HTTP Method."
	}
}
```

## API 목록

### 폴더 생성

[Request Url]

|Method|Request Url|
|---|----|
|POST|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/folders|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|
|Content–Type|application/json| - |

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| - |토스트 클라우드 web console을 통해서 생성한 appkey|

[Request Body]

- json 객체로 전달합니다.

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|path|string|Y| - |생성 할 폴더의 절대 경로, 상위 폴더 자동 생성|

[Request Example]

- myfolder라는 이름의 폴더를 루트 폴더 하위에 생성합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X POST "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/folders" \
-H "Authorization: {secretKey}" \
-H "Content-Type: application/json" \
--data "{'path': '/myfolder'}"
```

[Response Field]

|이름|Type|설명|
|---|---|----|
|folder|object|폴더 정보|

[folder Type]

|이름|Type|설명|
|---|---|----|
|isFolder|boolean|폴더 여부|
|id|string|고유 ID|
|name|string|폴더 이름|
|path|string|폴더 절대 경로|
|updatedAt|date|최종 수정일|

[Response Sample]

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
	},
	"folder": {
		"id": "c337256d-b17e-42ce-9f63-a792a05ae0ef",
		"name": "myfolder",
		"path": "/myfolder",
		"isFolder": true,
		"updatedAt": "2015-09-02T10:27:15+0900"
	}
}
```

### 폴더 내 파일 목록 (검색)

[Request Url]

|Method|Request Url|
|---|----|
|GET|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/folders|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| - |토스트 클라우드 web console을 통해서 생성한 appkey|
|basepath|string|Y| - |조회 할 폴더의 절대 경로|
|createdBy|string|N| - |목록 조회 대상 (공백: 전체, U: 사용자 업로드 이미지, P: 오퍼레이션 이미지)|
|name|string|N| - |검색 할 이미지 이름|
|page|integer|N|1|페이지 번호|
|rows|integer|N|100|조회 개수 (max: 10000)|
|sort|string|N|name:asc|정렬 방식 (정렬대상 : name or date, 정렬방식 : asc or desc)|

[Request Example]

- /myfolder 하위의 폴더와 파일을 조회합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X GET "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/folders?basepath=/myfolder" \
-H "Authorization: {secretKey}"
```

[Response Field]

|이름|Type|설명|
|---|---|----|
|paging|object|페이징 정보|
|folders|list|폴더 목록|
|files|list|이미지 파일 목록|

[paging Type]

|이름|Type|설명|
|---|---|----|
|page|integer|요청 페이지 번호|
|rows|integer|요청 조회 개수|
|totalFolderCount|long|전체 폴더 개수|
|totalFileCount|long|전체 폴더 개수|

[folders Types]

|이름|Type|설명|
|---|---|----|
|isFolder|boolean|폴더 여부|
|id|string|고유 ID|
|name|string|폴더 이름|
|path|string|폴더 절대 경로|
|updatedAt|date|최종 수정일|

[files Types]

|이름|Type|설명|
|---|---|----|
|isFolder|boolean|폴더 여부|
|id|string|고유 ID|
|url|string|이미지 서비스 Url|
|name|string|이미지 이름|
|path|string|이이미지 절대 경로|
|bytes|long|이미지 파일 크기|
|createdBy|string|이미지 구분 (U: 사용자 업로드 이미지, P: 오퍼레이션 이미지)|
|updatedAt|date|최종 수정일|
|operationId|string|createdBy === P 의 경우 참조 된 오퍼레이션 ID|
|imageProperty|object|이미지 속성|
|queues|list|작업 정보 목록 (해당 API에서는 사용되지 않음)|

[imageProperty Types]

|이름|Type|설명|
|---|---|----|
|width|integer|가로 크기|
|height|integer|세로 크기|
|coordinate|object|GPS 정보|
|createdAt|date|촬영일 또는 생성일|

[coordinate Type]

|이름|Type|설명|
|---|---|----|
|lat|double|위도|
|lng|double|경도|

[Response Sample]

```
{
	"header": {
		// 생략
	},
	"paging": {
		"page": 1,
		"rows": 100,
		"totalFolderCount": 1,
		"totalFileCount": 1
	},
	"folders": [
		{
		"isFolder": true,
		"id": "5b6ad839-a920-4b88-895d-64ffc3f4d89a",
		"name": "banner",
		"path": "/myfolder/banner",
		"updatedAt": "2016-02-26T15:57:06+0900"
		}
	],
	"files": [
		{
			"isFolder": false,
			"id": "69528a77-0cc2-4407-a83d-ea4aacbe207f",
			"url": "http://image.toast.com/aaaaaag/myfolder/toast.png",
			"name": "toast.png",
			"path": "/myfolder/toast.png",
			"bytes": 10173,
			"createdBy": "U",
			"updatedAt": "2016-02-26T15:57:14+0900",
			"operationId": "",
			"queues": []
			"imageProperty": {
				"width": 90,
				"height": 90,
				"createdAt": "2016-02-26T15:56:50+0900"
				"coordinate": {
					"lat": 0,
					"lng": 0
				}
			}
		}
	]
}
```

### 폴더 속성 조회

[Request Url]

|Method|Request Url|
|---|----|
|GET|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/properties|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| - |토스트 클라우드 web console을 통해서 생성한 appkey|
|path|string|Y| -|조회 할 폴더의 절대 경로|

[Request Example]

- myfolder의 폴더의 속성을 조회합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X GET "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/properties?path=/myfolder" \
-H "Authorization: {secretKey}"
```

[Response Field]

|이름|Type|설명|
|---|---|----|
|folder|object|폴더 정보|

[folder Type]

|이름|Type|설명|
|---|---|----|
|isFolder|boolean|폴더 여부|
|id|string|고유 Id|
|name|string|폴더 이름|
|path|string|폴더 절대 경로|
|bytes|long|폴더 크기 (byte)|
|totalFolderCount|long|하위 폴더 개수|
|totalFileCount|long|하위 파일 개수|
|updatedAt|date|최종 수정일|

[Response Sample]

```
{
	"header": {
		// 생략
	},
	"folder": {
		"isFolder": true,
		"id": "996dd430-5172-4178-86c9-0704e88b28e3",
		"name": "myfolder",
		"path": "/myfolder",
		"bytes": 64857,
		"totalFolderCount": 1,
		"totalFileCount": 2,
		"updatedAt": "2016-02-26T15:57:06+0900"
	}
}
```

### 단일 파일 업로드

[Request Url]

|Method|Request Url|
|---|----|
|PUT|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/images|

[Request Header]

|이름|Type|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -| 토스트 클라우드 web console을 통해서 생성한 appkey|
|path|string|Y|-|생성 할 절대 경로의 파일명|
|overwrite|boolean|N|false|같은 이름이 있을 경우 덮어쓰기 여부|
|autorename|boolean|N|false|같은 이름이 있을 경우 '이름(1).확장자' 형식으로 파일명 변경 여부|
|operationIds|string|N| -|이미지 오퍼레이션 ID 리스트 (콤마로 구분됨)|

- 이미지 오퍼레이션 ID를 추가해서 요청할 경우, 업로드 시 원하는 옵션으로 오퍼레이션 파일을 생성할 수 있습니다. 이미지 오퍼레이 션 관련 API를 참고합니다.

[Request Body]

- 이미지 파일의 Binary Data를 넣습니다.

[Request Example]

- /myfolder 폴더에 sample.png 이미지를 업로드 합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X PUT "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/images?path=/myfolder/sample.png&overwrite=true" \
-H "Authorization: {secretKey}" \
-H "Content-Type:application/octet-stream" \
--data-binary "path/to/imageFile/@sample.png"
```

[Response Field]

|이름|Type|설명|
|---|---|----|
|file|object|이미지 파일 정보|

[file Types]

|이름|Type|설명|
|---|---|----|
|isFolder|boolean|폴더 여부|
|id|string|고유 Id|
|url|string|이미지 서비스 Url|
|name|string|이미지 이름|
|path|string|이미지 절대 경로|
|bytes|long|이미지 파일 크기|
|createdBy|string|이미지 구분 (U: 사용자 업로드 이미지, P: 오퍼레이션 이미지)|
|updatedAt|date|최종 수정일|
|operationId|string|createdBy === P 의 경우 참조 된 오퍼레이션 ID|
|imageProperty|object|이미지 속성|
|queues|list|operationIds 요청에 의한 작업 정보 목록|

[imageProperty Type]

|이름|Type|설명|
|---|---|----|
|width|integer|가로 크기|
|height|integer|세로 크기|
|coordinate|object|GPS 정보|
|createdAt|date|촬영일 또는 생성일|

[coordinate Type]

|이름|Type|설명|
|---|---|----|
|lat|double|위도|
|hlng|double|경도|

[queues Type]

|이름|Type|설명|
|---|---|----|
|queueId|string|작업 고유 ID|
|queueType|string|작업 구분 (image: 오퍼레이션, swift: 파일 및 폴더 삭제)|
|status|string|작업 상태 (W: 대기중, D: 완료, P: 작업중, F: 실패)|
|tryCount|integer|다시 시도 횟수|
|queuedAt|date|작업 등록일|
|operationId|string|참조되는 오퍼레이션 ID|
|url|string|서비스 될 이미지 서비스 Url|
|name|string|생성 될 이미지 이름|
|path|string|생성 될 이미지 절대 경로|

[Response Sample]

```
{
	"header": {
		// 생략
	},
	"file": {
		"isFolder": false,
		"id": "9cf11176-045c-4708-8dbd-35633f029a91",
		"url": "http://image.toast.com/aaaaach/myfolder/sample.png",
		"name": "sample.png",
		"path": "/myfolder/sample.png",
		"bytes": 54684,
		"createdBy": "U",
		"updatedAt": "2016-02-26T16:38:34+0900",
		"operationId": "100x100",
		"imageProperty": {
			"width": 200,
			"height": 150,
			"createdAt": "2016-02-26T16:38:11+0900",
			"coordinate": {
				"lat": null,
				"lng": null
			}
		},
		"queues": [
			"queueId": "0256316c-7dcf-4940-975b-673afb62e8a3",
			"queueType": "image",
			"status": "W",
			"tryCount": 0,
			"queuedAt": "2016-02-26T16:38:11+0900",
			"operationId": "100x100",
			"url": "http://image.toast.com/aaaaach/myfolder/sample_100x100.png",
			"name": "sample_100x100.png",
			"path": "/myfolder/sample_100x100.png"
		],
	}
}
```

### 다중 파일 업로드

|Method|Request Url|
|---|----|
|POST|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/images|

[Request Header]

|이름|Type|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appkey|

[Request Body]

- multipart/form–data 형식으로 전달합니다.

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|files|multipart/form–data|Y| -|이미지 파일 리스트|
|params|string|Y|-|업로드 옵션(json 형태의 문자열)|

[params 옵션]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|basepath|string|Y| -|업로드 할 절대 경로|
|overwrite|boolean|N|false|같은 이름이 있을 경우 덮어쓰기 여부|
|autorename|boolean|N|false|같은 이름이 있을 경우 '이름(1).확장자' 형식으로 파일명 변경 여부|
|operationIds|list|N| -|이미지 오퍼레이션 ID 리스트 (json 형태의 문자열). 업로드 시 원하는 옵션으로 오퍼레이션 파일을 생성. 이미지 오퍼레이션 관련 API 참고|
|callbackUrl|string|N| -|처리 결과를 통보받을 Url 경로. query string 형식으로 id를 적으면 callback 전송 시 같이 전달됨. 포트는 80, 443만 지원|

[Request Example]

- /myfolder/banner 폴더에 left.png, right.png 이미지를 업로드 합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X POST "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/images" \
-H "Authorization: {secretKey}" \
-F "params={'basepath': '/myfolder/banner', 'overwrite': true, 'operationIds':['100x100']}" \
-F "files=@left.png" \
-F "files=@right.png"
```

[Response Field]

|이름|Type|설명|
|---|---|----|
|errors|list|업로드 실패 목록|
|successes|list|업로드 성공 목록|

[errors Type]

|이름|Type|설명|
|---|---|----|
|path|string|파일 절대 경로|
|bytes|long|파일 크기|
|error|object|에러 정보|

[error Type]

|이름|Type|설명|
|---|---|----|
|resultCode|integer|에러 코드|
|resultMessage|string|에러 메시지|

[successes Type]

|이름|Type|설명|
|---|---|----|
|isFolder|boolean|폴더 여부|
|id|string|고유 ID|
|url|string|이미지 서비스 Url|
|name|string|이미지 이름|
|path|string|이미지 절대 경로|
|bytes|long|이미지 파일 크기|
|createdBy|string|이미지 구분 (U: 사용자 업로드 이미지, P: 오퍼레이션 이미지)|
|updatedAt|date|최종 수정일|
|operationId|string|createdBy === P 의 경우 참조 된 오퍼레이션 ID|
|imageProperty|object|이미지 속성|
|queues|list|operationIds 요청에 의한 작업 정보 목록|

[imageProperty Type]

|이름|Type|설명|
|---|---|----|
|width|integer|가로 크기|
|height|integer|세로 크기|
|coordinate|object|GPS 정보|
|createdAt|date|촬영일 또는 생성일|

[coordinate Type]

|이름|Type|설명|
|---|---|----|
|lat|double|위도|
|lng|double|경도|

[queues Type]

|이름|Type|설명|
|---|---|----|
|queueId|string|작업 고유 ID|
|queueType|string|작업 구분 (image: 오퍼레이션, swift: 파일 및 폴더 삭제)|
|status|string|작업 상태 (W: 대기중, D: 완료, P: 작업중, F: 실패)|
|tryCount|integer|다시 시도 횟수|
|queuedAt|date|작업 등록일|
|operationId|string|참조되는 오퍼레이션 ID|
|url|string|서비스 될 이미지 서비스 Url|
|name|string|생성 될 이미지 이름|
|path|string|생성 될 이미지 절대 경로|

[Response Sample]

```
{
	"header": {
		// 생략
	},
	"errors": [],
	"successes": [
		{
			"isFolder": false,
			"id": "5fa8ce52-d066-490c-85dd-f8cef181dd28",
			"url": "http://image.toast.com/aaaaach/myfolder/banner/left.png",
			"name": "left.png",
			"path": "/myfolder/banner/left.png",
			"bytes": 7322,
			"createdBy": "U",
			"updatedAt": "2016-02-26T16:56:50+0900",
			"operationId": "100x100",
			"imageProperty": {
				"width": 60,
				"height": 60,
				"createdAt": "2016-02-26T16:56:27+0900"
				"coordinate": {
					"lat": null,
					"lng": null
				}
			},
			"queues": [
				{
					"queueId": "bef1736f-6637-459e-ae10-ac0961ebf59c",
					"queueType": "image",
					"status": "W",
					"tryCount": 0,
					"queuedAt": "2016-02-26T16:56:29+0900",
					"operationId": "100x100",
					"url": "http://image.toast.com/aaaaach/myfolder/banner/left_100x100.png",
					"name": "left_100x100.png",
					"path": "/myfolder/banner/left_100x100.png"
				}
			]
		},
		{
			"isFolder": false,
			"id": "96f726bd-93e4-4f7c-ad55-56e85aa323a8",
			"url": "http://image.toast.com/aaaaach/myfolder/banner/right.png",
			"name": "right.png",
			"path": "/myfolder/banner/right.png",
			"bytes": 267801,
			"createdBy": "U",
			"updatedAt": "2016-02-26T16:56:51+0900",
			"operationId": "100x100",
			"imageProperty": {
				"width": 1440,
				"height": 2560,
				"createdAt": "2016-02-26T16:56:28+0900",
				"coordinate": {
					"lat": null,
					"lng": null
				}
			},
			"queues": [
				{
					"queueId": "6691a01a-4585-4e26-989c-8ef25dd627a0",
					"queueType": "image",
					"status": "W",
					"tryCount": 0,
					"queuedAt": "2016-02-26T16:56:29+0900",
					"operationId": "100x100",
					"url": "http://image.toast.com/aaaaach/myfolder/banner/right_100x100.png",
					"name": "right_100x100.png",
					"path": "/myfolder/banner/right_100x100.png"
				}
			]
		}
	]
}
```

[Callback Sample]

```
{
	"header": {
		"resultMessage": "Partial Success",
		"resultCode": 1,
		"isSuccessful": false
	},
	"id": "100",
	"successFiles": [
		{
			"id": "c4de7cec-ba9c-44fa-b1fc-93e2eb29ed10",
			"name": "image.jpg",
			"path": "/myfolder/image.jpg",
			"url": "http://image.toast.com/aaaaach/myfolder/image.jpg",
			"width": 546,
			"height": 304,
			"sizeByte": 105190,
			"updatedAt": "2017-11-28T14:30:14+0900"
		}
	],
	"failFiles": [
		{
			"name": "test.jpg",
			"path": "/myfolder/test.jpg",
			"sizeByte": 105190,
			"resultCode": 20010,
			"resultMessage": "There is same file name."
		},
		{
			"name": "big_size.jpg",
			"path": "/myfolder/big_size.jpg",
			"sizeByte": 12925663,
			"resultCode": 11004,
			"resultMessage": "It was exceed the max volume you can upload at once."
		}
	]
}
```

### 단일 삭제 (동기)

[Request Url]

|Method|Request Url|
|---|----|
|DELETE|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/images/sync|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appkey|
|folderId|integer| -| -| 삭제 할 폴더의 ID|
|fileld|integer| -| -| 삭제 할 파일의 ID|
|includeThumbnail|boolean|N|false|삭제 할 파일에 의해 생성된 오퍼레이션 파일도 삭제|

- 'folderId' 또는 'fileId'는 최소 하나 필수 파라미터로 사용해야 합니다.

[Request Example]

- /myfolder/sample.png의 파일을 삭제합니다.
- /myfolder/sample.png의 ID는 우측 메뉴의 '폴더 내 파일 목록 (검색)' API를 통해서 알 수 있습니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X DELETE "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/images/sync?
fileId=9cf11176-045c-4708-8dbd-35633f029a91" \
-H "Authorization:{secretKey}"
```

[Response Sample]

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
	}
}
```

### 다중 삭제 (비동기)

[Request Url]

|Method|Request Url|
|---|----|
|DELETE|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/images/async|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appkey|
|folderIds|string| -| -|삭제 할 폴더의 ID 리스트 (콤마로 구분됨)|
|files|string| -| -| 삭제 할 파일의 ID 리스트 (콤마로 구분됨)|
|includeThumbnail|boolean|N|false|삭제 할 파일에 의해 생성된 오퍼레이션 파일도 삭제|

- 'folderIds' 또는 'fileIds'는 최소 하나 필수 파라미터로 사용해야 합니다.

[Request Example]

- /myfolder/banner/left.png, /myfolder/banner/right.png의 파일을 삭제합니다.
- ID는 우측 메뉴의 '폴더 내 파일 목록 (검색)' API를 통해서 알 수 있습니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X DELETE "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/images/async?
fileIds=5fa8ce52-d066-490c-85dd-f8cef181dd28,96f726bd-93e4-4f7c-ad55-56e85aa323a8" \
-H "Authorization:{secretKey}"
```

[Response Sample]

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
	}
}
```

## 이미지 오퍼레이션

- 이미지 오퍼레이션 API를 통해 다양한 썸네일을 생성할 수 있습니다.
- 썸네일 크기, 흑백 필터, 크롭(Rectangle, Circle, Slice), 워터마크 제공

### 이미지 오퍼레이션 생성 및 수정

[Request Url]

|Method|Request Url|
|---|----|
|PUT|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/operations/{operationId}|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|
|Content–Type|application/json| -|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appKey|
|operationId|string|Y| -|생성 및 수정 할 오퍼레이션 이름|

[Request Body]

- json 객체로 전달합니다.

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|description|string|N| -|오퍼레이션 설명|
|realtimeService|boolean|N|true|실 시간 서비스 제공 여부|
|data|list|N| -|오퍼레이션 Flow|

[data 옵션]

- 썸네일 크기

```
{
	"templateOperationId": "resize_max_fit", 	// (required, value: resize_fixed / resize_min_fit / resize_max_fit)
												// 기반이 되는 템플릿 ID
	"option": {
		"width": int, 							// (required) 가로 사이즈
		"height": int, 							// (required) 세로 사이즈
		"quality": double, 						// (optional, default: 75, value: 1~100) 품질
		"upDownSizeType": String, 				// (optional, default: downOnly, value: downOnly / upOnly / upDownAll)
												// 원본 이상으로 확대/축소 불가 여부
		"keepAnimation": boolean, 				// (optional, default: true) GIF 애니메이션 유지 여부
		"keepExi"f: boolean, 					// (optional, default: true) 메타정보 유지 여부
		"autoOrient": boolean, 					// (optional, default: false) Orientation 정보를 기준으로 회전 여부
		"targetExtension": String 				// (optional, default: null) 출력 포맷(확장자)
	}
}
```

- 흑백 필터

```
{
	"templateOperationId": "gray", 		// (required) 기반이 되는 템플릿 ID
	"option": {  						// (required) 옵션 없음
		"keepAnimation": boolean 		// (optional, default: true) GIF 애니메이션 유지 여부
	}
}
```

- Rectangle 크롭

```
{
	"templateOperationId": "rectangle",	// (required) 기반이 되는 템플릿 ID
	"option": {
		"gravity": String, 				// (optional, default: Center, value: NorthWest / North / NorthEast /
										// West / Center / East / SouthWest / South / SouthEast)
										// 기준 위치
		"offsetX": int,					// (optional, default: 0) 기준 위치 이동. 음수는 반대로 이동
		"offsetY": int,					// (optional, default: 0) 기준 위치 이동. 음수는 반대로 이동
		"width": int, 					// (required) 가로 사이즈
		"height": int, 					// (required) 세로 사이즈
		"keepAnimation": boolean 		// (optional, default: false) GIF 애니메이션 유지 여부
	}
}
```

- Circle 크롭

```
{
	"templateOperationId": "circle", 	// (required) 기반이 되는 템플릿 ID
	"option": {
		"gravity": String, 				// (optional, default: Center, value: NorthWest / North / NorthEast /
										// West / Center / East / SouthWest / South / SouthEast)
										// 기준 위치
		"offsetX": int,					// (optional, default: 0) 기준 위치 이동. 음수는 반대로 이동
		"offsetY": int,					// (optional, default: 0) 기준 위치 이동. 음수는 반대로 이동
		"radius": int 					// (required) 반지름
	}
}
```

- Slice 크롭 : 가로, 세로 분할

```
{
	"templateOperationId": "slice",		// (required) 기반이 되는 템플릿 ID
	"option": {
		"sliceCropType": String, 		// (optional, default: "vertical") 분할 방식 (vertical, horizontal)
		"sliceSize": int, 				// (optional, default: 0) 분할 크기
		"keepAnimation": boolean, 		// (optional, default: true) GIF 애니메이션 유지 여부
		"callbackUrl": string 			// (optional) 처리 결과를 통보받을 Url 경로. 포트는 80, 443만 지원
	}
}
```

- Slice 크롭 : 격자 분할

```
{
	"templateOperationId": "grid", 		// (required) 기반이 되는 템플릿 ID
	"option": {
		"countX": int, 					// (Required) 가로 분할 갯수
		"countY": int, 					// (Required) 세로 분할 갯수
										// 원본 사이즈에 따라서 분할 개수로 나누었을때 조각 사이즈가 정수가 아닌
										// 경우에는 설정한 분할 개수 보다 적은 수로 분할 될 수 있습니다.
		"callbackUrl": string 			// (optional) 처리 결과를 통보받을 Url 경로. 포트는 80, 443만 지원
	}
}
```

- 워터마크

```
{
	"templateOperationId": "watermark", // (required) 기반이 되는 템플릿 ID
	"option": {
		"gravity": String, 				// (optional, default: Center, value: NorthWest / North / NorthEast /
										// West / Center / East / SouthWest / South / SouthEast)
										// 기준 위치
		"offsetX": int,					// (optional, default: 0) 기준 위치 이동. 음수는 반대로 이동
		"offsetY": int,					// (optional, default: 0) 기준 위치 이동. 음수는 반대로 이동
		"watermarkImagePath": String 	// (Required) 합성 할 이미지 파일의 경로
	}
}
```

[Request Example]

- 긴 축 길이 기준으로 사이즈를 100x100으로 줄이는 작업을 100x100이라는 이름으로 생성 또는 수정합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X PUT "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/operations/100x100" \
-H "Authorization:{secretKey}" \
-H "Content-Type: application/json" \
--data "{'description': '', 'realtimeService': true, 'data': [{'templateOperationId': 'resize_max_fit',
'option': {'resizeType': 'max_fit', 'width': 100, 'height': 100, 'quality': 80, 'upDownSizeType':
'downOnly'}}]}"
```

[Response Field]

|이름|Type|설명|
|---|---|----|
|operation|object|오퍼레이션 정보|

[operation Type]

|이름|Type|설명|
|---|---|----|
|appKey|string| 사용자 appkey|
|operationId|string|오퍼레이션 이름|
|description|string|오퍼레이션 설명|
|realtimeService|boolean|실 시간 서비스 제공 여부|
|jobTemplate|list|오퍼레이션 Flow|
|updatedAt|date|최종 수정일|

[jobTemplate Type]

|이름|Type|설명|
|---|---|----|
|templateOperationId|string|기반이 되는 템플릿 ID|
|jobType|string|작업 구분|
|option|object|작업 내용|

[Response Sample]

```
{
	"header": {
		// 생략
	},
	"operation": {
	"appKey": {appKey},
	"operationId": "100x100",
	"description": "",
	"realtimeService": true,
	"updatedAt": "2016-02-26T17:42:27+0900",
	"jobTemplate": [
		{
			"templateOperationId": "resize_max_fit",
			"jobType": "ResizeJob",
			"option": {
				"resizeType": "max_fit",
				"width": 100,
				"height": 100,
				"quality": 80,
				"shrinkLargerOnly": false,
				"upDownSizeType": "downOnly",
				"keepAnimation": false,
				"keepExif": true,
				"autoOrient": true
			}
		}
	]
}
```

### 이미지 오퍼레이션 목록 (검색)

[Request Url]

|Method|Request Url|
|---|----|
|GET|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/operations|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appkey|
|template|boolean|N|false|목록 조회 대상 (true: 기본 오퍼레이션, false: 사용자 생성 오퍼레이션)|
|name|string|N| -|검색 할 오퍼레이션 이름 (입력 값으로 시작하는)|
|page|integer|N|1|페이지 번호|
|rows|integer|N|20|조회 개수 (max: 10000)|
|sort|string|N|date:desc|정렬 방식 (정렬대상 : name or date, 정렬방식 : asc or desc)|

[Request Example]

- 사용자가 생성한 오퍼레이션 목록을 조회합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X GET "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/operations?template=false" \
-H "Authorization:{secretKey}"
```

[Response Field]

|이름|Type|설명|
|---|---|----|
|paging|object|페이징 정보|
|operations|list|오퍼레이션 목록|

[paging Type]

|이름|Type|설명|
|---|---|----|
|page|integer|요청 페이지 번호|
|rows|integer|요청 조회 개수|
|totalCount|long|전체 개수|

[operations Type]

|이름|Type|설명|
|---|---|----|
|appKey|string|사용자 appkey|
|operationId|string|오퍼레이션 이름|
|description|string|오퍼레이션 설명|
|realtimeService|boolean|실 시간 서비스 제공 여부|
|jobTemplate|list|오퍼레이션 Flow|
|updatedAt|date|최종 수정일|

[jobTemplate Type]

|이름|Type|설명|
|---|---|----|
|templateOperationId|string|기반이 되는 템플릿 ID|
|jobType|string|작업 구분|
|option|object|작업 내용|

[Response Sample]

```
{
	"header": {
		// 생략
	},
	"paging": {
		"page": 1,
		"rows": 20,
		"totalCount": 1
	},
	"operations": [
		{
			"appKey": {appKey},
			"operationId": "100x100",
			"description": "",
			"realtimeService": true,
			"updatedAt": "2016-02-26T17:42:27+0900",
			"jobTemplate": [
				{
					"templateOperationId": "resize_max_fit",
					"jobType": "ResizeJob",
					"option": {
						"resizeType": "max_fit",
						"width": 100,
						"height": 100,
						"quality": 80,
						"shrinkLargerOnly": false,
						"upDownSizeType": "downOnly",
						"keepAnimation": false,
						"keepExif": true,
						"autoOrient": true
					}
				}
			]
		}
	]
}
```

### 이미지 오퍼레이션 상세 조회

[Request Url]

|Method|Request Url|
|---|----|
|GET|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/operations/{operationId}|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appkey|
|operationId|string|Y| -|조회 할 오퍼레이션 이름|

[Request Example]

- 100x100 오퍼레이션을 조회합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X GET "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/operations/100x100" \
-H "Authorization:{secretKey}"
```

[Response Field]

|이름|Type|설명|
|---|---|----|
|operation|object|오퍼레이션 정보|

[operation Type]

|이름|Type|설명|
|---|---|----|
|appKey|string|사용자 appkey|
|operationId|string|오퍼레이션 이름|
|description|string|오퍼레이션 설명|
|realtimeService|boolean|실 시간 서비스 제공 여부|
|jobTemplate|list|오퍼레이션 Flow|
|updatedAt|date|최종 수정일|

[jobTemplate Type]

|이름|Type|설명|
|---|---|----|
|templateOperationId|string|기반이 되는 템플릿 ID|
|jobType|string|작업 구분|
|option|object|작업 내용|

[Response Sample]

```
{
	"header": {
		// 생략
	},
	"operation": {
		"appKey": {appKey},
		"operationId": "100x100",
		"description": "",
		"realtimeService": true,
		"updatedAt": "2016-02-26T17:42:27+0900",
		"jobTemplate": [
			{
				"templateOperationId": "resize_max_fit",
				"jobType": "ResizeJob",
				"option": {
					"resizeType": "max_fit",
					"width": 100,
					"height": 100,
					"quality": 80,
					"shrinkLargerOnly": false,
					"upDownSizeType": "downOnly",
					"keepAnimation": false,
					"keepExif": true,
					"autoOrient": true
				}
			}
		]
	}
}
```

### 이미지 오퍼레이션 삭제

[Request Url]

|Method|Request Url|
|---|----|
|DELETE|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/operations/{operationId}|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appkey|
|operationId|string|Y| -|삭제 할 오퍼레이션 이름|

[Request Example]

- 100x100 오퍼레이션을 삭제합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X DELETE "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/operations/100x100" \
-H "Authorization:{secretKey}"
```

[Response Sample]

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
	}
}
```

### 이미지 오퍼레이션 실행

[Request Url]

|Method|Request Url|
|---|----|
|POST|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/operations-exec|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|
|Content–Type|application/json| - |

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appkey|

[Request Body]

- json 객체로 전달합니다.

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|basepath|string|Y| -|기준이 되는 폴더의 절대 경로|
|filepaths|list|Y| -|실행 할 절대 경로의 폴더 및 파일 리스트|
|operationIds|list|Y| -|실행 할 오퍼레이션 ID 리스트|
|callbackUrl|string|N| -|처리 결과를 통보받을 URL 경로. query string 형식으로 id를 적으면 callback 전송 시 같이 전달됨. 포트는 80, 443만 지원|

[Request Example]

- /myfolder/left.png, /myfolder/right.png 원본 파일로 100x100 오퍼레이션 옵션이 적용된 파일을 생성합니다
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X POST "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/operations-exec" \
-H "Authorization:{secretKey}" \
-H "Content-Type: application/json" \
--data "{'basepath': '/myfolder', 'operationIds': ['100x100'], 'filepaths': ['/myfolder/left.png',
'/myfolder/right.jpg']}"
```

[Response Field]

|이름|Type|설명|
|---|---|----|
|operations|list|실행 오퍼레이션 목록|
|queues|list|작업 정보 목록|

[operations Type]

|이름|Type|설명|
|---|---|----|
|appKey|string|사용자 appkey|
|operationId|string|오퍼레이션 이름|
|description|string|오퍼레이션 설명|
|realtimeService|boolean|실 시간 서비스 제공 여부|
|jobTemplate|list|오퍼레이션 Flow|
|updatedAt|date|최종 수정일|

[jobTemplate Type]

|이름|Type|설명|
|---|---|----|
|templateOperationId|string|기반이 되는 템플릿 ID|
|jobType|string|작업 구분|
|option|object|작업 내용|

[queues Type]

|이름|Type|설명|
|---|---|----|
|queueId|string|작업 고유 ID|
|queueType|string|작업 구분 (image: 오퍼레이션, swift: 파일 및 폴더 삭제)|
|status|string|작업 상태 (W: 대기중, D: 완료, P: 작업중, F: 실패)|
|tryCount|integer|다시 시도 횟수|
|queuedAt|date|작업 등록일|
|operationId|string|참조되는 오퍼레이션 ID|
|url|string|서비스 될 이미지 서비스 URL|
|name|string|생성 될 이미지 이름|
|path|string|생성 될 이미지 절대 경로|

[Response Sample]

```
{
	"header": {
		// 생략
	},
	"paging": {
		"page": 1,
		"rows": 20,
		"totalCount": 1
	},
	"operations": [
		{
			"appKey": {appKey},
			"operationId": "100x100",
			"description": "",
			"realtimeService": true,
			"updatedAt": "2016-02-26T17:42:27+0900",
			"jobTemplate": [
				{
					"templateOperationId": "resize_max_fit",
					"jobType": "ResizeJob",
					"option": {
						"resizeType": "max_fit",
						"width": 100,
						"height": 100,
						"quality": 80,
						"shrinkLargerOnly": false,
						"upDownSizeType": "downOnly",
						"keepAnimation": false,
						"keepExif": true,
						"autoOrient": true
					}
				}
			]
		}
	],
	"queues": [
		{
			"queueId": "6691a01a-4585-4e26-989c-8ef25dd627a0",
			"queueType": "image",
			"status": "W",
			"tryCount": 0,
			"queuedAt": "2016-02-26T16:56:29+0900",
			"operationId": "100x100",
			"url": "http://alpha-image.toast.com/aaaaach/myfolder/banner/right_100x100.png",
			"name": "right_100x100.png",
			"path": "/myfolder/banner/right_100x100.png"
		}
	]
}
```

[Callback Sample]

```
// fail sample
{
	"header": {
		"isSuccessful": false,
		"resultCode": 500201,
		"resultMessage": "Operation fail. "
	},
	"id": "100",
	"operationId": "100x100",
	"sourceFile": {
		"url": "http://image.toast.com/aaaaach/myfolder/banner/left.png",
		"name": "left.png",
		"path": "/myfolder/banner/left.png"
	}
}

// success sample
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
	},
	"id": "100",
	"operationId": "slice-v",
	"sourceFile": {
		"url": "http://image.toast.com/aaaaach/myfolder/banner/vertical.png",
		"name": "vertical.png",
		"path": "/myfolder/banner/vertical.png"
	},
	"resultFiles": [
		{
			"url": "http://image.toast.com/aaaaach/myfolder/banner/vertical_slice-v_0000.png",
			"name": "vertical_slice-v_0000.png",
			"path": "/myfolder/banner/vertical_slice-v_0000.png"
		},
		{
			"url": "http://image.toast.com/aaaaach/myfolder/banner/vertical_slice-v_0001.png",
			"name": "vertical_slice-v_0001.png",
			"path": "/myfolder/banner/vertical_slice-v_0001.png"
		}
	]
}
```

### 사용자별 실 시간 서비스 조회

[Request Url]

|Method|Request Url|
|---|----|
|GET|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/users|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appkey|

[Request Example]

- 사용자의 실 시간 서비스를 조회합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X GET "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/users" \
-H "Authorization:{secretKey}"
```

[Response Field]

|이름|값|설명|
|---|---|----|
|user|object|사용자 정보|

[user Type]

|이름|값|설명|
|---|---|----|
|appKey|string|사용자 appkey|
|containerName|string|사용자의 컨테이너 정보|
|realtimeService|boolean|실 시간 서비스 제공 여부|

[Response Sample]

```
{
	"header": {
		// 생략
	},
	"user": {
		"appKey": {appkey},
		"containerName": "aaaaach",
		"realtimeService": true
	}
}
```

### 사용자별 실 시간 서비스 변경

[Request Url]

|Method|Request Url|
|---|----|
|PUT|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/users|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|
|Content–Type|application/json| -|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appkey|

[Request Body]

- json 객체로 전달합니다.

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|realtimeService|boolean|Y| -|실 시간 서비스 제공 여부|

[Request Example]

- 사용자의 실 시간 서비스를 변경합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X PUT "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/users" \
-H "Authorization:{secretKey}" \
-H "Content-Type: application/json" \
--data "{'realtimeService': false}"
```

[Response Sample]

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
	}
}
```

### 작업 조회

[Request Url]

|Method|Request Url|
|---|----|
|GET|https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/queues/{queueId}|

[Request Header]

|이름|값|설명|
|---|---|----|
|Authorization|{secretKey}|토스트 클라우드의 Web Console에서 생성한 secretKey|

[Request Parameters]

|이름|Type|필수 여부|기본값|설명|
|---|---|---|---|----|
|appkey|string|Y| -|토스트 클라우드 web console을 통해서 생성한 appkey|
|queueId|string|Y| -|조회 할 작업 고유 ID|

[Request Example]

- 오퍼레이션 요청에 대한 현재 상태를 조회합니다.
- {secretKey}와 {appKey}는 Web Console에서 확인한 값으로 변경합니다.

```
curl -X GET "https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/queues/6691a01a-4585-4e26-989c-8ef25dd627a0" \
-H "Authorization:{secretKey}"
```

[Response Field]

|이름|Type|설명|
|---|---|----|
|queue|object|작업 정보|

[queue Type]

|이름|Type|설명|
|---|---|----|
|queueId|string|작업 고유 ID|
|queueType|string|작업 구분 (image: 오퍼레이션, swift: 파일 및 폴더 삭제)|
|status|string|작업 상태 (W: 대기중, D: 완료, P: 작업중, F: 실패)|
|tryCount|integer|다시 시도 횟수|
|queuedAt|date|작업 시작일|
|operationId|string|참조되는 오퍼레이션 ID|
|url|string|서비스 될(되는) 이미지 서비스 URL|
|name|string|생성 될(된) 이미지 이름|
|path|string|생성 될(된) 이미지 절대 경로|

[Response Sample]

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "성공"
	},
	"queue": {
		"queueId": "6691a01a-4585-4e26-989c-8ef25dd627a0",
		"queueType": "image",
		"status": "D",
		"tryCount": 0,
		"queuedAt": "2016-02-26T16:56:52+0900",
		"operationId": "100x100",
		"url": "http://image.toast.com/aaaaach/myfolder/banner/right_100x100.png",
		"name": "right_100x100.png",
		"path": "/myfolder/banner/right_100x100.png"
	}
}
```

## Response Code

Response Code, Message

|resultCode|resultKey|resultMessage|
|---|---|----|
|0|SUCCESS|Success|
|1|PARTIAL_SUCCESS|Partial Success|
|2|UPLOAD_ALL_FAIL|Fail|
|-1|FAIL|Unknown error.|
|400|BAD_REQUEST|It could not understand the request due to invalid syntax.|
|401|UNAUTHORIZED|Unauthorized|
|403|NOT_ALLOWED|Not Allowed.|
|404|NOT_FOUND|Please check your API Url, HTTP Method.|
|415|UNSUPPORTED_MEDIA_TYPE|Please check your Media type. Only can be using 'application/json'|
|5000|INVALID_PARAM|'{}' is invalid.|
|5001|INVALID_PARAM_MORE_THAN|'{}' must be more than '{}'.|
|5002|INVALID_PARAM_BETWEEN_THAN|'{}' must be more than '{}' or less than '{}'.|
|5003|INVALID_PARAM_NOT_DEFINED|'{}' is not defined.|
|5004|INVALID_PARAM_IS_NOT_NULL|'{}' must be not null|
|5005|INVALID_PARAM_EXTENSION|it must be '{}'|
|5006|INVALID_PARAM_IS_SAME|the value of '{}' and '{}' must be differed.|
|5007|INVALID_PARAM_NOT_TOGETHER|'{}' can not use with '{}'.|
|10001|INVALID_PARAMETERS|parameter is invalid.|
|10002|INVALID_FILES|There is not exist upload file or it is an invalid file.|
|11001|PATH_LENGTH_LIMIT|The max byte size of full path is 1024.|
|11002|FILENAME_LENGTH_LIMIT|It is 2 ~ 255 the length of file and folder.|
|11003|FILE_COUNT_LIMIT|It is 10,000 the max count of file and folder you can request.|
|11004|UPLOAD_SIZE_LIMIT|It was exceed the max volume you can upload at once.|
|11050|INVALID_FILENAME|Invalid Characters in File or Folder Name|
|11051|INVALID_URL|It is 80,443 the upload port count of URL.|
|11060|FILENAME_EMPTY|You must register the name of file or folder.|
|20000|FOLDER_DUPLICATED_NAME|There is already a folder at the given destination. path: {}|
|20001|FOLDER_NOT_EXISTS|No file was found at the specified path.|
|20002|INVALID_BASE_PATH|It does not exist upper folder.|
|20010|FILE_DUPLICATED_NAME|There is same file name.|
|21002|MARKED_ALREADY_DELETING|It was file or folder requested to delete.|
|21003|DELETE_FILE_COUNT_LIMIT|It is 10,000 the max count of file and folder you are able to delete.|
|21011|NO_FILE_TO_DELETE|It does not exist the data to delete.|
|21030|FAIL_TO_DELETE|It occurred some problem while deleting.|
|22001|FAIL_TO_CREATE_FOLDER|It occurred some problem while creating folder.|
|23002|FAIL_TO_META_IMAGES|It could not extract the image attribute.|
|24001|FAIL_TO_SWIFT|The storage access was denied.|
|24002|FAIL_TO_SWIFT_DELETE_OBJECT|It occurred some proble while storage deleting|
|24003|FAIL_TO_SWIFT_CREATE_CONTAINER|Failed to create new swift container.|
|24004|FAIL_TO_SWIFT_SET_PRIVATE|Failed to set private swift container.|
|30004|INVALID_APPKEY|The authentication is invalid.|
|30005|INVALID_USER|The user is invalid.|
|40000|OPERATION_NOT_ALLOWED|You have no operation authority.|
|40001|OPERATION_FAIL|Failed to process the image operation.|
|40002|OPERATION_DUPLICATED_NAME|There is same name of Image Operation.|
|40003|OPERATION_NO_PROCESSING_OPTION|There are not any option for image processing.|
|40004|OPERATION_PARSING_FAIL|Failed to parsing the image operation.|
|50001|DUPLICATED_APPKEY|Failed to enable given appKey, since the appKey is already exist.|
|50002|FAIL_TO_ISSUE_NEW_MEMBERKEY|Failed to issue new memberKey.|
|50003|FAIL_TO_ENABLE_USER_INTERNAL_ERROR|Failed to enable appKey due to inter server error.|
|50004|SHORTAGE_MEMBERKEY|Available member key is shortage.|
|50011|PENDING_DELETE_ALL_JOB|Pending deleting all image files.|
|50012|FAIL_TO_EXIST_IMAGE_FILE|Remains image files. Please proceed delete all image files.|
|50013|FAIL_TO_DISABLE_USER_INTERNAL_ERROR|Failed to disable appKey due to inter server error.|
