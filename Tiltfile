SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='tanzupartnerworkshop.azurecr.io/tap15/build-service/partnertapdemo-tap-install')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='tap-dev4')
APPNAME = os.getenv("APP_NAME", default='partnertapdemo')
CLUSTERNAME = os.getenv("CLUSTER_NAME", default='tap15-aks-fullcluster')

k8s_custom_deploy(
    APPNAME,
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload " + APPNAME + " --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource( APPNAME , port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': APPNAME }])
update_settings ( max_parallel_updates = 3 , k8s_upsert_timeout_secs = 600 , suppress_unused_image_warnings = None )
allow_k8s_contexts('tap15-aks-fullcluster')
