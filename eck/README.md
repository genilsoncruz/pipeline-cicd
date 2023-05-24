https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-quickstart.html

https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-beat-quickstart.html

watch kubectl get all --all-namespaces

kubectl create -f https://download.elastic.co/downloads/eck/2.5.0/crds.yaml
kubectl apply -f https://download.elastic.co/downloads/eck/2.5.0/operator.yaml
kubectl -n elastic-system logs -f statefulset.apps/elastic-operator

cat <<EOF | kubectl apply -f -
  apiVersion: elasticsearch.k8s.elastic.co/v1
  kind: Elasticsearch
  metadata:
    name: quickstart
  spec:
    version: 8.5.3
    nodeSets:
    - name: default
      count: 1
      config:
        node.store.allow_mmap: false
  EOF
  kubectl get elasticsearch
  kubectl get pods --selector='elasticsearch.k8s.elastic.co/cluster-name=quickstart'
  kubectl logs -f quickstart-es-default-0
  kubectl describe pod/quickstart-es-default-0

kubectl get pods --selector='elasticsearch.k8s.elastic.co/cluster-name=quickstart' 
kubectl logs -f quickstart-es-default-0
kubectl get service quickstart-es-http

PASSWORD=$(kubectl get secret quickstart-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')
curl -u "elastic:$PASSWORD" -k "https://quickstart-es-http:9200"
kubectl logs -f quickstart-es-default-0
PASSWORD=$(kubectl get secret quickstart-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')
curl -u "elastic:$PASSWORD" -k "https://quickstart-es-http:9200"
curl -u "elastic:$PASSWORD" -k "https://localhost:9200"

cat <<EOF | kubectl apply -f -
apiVersion: kibana.k8s.elastic.co/v1
kind: Kibana
metadata:
   name: quickstart
spec:
  version: 8.5.3
  count: 1
  elasticsearchRef:
    name: quickstart
EOF

kubectl get kibana
kubectl get pod --selector='kibana.k8s.elastic.co/name=quickstart'
kubectl get service quickstart-kb-http
kubectl get secret quickstart-es-elastic-user -o=jsonpath='{.data.elastic}' | base64 --decode; echo

kubectl port-forward service/quickstart-es-http 9200
kubectl port-forward service/quickstart-kb-http 5601

echo $PASSWORD
  
cat <<EOF | kubectl apply -f -
apiVersion: beat.k8s.elastic.co/v1beta1
kind: Beat
metadata:
  name: quickstart
spec:
  type: filebeat
  version: 8.5.3
  elasticsearchRef:
    name: quickstart
  config:
    filebeat.inputs:
    - type: container
      paths:
      - /var/log/containers/*.log
  daemonSet:
    podTemplate:
      spec:
        dnsPolicy: ClusterFirstWithHostNet
        hostNetwork: true
        securityContext:
          runAsUser: 0
        containers:
        - name: filebeat
          volumeMounts:
          - name: varlogcontainers
            mountPath: /var/log/containers
          - name: varlogpods
            mountPath: /var/log/pods
          - name: varlibdockercontainers
            mountPath: /var/lib/docker/containers
        volumes:
        - name: varlogcontainers
          hostPath:
            path: /var/log/containers
        - name: varlogpods
          hostPath:
            path: /var/log/pods
        - name: varlibdockercontainers
          hostPath:
            path: /var/lib/docker/containers
EOF

kubectl get beat
kubectl get pods --selector='beat.k8s.elastic.co/name=quickstart'
kubectl get logs quickstart-beat-filebeat-nllg2
kubectl logs quickstart-beat-filebeat-nllg2
curl -u "elastic:$PASSWORD" -k "https://localhost:9200/filebeat-*/_search"

cat <<EOF | kubectl apply -f -
apiVersion: elasticsearch.k8s.elastic.co/v1
kind: Elasticsearch
metadata:
  name: quickstart
spec:
  version: 8.5.3
  nodeSets:
  - name: default
    count: 3
    config:
      node.store.allow_mmap: false
EOF

kubectl get pods --selector='elasticsearch.k8s.elastic.co/cluster-name=quickstart'
kubectl get elasticsearch
kubectl get pod/quickstart-es-default-1
kubectl describe pod/quickstart-es-default-1