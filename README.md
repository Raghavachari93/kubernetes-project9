<h1>Project 9 — Monitoring Kubernetes (Python App + Prometheus + Grafana)</h1>

<hr/>

<h2>🎯 Goal (Real-World Context)</h2>
<p>You will:</p>
<ul>
  <li>Run a <strong>real Python application</strong></li>
  <li>Expose <strong>metrics</strong></li>
  <li>Install <strong>Prometheus</strong></li>
  <li>Visualize using <strong>Grafana</strong></li>
</ul>

<p>👉 This is exactly how companies monitor systems in production.</p>

<hr/>

<h2>🧠 Architecture</h2>

<p><strong>Flow:</strong></p>
<p>
  Python App → /metrics → Prometheus → Grafana Dashboard
</p>

<hr/>

<h2>📁 Project Structure</h2>
<pre>
kubernetes-project9/

├── app/
│   ├── app.py
│   └── requirements.txt

├── Dockerfile

├── k8s/
│   ├── deployment.yaml
│   ├── service.yaml

└── monitoring/
    └── prometheus-values.yaml
</pre>

<hr/>

<h2>🧩 Step 1 — Python App with Metrics</h2>
<p>We expose Prometheus metrics from a Flask application.</p>

<h3>📄 app/app.py</h3>
<p>Creates endpoints:</p>
<ul>
  <li><code>/</code> → increments request counter</li>
  <li><code>/metrics</code> → exposes Prometheus metrics</li>
</ul>

<h3>📄 requirements.txt</h3>
<ul>
  <li>flask</li>
  <li>prometheus_client</li>
</ul>

<hr/>

<h2>🐳 Step 2 — Dockerfile</h2>
<p>Containerizes the Python application and exposes port <strong>5000</strong>.</p>

<h3>🚀 Build Image</h3>
<pre>
eval $(minikube docker-env)
docker build -t python-monitoring-app:v1 .
</pre>

<hr/>

<h2>⚙️ Step 3 — Kubernetes Deployment</h2>

<h3>📄 deployment.yaml</h3>
<ul>
  <li>Creates <strong>2 replicas</strong></li>
  <li>Runs container on port <strong>5000</strong></li>
</ul>

<h3>📄 service.yaml</h3>
<ul>
  <li>Exposes app using <strong>NodePort</strong></li>
  <li>Accessible via port <strong>30012</strong></li>
</ul>

<h3>🚀 Deploy App</h3>
<pre>
kubectl apply -f k8s/
</pre>

<hr/>

<h2>📊 Step 4 — Install Prometheus + Grafana (Helm)</h2>

<h3>Check Helm</h3>
<pre>helm version</pre>

<h3>Add Repository</h3>
<pre>
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
</pre>

<h3>Install Monitoring Stack</h3>
<pre>
helm install monitoring prometheus-community/kube-prometheus-stack
</pre>

<hr/>

<h2>🔍 Step 5 — Access Grafana</h2>

<pre>
kubectl port-forward svc/monitoring-grafana 3000:80
</pre>

<p>Open in browser:</p>
<p><strong>http://localhost:3000</strong></p>

<h3>🔑 Login Credentials</h3>
<ul>
  <li><strong>Username:</strong> admin</li>
</ul>

<pre>
kubectl get secret monitoring-grafana -o jsonpath="{.data.admin-password}" | base64 --decode
</pre>

<hr/>

<h2>📈 Step 6 — View Metrics</h2>

<ul>
  <li>Go to <strong>Dashboards</strong></li>
  <li>Use Kubernetes dashboards OR create a custom panel</li>
</ul>

<p><strong>Custom Metric:</strong></p>
<pre>app_requests_total</pre>

<hr/>

<h2>🧪 Test Metrics</h2>

<pre>
minikube service monitoring-service
</pre>

<p>👉 Refresh multiple times to generate traffic</p>
<p>👉 Watch metrics increase in Grafana 🔥</p>

<hr/>

<h2>🧠 What You Just Built</h2>
<ul>
  <li>Metrics-enabled application</li>
  <li>Prometheus scraping</li>
  <li>Grafana visualization</li>
  <li>Complete observability pipeline</li>
</ul>

<hr/>

<h2>❌ Common Mistakes</h2>
<ul>
  <li>❌ <code>/metrics</code> endpoint not exposed</li>
  <li>❌ Prometheus not scraping app</li>
  <li>❌ Port mismatch</li>
  <li>❌ Helm not installed</li>
</ul>

<hr/>

<h2>🔍 Debug Like a Pro</h2>
<pre>
kubectl get pods
kubectl logs &lt;pod&gt;
kubectl get svc
</pre>

<hr/>

<h2>🔥 Final Note</h2>
<p>
This project moves you from guessing system behavior to 
<strong>real observability engineering</strong> — exactly what production teams do.
</p>
