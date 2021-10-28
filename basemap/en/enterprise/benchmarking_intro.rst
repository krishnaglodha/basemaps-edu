.. _geoserver.jmeter_benchmarking_intro:

Benchmarking primer
=========================================================================

Benchmarking and Stress Testing
+++++++++++++++++++++++++++++++++++

Benchmarking and Stress Testing is an important phase in a project since it gives you various information about your application and the deploy infrastructure. It is crucial to investigate and characterize:
 * Scalability
 * Robustness in extreme conditions
 * Presence of any bottleneck inside the application
 * Performance 

Deciding where to send the test requests is important. Network connection sometimes can compromise the results of your tests since:
 * In case of unrelated high network overload you may see low throughput;
 * This can be associated to a bottleneck of the connection which results in bad performance without concerning about the request type.

For this reason, be careful to set up your remote tests in a separate network in order to avoid network overload.

The main reason why benchmarking is performed is to look for bottlenecks, which could greatly hinder performance and scalability.
Typical bottlenecks are as follows:

 * CPU: at high load all the CPUs are pegged.
 * Disk: CPUs are not hot, the server is reading a lot from the disk during the benchmark.
 * Network (data sources): CPUs are not hot, but the local network (maybe towards the DB) is at full capacity.
 * Network (output): all local resources are mildly used, but the outbound connection is at full capacity.
 * Code scalability: none of the above, every resource seems to be in light use and usage is not going up despire higher client load.

Definitions
++++++++++++++++

Here below you can find some definitions that are going to be useful throughout this section.

Throughput
#############
Citing `wikipedia <https://en.wikipedia.org/wiki/Throughput>`_, *Throughput is the rate at which requests are processed by a running system*

Response Time
#############
Citing `wikipedia <https://en.wikipedia.org/wiki/Response_time_(technology)>`_, *Response time is the total amount of time it takes to respond to a request for service.*

Scalability
#############
Citing `wikipedia <https://en.wikipedia.org/wiki/Scalability>`_, *Scalability is the capability of a system, network, or process to handle a growing amount of work, or its potential to be enlarged in order to accommodate that growth.*

(High) Availability
##########################
Citing `wikipedia <https://en.wikipedia.org/wiki/High_availability>`_, *High availability is a characteristic of a system, which aims to ensure an agreed level of operational performance for a higher than normal period.*

Perceived Performance
######################
Citing `wikipedia <https://en.wikipedia.org/wiki/Perceived_performance>`_, *perceived performance, in computer engineering, refers to how quickly a software appears to perform its task.* The concept applies mainly to user acceptance aspects and it is very different from pure performance. 


Response time VS throughput
++++++++++++++++++++++++++++
It is important now to spend a few words on how to read and interpret the results of benchmarking tests from JMeter.

Looking at the picture below, the goal of running stress tests is to actually infer the throughput curve and the avg response time curves in order to understand how the system behaves under increasing load and find the moment when we hit the first bottleneck and the throughput starts to fall (while the reponse time starts to grow quickly); this conditions is called **maximum resource utilization**, it basically manifests itself when the system cannot cope with additional load as one or more of the critical resources (CPU, RAM, Disk) or the software itself becomes a bottleneck: in this situation if the load keeps growing, the stability of the system will be compromized and the user experience will suffer greatly.

	.. figure:: img/response_time.png
	   
	   Response time versus throughput against load


The goals of the enterprise set up section can be summarized as follows:
 
 * the information and the examples that we provide on JMeter are directed towards providing you with the basic knowledge to perform stress tests as well as capture and make sense of the results.
 
 * the information that we provide about the various ways to tune and optimize GeoServer will be crucial in deciding where and how to intervene in order to improve the results.
 
