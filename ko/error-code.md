## Content Delivery > Image > 오류 코드

| resultCode | resultKey | resultMessage |
|---|---|----|
| 0 | SUCCESS | Success |
| 1 | PARTIAL_SUCCESS | Partial Success |
| 2 | UPLOAD_ALL_FAIL | Fail |
| -1 | FAIL | Unknown error. |
| 400 | BAD_REQUEST | It could not understand the request due to invalid syntax. |
| 401 | UNAUTHORIZED | Unauthorized |
| 403 | NOT_ALLOWED | Not Allowed. |
| 404 | NOT_FOUND | Please check your API Url, HTTP Method. |
| 415 | UNSUPPORTED_MEDIA_TYPE | Please check your Media type. Only can be using 'application/json' |
| 5000 | INVALID_PARAM | '{}' is invalid. |
| 5001 | INVALID_PARAM_MORE_THAN | '{}' must be more than '{}'. |
| 5002 | INVALID_PARAM_BETWEEN_THAN | '{}' must be more than '{}' or less than '{}'. |
| 5003 | INVALID_PARAM_NOT_DEFINED | '{}' is not defined. |
| 5004 | INVALID_PARAM_IS_NOT_NULL | '{}' must be not null |
| 5005 | INVALID_PARAM_EXTENSION | it must be '{}' |
| 5006 | INVALID_PARAM_IS_SAME | the value of '{}' and '{}' must be differed. |
| 5007 | INVALID_PARAM_NOT_TOGETHER | '{}' can not use with '{}'. |
| 10001 | INVALID_PARAMETERS | parameter is invalid. |
| 10002 | INVALID_FILES | There is not exist upload file or it is an invalid file. |
| 11001 | PATH_LENGTH_LIMIT | The max byte size of full path is 1024. |
| 11002 | FILENAME_LENGTH_LIMIT | It is 2 ~ 255 the length of file and folder. |
| 11003 | FILE_COUNT_LIMIT | It is 10,000 the max count of file and folder you can request. |
| 11004 | UPLOAD_SIZE_LIMIT | It was exceed the max volume you can upload at once. |
| 11050 | INVALID_FILENAME | Invalid Characters in File or Folder Name |
| 11051 | INVALID_URL | It is 80,443 the upload port count of URL. |
| 11060 | FILENAME_EMPTY | You must register the name of file or folder. |
| 20000 | FOLDER_DUPLICATED_NAME | There is already a folder at the given destination. path: {} |
| 20001 | FOLDER_NOT_EXISTS | No file was found at the specified path. |
| 20002 | INVALID_BASE_PATH | It does not exist upper folder. |
| 20010 | FILE_DUPLICATED_NAME | There is same file name. |
| 21002 | MARKED_ALREADY_DELETING | It was file or folder requested to delete. |
| 21003 | DELETE_FILE_COUNT_LIMIT | It is 10,000 the max count of file and folder you are able to delete. |
| 21011 | NO_FILE_TO_DELETE | It does not exist the data to delete. |
| 21030 | FAIL_TO_DELETE | It occurred some problem while deleting. |
| 22001 | FAIL_TO_CREATE_FOLDER | It occurred some problem while creating folder. |
| 23002 | FAIL_TO_META_IMAGES | It could not extract the image attribute. |
| 24001 | FAIL_TO_SWIFT | The storage access was denied. |
| 24002 | FAIL_TO_SWIFT_DELETE_OBJECT | It occurred some proble while storage deleting |
| 24003 | FAIL_TO_SWIFT_CREATE_CONTAINER | Failed to create new swift container. |
| 24004 | FAIL_TO_SWIFT_SET_PRIVATE | Failed to set private swift container. |
| 30004 | INVALID_APPKEY | The authentication is invalid. |
| 30005 | INVALID_USER | The user is invalid. |
| 40000 | OPERATION_NOT_ALLOWED | You have no operation authority. |
| 40001 | OPERATION_FAIL | Failed to process the image operation. |
| 40002 | OPERATION_DUPLICATED_NAME | There is same name of Image Operation. |
| 40003 | OPERATION_NO_PROCESSING_OPTION | There are not any option for image processing. |
| 40004 | OPERATION_PARSING_FAIL | Failed to parsing the image operation. |
| 50001 | DUPLICATED_APPKEY | Failed to enable given appKey, since the appKey is already exist. |
| 50002 | FAIL_TO_ISSUE_NEW_MEMBERKEY | Failed to issue new memberKey. |
| 50003 | FAIL_TO_ENABLE_USER_INTERNAL_ERROR | Failed to enable appKey due to inter server error. |
| 50004 | SHORTAGE_MEMBERKEY | Available member key is shortage. |
| 50011 | PENDING_DELETE_ALL_JOB | Pending deleting all image files. |
| 50012 | FAIL_TO_EXIST_IMAGE_FILE | Remains image files. Please proceed delete all image files. |
| 50013 | FAIL_TO_DISABLE_USER_INTERNAL_ERROR | Failed to disable appKey due to inter server error. |
