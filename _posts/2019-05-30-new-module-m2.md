---
layout: post
title:  "Como criar um módulo no Magento 2"
author: davi
categories: [ dicas, magento 2, tutorial ]
image: assets/images/new-module/new-module.jpeg
comments: false
share: false
---
A dica de hoje é pra quem esta começando com desenvolvimento no magento... 
## 1º Criar arquivo “module.xml”
A partir da pasta raiz de sua instalação Magento 2, entre em "app/code", e em seguida crie duas subpastas da seguinte forma: <Vendor>/<Modulo, onde “Vendor” é a empresa/desenvolvedor e “Modulo” o nome do módulo em si.

Agora na pasta raiz do módulo, crie a pasta “etc” com o arquivo “module.xml” com o seguinte conteúdo:


```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="../../../../../lib/internal/Magento/Framework/Module/etc/module.xsd">
    <module name="Vendor_Module" setup_version="1.0.0">
        <sequence>
            <module name="Magento_Sales"/>
            <module name="Magento_Payment"/>
            <module name="Magento_Checkout"/>
        </sequence>
    </module>
</config>
```

Repare que a tag “module” possuí dois atributos: “name” que é o nome do módulo, e “setup_version” que é a versão.

Na tag “sequence” informamos todos o módulos no qual nosso módulo depende, sendo que estes serão carregados por primeiro.

## 2º Criar arquivo “registration.php”

Agora na pasta root do nosso módulo criamos o arquivo “registration.php” com o seguinte conteúdo:

```php
<?php
\Magento\Framework\Component\ComponentRegistrar::register(
\Magento\Framework\Component\ComponentRegistrar::MODULE,
‘Vendor_Module’,
__DIR__
);
```

Novamente troque “Vendor_Module” pela nomenclatura do seu módulo.

Pimba! Nossa base do módulo esta pronta, e sua estrutura de diretórios deve se parecer com esta:

![walking]({{ site.baseurl }}/assets/images/new-module/structure.png)

## 3º Instalando e habilitando o módulo

Através do terminal ou cmd, navegue até a pasta raiz do Magento 2 e execute o comando de instalação:

```shell
bin/magento setup:upgrade
```

Caso tenha problemas nesta hora, certifique-se de não conter erros de estrutura de pastas ou sintaxe.
Se tudo for bem, haverá o registo do nosso módulo na tabela “setup_module” do banco de dados.

### Habilitando o módulo:
```shell
bin/magento module:enable Vendor_Module
```

### Verificando o status:
O comando abaixo lista os módulos agrupados por status, verifique se o seu módulo aparece no grupo “Enabled modules”.
```shell
bin/magento module:status
```

E por fim recompile a loja:
```shell
bin/magento setup:di:compile
```
É isso, espero ter ajudado.