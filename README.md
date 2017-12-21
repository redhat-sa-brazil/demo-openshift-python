## Sobre
 
Este repositório hospeda um aplicativo Python muito simplista usado para demonstração dos recursos da plataforma OpenShift. Esta aplicação usa:

* Flask - Web Micro-framework
* PyMongo - MongoDB Interface

## Capacidades

* Criação de um aplicativo Python usando Source-to-Image
* Uso de Secret e ConfigMap para personalizar o comportamento do aplicativo
* Integração através da camada de serviço (Service Discovery com base no DNS)
* Escalabilidade horizontal do componente de frontend (web), sem afetar o componente de backend (db)
* Uso de Pipeline com Jenkins

## Roteiro de Demonstração

1. Crie um projeto para hospedar o aplicativo e seus recursos:
```
$ oc new-project demo-openshift
```
2. Crie o componente de frontend (web) do aplicativo:
```
$ oc new-app https://github.com/redhat-sa-brazil/demo-openshift-python.git' --name='web'
```
3. Exponha uma rota pública para o componente web e teste-o:
```
$ oc expose service/web --hostname='demo.apps.ocp.acme.com'
```
4. Escale o component:
```
$ oc scale dc web --replicas=2
```
5. Crie o ConfigMap e verifique se o componente web será redistribuído:
```
$ oc create -f extras/web-configmap.yaml
```
6. Atualize o DeploymentConfig do componente da Web para usar o ConfigMap e valide o resultado (/webconfig):
```
> Use a GUI para demonstrar simplicidade.
```
7. Implante o componente db-tier com base no modelo MongoDB Persistent:
```
> Use a GUI para demonstrar outra maneira de criar componentes usando o catálogo e os modelos.
```
8. Atualize o DeploymentConfig *Web* para usar o Secret com as credenciais do MongoDB e valide o resultado:
```
$ oc set env --from=secret/mongodb dc/web
```
9. Demonstrar a capacidade de auto-recuperação (ReplicationController) e como os dados do contêiner DB não são afetados:
```
$ oc delete pod mongodb-(...)
$ oc delete pod web-(...)
```
10. Mostre que o OpenShift provisionou não apenas o contêiner MongoDB, mas também a persistência necessária automaticamente:
```
$ oc get pvc
```
11. Desativar 'deployment triggers' para novas tags ImageStream:
```
> Use a GUI para demonstrar como devemos modificar os parâmetros do DeploymentConfig.
```
12. Crie o CI/CD em contêiner com base em Jenkins e demonstre sua execução:
```
$ oc create -f extras/web-pipeline.yaml
$ oc start-build web-pipeline
```
13. Configure o Github Webhook no BuildConfig web-pipeline e demonstre novamente o fluxo de trabalho CI/CD:
```
> Use a GUI para configurar o webhook e criar um commit no repositório do GitHub.
```
14. Demonstrar as capacidades de Métricas e Logging já incluídas no OpenShift:
```
> Use a GUI para acessar as informações de Monitoramento, Logs e Métricas.
```
15. Limpe tudo:
```
$ oc delete project demo-openshift
```
