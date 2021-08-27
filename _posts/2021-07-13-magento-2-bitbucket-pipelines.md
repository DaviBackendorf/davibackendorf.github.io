---
layout: post
title:  "Magento 2 + Bitbucket Pipelines (Básico)"
description: "Simples deploy automatizado Magento 2 + Bitbucket Pipelines"
author: davi
categories: [ magento 2, DevOps ]
image: assets/images/bitbucket-pipelines/capa.png
featured: true
hidden: true
comments: false
share: false
---

Configure o deploy automatizado da sua loja usando **_Bitbucket Pipelines_** e tenha entregas mais rápidas, com
qualidade, e sem trabalho manual.

#### Habilitar a funcionalidade

Após fazer _login_ na sua conta do Bitbucket, habilite a funcionalidade de _Pipelines_. No seu repositório, vá
  em _**Repository settings->Pipelines->Settings**_ e habilite a opção **_Enable Pipelines_**.

![walking]({{ site.baseurl }}/assets/images/bitbucket-pipelines/enable-pipelines.png)

- Clique em "Configure bitbucket-pipelines.yml"
  ![walking]({{ site.baseurl }}/assets/images/bitbucket-pipelines/config-file.png)

#### Pertmitir que o Bitbucket se conecte ao seu servidor via SSH

- Vá em  _**Repository settings->SSH Keys**_ e clique em **_Generate keys_**
  ![walking]({{ site.baseurl }}/assets/images/bitbucket-pipelines/generate-keys.png)

#### Adicionar hosts conhecidos
Informe o **_IP:PORTA_** do servidor SSH e clique em "**_Fetch_**", depois em "**_Add_**"
![walking]({{ site.baseurl }}/assets/images/bitbucket-pipelines/know-hosts.png)

Acesse o seu servidor via SSH e edite/crie o arquivo `~/.ssh/authorized_keys` com o seu editor favorito. Copie a chave
publica gerada anteriormente no Bitbucket e cole no arquivo, salve e feche.

**Pronto, neste ponto o Bitbucket já deve ter acesso ao seu servidor!**

### Preparando o ambiente no servidor
#### Clonando repositório via SSH
Para que funcione sem complicação, clone o repositório no servidor de _deploy_ via _SSH_. Se necessário gere a chave com
o comando abaixo e siga os passos que aparecerão no terminal.

```shell
 ssh-keygen
 ```

Depois exiba a chave gerada usando:

```shell
 cat ~/.ssh/id_rsa.pub
 ```

Adicione essa chave no Bitbucket, em **_Repository Settings->Access keys_**, clique em **"Add
key"**, coloque um apelido para a chave, cole-a no local correspondente e finalize com **"Add SSH Key"**.

#### Personalizando o bitbucket-pipelines.yml

Este arquivo é quem contem as instruções para _Bitbucket Pipelines_, nele definimos os ambientes que serão usados e os 
_scripts_ a serem executados, entre outros.

Este arquivo pode diferir para cada branch, no nosso caso, somente mudará o parâmetro **_deployment_**. Coloque o
seguinte conteúdo:

```yml
image: atlassian/default-image:2

pipelines:
  default:
    - step:
        name: 'Iniciando deploy'
        deployment: staging
        script:
          - ssh -Tp $SSH_PORT $SSH_USER@$SSH_HOST "cd $ROOT_DIRECTORY; chmod +x deploy.sh; ./deploy.sh $BRANCH_NAME";
```

Repare que temos algumas variáveis no _script_, estas são configuradas pelo painel do Bitbucket, e possibilitam fazer as
coisas de uma fora mais dinâmica entre os ambientes.

Vá em **_Repository Settings->Pipelines->Deployments_**, repare que há alguns ambientes como padrão, e que as variáveis
podem ser definidas de acordo com cada ambiente.
Configure todas as variáveis do _script_ para cada ambiente que for usar.  

Quem define qual ambiente é usado na execução da _pipeline_ é o parâmetro **_deployment_** do
_**bitbucket-pipelines.yml**_.


Considerando que temos branches diferentes para cada ambiente, mudamos apenas este parâmetro
para indicar qual configuração será usado para execução da nossa _pipeline_.

#### Adicionando script de deploy

Adicione o arquivo `deploy.sh` no diretório inicial da sua loja com o seguinte conteúdo:

```shell
git reset --hard;
git clean -df;
git checkout "$1";
git pull;

composer install;
php bin/magento maintenance:e;
rm -rf var/view_preprocessed/*;
rm -rf var/cache/*;
rm -rf pub/static/adminhtml/;
rm -rf pub/static/frontend/;
rm -rf generated/*;
php bin/magento cache:flush;
php bin/magento cache:clean;
php bin/magento setup:upgrade;
php bin/magento deploy:mode:set production;
php bin/magento maintenance:d;
php bin/magento c:f;
```

Faça commit das alterações e verifique o resultado da _pipeline_ via painel do Bitbucket. Se tudo ocorrer bem, a sua 
_pipeline_
será executada e o _deploy_ será feito.