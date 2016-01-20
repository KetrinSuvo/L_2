# L_2

# Задание №2

Для утилиты lftp создать профиль apparmor, позволяющий ей исправно функционировать в рамках минимально необходимых и достаточных ограничений в операционной системе

 Результат представить в виде git-репозитария на github.com


# 1

cd  /etc/apparmor.d/
  ls
  which lftp
  aa-autodep /usr/bin/lftp
  sudo aa-autodep /usr/bin/lftp
  ls


  ketrin@ketrin-desktop:/etc/apparmor.d$ ls -l
итого 224
drwxr-xr-x 4 root root 4096 нояб. 30 22:26 abstractions
drwxr-xr-x 2 root root 4096 янв.   9 13:32 apache2.d
-rw-r--r-- 1 root root  795 июня  18  2015 bin.ping
drwxr-xr-x 2 root root 4096 янв.   9 13:32 cache
drwxr-xr-x 2 root root 4096 февр. 18  2015 disable
drwxr-xr-x 2 root root 4096 дек.   4  2013 force-complain
-rw-r--r-- 1 root root  357 дек.  11  2014 lightdm-guest-session
drwxr-xr-x 2 root root 4096 янв.   9 13:32 local
drwxr-xr-x 2 root root 4096 янв.   9 13:32 program-chunks
-rw-r--r-- 1 root root 2461 марта 30  2015 sbin.dhclient
-rw-r--r-- 1 root root  971 июня  18  2015 sbin.klogd
-rw-r--r-- 1 root root 1217 июня  18  2015 sbin.syslogd
-rw-r--r-- 1 root root 1570 июня  18  2015 sbin.syslog-ng
drwxr-xr-x 5 root root 4096 нояб. 30 22:24 tunables
-rw-r--r-- 1 root root 7597 июня  18  2015 usr.bin.chromium-browser
-rw-r--r-- 1 root root 5528 июня   4  2014 usr.bin.evince
-rw-r--r-- 1 root root 5106 янв.  26  2015 usr.bin.firefox
-rw------- 1 root root  141 янв.   9 13:36 usr.bin.lftp  <--- наш файл
-rw-r--r-- 1 root root  733 июня  18  2015 usr.lib.dovecot.anvil
-rw-r--r-- 1 root root 1204 июня  18  2015 usr.lib.dovecot.auth
-rw-r--r-- 1 root root  871 июня  18  2015 usr.lib.dovecot.config
-rw-r--r-- 1 root root 1153 июня  18  2015 usr.lib.dovecot.deliver
-rw-r--r-- 1 root root  887 июня  18  2015 usr.lib.dovecot.dict
-rw-r--r-- 1 root root 1056 июня  18  2015 usr.lib.dovecot.dovecot-auth
-rw-r--r-- 1 root root  954 июня  18  2015 usr.lib.dovecot.dovecot-lda
-rw-r--r-- 1 root root  971 июня  18  2015 usr.lib.dovecot.imap
-rw-r--r-- 1 root root 1008 июня  18  2015 usr.lib.dovecot.imap-login
-rw-r--r-- 1 root root  937 июня  18  2015 usr.lib.dovecot.lmtp
-rw-r--r-- 1 root root  680 июня  18  2015 usr.lib.dovecot.log
-rw-r--r-- 1 root root  953 июня  18  2015 usr.lib.dovecot.managesieve
-rw-r--r-- 1 root root 1109 июня  18  2015 usr.lib.dovecot.managesieve-login
-rw-r--r-- 1 root root  900 июня  18  2015 usr.lib.dovecot.pop3
-rw-r--r-- 1 root root  967 июня  18  2015 usr.lib.dovecot.pop3-login
-rw-r--r-- 1 root root  790 июня  18  2015 usr.lib.dovecot.ssl-params
-rw-r--r-- 1 root root 6904 апр.   7  2014 usr.lib.telepathy
-rw-r--r-- 1 root root  858 июня  18  2015 usr.sbin.avahi-daemon
-rw-r--r-- 1 root root  420 апр.   8  2014 usr.sbin.cups-browsed
-rw-r--r-- 1 root root 4490 сент.  6  2014 usr.sbin.cupsd
-rw-r--r-- 1 root root 2121 июня  18  2015 usr.sbin.dnsmasq
-rw-r--r-- 1 root root 2012 июня  18  2015 usr.sbin.dovecot
-rw-r--r-- 1 root root  946 июня  18  2015 usr.sbin.identd
-rw-r--r-- 1 root root  938 июня  18  2015 usr.sbin.mdnsd
-rw-r--r-- 1 root root  761 июня  18  2015 usr.sbin.nmbd
-rw-r--r-- 1 root root 1244 июня  18  2015 usr.sbin.nscd
-rw-r--r-- 1 root root 1394 апр.  25  2015 usr.sbin.rsyslogd
-rw-r--r-- 1 root root 1440 июня  18  2015 usr.sbin.smbd
-rw-r--r-- 1 root root 1418 янв.  13  2014 usr.sbin.tcpdump
-rw-r--r-- 1 root root  842 июня  18  2015 usr.sbin.traceroute


получили файл


ketrin@ketrin-desktop:/etc/apparmor.d$ sudo cat usr.bin.lftp
# Last Modified: Sat Jan  9 13:33:40 2016
#include <tunables/global>

/usr/bin/lftp {
  #include <abstractions/base>

  /usr/bin/lftp mr,

}

================================

создаем лог трейса работы программы:
sudo strace -f -e trace=open,access,execve -o 1_strace.log /usr/bin/lftp -u test 192.168.1.41

очищаем этот лог
cat 1_strace.log | cut -d"=" -f1 | cut -d"(" -f2 | sort -u > 2_strace_clear.log
получаем список того, что программе надо из системных вызовов

начинаем писать профиль ( используя заготовку)

=====================================
#include <tunables/global>
/usr/bin/lftp {
#include <abstractions/base>
#include <abstractions/nameservice>

	#разрешаем самого запуск
  	/usr/bin/lftp mr,

  	#разрешаем конфиги
  	/etc/lftp.conf rk,
  	/etc/inputrc r,
  	/etc/localtime r,
	/etc/terminfo/x/xterm r,

	#не даем сливать важную информацию
	deny @{HOME}/.ssh/ mrwkl,
	deny @{HOME}/.pki/* mrwkl,

	#в домашнем каталоге тонко нарезаем разрешения согласно логу strace	
	@{HOME}/.config/lftp/rc r,
	@{HOME}/.lftprc r,
	@{HOME}/.local/share/lftp/cwd_history rwk,
	@{HOME}/.local/share/lftp/rl_history rwk,
	@{HOME}/.local/share/lftp/transfer_log ra,
	@{HOME}/.netrc r,
	/lib/terminfo/x/xterm r,
	@{HOME}/[^.]* rw,
	@{HOME}/[^.]*/ rw,
	@{HOME}/[^.]** rw,
	@{HOME}/[^.]*/** rw,

	
}
==================================

после сохранения профиля, его необходимо подгрузить
sudo apparmor_parser -r usr.bin.lftp


