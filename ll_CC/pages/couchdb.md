#CouchDB

공식사이트 : [http://couchdb.apache.org](http://couchdb.apache.org)
CouchDB의 JS 구현체인 [PouchDB](https://pouchdb.com)와 함께 쓸 수 있다.
로컬에서 PouchDB에 내용을 저장해두고, 인터넷이 연결되었을때 서버(CouchDB)와 Sync하는 식으로 사용한다.

## 설치
윈도우나 맥은 공식사이트에서 배포중인 바이너리를 바로 실행하면 되고,
서버는 개인적으로 우분투를 사용하고있어서 ubuntu couchdb로 검색하면 나오는 [이 문서](https://www.digitalocean.com/community/tutorials/how-to-install-couchdb-and-futon-on-ubuntu-14-04)를 보고 설치하였다.

다만 설치후 외부에서 접속이 가능하게 하려면 몇가지 작업해줘야할 것이 있는데
1. AWS보안 설정에서 couchdb가 사용하는 5984번을 열어주는 것
2. 관리자 권한으로 접근해야하는 /etc/couchdb/default.ini 파일내에서 
[httpd]
port = 5984
bind_address = 127.0.0.1
위의 bind_address를 0.0.0.0으로 바꿔줘야 하는 것
3. http://serveradd:5984/_utils 로 접속하여 관리자 이이디와 비밀번호를 설정해야 한다.
4. 마지막으로 앱에서 접근할때 cors가 나지않도록,
npm install -g add-cors-to-couchdb (서버가 아닌 pc에서해도 됨)
add-cors-couchdb http://serveraddr -u adminId -p adminPw
명령어를 실행하여준다, 
