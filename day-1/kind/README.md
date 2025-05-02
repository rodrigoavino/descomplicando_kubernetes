# 🚀 Kind: Simulação de Cluster Kubernetes

O **Kind** é uma ferramenta para execução de contêineres Docker que simulam o funcionamento de um cluster Kubernetes.  
É utilizado para **fins didáticos, de desenvolvimento e testes**.  
⚠️ **O Kind não deve ser utilizado para produção.**

## 📌 Instalação do Kind

Execute os seguintes comandos para instalar o **Kind** no Linux:

```sh
# Baixar o binário do Kind
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.14.0/kind-linux-amd64

# Dar permissão de execução
chmod +x ./kind

# Mover o binário para um diretório acessível globalmente
sudo mv ./kind /usr/local/bin/kind
