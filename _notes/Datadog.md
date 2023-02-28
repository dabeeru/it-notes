---
tags:
  - toBeRefined
  - outOfStructure
---
# Datadog

# Log Management

- You can generate metrics based on logs
- You can correlate logs with metrics and traces

## Ingesting

- There is support for log deamons like rsyslog, syslog\-ng,nxlog, fluentd, logstash
- There is logging collection library for specific languages \(javascript included\)
- Stacktraces should be logged with some specific metadata
- Logs can be ingested also from Browsers
- Logs can be associated to APM traces by adding trace ID, spand ID, env, service, version
- For node.js using winston as logging library is recommended
- You can use winston HTTP transport to send your logs directly to Datadog API
- There are also a lot of native integrations with AWS services through AWS integration plugin 

## Configuration

- Datadog automatically parses JSON logs
- If logs are not JSON formatted you can use datadog pipeline to normalize it
- Pipeline
    - Pipeline is defined by sequence of processors
    - When log is going thourh pipelines, filters are applied, if filter returns match then log event is processed by all processors in the pipeline \(?\)
    - Before passing event to the pipeline, preprocessing is being applied to extract basic metadata like timestamp, status, host, service and message
    - There are predefined integration pipelines for specific data sources
    - Pipelines can also include netsted pipelines
- Processors
    - Processors extract and transform data from fields
    - Grok processor \- allows you to extract some specific data from log message according to the pattern
    - Log data remapper
    - Log status remapper
    - Service remapper
    - Log message remapper
    - Generic remapper \- remap any field to another one
    - URL parser \- decompose URL to mutliple attributes like host, path, query strings etc.
    - User agent parser 
    - Category \- you can create new category filed to classify logs
    - Aritmetic processor \- allows you to calculate new fields based on existing
    - string builder processor
    - GeoIP parser
    - Lookup processor \- mapping field value to another
    - Trace remapper \- allowing to add trace id to the log during the ingestion \(how can it work exactly?\) 
- Parsing
    - Goku parser is main way to Parse unstructured logs
        - Machers \- can be used to match data to be extract eg. date matcher, string matcher, regexp, integert etc.
        - filters can be used to apply further processing
            - There are predefined filters for processing formats like CSV etc. for example grouping multiple CSV attributes into one object
- Attributes & Aliasing
    - Reserved attributes
        - host
        - source
        - status
        - service
        - trace\_id
        - message
    - Standard attributes \- attributes defined by datadog administrator as standard for whole rganization \( existing attributees can be promoted to standard ones in log explorer \)
    - Aliasing \- it allows you to alias some existing attribute into standard one
- Indexes \- after indexing logs are kept in indexes with various configuration like quotas, exclusion filters or retention policies
- Archives \- logs can be archived on cloud storage owned by customer, logs can be then rehydrated from the archive into the custom index

### Logs Explorer

- Live trail 
- Search 
- Search syntax
    - AND, OR, \- \(negation\)
    - by default search is for the message
    - @attribute search
    - ? \- wildcard single character
    - \* wilcard multiple characters
    - numerical values @attribute:\>100 or @attribute:\[400 TO 499\]
    - Tags \- eg. env:prod
    - Arrays can be used as facets
- Facets
    - Dimensions \(non\-qualitative\) \- for counting or grouoping
    - Measures \- for aggregating, sorting, filtering
    - Different attributes can be aliased with a facet, so the same information presented differently in many log events can be normalized 
    - In table view you can sort only by attributes used by facets
- Grouping
    - There can be multiple search queries added into the single search for frther result aggregation \(etc.\)
    - Multiple formulas can be applied to the same grouped view
    - On top of aggreagation results there can be formulas applied \(eg. some aritmetics or timeshift to get another dataseries from the week before\)
    - **Patterns \-** Allow you to group messages by patterns recognized in log messages
    - **Transactions \-** How to use transactions t 
- visualize...
- Saved Views
    - Ability to setup specific configuration for log explorer which fit purpose of the specific investigation 
    - Many Datadog integration have their own view

## Security

- Agents can obfuscate and filter logs before ingestion to Datadog

# APM

- Trace is  a sequence of operations serving the same transactions through the multiple services
- Span is a single operation within the trace 
- Custom metrcis can be generated from ingested spans
- traces can be connected with synthetic tests and RUM \(Real User Monitoring\) \- can be used to investigate failed synthetic tests
- There is predifned view for traces connedted with errors

## Sending traces

- In order to ingest traces, tracing library you need to use tracing library and DataDog Agent
- Library fully supports node.js \>= 12
- Library needs to be initialized before any other module
- In case of getting incomplete traces it might be that tracer has not been properly initialized in an app
- When using transpilers like TypeScript, Webpack or Babel intialize tracer in an external file and import it
- Tracer can be configured with custom service, env and version tags \- this configuration can be also provided as env variables
- Tracing for ECS
    - Traditionally \(when not using fargate\) there should be Datadog Agent run as a ECS daemon container on every machine
    - Additionally Agent can also be used to ingest data regarding **processes**, **network performance** and **logs**
    - On Fargate there is a need to run DataDog Agent as additional container in the task \- agent is getting docker stats form ECS task metadata endpoint
    - Without Agent Datadog can also pull some ECS metrics from cloudwatch in 10 minutes intervals, those metrics are also less granular than those fetched by Agent
    - AWS FireLens a log router for ECS enabling sending ECS logs to various services
- OpenTelemetry and OpenTracing
    - There is Datadog exporter for OpenTelemetry collector \(beta\) \- it can be used insted of DataDog Agent
    - It can be also run aside to DataDog Agent and report all metrics to it
- Serverless tracing
    - You can use X\-Ray traces or interface Datadog tracing library
    - While using X\-Ray, there will be more delay and no sampling rate can be configured
- Custom instrumentation..
    - You can create custom spans around some specific operations within when serving incoming request
- There are also some obfuscation and filtering capabilities for APM traces

## Trace explorer

- 100% of ingested traces can be explored for last minutes
- Custom filters can be applied to decide which traces to index for 15 days period
- 'Intelligent' \(predefined\) filters can be applied to index data for 30 days period
- Intelligent filters are indexing all traces connected with errors, high latencies and low\-throughput services
- Every service can send up to 50 traces per second
- Request map \- shows connections between service along with latencies, throughput, and general service health
- APM Monitor \- Monitor based on trace

## Trace ingestion

- Each span in ingested includes _ingestion reason_
- Head\-based default ingestion mechanism
    - decision if trace should be ingested is made at a beginning of the trace \- on the root span \- and propagated through the trace 
    - Agent decides on sampling rate for the specific service \(if there is more traffic for the service it might increase sampling rate for it\)
    - This behavior can be overrided on level of tracing library
        - override sampling rate for all of the requests
        - set up sampling rates for some specific root services
        - set limit for traces per second 
    - There will be no incomplete traces \- everything is ingested or nothing is being ingested
- Force keep and drop
    - On tracing library level exceptions can be made for some specific tracsaction to force keep or drop related trace ingestion
- Error and rar traces \- addtionally even is trace in not decided to be ingested by Head\-based mechanism it can exceptionalyl ingested if it's connected with an error or is rare trace \(so visibility of rarely used services will be preserved
- Stats and metrcis are always calculated on all traces 
- Datadog agent do not drop any traces up to 50 traces per second 

## Deployment tracking

- Version tag can be added to every span which can give visibility on behavior of different service versions 
- There is a predefined view which allows you to compare two specific versions
- For single versions there are also view showing information of errors and endpoint performance for the specific version

## Continuous Profiler

- There is also a profiler which is capturing metrics connected with execution of specifcic functions within the code \(time of execution, memory allocation, cpu, i/o locks etc.\) 
- It can raise code improvement proposals autmatically
- Profiles can be easily connected with Traces
- Profiles for different time periods or service versions can be compared 

# Dashboards

- There are general dashboads, timeboards and screenboards \(for story telling\)

# Alerting

- Monitors need to be configured based on selected metric and threshold
- Monitor types
    - host availability
    - Metrics \(threshold\)
    - anomaly
    - outliners
    - Forecast \(it's forecasted to cross the threshold\)
    - Process \(check if process is on available on the host\)
    - Network TCP/HTTP check
    - Logs
    - APM
    - RUM
    - CI pipeline
    - Composite \(combining other monitors\)
- Alerts can be associated with notifications
- Renotifications can be sent if issues are not resolved
- Service Level objectives
    - SLO \(Service Level Objectives\) can be defined including SLI \(Service Level indicatiors = good behavior time / all time or Proper metrics / all metrics\)
    - SLOs can be defined for both Monitors or metrics
    - Errors Budget can be configured for SLOs 
    - Alerts and notfications can be raised whenever budget for SLO is exceeded
    - Burn Rate notifications can be raised  \- budget is not exceeded yet but it's portion for specific interval has been exceeded so if that would continue there would be a problem....
- Indicent management module is available with ingrations with Jira and PagerDuty

# Metrics

- ![image.png](image/image.png)
    Querying metrics:
- Metrics types
    - Count
    - Rate
    - Gauge
    - Histogram \(avg, count, median, 95th percentile, max\)
    - Distribution \(like histogram but with specific percentiles\)
- Explorer
    - only Metrcis submitted to datadog in last 24 hours show in Graph dropdown
    - Distributions \- you can configure time interval and use basic metrics for it \(avg, mig, max...\) plus configure calculating some specific percentile values
- Various units can be associated with the metrics
- It can be limited what metrics are indexed and which are not to limit costs
- Everything which is not posted by one of DataDog official integrations is treated as custom metric, examples:
    - some business metrics posted by app
    - some metrics caluclated from logg or traces
- Ingestion \- metrics can be ingested by
    - Datadog agent
    - DogStatsD Agent
    - Datadog API

# Events

- Events can be triggered by multiple various datadog integration
- Custom Eventss can be sumited via
    - DataDog API
    - Custom Agent Check
    - DogStatsD
    - Events Email API

# Database Monitoring

- Supporting
    - Self\-hosted
        - PosgreSQL
        - MySQL
    - Managed databases \(still PostgreSQL and Aurora\)
        - Amazon RDS 
        - Amazons Aurora 
        - Google cloud SQL
- Metrcis \- ingested for 200 tops normalized queries
    - Quries performance
    - database state
    - events
    - failovers
    - connections
    - buffer pools
- Views
    - Query samples \- analyzing specific queries performance
    - Explain plans \- showing query execution plan
- Dashboards \- db vendor specific dashboards
- Security \- datadog obfuscates all sensitive informations like passwords or PII data

# Network Performance Monitoring \(NPM\)

- Monitoring network traffic with persion to port and PID
- Istio is supported
- Celium and SElinux are supproted
- CLoud services can be autodetected
- All metrcis are collected by Datadog Agent
- IP's are resolved to DNS names
- For kubernetes Pre\-NAT IPs \(external for Kubernetes\) are being resolved
- Network ID is attached to distinguish endpoint in different networks with overlapping IP ranges
- Network map \- visualizing connections between all endpoints
- DNS queries response time and success rate can be also monitored
- Various Network devices can be monitored via SNMP \(with DataDog Agent\)

# Continuous Integration Visibility

- Test execution and pipeline result monitoring is supported
- Datado Agent needs to be used \- it can installed on the CI worker node or externally
- Major CI systems are supported \(but no Concourse CI\)
- Dashboards showing status of different tests and pipelines are available

# Infrastructure

- List of host \- list of all monitored hosts with basic information and metrics regarding resources utilization
- Map of hosts 
- Container map
- Processes monitoring \- giving visibility into processes run on the monitoring hosts and containers along with Resource utilization
- Cloud infrastructure costs are also being monitored

# Watchdog

- Part of APM
- Tool using algorithms to detect anomalies in
    -  APMs metrics like request and error rates or latencies
    - Integrations
        - PostgreSQL
        - AWS services 
            - S3 
            - LBs
            - Cloudfront
            - DynamoDB
        - NGNIX
        - Redies
- Root cause analysis
    - Can automatically correlate data like
        - APM traces
        - APM metrics
        - sevice version changes
        - Infrastrucutre metrics
        - Error log patterns
        - Changes in database queries
- Can analyze impact on Real users \(with RUM\)

# Integrations

- There is Concourse CI monitoring integration

# VWFS takeaways

- **We can create metrics from logs \- we can create detailed dashboard for latency based on that**
- Are Stack traces logged properly?
- **We can share Saved View which could be preconfigured for BFF specific log structure \- creating proper columns, facets and maybe some** 
- How exactly logs ingestion works in VW? 
    - Is Winston always producing JSON?
    - Is Cloudwatch encapsulating Winston logs in it's own JSON?
    - What is Kinesis stream doing exactly?
- Is URL parser used properly?
- Live Trail can be indeed useful for observing the outcome of the new deployment
- **Example use of log explorer showcasing its capabilities:**
    - ...

# **TODO**

- [ ] Finish reading documentation
- [ ] Create service latency metrics and dashboard
- [ ] Create some common view for BUS services
- [ ] Verify VW datadog adoption
    - [ ] Are Stack traces logged properly?
    - [ ] How exactly logs ingestion works in VW? 
        - [ ] Is Winston always producing JSON?
        - [ ] Is Cloudwatch encapsulating Winston logs in it's own JSON?
        - [ ] What is Kinesis stream doing exactly?
    - [ ] What are currently available AWS integrations? ECS, DynamoDB, S3? Kong?
- [ ] Explore... the log explorer
    - [ ] Check some log explorer [guides](https://docs.datadoghq.com/logs/guide/)
- [ ] Prepare the training
    - [ ] Is URL parser used properly?
