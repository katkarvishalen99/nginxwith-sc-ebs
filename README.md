# nginxwith-sc-ebs

Create deployment which will run a pod, mount a volume and store a temp.txt file or html file in volume.
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

  
