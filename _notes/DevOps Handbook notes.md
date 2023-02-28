---
tags:
  - toBeRefined
  - outOfStructure
---
# DevOps Metodology

# Introduction

- Teams should be cross\-functional to be able to deliver feature autonomously
- Good deployment process should last minutes or hours, not days or weeks
- Good deployment process shouldn't include risk of big outage or chronic firefighting
- Every IT department pursue two contradicting goals:
    - Respond to rapidly changing requirements
    - Provide stable, reliable, and secure service to the customer
- In non DevOps organization developers takes care of the first role, IT Ops regarding second, so both groups have contradictive goals... \- **the core chronic conflict**
- Downward spiral
    - Without proper processes \(automation, procedures, and DevOps collaboration\) very important systems are fragile and very hard to change, which results in not meeting the promises of development and features
    - Then new promises that are meant to compensate previous failures are coming which results with cutting corners and acquiring more technical debt
    - When there is a lot of technical debt, and there are already a lot of procedures which are trying to compensate all of the uncertainty  everything slows down, and it becomes hard to introduce any new changes....
- Every company is technology company...
- It is virtually impossible to make any business decision that doesn't result in at least one IT change
- Code deployment should be routine and predictible, they should be done in working hours 
- Feedback loops short be as short as possible
- There should be a good telemetry on production systems, that ensures that every error is discovered and corrected
- The platform should be provided in self\-service manner
- Dark launch technique \- it's possible to deploy feature to production without enabling it, or enabling it in canary mode
- Hypothesis driven development...
- No culture of fear, but have a high trust collaborative culture where people are rewarded for taking risks and talking about problems openly. 
- Blameless post\-mortems reporst should be done to spread the knowledge on how to prevent such accident in future
- Systems can be broken by purpose even on production to test if they are resilient enough
- DevOps allow to scale up number of developers without decreasing efficiency

# Three Ways

## Agile, Continuous Delivery, and the Three Ways

- One of tenets of Lean \(one of methodologies which DevOps was derived from\) is to decrease size of work batches in order to be able to optimize their movement through value stream more easily
- Core principle behind agile is also about working in short increments and decreasing size of the single software release

### Value stream

- Value stream is the sequence of activities needed to deliver the value to the end customer, including also related stream of information.
- Effective value stream is characterized by
    - Small batch sizes
    - Reduced amount of WIP
    - Preventing rework \(not moving unfinished or faulty items to next stage of value stream\)
    - Continuous improvement
- Value stream in DevOps \- in DevOps Values stream is about transforming business related hypothesis into the working technology or service, amount in time spent in the Value streams stands for Lead TIme
    1. Inputs is always concept, idea or hypothesis added into backlog
    2. Typically, concept is being transformed into the user stories via standard Agile process
    3. Code is being developed and checked into source code repository
    4. Deployment pipeline starts which ensures proper quality and integration with the whole system
- Fast flow is not the only important thing \- it needs to be ensured that releasing new software does not disrupt already existing system
- Deployment Lead Time is the part  of value stream which can be optimised most easily and where uncertainty can be minimised the most
- Development and design tends to be much more variable and uncertain \(mostly because its nature needing a lot of human creativity\)
- The time in which work item is being processed in Processing time
- Processing Time \+ Time spent in the queue is Lead Time
- A lot of organisations tends to work with Work Items of big size and has multilevel complicated processes, including a lot of handovers of Work Items between teams, a lot of uncertain manual operations, a lot of manual approvals and waiting in the queue which result in Lead Time measured in month of years
- Organisations which are following DevOps approach are working on much of smaller product iterations which are developed in shorten time, are verified and deployed to production in much faster and automated way
- Modular, encapsulated architecture serves such approach much better that monolithic one
- Development teams should be also heavily autonomous
- Domain of potential failure when introducing such small change should as small as possible
- %C/A metrics \- percentage of Completed/Accurate work items \- should be as high as possible to ensure that there won't be any rework needed after passing work item form one stage of value stream to another
- Effectiveness of values stream shouldn't be measure in proxy metrics \(eg. lines of code, numbers of deployment\) \- metrics which are not measuring direct impact on business outcomes like company's revenue
- Value stream flow efficiency metrics:
    - Flow velocity \- number of work items completed in time period, show if stream is accelerating
    - Flow efficiency \- Opened Work Items / Total time elapsed shows inefficiencies in currently processed items
    - Flow time \- ...
    - Flow load \- Number of active or waiting flow items in the value stream
    - Flow distribution \- distribution of different work item types
- Other example metrics \(American Airlines case study\)
    - deployment frequency
    - deployment cycle time
    - change failure time
    - development cycle time
    - number of incidents
    - mean time to recover \(MTTR\)

### Three ways

- First way \- maintaining left to right flow
    - assumes working on small batch sizes, work visibility, quality assurance
- Second way \- fast and constant feedback from right to left
    - issues needs to be discovered and resolved fast
    - Feedback loops should be short
- Third way \- experimental approach
    - high trust culture
    - scientific approach to experimentation and risk taking
    - Organisational leaning from both successes and failures
    - Transforming local discoveries to global solutions

## The First Way: The Principles of Flow

### Make work visible

- Work in IT is quite invisible which makes managing it harder
- Ease of handing over work from one team to another often result in overcomplicated flows in value streams
- It should be always clear for all members of the _work center_, what is the priority
- Work items should be encapsulated in backlog

### Limit WIP

- Too much of WIP will lead to a lot of context switching and chronic inability to get things done
- There should be a hard limit on how many work items can be in WIP state
- _Stop starting, Start finishing._

### Reduce batch sizes

- In case you have multiple work items which needs to go through all subsequent stages of the value stream, it would be much more efficient to move every of them one by one through the stream instead of finishing work related to each stage for all item first and only then moving to the next stage
- Then Lead time for delivering the first finished value \(first real value from customer perspective\) will be much lower
- Additionally flow can be constantly refined and all not processed items will benefit from it in their iteration 
- In this approach there will also not be any big release which might disrupt completely work of the whole organisation, every day is business as usual while value is being provided continuously to the end customer
- The larger is the batch added to the production, the more difficult investigation of potential problem might be

### Reduce number of handoffs

- Every hand off lead to..:
    - communication overhead
    - data and knowledge loss
    - possible time wasted in the queue
    - ...increased Lead time
- Hand offs should be potentially automated 
- In ideal world \( I assume \) cross\-functional team should be responsible for moving work item through the whole value stream and all of the extrernal dependencies should be resolved in automatic self\-service way

### Continually identify constraints

- Focus on improving the flow should be always on the biggest bottleneck in the whole system
- Usual constraints in tech companies:
    - Environment creation
    - Code deployment
    - Test setup and run 
    - Too tight architecture \- inability to develop feature without going through time consuming process of solution approval
- If Deployment pipeline is working properly, usually, the biggest constraint in Tech company is development

### Eliminate hardships and waste

- Every activity which if bypassed is not changing the end result of the value stream is a waste
- Waste examples:
    - Work done only partially
    - Extra process \- activity which is not adding anything to the value delivered to the customer
    - Extra feature \- feature of the product which is not needed by customer or organisation
    - Task switching \- unnecessary context switching
    - Waiting
    - Motion
    - Defects
    - Work which is unnecessarily manual
    - Heroics \- focus on heroic firefighting instead of continuously improving pipeline efficiency

## The Second Way: The principles of Feedback

- Constant backward and forward feedback
- Detecting problems quickly while they are small and cheap to resolve
- Failures are opportunities to learn rather than to point punishment and blame

### Working safely with complex systems

- System is complex when no single person can understand it as a whole
- Typically tightly coupled and interconnected
- Often doing same thing twice can lead to different outcomes
- Failure is inevitable, so has to be detected quickly before it can grow and have big negative impact
- Working safely:
    - local knowledge needs to exploited globally
    - leaders create leaders who continually grow these types of capabilities
    - problems are swarmed and resolved quickly
- Flow of information must be quick on all stages of production
- CI
    - Automated CI pipeline should ensure and produce deployable versions of software
    - CI systems should run prevent any problems to process downstream and reduce technical debt
    - Would be the best if pipeline give as much feedback as possible, doing 360 verification so all problems can be fixed within next build
    - As much checks as possible should easily reproducible on local machine or enforced during code push
    - As much as possible \(perfectly all\) of QA and Security verification should be automated
- Telemetry of all system components should detect issue early
- Decision making should be close to the place where work is being done otherwise strength of feedback is weaker so effectivness decreases
- If there is manual approval it should be done by person which is very close to the process \( there is nothing worse than waiting weeks for approvals from busy person who is not engaged with the topic \)
- Creating to much detailed documentation doesn't make sens since it tends to get obsolete shortly
- Every person working in the value stream should resolve problems in his/her area as part of daily work
- People in the process should care about our internal client \(next in the chain\)
- Non\-functional requirements needs to be proritized as much as features itself:
    - Architecture
    - Performance
    - Stability
    - Testability
    - Configurability
    - Security

## The Third Way: The Principles of Continual Learning and Experimentation

- local individual knowledge should be constantly turned into team and organizational knowledge
- There are pathological, burecracy and generative organizations
- Pathological
    - Culture of fear
    - "name, blame, shame"
    - Problems are often hidden till they transform into the catastrophy
    - A lot of politics
- Burecracy
    - A lot of process approvals which are introduced to the process a response to earlier human failures
    - Failure is proceed by system of judgement resulting in punishment or justice and mercy
- Generative 
    - High trust culture
    - everybody is encourage to experiment daily on how to improve the process
    - local learnings are rapidly transformed to general knowledge
    - there is reserved time for improvements
    - Stress  and failure are introduced to value stream on purpose to ensure systems resiliency 
    - Teams can quickly adapt to ever\-changing environment
    - Everyone is constantly seeking to improve performance and resolve issue within their area
- In complex systems we cannot perfectly predict all outcomes of our actions so it's important to know how to deal with failure
- Blameless Post\-mortem reports should be made in order to transform failure into process improvement
- "By removing the blame, you remove the fear, by removing fear, you enable honesty, and honesty enables prevention" 

### Continuous improvement

- Processes do not stay the same over time, due to chaos and entropy the degrade
- Not fixing problems, but going with workarounds accumulates technical debt, which shortens capacity to do real productive work
- There should always some margin of time reserved for process improvement
- Local discoveries should be codified and transformed into general knowledge 
- Shared libraries is an example of codifying local discovery or improvement across multiple services
- There should always be a goal to continuously improve code coverage, decrease test execution and deployment times
- There should be also fair amount of failure and uncertainty added injected to the system to test its resilience 

### Leader as learning culture reinforcers

- Leaders shouldn't focus on making right decisions but rather creating conditions where every team member can improve the process as part of his/her daily work
- There should be always mutual respect
- Leaders are never close enough to the work to be able to solve the problem efficiently, and workers will not ever have enough of broader context

# Where To start

## Selecting Which Value Stream to Start With

## Understanding the Work in Our Value Stream, Making it Visible, and Expanding it Across the Organization

## How to Design Our Organization and Architecture with Conway's Law in Mind

## How to Get Great Outcomes by Integrating Operations into the Daily Work of Development

# The First Way: The Technical Practices of Flow

## Create the Foundations of Our Deployment Pipeline

## Enable fast and Reliable Automated Testing

## Enable and Practice Continuous Integration

## Automate and Enable Low\-Risk Releases

## Architect for Low\-Risk Releases

# The Second Way: The Technical Practices of Feedback

## Create Telemetry to Enable Seeing and Solving Problems

## Analyze Telemetry to Better Anticipate Problems and Achieve Goals

## Enable Feedback So Development and Operations Cn Safely Deploy Code

## Integrate Hypothesis\-Driven Development and A/B Testing into Our Daily Work

## Create Review and Coordination Processes to Increase Quality of Our Current Work

# The Third Way: The Technical Practices of Continual Learning and Experimentation

## Enable and Inject Learning into Daily Work

## Convert Local Discoveries into Global Improvements

## Reserve Time to Create Organizational Learning and Improvement

# The Technological Practices of Integrating Information Security, Change Management, and Compliance

## Information Security Is Everyone's Job Every Day

## Protecting the Deployment Pipeline
