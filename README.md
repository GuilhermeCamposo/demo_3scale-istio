# 3scale and Service Mesh Demo

## Environment

- OCP 4.9
- oc  >= OCP 4.9 installed in the controller host
- 3scale 2.11


## How to run the playbook

    ansible-playbook -e token= -e domain= install-demo.yml


## Relax Limits

If your environment has limits set, you may need to adjust it as in this example:

    oc patch -n istio-system limitRange istio-system-core-resource-limits --type='json' -p='[{"op": "replace","path":"/spec/limits/0/max/cpu", "value":"6"}, {"op": "replace","path":"/spec/limits/1/max/cpu", "value":"8"}]'    



## Generate 3scale Istio Adapter Objects

In case you want to generate adapters

    oc delete handler threescale &&  oc delete instance threescale && oc delete rule threescale

    export NS="istio-system" URL="https://3scale-admin.apps.${domain}" NAME="threescale" TOKEN=""

    oc exec -n ${NS} $(oc get po -n ${NS} -o jsonpath='{.items[?(@.metadata.labels.app=="3scale-istio-adapter")].metadata.name}') \
    -it -- ./3scale-config-gen --url ${URL} --name ${NAME} --token ${TOKEN} -n ${NS}
