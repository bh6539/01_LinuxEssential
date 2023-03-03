Linux Essential 과정 정리

########################
제 01장. 리눅스 소개
########################

자격증
* 민간 자격증
* 국가 자격증
   리눅스 마스터 1급/2급
   네트워크 관리사 1급/2급
   
   정보처리기사
* 국제 공인 자격증
   AWS SA
   RedHat RHCSA/RHCE
   LPIC

[참고] 복제된 운영체제에서 변경 사항
* 호스트 이름
   # hostnamectl
   # hostnamectl set-hostname server2.example.com
IP 설정
   # nm-connection-editor
   # nmcli connection up ens160


########################
제 02장 리눅스 설치
########################



########################
제 03장 리눅스 환경 설정
########################

리눅스의 선수지식
Runlevel == Target
runlevel 3 == multi-user.target
runlevel 5 == graphical.target


systemctl isolate multi-user.target|graphical.target
systemctl set-default multi-user.target|graphical.target
서비스 기동
systemctl enable|disable firewalld
systemctl start|stop firewalld

제어 문자(Control Character)
 <CTRL + C>, <CTRL + D>

도움말과 암호변경
man CMD
man ls
man -k calendar (키워드)
man -f passwd (목록)
man -s 5 passwd
Section 1 : 명령어 및 데몬등에 대한 매뉴얼
Section 5 : 운영체제 설정 파일 및 데몬의 설정 파일 매뉴얼
passwd CMD
passwd fedora






########################
제 04장 리눅스 기본 정보
########################

시스템 기본 정보 확인

uname CMD
uname -a (all) (운영체제 커널버전 확인)
cat /etc/redhat-release (배포판 버전 확인) (ex. ls /etc/*release*

		참고 ) 기본 시스템 설정 구성 Red Hat Enterprise Linux 8 | Red Hat Customer Portal
data CMD
date 08301300 (시간변경가능)
date +%m%d (시간출력 포멧 변경가능)
		참고) NTP 서버에 시간 동기화
		
		[실무예] NTP 서버에 시간 동기화
	vi /etc/chrony.conf
		server time.bora.net iburst
systemctl stop chronyd
systemctl start chronyd
cal CMD





########################
제 05장 파일 및 디렉토리 관리
########################

디렉토리 이동 관련 명령어
	[참고] 파일시스템 기본 구조
		man 7 hier

	pwd CMD
		[참고]  PS1 변수 ( $HOME/.bashrc)
		export PS1=’[\u@\h \w]\$ ‘

	cd CMD
		경로 (Path)
		 -상대경로(Relative Path)	cd dir1
		 -절대경로(Absolute Path)	cd /dir1

		[참고] 자신의 홈 디렉토리 이동
		 cd
		 cd ~
		 cd $HOME

		[참고] 사용자 홈디렉토리 이동
		 cd ~fedora

		[실무예] 이전 디렉토리로 이동
		 cd -
		
		[실무예] 옆에 있는 디렉토리로 이동
		 cd ../dir2

디렉토리 관리 명령어
	ls CMD
	ls -l dir1
	ls -ld dir1
	OPTIONS : -l(자세히), -d(디렉토리의 정보), -R(Recursive 하위디렉토리까지), -a(all 모든 정보 .숨김파일까지), -F(파일 타입), -i(inode), h(사용자가 보기편하게 출력), -t(시간순서대로 정렬 최신 부터), -r(역순 출력)

	[참고] alias
	(선언) alias ls\’ls -l | grep “^-”’
	(확인) alias
	(해제) unalias ls 

	[실무예] 실무에서 많이 사용되는 ls CMD
	cd /Log_dir
	ls -altr

mkdir CMD
	mkdir -p dir1/dir2
	OPTIONS : -p(디렉토리 경로로 생성)

rmdir CMD
	rm -rf dir1

파일 관리 명령어
	touch CMD
		touch file1 (빈 파일 생성)
		touch -t 08301300 file (파일 시간을 변경)
	cp CMD
		cp file1 file2
		cp file1 dir1
		cp -r dir1 dir2 (dir2가 없다면 디렉토리를 복사, dir2가 있다면 dir2내부에 복사)
		OPTIONS : -r(디렉토리복사), -i(대화형 모드, 물어봄), -f(i와 반대 물어보지않고 실행), -p(퍼미션 값, 시간을 그대로 유지, 백업 복원시 사용), -a(아카이브 여러개옵션이 포함)
		
		[실무예] 설정 파일을 백업하는 경우
		cp -p httpd.conf httpd.conf.orig
		cp -a /src /src.orig

		[실무예] 로그 파일 비우기
		cp /dev/null file.log
		cat /dev/null > file.log
		> file.log

	mv CMD(파일 옮기기, 이름 바꾸기)
		mv file1 file2 (file1을 file2로 이름변경)
		mv file1 dir1 (file1을 dir1로 이동)
		mv dir1 dir2 (dir2가 없으면 이름변경, dir1가 있다면 dir2내부로 이동)
		OPTIONS : -i(대화형, 물어봄), -f(i와 반대, 강제로 실행)

		[참고] 파일시스템(File System) : 파일을 저장하고 관리하는 구조 체계
			ex) ext4, xfs, fat32, ntfs

		[참고] 와일드 카드 문자
			* : 여러 글자 (.file 제외)
			? : 한글자 
			{} : 선택적 단어
			[] : 선택적 글자
	
rm CMD
		rm file1
		rm file1 file2
		rm -rr dir1
		OPTION : -r
		[실무예] rm 명령어로 지운 파일 복원하기
		(TUI) extundelete CMD
		(GUI) Testdisk 툴
	
파일 내용 확인 명령어
	cat CMD
		cat -n file1 (줄번호 같이 출력)
		cat file1 file2 > file3 (파일 합치기)

	more CMD
		CMD | more (출력량이 길때 more 명령어와 같이 사용)
		ex) ps -ef | more
		 cat /etc?services | more
		 netstat -an | more
		 systemctl list-unit-files (명령어에 내장되어 사용)

	head CMD
		[실무예] pps/nstat 엘리어스 만들기
alias pps='ps -ef | head -1 ; ps -ef | grep $1'
alias nstate='netstat -antup | head -2 ; netstat -antup | grep :$1'

	tail CMD
		top
		tail -f /var/log/messages

		tail -f /var/log/messages | egrep -i ‘warn|error|fail|crit|alert|emerg’
		tail -f /var/log/messages /var/log/secure

		[참고] telnet 서비스 기동하기
		yum install tenlet telnet-server
		systemctl enable –now telnet.socket


기타 관리용 명령어
	wc CMD
		[참고] 데이터 수집 (Data Gathering)
		ps -ef | tail -n +2 | wc -l
		ps -ef | grep httpd | wc -l
		cat /etc/passwd | wc -l
		rpm -qa | wc -l
		df -k / | tail -1 | awk ‘{print $5}’
		cat /var/log/messages | grep ‘Jun 19’ | grep ‘Started Telnet Server’ | wc -l

	su CMD
		su fedora	이전 사용자의 환경을 그대로 fedora에 접속
		su - fedora	fedora의 환경으로접속
		 > ssh fedora@localhost == su - fedora
	
sudo CMD (/etc/sudoers, /etc/sudoers.d/*)
		sudo CMD
		sudo -l (권한 목록확인)
		sudo -i (관리자로 스위칭)

	id CMD
	groups CMD

	last CMD (/var/log/wtmp)
		last -i 
		last -f /var/log/wtmp.20230128

	lastlog (/var/log/lastlog)
ex) 휴면계정을 찾을때 사용
	
	lastb CMD (/var/log/btmp) 로그인 실패했을때 기록이 남음

	who CMD (/var/run/utmp) 누가 접속해있는지 확인

	w CMD 
		while true (모니터링)
		do
			CMD
			sleep 2
		done

		[참고] watch CMD (모니터링)

	exit CMD



########################
제 06장 파일 종류
########################
파일 종류
	일반 파일 (Regular File)

	디렉토리 파일 (Directory File)

	링크 파일 (Link File)
하드 링크 파일 (Hard Link File)
	ln file1 file2
심볼릭 링크 파일 (Symbolic Link File)
			ln -s file1 file2

	장치 파일 (Device)
		블록 장치 파일 (Block Device File)
		캐릭터 장치 파일 (Character Device File)

########################
제 07장 파일 속성 관리
########################
chown CMD
	chown fedora file1 (오너 변경)
	chown .user01 file1 (그룹 변경)
	chown -R fedora:fedora /home/fedora (-R 하위파일까지 변경)
chgrp CMD
chmod CMD
	심볼릭 모드 (Symbolic Mode)
		u(유저), g(그룹), o(다른사용자), a(all)
		chmod u+r file1
		chmod g-w file1
		chmod o=w file1
		chmod a=rwx file1
	옥탈 모드 (Octal Mode)
		chmod 744 file1
	파일 & 디렉토리 퍼미션 의미
		파일(r / w / x)
		디렉토리 (r (ls) / w(생성/삭제) / x (cd))


	퍼미션 적용 순서
		UID check -> GIDs sheck -> Other
	umask (002 -> 022 -> 027)
		(관리자) /etc/bashrc
		(사용자) $HOME/.bashrc


MAC
IP address - 호스트 구분하는 번호
Port address - 서비스 구분하는 번호 ( 0 - 65535)
	0 - 1023 : 잘 알려진 서비스를 위해 할당하는 포트 번호
		22 : SSH	23 : Telnet	25 : SMTP	53 : DNS	80 : HTTP
		110 : POP3	143 : IMAP4	123 : NTP


	

########################
제 08장 VI/VIM 편집기
########################

VI 편집기 모드
	명령 모드 (Command/Edit Mode)
		이동, 삭제, 실행취소/재실행, 검색, 검색&바꾸기, 
	입력 모드 (Insert/Input Mode)
		i, a ,o, I, A, O
	최하위행 모드 (Last Line/Ex Mode)
		저장&나가기
		

VI 편집기 환경파일
	$HOME/.vimrc		(rc = run command)
	set nu
	set ai
	set ts=4




########################
제 09장 사용자와 통신할 때 사용하는 명령어
########################
mail/mailx CMD
	mail -s ‘제목’ admin@example.com < report.txt

wall CMD
	wall < /etc/MESS/work.txt
	
	[참고] 긴급 작업 절차
	touch /etc/nologin		(접속이 불가능한 상태로 만듬)
	wall < /etc/MESS/work.txt  	(접속중인 사용자에게 메세지를 보냄)
	fuser -cu /home		(접속중인 사용자의 리스트 확인)
	fuser -ck /home		(접속중인 사용자의 연결 종료)
	rm -f /etc/nologin		(접속이 가능한 상태로 만듬)












########################
제 10장 관리자가 알아두면 유용한 명령어
########################

cmp/diff CMD
	[실무예] 설정 파일 비교
	diff httpd.conf httpd.conf.OLD

	[실무예] 디렉토리 마이그레이션 종료 후 비교
find	/was1 | wc -l ; find /was2 | wc -l
diff -r /was1 /was2

sort CMD
	CMD | sort -k 3
	CMD | sort -k 3 -r

	df -k
	du -sk /var
	cd /var ; du -sk * | sort -nr | more

file CMD
file *
########################
제 11장 검색 관련 명령어
########################
grep CMD
	grep OPTIONS ‘PATTERN’ file1
	OPTIONS : -i(대소문자 구분없이), -v(제외), -l(패턴이있는 파일이름만), –r(내용까지 확인), -n(라인번호 출력), –color, -A(
	PATTERN : * . ^ $ [abc] [a-c] [^a](a 제외)

	CMD | grep root
		cat /etc/passwd | prep root
		rpm -qa | grep httpd
		ps -ef | grep rsyslogd
		systemctl list-unit-files | grep ssh
		netstat -antup | grep :22

	[실무예] 로그 파일 점검 스크립트
	aliias chklog=’cat $1 | egrep -i “warn|error|fail|crit|alert|emerg”’

find CMD
	find / -name core -type f
	find / -user fedora -group fedora
	find -mtime -7|7|+7
	find / -size -30M|30M|+30M
	find / -perm -755|755
	find / -name core file1 -exec CMD {} \;

	[실무예] 오래된 로그 파일 삭제
	find /LogDir -name “*.log” -type f -mtime +30 exec rm -f {} \;

	[실무예] 갑자기 파일시스템 풀 난 경우
	df CMD + du CMD + find CMD + lsof CMD
	find /var -mtime -2 -size +1G -type f

	[실무예] 에러 메세지 검색
	(소스 코드 O) find /src -type f -exec grep -l ‘Server Error’ {} \;
	(소스 코드 X) 구글링 키워드 이용
		site:redhat.com “Server Error”
		site:redhat.com “Server Error” AND “vmware”
		가상화 .pdf






########################
제 12장 압축과 아카이빙
########################
압축(Compress)
	gzip/gunzip CMD
		gzip file1
		gunzip -c file1.gz (내용확인)
		gunzip file1.gz (압축해제)

	bzip2/bunzip2 CMD
		bzip2 file1 (압축)
		bunzip2 -c file1.bz2 (내용확인)
		bunzip2 file1.bz2 (압축해제)

	xz/unxz CMD
		xz file1 (압축)
		unxz -c file1.xz (내용확인)
		unxz file1.xz (압축해제)

아카이빙(Archive)
	tar CMD
		tar cvf file.tar file1 file2 (묶기)
		tar tvf file.tar	(내용확인)
		tar xvf file.tar (풀기)

압축 + 아카이빙
	tar CMD
		tar cvzf file.tar.gz (tar CMD + z 옵션)
		tar cvzf file.tar.gz
		tar cvzf file.tar.gz

		tar cvjf file1.tar.bz2 (tar CMD + j 옵션)
		tar cvjf file.tar.bz2
		tar cvjf file.tar.bz2

		tar cvzf file1.tar.xz (tar CMD + J 옵션)
		tar cvJf file.tar.xz
		tar cvJf file.tar.xz

	jar CMD
jar cvf file.tar file1 file2 (묶기)
		jar tvf file.tar	(내용확인)
		jar xvf file.tar (풀기)

zip CMD
zip file.zip file1 file2 (압축)
unzip -l file.zip (내용확인 리스트 -c:내용까지 확인)
unzip file.zip (압축풀기)
########################
제 13장 배시쉘의 특성
########################
리다이렉션
	fd (파일 기술자)
	 0	stdin ( keyboard)
	 1	stderr (Screen)
	 2	stderr (Screen)

	입력 리다이렉션 (Redirection stdin)	wall < /etc?MESS/work.txt
	출력 리다이렉션 (Redirection stdout) ls -l > lsfile.txt
	에러 리다이렉션 (Redirection stderr) ls -l /test /nodir > list.txt 2>&1

	[실무예] 스크립트 로그 파일 생성
	 ./script.sh > script.log 2>&1

	[실무예] 출력 내용이 긴 명령 수행시 출력 화면 분석
	 ./configure > config.log 2>&1

	[실무예] 일반사용자가 명령 수행시 에러메세지를 지우는 경우
	 find / -name core -type f 2>/dev/null

파이프(Pipe)
	CMD | more
	CMD | grep inetd
	CMD | CMD | …

	[실무예] 모니터링 구문 + 데이터 수집(CMD | tee -a httpd.cnt)
	while true
	do
		ps -ef | grep httpd | wc -l | tee -a httpd.cnt
		sleep 2
	done

	[실무예] 여러 터미널 화면을 공유하는 경우
	script -a /dev/null | tee /dev/pts/1 | tee /dev/pts/2

배쉬셀 기능
	set -o (전체 기능 목록)
	set -o vi (기능 켜기)
	set +o vi (기능 끄기)

	set - o ingnoreeof (로그아웃 방지기능)
	set -o noclobber (덮어쓰기 방지)




<TAB>
	파일이름 자동 완성 기능
	디렉토리 안에 파일 목록 보기
	명령어 검색하기
화살표
	이전에 수행된 명령어를 편집해서 사용하기
	이전에 수행된 명령어를 확장해서 사용하기
	확인 + 명령수행 + 확인
Copy & Paste

변수(Variable)
	변수 종류
		지역변수 (Local Variable)		var=5
		환경변수 (Environment Variable)	export var=5
		특수변수 (Special Variable)		
$$, $?, $!, $0, $1, $2, $#, $*(인수 전체)
	변수 선언 방법
		var=5
		export var=5
		echo $var
		unset var
	export 의미
	시스템/쉘 환경변수 (set/env)
		PS1 변수 : export PS1=’[\u@\h \w]\$ ‘ ($HOME/.bashrc)
		PS2 변수 : export PS2=’> ’
		PATH 변수 : export PATH=$PATH:/root/scripts ($HOME/.bash_profile)
		HISTTIMEFORMAT 변수 : HISTTIMEFORMAT=”%F %T    ” (/etc/profile)
		HOME 변수
		PWD 변수
		LOGNAME 변수
		USER 변수
		UID 변수
		TERM 변수 : export TERM=vt100
		LANG 변수 : export LANG=ko_KR.UTF-8
		
	
쉘 메타캐릭터 (Shell Metacharacter)
	‘’ “” `` \ ;

명령어 히스토리 (Command History)
	HISTSIZE=512
	HISTFILE=$HOME/.bash_history
	HISTFILESIZE=512

엘리어스 (Alias)
	alias cp=’cp -i’
	alias
	unalias cp

환경파일
	/etc/profile
	
$HOME/.bash_profile

	$HOME/.bashrc


	/etc/profile
		/etc/profile.d/{*.sh, sh.local}
		/etc/bashrc
			/etc/profile.d/*sh
	~/.bash_profile
		~/.bashrc
			/etc/bashrc
				/etc/profile.d/&.sh
	~/.bashrc
		/etc/bashrc
			/etc/profile.d/*sh




########################
제 14장 프로세스 관리
########################

프로세스 정보
	/proc/PID/*
	-PID, PPID

프로세스 관리
	프로세스 관리1
		프로세스 실행
			fg) gedit
			bg) gedit &
		프로세스 확인
			ps -ef | grep sshd
			ps aux | grep sshd
		프로세스 종료
			kill -1(재실행)|-2(키보드인터럽트)|-9(강제종료)|-15(정상종료) PID PID
	
			[참고] killall CMD, pkill CMD
			[참고] kill vs killall/pkill

	프로세스(Job) 관리2
		잡 실행
			fg) gedit
			bg) gedit &
		잡 확인
			jobs
			fg %1
			bg %1
			<CTRL + z> 포그라운드 실행중인 잡 일시정지
		잡 종료
			kill %1

프로세스 모니터링
	top CMD
		top
		top -u wasuser
		
	[참고] 모니터링 툴
		top/htop	: CPU/MEM
		iotop		: DISK I/O
		iftop		: Network I/o
		atop		: data gathering ( 데이터 수집 )
		gnome-system-monitor



프로세스 모니터링 관련 명령어
lsof CMD
		lsof
			lsof /usr/sbin/sshd
			lsof /tmp
		lsof /etc/passwd
		lsof -c sshd
			lsof /usr/sbin/sshd
			lsof -c sshd
		lsof -p PID
		lsof -i
	
pmap CMD
		pmap PID
	
	pstree CMD
		pstree
		pstree user01
		pstree -alup PID

	nice/renice CMD
[실무예] 백업 스크립트/데이터수집 스크립트 실행할 때
	nice ./backup.sh &

[실무예] CPU 부하가 높은 프로세스가 존재하는 경우
	renice -n 10 PID



########################
제 15장 원격접속과 파일전송
########################

파일전송
	scp CMD
		scp file1 server2:/tmp/file2
		scp server2:/tmp/file2 /test/file1
		scp -r dir1 server2:/tmp	(-r 복사대상이 디렉토리일때)
	sftp CMD
		
원격접속
	ssh CMD
		ssh server2
		ssh server2 CMD

		[참고] Public Key Authentication
		ssh-kegen
		ssh-copy-id -i id_rsa.pub root@server2
		
