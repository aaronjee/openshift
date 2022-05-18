# RH-SSO Deployment on OpenShift 4

1. 프로젝트 생성
oc new-project sso

2. RHSSO 배포
Console > Developer
catolog > Red Hat Single Sign-On 7.3 + PostgreSQL (Persistent)

3. Template
RH-SSO Administrator Username/RH-SSO Administrator Password > admin 

4. PV를 사용하기 떄문에 bastion에 nfs 구성
$ dnf install nfs-utils
$ mkdir -p /exports/sso
$ vi /etc/exports
/exports/sso *(rw,sync,no_root_squash)
$ systemctl enable nfs-server; systemctl start nfs-server

5. Pod생성 확인
5-1. postgresql error
pg_ctl: another server might be running; trying to start server anyway
waiting for server to start....2022-05-18 12:35:45.502 UTC [24] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2022-05-18 12:35:45.543 UTC [24] LOG:  listening on Unix socket "/tmp/.s.PGSQL.5432"
.2022-05-18 12:35:46.640 UTC [24] LOG:  redirecting log output to logging collector process
2022-05-18 12:35:46.640 UTC [24] HINT:  Future log output will appear in directory "log".
.. done
server started
/var/run/postgresql:5432 - accepting connections
=> sourcing /usr/share/container-scripts/postgresql/start/set_passwords.sh ...
psql: FATAL:  database "postgres" does not exist

>> solution : userdata 디렉토리 삭제 후 Pod 재기동
