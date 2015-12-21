---
layout: post

edition: first

title: Adventures in Tycho
subtitle: Creating a maker friendly DIY IoT home automation solution

author:
  name: BITtailor
  twitter: bittailor_ch
  image: avatar.png
  bio: passion for the bits and bytes
---
[Kura]: https://eclipse.org/kura/
[Hello World Example]: http://eclipse.github.io/kura/doc/hello-example.html
[Getting Started]: http://eclipse.github.io/kura/doc/kura-setup.html
[RPi]: http://www.raspberrypi.org/
[Raspberry Pi Quick Start]: http://eclipse.github.io/kura/doc/raspberry-pi-quick-start.html
[nRF24]: https://www.nordicsemi.com/eng/Products/2.4GHz-RF/nRF24L01P
[Tycho]: https://eclipse.org/tycho/
[OSGI]: http://www.osgi.org/Main/HomePage
[Java]: http://www.oracle.com/technetwork/java/index.html
[Maven]: http://maven.apache.org/
[Continuous Integration]: http://www.martinfowler.com/articles/continuousIntegration.html
[mockito]: http://mockito.org/

To build my OSGI bundels that implement the MQTT-SN gateway I went with [Tycho][Tycho] since I saw that Kura uses Tycho and also the [kura-greenhouse-demo](https://github.com/kartben/kura-greenhouse-demo) uses Tycho as build system. I expected that the Tycho build system is quite similar to *regular* maven build system, but from my point of view (or lack of knowlege) Tycho is quite different than what I was used to from maven.

<!-- more -->
Besides compiling the sources, I expected that it would be straight forward to:

1. define the build, runtime and test dependencies of a bundle. Then the build system will take care of fetching and caching them from binary repositories, so that there is no need to add binaries the the source code repository.
2. having unit tests for a bundle which will be run by the build system.

### Lets start with the **#2**
With Tycho it is not possible to put your unit tests under <small><tt>src/test/java</tt></small> so I searched and found that one way is to create a [plug-in fragment](http://www.vogella.com/tutorials/EclipseFragmentProject/article.html) and use the **eclipse-test-plugin** as the Maven packaging. Unfortunately this did not what I was expecting for unit tests. The **eclipse-test-plugin** launches a complete OSGI environment, which, in my opinion, isn't unit testing but rather integration testing. But my problem was that my bundle depends on the jdk.dio bundle which is only available on the target and not on the development machine and so the OSGI resolving failed. Therefore I could not run **eclipse-test-plugin** tests on my development machine. I tried various alternatives and settled with the following:

I still create a seperate plug-in fragment but use **eclipse-plugin** as the Maven packaging. I put all the unit tests into the <small><tt>src/main/java</tt></small> folder of this plug-in fragment project and then add the **maven-surefire-plugin** to the POM of the plug-in fragment and configure the regular output directory as testClassesDirectory. You also have to add a dependency to junit 4 to the POM so that the **maven-surefire-plugin** knows how to launch the tests;

```markup
<project>
  <modelVersion>4.0.0</modelVersion>
  <parent>
	  <groupId>ch.bittailor.iot</groupId>
 	  <artifactId>ch.bittailor.iot.parent</artifactId>
  	<version>1.0.0-SNAPSHOT</version>
    <relativePath>../pom.xml</relativePath>
  </parent>

  <artifactId>ch.bittailor.iot.core.test</artifactId>
  <packaging>eclipse-plugin</packaging>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.18.1</version>
        <executions>
          <execution>
            <id>test</id>
            <phase>test</phase>
            <configuration>
              <testClassesDirectory>${project.build.outputDirectory}</testClassesDirectory>
            </configuration>
            <goals>
              <goal>test</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>

  <dependencies>
  	<dependency>
  		<groupId>junit</groupId>
  		<artifactId>junit</artifactId>
  		<version>4.12</version>
  		<scope>test</scope>
  	</dependency>
  </dependencies>
</project>
```  

This now launches my tests as simple unit tests during a **mvn clean verify**. So the next thing a wanted was to have [mockito][mockito] available as mocking framework in my unit tests. With Tycho it is not possible to just add it as test dependency to the POM since Tycho uses a target-platform to resolve all its dependencies.

### Back to **#1**, how to manage dependencies with Tycho
I'm still struggeling with how to manage the dependencies with Tycho. As far as I understood it, Tycho uses the target platform to resolve all its dependencies. But I still could not find a way to define the the target platform for my needs (Kura bundles and mockito for testing) without adding binaries to the source code repository. I again [asked](https://www.eclipse.org/forums/index.php/t/1028830/) the Kura discussion forum for help. The current answer state is to write a "first-time-setup" script. But since I'm running out of time I focused on the implementation of the MQTT-SN gateway. So my source code is currently not buildable out of the source code repository but needs a handcrafted (kura) target platform on the development machine.

I recognized that also the [kura-greenhouse-demo](https://github.com/kartben/kura-greenhouse-demo) does not build out of the source code repository since it also relies on a kura target platform already present on the development machine. So my code is in good company <i class="fa fa-smile-o"></i>.

Nevertheless I'm still eagerly interested in a solution to manage the kura dependencies/target-platform with Tycho without adding binaries into the source code repository. So that my maven experience of just doing **git clone && mvn clean verify** also holds with Tycho. So please comment here or in the Kura discussion forum [topic](https://www.eclipse.org/forums/index.php/t/1028830/) if you have some insights.
