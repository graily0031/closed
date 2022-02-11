---
title: log4j -> log4j2 변경 중 발생한 이슈
date: "2022-02-03T18:55:00.000Z"
template: "blog"
draft: false
slug: "/blog/log4j_issue/"
category: "web"
description: "log4j 변경하는 과정에서 황당한 이슈를 경험했다"
---

최근에 일어난 log4j 보안 이슈로 기존의 `1.2 에서 2.17.0` 버전으로 마이그레이션을 진행하였다.   
xml 소스코드를 변경해야 했기에 기존 1버전의 xml를 버전에 맞게 변경하였는데...   
예를 들어 아래와 같은 코드로 변경했다고 가정했을 때   
(스프링 로깅은 콘솔에서, db 로깅은 tomcat의 logs 폴더에 저장하는 방식으로 진행했다.)

``` xml
<!-- log4j2.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
	<Appenders>
        <Console name="console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d [%t] %-5level %c(%M:%L) - %m%n" />
        </Console>
		<RollingFile name="db_file" fileName="../logs/AppLog.log"
			filePattern=".
            ./logs/AppLog%d{yyyy-MM-dd}.log">
			<PatternLayout pattern="%d{yyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />
		</RollingFile>
	</Appenders>
	<Loggers>
        <Logger name="org.springframework" level="info" additivity="false" >
            <AppenderRef ref="console" />
        </Logger>
         <Logger name="co.graily.db1" level="info" additivity="false" >
            <AppenderRef ref="db_file" />
        </Logger>
        <Logger name="co.graily.db2" level="info" additivity="false" >
            <AppenderRef ref="db_file" />
        </Logger>
		<Root level="info" additivity="false">
			<appender-ref ref="console" />
		</Root>
	</Loggers>
</Configuration>
```

서버를 재시작하여 테스트했을 때, 로그파일에 로깅을 저장하지 못하는 현상을 발견하였다.   
이에 두 가지 실험을 진행하였는데...
> 1. 기존 로그파일을 모두 지우고 서버 재시작
> 2. 로그파일 및 콘솔 출력   


1번 실험은 `파일은 생성되나 로깅 저장이 안되고`
2번 실험의 경우엔 `콘솔에서만 정상적으로 출력`되는 것을 확인했다.

``` xml
<RollingFile name="db_file" fileName="../logs/AppLog.log"
	filePattern="../logs/AppLog%d{yyyy-MM-dd}.log">
	<PatternLayout pattern="%d{yyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />
</RollingFile>
```

결론을 먼저 말하자면, `'RollingFile name'`에 포함된 `'_'`(언더바)의 문제인 것으로 확인하였다.
이전 버전인 1.2에선 정상적으로 작동했었기에 그대로 옮겨간 것이 문제인 듯 했다.   
왜 이 문자가 들어갈 시 로그 파일에 정상적으로 저장이 되지 않는지 이해되지 않았고 관련 정보도 찾을 수 없었다.   
(추후 이와 관련된 새로운 정보가 있다면 수정 할 것)   
해당 이슈때문에 반나절간 삽질한 내 자신에게 치얼스....😓