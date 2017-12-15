## Contents > Image > Release Notes

### 2017.12.21
#### 기능 개선/변경
* [Console] 화면 UI 디자인이 변경되었습니다.

### 2017.11.30
#### 기능 추가
* 처리 결과 Callback 기능 추가
	* API 호출 시 callbackUrl을 파라미터로 전달하면 처리 결과를 callbackUrl로 전송해주는 기능이 추가되었습니다.
		* [다중 이미지 업로드](./Developer`s Guide/#_6)
		* [이미지 오퍼레이션 실행](./Developer`s Guide/#_14)

#### 버그 수정
 * [Console] 압축 파일이 포함된 폴더 업로드 시 하위 폴더 경로가 잘못 생성되던 버그가 수정되었습니다.

### 2017.11.23
#### 기능 개선/변경
* [이미지 처리 기능 추가](./Developer`s Guide/#_10)
	* 이미지 분할 기능이 기존 가로, 세로 두 가지에서 격자 분할이 추가되었습니다.
	* 워터마크 기능이 추가되었습니다.
* [[Console] 이미지 처리 옵선 추가](./Getting Started/#_8)
	* 기존 Resize에서만 설정 가능했던 옵션을 공통 옵션으로 설정 가능하도록 수정하였습니다.
		* 품질, 이미지 포맷, 결과 콜백 URL, 메타정보 유지 여부, Orientation 정보를 기준으로 회전 여부
	* GIF 애니메이션 유지 옵션의 default 값 변경 : 유지하지 않음 에서 유지 함 으로 변경되었습니다.
* [[Console] 이미지 처리 기능에 따라 Grouping](./Getting Started/#_8)
	* Group 1 기본 처리 : Resize, Gray, Rectangle Crop
	* Group 2 분할 처리 : Slice Crop (가로, 세로, 격자)
	* Group 3 합성 처리 : Circle Crop
	* 이미지 처리는 Group 순서대로 처리가 됩니다.
* [[Console] 상품 종료 프로세스 변경](./Getting Started/#_9)
	* 상품 이용 종료 시 남아있는 파일이 있는 경우 이용 종료가 불가능 합니다.
	* 전체 파일 삭제 기능이 추가되었고, 전체 삭제 후 상품 이용 종료가 가능합니다.

### 2017.05.25
#### 버그 수정
* Image Meta 정보 파싱 에러 수정

### 2017.04.20
#### 기능 개선/변경
* [썸네일 크기조절 방식 추가](./Developer`s Guide/#_10)
    * Option의 가로 세로에 맞게 Size 변경
    * Option의 가로 기준 Size 변경
    * Option의 세로 기준 Size 변경
* [크롭 방식 추가](./Developer`s Guide/#_10)
    * Slice Crop 추가
* [[Console] Gif 애니메이션 유지 옵션 글로벌 설정으로 변경](./Developer`s Guide/#_10)

#### 버그 수정
* 물결(~) 문자가 포함된 폴더가 생성되지 않도록 수정

### 2017.03.23
#### 버그 수정
* [Console] macOS에서 업로드한 한글 파일 검색 불가 이슈 수정
