# Descomplicando o Kubernetes - Expert Mode

## DAY-1

### Início da aula do Day-1

#### Qual distro GNU/Linux devo usar?

Devido ao fato de algumas ferramentas importantes, como o `systemd` e `journald`, terem se tornado padrão na maioria das principais distribuições disponíveis hoje, você não deve encontrar problemas para seguir o treinamento, caso você opte por uma delas, como Ubuntu, Debian, CentOS e afins.

---

### Alguns sites que devemos visitar

**Sites oficiais do projeto Kubernetes:**

* [https://kubernetes.io](https://kubernetes.io)
* [https://github.com/kubernetes/kubernetes/](https://github.com/kubernetes/kubernetes/)
* [https://github.com/kubernetes/kubernetes/issues](https://github.com/kubernetes/kubernetes/issues)

**Certificações Kubernetes (CKA, CKAD e CKS):**

* [https://www.cncf.io/certification/cka/](https://www.cncf.io/certification/cka/)
* [https://www.cncf.io/certification/ckad/](https://www.cncf.io/certification/ckad/)
* [https://www.cncf.io/certification/cks/](https://www.cncf.io/certification/cks/)

---

### O Container Engine

O Container Engine é responsável por gerenciar as imagens e volumes, garantindo o isolamento de recursos utilizados pelos containers, como vida útil, armazenamento, rede, etc.

Antigamente o Docker era a única opção, mas hoje temos:

* **Docker** (utiliza `containerd` como runtime)
* **CRI-O**
* **Podman**

---

### O Container Runtime

Responsável por executar containers nos nós. É utilizado pelo Container Engine.

**Tipos de Container Runtime:**

* **Low-level:** executado diretamente pelo kernel (ex: `runc`, `crun`, `runsc`)
* **High-level:** executado pelo Container Engine (ex: `containerd`, `CRI-O`, `Podman`)
* **Sandbox:** executa containers de forma segura com proxies ou unikernels (ex: `gVisor`)
* **Virtualized:** executa containers em VMs com maior segurança (ex: `Kata Containers`)

---

### O que é o Kubernetes?

O Kubernetes é um orquestrador de contêineres criado pela Google em 2014, com base no projeto Borg. É um projeto open source conhecido como **k8s**, em referência ao termo "Kubernetes" (k seguido de 8 letras e um s).

#### Arquitetura do Kubernetes

Modelo `control plane/workers` com, no mínimo, 3 nós:

* **Control plane:** gerencia o cluster
* **Workers:** executam os pods

**Ferramentas para criar clusters locais:**

* `kind`
* `minikube`
* `MicroK8s`
* `k3s`
* `k0s`

#### Componentes do cluster:

* **API Server:** interface REST via JSON para interação com o cluster
* **etcd:** datastore chave-valor
* **Scheduler:** decide em qual nó executar os pods
* **Controller Manager:** garante que o estado atual seja igual ao definido
* **Kubelet:** agente que gerencia pods em nós workers
* **Kube-proxy:** roteia requisições e faz balanceamento de carga

#### Portas importantes:

**CONTROL PLANE**

| Protocol | Direction | Port Range | Purpose                 | Used By              |
| -------- | --------- | ---------- | ----------------------- | -------------------- |
| TCP      | Inbound   | 6443\*     | Kubernetes API server   | All                  |
| TCP      | Inbound   | 2379-2380  | etcd server client API  | kube-apiserver, etcd |
| TCP      | Inbound   | 10250      | Kubelet API             | Self, Control plane  |
| TCP      | Inbound   | 10251      | kube-scheduler          | Self                 |
| TCP      | Inbound   | 10252      | kube-controller-manager | Self                 |

**WORKERS**

| Protocol | Direction | Port Range  | Purpose     | Used By             |
| -------- | --------- | ----------- | ----------- | ------------------- |
| TCP      | Inbound   | 10250       | Kubelet API | Self, Control plane |
| TCP      | Inbound   | 30000-32767 | NodePort    | Services All        |

---

### Conceitos-chave do Kubernetes

* **Pod:** menor unidade do k8s; agrupa contêineres que compartilham recursos
* **Deployment:** gerencia ciclo de vida dos pods, usa `ReplicaSet`
* **ReplicaSet:** garante o número desejado de pods
* **Service:** expõe pods para acesso via `ClusterIP`, `NodePort` ou `LoadBalancer`

---

## Instalando e customizando o kubectl

### GNU/Linux:

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```

### macOS (Homebrew):

```bash
sudo brew install kubectl
kubectl version --client
# ou
sudo brew install kubectl-cli
kubectl version --client
```

### macOS (Tradicional):

```bash
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```

### Customizando o kubectl

**Auto-complete (Bash):**

```bash
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

**Alias:**

```bash
alias k=kubectl
complete -F __start_kubectl k
```

**Verificando os nós do cluster:**

```bash
kubectl get nodes
```

---

## Primeiros passos no Kubernetes

### Verificando namespaces e pods:

```bash
kubectl get namespaces
kubectl get pod -n kube-system
kubectl get pods -A
kubectl get pods -A -o wide
```

### Executando um pod:

```bash
kubectl run nginx --image nginx
kubectl get pods
kubectl delete pod nginx
```

### Criando manifesto com `--dry-run`:

```bash
kubectl run meu-nginx --image nginx --dry-run=client -o yaml > pod-template.yaml
kubectl apply -f pod-template.yaml
```

### Expondo o pod como Service:

```bash
kubectl expose pod meu-nginx --port=80
kubectl get services
```

### Limpando os recursos:

```bash
kubectl get all
kubectl get pod,service
kubectl get pod,svc
kubectl delete -f pod-template.yaml
kubectl delete service nginx
```

---

Fim do Day-1.
