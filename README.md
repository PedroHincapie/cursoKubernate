# curso Kubernate K8s
#### Este repositorio representa los estudios realizando del curso de K8S.

Dentro del presente encontraremos todos los comandos necesarios para la administración del mismo:
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

Luego, solo vas a http://localhost:7000 y deberías ver tu pod!

* Para acceder a los pod cuanto están ejecutándose:
 ```
 kubectl exec -ti {nombre del pod} -- sh
 ```

 * Para acceder a los log de un pod: 
```
kubectl logs {nombre del pod} 
```
en el caso que necesitemos hacer seguimiento lo podemos hacer agregando -f al final
```
kubectl logs {nombre del pod} -f 
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
* Para conocer la lista de namespace:
```
kubectl get namespace
```

* Para acceder a recursos del cluster partiendo de un namespace:kubectl get pods 
```
kubectl get pods --namespace=<insert-namespace-name-here>
```

* Para acceder a una lista extensa de comandos tenemos:
[Link](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#rollout)

#### *Para realizar un roll back a una revision anterior por causa de algún problema*
```
kubectl rollout undo deployment {front-deployment} --to-revision={2}
```
*recordemos que la revisiones las obtenemos con el history.*

