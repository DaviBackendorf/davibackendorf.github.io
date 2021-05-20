---
layout: post
title:  "Módulo de Parcelamento para Magento 2"
description: "Módulo de preço parcelado para magento 2"
author: davi
categories: [ magento 2, modulo ]
image: assets/images/parcelamento/capa.jpg
featured: true
hidden: true
---

Por padrão o Magento 2 não atende a necessidade dos lojistas em exibir as opções de parcelamento no catálogo de produtos. Então desenvolvi um módulo que implementa esta funcionalidade...

O módulo foi desenvolvido pensando na praticidade e garantindo o seu funcionamento da maneira mais simples possível, sem esquecer dos padrões e melhores práticas de desenvolvimento na plataforma Magento 2.

## Funcionalidades do módulo:

### Parcelamento
- Quantidade máxima de parcelas;
- Valor mínimo da parcela;
- Mostrar todas as opções de parcelamento na página de produto;
- Tipo de cálculo de juros (simples ou composto);
- Percentual de juros sobre cada parcela (até a 12º);
- Mostrar sempre a 1º parcela;
- Mostrar a parcela mais baixa no carrinho;
- Notificar o desenvolvedor sobre erros;

### Descontos
- Nome e percentual do desconto;
- Descontos múltiplos;

### Layout de exibição
*"O layout dos preços é facilmente editado via painel."*
- Na página de categoria;
- Nos resultados de busca;
- Na página do produto;
- Template de desconto;
- No carrinho;
- Template para todas as opções de parcelamento;
- Texto com/sem juros;

#### Como é a configuração

{% include image-gallery.html folder="/assets/images/parcelamento/galeria/config" %}

#### Como fica na loja

{% include image-gallery.html folder="/assets/images/parcelamento/galeria/frontend" %}

Disponível no [Git Hub](https://github.com/DaviBackendorf/magento2-installment)
