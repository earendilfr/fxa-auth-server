SERVER_URL = https://api-accounts.stage.mozaws.net

.PHONY: build test bench

# Build virtualenv, to ensure we have all the dependencies.
build:
	virtualenv --no-site-packages .
	./bin/pip install gevent
	./bin/pip install pexpect
	./bin/pip install https://github.com/mozilla-services/loads/archive/master.zip
	./bin/pip install 'hawkauthlib'
	rm -rf ./local  # ubuntu, why you create this useless folder?

# Clean all the things installed by `make build`.
clean:
	rm -rf ./include ./bin ./lib ./lib64 *.pyc

# Run a single test from the local machine, for sanity-checking.
test:
	./bin/loads-runner --config=./config/test.ini --server-url=$(SERVER_URL) loadtests.LoadTest.test_auth_server

# Run a fuller bench suite from the local machine.
bench:
	./bin/loads-runner --config=./config/bench.ini --server-url=$(SERVER_URL) loadtests.LoadTest.test_auth_server

# Run a full bench, by submitting to broker in AWS.
megabench:
	./bin/loads-runner --config=./config/megabench.ini --user-id=$(USER) --server-url=$(SERVER_URL) loadtests.LoadTest.test_auth_server

# Purge any currently-running loadtest runs.
purge:
	./bin/loads-runner --config=./config/megabench.ini --purge-broker
