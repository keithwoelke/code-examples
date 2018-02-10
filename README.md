This package contains code examples and samples.

It currently contains:

* POM file (with reference to parent POM file)
* Parent POM file
* .gitignore file
* logback.xml file with TestNGReportAppender for ReportNG reports
* Makefile (centered around docker commands)
* Dockerfile (copies source code and .m2 folder to container) 
* docker-compose file for tying together multiple test dependencies (with stubs)
* Basic Jenkinsfile (for checking out and building a Docker container)