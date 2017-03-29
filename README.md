# Assignment-12.1
1) Explain the need of Flume.
Ans : Often it is assumed that the data is already in HDFS, or can be copied there in bulk.
• However, there are many systems that don’t meet this assumption.
• They produce streams of data that we would like to aggregate, store, and analyze using
Hadoop — and these are the systems that Apache Flume is an ideal fit for.
• Flume is designed for high-volume ingestion into Hadoop of event-based data.
• The canonical example is using Flume to collect logfiles from a bank of web servers, then
moving the log events from those files into new aggregated files in HDFS for processing.
• The usual destination (or sink in Flume parlance) is HDFS. However, Flume is flexible
enough to write to other systems, like HBase or Solr.

2) Explain the working of Flume and its components in brief.

• To use Flume, we need to run a Flume agent, which is a long-lived Java process that runs
sources and sinks, connected by channels. A source in Flume produces events and delivers
them to the channel, which stores the events until they are forwarded to the sink.
• You can think of the source-channel-sink combination as a basic Flume building block.
• Flume installation is made up of a collection of connected agents running in a distributed
topology.
• Agents on the edge of the system (co-located on web server machines, for example) collect
data and forward it to agents that are responsible for aggregating and then storing the data
in its final destination.
• Agents are configured to run a collection of particular sources and sinks, so using Flume is
mainly a configuration exercise in wiring the pieces together.

The purpose of Flume is to provide a distributed, reliable, and available system for efficiently collecting, aggregating and moving large amounts of log data from many different sources to a centralized data store.
Components of Flume are :

 *Event: A byte payload with optional string headers that represent the unit of data that Flume can transport from it’s point of origination to it’s final destination.
*Flow: Movement of events from the point of origin to their final destination is considered a data flow, or simply flow. This is not a rigorous definition and is used only at a high level for description purposes. 
*Client: An interface implementation that operates at the point of origin of events and delivers them to a Flume agent. Clients typically operate in the process space of the application they are consuming data from. For example, Flume Log4j Appender is a client.
*Agent: An independent process that hosts flume components such as sources, channels and sinks, and thus has the ability to receive, store and forward events to their next-hop destination. 
*Source: An interface implementation that can consume events delivered to it via a specific mechanism. For example, an Avro source is a source implementation that can be used to receive Avro events from clients or other agents in the flow. When a source receives an event, it hands it over to one or more channels.
*Channel: A transient store for events, where events are delivered to the channel via sources operating within the agent. An event put in a channel stays in that channel until a sink removes it for further transport. An example of channel is the JDBC channel that uses a file-system backed embedded database to persist the events until they are removed by a sink. Channels play an important role in ensuring durability of the flows.
*Sink: An interface implementation that can remove events from a channel and transmit them to the next agent in the flow, or to the event’s final destination. Sinks that transmit the event to it’s final destination are also known as terminal sinks. The Flume HDFS sink is an example of a terminal sink. Whereas the Flume Avro sink is an example of a regular sink that can transmit messages to other agents that are running an Avro source.
