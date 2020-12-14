[![](https://img.shields.io/badge/tag-authzforce-orange.svg?logo=stackoverflow)](http://stackoverflow.com/questions/tagged/authzforce)
[![Docker badge](https://img.shields.io/docker/pulls/authzforce/restful-pdp.svg)](https://hub.docker.com/r/authzforce/restful-pdp/)

# AuthzForce RESTful PDP
RESTful PDP API implementation, compliant with REST Profile of XACML 3.0. This is minimalist compared to [AuthzForce server project](http://github.com/authzforce/server) as it does not provide multi-tenant PDP/PAP but only a single PDP (per instance). Therefore, this is more suitable for microservices, or, more generally, simple applications requiring only one PDP per instance.

In particular, the project provides the following (Maven groupId:artifactId):
* `org.ow2.authzforce:authzforce-ce-restful-pdp-cxf-spring-boot-server`: a fully executable RESTful XACML PDP server (runnable from the command-line), packaged as a [Spring Boot application](https://docs.spring.io/spring-boot/docs/current/reference/html/deployment-install.html) or [Docker image](https://hub.docker.com/repository/docker/authzforce/restful-pdp) (see the [Docker Compose example](docker) for usage).
* `org.ow2.authzforce:authzforce-ce-restful-pdp-jaxrs`: pure JAX-RS implementation of a PDP service, that you can reuse as a library with any JAX-RS framework, especially other than Apache CXF, to provide your own custom RESTful PDP service.

**Go to the [releases](https://github.com/authzforce/restful-pdp/releases) page for
specific release info: downloads (Linux packages), Docker image,
[release notes](CHANGELOG.md)**

## Features
### XACML PDP engine
See [AuthzForce Core features](https://github.com/authzforce/core#features) for the XACML PDP engine's features.

### REST API
* Conformance with [REST Profile of XACML v3.0 Version 1.0](http://docs.oasis-open.org/xacml/xacml-rest/v1.0/xacml-rest-v1.0.html)
* Supported data formats, aka content types: 
	
	* `application/xacml+xml`: XACML 3.0/XML content, as defined by [RFC 7061](https://tools.ietf.org/html/rfc7061), for XACML Request/Response only;
	* `application/xml`: same as `application/xacml+xml`;
	* `application/xacml+json`: XACML 3.0/JSON Request/Response, as defined by [XACML v3.0 - JSON Profile Version 1.0](http://docs.oasis-open.org/xacml/xacml-json-http/v1.0/xacml-json-http-v1.0.html);
	* `application/json`: same as `application/xacml+json`.

## Limitations
See [AuthzForce Core limitations](https://github.com/authzforce/core#limitations).

## System requirements
Java (JRE) 8 or later.


## Versions
See the [change log](CHANGELOG.md) following the *Keep a CHANGELOG* [conventions](http://keepachangelog.com/).

## License
See the [license file](LICENSE).

## Getting started
Get the [latest executable jar](http://central.maven.org/maven2/org/ow2/authzforce/authzforce-ce-restful-pdp-cxf-spring-boot-server/) from Maven Central with groupId/artifactId = `org.ow2.authzforce`/`authzforce-ce-restful-pdp-cxf-spring-boot-server`. 

Make sure it is executable (replace `M.m.p` with the current version):

```sh
chmod u+x authzforce-ce-restful-pdp-cxf-spring-boot-server-M.m.p.jar
```

Copy the content of [that folder](cxf-spring-boot-server/src/test/resources/server) to the same directory and run the executable from that directory as follows (replace `M.m.p` with the current version):

```sh
$ ./authzforce-ce-restful-pdp-cxf-spring-boot-server-M.m.p.jar
```

If it refuses to start because the TCP listening port is already used (by some other server on the system), you can change that port in file `application.yml` copied previously: uncomment and change `server.port` property value to something else (default is 8080).

You know the embedded server is up and running when you see something like this (if and only if the logger for Spring classes is at least in INFO level, according to Logback configuration file mentioned down below) :
```
... Tomcat started on port(s): 8080 (http)
```

(You can change logging verbosity by modifying the Logback configuration file `logback.xml` copied previously.)

Now you can make a XACML request from a different terminal (install `curl` tool if you don't have it already on your system):

```sh
$ curl --include --header "Content-Type: application/xacml+json" --data @IIA001/Request.json http://localhost:8080/services/pdp
```
*Add --verbose option for more details.*
You should get a XACML/JSON response such as:

```
{"Response":[{"Decision":"Permit"}]}
```


## Extensions
If you are missing features in AuthzForce, you can extend it with various types of plugins (without changing the existing code), as described on AuthzForce Core's [wiki](https://github.com/authzforce/core/wiki/Extensions).

In order to use them, put the extension JAR(s) into an `extensions` folder in the same directory as the executable jar, already present if you followed the previous *Getting started* section. If the extension(s) use XML configuration (e.g. AttributeProvider), add the schema import into `pdp-ext.xsd` (import namespace only, do not specify schema location) and schema namespace-to-location mapping into `catalog.xml`. Then run the executable as follows (replace `M.m.p` with the current version):

```sh
$ java -Dloader.path=extensions -jar authzforce-ce-restful-pdp-cxf-spring-boot-server-M.m.p.jar
```

## Vulnerability reporting
If you want to report a vulnerability, you must do so on the [OW2 Issue Tracker](https://gitlab.ow2.org/authzforce/restful-pdp/issues) and when creating the issue, check the box labeled **"This issue is confidential and should only be visible to team members with at least Reporter access"**. Then, if the AuthzForce team can confirm it, they will make it public and set a fix version.

## Support
If you are experiencing any issue with this project except for vulnerabilities mentioned previously, please report it on the [GitHub Issue Tracker](https://github.com/authzforce/restful-pdp/issues).
Please include as much information as possible; the more we know, the better the chance of a quicker resolution:

* Software version
* Platform (OS and JDK)
* Stack traces generally really help! If in doubt include the whole thing; often exceptions get wrapped in other exceptions and the exception right near the bottom explains the actual error, not the first few lines at the top. It's very easy for us to skim-read past unnecessary parts of a stack trace.
* Log output can be useful too; sometimes enabling DEBUG logging can help;
* Your code & configuration files are often useful.

If you wish to contact the developers for other reasons, use [AuthzForce contact mailing list](http://scr.im/azteam).

## Contributing
See [CONTRIBUTING.md](CONTRIBUTING.md).
