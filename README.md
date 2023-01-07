# argoCD

[ArgoCD](https://argo-cd.readthedocs.io/en/stable/getting_started/) é uma ferramenta que lê a configuração do seu ambiente (escrita como um chart helm, arquivos kustomize, 
jsonnet ou arquivos yaml simples) do seu repositório git e a aplica aos namespaces do Kubernetes.

## Alguns dos recursos do ArgoCD são:

    * automação e rastreabilidade via fluxo de trabalho GitOps
    * suporte para declarações de deployments helm, kustomize e jsonnet
    * IU da web para visualizar recursos do Kubernetes
    * argo cli
    * painel de métricas Grafana
    * trilhas de auditoria para eventos de aplicativos e chamadas de API

## Começando ...

clone este repositório

```
git clone https://github.com/danielbatubenga/argocd-deploy-kubernetes.git
cd argcd-deploy-kubernetes
```

crie os namespace kubernetes
```
kubectl create namespace argocd
kubectl create namespace nginx
```
aplicando os manifestos kubernetes

```
kubectl apply -f argoCD_conf/install..yaml -n argocd
```

Terminando a instalação faça o port-forwarding para acesso externo.

```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

No seu navegador use: localhost:8080 se tudo certo aparecerá a tela do kubernetes para login e agora
será necessario uma senha, o argocd gera as senhas randomicas para cada instalação e o usue é admin.

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

Copie a senha, exibida na saida do comando acima, e passe no navegador.

Neste ponto precisas subir apenas o manifesto app.yal que será responsavel pela verificação e atualização
dos pods.

execute 
```
kubectl apply -f argoCD_conf/app.yaml
```

Então agora sempre que mudar alguma alguma coisa no seu repo, o argoCD atualizará os pods.


[DOCS](https://argo-cd.readthedocs.io/en/stable/getting_started/)


