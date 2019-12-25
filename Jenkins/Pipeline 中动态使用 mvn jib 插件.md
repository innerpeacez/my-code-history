```shell
mvn compile -DskipTests com.google.cloud.tools:jib-maven-plugin:1.8.0:build \
-Djib.from.image=registry-hz.rubikstack.com/library/java:8 \
-Djib.to.image=registry-hz.rubikstack.com/lichen/${args.name} \
-Djib.to.tags=${timeUtils.imageTagByTimeStamp()}-${BUILD_NUMBER} \
-Djib.container.appRoot=/app \
-Djib.container.useCurrentTimestamp=true \
-Djib.container.workingDirectory=/app
```

