# Jaeger

## Capabilities
- Distributed context propagation
- Distributed transaction monitoring
- Root cause analysis
- Service dependency analysis
- Performance / latency optimization 

## General
- [[OpenTracing]] compatible data model
	- `span id`
	- `trace id`
	- `baggage`
- many libraries ofr [[Go]], [[Java]], [[Node]], [[Python]], [[C++]] and [[C#]]
- Backends
	- [[Cassandra]]
	- [[Elasticsearch]]
	- [[Kafka]]
	- Memory
- React UI
- Backend components in [[Go]]

## Terminology
- Span - logical unit of work which [[Jaeger]] tracks
- Trace - acyclic directed graph of spans
 ![[Pasted image 20230226175231.png]]

## Architecture
- Can be also deployed as all-in-one binary

### Components
- Agent - network daemon listening od UDP which collects data from clinet library and sends it to the jaeger collector
- Collector - recieves emssages from agents and puts traces into processing pipeline where transormation, validation, indexing and storage happens
- Query - service which queries backend and allows UI to display it
- Ingester - reads data from Kafka topic and stores them in other backend (eg. Cassandra, Elasticsearch)

### Setups
#### Direct to storage flow
![[Pasted image 20230226175409.png]]
#### Kafka as itermediate buffer
![[Pasted image 20230226175429.png]]

## Processing
- `spanId` and `traceId` are propagated with every message but `bagagge` is asonchrynously sent to Jaeger backed in the backgroud

## API 
- Thrift 
- Protobuf via gRPC
- Zipkin format

## Deployment
- OpenTelemtry Operator is discontinued
- There Is Kubernetes operator