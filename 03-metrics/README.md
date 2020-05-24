# Introduction - metrics with Sysdig

Here you will find the instructions for setting up the environment we are going to work with during the week.


## Observability
“Observability” is a term that comes from control theory. 

From Wikipedia:
> “In control theory, observability is a measure of how well internal states of a system can be inferred from knowledge of its external outputs. The observability and controllability of a system are mathematical duals.”

Observability is everything I need to find out why something is not working.

### Metrics: Goldensignals
As originally described from [Googles SREs]( https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/#xref_monitoring_golden-signals ).

```
Latency 		The time it takes to service a request
Traffic			Demand is being placed on your system
Errors			The rate of requests that fail
Saturation 		How “full” your service is.
```

This is just to remind ourselves about what we are after here, as the key metrics elements.

## Sysdig and metrics
The Sysdig service is already set for this bootcamp. You can find it under [observability -> monitoring](https://cloud.ibm.com/observe/monitoring)

### How it is setup for ROKS
The way it is setup for the ROKS cluster you can see clicking on *Edit log sources* under Sources. 

Have a look at what does it (the deployment) actually do; recalling how/what logdna did might be helpful. Where is the configuration for the agents stored? What namespace is used? Where is the data sent to? How is it identified for the instance? What does the agent capture?

Explore the file (suggest to use curl - but this is not needed).
How is the *configmap* (and secret) mapped into the pods?

How does the actual DaemonSet declaration look like on the ROKS cluster?

Can you see the output from the console of one of the Sysdig pods either on CLI or webconsole?

### Sysdig console
Now find the same information in the sysidg console.

Start here: Sysdig service is already set for this bootcamp. You can find it under [observability -> monitoring](https://cloud.ibm.com/observe/monitoring)

Select the *View Sysdig* under View dashboard.

Once you have logged in this way you should be able to go [directly to the dashboard](https://eu-de.monitoring.cloud.ibm.com/#)

On the left side you should see Overview, Explore, Dashboard etc..

#### Overview
Suggest to have a view on `Nodes` does the load look fair on the nodes?

Look also under `Workloads` and select your own namespace. There you should be able to see all of your deployments (expect no StatefulSet or DaemonSet).


#### Dashboards
Suggest search for the "Golden Signals". Where do you find it?

You may also look for the HTTP views.

#### Generate some load
GO back to the golden signals and generate some load....

```
for i in {1..100}; do sleep 0.1; curl $INGRESS_HOST/$INITIALS/productpage; done
```

for i in {1..100}; do sleep 0.1; curl 127.0.0.1:9080/productpage; done


##### FYI: Not an exercise right now: See slack connector

https://api.slack.com/tutorials/slack-apps-hello-world - we will come back to this though as part of chatops :-)


