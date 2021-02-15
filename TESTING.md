## Testing kaf-fleetshard-operator

### Running systemtest

#### Requirements
* kubectl/oc client installed
* connected to a running kubernetes/openshift cluster
* image of operator is built and stored in any container registry

### Information
* suite installs latest released strimzi operator
* suite installs CRD generated by kas-fleetshard-api
* suite by default gets examples of operator deployment from [here](kas-fleetshard-operator/src/main/kubernetes)
* suite replaces IMAGE in deployment.yml with default image path which is `localhost:5000/bf2/kas-fleetshard-operator:latest`, to override this default image registry run test suite with env var `FLEET_SHARD_OPERATOR_IMAGE=registry/org/operator-name:tag`
* if you want to use different deployment files run test suite with env var `YAML_BUNDLE_PATH=/custom/path`

#### Running full suite
```
mvn clean verify -P systemtest -pl systemtest -am
```

#### Running smoke tests
```
mvn clean verify -P systemtest -pl systemtest -Dgroups=smoke -am
```

#### Running single test or subset of tests
```
mvn clean verify -P systemtest -pl systemtest -am -Dtest=ManagedKafkaST#testDeployManagedKafka
```
```
mvn clean verify -P systemtest -pl systemtest -am -Dtest=ManagedKafkaST,RecoveryST
```

### Running testsuite in parallel
```
mvn verify -P systemtest -pl systemtest -am -Djunit.jupiter.execution.parallel.enabled=true
```
```
mvn verify -P systemtest -pl systemtest -am \
            -Djunit.jupiter.execution.parallel.enabled=true \
            -Djunit.jupiter.execution.parallel.config.fixed.parallelism=2
```