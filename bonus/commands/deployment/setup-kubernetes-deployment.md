---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [tipo-deployment] | --microservices | --monolith | --stateful | --full-stack | --production-ready
description: Configure implantação abrangente do Kubernetes com manifests, segurança, scaling e melhores práticas de produção
---

# Configuração de Deployment Kubernetes

Configure deployment Kubernetes: $ARGUMENTS

## Análise do Ambiente Atual

- Tipo de aplicação: @package.json ou @Dockerfile (detectar prontidão para containerização)
- Config K8s existente: !`find . -name "*.yaml" -o -name "*.yml" | grep -E "(k8s|kubernetes|deployment|service)" | head -3`
- Acesso ao cluster: !`kubectl cluster-info 2>/dev/null | head -2 || echo "Sem acesso ao cluster"`
- Container registry: @docker-compose.yml ou verificar configuração de registry
- Requisitos de recursos: Análise necessária baseada no tipo de aplicação

## Tarefa

Implementar deployment Kubernetes pronto para produção:

1. **Planejamento da Arquitetura Kubernetes**
   - Analisar arquitetura da aplicação e requisitos de deployment
   - Definir requisitos de recursos (CPU, memória, armazenamento, rede)
   - Planejar organização de namespaces e estratégia multi-tenant
   - Avaliar requisitos de alta disponibilidade e recuperação de desastres
   - Definir estratégias de scaling e requisitos de performance

2. **Configuração e Setup do Cluster**
   - Configurar cluster Kubernetes (gerenciado ou self-hosted)
   - Configurar networking do cluster e plugin CNI
   - Configurar storage classes e persistent volumes do cluster
   - Configurar políticas de segurança e RBAC do cluster
   - Configurar infraestrutura de monitoramento e logging do cluster

3. **Containerização da Aplicação**
   - Garantir que a aplicação está adequadamente containerizada
   - Otimizar imagens de container para deployment Kubernetes
   - Configurar builds multi-stage e security scanning
   - Configurar registry de containers e gerenciamento de imagens
   - Configurar políticas de pull de imagens e secrets

4. **Criação de Manifests Kubernetes**
   - Criar manifests Deployment com limites de recursos apropriados
   - Configurar manifests Service para comunicação interna e externa
   - Configurar ConfigMaps e Secrets para gerenciamento de configuração
   - Criar PersistentVolumeClaims para armazenamento de dados
   - Configurar NetworkPolicies para segurança e isolamento

5. **Load Balancing e Ingress**
   - Configurar controllers Ingress e regras de roteamento
   - Configurar terminação SSL/TLS e gerenciamento de certificados
   - Configurar estratégias de load balancing e session affinity
   - Configurar DNS externo e gerenciamento de domínios
   - Configurar gerenciamento de tráfego e deployments canary

6. **Configuração de Auto-scaling**
   - Configurar Horizontal Pod Autoscaler (HPA) baseado em métricas
   - Configurar Vertical Pod Autoscaler (VPA) para otimização de recursos
   - Configurar Cluster Autoscaler para scaling de nós
   - Configurar métricas customizadas e políticas de scaling
   - Configurar quotas de recursos e limites

7. **Health Checks e Monitoramento**
   - Configurar probes de liveness e readiness
   - Configurar startup probes para aplicações que iniciam lentamente
   - Configurar endpoints de health check e monitoramento
   - Configurar coleta de métricas da aplicação
   - Configurar alertas e sistemas de notificação

8. **Segurança e Conformidade**
   - Configurar Pod Security Standards e políticas
   - Configurar segmentação de rede e políticas de segurança
   - Configurar service accounts e permissões RBAC
   - Configurar gerenciamento de secrets e rotação
   - Configurar security scanning e monitoramento de conformidade

9. **Integração com CI/CD**
   - Configurar pipelines automatizados de deployment Kubernetes
   - Configurar workflows GitOps com ArgoCD ou Flux
   - Configurar testes automatizados em ambientes Kubernetes
   - Configurar estratégias de deployment blue-green e canary
   - Configurar procedimentos de rollback e recuperação de desastres

10. **Operações e Manutenção**
    - Configurar procedimentos de manutenção e atualização do cluster
    - Configurar estratégias de backup e recuperação de desastres
    - Configurar otimização de custos e gerenciamento de recursos
    - Criar runbooks operacionais e guias de troubleshooting
    - Treinar time em operações e melhores práticas Kubernetes
    - Configurar gerenciamento e governança do ciclo de vida do cluster