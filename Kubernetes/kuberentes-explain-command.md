# The explain command

## Listing the available API resources

```bash
kubectl api-resources
```

## Explain a resource

```bash
# Lists the top fields
kubectl explain <resource>
```

```bash
## Lists the field of the spec
kubectl explain <resource>.spec
```

```bash
# Lists all fields 
kubectl explain <resource> 