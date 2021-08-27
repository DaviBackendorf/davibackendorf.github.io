---
layout: post
title:  "Criar novo usuário no Magento 2 (Pelo terminal)"
author: davi
categories: [ dicas, magento 2, tutorial ]
image: assets/images/new-admin-user/capa.png
comments: false
share: false
---
Navegue através do terminal até a pasta principal da sua loja e cole o seguinte conteúdo substituindo os valores:

```shell
bin/magento admin:user:create --admin-user="admin" --admin-password="admin123" --admin-email="admin@gmail.com" --admin-firstname="Nome" --admin-lastname="Sobrenome"
```

Tecle enter, aguarde a mensagem de confirmação da operação e pronto!