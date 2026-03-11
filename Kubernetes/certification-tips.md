# Certification Tips

## Imperative commands

* to save time it's easier to create resources via commands, and output them to an yaml file, via the `-o yaml` flag
* you can also use the `--dry-run=client` to avoid creating the actual resource

* Example: Generate pod manifest YAML file, but don't actually create the pod:

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

* Example: Generate deployment definition file

```bash
kubectl create deployment --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

## Formatting the output

* The output of the commands can be formatted in different ways:
    * `-o json`
    * `-o name` print only the resource name and nothing else
    * `-o wide` output in plain text format with any additional info
    * `-o yaml` output a yaml formatted API object

