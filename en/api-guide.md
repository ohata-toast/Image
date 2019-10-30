## Content Delivery > Image > API 가이드

Image 서비스의 API를 설명합니다.


## API 공통 정보

### 사전 준비

- API 사용을 위해서는 앱 키와 보안 키가 필요합니다.
- 앱 키와 보안 키는 콘솔 상단 "URL & Appkey" 메뉴에서 확인이 가능합니다.

### 요청 공통 정보

- API를 사용하기 위해서는 보안 키 인증 처리가 필요합니다.
- 모든 API 요청 헤더에 'Authorization'에 보안 키를 넣어서 요청해야 합니다.

[요청 헤더]

| 이름 | 값 | 설명 |
|---|---|---|
| Authorization | {secretKey} | 콘솔에서 발급받은 보안 키 |

### 응답 공통 정보

- 모든 API 요청에 "200 OK"로 응답합니다. 자세한 응답 결과는 응답 본문의 헤더를 참고합니다.

[성공 응답 본문]

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
	}
}
```

[실패 응답 본문]

```
{
	"header": {
		"isSuccessful": false,
		"resultCode": 404,
		"resultMessage": "Please check your API Url, HTTP Method."
	}
}
```


## 폴더 API

### 폴더 생성

- 지정된 경로에 폴더를 생성합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/folders |

[요청 본문]

- myfolder라는 이름의 폴더를 루트 폴더 하위에 생성합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X POST 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/folders' \
-H 'Authorization: {secretKey}' \
-H 'Content-Type: application/json' \
--data '{"path": "/myfolder"}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| path | String | 최소 2글자, 최대 255Byte | 필수 |  | 생성할 폴더의 절대 경로, 상위 폴더 자동 생성 |

#### 응답

[응답 본문]

```
{
	"header": {
		// 생략
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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| folder | Object | 폴더 정보 |
| folder.isFolder | boolean | 폴더 여부 |
| folder.id | String | 고유 ID |
| folder.name | String | 폴더 이름 |
| folder.path | String | 폴더 절대 경로 |
| folder.updatedAt | DateTime | 최종 수정일 |


### 폴더 내 파일 목록 조회

- 지정된 경로 하위의 목록을 조회하거나 이름에 특정 문자를 포함한 목록을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/folders |

[요청 본문]

- /myfolder 하위의 폴더와 파일을 조회합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/folders?basepath=/myfolder' \
-H 'Authorization: {secretKey}'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| basepath | String | 최소 2글자, 최대 255Byte | 필수 |  | 조회할 폴더의 전체 경로 |
| createdBy | String | 1글자 | 선택 |  | 목록 조회 대상 <br>(공백: 전체, <br>U: 사용자 업로드 이미지, <br>P: 오퍼레이션 이미지) |
| name | String | 최소 2글자, 최대 255Byte | 선택 |  | 검색할 이미지 이름 |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| rows | int | 최소 1, 최대 10,000 | 선택 | 100 | 조회 개수 |
| sort | String | | 선택 | name:asc | 정렬 방식 (정렬대상 : name or date, 정렬방식 : asc or desc) |

#### 응답

[응답 본문]

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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| paging | Object | 페이징 정보 |
| paging.page | int | 요청 페이지 번호 |
| paging.rows | int | 요청 조회 개수 |
| paging.totalFolderCount | long | 전체 폴더 개수 |
| paging.totalFileCount | long | 전체 폴더 개수 |
| folders | List | 폴더 목록 |
| folders[0].isFolder | boolean | 폴더 여부 |
| folders[0].id | String | 고유 ID |
| folders[0].name | String | 폴더 이름 |
| folders[0].path | String | 폴더 절대 경로 |
| folders[0].updatedAt | DateTime | 최종 수정일 |
| files | List | 이미지 파일 목록 |
| files[0].isFolder | boolean | 폴더 여부 |
| files[0].id | String | 고유 ID |
| files[0].url | String | 이미지 서비스 Url |
| files[0].name | String | 이미지 이름 |
| files[0].path | String | 이이미지 절대 경로 |
| files[0].bytes | long | 이미지 파일 크기 |
| files[0].createdBy | String | 이미지 구분 (U: 사용자 업로드 이미지, P: 오퍼레이션 이미지) |
| files[0].updatedAt | DateTime | 최종 수정일 |
| files[0].operationId | String | createdBy === P 의 경우 참조 된 오퍼레이션 ID |
| files[0].queues | List | 작업 정보 목록 (해당 API에서는 사용되지 않음) |
| files[0].imageProperty | Object | 이미지 속성 |
| files[0].imageProperty.width | int | 가로 크기 |
| files[0].imageProperty.height | int | 세로 크기 |
| files[0].imageProperty.coordinate | Object | GPS 정보 |
| files[0].imageProperty.createdAt | DateTime | 촬영일 또는 생성일 |
| files[0].imageProperty.coordinate.lat | double | 위도 |
| files[0].imageProperty.coordinate.lng | double | 경도 |


### 폴더 속성 조회

- 폴더의 ID, 용량, 파일 개수 등의 속성을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/properties |

[요청 본문]

- myfolder의 폴더의 속성을 조회합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/properties?path=/myfolder' \
-H 'Authorization: {secretKey}'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| path | String | 최소 2글자, 최대 255Byte | 필수 |  | 조회할 폴더의 절대 경로 |

#### 응답

[응답 본문]

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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| folder | Object | 폴더 정보 |
| folder.isFolder | boolean | 폴더 여부 |
| folder.id | String | 고유 ID |
| folder.name | String | 폴더 이름 |
| folder.path | String | 폴더 절대 경로 |
| folder.bytes | long | 폴더 크기 (byte) |
| folder.totalFolderCount | long | 하위 폴더 개수 |
| folder.totalFileCount | long | 하위 파일 개수 |
| folder.updatedAt | DateTime | 최종 수정일 |



## 업로드 API

### 단일 파일 업로드

- 이미지 파일 한 개를 업로드 합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/images |

[요청 본문]

- /myfolder 폴더에 sample.png 이미지를 업로드 합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.
- 이미지 파일의 Binary Data를 넣습니다.

```
curl -X PUT 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/images?path=/myfolder/sample.png&overwrite=true' \
-H 'Authorization: {secretKey}' \
-H 'Content-Type:application/octet-stream' \
--data-binary 'path/to/imageFile/@sample.png'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| path | String | 최소 2글자, 최대 255Byte | 필수 |  | 생성할 절대 경로의 파일명 |
| overwrite | boolean |  | 선택 | false | 같은 이름이 있을 경우 덮어쓰기 여부 |
| autorename | boolean |  | 선택 | false | 같은 이름이 있을 경우 <br>"이름(1).확장자" 형식으로 파일명 변경 여부 |
| operationIds | String List |  | 선택 |  | 이미지 오퍼레이션 ID 리스트 (콤마로 구분됨) |

- 이미지 오퍼레이션 ID를 추가해서 요청할 경우, 업로드 시 원하는 옵션으로 오퍼레이션 파일을 생성할 수 있습니다.
- [이미지 오퍼레이션 API](./api-guide/#api_4)를 참고합니다.

#### 응답

[응답 본문]

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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| file | Object | 이미지 파일 정보 |
| file.isFolder | boolean | 폴더 여부 |
| file.id | String | 고유 ID |
| file.url | String | 이미지 서비스 Url |
| file.name | String | 이미지 이름 |
| file.path | String | 이미지 절대 경로 |
| file.bytes | long | 이미지 파일 크기 |
| file.createdBy | String | 이미지 구분 (U: 사용자 업로드 이미지, P: 오퍼레이션 이미지) |
| file.updatedAt | DateTime | 최종 수정일 |
| file.operationId | String | createdBy === P 의 경우 참조 된 오퍼레이션 ID |
| file.imageProperty | Object | 이미지 속성 |
| file.imageProperty.width | int | 가로 크기 |
| file.imageProperty.height | int | 세로 크기 |
| file.imageProperty.createdAt | DateTime | 촬영일 또는 생성일 |
| file.imageProperty.coordinate | Object | GPS 정보 |
| file.imageProperty.coordinate.lat | double | 위도 |
| file.imageProperty.coordinate.lng | double | 경도 |
| file.queues | List | operationIds 요청에 의한 작업 정보 목록 |
| file.queues[0].queueId | String | 작업 고유 ID |
| file.queues[0].queueType | String | 작업 구분 (image: 오퍼레이션, delete: 파일 및 폴더 삭제) |
| file.queues[0].status | String | 작업 상태 (W: 대기중, D: 완료, P: 작업중, F: 실패) |
| file.queues[0].tryCount | int | 다시 시도 횟수 |
| file.queues[0].queuedAt | DateTime | 작업 등록일 |
| file.queues[0].operationId | String | 참조되는 오퍼레이션 ID |
| file.queues[0].url | String | 서비스 될 이미지 서비스 URL |
| file.queues[0].name | String | 생성 될 이미지 이름 |
| file.queues[0].path | String | 생성 될 이미지 절대 경로 |


### 다중 파일 업로드

- 여러개의 이미지 파일을 업로드 합니다.
- 압축 파일 업로드도 가능합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/images |

[요청 본문]

- /myfolder/banner 폴더에 left.png, right.png 이미지를 업로드 합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.
- multipart/form–data 형식으로 전달합니다.

```
curl -X POST 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/images' \
-H 'Authorization: {secretKey}' \
-F 'params={"basepath": "/myfolder/banner", "overwrite": true, "operationIds":["100x100"]}' \
-F 'files=@left.png' \
-F 'files=@right.png'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| files | multipart/form–data |  | 필수 |  | 이미지 파일 리스트 |
| params | String | json 형태의 문자열 | 필수 |  | 업로드 옵션 |
| params.basepath | String | 최소 2글자, 최대 255Byte | 필수 |  | 업로드할 절대 경로 |
| params.overwrite | boolean |  | 선택 | false | 같은 이름이 있을 경우 덮어쓰기 여부 |
| params.autorename | boolean |  | 선택 | false | 같은 이름이 있을 경우 <br>"이름(1).확장자" 형식으로 파일명 변경 여부 |
| params.operationIds | String List |  | 선택 |  | 이미지 오퍼레이션 ID 리스트. <br>업로드 시 원하는 옵션으로 오퍼레이션 파일을 생성. <br>이미지 오퍼레이션 관련 API 참고 |
| params.callbackUrl | String |  | 선택 |  | 처리 결과를 통보받을 콜백 Url 경로. <br>query string 형식으로 id를 적으면 콜백 전송 시 같이 전달됨. <br>포트는 80, 443만 지원 |

#### 응답

[응답 본문]

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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| errors | List | 업로드 실패 목록 |
| errors[0].path | String | 파일 절대 경로 |
| errors[0].bytes | long | 파일 크기 |
| errors[0].error | Object | 에러 정보 |
| errors[0].error.resultCode | int | 에러 코드 |
| errors[0].error.resultMessage | String | 에러 메시지 |
| successes | List | 업로드 성공 목록 |
| successes[0].isFolder | boolean | 폴더 여부 |
| successes[0].id | String | 고유 ID |
| successes[0].url | String | 이미지 서비스 URL |
| successes[0].name | String | 이미지 이름 |
| successes[0].path | String | 이미지 절대 경로 |
| successes[0].bytes | long | 이미지 파일 크기 |
| successes[0].createdBy | String | 이미지 구분 (U: 사용자 업로드 이미지, P: 오퍼레이션 이미지) |
| successes[0].updatedAt | DateTime | 최종 수정일 |
| successes[0].operationId | String | createdBy === P 의 경우 참조 된 오퍼레이션 ID |
| successes[0].imageProperty | Object | 이미지 속성 |
| successes[0].imageProperty.width | int | 가로 크기 |
| successes[0].imageProperty.height | int | 세로 크기 |
| successes[0].imageProperty.createdAt | DateTime | 촬영일 또는 생성일 |
| successes[0].imageProperty.coordinate | Object | GPS 정보 |
| successes[0].imageProperty.coordinate.lat | double | 위도 |
| successes[0].imageProperty.coordinate.lng | double | 경도 |
| successes[0].queues | List | operationIds 요청에 의한 작업 정보 목록 |
| successes[0].queues[0].queueId | String | 작업 고유 ID |
| successes[0].queues[0].queueType | String | 작업 구분 (image: 오퍼레이션, delete: 파일 및 폴더 삭제) |
| successes[0].queues[0].status | String | 작업 상태 (W: 대기중, D: 완료, P: 작업중, F: 실패) |
| successes[0].queues[0].tryCount | int | 다시 시도 횟수 |
| successes[0].queues[0].queuedAt | DateTime | 작업 등록일 |
| successes[0].queues[0].operationId | String | 참조되는 오퍼레이션 ID |
| successes[0].queues[0].url | String | 서비스 될 이미지 서비스 Url |
| successes[0].queues[0].name | String | 생성 될 이미지 이름 |
| successes[0].queues[0].path | String | 생성 될 이미지 절대 경로 |

[요청 결과 콜백 본문]

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
			"overwritten": true,
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



## 삭제 API

### 단일 삭제 (동기)

- 폴더 또는 파일 한 개를 삭제합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/images/sync |

[요청 본문]

- /myfolder/sample.png의 파일을 삭제합니다.
- /myfolder/sample.png의 ID는 우측 메뉴의 "폴더 내 파일 목록 조회" API를 통해서 알 수 있습니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X DELETE 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/images/sync?
fileId=9cf11176-045c-4708-8dbd-35633f029a91' \
-H 'Authorization: {secretKey}'
```

[필드]

- "folderId" 또는 "fileId"는 최소 하나를 필수로 사용해야 합니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| folderId | String | 최대 50글자 |  |  | 삭제할 폴더의 ID |
| fileId | String | 최대 50글자 |  |  | 삭제할 파일의 ID |
| includeThumbnail | boolean |  | 선택 | false | 삭제할 파일에 의해 생성된 오퍼레이션 파일도 삭제 |

#### 응답

[응답 본문]

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

- 여러 개의 폴더와 파일을 삭제합니다.
- 실제 데이터 삭제는 비동기로 처리됩니다.
- 처리 결과는 응답으로 전달 받은 "queueId"로 [작업 조회 API](./api-guide/#api_6)를 통해 확인할 수 있습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/images/async |

[요청 본문]

- /myfolder/banner/left.png, /myfolder/banner/right.png의 파일을 삭제합니다.
- 파일 및 폴더 ID는 [폴더 내 파일 목록 조회](./api-guide/#_7)를 통해서 알 수 있습니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X DELETE 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/images/async?
fileIds=5fa8ce52-d066-490c-85dd-f8cef181dd28,96f726bd-93e4-4f7c-ad55-56e85aa323a8' \
-H 'Authorization: {secretKey}'
```

[필드]

- "folderIds" 또는 "fileIds"는 최소 하나 필수 파라미터로 사용해야 합니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| folderIds | String | ID 하나 당 최대 50글자 |  |  | 삭제할 폴더의 ID 리스트 (콤마로 구분됨) |
| fileIds | String | ID 하나 당 최대 50글자 |  |  | 삭제할 파일의 ID 리스트 (콤마로 구분됨) |
| includeThumbnail | boolean |  | 선택 | false | 삭제할 파일에 의해 생성된 오퍼레이션 파일도 삭제 |

#### 응답

[응답 본문]

```
{
	"header": {
		// 생략
	},
	"queue": {
		"tryCount": 0,
		"queueId": "f1d27571-84e1-4892-9261-7d46755d45cc",
		"queueType": "delete",
		"status": "W",
		"queuedAt": "2018-01-12T10:44:44+0900",
		"operationId": "",
		"url": "",
		"name": "",
		"path": ""
	}
}
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| queue | Object | 작업 정보 |
| queue.queueId | String | 작업 고유 ID |
| queue.queueType | String | 작업 구분 (delete: 파일 및 폴더 삭제) |
| queue.status | String | 작업 상태 (W: 대기중, D: 완료, P: 작업중, F: 실패) |
| queue.tryCount | int | 다시 시도 횟수 |
| queue.queuedAt | DateTime | 작업 등록일 |
| queue.operationId | String | 참조되는 오퍼레이션 ID |
| queue.url | String | 서비스 될 이미지 서비스 URL |
| queue.name | String | 생성 될 이미지 이름 |
| queue.path | String | 생성 될 이미지 절대 경로 |


## 이미지 오퍼레이션 API

- 이미지 오퍼레이션 API를 통해 다양한 썸네일을 생성할 수 있습니다.
- 썸네일 크기, 흑백 필터, 크롭(Rectangle, Circle, Slice), 워터마크 제공

### 이미지 오퍼레이션 생성 및 수정

- 이미지 처리를 위한 오퍼레이션을 생성 또는 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/operations/{operationId} |

[요청 본문]

- 이미지의 가로 세로 중 긴 축 길이를 기준으로 사이즈를 100x100으로 줄이는 작업을 100x100이라는 이름으로 생성 또는 수정합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X PUT 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/operations/100x100' \
-H 'Authorization: {secretKey}' \
-H 'Content-Type: application/json' \
--data '{"description": "", "realtimeService": true, "data": [{"templateOperationId": "resize_max_fit",
"option": {"resizeType": "max_fit", "width": 100, "height": 100, "quality": 80,
"upDownSizeType": "downOnly"}}]}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| operationId | String | 최소 1글자, 최대 20글자, <br>영문 또는 숫자 | 필수 |  | 생성 및 수정할 오퍼레이션 이름 |
| description | String | 최대 30글자 | 선택 |  | 오퍼레이션 설명 |
| realtimeService | boolean |  | 선택 | true | 실시간 서비스 제공 여부 |
| deleteThumbnail | boolean |  | 선택 | false | 기존에 해당 오퍼레이션으로 생성된 썸네일을 삭제할지 여부 |
| data | List |  | 선택 |  | 오퍼레이션 작업 목록 |

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
		"watermarkImagePath": String 	// (Required) 합성할 이미지 파일의 경로
	}
}
```


#### 응답

[응답 본문]

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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| operation | Object | 오퍼레이션 정보 |
| operation.appKey | String | 사용자 앱 키 |
| operation.operationId | String | 오퍼레이션 이름 |
| operation.description | String | 오퍼레이션 설명 |
| operation.realtimeService | boolean | 실시간 서비스 제공 여부 |
| operation.updatedAt | DateTime | 최종 수정일 |
| operation.jobTemplate | List | 오퍼레이션 작업 목록 |
| operation.jobTemplate[0].templateOperationId | String | 기반이 되는 템플릿 ID |
| operation.jobTemplate[0].jobType | String | 오퍼레이션 작업 타입 |
| operation.jobTemplate[0].option | Object | 오퍼레이션 작업 내용 |

### 이미지 오퍼레이션 목록 조회

- 이미지 오퍼레이션 목록을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/operations |

[요청 본문]

- 사용자의 오퍼레이션 목록을 조회합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/operations' \
-H 'Authorization: {secretKey}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| name | String | 최대 20글자, <br>영문 또는 숫자 | 선택 |  | 검색할 오퍼레이션 이름 (입력 값으로 시작하는) |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| rows | int | 최대 10,000 | 선택 | 20 | 조회 개수 |
| sort | String |  | 선택 | date:desc | 정렬 방식 (정렬대상 : name or date, 정렬방식 : asc or desc) |
| template | boolean |  | 선택 | false | 목록 조회 대상 (true: 기본 오퍼레이션, false: 사용자 생성 오퍼레이션) |

#### 응답

[응답 본문]

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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| paging | Object | 페이징 정보 |
| paging.page | int | 요청 페이지 번호 |
| paging.rows | int | 요청 조회 개수 |
| paging.totalCount | long | 전체 개수 |
| operations | List | 오퍼레이션 목록 |
| operations[0].appKey | String | 사용자 앱 키 |
| operations[0].operationId | String | 오퍼레이션 이름 |
| operations[0].description | String | 오퍼레이션 설명 |
| operations[0].realtimeService | boolean | 실시간 서비스 사용 여부 |
| operations[0].updatedAt | DateTime | 최종 수정일 |
| operations[0].jobTemplate | List | 오퍼레이션 작업 목록 |
| operations[0].jobTemplate[0].templateOperationId | String | 기반이 되는 템플릿 ID |
| operations[0].jobTemplate[0].jobType | String | 오퍼레이션 작업 타입 |
| operations[0].jobTemplate[0].option | Object | 오퍼레이션 작업 내용 |

### 이미지 오퍼레이션 상세 조회

- 특정 이미지 오퍼레이션 상세 내용을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/operations/{operationId} |

[요청 본문]

- 100x100 오퍼레이션을 조회합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/operations/100x100' \
-H 'Authorization: {secretKey}'
```

#### 응답

[응답 본문]

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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| operation | Object | 오퍼레이션 정보 |
| operation.appKey | String | 사용자 앱 키 |
| operation.operationId | String | 오퍼레이션 이름 |
| operation.description | String | 오퍼레이션 설명 |
| operation.realtimeService | boolean | 실시간 서비스 제공 여부 |
| operation.updatedAt | DateTime | 최종 수정일 |
| operation.jobTemplate | List | 오퍼레이션 작업 목록 |
| operation.jobTemplate[0].templateOperationId | String | 기반이 되는 템플릿 ID |
| operation.jobTemplate[0].jobType | String | 오퍼레이션 작업 타입 |
| operation.jobTemplate[0].option | Object | 오퍼레이션 작업 내용 |

### 이미지 오퍼레이션 삭제

- 특정 이미지 오퍼레이션을 삭제합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/operations/{operationId} |

[요청 본문]

- 100x100 오퍼레이션을 삭제합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X DELETE 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/operations/100x100' \
-H 'Authorization: {secretKey}'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| deleteThumbnail | boolean |  | 선택 | false | 기존에 해당 오퍼레이션으로 생성된 썸네일을 삭제할지 여부 |

#### 응답

[응답 본문]

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
	}
}
```

### 이미지 오퍼레이션 실행 (비동기)

- 지정된 파일에 오퍼레이션을 실행하여 썸네일을 생성합니다.
- 처리 결과는 응답으로 전달 받은 "queueId"로 [작업 조회 API](./api-guide/#api_6)를 통해 확인할 수 있습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/operations-exec |

[요청 본문]

- /myfolder/left.png, /myfolder/right.png 원본 파일로 100x100 오퍼레이션 옵션이 적용된 파일을 생성합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X POST 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/operations-exec' \
-H 'Authorization: {secretKey}' \
-H 'Content-Type: application/json' \
--data '{"basepath": "/myfolder", "operationIds": ["100x100"],
"filepaths": ["/myfolder/left.png", "/myfolder/right.jpg"]}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| basepath | String | 최소 2글자, 최대 255Byte | 필수 |  | 기준이 되는 폴더의 절대 경로 |
| filepaths | String List |  | 필수 |  | 실행할 절대 경로의 폴더 및 파일 리스트 |
| operationIds | String List |  | 필수 |  | 실행할 오퍼레이션 ID 리스트 |
| callbackUrl | String |  | 선택 |  | 처리 결과를 통보받을 URL 경로. <br>query string 형식으로 id를 적으면 callback 전송 시 같이 전달됨. <br>포트는 80, 443만 지원 |

#### 응답

[응답 본문]

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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| operations | List | 실행 오퍼레이션 목록 |
| operations[0].appKey | String | 사용자 앱 키 |
| operations[0].operationId | String | 오퍼레이션 이름 |
| operations[0].description | String | 오퍼레이션 설명 |
| operations[0].realtimeService | boolean | 실시간 서비스 제공 여부 |
| operations[0].updatedAt | DateTime | 최종 수정일 |
| operations[0].jobTemplate | List | 오퍼레이션 작업 목록 |
| operations[0].jobTemplate[0].templateOperationId | String | 기반이 되는 템플릿 ID |
| operations[0].jobTemplate[0].jobType | String | 작업 구분 |
| operations[0].jobTemplate[0].option | Object | 작업 내용 |
| queues | List | 작업 정보 목록 |
| queues[0].queueId | String | 작업 고유 ID |
| queues[0].queueType | String | 작업 구분 (image: 오퍼레이션, delete: 파일 및 폴더 삭제) |
| queues[0].status | String | 작업 상태 (W: 대기중, D: 완료, P: 작업중, F: 실패) |
| queues[0].tryCount | int | 다시 시도 횟수 |
| queues[0].queuedAt | DateTime | 작업 등록일 |
| queues[0].operationId | String | 참조되는 오퍼레이션 ID |
| queues[0].url | String | 서비스 될 이미지 서비스 URL |
| queues[0].name | String | 생성 될 이미지 이름 |
| queues[0].path | String | 생성 될 이미지 절대 경로 |

[요청 결과 콜백 본문]

```
// fail sample
{
	"header": {
		"isSuccessful": false,
		"resultCode": 500201,
		"resultMessage": "Operation fail. "
	},
	"id": "100",
	"overwritten": false,
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
	"overwritten": true,
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


## 실시간 서비스 API

### 실시간 서비스 조회

- 사용자의 이미지 오퍼레이션 실시간 서비스 사용 여부를 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/users |

[요청 본문]

- 사용자의 실시간 서비스를 조회합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/users' \
-H 'Authorization: {secretKey}'
```

#### 응답

[응답 본문]

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

[필드]

| 이름 | 값 | 설명 |
|---|---|---|
| user | Object | 사용자 정보 |
| user.appKey | String | 사용자 앱 키 |
| user.containerName | String | 사용자의 컨테이너 정보 |
| user.realtimeService | boolean | 실시간 서비스 제공 여부 |


### 실시간 서비스 변경

- 사용자의 이미지 오퍼레이션 실시간 서비스 사용 여부를 변경합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/users |

[요청 본문]

- 사용자의 실시간 서비스를 변경합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X PUT 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/users' \
-H 'Authorization: {secretKey}' \
-H 'Content-Type: application/json' \
--data '{"realtimeService": false}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| realtimeService | boolean |  | 필수 |  | 실시간 서비스 제공 여부 |

#### 응답

[응답 본문]

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
	}
}
```


## 작업 API

### 작업 조회

- 이미지 오퍼레이션 처리 또는 삭제 작업을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://api-image.cloud.toast.com/image/v2.0/appkeys/{appkey}/queues/{queueId} |

[요청 본문]

- 오퍼레이션 요청에 대한 현재 상태를 조회합니다.
- {appKey}와 {secretKey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://api-image.cloud.toast.com/image/v2.0/appkeys/{appKey}/queues/6691a01a-4585-4e26-989c-8ef25dd627a0' \
-H 'Authorization: {secretKey}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| queueId | String | 최대 64글자 | 필수 |  | 조회할 작업 고유 ID |

#### 응답

[응답 본문]

```
{
	"header": {
		"isSuccessful": true,
		"resultCode": 0,
		"resultMessage": "Success"
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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| queue | Object | 작업 정보 |
| queue.queueId | String | 작업 고유 ID |
| queue.queueType | String | 작업 구분 (image: 오퍼레이션, delete: 파일 및 폴더 삭제) |
| queue.status | String | 작업 상태 (W: 대기중, D: 완료, P: 작업중, F: 실패) |
| queue.tryCount | int | 다시 시도 횟수 |
| queue.queuedAt | DateTime | 작업 시작일 |
| queue.operationId | String | 참조되는 오퍼레이션 ID |
| queue.url | String | 서비스 될(되는) 이미지 서비스 URL |
| queue.name | String | 생성 될(된) 이미지 이름 |
| queue.path | String | 생성 될(된) 이미지 절대 경로 |
