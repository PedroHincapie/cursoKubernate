# cursoKubernate
Este repositorio represente los estudios realizando del curso de k8s

Para empezar tenemos :
Listar los pod de un cluster : kubectl get pods

Crear un pod :  kubectl run --generator=run-pod/v1 podtest --image=nginx:alpine

Cuando necesitamos conocer que le paso a un objeto en el tiempo :  kubectl describe pod {nombre del pod}

Para cdesonocer todos los recursos expuesto por la api de k8s: kubectl api-resources

Eliminacion de pod: kubectl delete pod {nombre del pod } {nombre del pod} ...

Para obtener la declaracion de como fue creado este pod o el manifiesto de como fue creado el pod en k8s: kubectl get pod {nombre pos} -o {yaml}{json}

Para ver el contenido de lo que corre dentro de nuestro pod lo que hacemos es tomar la ip del pod y pegarla en el navegador web, ojo esto solo si el cluster es nuestra propia maquina local, en el caso contrarrio toca hacer un :
Para el caso en que desde tu local quieras hacer una redireccion a tu pod: 
kubectl port-forward <pod-name> 7000:<pod-port>
Luego, solo vas a http://localhost:7000 y deberías ver tu pod!

Para acceder a los pod cuanto estan ejecutandose:
 kubectl exec -ti {nombre del pod} -- sh


Para acceder a los log de un pod: kubectl logs {nombre del pod} 
en el caso que necesitemos hacer seguimiento lo podemos hacer agregandole -f al final

Para obtener la lista de versiones de la api de K8s tenemos:
kubectl api-version
De esta forma obtenemos la lista

Para la creacion de Pod mediante algun manifiesto, que sera la forma en como se realizara:
kubectl apply -f {archivo_pod_yaml}

Para la eliminacion de pod partiendo de un archivo manifiest utilizamos:
kubectl delete -f {nombre y ruta del archivo yaml}

Recordemos que para la creacion de multiples pod dentro de un mismo yaml, tan solo basta con separar dentro de mismo yaml los descripciones del uno con el otro por medio de "---"

Cuando necesitamos acceder a los logs de contenedores que existen en un pod con mas de un contenedor:
kubectl logs {nombre-pod} -c {nombre del contenedor}

Para acceder  a uno de esos contenedores que corre dentro del pod:
kubectl exec -ti dos-cont -c {nombre-contenedor} -- sh

para instalar curl dentro de una maquina linux:
apk add -U curl

Aplicar filtros a nivel de labels en un k8s:
kubectl get pods -l app=backend

Los comandos apply funcionan para todos, en este caso aplicaran para los replicaSet que tenemos:
kubectl apply -f replicaset.yaml

Los filtros en los replicaset cumplen lo misma filosofia:
kubectl get pods -l app=pod-labels

para listar los replicaset utilizamos los comandos:
       kubectl get rs 
O tambien podemos utilizar la forma larga:
      kubectl get replicaset

Tener presente que los replicaset tambien le podemos obtener sus describer y sus manifiesto: 
    kubectl get rs {nombre_replicaset}-o yaml
    kubectl describe rs {nombre_rs}

Para motrar la lista le labels asociados a un pod tenemos el siguiente comando, adems de informar que la lista de despliegues siempres es con apply:

              kubectl apply -f deployment.yaml
Para listar los pods con sus respectivos labels tenemos el comando:           
 kubectl get pods --show-labels

SUPER COMANDO
Para concer el estado de un deployment tenemos el siguiente comando:
Tengamos presente que un rollout es lo nuevo del despligue:
              kubectl rollout status deployment {nginx-deployment} este ultimo es el nombre del deployment.

En el caso que se requiera conocer el estado sin saber la traza completa del rollout podemos aplicar el siguiente :
      kubectl rollout status deployment error-deployment --watch=false

Para concer el el historial de deployment realizados por deployment tenemos:
kubectl rollout history deployment {front-deployment} nombre del deployment

Para visualizar el contexto actula de k8s:
  kubectl config current-context

Para visualizar el contenido del contexto actual tenemos:
     kubectl config view