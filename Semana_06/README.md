# 🖥️ Semana 6: Hardware e Infraestrutura Física para Big Data

## 📌 Objetivo da Aula

Compreender os componentes físicos de um computador e a importância da infraestrutura de rede para sistemas de Big Data. Esta semana conecta o conhecimento teórico com a prática de montagem e cabeamento de equipamentos.

---

## 🔗 Por que isso é importante para Big Data?

A infraestrutura de Big Data não é apenas software (Hadoop, Spark, etc.). É fundamental entender:

- **Hardware**: Processadores, memória e armazenamento definem a capacidade de processamento paralelo
- **Rede**: A comunicação entre nós é crítica - dados precisam ser transferidos rapidamente em clusters
- **Escalabilidade**: Ao replicar a arquitetura em múltiplas máquinas, você cria um ambiente distribuído

---

## 🖲️ Componentes de um Computador

Abaixo estão os principais componentes que permitem que um servidor funcione em um ambiente de Big Data:

![Peças de um computador](./imagens/Peças%20de%20um%20computador.jpg)

### Componentes-chave:

| Componente | Função | Relevância para Big Data |
|-----------|--------|------------------------|
| **Placa Mãe** | Conecta todos os componentes | Determina compatibilidade e velocidade |
| **Processador (CPU)** | Executa operações | Processamento paralelo de dados |
| **Memória RAM** | Armazenamento temporário | Velocidade de acesso aos dados em processamento |
| **HD (Disco Rígido)** | Armazenamento permanente | Armazenamento de dados no HDFS |
| **Fonte de Alimentação** | Fornece energia | Estabilidade do sistema |

---

## 🌐 Cabeamento de Rede (Cabo RJ45)

A comunicação entre nós de um cluster Big Data acontece através de cabos de rede. Entender a estrutura do cabo RJ45 é essencial para montar e diagnosticar problemas de conectividade.

![cabo de rede](./imagens/cabo%20de%20rede.jpeg)

### Estrutura do Cabo RJ45

O cabo de rede possui **8 fios** internos com cores específicas, seguindo um padrão internacional:

![Ordem dos fios](./imagens/Ordem%20dos%20fios.png)

### Sequência Padrão (EIA/TIA-568A):

1. **Branco-Verde** 🟢
2. **Verde** 🟢
3. **Branco-Laranja** 🟠
4. **Azul** 🔵
5. **Branco-Azul** 🔵
6. **Laranja** 🟠
7. **Branco-Marrom** 🟤
8. **Marrom** 🟤

> **Dica**: A sequência é importante porque determina quais fios transmitem e recebem dados. Dessa forma, o sinal viaja corretamente entre os computadores.

> **Padrão utilizado**: A sequência acima segue o padrão **EIA/TIA-568A**, uma das duas normas internacionais para crimpagem de cabos RJ45. Ela difere do padrão 568B apenas na posição dos pares laranja e verde (pinos 1, 2, 3 e 6). Ambos os padrões funcionam corretamente, desde que as duas pontas do cabo usem a **mesma norma** (cabo direto) — ou normas opostas intencionalmente (cabo crossover).

---

## 🏗️ Aplicação em Clusters de Big Data

### Por que precisamos disso?

```
Máquina 1 (Node Master)     ← Cabo de Rede →     Máquina 2 (DataNode)
        ↓                                                  ↓
   Processador                                      Processador
   CPU Cores: 8                                     CPU Cores: 8
   RAM: 32GB                                        RAM: 32GB
        ↓                                                  ↓
   HDFS (Armazenamento)    ← Comunicação rápida →  HDFS (Dados)
   MapReduce (Proc.)                                 Dados replicados
```

- **Múltiplas máquinas** conectadas com cabos de rede adequados
- **Processadores rápidos** para executar Map e Reduce em paralelo
- **Memória suficiente** para carregar dados em processamento
- **Cabos bem montados** garantem latência baixa e bandwidth alto

---

## 📊 Impacto na Performance

| Aspecto | Impacto |
|--------|--------|
| CPU fraca | Processamento lento, gargalo no cálculo |
| Pouca RAM | Spill to disk, reduz velocidade drasticamente |
| Rede lenta | Comunicação entre nós é mais lenta que processamento |
| Cabeamento mal feito | Perda de pacotes, timeouts, instabilidade |

> **Importante**: A rede é frequentemente o **gargalo** em clusters de Big Data. Um cabo mal montado pode corromper pacotes.

---

## ✅ Resumo

Esta semana você aprendeu:
- ✅ Componentes de um servidor e sua função
- ✅ Como montar uma CPU em uma placa mãe
- ✅ Estrutura e montagem de cabos RJ45
- ✅ A importância da infraestrutura física para Big Data

**Próximo passo**: Usar esse conhecimento para diagnosticar problemas de rede e otimizar a comunicação em clusters!

---

**Data da aula**: 28/04/2026  
**Documentado por**: Thais 👩‍💻
