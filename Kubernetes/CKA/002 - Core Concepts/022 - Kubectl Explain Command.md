
### Kubectl Explain Command

`kubectl api-resources`
- Gives all Kubernetes resources and its api versions etc

`kubectl explain <resource_name_like_pod/deploy/service/etc>`
- Gives a summary of the Kubernetes resource specified
- Explains all top-level fields in the definition file

`kubectl explain deploy.spec.replicas`
- Gives specific explanation of the field in the definition file

`kubectl explain <resource_name_like_pod/deploy/service/etc> --recursive`
- Gives all details of all the fields in the definition file
