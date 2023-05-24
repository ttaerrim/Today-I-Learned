# fetch vs axios

## error handling

| fetch                                          | axios                            |
| ---------------------------------------------- | -------------------------------- |
| 네트워크 장애가 발생한 경우에만 Promise reject | 200번대 이상 코드 Promise reject |
