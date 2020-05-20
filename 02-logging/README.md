# Introduction - logging with logdna

Here you will find the instructions for setting up the environment we are going to work with during the week.


## Principle 11: Logging
From the [twelve factor app](https://12factor.net/) [principles 11](https://12factor.net/logs):

>XI. Logs
>A twelve-factor app never concerns itself with routing or storage of its output stream.
> It should not attempt to write to or manage logfiles.
> Instead, each running process writes its event stream, unbuffered, to stdout.
> During local development, the developer will view this stream in the foreground of their terminal to observe the app’s behavior.

## Logdna
Logdna service is already set for this bootcamp. You can find it under [observability -> logging](https://cloud.ibm.com/observe/logging)

### How it is setup for ROKS
The way it is setup for the ROKS cluster you can see clicking on *Edit log sources* under Sources. 

You can see how to set up logging for different sources such as Kubernetes, openshift and various linux - it is also possible to set it up for other sources.
Have a look at the steps mentioned for openshift.

Notice a seperate project/namespace is used for ibm-observe services. How a service account is setup for it, and how `privileged` Security Context Constraints (SCC) are setup to ensure all logs are reachable.
And also a secret/key is given that is unique for the LogDNA instance, so logs are sent to the right place and only visible in that correct instance.

#### Daemonset for LogDNA
Notice the last command:
```
oc create -f https://assets.eu-de.logging.cloud.ibm.com/clients/logdna-agent-ds-os.yaml -n ibm-observe
```

What does it do?
Explore the file (suggest to use curl - but this is not needed).
How is the secret mapped into the pods?
What log files paths can be expected to be harvested?

How does the actual DaemonSet-declaration look like on the ROKS cluster? Where do you find it?

Can you see the output from the console of one of the logdna pods either on CLI or webconsole?

### Logdna console
Now find the same information in the logdna console.
Start here: Logdna service is already set for this bootcamp. You can find it under [observability -> logging](https://cloud.ibm.com/observe/logging)

Select the *View logDNA* under View dashboard.

Once the dashboard is open look under views (the default page you start at). Notice the `All sources` canyou find the logdna and filter by it here? How many instances are there?

Now select `Everything` again, and then look under `All Apps` and look for logdna there. How many apps do you find ?

#### Finding all logs for a namespace
How do you find information for all logs in a namespace?

It does not exist in the top bar, but you can always do more finegrain searches in the bottom search bar.
To filter by a namespace, for instance namespace=bookinfo-nn then type `namespace:bookinfo-nn` (replace with your own namespace).

You might see a lot of events from istio for instance:
> May 10 11:50:58 reviews-v1-5874566865-7rqvz info watchFileEvents: notifying
> May 10 11:51:06 details-v1-789c5f58f4-8tc7r info watchFileEvents: notifying

You can remove them to find information you are after by using `-`. For instance `-"watchFileEvents"`.

### Logs from the bookinfo app
Can you find logs from the bookinfo app itself?


## More logging from bookinfo prodictpage
We have provided a productpage image that is a bit more informative.

Find your deployment for productpage and replace with this image: `docker.io/jebn/productpage-log`

Check the logs and see how it flows to logDNA as well.

