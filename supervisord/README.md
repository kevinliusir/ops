# ops

# tomcat
## 修改catlina.sh ##
增加|  /usr/sbin/cronolog "$CATALINA_OUT" >> /dev/null
```ruby
303 elif [ "$1" = "run" ]; then
304 
305   shift
306   if [ "$1" = "-security" ] ; then
307     if [ $have_tty -eq 1 ]; then
308       echo "Using Security Manager"
309     fi
310     shift
311     eval exec "\"$_RUNJAVA\"" "\"$LOGGING_CONFIG\"" $LOGGING_MANAGER $JAVA_OPTS $CATALINA_OPTS \
312       -Djava.endorsed.dirs="\"$JAVA_ENDORSED_DIRS\"" -classpath "\"$CLASSPATH\"" \
313       -Djava.security.manager \
314       -Djava.security.policy=="\"$CATALINA_BASE/conf/catalina.policy\"" \
315       -Dcatalina.base="\"$CATALINA_BASE\"" \
316       -Dcatalina.home="\"$CATALINA_HOME\"" \
317       -Djava.io.tmpdir="\"$CATALINA_TMPDIR\"" \
318       org.apache.catalina.startup.Bootstrap "$@" start
319   else
320     eval exec "\"$_RUNJAVA\"" "\"$LOGGING_CONFIG\"" $LOGGING_MANAGER $JAVA_OPTS $CATALINA_OPTS \
321       -Djava.endorsed.dirs="\"$JAVA_ENDORSED_DIRS\"" -classpath "\"$CLASSPATH\"" \
322       -Dcatalina.base="\"$CATALINA_BASE\"" \
323       -Dcatalina.home="\"$CATALINA_HOME\"" \
324       -Djava.io.tmpdir="\"$CATALINA_TMPDIR\"" \
325       org.apache.catalina.startup.Bootstrap "$@" start |  /usr/sbin/cronolog "$CATALINA_OUT" >> /dev/null
326   fi
327 
```
```bash 
[program:tomcat_oh]
command=/usr/local/tomcat_oh/bin/catalina.sh run
directory=/usr/local/tomcat_oh ; first change to the dir and exec the command
autostart = true
startsecs = 5
autorestart = true
startretries = 3
stopasgroup=true ;默认为false,进程被杀死时，是否向这个进程组发送stop信号，包括子进程
killasgroup=true ;默认为false，向进程组发送kill信号，包括子进程
redirect_stderr=true ; 把stderr重定向到stdout，默认false
;stdout_logfile_maxbytes=50MB  ; stdout 日志文件大小，默认50MB
;stdout_logfile_backups = 10   ; stdout 日志文件备份数，默认是10
; stdout 日志文件，需要注意当指定目录不存在时无法正常启动，所以需要手动创建目录（supervisord 会自动创建日志文件）
;stdout_logfile= /usr/local/tomcat_oh/logs/catalina.out
stdout_logfile=syslog
stderr_logfile=syslog
;redirect_stderr=true        ;redirect the error log to stdout
;stdout_logfile = /mnt/logs/ohdubbo_front_base/oh_front_base.log
;loglevel=info



```
