# Sample Weave Module

This module can be used as a template of how to create a module with DW files and then reference it from Studio.


## Notes

1. Always use namespaces for all your mappings or modules (in this example org/mule/commons)
2. Remember to change the groupId with your Anypoint Organization Id and the same on the distributionManagement part
### Steps 

#### Steps to adequate this project to work with your implementation

1. In your pom.xml you have to change the next markups content:

Here you have to put your Organization Id. You can find it in Anypoint Platform
```xml
<groupId>721a4a78-a1ca-4405-99cf-17cd285ebe57</groupId>
```

Here you have to change your Organization Id in the URL markup.
Also you need to change the id markup to a value you are comfortable with.
```xml
	<distributionManagement>
        <repository>
            <id>Exchange-imanol</id>
            <name>Exchange</name>
        	<url>https://maven.anypoint.mulesoft.com/api/v2/organizations/721a4a78-a1ca-4405-99cf-17cd285ebe57/maven</url>
        	<layout>default</layout>
        </repository>
    </distributionManagement>
```

You can change the name of the project to whatever you like.
```xml
<name>string-utils-weave-module</name>
```

2. Before you deploy, you need to add configurations to your maven settings.
find your settings.xml file and add the next configuration:
in the servers section, you add the next code:
```xml
	<server>
		<id>(here you put the distribution management repository id)</id>
    	<username>(here you put your username for Anypoint Platform account)</username>
    	<password>(here you put your password for Anypoint Platform account)</password>
    </server>
```

3. After adding all your dataweave files, you need to build and deploy the project with maven.
You go to the folder of your project, and run the next command:

```terminal
mvn clean deploy
```

If it fails, the most common problem is that you have to change the version of the project.

4. After you run in and get the build success message, you get the last line of the console that says "Uploaded to (your distribution management repository id)".
You open the link in an explorer and you will need the metadata for the next step.

#### Steps to add the library to a project

1. In your pom.xml add this. You get the next info from the metadata link.
```xml
<dependency>
     <groupId>(the one that appears in the metadata as groupId)</groupId>
     <artifactId>(the one that appears in the metadata as artifactId)</artifactId>
     <version>(you can choose any version from the versions markup)</version>
</dependency>
```

2. You need to add a new repository in the pom.xml file:
```xml
<repository>
 	(here you put the same info as the distribution management markup in your library)
</repository>
```

3. Then in the console, on the folder of the project, you run the next command in order to download and install your library:
```terminal
mvn clean install
```

And then you can reference it 
```weave
%dw 2.0
import * from org::mule::commons::StringUtils
output application/json
---
times("Hello", 2)
```