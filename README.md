# kubernetes-mongodb-statefulset
kubernetes-mongodb-statefulset
## Files

- **`service.yml`**: Defines the Kubernetes Service for MongoDB. The service is of type `ClusterIP` set to `None`, enabling stable DNS records for each MongoDB pod.
- **`statefulset.yml`**: Defines the StatefulSet for MongoDB with 2 replicas. Each pod is configured to mount a persistent volume at `/data/db`.
- 
## Configuration Details

### Service (`service.yml`)
- **Name**: `mongodb`
- **Cluster IP**: `None` (Required for StatefulSet)
- **Port**: 27017 (default MongoDB port)
- **Target Port**: 27017 (mapped to the MongoDB container)

### StatefulSet (`statefulset.yml`)
- **Name**: `mongodb`
- **Replicas**: 2
- **Container Image**: `mongo:4.0.17`
- **Container Port**: 27017
- **Volume Mount**: `/data/db` (for MongoDB data)
- **Volume Claim Template**: 
  - **Access Mode**: `ReadWriteOnce`
  - **Storage Request**: 1Gi

## Usage

1. **Deploy the Service**:
   ```bash
   kubectl apply -f service.yml
   ```

2. **Deploy the StatefulSet**:
   ```bash
   kubectl apply -f statefulset.yml
   ```

3. **Verify the Deployment**:
   ```bash
   kubectl get pods -l app=mongodb
   ```

4. **Access MongoDB**:
   - Each MongoDB pod can be accessed individually via its DNS name, following the pattern `<statefulset_name>-<pod_index>.<service_name>`, e.g., `mongodb-0.mongodb` and `mongodb-1.mongodb`.

## Notes

- The StatefulSet ensures that each MongoDB replica has its own persistent storage.
- The Service with `clusterIP: None` provides stable network identities to the pods.

## Future Enhancements

- Adding readiness and liveness probes to ensure that the MongoDB instances are healthy.
- Defining resource requests and limits for better resource management.
- Implementing anti-affinity rules to ensure MongoDB replicas are distributed across nodes for higher availability.

