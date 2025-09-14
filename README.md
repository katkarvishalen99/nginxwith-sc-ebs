# nginxwith-sc-ebs

POC :-

Create deployment which will run a pod, mount a volume and store a abc.txt file or html file in volume.
Then delete pod and once new pod is created as a part of same deployment, 
then you should still be able to see or access abc.txt or html file which was created in old pod.

AccessModes : RWO

PV autocreation with storageClass will be kind of building block for real-world implementation

Solution:
# Associate OIDC Provider

eksctl utils associate-iam-oidc-provider \
  --region ap-south-1 \
  --cluster eks-cluster \
  --approve

# Create IAM Role for CSI Driver - IRSA

eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa \
  --namespace kube-system \
  --cluster eks-cluster \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve

# Install CSI Driver Add-on   - use role name which is created using above IRSA

eksctl create addon \
  --name aws-ebs-csi-driver \
  --cluster eks-cluster \
  --service-account-role-arn arn:aws:iam::509054413276:role/eksctl-eks-cluster-addon-iamserviceaccount-ku-Role1-dHiYYSjWS7cD \
  --region ap-south-1 \
  --force

# Delete CSI driver:

  eksctl delete addon \
  --name aws-ebs-csi-driver \
  --cluster eks-cluster \
  --region ap-south-1

aws iam get-role --role-name eksctl-eks-cluster-addon-iamserviceaccount-ku-Role1-dHiYYSjWS7cD 

check oidc of cluster and update oidc in role trust policy

IRSA and trust policy:

<img width="1870" height="656" alt="image" src="https://github.com/user-attachments/assets/75f34192-35ca-4103-b7ed-7655e39055a9" />

EKS CLUSTER:

<img width="1903" height="766" alt="image" src="https://github.com/user-attachments/assets/82533914-aff6-434f-a82f-18925f539fdc" />




  
