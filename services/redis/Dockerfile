FROM redis:7.0.8

# 设置时区为上海
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

COPY redis.conf /etc/redis/redis.conf
CMD [ "redis-server", "/etc/redis/redis.conf" ]
