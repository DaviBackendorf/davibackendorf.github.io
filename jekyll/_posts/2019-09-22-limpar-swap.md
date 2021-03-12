---
layout: post
title:  "Como limpar a memória SWAP no Ubuntu"
author: davi
categories: [ dicas, linux ]
image: assets/images/swap/swap.png
---

Dica simples para limpar a memória SWAP alocada:

```c
sudo swapoff -a && sudo swapon -a
```

Basicamente este comando desabilita e reabilita a memória SWAP.