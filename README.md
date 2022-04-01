<h1> DESAFIO AULA 2 - Kubernetes </h1>

<p></p>
Passos para criação de cluster, usando k3d, para deployment de  Rotten Potatoes:
<p></p>

1: Comando: <b>docker image build -t pauloazevedo88/rotten-potatoes:v1 .</b> para criar a imagem

O output será semelhante a:
```bash
Sending build context to Docker daemon  14.63MB
Step 1/7 : FROM python:bullseye
bullseye: Pulling from library/python
dbba69284b27: Pull complete
9baf437a1bad: Pull complete
6ade5c59e324: Pull complete
b19a994f6d4c: Pull complete
8fc2294f89de: Pull complete
9dc715194c21: Pull complete
2bb16cc2dfa7: Pull complete
4bc546858b13: Pull complete
251bc2e5e4fa: Pull complete
Digest: sha256:8de5a838ee54cec783fae12eada6728171d3a76265ecb7200ed880a6519d6cba
Status: Downloaded newer image for python:bullseye
 ---> 73381281305e
Step 2/7 : WORKDIR /app
 ---> Running in 31d2aae7889b
Removing intermediate container 31d2aae7889b
 ---> ddf31cc57a06
Step 3/7 : COPY requirements.txt .
 ---> e404484888fe
Step 4/7 : RUN python -m pip install -r requirements.txt
 ---> Running in e3d4e6c70b4e
Collecting click==7.1.2
  Downloading click-7.1.2-py2.py3-none-any.whl (82 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 82.8/82.8 KB 4.3 MB/s eta 0:00:00
     .
     .
     .
  Downloading WTForms-3.0.0-py3-none-any.whl (136 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 136.3/136.3 KB 9.8 MB/s eta 0:00:00
Building wheels for collected packages: MarkupSafe, Pillow, prometheus-flask-exporter, pymongo, PyYAML
Successfully built MarkupSafe Pillow prometheus-flask-exporter pymongo PyYAML
Installing collected packages: Werkzeug, PyYAML, pymongo, prometheus-client, Pillow, MarkupSafe, itsdangerous, idna, gunicorn, dnspython, click, WTForms, mongoengine, Jinja2, flask-prometheus-metrics, email-validator, Flask, prometheus-flask-exporter, Flask-WTF, flask-mongoengine
Successfully installed Flask-1.1.2 Flask-WTF-0.14.3 Jinja2-2.11.2 MarkupSafe-1.1.1 Pillow-8.2.0 PyYAML-5.3.1 WTForms-2.3.3 Werkzeug-1.0.1 click-7.1.2 dnspython-2.1.0 email-validator-1.1.2 flask-mongoengine-1.0.0 flask-prometheus-metrics-1.0.0 gunicorn-20.0.4 idna-3.1 itsdangerous-1.1.0 mongoengine-0.23.1 prometheus-client-0.10.1 prometheus-flask-exporter-0.18.2 pymongo-3.11.4
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
Removing intermediate container e3d4e6c70b4e
 ---> 5c1f7753a466
Step 5/7 : COPY . /app
 ---> 4a32448fa48d
Step 6/7 : EXPOSE 5000
 ---> Running in 5f48e63e9636
Removing intermediate container 5f48e63e9636
 ---> 3523dfacfd15
Step 7/7 : CMD [ "gunicorn" , "--workers=2", "--bind", "0.0.0.0:5000", "-c", "config.py", "app:app"]
 ---> Running in c2b8ddb31043
Removing intermediate container c2b8ddb31043
 ---> 3a4745e39d98
Successfully built 3a4745e39d98
Successfully tagged pauloazevedo88/rotten-potatoes:v1
```

Comando: <b>docker image ls</b> 

O output será semelhante a:
```bash
REPOSITORY                             TAG               IMAGE ID       CREATED          SIZE
pauloazevedo88/rotten-potatoes         v1                3a4745e39d98   13 seconds ago   1.01GB
```

2- Comando: <b>docker image push pauloazevedo88/rotten-potatoes:v1</b> para publicar a imagem no <a href="https://hub.docker.com/">Docker Hub</a>

O output será semelhante a:
```bash
The push refers to repository [docker.io/pauloazevedo88/rotten-potatoes]
71382145350f: Pushed
62f7b769b8b7: Pushed
c9f348d2d948: Pushed
e26827c5221d: Pushed
e8635f15dd3d: Mounted from library/python
147cd3e8f800: Mounted from library/python
0fd04db47c1e: Mounted from library/python
89f49a7a4e4a: Mounted from library/python
c5579a205adc: Mounted from library/python
7a7698da17f2: Mounted from library/python
d59769727d80: Mounted from library/python
348622fdcc61: Mounted from library/python
4ac8bc2cd0be: Mounted from library/python
v1: digest: sha256:7a9fc1bb147ac6ff609926fcc39bf90a4792d4563a8b784ba23383a1860ab090 size: 3056
```

3- Comando: <b>k3d cluster create rottenpotatoes --servers 2 --agents 2 -p "8080:30000@loadbalancer"</b>

O output será semelhante a:
```bash
INFO[0000] portmapping '8080:30000' targets the loadbalancer: defaulting to [servers:*:proxy agents:*:proxy]
INFO[0000] Prep: Network
INFO[0000] Created network 'k3d-rottenpotatoes'
INFO[0000] Created image volume k3d-rottenpotatoes-images
INFO[0000] Starting new tools node...
INFO[0000] Creating initializing server node
INFO[0000] Creating node 'k3d-rottenpotatoes-server-0'
INFO[0000] Starting Node 'k3d-rottenpotatoes-tools'
INFO[0001] Creating node 'k3d-rottenpotatoes-server-1'
INFO[0001] Creating node 'k3d-rottenpotatoes-agent-0'
INFO[0001] Creating node 'k3d-rottenpotatoes-agent-1'
WARN[0001] You're creating 2 server nodes: Please consider creating at least 3 to achieve etcd quorum & fault tolerance
INFO[0001] Creating LoadBalancer 'k3d-rottenpotatoes-serverlb'
INFO[0001] Using the k3d-tools node to gather environment information
INFO[0002] HostIP: using network gateway 172.27.0.1 address
INFO[0002] Starting cluster 'rottenpotatoes'
INFO[0002] Starting the initializing server...
INFO[0002] Starting Node 'k3d-rottenpotatoes-server-0'
INFO[0004] Starting servers...
INFO[0004] Starting Node 'k3d-rottenpotatoes-server-1'
INFO[0023] Starting agents...
INFO[0023] Starting Node 'k3d-rottenpotatoes-agent-1'
INFO[0023] Starting Node 'k3d-rottenpotatoes-agent-0'
INFO[0030] Starting helpers...
INFO[0031] Starting Node 'k3d-rottenpotatoes-serverlb'
INFO[0038] Injecting records for hostAliases (incl. host.k3d.internal) and for 5 network members into CoreDNS configmap...
INFO[0040] Cluster 'rottenpotatoes' created successfully!
INFO[0040] You can now use it like this:
kubectl cluster-info
```

4- Comando: <b>kubectl apply -f Deployment.yaml</b>

Variáveis de ambiente para applicação web Rotten Tomatoes:

<p>MONGODB_DB => Nome do database</p>
<p>MONGODB_HOST => Host do MongoDB</p>
<p>MONGODB_PORT => Posta de acesso ao MongoDB</p>
<p>MONGODB_USERNAME => Usuário do MongoDB</p>
<p>MONGODB_PASSWORD => Senha do MongoDB</p>

O output será semelhante a:
```bash
deployment.apps/mongodb created
service/mongodb created
deployment.apps/rottentomatoes created
service/rottentomatoes created
```
Comando: <b>kubectl get all</b>
O output será semelhante a:
```bash
NAME                                 READY   STATUS    RESTARTS   AGE
pod/mongodb-66b8d4645-lpkk8          1/1     Running   0          10m
pod/rottentomatoes-cf7f4b4fd-8mslf   1/1     Running   0          5m16s

NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/kubernetes       ClusterIP   10.43.0.1       <none>        443/TCP        10m
service/mongodb          ClusterIP   10.43.138.133   <none>        27017/TCP      10m
service/rottentomatoes   NodePort    10.43.73.101    <none>        80:30000/TCP   4m44s

NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongodb          1/1     1            1           10m
deployment.apps/rottentomatoes   1/1     1            1           5m16s

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/mongodb-66b8d4645          1         1         1       10m
replicaset.apps/rottentomatoes-cf7f4b4fd   1         1         1       5m16s
```

<h1> DESAFIO AULA 3 - Pipeline CICD </h1>

<p></p>
Passos para criação de cluster, usando k3d, para deployment de  Rotten Potatoes:
<p></p>

1- Após a criação de cluster na cloud DigitalOcean. Comando <b>kubectl get nodes</b>

O output será semelhante a:
```bash
NAME                 STATUS   ROLES    AGE     VERSION
pool-default-ccb9d   Ready    <none>   7m41s   v1.22.7
pool-default-ccb9v   Ready    <none>   7m31s   v1.22.7
```

Comando <b>kubectl get nodes</b>
O output será semelhante a:
```bash
deployment.apps/mongodb created
service/mongodb created
deployment.apps/rottentomatoes created
service/rottentomatoes created
```