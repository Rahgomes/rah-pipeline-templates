# rah-pipeline-templates

Templates YAML reutilizáveis para Azure DevOps CI/CD.

## Templates disponíveis

### Steps (blocos individuais)
| Template | Descrição |
|----------|-----------|
| `templates/notify-start.yml` | Notificação Telegram de início de deploy |
| `templates/build-docker.yml` | Build de imagem Docker com tags |
| `templates/deploy-container.yml` | Deploy container com Traefik + Coolify labels |
| `templates/notify-result.yml` | Notificação Telegram de resultado (sucesso/falha) |

### Stages (fluxos completos)
| Template | Descrição |
|----------|-----------|
| `templates/stage-build.yml` | Stage completo: checkout + notify + build |
| `templates/stage-deploy.yml` | Stage completo: deploy + notify result |

## Como usar

### 1. Registrar como resource no pipeline

```yaml
resources:
  repositories:
    - repository: templates
      type: github
      name: Rahgomes/rah-pipeline-templates
      ref: main
      endpoint: Rahgomes
```

### 2. Referenciar templates

```yaml
stages:
  - template: templates/stage-build.yml@templates
    parameters:
      appName: MeuApp
      imageName: meuapp

  - template: templates/stage-deploy.yml@templates
    parameters:
      appName: MeuApp
      containerName: meuapp
      imageName: meuapp
      domain: meuapp.exemplo.com.br
      port: "3000"
      coolifyAppId: "1"
      coolifyUuid: "abc123"
      coolifyResourceName: MeuApp
      coolifyProjectName: meu-app
      envFlags: '-e "VAR1=$(VAR1)" -e "VAR2=$(VAR2)"'
```
