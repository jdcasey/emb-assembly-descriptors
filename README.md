EMB Assembly Descriptors
========================

These descriptors are for use with the maven-assembly-plugin, in order to help users construct customized EMB distributions.

Here's a sample distribution POM:

    <?xml version='1.0' encoding='UTF-8'?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    
    [...]
  
      <dependencies>
        <dependency>
          <groupId>org.commonjava.emb</groupId>
          <artifactId>emb-distro-commons</artifactId>
          <version>${project.version}</version>
        </dependency>
        <dependency>
          <groupId>org.commonjava.emb.integration</groupId>
          <artifactId>emb-autonx-m3-resolver</artifactId>
          <version>${project.version}</version>
        </dependency>
      </dependencies>

      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-dependency-plugin</artifactId>
            <version>2.1</version>
            <executions>
              <execution>
                <id>unpack-dependencies</id>
                <phase>prepare-package</phase>
                <goals>
                  <goal>unpack-dependencies</goal>
                </goals>
                <configuration>
                  <includeArtifactIds>emb-distro-commons</includeArtifactIds>
                  <outputDirectory>${project.build.directory}/commons</outputDirectory>
                </configuration>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-assembly-plugin</artifactId>
            <dependencies>
              <dependency>
                <groupId>org.commonjava.emb</groupId>
                <artifactId>emb-assembly-descriptors</artifactId>
                <version>${project.version}</version>
              </dependency>
            </dependencies>
            <executions>
              <execution>
                <id>create-distro</id>
                <phase>package</phase>
                <goals>
                  <goal>single</goal>
                </goals>
                <configuration>
                  <descriptorRefs>
                    <descriptorRef>emb-distro</descriptorRef>
                  </descriptorRefs>
                </configuration>
              </execution>
            </executions>
          </plugin>
        </plugins>
      </build>
    </project>

In the above example, there are two dependencies: `emb-distro-commons` and `emb-autonx-m3-resolver`. `emb-distro-commons` provides a skeletal distribution, so it's unpacked into the `target/commons` directory for the assembly plugin to pickup and use. The `emb-autonx-m3-resolver` dependency is a custom EMB extension that enables auto-mirroring based on discovered Nexus instances. This additional dependency is managed by the assembly plugin, using the `emb-distro` descriptor.
