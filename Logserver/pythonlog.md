## Python -> Remote Logging Server


# 로그 설정 1
```python

import logging, logging.handlers
```
라이브러리 임포트

```python

logger = logging.getLoger()

```
logger 객체 참조 

```python
logger.setLevel(logging.ERROR)#CRITICAL,ERROR,WARNING,INFO,DEBUG

```
로그를 찍을 수준 선택 
-> 공간확보를 위해 ERROR 수준으로 통일 or INFO선택시 꼭 필요한 로그만

```python

formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

```
logger formatter 설정 (통일 해야함)


```python

handler = logging.handlers.SysLogHandler(address = ('172.47.20.200', 514), facility = logging.handlers.SysLogHandler.LOG_LOCAl3)

handler.setFormatter(formatter)
#handler에 formatter 적용
```

원격 로그서버에 접속하고 저장위치 설정을위한 handler 설정 

*로컬에서 로그 확인시
```python
stream_handler = logging.StreamHandler()
stream_handler.setFormatter(formatter)

```

```python
logger.addHandler(handler)
#로컬로그 추가시 logger.addHandler(stream_handler)
```

# 로그 설정 2
예)
```python
    def insert_db(self,device_id, device_ip, data):
        try:
            logging.info('insert database..') #info레벨 로그 설정(db 접속)      
            sql = f"INSERT INTO test_table(id,device_id,device_ip,data) VALUES(NULL,%s, %s, %s)"
            self.cursor.execute(sql,(device_id,device_ip,data))
            self.connection.commit()
        except:
            logging.error('insert_db error') #error레벨 로그 설정(db 접속 에러)
            raise ('insert_db failed')
```
