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

Let’s start with a simple example. Let’s consider a realtime application where we have to store the last price in dollars of the Bitcoin (BTC), Ethereum (ETH) and Litecoin (LTC).

| Key | Value   |
| --- | ------- |
| BTC | 4478.12 |
| ETH | 133.62  |
| LTC | 33.19   |

If we just need to keep this snapshot in memory, in Elixir we can use a Map. But persistence is another story. There are many different ways and technologies we could use to store this map and make it persistent.

If this snapshot would be updated just few times in a day, with just few currencies, then serialising the map into a file would be fine and easy to do, but this is obviously not our case! Our imaginary crypto application needs to keep track of any market’s movement with hundreds of updates per second for hundreds of currencies.

But how can we use an append-only file, where data written is immutable by nature, to store the mutable data of a key-value store, to leverage sequential access and to keep our map persistent?

The idea is pretty simple:

- append to our log each single price update (value) for any currency (key)
- use our Map as an index, keeping track of the position and size of the values within our logfile.

![Concept of key-value persistence using a log](https://i.imgur.com/06A0ViN.png)

> Concept of key-value persistence using a log

1. 17:14:59 – LTC trades at 32.85$. We append the string "32.85" to the log and we update the "LTC" key of our index (implemented with a Map) with value’s offset (0 since it’s the first value in the file) and it’s size (5 bytes, since it’s a string).
2. 17:15:00 – ETH trades at 130.98$. We append the string "130.98" to the log and we update the "ETH" key of our index with offset 5 and size 6 bytes.
3. 17:15:01 – BTC trades at 4411.99$. We append the string "4411.99" to the log and we update the "BTC" key of our index with offset 11 and size 7 bytes.

What happens if we receive a new price for ETH? How can we overwrite the value in the log since the value we wrote is immutable and we can just append?

4. 17:15:09 – ETH trades at 131.00$.

![Transação ETH](https://i.imgur.com/DiWczpP.png)

Since to leverage sequential writes we can just append, we then just append the new value updating the index with the new offset and size.

The read is efficient too. To retrieve the values from the log, we just use offset and size in the index and with need one seek of the disk to load our value into memory.