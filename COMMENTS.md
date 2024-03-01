# Resolução

## Automatizar a infraestrutura e o provisionamento dos hosts (IaaS)
Para automatizar a infraestrutura e o provisionamento dos hosts (IaaS) foi utilizado o terraform devido à sua capacidade de declarar a infraestrutura como código (IaC), o que permite definir e gerenciar a infraestrutura de forma programática e repetível.
Sua abordagem baseada em configuração permite uma automação eficiente do provisionamento de recursos de infraestrutura, o que é essencial para garantir uma implantação consistente e escalável da aplicação.

Acesse ```main.tf```


## Automatizar o setup e configuração dos hosts (IaC)
Para automatizar o setup e configuração dos hosts (IaC) resolvi usar o Ansible devido à sua simplicidade, eficiência e escalabilidade. Ele permite definir as configurações dos hosts em arquivos de playbook YAML, tornando fácil entender e manter a automação da infraestrutura. Além disso, o Ansible possui uma vasta comunidade e uma extensa biblioteca de módulos, o que facilita a integração com uma variedade de tecnologias e ambientes.

Acesse ```setup.yml```

## Pipeline de deploy automatizado

Para este pipeline, utilizamos o GitLab CI/CD, que é uma ferramenta integrada ao GitLab para automação de integração contínua (CI) e entrega contínua (CD).

### Etapas do Pipeline

#### Build

* Nesta etapa, a aplicação é construída e as dependências são instaladas.

* O ambiente é preparado para garantir que todas as dependências necessárias estejam presentes e que a aplicação esteja pronta para ser testada e implantada.

```
build:
  stage: build
  script:
    - echo "Instalando dependências..."
    - cd app
    - pip install -r requirements.txt

 ```


 #### Deploy

* Na etapa de deploy, a aplicação é implantada nos hosts de produção.

* Um playbook Ansible é executado para configurar os hosts e iniciar a aplicação.

```deploy:
  stage: deploy
  script:
    - echo "Implantando deploy..."
    - ansible-playbook setup.yml
  only:
    - master
 ```

 #### Deploy monitoring 

* Nesta etapa após deploy, a aplicação é monitorada utilizando Prometheus e o Grafana

```deploy_monitoring:
  stage: deploy_monitoring 
  script:
    - ansible-playbook deploy_monitoring.yml  
  only:
    - master
 ```


Acesse ```.gitlab-ci.yml```

### Considerações finais:

Uma etapa de execução de testes automatizados, como testes unitários, de integração ou de aceitação também poderiam ser incluida no antes do deploy para garantir que a aplicação está funcionando conforme esperado.


## Monitoramento dos serviços e métricas da aplicação

Para garantir a disponibilidade contínua das métricas da aplicação e a visualização dos dados através de dashboards, integramos o Prometheus e o Grafana em nosso pipeline de deploy.

O Ansible foi utilizado para automação de tarefas de configuração e atualização do Prometheus e Grafana nos hosts de destino.

### Execução do Pipeline
Após enviar o código para o repositório GitLab, o pipeline de deploy é acionado automaticamente.

A etapa deploy_monitoring é executada após a implantação da aplicação, garantindo que o Prometheus e o Grafana sejam configurados corretamente.

Após a conclusão do pipeline, as métricas da aplicação são continuamente coletadas pelo Prometheus e disponibilizadas para visualização no Grafana.

Os dashboards do Grafana são atualizados automaticamente, fornecendo insights valiosos sobre o desempenho e a saúde da aplicação.