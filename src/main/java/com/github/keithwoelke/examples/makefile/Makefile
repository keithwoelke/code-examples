reportDirectory= target/test-output

dockerCmd = docker ps --all --quiet --filter name=service-test-container
testsExited = 0
testsRunning = 0

serviceContainer = service-container
serviceContainerTests = service-container-tests
serviceStubs = wiremock-stubs

prepare: up
	# prepare steps

up:
	docker-compose --project-name $(service-container) up -d

down:
	-docker stop $(serviceContainerTests)
	-docker rm $(serviceContainerTests)
	docker-compose down

build:
	docker build . -t $(serviceContainerTests)

test: up build
	set -e ;\
	if [[ -z `sh -c "$(dockerCmd) --filter status=exited"` ]] ;\
	then \
	  testsExited=0 ;\
	else \
	  testsExited=1 ;\
	fi ;\
	\
	if [[ -z `sh -c "$(dockerCmd) --filter status=running"` ]] ;\
	then \
	  testsRunning=0 ;\
	else \
	  testsRunning=1 ;\
	fi ;\
	\
	if [[ $$testsExited -eq 1 ]] ;\
	then \
	  docker rm $(serviceContainerTests) ;\
	  testsExited=0 ;\
	fi ;\
	\
	if [[ $$testsExited -eq 0 && $$testsRunning -eq 0 ]] ;\
	then \
	   docker run --interactive --detach --name $(serviceContainerTests) --link $(serviceContainer):$(serviceContainer) --net $(serviceContainer)
	   _default $(serviceContainerTests) ;\
	fi ;

	-docker exec $(serviceContainerTests) mvn verify
	mkdir -p $(reportDirectory)
	docker cp $(serviceContainerTests):/usr/src/app/target/test-output/ target/

clean:
	-docker stop $(serviceContainerTests) $(serviceContainer) $(service) $(serviceStubs)
	-docker rm $(serviceContainerTests) $(serviceContainer) $(service) $(serviceStubs)