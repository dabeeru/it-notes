---
tags:
  - toBeRefined
  - outOfStructure
---
# Load testing

# K6 API load testing

- Can either hammer the endpoint or simulate real user scenario
- Checks and thresholds
    - Checks \- Checks are assesing the outcome and saving result of the assesments
    - Thresholds \- Results of checks are compared with thresholds and it's assesed if whole test case fails or not
- Configuration
    - Params regarding number of VUs and Iterations can be passed via CLI
    - There are many other parameters to be set
    - Some Parameters can meconfigured with also with ENV variables
    - http client params
        - Params.auth \- authentication method
        - may other connected with cookkies, redirections and other http protocol settings
    - Specific useful params
        - Batch \- limit for simultaneous requests for single VU or single hostname \(API domain
        - config \- ..file can be passed 
        - exit on running \- script can be finished after test being spinned of
        - http\-debug
        - include\-system\-env\-vars
        - no\-connection\-reuse
        - rps
        - show logs
        - summary\-export  \- exporting summary to json file
        - \-\-env \- passing environment variables
        - \-\-sumary\-trend\-stats
        - thresholds 
        - throw \- throwing 
        - \-\-verbose
    - env variables can be taken from \_\_ENV object
    - those env variables can be passed by \-\-env / \-e swithc or by \-\-include\-system\-env\-vars
    - There are various predefined env variables which can be setup in k6 execution context to control its behavior
    - command line flags always have the highest precedence
- k6 HTTP request client
    - batch\(\) \- sending multiple requests in parallel
    - besides there are simply functions for every HTTP method call
- Scenarios
    - There can be also more advanced scenarios configured for the test having multiple executors to simulate multiple different traffic paterns in the same time 
    - There is also constant\-arrival\-rate executor
    - every executor has its own specific configuration 
- response body can be discarded in order not to overload the test executor \(still can accept it for request where responseBody needs to be inspected\)
- Writing your own script is recommended way to create K6 scenario \(not generating\), when generating scenario altering it is advised or even necessary
- Open\-api specification is quite limited
- There is a json output option
- There is server exposing REST API being run when executing the tests
- Grouping
    - request can be grouped into one aggregation space with k6 tags passed by parameters
    - all request in group gets group tag by default
    - Every group has its own metrics
- There name tag to annotate test request with some descriptive name 
- Running tests
    - test script must export default function \- this is recognized as 'VU code'
    - code outside VU code is called init code

## K6 Open source vs Premium

- OSS License
    - k6 is under GNU Affero General Public License v3.0 license which is allowing a commercial use, modification and distribution
    - you can take it, modify it and build your own SaaS from it but you need to make modified source code available 
    - You can use it in commercially sold software

### Premium 

- Automatical scalling capabilities
- Storage for test result
- Dashboards
- Integrations 
    - Datadog
    - Grafana 
- Automatic performance analysis
- Collaborative wokrspace 
- Premium support
- 17 Geographic loccations form which tests can be run

# Load tests types

- Smoke test
    - regular, minimal load
    - it can test if service is still capable any load or if test scenario is working properly
- Load test 
    - assessing current performance of your system in terms of concurrent users or request per second
    - checking system behavior during normal and peak condition
    - Assessing if system is meeting the performance goals
    - Test should have ramp\-up stage, nominal traffic stage and teardown stage, can also have peak stage
        - System should have time to scale up
- Stress test
    - Checking service stability during heavy load
    - It gives you information of maximum users capacity, the breaking point, how system behaves when failing and how it recovers from the failure
    - There should be multiple stages where traffic is increased gradually
- Spike tests 
    - Special example of stress test where load is not being increased gradually but is highest from the beginning
- Soak test
    - Test checking system reliability during long period of time 
    - Good for checking memory leaks, insufficient quotas etc.

General insights:

- Load testing machine should be close to target machine
- Nothing else should be run on the target machine specially not load testing tool
- Load testing scenario types
    - Capacity test \- you are increasing load and checking when it will break
    - Stress test \- you are simulating expected peak and check if target handles it
    - Soak test \- you are simulating the load over long period of time to check long terms implications of handling such load

---

# General considerations on running perfomance test

- Monitor resources on the side of lad generator
- If there are doubts that load generator is able to generate enough load or its measuments look to be off try to run some simple test case towards realiable target like nginx server
- Network delay cannot be to big \(\<1ms is great\) because otherwise single tool wouldn't be able to open enough TCP connections per second.. 
- Bandwidth might be also important
- In general many laod generators may measure worse reponses times than they are in reality \(due to load generator resource utilisation\) 

# Microservice load testing
How to design the test case scenario?
## Problem
1. There are multiple clients which can utilize the service in various different ways
2. Every client may use a different payload