# usecase in OS or Jenkins
In your Pom.xml file, you can include the following information:

```sh
<distributionManagement>
  <repository>
    <id>maven-releases</id>
    <!-- CHANGE HERE by your team Nexus server -->
    <url>http://13.126.124.159:8081/repository/maven-releases/</url>
  </repository>
  <snapshotRepository>
    <id>maven-snapshots</id>
    <!-- CHANGE HERE by your team Nexus server -->
    <url>http://13.126.124.159:8081/repository/maven-releases/</url>
  </snapshotRepository>
</distributionManagement>
```

```sh
<repository>
  <id>maven-releases</id>
  <name>maven-releases</name>
  <url>http://13.126.124.159:8081/repository/maven-releases/</url>
</repository>
```
Remember to replace the Nexus server URL ```(http://13.126.124.159:8081/repository/maven-releases/)``` with your actual Nexus server URL