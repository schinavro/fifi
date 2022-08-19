# INSTALL

# yum proxy configuration
To use `yum` package manage in the calculation node, you need to set up the proxy environment. 

`[root@node08]# echo "proxy=http://headnode:3128" >> /etc/yum.conf`

# Error

## rpmdb error

```
[root@node30] # yum -y intall openssl openssl-devel
오류: rpmdb: BDB0113 Thread/process 5632/139869042812736 failed: BDB1507 Thread died in Berkeley DB library오류: dbenv->failchk의 db5 오류(-30973): BDB0087 DB_RUNRECOVERY: Fatal error, run database recovery오류: db5(을)를 이용하여 Packages 인덱스를 열 수 없습니다 -  (-30973)오류: /var/lib/rpm 안의 패키지 데이터베이스를 열 수 없습니다CRITICAL:yum.main:
Error: rpmdb open failed
```

이럴땐 기존  db를 지워주고 다시 db를 리빌딩 해주면 된다.

`[root@node30] # rm -f /var/lib/rpm/_db*# rpm -vv --rebuilddb`

그러면 yum이 정상적으로 실행된다.

출처: https://nowknowing.tistory.com/91 [nowknowing:티스토리]
