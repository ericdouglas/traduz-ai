# Construindo um Simples Armazenador de Chave-Valor em Elixir, usando Logs - Parte 1

-   **Artigo original**: [Build a Simple Persistent Key-Value Store in Elixir, using Logs – Part 1](https://www.poeticoding.com/build-a-simple-persistent-key-value-store-in-elixir-using-logs-part-1/)
-   **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

![Capa do artigo](https://i.imgur.com/fxj86xv.png)

- [Construindo um Simples Armazenador de Chave-Valor em Elixir, usando Logs - Parte 1](#construindo-um-simples-armazenador-de-chave-valor-em-elixir-usando-logs---parte-1)
	- [⚠ WIP ⚠](#%e2%9a%a0-wip-%e2%9a%a0)
	- [Introdução](#introdu%c3%a7%c3%a3o)
	- [What is a log?](#what-is-a-log)
	- [O que é um log?](#o-que-%c3%a9-um-log)
	- [Using a Log to implement a simple KV store](#using-a-log-to-implement-a-simple-kv-store)
	- [Usando um Log para implementar um simples armazenador CV (chave-valor)](#usando-um-log-para-implementar-um-simples-armazenador-cv-chave-valor)

## ⚠ WIP ⚠

The code in this article is heavily inspired from the concepts amazingly explained in the book Designing Data-Intensive Applications by Martin Kleppmann.
_O código neste artigo é fortemente inspirado em conceitos incrivelmente explicados no livro [Designing Data-Intensive Applications](https://dataintensive.net/) por Martin Kleppmann_

Disclaimer, all the code you find in this article, and on the github repo, is written for pure fun and meant just as an experiment.
Aviso, todo o código que você encontrar nesse artigo e no repositório do GitHub, foi escrito por pura diversão e criado como um experimento.

Published articles in the series:
Artigos publicados nessa série:

1. Parte 1 (esse artigo)
1. Parte 2

## Introdução

In the last year I’ve got interested in logs and how something so simple can be the solid foundation of databases like Riak, Cassandra and streaming systems like Kafka.
No último ano fiquei interessado em _logs_ (registros) e como algo tão simples pode ser a fundação sólida de banco de dados como Riak, Cassandra e sistemas de _streaming_ como Kafka.

In this series of articles we will see the different concepts behind a key-values store (Logs, Segments, Compaction, Memtable, SSTable) implementing a simple engine in Elixir, which is a great language to build highly-concurrent and fault-tolerant architectures.
Nesta série de artigos vamos ver os diferentes conceitos por trás de armazenadores chave-valor (Logs, Segmentos, Compactação, _Memtable_ e _SSTable_) implementando um motor simples em Elixir, a qual é uma grande linguagem para construir arquiteturas altamente concorrentes e tolerantes a falha.

In this first part we will see:
Nessa parte vamos ver:

-   What is a log?
-   O que é um log?
-   Making a KV persistent using a log and an index. Using an example we will use along the series, crypto market prices and trades, we are going to see how to store the values in a log using using an index.
-   Como fazer um KV (_key-value_ ou chave-valor) persistente usando log e um índice. Com um exemplo que vamos usar ao longo da série, preços e transações do mercado de criptomoedas, iremos ver como armazenar os valores em um log usando um índice.
-   LogKV in Elixir. A initial super simple implementation in elixir of a Writer, an Index and a Reader.
-   LogKV em Elixir. Uma implementação inicial super simples em Elixir de um Escritor, um Índice e um Leitor.

## What is a log?

## O que é um log?

Let’s think at the most common type of log file, the one we use everyday to debug events and error messages in our applications. This simple file enshrines an interesting property, it’s an append-only file. This means that only sequential writes are allowed and everything we write in the log is immutable.

Vamos pensar no tipo de arquivo de log mais comum, aquele que usamos todos os dias para debugar eventos e mensagens de erro em nossas aplicações. Este arquivo simples consagra uma propriedade interessante, ele é um arquivo _append-only_ (que aceita adições de conteúdo apenas no fim do arquivo). Isso significa que apenas escritas sequenciais são permitidas e tudo que escrevemos no log é imutável.

Why sequential writes could be important for us? Well… speed!

Por que escrita sequencial pode ser importante pra nós? Bom... velocidade!

![Random vs Sequential Access – The Pathologies of Big Data](https://i.imgur.com/qDcMjkU.png)

> Acesso Aleatório vs Sequencial - [As Patologias do Big Data](https://queue.acm.org/detail.cfm?id=1563874)

We see the huge difference between random and sequential access on both classic magnetic disks, SSD and even memory. So, the idea is to leverage the sequential access speed using an append-only file to save the data of our key-value store.

Pudemos ver a grande diferença entre acesso aleatório e sequencial em ambos discos magnéticos clássicos, SSD e até mesmo a memória. Então, a ideia é potencializar a velocidade do acesso sequencial usando um arquivo apenas de adição para salvar os dados do nosso armazenador chave-valor.

## Using a Log to implement a simple KV store

## Usando um Log para implementar um simples armazenador CV (chave-valor)
