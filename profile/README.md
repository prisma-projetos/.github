<div align="center">

# PRISMA

### O teor da cobertura da imprensa sobre partidos e candidatos, rastreável até a fonte

O PRISMA coleta notícias políticas, identifica os políticos e partidos citados, classifica o teor de cada trecho (**negativo · neutro · positivo**) e gera, com LLMs, panoramas mensais que o eleitor pode conferir matéria por matéria.

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Java](https://img.shields.io/badge/Java_25-ED8B00?logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?logo=springboot&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?logo=nextdotjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?logo=rabbitmq&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-EF7B4D?logo=argo&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-000000?logo=ollama&logoColor=white)

</div>

---

## Equipe

<table>
  <tr>
    <td align="center"><a href="https://github.com/and3510"><img src="https://github.com/and3510.png?size=100" width="80" alt="Anderson"/><br/><b>Anderson</b></a><br/><sub>@and3510</sub></td>
    <td align="center"><a href="https://github.com/whosbea"><img src="https://github.com/whosbea.png?size=100" width="80" alt="Beatriz"/><br/><b>Beatriz</b></a><br/><sub>@whosbea</sub></td>
    <td align="center"><a href="https://github.com/cristovam10000"><img src="https://github.com/cristovam10000.png?size=100" width="80" alt="Cristovam"/><br/><b>Cristovam</b></a><br/><sub>@cristovam10000</sub></td>
    <td align="center"><a href="https://github.com/gustavodovale"><img src="https://github.com/gustavodovale.png?size=100" width="80" alt="Gustavo"/><br/><b>Gustavo</b></a><br/><sub>@gustavodovale</sub></td>
    <td align="center"><a href="https://github.com/lcsp14"><img src="https://github.com/lcsp14.png?size=100" width="80" alt="Lucas"/><br/><b>Lucas</b></a><br/><sub>@lcsp14</sub></td>
    <td align="center"><a href="https://github.com/Furvos"><img src="https://github.com/Furvos.png?size=100" width="80" alt="Nicolas"/><br/><b>Nicolas</b></a><br/><sub>@Furvos</sub></td>
  </tr>
</table>

## Repositórios

| Repositório | Stack |
|---|---|
| **[scrapping-prisma](https://github.com/prisma-projetos/scrapping-prisma)** | Python · PostgreSQL · RabbitMQ |
| **[classifier-prisma](https://github.com/prisma-projetos/classifier-prisma)** | Python · PostgreSQL · RabbitMQ |
| **[analyser-prisma](https://github.com/prisma-projetos/analyser-prisma)** | Python · Ollama/LLM · PostgreSQL |
| **[api-prisma](https://github.com/prisma-projetos/api-prisma)** | Java 25 · Spring Boot 4 |
| **[app-prisma](https://github.com/prisma-projetos/app-prisma)** | Next.js 16 · React 19 · Tailwind 4 |
| **[experiments-prisma](https://github.com/prisma-projetos/experiments-prisma)** | Python · scikit-learn · LaTeX |

## Princípios

| | |
|---|---|
| **Rastreabilidade** | Todo número e todo panorama apontam para as matérias que os sustentam |
| **Teor, não viés** | Medimos o que foi publicado sobre cada entidade. Não classificamos veículos nem pessoas no espectro ideológico |
| **IA identificada** | Todo texto gerado por modelo traz o rótulo e o nome do modelo, e cada agregado traz o método e a versão do pipeline |
| **Fronteiras claras** | Cada serviço tem uma responsabilidade, e os serviços se comunicam por mensageria com *outbox* transacional |

## Arquitetura

```mermaid
flowchart LR
    GN[("Google News<br/>RSS")] --> SCR

    subgraph coleta["Coleta"]
        SCR["scrapping-prisma"]
        SDB[("Scraper DB<br/>+ outbox")]
        SCR <--> SDB
    end

    SCR -->|"RabbitMQ"| CLS

    subgraph classificacao["Classificação"]
        CLS["classifier-prisma"]
    end

    CLS -->|"outbox → RabbitMQ"| ANA

    subgraph analise["Análise"]
        ANA["analyser-prisma"]
        LLM(["LLM<br/>OpenAI-compatible"])
        ANA <--> LLM
    end

    SYS[("System DB<br/>PostgreSQL")]
    CLS --> SYS
    ANA --> SYS

    subgraph entrega["Entrega"]
        API["api-prisma"]
        APP["app-prisma"]
    end

    SYS --> API
    API -->|"X-API-KEY via BFF"| APP
    APP --> USR(["Eleitor"])
```

| # | Etapa | Módulo |
|:-:|---|---|
| 1 | **Coleta** | [`scrapping-prisma`](https://github.com/prisma-projetos/scrapping-prisma) |
| 2 | **Classificação** | [`classifier-prisma`](https://github.com/prisma-projetos/classifier-prisma) |
| 3 | **Análise** | [`analyser-prisma`](https://github.com/prisma-projetos/analyser-prisma) |
| 4 | **Consulta** | [`api-prisma`](https://github.com/prisma-projetos/api-prisma) |
| 5 | **Visualização** | [`app-prisma`](https://github.com/prisma-projetos/app-prisma) |

## Plataforma e DevOps

**Mensageria confiável** · **Idempotência** · **Containers** · **CI/CD + GitOps** · **Kubernetes** · **Segurança** · **Decisões documentadas**

## Pesquisa

Estudo exploratório, protocolo de anotação e decisões de método em **[experiments-prisma](https://github.com/prisma-projetos/experiments-prisma)**.

---

<div align="center">

<sub>As classificações de sentimento e os panoramas são gerados por modelos de IA e podem conter erros. Os panoramas resumem o que as fontes publicaram, não fatos verificados por esta plataforma.</sub>

</div>
