Ниже представлено описание представленных файлов

passwd - копия /etc/passwd
shadow - копия /etc/shadow
group - копия /etc/group
suid-files.out - вывод всех файлов с битом set-UID
monitoring.out - вывод мониторинга процессов: процессы с euid=0 и реальным UID обычного пользователя
stat.out - вывод stat для /home/user1 и для утилит testcat (set-UID) и testchown (capabilities)
getcap.out - вывод getcap /home/user1/mychown 
sudoers - копия /etc/sudoers
user1-time - копия правила из /etc/sudoers.d/: user1 разрешено запускать от root date и timedatectl для изменения системного времени
history.out - история команд пользователя fedor
history_user1.out - история команд пользователя user1


Было выполнено следующее:

1. Был создан user1 (UID 1234) в группе students, пароль меняется каждые 90 дней с помощью команды useradd -m -u 1234 -G students user1, chage -M 90 user1
2.1 Найдены все файлы с set-UID с помощью команды find / -type f -perm -4000
2.2 Найдены процессы с euid=0 и ruid обычного пользователя (passwd, fusermount3) с помощью команды ps -eo euid,ruid,pid,user,comm \| awk '$1==0 && $2>=1000'` 
3. Копия cat с битом set-UID читает /etc/shadow под user1 с помощью команды chmod u+s /home/user1/mycat
4. Копия chown с CAP_CHOWN меняет владельца файла под user1 с помощью setcap cap_chown+ep /home/user1/mychown
5. user1 может менять системное время /etc/sudoers.d/newtime: user1 ALL=(root) /usr/bin/date, /usr/bin/timedatectl
