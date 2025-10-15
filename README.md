# GitOps Monorepo - Estrutura de Manifests Kubernetes

Este repositório segue uma **estrutura padrão de GitOps** para gerenciar a infraestrutura de aplicações em Kubernetes, garantindo organização, modularidade e facilidade de manutenção.

---

## 1. Estrutura de Diretórios

O repositório é organizado por **ambiente** e **serviço**:

gitops/
├── dev/
│ ├── backend-app/
│ │ ├── namespace.yaml
│ │ ├── deployment.yaml
│ │ ├── service.yaml
│ │ └── hpa.yaml
│ └── frontend-app/
│ ├── deployment.yaml
│ └── service.yaml
├── staging/
│ ├── backend-app/
│ └── frontend-app/
└── prod/
├── backend-app/
└── frontend-app/


- **Ambientes**: `dev`, `staging`, `prod`  
- **Serviços**: cada aplicação tem seu próprio diretório (`backend-app`, `frontend-app`)  
- **Arquivos padrão por serviço**:
  - `namespace.yaml` → define o namespace do ambiente
  - `deployment.yaml` → Deployment Kubernetes da aplicação
  - `service.yaml` → Service Kubernetes (ClusterIP, NodePort ou LoadBalancer)
  - `hpa.yaml` → HorizontalPodAutoscaler (opcional)

---

## 2. Regras de versionamento e deploy

1. **Código da aplicação** fica em repositórios separados (ex.: `backend-java`, `frontend-react`)  
2. **GitOps repo** contém apenas manifests Kubernetes  
3. Cada **pipeline CI/CD** do projeto builda a imagem e **atualiza o GitOps repo** com a nova tag da imagem  
4. **Secrets** devem ser mantidos criptografados (SealedSecrets) ou criados diretamente no cluster  
5. Argo CD aplica as alterações do GitOps repo automaticamente no cluster

---

## 3. Naming Conventions

- Diretórios: `<ambiente>/<servico>/`  
- Deployment: `<servico>`  
- Service: `<servico>`  
- HPA: `<servico>-hpa`  
- Namespace: nome do ambiente (`dev`, `staging`, `prod`)  

---

## 4. Variáveis Sensíveis

- Não colocar senhas ou tokens em texto puro no GitOps repo  
- Usar **SealedSecrets** ou **External Secrets**  
- Pipeline CI/CD pode gerar SealedSecrets usando chave segura armazenada em GitHub Secrets  

---

## 5. Exemplo de Deploy Backend Dev

gitops/dev/backend-app/
├── namespace.yaml
├── deployment.yaml
├── service.yaml
└── hpa.yaml


- Namespace: `dev`  
- Deployment: `backend-app`  
- Service: `backend-app`  
- HPA: `backend-app-hpa`  
- Secrets: referenciados via `env.valueFrom.secretKeyRef`

---

> Seguindo esta estrutura, conseguimos manter **padronização, modularidade e segurança**, facilitando a manutenção e expansão do GitOps repo à medida que novos serviços e ambientes são adicionados.
