Linux Essential  과정 정리

 리눅스의 선수지식
 Runlevel = Target
    Runlevel 3 = multi-user.target
	Runlevel 5 = graphical. target
	
  Runlevel 변경 = 일시 변경 systemctl isolate multi-user.target| graphical.target
                 영구 변경(부팅시 변경) systemctl set-defult multi-user.target| graphical.target

  서비스 기동
   systemctl enable|disable firewalld
   systemctl start|stop <서비스 이름>
  
  제어 문자(Control Character)
   CTRL + c : 인터럽트 걸어서 실행중인 명령어 강제 종료
   
   CTRL + d : 1.파일의 끝 2. 현재 쉘 종료
  
  * 도움말과 암호변경
      man CMD
	 <메뉴얼>
      passwd CMD
    <패스워드 변경>	  
	
	CMD(명령어) --help 간략하게 명령어에 대한 정보 확인
	man CMD(명령어) 해당 명령어에 대한 자세한 정보 확인
	   #mas ls
		   /-검색할문자 (ex /-a) 다음찾기 = n 이전찾기 = 쉬프트+n (대문자 N)
	/SEE ALSO = 검색한 명령어와 관련한 다른 정보 찾아줌
	
   명령어를 모르는 경우
	   mandb 로 명령어 메뉴얼 db생성
	   man -k 명령어와 관련된 단어(calendar) = 
	   man -f passwd = 같은 이름을 가진 메뉴얼이 여러개인지 확인
	   man -s 5 passwd = 위에서 찾은 섹션을 토대로 검색
	
	메뉴얼 section 1 : passwd 명령어 메뉴얼
	메뉴얼 section 5 : /etc/passwd 파일에 대한 메뉴얼
	
	# which ls : 명령어의 위치를 확인하는 명령어
			<ls 명령의 위치를 알려줌>
	# ls --help
	
4장 리눅스 기본 정보 확인

	시스템 기본 정보 확인확인
		uname CMD
			<uname -a> : 주로 많이 쓰임 / 커널 릴리즈, cpu종류 확인 할때 많이 쓰임
			cat /etc/redhat-release : 배포판 버전 확인하는데 쓰임
			cat /etc/*release* : 배포판 마다 이름이 달라서 와일드카드를 통해 검색하는것이 좋음
		date CMD
			# date 08301300
			# date +%m%d
			NTP 서버에 시간 동기화
				#vi /etc/chrony.conf
				   # pool 2.centos.pool.ntp.org iburst (이전 서버 주석처리)
					 server time.bora.net iburst (NTP서버 지정)
				systemctl stop chronyd 
				systemctl start chronyd
				systemctl enable chronyd
				
				rpm -qa : 설치된 모든 패키지 확인
					rpm -qa | grep <패키지이름> or (yum list <패키지이름> : 인터넷 연결 필요)
			
		cal CMD (달력 출력)
		
5장 파일 및 디렉토리 관리
	용어:파일 시스템? = 파일을 저장하고 관리하는 구조체계
	
	디렉토리 이동 관련 명령어
		[참고] 파일시스템 기본 구조
		# man 7 hier
		
		pwd CMD : 현재 디렉토리
			[참고] PS1 변수 : pwd를 안쓰기위해 변수 내용 변경 ($home/.bashrc)
				export PS1='[\u@\h \w<-소문자w인게 중요]\$ ' 
									= 현재 있는 경로가 다 표시 되므로 PWD를 사용 할 필요가 없어짐
				
		CD CMD : 디렉토리 변경 cd만 입력하면 자신의 홈폴더 이동
			(~ = /home) 
			경로(Path)
				상대경로 ex) cd dir1
				
				절대경로 ex) cd /dir1 (/가 있으면 절대경로)
				
			자신의 홈 디렉토리 이동
				# cd
				# cd ~
				# cd $HOME
				
			사용자 홈 디렉토리 이동
				# cd ~fedora
			
			이전 디렉토리 이동 [실무에서 자주 쓰임]
				# cd -
				
			옆에 있는 디렉토리 이동
			    # cd ../<디렉토리 이름>
		
	디렉토리 관리 명령어
		ls CMD
			# ls -l <디렉토리이름> = 디렉토리의 내용을 출력
			# ls -ld dir1 = 디렉토리 정보를 자세히 출력
				옵션 : -l -d(=--directory) -R -a(.으로 시작하는것도 보여줌) 
					  -i(inode까지 보여줌) -h(용량을 단위 환산해줌)
					  -t(시간순 가장아래가 가장최근) -r(역시간순)
			# ls -R(=--recursive) = 해당 디렉토리와 하위 디렉토리 내용을 출력
			
			alias
				(선언) # alias ls='ls -h --color=tty'
				(확인) # alias 
				(해제) # unalias ls
				
			실무에서 많이 쓰이는 ls  CMD
				# cd /Log_dir 
				# ls -altr
			
		mkdir CMD
			mkdir -p dir1/dir2 (-p = dir1이 없어도 dir1을 만들고 하위에 dir2를 만듬)
		rmdir CMD
			# rm -rf dir1       /* -r : recursive(안에 있는 모두다), -f : force(묻지 않고) */
				                (주의) 파일/디렉토리 삭제시 지워야 되는건지 고민하고 지워야함
	
	파일 관리 명령어
		touch CMD
			# touch -t 08301300 file2 (-t = 파일의 변경시간을 변경)
		cp CMD
			# cp file1 file2 			/* file1 파일내용을 file2로 생성 */
			# cp file1 dir1 			/* file1 파일내용을 dir1디렉토리에 file1 생성 */
			# cp -r dir1 dir2   (-r = dir1 디렉토리를 dir2디렉토리로 생성
			                          원래 dir2가 없으면 dir2 안에 dir1 내용이 복사 되고 
									  있으면 dir2 안에 /dir1 이 생성됨)
			-r(서브 디렉토리 내에 있는 모든 파일까지 통째로 복사)
			-i(복사할 파일이 존재하는 경우 복사할 것인지 물어봄 = 덮어쓰기) 
			-f(복사할 파일이 존재할 때 삭제하고 복사)
			-p(원본 파일의 소유, 그룹, 권한, 허용 시간을 보존한 채로 복사)
			-a(= -rp)(원본 파일의 속성, 링크 정보를 유지 하면서 복사)
			
			실무예 ) 설정 파일을 백업하는 경우
				# cp -p httpd.conf httpd.conf.old 
				# cp -p httpd.conf.old httpd.conf
				
				# cp -a /src /src.orig
				
			로그 파일 내용을 비울때
			# cp /dev/null file.log 
			# cat /dev/null > file.log 
			# > file.log (가장 좋은 명령어)
			세개다 비슷한 뜻임
			
		mv CMD
		
			# mv file1 file2		/* file1 파일이 이름이 file2로 변함 */
			# mv file1 dir1			/* file1 파일이 dir1 디렉토리에 하위경로로 이동 */
			# mv dir1 dir2			/* dir1 디렉토리가 dir2 디렉토리에 하위경로로 이동
                                       dir2가 없다면 dir1이 dir2로 이름이 바뀜 */
			
			② 여러개의 파일을 지정하기 위해서 wild card character 사용하기
				# mkdir dir1 
				# mv file* dir1 (= # mv file1 file2 file3 file4 dir1)
				# ls dir1 
			
			
		rm CMD
			
			# rm -rf dir1 (-rf = 묻지 않고 모두다)
			
			rm 명령어로 지운 파일 복원하기
			(TUI)	extundelete CMD
			(GUI)	TestDisk 툴
		
		와일드 카드 문자
			● * : 0 or more character (except .file .<이름>은 제외) (EX) # cp file* dir1)
			● ? : one charater (EX) # cp file? dir1)
			● { } : 선택적인 하나의 문자열(단어) (EX) # cp file{apple,bannar,orange} dir1
			● [ ] : 선택적인 하나의 문자 (EX) # cp file[123] dir1

	파일 내용 확인 명령어
		cat CMD
			# cat -n file1 (-n 줄번호가 같이 출력됨)
			# cat file1 file2 > file3 (file1,2 내용을 file3에 합침)
			
		more CMD
			# CMD | more (보통 명령어 뒤에 파이프 뒤에 쓰는경우가 많음)
			# ps -ef | more
			# cat /etc/services |more
			# netstats -an | more
			# systemctl list-unit-file (자체적으로 more이 내장되어 있는 경우도 있음)
			[참고] 파이프(|) 기호의 의미에 대해
				ps -ef | more (뒤에 파일을 지정하면 안됨)
				cat /etc/passwd | grep fedora 				
				(앞에 명령어의 출력결과를 파이프 뒤에 명령어의 입력으로 넣는다)
				
		head CMD (내용의 상위 10줄을 출력함)
			# head /etc/passwd  (# head -10 /etc/passwd, # head -n 10 /etc/passwd)
			# head -n 5 /etc/passwd	(= head -5 /etc/passwd)
				(숫자에 해당하는 라인 번호 수 만큼만 출력 (기본은 10줄))
			<실무예>
			# alias pps='os -ef | head -1 ; ps -ef | grep $1'
			# alias nstate='netstat -antup | head -2 ; netstat -antup | grep $1'
		tail CMD (head와는 반대로 끝에 10줄을 출력함)
			# top
			# tail -f <파일이름> (tail 명령어에서 제일 중요!)
			
			# tail -f /var/log/messages | egrep -i '(warn|fail|error|crit|alert|emerg)'
				(안좋은 로그를 검색하는것)
			# tail -f /var/log/messages /var/log/secure (동시에 여러 로그도 볼수있음)
			
		[참고] telnet 서비스 오픈
			# yum install telnet telnet-server (텔넷 패키지 설치)
			# systemctl enable --now telnet.socket (start와 enable동시에 해줌)
			
			
	
	기타 관리용 명령어
		wc CMD
			[참고] 데이터 수집
				# ps -ef | tail -n +2 | wc -l
				# ps -ef | grep httpd | wc -l
				# cat /etc/passwd | wc -l
				# rpm -qa | wc -l
				# df -k / | tail -l | awk '{print $5}'
				# cat /var/log/messages | grep 'Jan 19' | grep 'Started Telnet Server' | wc -l
		
		su CMD
			# su fedora < 페도라 사용자가 되지만 이전 사용자가 사용하던 환경 변수가 그대로 같이 스위칭됨
			# su - fedora < 완전히 새로 로그인 fedora 사용자에 있는 환경변수로 전부 바뀜 (이것이 기본형식)
		
		sudo CMD (관리자만 사용가능한 명령어를 사용자가 사용할수 있게함 사용가능한 명령어 지정가능)
			(/etc/sudoers, /etc/sudoers.d/*)
			# sudo -l (sudo로 사용가능한 명령어 확인)
			# sudo -i (관리자 전환 완전한 root는 아님)
		
		
		id CMD
		groups CMD
		
		last CMD (/var/log/wtmp 이 파일에 내용을 출력함 데이터 형태의 파일로 cat로 출력이 안됨) 
				 (사용자에 로그인,로그아웃 정보를 출력함)
			last -i
			last -f /var/log/wtmp.20230128
			
		lastlog CMD(/var/log/lastlog)
				   (사용자에 마지막으로 로그인한 시간을 알려줌)
			
		lastb CMD(/var/log/btmp)
				 (접속에 실패한 기록을 보여줌)
			
		who CMD(/var/run/utmp)
			   (서버에 현재 접속중인 사용자롤 확인)
			   
		w CMD (로그인되어 있는 사용자들이 수행한 명령어를 확인)
			모니터링시 사용하는 CMD
			while ture (기억할것)
				do
					echo "---------`date`----------"
					CMD
					sleep 2
				done
			2초마다 CMD를 실행함
			
			[참고] watch CMD
		
		exit CMD (현재쉘을 종료)
	
6장 파일의 종류
	■ 파일의 종류
		■ 일반 파일(Egular File)
		■ 디렉토리 파일(Directory File)
		■ 링크 파일(Link File)(중요)
			* 하드 링크 파일(Hard Link File) (같은 파일시스템 내에서만 링크가 가능)
				■ 하드링크(# ln file1 file2)
				
					● 두개의 파일의 inode 번호가 동일한가? yes
					● ls -li 특이한 변화는? Hard Link 가 1올라간다
					● 두개의 파일 사이즈는 동일한가? yes
					● file2의 편집하면? 동시에 편집됨
					
			* 심볼릭 링크 파일(Symbolic Link File)(= 소프트 링크 파일)(파일 시스템을 넘어서 링크가 가능)(중요중요) 
				■ 심볼릭 링크(# ln -s file1 file2)
					
					● 두개의 파일의 inode 번호가 동일한가? no
					● ls -li 특이한 변화는?  lrwxrwxrwx 1 root root  5 Feb 17 10:08 file2 -> file1
					● 두개의 파일 사이즈는 동일한가? no
					● file2의 편집하면? file1도 편집됨
					● 디렉토리도 링크가 가능하다
					● 특이한 경우가 아닌한 일반적으로 절대경로를 통해 링크를 걸어야 한다!
				
		■ 장치 파일(Device File)
			* 블록 장치 파일(Block Device File)
			* 캐릭터 장치 파일(Character Device File)
		^^^^관리자가 관리^^^^
		
		
		■ 소켓 파일(Socket File)
		■ 도어 파일(Door File)
		■ 파이프 파일(Pipe File)
	
7장 파일 속성 관리
	
	chown CMD (파일의 소유자와 그룹을 바꿈)
		# chown -R fedora:fedora /home/fedora (해당 디렉토리와 그밑에 있는 모든 파일, 디렉토리의 소유자,그룹을 변경함)
	chgrp CMD (파일의 그룹을 바꿈)
	chmod CMD (파일에 대한 권한을 변경)
		퍼미션 변경
			* 심볼릭 모드(Symbolic Mode) # chmod u+x file1
			* 옥탈 모드(Octal Mode) # chmod 744 file1 (-(2^2)-(2^1)-(2^0) rwx = 7)

	파일,디렉토리 퍼미션 의미
		* 파일 (r / w / x)
		* 디렉토리 (r(ls -l) / w(생성,삭제) / x(cd CMD))
	퍼미션 적용 순서
		UID cheak -> GID Cheak -> other
	
	umask CMD(파일이나 디렉토리가 생성될때 기본퍼미션(Defult Permission)을 조정) 
		ex) umask=0022 기본퍼미션 파일=666 디렉토리=777 새파일 = 666-022=644 새디렉토리 777-022=755
		(주로 002 -> 022 -> 027)
		* (관리자) /etc/bashrc
		* (사용자) $HOME/.bashrc
	
	
8장 vi/vim 편집기

	vi(vim)
		편집기 3가지 모드
		ESC Mode(CMD mode/edit Mode) ----i,a,o,O----> Iput/Insert Mode
			|	 		             <------ESC------ 
			|
		shift + :
			|
		Last Line Mode (w!, wq, q!, wq! /w: write, q: quit)
		
		 *명령 모드
			이동, 삭제(x(한글자), dd(한라인)), 실행취소/재실행(u, U(라인 전체)), ...
		 *입력 모드
			i,a,o,O
		 *최하위 행 모드
			저장&나가기(w!, wq, q!, wq! /w: write, q: quit), 검색, 검색&바꾸기, 
			복사/이동하기(▪ :1,3 co 5	: 첫 번째 라인부터 3번째 라인까지 복사하여 5번째 라인 아래에 붙이기
					   ▪ :1,3 m 5	: 첫 번째 라인부터 3번째 라인까지 5번째 라인 아래에 이동하기)
		
		vi filename
		vi -r filename (비정상적으로 끝난 파일 복구)
		vu -L (복구할 파일들에 대한 전체적인 목록)
		
		[참고] 편집기 참고 기능
			라인번호 표시하기
				: set number   	(: set nu)
				: set nonumber 	(: set nonu)
			특정 줄로 이동
				: 30     (30G)    nG
			특정 단어 검색
				/ftp -> n -> N
				
	vi 편집기 환경파일
		* $HOME/.vimrc (~/.vimrc) 
			set nu (줄번호 보이기)
			set ai
			set ts=4 (탭 눌렀을때 들여쓰는 칸)

9장 사용자와 통신할 때 사용하는 명령어
	
	mail/mailx CMD
		# mailx -s '<제목>' email@email.com < /root/report.txt
 	wall CMD (접속된 사용자 전체에게 공지)
		# wall < /etc/MESS/work.txt
		[참고] 긴급 작업 절차
			# touch /etc/nologin (신규 로그인 막음)
			# wall < /etc/MESS/work.txt (접속한 사용자 전체에게 공지)
			...
			# fuser -cu /home (아직 접속중인 사용자 확인)
			# fuser -ck /home (아직 접속중인 사용자 강제 종료)
			...작업후
			# rm -f /etc/nologin (신규 로그인 허용)
			
			
10자 유용한 명령어

	cmp/diff CMD
		[실무예] 설정 파일 비교
			# diff httpd.conf httpd.conf.old

		[실무예] 디렉토리 마이그레이션 종료 후 비교
			# diff -r /was1 /was2
			
	sort CMD (정렬)
	
		* 일반적인 sort 명령어 사용
			# CMD | sort -k 3
			# CMD | sort -k 3 -r
		
		[실무예] 파일/파일시스템 사용량 점검
			# cd /var 
			# du -sk * | sort
			# du -sk * | sort -n
			# du -sk * | sort -nr | more
			
		
	file CMD
		# file * 
		
11장 검색 관련 명령어
	
	grep CMD
		grep options 'pattern' filename
			* options: -l(패턴이 있는 파일이름만을 출력한다.)
					   -r(패턴이 있는 경로와 내용을 출력한다)
					   -n(패턴을 포함하는 줄을 출력할 때 줄번호와 함께 출력한다.)
					   -v(패턴을 포함하는 줄을 제외하고 출력한다.)
					   -i(패턴을 찾을 때 대소문자를 구분하지 않는다.)
					   -w(패턴과 완전히 일치하는 행만 출력한다 ex) -w root = roothello(검색x) root hello(검색o))
					   -A num(찾은 패턴 아래 num만큼 행을 더 출력함 )
					   --color
			* pattern: * (# grep 'roo*t' /etc/passwd 앞의 문제가 0회 이상 math)
					   . (# grep 'no...y' /etc/passwd 1개의 문자 매치(정확히 1개의 문자와 매치))
					   ^root (# grep '^root' /etc/passwd 문자열의 라인의 처음)
					   root$ (# grep 'bash$' /etc/passwd 문자열 라인의 마지막)
					   [abc]=[a-c] (# grep 'user0[123]' /etc/passwd 대괄호에 포함된 문자 중 한개와 매치)
					   [^a] (a만 아니면 됨)
					   
		CMD | grep root
			# cat /etc/passwd | grep root 
			# rpm -qa | grep httpd 
			# ps -ef | grep rsyslogd 
			# systemctl list-unit-files | grep ssh 
			# netstat -antup | grep :22 
			
			[실무예] 로그 파일 점검 스크립트
				# alias chklog="cat $1 | egrep -i --color '(warn|error|fail|crit|alert|emerg)'"
				# chklog.sh /root/var/log/messages
				---------------------------------------
				#!/bin/bash

					RE1=$(date +'%b')
					RE2=$(date +'%d')
					if [ $RE2 -le 9 ] ; then
						RE2=$(echo $RE2 | cut -c2-)
						RE2=" $RE2"
					fi
				cat $1 | egrep "$RE1 $RE2" | egrep -i --color 'warn|errror|fail|crit|alert|emerg' 
				---------------------------------------
	find CMD
	
		● 파일 이름 검색		(예: # find / -name core -type f)
		● 소유자/그룹 검색	(예: # find / -user fedora -group fedora)
		● 수정 시간 검색		(예: # find / -mtime -7|7|+7)
		● 크기 검색			(예: # find / -size -300M|300M|+300M)
		● 퍼미션 검색		(예: # find / -perm -755|755)
		● 다른 명령어와 연결	(예: # find / -name core -type f -exec CMD {} \;)
		(+ = 초과 / - = 미만)
		
		[실무예] 갑자기 파일시스템 풀(FULL) 나는 경우
			df CMD + find CMD + lsof CMD
			# find /var -mtime -2 -size +1G -type f
		
		[실무예] 오래된 로그 파일 삭제
			# find /Log_Dir1 -name "*.log" -type f -mtime +30 -exec rm -f {} \;
		
		[실무예] 에러 메세지 검색
			(소스 코드가 존재하는 경우) # find /src -type f -exec grep -l 'Server Error' {} \;
								 # grep -r 'Server Error' /src
			(소스 코드가 없는 경우)
				site:redhat.com "Server Error"
				site:redhat.com "Server Error" AND "vmware" AND "Windows 10"
				가상화 .pdf
	
11장 압축과 아카이빙

	압축(Compress)
		gzip/gunzip CMD
			# gzip file1 (압축)
			# gunzip -c file1.gz (내용확인)
			# gzip -d file1.gz(해제)
			# gunzip file1.gz(해제)
			
		bzip2/bunzip2 CMD
			# bzip2 file1
			# bunzip2 -c file1.bz2(내용확인)
			# bunzip2 file1.bz
			
		xz/unxz CMD
			# yum -y install xz(설치)
			# xz file1
			# unxz -c file1.xz(내용확인)
			# unxz file1.xz
			
	아카이빙(Archive)
		tar CMD
			# tar cvf file.tar file1 file2 file3 (아카이빙)
			# tar tvf file.tar (내용 확인)
			# tar xvf file.tar (해제)
	압축+아카이빙
		tar CMD
		
			gzip+tar
				# tar cvzf file.tar.gz file1 file2 file3
				# tar tvzf file.tar.gz
				# tar xvzf file.tar.gz
			bz2+tar
				# tar cvjf file.tar.bz2 file1 file2 file3
				# tar tvjf file.tar.bz2
				# tar xvjf file.tar.bz2
			xz+tar
				# tar cvJf file.tar.xz file1 file2 file3
				# tar tvJf file.tar.xz
				# tar xvJf file.tar.xz
			jar
				# jar cvf file.jar.gz file1 file2 file3
				# jar tvf file.jar.gz
				# jar xvf file.jar.gz
		
		zip/unzip
			# zip file.zip file1 file2 file3
			# unzip -l file.zip
			# unzip file.zip

13장 쉘의 특성

	----------------------------------
		0 stdin (Keyboard)
		1 stout (Screen)
		2 stderr (Screen)
	----------------------------------
	
	리다이렉션(Redirection)
		입력 리디렉션(Redirection stdin) # wall < /etc/MESS/work.txt
		출력 리디렉션(Redirection stdout) # ls -l > lsfile.txt
		에러 리디렉션(Redirection sterr) # ls -l /test /nodir > errorfile.txt
	
		[실무예] 스크립트 로그 파일 생성
			# ./script.sh > script.log 2>&1
			
		[실무예] 출력 내용이 긴 명령 수행시 출력 화면 분석
			# ./configure > config.log 2>&1
			
		[실무예] 일반사용자가 명령 수행시 에러메세지를 지우는 경우
			$ find / -name core -type f 2>/dev/null
		
	파이프
		CMD | more
		CMD | grep CMD
		CMD | CMD | ...
		
		[실무예] 모니터링 구문 + 데이터 수집
			while true
				do
					ps -ef | grep httpd | wc -1 | tee -a httpd.cnt
					sleep 2
				done
		[실무예] 여러 터미널 화면을 공유하는 경우
			# script -a /dev/null | tee /dev/pts/1 | tee /dev/pts/2
	배시쉘 기능
		# set -o
		# set -o vi (기능 ON)
		# set +o vi (기능 OFF)
		
		set -o ignoreeof
		set -o noclobber
	<TAB>
		* 파일이름 자동완성 기능
		* 디렉토리 안에 파일 목록 보기
		* 명령어 검색 하기
	
	화살표
		*이전에 수행된 명령어를 편집해서 사용하기
		*이전에 수행된 명령어를 확장해서 사용하기
		*확인 + 명령수행 + 확인
		
	copy & paste 

	변수
		변수의 종류
			*지역변수 (Local Variable)
			*환경변수 (Enviroment Variable)
			*특수변수 (Special Variable) 	$$ 현재쉘의 PID
										$? 바로 이전에 수행된 명령어가 정상종료 되었다면 ? 변수에는 0이 들어있고 비정상적인 경우에는 0이 아닌 값이 들어있다
										$! 바로 이전에 백그라운드로 실행한 프로세스의 PID번호
										$0 프로그램 이름
										$1 프로그램 첫번째 인자(argument)
										$2 프로그램 두번째 인자
										$# 인자 전체($1, $2, $3, ...)
										$* 인자 개수(num($1, $2, $3, ...))

		변수 선언 방법
			# var=5 <- 지역 변수 선언
			# export var <- 환경변수 선언
			# echo $var 
			# unset var <- 변수 해제
		■ set : 모든 변수에 대해 출력 (지역변수 + 환경변수) set | grep var
		■ env : 환경변수만 출력 env | grep var
		시스템/쉘 환경변수
			PS1 변수: export PS1='[\u@\h \w]$ ' ($home/.bashrc)
			PS2 변수
			PATH 변수 : export PATH=$PAHT:/root/script ($HOME/.bash_profile)
			HISTTIMEFORMAT : export HISTTIMEFORMAT="%F %T     " (# vi /etc/profile )
			HOME 변수
			PWD 변수
			LOGNAME 변수
			USER 변수
			UID 변수
			TERM 변수 : export TERM=vt100
			LANG 변수 : export LANG=ko_KR.UTF-8(임시언어 변경)
					   localectl set-locale LANG=ko_KR.UTF-8 ; reboot(영구변경)
					   locale -a (언어 검색)
	메타캐릭터
		'', "", ``, \, ;
		
	명령어 히스토리 (Command History)
		histsize=1000 <- 히스토리 스택 사이즈 
		histfile $HOME/.bash_history <- 히스토리 저장되는 이름
		histfilesize=1000
	
	엘리어스
		(선언) # alias cp='cp -i'
		(확인) # alias   (# alias cp)
		(해제) # unalias cp 
	환경파일
		/etc/profile <- 모든 사용자에게 영향 (로그인시 읽힘)
		
		$HOME/.bash_profile <- 해당 유저에게 영향(로그인시 읽힘)
		
		$HOME/.bashrc <- 유저가 쉘을 실행할때 영향(쉘이 실행 될때 마다 읽힘)

	/etc/profile
		/etc/profile/d/{*.sh,sh.local}
		/etc/bashrc
			/etc/profile.d/*.sh
	~/.bash_profile
		~/.bashrc
			/etc/bashrc
				/etc/profile.d/*.sh
	~/.bashrc
		/etc/bashrc
			/etc/profile.d/*.sh

14장 프로세스 관리
	
	프로세스 정보
		/proc/PID/*
		- PID, PPID
	프로세스 관리
		프로세스 관리1
			프로세스 실행
				fg(forground) # gedit
				bg(background) # gedit &
			프로세스 확인
				# ps -ef | grep sshd
					(e : 모든 프로세스 리스트를 출력한다,
					 f : 프로세스 시작시간, 프로세스의 부모 ID, 그 프로세스에 관련된 사용자 ID, 
						 명령 이름과 가능한 매개변수등 모든 정보를 출력한다. )
				# ps aux | grep sshd 
					(a : 다른 사용자의 프로세스 상태도 표시, 
					 x : 화면에 보이지 않는 프로세스까지 모두 표시, 
					 u : 프로세스를 사용한 사용자와 실행 시간까지 표시)
			프로세스 종료
				# kill (-1, -2, -9, -15) PID PID ... 
					시그널번호를 생략시 기본이 -15 PID는 여러개 가능
					-2은 <CTRL + C>와 같은 기능
					-1은 설정을 바꾸거나 해서 프로그램 재실행이 필요할때 씀
					
				[참고] killall CMD, pkill CMD
					kill 은 PID를 써야하고
					killall/pkill은 프로그램 이름을 쓰면 된다.
		프로세스(잡, job) 관리2
			잡 실행
				fg) # gedit
				bg) # gedit &
			잡 확인
				# jobs
			잡 이동 	
				# fg %1 
				# bg %1
				<CTRL + Z> fg잡을 일시정지
			잡 종료
				# kill %1
				
	프로세스 모니터링
		★★top CMD 
			# top
			# top -u user (특정 유저만도 가능) 
			해석하는것도 중요
		[참고] 모니터링 툴
			htop (CPU/MEM)
			iotop (DISK I/O)
			iftop (Network I/O )
			atop (data gathering가능)
			설치하는 방법
				# yum install epel-release  /* fedora project */
				# yum install yum-utils      /* yum-config-manager CMD */
				# yum install htop iftop atop iotop
				
			gnome-system-monitor (GUI방식)
		
	프로세스 모니터링 관련 명령어
		lsof CMD (프로세스가 열고 있는 모든 파일을 보여줌)
			# lsof 
				# lsof /usr/sbin/sshd (데몬)
				# lsof /tmp (디렉토리)
				# lsof /etc/passwd (파일)
			# lsof -c sshd (자세하게 보여줌)
				# lsof /usr/sbin/sshd (거의같은뜻)
			# lsof -p PID
			# lsof -i
	
	pmap PID (프로세스가 사용하고 있는 메모리의 주소를 확인)
		
	pstree CMD
		# pstree
		# pstree user01
		★★# pstree -alup PID
	
	nice/renice CMD 
		# nice -n (-20 ~ 19) (숫자가 작을 수록 우선순위는 높아짐)
		# nice sleep 600 & (= nice -n 10 sleep 600 &) (숫자 생략시 기본값은 10)
		
		(실무 예) nice, renice 사용 예
			(백업스크립트/데이트 수집 스크립트)
				(X) # /root/bin/backup.sh & 
				(0) # nice -n 10 /root/bin/backup.sh & (# nice /root/bin/backup.sh &)
		
			(부하량을 주는 프로그램)
				# top 
				# renice -n 10 PID  (PID : 부하량을 주는 프로그램's PID)

			(주의) 서버의 목적인 프로세스는 프로세스의 우선순위를 조정하지 않는다.

15장 원격 접속과 파일 전송

	파일 전송
		scp CMD
			# scp file1 server2:/tmp/file2
			# scp server2:/tmp/file2 /test/file1
			# scp -r dir1 server2:/tmp
		
		sftp CMD
	
	원격 접속
		ssh CMD
			# ssh server2
			# ssh server2 CMD
			
			[참고] 공개키 인증 (public key authentication)
				# ssh-keygen -t rsa (암호화 방식 생략시 –t rsa가 기본값) (퍼블릭/공개 키 생성)
					[-t dsa | ecdsa | ds25519 | rsa]
				# ssh-copy-id -i id_rsa.pub root@server2 (공개기 배포)
				
		telnet CMD
