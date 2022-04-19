`python
내 드라이브
import socketserver, threading, time
import pymysql
import pymysql.cursors

class UDPHandler(socketserver.BaseRequestHandler):
    global frame,headerlist
    frame = []
    headerlist = [b'A1',b'A2',b'A3',b'A4',b'A5']

    def create_db_connection(self):
        self.mysql_host = '172.47.20.213'
        self.mysql_port = 3306
        self.mysql_user = 'test_user'
        self.mysql_password = '^Neoable'
        self.mysql_db_name = 'test_db'
        self.mysql_db_table = 'test_table'
      
        try:
            self.connection = pymysql.connect(
                host =self.mysql_host,
                port = self.mysql_port,
                user = self.mysql_user,
                passwd= self.mysql_password,  
                db = self.mysql_db_name,
            )
            print('SQL connect success!')
            self.cursor = self.connection.cursor()
        except:
            raise ConnectionError('sql connect failed')
    
    
    def db_merge(self,device_id,device_ip, data):
        frame.append((device_id,device_ip,data))

        if len(frame) != 4:
            lock.release()
        else:
            self.insert_db(frame[0][0],frame[0][1],frame[0][2]+','+frame[1][2]+','+frame[2][2]+','+frame[3][2])
            frame.clear()

    def insert_db(self, device_id, device_ip, data):
        lock.release()

        self.create_db_connection()            
        sql_data = (device_id,device_ip,data)
        print(sql_data)
        sql = f"INSERT INTO test_table(id,device_id,device_ip,data) VALUES(NULL,%s, %s, %s)"
        self.cursor.execute(sql,(device_id,device_ip,data))
        self.connection.commit()


    def handle(self) -> None:
        lock.acquire()
        datas = self.request[0].strip()
        socket = self.request[1]
        current_thread = threading.current_thread()
        print(current_thread)
        device_id = None
        header= datas[3:5]
        tail = datas[-3:]
        device_ip = self.client_address[0]
        if header in headerlist:
            db_data = bytes.hex(datas[12:])
            device_id = header
            db_data = [str(int(db_data[i:i+2], 16)) for i in range(0, len(db_data),4)]
            db_data = ','.join(db_data)
            data = db_data
            self.db_merge(device_id, device_ip, data)
    
        else:
            if tail in [b'FFE']:
                db_data = bytes.hex(datas[:-3])
                db_data = [str(int(db_data[i:i+2], 16)) for i in range(0, len(db_data),4)]
                db_data = ','.join(db_data)
                data = db_data
                self.db_merge(device_id, device_ip, data)
            
            else:
                db_data = bytes.hex(datas)
                db_data = [str(int(db_data[i:i+2], 16)) for i in range(0, len(db_data),4)]
                db_data = ','.join(db_data)
                data = db_data
                
                self.db_merge(device_id, device_ip, data)

       
class ThreadUDPSERVER(socketserver.ThreadingMixIn, socketserver.UDPServer):
    pass


if __name__ == '__main__':
    HOST, PORT = '0.0.0.0', 5009

    lock = threading.Lock()

    server = ThreadUDPSERVER((HOST, PORT), UDPHandler)

    server_thread = threading.Thread(target=server.serve_forever)
    server_thread.daemon = True

    try:
        
        server_thread.start()
        print('Server started at {} port {}'.format(HOST,PORT))
        while True: time.sleep(100)
    
    except (KeyboardInterrupt, SystemExit):
        server.shutdown()
        server.server_close()
        
        exit()
`
