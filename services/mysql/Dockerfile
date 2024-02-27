FROM mysql:8.0

# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN chmod  -R 777 /var/lib/mysql
RUN mkdir -p /var/log/mysql-logs && chmod 777 /var/log/mysql-logs