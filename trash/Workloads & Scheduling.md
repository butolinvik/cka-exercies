# Understand deployments and how to perform rolling update and rollbacks
# Use ConfigMaps and Secrets to configure applications
# Exercies configMaps
--- Создать configmap с именем confmap , со значениями db_name=name_db , port=1443
apiVersion: v1
kind: ConfigMap
metadata:
  creationTimestamp: null
  name: confmap
data:
  db_name: name_db
  port: "1443"
# Know how to scale applications
# Understand the primitives used to create robust, self-healing, application deployments
# Understand how resource limits can affect Pod scheduling
# Awareness of manifest management and common templating tools