# Event Streaming Analytics Platform

A real-time data analytics platform that tracks the actions and behavior of your visitors. It enables immediate exploration, analyzing, processing, and visualization of event / behavioral data that meets the needs of your business today and in the future.

 It provides an understanding of the customer journey on on the site or apps and the different factors that contributed to a conversion which helps you improve visitor alerts that send you a notification when an important visitor returns to the website.

Also allows you to track the most important metrics, campaign activity, and paid traffic analytics that helps you improve and optimize your paid advertising campaigns against this objective. 


## Table of contents
* [Architecture](#architecture)
* [Real-time stream processing pipeline](#real-time-stream-processing-pipeline)
* [Implementaion](#implementaion)
* [Acknowledgments](#acknowledgments)

## Architecture
- Structural Architecture: 
The basic architecture behind the functionality of the proposed Platform structurally is broken down into a set of components as in the following diagram.

![Structural architecture of proposed event streaming analytics platform](architecture-components.png "Structural Architecture")

The structural architecture of the proposed event streaming analytics platform pipeline

- Event Streaming Analytics Pipeline: The next diagram shows the high-level architecture of the proposed event streaming analytics platform pipeline.

![Overall architecture of proposed event streaming analytics pipeline](architecture-pipeline.png "Pipeline Architecture")

The overall architecture of the proposed event streaming analytics pipeline

## Event Streaming Analytics Platform components
The event streaming analytics pipeline is pluggable and we can add a new component or remove one.

The platform as in the structural architecture comprises the following main components:

### Configuration
The settings that customize the events being fired and the data you want to collect. Also, determine the event pipeline and set up rules for processing it.

The configuration has the following sub-components:

#### Tag Manager
Tag Manager allows users to manage analytics tags on websites and mobile apps quickly and easily. It allows users to add all tracking codes without involving too much development team. Instead of updating all tracks on the website, you can use the tag manager interface to decide which code you want to install on the website or mobile app. 

Tag Manager has three main processes:
- Tag: The code which you want to place on the website pages, usually it is a JavaScript code.
- Trigger: When and where you want to place the code
- Variables: Repeated information that you need for tags and triggers.

**Implementation**:
- Through custom development, we can develop our `Tag Manager`.

- `Google Analytics` is a powerful analytics system that provides `Google Tag Manager` to implement all tracking tags with ease and efficiency. 

Google Analytics may give you free access to their services but in turn, they’re assembling data profiles on your website visitors, which they can then use for better targeting of advertisements across their network. If you want to keep control of your data, you need a tool that you can control. You won’t get that from Google Analytics. You can review these docs about [Why You Should Consider Using Google Analytics Alternatives](https://kinsta.com/blog/google-analytics-alternatives/#why).

- There are many `Open Source` alternatives to Google Analytics, we can use one of them and we can customize it for our business needs.
  * [Top 5 open source alternatives to Google Analytics](https://opensource.com/article/18/1/top-5-open-source-analytics-tools).
  * [13 Best Google Analytics Alternatives for Powerful Data Gathering](https://kinsta.com/blog/google-analytics-alternatives/).
  * [Plausible: Open source Google Analytics alternative](https://plausible.io/open-source-website-analytics).

#### Event Processors Flow Designer
Used to orchestrate the steps to process the fired events. 

Modeling the event driven processes pipeline which includes rules to determine the required event processors or microservices for processing events.

#### Metadata
Setting up the metadata required for all platform components.


### Analytics.JS and Mobile SDK
The analytics.js library is a JavaScript library for measuring website traffic and analyzing visitor's behavior and Mobile SDK is used for mobile apps.
It parses and fires analytics tags, and collects user event data (page views, Visits/Sessions, e-commerce transactions, site content, etc) from the client-side tier of your websites and mobile apps, and send data to collector service.

**Implementation**:
- We can develop our analytics.js library and SDK for different mobile types.
- There are a lot of open source libraries to do event tracking available on `Github`: [analytics-tracking](https://github.com/topics/analytics-tracking). 
we can use one of them, without the need to fully develop it and we can customize it for our business needs.


### Collection
Analytics library and tracking code on a website and mobile SDK send events to collector service which produces all incoming event data has to the event hub. 
Then event router routes all events to the channel in the event hub based on configured event process flow.


### Processing
Includes `Event Processors` to process the collected events and apply the business logic, and `Stream Processing` for real-time stream processing.


### Indexing
Consumes data from the event hub and stores it to required data storage based on the requirement and the target of the system.
- If we need to provide ** Full-text search** we can index data into `Elasticsearch` or `Solar` as a storage backend and use `Kibana` or `Banana` for dashboards and visualization.
- We can store the data to `NoSQL` databases or `HDFS`.


### Reporting & Visualization 
- Reporting provides access to all the processed event in the form of infographics through web interface, and also allows you to get the processed event through reporting API.
- Selecting the data storage and Visualization based on the requirement and the target of the system.


## Event Streaming Analytics pipeline

- The `Tracking script` role is to capture the different actions performed by users browsing the website or mobile app and pushes these events to the collector service.

- `Collector` service: push all the incoming events to a message broker `event hub` for ingestion.

The event flow starts with a client
sending an event to the collector, which is used to transport the event to the `event hub`.

- `Message broker`: A message broker is there to allow for the asynchronous processing of the data. One of the most popular message broker for data is `Apache Kafka`. 

- The `event mediator / router` receives the initial event from `event hub` and orchestrates that event by sending additional asynchronous events to `event channels` to execute each step of the process on event processes flow. 

It responsible for orchestrating the steps contained within the initial event. 
For each step in the initial event, the event mediator sends out a specific processing event to an event channel, which is then received and processed by the event processor.

The event mediator doesn’t actually perform the business logic necessary to process the initial event;rather, it knows of the steps required to process the initial event.

The mediator sends a message, get the reply and send other message based on the reply to the next Micro-Service.

The mediator is responsible to control the flow by setting the next queue so the next Micro-Service can pick it up.

- `Event processors / microservice` - The event processor components contain the application business logic necessary to process the processing event. 

Event processors (microservice) are self-contained, independent,  highly decoupled architecture components that perform a specific task in the application or system.

`Event processors`, which listen on the `event channels`  in event hub, receive the event from the `event mediator`, execute specific business logic to process the event and replying back.

- `Stream processing` can directly consume the data stream to compute real-time aggregates or filter the stream.

- `Indexer`: will take the incoming data from the `event hub` and push it to the storage layer.

- `Storage Layer`: The storage layer provides a long term storage for the incoming event data.

- After aggregating and organizing data, `reporting views` are built from where the Analytics can be presented to the end user.

On the front-end users have the flexibility to query, filter, group, setup conversion goals and consume the data in a variety of formats.


## Refrences

- [BUILDING AN EVENT-DRIVEN MESSAGING BROKER](https://www.irjet.net/archives/V7/i6/IRJET-V7I6733.pdf)
- [Chapter 2. Event-Driven Architecture](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch02.html)
- [Micro-Services and Mediator Pattern](https://www.linkedin.com/pulse/micro-services-mediator-pattern-eran-shaham/)
- [Using Apache Kafka as a Scalable, Event-Driven Backbone for Service Architectures](https://www.confluent.io/blog/apache-kafka-for-service-architectures/)
- [What is an Event-Driven Architecture?](https://aws.amazon.com/event-driven-architecture/)
- [4 Key Components of a Streaming Data Architecture (with Examples)](https://www.upsolver.com/blog/streaming-data-architecture-key-components)
- [Building Microservices Through Event-Driven Architecture, Part 6: Implementing EventSourcing on Domain Model](https://dzone.com/articles/building-microservices-through-event-driven-archit-4#)
- [Event-Driven Architecture and Its Application in Java - Part 2](https://blog.avenuecode.com/event-driven-architecture-and-its-application-in-java-part-2)
- [Everything Should Know About Google Tag Manager.](https://www.adlift.com/in/blog/everything-should-know-about-google-tag-manager/)
- [Web Analytics](http://www.websell.com.sg/web-analytics-2/)
- [4 Pillars of Analytics](https://medium.com/analytics-and-data/4-pillars-of-analytics-1ee79e2e5f5f)
- [Google Analytics Architecture Explained for Beginners](https://marketlytics.com/blog/understanding-google-analytics-architecture/)
- [How to collect the data you need to bootstrap your digital marketing analytics](https://dev.to/julienkervizic/how-to-collect-the-data-you-need-to-bootstrap-your-digital-marketing-analytics-hhm)
- [Data Flow Architecture Presentation Design](https://www.slideteam.net/technology-ppt-slides/data-flow-architecture-presentation-design.html)
- [Using clickstream event collectors to complement your Google Analytics](https://medium.com/analytics-and-data/using-clickstream-event-collectors-to-complement-your-google-analytics-777bd7aad61)
- [Top 5 open source alternatives to Google Analytics](https://opensource.com/article/18/1/top-5-open-source-analytics-tools)
- [13 Best Google Analytics Alternatives for Powerful Data Gathering](https://kinsta.com/blog/google-analytics-alternatives/)
- [Plausible: Open source Google Analytics alternative](https://plausible.io/open-source-website-analytics)
- [analytics-tracking](https://github.com/topics/analytics-tracking)
- [SNOWPLOW: FULL SETUP WITH GOOGLE ANALYTICS TRACKING](https://www.simoahava.com/analytics/snowplow-full-setup-with-google-analytics-tracking/)
- [Building a Streaming Analytics Stack with Apache Kafka and Druid](https://www.confluent.io/blog/building-a-streaming-analytics-stack-with-apache-kafka-and-druid/)

## Acknowledgments
Thanks for your time to read this doc. 
