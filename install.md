# platform Install

## pre-install
- 이 스크립트는 deepops github 소스에 추가하여 사용하는 것을 원칙으로 한다.
```bash
# 스크립트 및 설정파일들을 deepops 레포지토리에 복사
$ cp haiqv-config/scripts/* deepops/scripts/k8s/
$ cp -r haiqv-config/files deepops/config
$ cp -r haiqv-config/group_vars deepops/config
$ cp -r haiqv-config/helm deepops/config
```

## haiqv db install
```bash
$ cd deepops
$ ./scripts/k8s/deploy_dex_postgresql_haitf.sh
```

## monitoring
```bash
$ ./scripts/k8s/deploy_monitoring_haitf.sh
```

## haiqv portal

backend에서 사용할 kubeconfig를 마스터 노드에서 추출하여 
deepops/config/files/haiqv/backend/base/config.map 에 적용
```bash
$ ./scripts/k8s/deploy_kubeflow_haitf.sh
```

## monitoring 후속조치
```bash
# grafana init job
$ kubectl apply -f config/files/grafana/configmap.yaml
$ kubectl apply -f config/files/grafana/job.yaml
```

## logging
```bash
$ kubectl create ns logging
# 파일떨구는 용으로 es에 마운트할 pvc 생성 (PoC 한정)
$ kubectl apply -f config/files/logging/elasticsearch/pvc.yaml
$ ./scripts/k8s/deploy_logging_elasticsearch_haitf.sh
$ kubectl apply -f config/files/logging/elasticsearch/cronjob.yaml

$ ./scripts/k8s/deploy_logging_filebeat_haitf.sh
$ ./scripts/k8s/deploy_logging_kibana_haitf.sh
$ ./scripts/k8s/deploy_logging_metricbeat_haitf.sh
```
