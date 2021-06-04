# curso Kubernates - K8s
##### Guiá practica de algunos de los comandos mas utilizados

* listar los pod existentes dentro del cluster
```
kubectl get pods
```

* ¿Cómo se crea un pod?
```
kubectl run --generator=run-pod/v1 podtest --image=nginx:alpine
```

* ¿Cuando necesitamos conocer qué le paso a un objeto en el tiempo?
```
kubectl describe pod {nombre del pod}
```

* Para conocer todos los recursos expuesto por la api de k8s:
```
kubectl api-resources
```

* Eliminación de pod: 
```
kubectl delete pod {nombre del pod } {nombre del pod}
```

* Para obtener la declaración de cómo fue creado este pod o el manifiesto 
```
kubectl get pod {nombre pos} -o {yaml}{json}
```

* Para ver el contenido de lo que corre dentro de nuestro pod lo que hacemos es tomar la ip del pod y pegarla en el navegador web, **ojo esto solo si el cluster es nuestra propia máquina local,** *en el caso contrario toca hacer un* :
Para el caso en que desde tu local quieras hacer una re-dirección a tu pod: 
```
kubectl port-forward <pod-name> 7000:<pod-port>
```

* Para hacer un port-forward en un deployment:
```
kubectl port-forward deployment/back-deployment 7000:80 
```

* Para hacer un port-forward en un rs:
```
kubectl port-forward replicaset/back-deployment-647779c767 7000:80
```

* Para hacer un port-forward en un service:
```
kubectl port-forward service/servicio-node-port 7000:9000
```

Luego, solo vas a http://localhost:7000 y deberías ver tu pod!

En el caso que necesitas mas detalle de como hacer otros tipos de comunicaciones tenemos este [link](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/)


* Para conocer los eventos reciente del cluster:
```
kubectl --kubeconfig ~/.kube/config_aks --context apolo-apicore-aks -n oapi-dev get events
```

Para la creacion de ConfigMap y Secretos:
```
kubectl --kubeconfig ~/.kube/config_aks --context apolo-apicore-aks -n oapi-dev create configmap nombre_config_map --from=config.properties

kubectl --kubeconfig ~/.kube/config_aks --context apolo-apicore-aks -n oapi-dev create secret generic nombre_secret --from=secretos.properties

```

* Para la consulta de los ConfigMap y Secrets
```
kubectl --kubeconfig ~/.kube/config_aks --context apolo-apicore-aks -n oapi-dev get configmap

kubectl --kubeconfig ~/.kube/config_aks --context apolo-apicore-aks -n oapi-dev get secrets
```
* Para acceder a los pod cuanto están ejecutándose:
 ```
 kubectl -n nombre_namespaces exec -it nombre_pods -- /bin/bash
 ```

 * Para acceder a los log de un pod: 
```
kubectl logs {nombre del pod} 
```
en el caso que necesitemos hacer seguimiento lo podemos hacer agregando -f al final
```
kubectl logs {nombre del pod} -f 
```

en el caso que necesitemos hacer seguimiento lo podemos hacer agregando -f al final y al final de las lineas
```
kubectl -n nombre_namespaces logs -f nombre_contenedor --tail=50
```

* Para obtener la lista de versiones de la api de K8s tenemos:
```
kubectl api-version
```

* Para la creación de Pod mediante algún manifiesto, que será la forma en como se realizara:
```
kubectl apply -f {archivo_pod_yaml}
```

* Para la eliminación de pod partiendo de un archivo manifest utilizamos:
```
kubectl delete -f {nombre y ruta del archivo yaml}
```

##### Recordemos que para la creación de múltiples pod dentro de un mismo yaml, tan solo basta con separar dentro de mismo yaml los descripciones del uno con el otro por medio de "---"

* Cuando necesitamos acceder a los logs de contenedores que existen en un pod con mas de un contenedor:
```
kubectl logs {nombre-pod} -c {nombre del contenedor}
```

* Para acceder  a uno de esos contenedores que corre dentro del pod:
```
kubectl exec -ti dos-cont -c {nombre-contenedor} -- sh
```

* Para instalar curl dentro de una maquina linux:
apk add -U curl

* Aplicar filtros a nivel de labels en un k8s:
```
kubectl get pods -l app=backend
```

* Los comandos apply funcionan para todos, en este caso aplicaran para los replicaSet que tenemos:
```
kubectl apply -f replicaset.yaml
```

* Los filtros en los replicaSet cumplen lo misma filosofía:
```
kubectl get pods -l app=pod-labels
```

* Para listar los replicaSet utilizamos los comandos:
```
kubectl get rs 
```
* O tambien podemos utilizar la forma larga:
```
kubectl get replicaset
```

* Tener presente que los replicaSet también le podemos obtener sus descripción y sus manifiesto: 
```
kubectl get rs {nombre_replicaset}-o yaml
```
```
kubectl describe rs {nombre_rs}
```

* Para mostrar la lista le labels asociados a un pod tenemos el siguiente comando, ademas de informar que la lista de despliegues siempre es con apply:

```
kubectl apply -f deployment.yaml
```
* Para realizar un deployment de una version junto con la razon o la causa de cambio utilizamos el flag **--record**:

```
kubectl apply -f deployment.yaml --record
```
+ **_Recordemos que en este punto anterior tenemos diferentes formas de depositar la causa y una es por medio de este flag, pero hiciste otras:_**

* Para listar los pods con sus respectivos labels tenemos el comando:           
 ```
 kubectl get pods --show-labels
 ```

***SUPER COMANDO***
* Para conocer el estado de un deployment tenemos el siguiente comando:
Tengamos presente que un rollout es lo nuevo del despliegue:
```
kubectl rollout status deployment {nginx-deployment} 
```
este ultimo es el nombre del deployment.

* En el caso que se requiera conocer el estado sin saber la traza completa del rollout podemos aplicar el siguiente :
```
kubectl rollout status deployment error-deployment --watch=false
```

* Para conocer el el historial de deployment realizados por deployment tenemos:
```
kubectl rollout history deployment {front-deployment} nombre del 
deploym
```

**_Para la creación de archivos de configuración y de acceso a los a sus diferentes cluster les dejo el s_**
[Dentro de este link](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/) encontraras un detalle mas completo y oficial.

* Para setear la configuración que necesitas que rija en el momento que necesites acceder al cluster:

```
kubectl config --kubeconfig=config-demo set-context dev-frontend --cluster=development --namespace=frontend --user=developer
```

Para visualizar el archivo de configuraciones
```
kubectl config --kubeconfig=config-demo view
```
* Para visualizar el contexto actual de k8s:
```
kubectl config current-context
```

* Para visualizar el contenido del contexto actual tenemos:
```
kubectl config view
```

* Para cambiarnos entre los diferentes contextos de nuestro archivo:
```
kubectl config use-context apicore-desa
```


* Para conocer la lista de namespace:
```
kubectl get namespace
```

* Para acceder a recursos del cluster partiendo de un namespace:kubectl get pods 
```
kubectl get pods  -n =<insert-namespace-name-here>
```

* Para acceder a una lista extensa de comandos tenemos:
[Link](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#rollout)

#### *Para realizar un roll back a una revision anterior por causa de algún problema*
```
kubectl rollout undo deployment {front-deployment} --to-revision={2}
```
*recordemos que la revisiones las obtenemos con el history.*

* Para realizar el despliegue de un servicio en diferente ruta a la del deployment:

```
kubectl apply -f service.yaml -f ../deployment/deployment-version.yaml --record
```
##### Pero se hace la aclaracion dentro de un archivo yaml podemos declarar la creacion de varios objetos de k8s mediante el sepadaros de objetos *(---)*

* Para explorar la descripción de un service creado:

```
kubectl get svc --show-labels
```
Este flag del final es para aplicar el filtro por los labels  o podemos aplicar:

```
kubectl get svc -l app=nginx-front
```

* Para describir el service e identificar que lo compone, tenemos:
```
kubectl describe svc my-service
```

* Para describir los endpoints asociados a nuestro service, tenemos:
```
kubectl get endpoints
```

### Como hoja de trucos tenemos esta [enlace](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

* Para listar los pod con mas detalle tenemos:
```
kubectl get pods -o wide 
```

* Para la creación de pod que pueda acceder a ellos de forma interactiva ademas de borrarse al salir de el:
```
kubectl run -ti --rm --generator=run-pod/v1 podtest --image=nginx:alpine -- sh
```

* Para conocer la descripción del node en que se esta corriendo nuestros servicios:

```
kubectl describe node 
```

* Para conocer el conjunto de namespace existentes en un cluster:

```
kubectl get namespaces
```

* Para conocer los pod existentes dentro de un namespaces:

```
kubectl get pods --namespace default
```
una forma abreviada es:
```
kubectl get pods -n default
```

* Para conocer todo lo que tenemos creado:

```
kubectl get all
```

* Para crear nuevos namespaces:

```
 kubectl apply -f namespaces.yaml

 kubectl create namespace pedro
```

* Para la eliminacion de un namespaces
```
kubectl delete namespace nombre_namespaces

kubectl delete -f namespaces.yaml
```

* Para correr los pod, abrirlo de modo interactivo y eliminarlo apenas salgas de la consola:

```
kubectl run -ti --rm --generator=run-pod/v1 podtest --image=nginx:alpine -- sh
```

* Para la creación de un pod dentro de un namespace propio:

```
kubectl run -ti --rm --generator=run-pod/v1 podtest --image=nginx:alpine --namespace development 
```

* Para la eliminación de un pod de un namaspace especifico:

```
kubectl delete pod nombre_pod -n nombre_namaspace 
```

* Como se construye el dns de un deployment generado apartir de una namespace:

```
nombre_servicio.nombre_namespace.svc.cluster.local
```

* Para scale de un deployment:

```
kubectl --kubeconfig ~/.kube/config_aks --context apolo-apicore-aks -n oapi-uat scale deployment/ms-validaciones-ci-facade --replicas=1
```

