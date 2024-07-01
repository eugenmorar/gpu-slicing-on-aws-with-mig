# gpu-slicing-on-aws-with-mig
GPU slicing is possible with NVIDIA Multi-Instance GPU (MIG) feature on EC2 P4d instances running NVIDIA A100 Tensor Core GPUs.

To run workloads that use gpu fractions on our eks cluster we need to choose the right instance and to install a plugin (k8s-device-plugin) on the gpu nodes.

We have two options:

A. Use a new managed node group in EKS running exclusively P4d because is dedicated and all the compute is going to ML.
B. Use the actual EKS cluster running Karpenter to autoscale to any type of workloads spawning P4d instances when ML workloads need to be scheduled. 

For workloads to use gpu slicing we need to create limits and selectors.

```
spec:
    containers:
        - name: gpu-example
          image: nvidia/cuda
          resources:
            limits: 
                nvidia.com/mig-1g.5gb: 1
    nodeSelector:
        nvidia.com/gpu.product: A100-PCIE-40GB
        nvidia.com/cuda.runtime: 11.4
        nvidia.com/cuda.driver: 470.161.03  
```