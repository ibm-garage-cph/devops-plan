# Introduction

Here you will find the instructions for setting up the environment we are going to work with during the week.
First a few tips.

## Tips

1. Have a good editor. We recommend [Visual Studio Code](https://code.visualstudio.com/)
2. You might want to store your own notes and various keys etc. We recommend to do so in a .secret file - this file is ignored via the `.gitignore` file, just to ensure it does not get uploaded to git! (For windows we have also ignored `secret*`)
3. You might want to store your own environment variabke values. We recommend to do so in a .env file - this file is ignored via the `.gitignore` file, just to ensure it does not get uploaded to git! (For windows we have also ignored `env*`)

## Loging in

Ensure to login to IBM cloud both from [web](https://cloud.ibm.com) and command line.

Ensure to login to the Redhat Openshift Kubernetes Service (ROKS) cluster too - 
both web and command line too.

Look at the material from before, or ask a fellow student in case you need a memory recall :-)

When logged-in, ensure you can do `oc get nodes` like in the example below:
```bash
oc get nodes
NAME             STATUS   ROLES           AGE   VERSION
10.215.133.195   Ready    master,worker   22d   v1.16.2
10.215.133.206   Ready    master,worker   22d   v1.16.2
10.215.133.230   Ready    master,worker   22d   v1.16.2
```

## Get the Ingress domain
All URLs in the rest of the bootcamp relates to the ingress domain of the ROKS cluster. Suggest to extract it and store it in your `.env` file.
You can find it with `ibmcloud ks cluster get -c roks-bootcamp`

Suggest to fill enviromment variable `INGRESS_SUBDOMAIN` with the value like:

```bash
export INGRESS_SUBDOMAIN="xxxxxx.eu-de.containers.appdomain.cloud"

echo "INGRESS_SUBDOMAIN=$INGRESS_SUBDOMAIN" >> .env
```

The above is for linux and mac. For windows use SET .....

## Save your INITIALS
If you want tu se variables to make some of the steps later easier we recommend to store your iniails both in the .env file and in your environment variables.

```bash
export INITIALS="<CHANGE THIS>"

echo "INITIALS=$INITIALS" >> .env
```

## Cleanup your projects from before if needed

In case you have projects (namespaces) from earlier, it's time to suggest a cleanup. Especially the bookinfo-<id> project.

You can list projects with `oc projects` (but you can also use the web console).

Projects are deleted with `oc delete project <project_name>`.

## Setup the bookinfo now also with your own fqdn
Previously you had a bookinfo url that was based on a path like `http://hostname.domain/bookinfo-<xx>/productpage`. Now you should set it up via your own Fully Qualified Domain Name (fqdn). With the format of `bookinfo-$INITIALS.$INGRESS_DOMAIN`.

The files to setup the project are found in this folder.
What do you need to change before applying below? (Hint: Check before doing each command and reflect on what does each line actually do for the project? Is there anything you need to change before running the command? Are environment variables interpreted inside yaml files?)
```bash
git clone https://github.com/ibm-garage-cph/devops-plan.git
cd devops-plan/01-basis

oc new-project bookinfo-$INITIALS

oc get secrets default-icr-io  -n default --export -o yaml | oc apply -f -
oc patch serviceaccount default -p '{"imagePullSecrets": [{"name": "default-icr-io"}]}'

oc label namespace bookinfo-$INITIALS istio-injection=enabled

oc apply -f bookinfo.yaml
# oc apply -f bookinfo-gateway.yaml   # This is how you used to set it up in previous bootcamp
oc apply -f bookinfo-gateway-own-fqdn.yaml
oc apply -f destination-rule-all.yaml

oc get pods
```

(be inspired by https://github.com/ibm-garage-cph/istio-roks-101/tree/master/workshop/exercise-3 and exercise-4 but not beyond)

Now look for the productpage webpage. Where do you find it?

FYI: The reason it sets up its own unique FQDN is due to a change in the istio-config now set to [automatic route creation](https://docs.openshift.com/container-platform/4.3/service_mesh/service_mesh_day_two/ossm-auto-route.html).

## Explore logs
Via `oc logs` ,  `kubectl logs` or via web console.


## Explore console
Via `oc exec`, `kubectl exec` or via web console.

What is the user id used for the pods? (use the `id` command for this)

## Portforwarding - to see yourself
Sometimes it's useful to be able to see directly what is going, unfiltered.

`oc port-forward` can help you with that.

What port is the productpage available on via the productpage service? (look in yaml, cli or web console...)

Get the productpage available via the service locally on your desktop. Use:
`oc port-forward service/productpage xxxx:yyyy`

Open a web browser to localhost:xxxx ... what do you see there?
Try /health ...

