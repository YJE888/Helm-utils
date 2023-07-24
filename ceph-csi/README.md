# Helm 을 사용하여 CephFS-CSI 설치
#### helm 설치
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh

./get_helm.sh
helm version
```

#### helm Repo 등록
```
helm repo add ceph-csi https://ceph.github.io/csi-charts
helm repo list
helm repo update
```

#### NS 생성
```
kubectl create namespace ceph-csi
```

#### vaule.yaml, secret, storageclass 정보 수정 및 배포
```
vi ceph-csi-cephfs-vaules.yaml
vi secret.yaml
vi storageclass.yaml

kubectl create -f secret.yaml
kubectl create -f storageclass.yaml
```

#### helm 차트 설치
```
helm install --namespace "ceph-csi" "ceph-csi-cephfs" ceph-csi/ceph-csi-cephfs --values ceph-csi-cephfs-values.yaml
helm status ceph-csi-cephfs -n ceph-csi

# 참고) 삭제
helm uninstall "ceph-csi-cephfs" --namespace "ceph-csi"
```
  
#### PVC, POD 생성 및 확인
```
kubectl create -f pvc-pod.yaml
```

##### ***Ceph-csi가 동작하지 않을 경우 provisioner deploy에서 ```--extra-create-metadata=true```로 변경해줘야됨***
