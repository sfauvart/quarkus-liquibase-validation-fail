# code-with-quarkus project

## Actual behavior

On native run I have this error :

```
__  ____  __  _____   ___  __ ____  ______
--/ __ \/ / / / _ | / _ \/ //_/ / / / __/
-/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/
2021-03-17 15:53:24,316 ERROR [io.qua.run.Application] (main) Failed to start application (with profile prod): liquibase.exception.ValidationFailedException: Validation Failed:
1 changes have validation failures
tableName is required, liquibase/changeLog.xml::1613578374533-3::sfauvart

	at liquibase.changelog.DatabaseChangeLog.validate(DatabaseChangeLog.java:299)
	at liquibase.changelog.DatabaseChangeLog.validate(DatabaseChangeLog.java:275)
	at liquibase.Liquibase.validate(Liquibase.java:2220)
	at io.quarkus.liquibase.runtime.LiquibaseRecorder.doStartActions(LiquibaseRecorder.java:49)
	at io.quarkus.deployment.steps.LiquibaseProcessor$startLiquibase763757012.deploy_0(LiquibaseProcessor$startLiquibase763757012.zig:67)
	at io.quarkus.deployment.steps.LiquibaseProcessor$startLiquibase763757012.deploy(LiquibaseProcessor$startLiquibase763757012.zig:40)
	at io.quarkus.runner.ApplicationImpl.doStart(ApplicationImpl.zig:629)
	at io.quarkus.runtime.Application.start(Application.java:90)
	at io.quarkus.runtime.ApplicationLifecycleManager.run(ApplicationLifecycleManager.java:100)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:66)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:42)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:119)
	at io.quarkus.runner.GeneratedMain.main(GeneratedMain.zig:29)
```

## Step to reproduce :

Start postgresql with docker :
```shell script
docker run --ulimit memlock=-1:-1 -it --rm=true --memory-swappiness=0 --name quarkusdb -e POSTGRES_USER=db_user -e POSTGRES_PASSWORD=asecretpassword -e POSTGRES_DB=cart -p 5432:5432 postgres:12.2
```

Install graalvm with sdkman, build project in native mode and run it :
```shell script
sdk install java 21.0.0.r11-grl
mvn clean package -Pnative
./target/code-with-quarkus-1.0.0-SNAPSHOT-runner
```

If you try with the classic JVM it's ok :
```shell script
./mvnw compile quarkus:dev
```
