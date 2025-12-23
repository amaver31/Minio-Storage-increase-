# Minio-Storage-increase-
Increase Minio persistent storage of dev which is running on aks Cluster

# Step 1: Check MinIO PVC
kubectl get pvc -n minio

# Step 2: Ensure StorageClass allows expansion
kubectl get sc
kubectl describe sc managed-csi

allowVolumeExpansion: true (If ❌ not present → you must create a new StorageClass).

# Step 3: Edit PVC size (THIS is the key step)
kubectl edit pvc minio-data -n minio

spec:
  resources:
    requests:
      storage: 300Gi

# Step 4: Verify resize
kubectl get pvc minio-data -n minio
kubectl describe pvc minio-data -n minio

# Step 5: Restart MinIO pod (important)
kubectl rollout restart statefulset minio -n minio

OR

kubectl delete pod minio-0 -n minio

# Method 2

You can edit the persistant-storage in Values-dev.yaml from Azure devops repo which will be synk using ArgoCD. 


