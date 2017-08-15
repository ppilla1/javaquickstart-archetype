# cd ${project.basedir}
# Edit settings.xml below entry under "servers" tag
		<server>
			<id>github</id>
			<username> ${github loginId} </username>
			<password> ${github password} </password>
		</server>

# mvn -s <settings.xml location> archetype:create-from-project
# cd ${project.build.directory}/generated-sources/archetype
# Edit pom.xml with below entries

      <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven-compiler-plugin.version>3.6.2</maven-compiler-plugin.version>
        <maven-surefire-plugin.version>2.20</maven-surefire-plugin.version>
        <maven-archetype-plugin.version>3.0.1</maven-archetype-plugin.version>
        <maven-release-plugin.version>2.5.2</maven-release-plugin.version>
        <archetype-packaging.version>3.0.1</archetype-packaging.version>
        <github-site-plugin.version>0.12</github-site-plugin.version>
        <java.version>1.8</java.version>

        <github.global.server>github</github.global.server>
        <github.username>ppilla1</github.username>
        <github.reponame>javaquickstart-archetype-repo</github.reponame>
        <github.outputdir>E:/tmp/archetype/</github.outputdir>
      </properties>

      <distributionManagement>
        <repository>
          <id>repo</id>
          <url>https://github.com/<github username>/<github repository name>/raw/master</url>
        </repository>
        <snapshotRepository>
          <id>snapshot-repo</id>
          <url>https://github.com/<github username>/<github repository name>/raw/master</url>
        </snapshotRepository>
      </distributionManagement>

....and under "build" tag

        <plugins>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-release-plugin</artifactId>
            <version>${maven-release-plugin.version}</version>
          </plugin>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>${maven-compiler-plugin.version}</version>
            <configuration>
              <source>${java.version}</source>
              <target>${java.version}</target>
            </configuration>
          </plugin>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>${maven-surefire-plugin.version}</version>
            <dependencies>
              <dependency>
                <groupId>org.apache.maven.surefire</groupId>
                <artifactId>surefire-junit47</artifactId>
                <version>${maven-surefire-plugin.version}</version>
              </dependency>
            </dependencies>
          </plugin>
          <plugin>
            <groupId>com.github.github</groupId>
            <artifactId>site-maven-plugin</artifactId>
            <version>${github-site-plugin.version}</version>
            <configuration>
              <server>github</server>
              <outputDirectory>${github.outputdir}</outputDirectory>
              <includes>
                <include>**/*</include>
              </includes>
              <repositoryOwner>${github.username}</repositoryOwner>
              <repositoryName>${github.reponame}</repositoryName>
              <branch>refs/heads/master</branch>
              <message>Deploy artifacts for ${project.version}</message>
            </configuration>
            <executions>
              <execution>
                <goals>
                  <goal>site</goal>
                </goals>
                <phase>deploy</phase>
              </execution>
            </executions>
          </plugin>
        </plugins>

# cd ${project.build.directory}/generated-sources/archetype/src/main/resources/archetype-resources
# Edit pom.xml with below entries

    <version>x.y</version>

    ....and under "build" tag

    <finalName>${artifactId}-${version}</finalName>
# cd ${project.build.directory}/generated-sources/archetype
# mvn -s <settings.xml location> -DaltDeploymentRepository=snapshot-repo::default::file:<what ever value for ${github.outputdir}> clean deploy

## To used archetype give below catalog location

https://github.com/<what every value ${github.username}>/<what every value ${github.reponame}>/tree/master/<archetype ${groupId}>/<archetype ${artifactId}>