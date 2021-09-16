# Introduction
 Import self-signed Java certificates to avoid PKIX validation errors

 ## Background
 Many at times you have a maven repo using a self signed certificate. If the certificate or its signing CA certificate is not imported is not added to the `$JAVA_HOME/lib/security/cacerts` you will get a PKIX validation error. You need to use it, and have to make it `work`

 ## The Solutions

 ### Ignore PKIX Validation Errors
 Maven can be configured on the command like to ignore all SSL validation errors using `mvn clean install -Dmaven.wagon.http.ssl.insecure=true`. That's handy for once off, but will require configuring multiple areas to ignore the error. You might also want to know other PKIX error like certificate expiry

 ### Import the CA cert
 This is achieved by following steps;
 * `javac Installcert.java` - compile the Java class in this repository
 * Run `java InstallCert nexus.server.local` to generate `jssecacerts` file
 * The above command will check the certificates and prompt for the certificate to save. Repeat for all certificates you need. Its useful when you certificate has a root CA chain and all the certs are required to complete validation.
 * Copy the generated `jssecacerts` to `$JAVA_HOME/lib/security/`
 * Now you can run you maven commands

## Finally
While the use case is for maven, its useful for any related PKIX validation error and especially when you have to trust certificated issues by custom Root CA

## Reference
* InstallCert.java source code  - https://raw.githubusercontent.com/elerch/InstallCert/master/InstallCert.java
* https://liferay.dev/blogs/-/blogs/fixing-suncertpathbuilderexception-caused-by-maven-downloading-from-self-signed-repository - describing the whole process
* https://stackoverflow.com/a/60523393/67796 - Maven flag to ignore SSL issues when running Maven
