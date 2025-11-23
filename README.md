[README.md](https://github.com/user-attachments/files/23697396/README.md)
 TransFlow - Backend de Gestão de Corridas

Backend protótipo para gerenciamento de corridas urbanas utilizando arquitetura assíncrona e banco de dados não relacional.

🛠 Tecnologias

Python 3.10+

FastAPI (API Rest)

FastStream (Mensageria com RabbitMQ)

MongoDB (Persistência de Corridas)

Redis (Cache e Gestão de Saldos em Tempo Real)

Docker & Docker Compose

  Instalação e Execução

Pré-requisitos

Docker e Docker Compose instalados.

Passo a Passo

Clone o repositório e entre na pasta transflow.

Suba o ambiente completo:

docker-compose up --build


Aguarde os logs indicarem que o app, mongo, redis e rabbitmq estão rodando.

A API estará disponível em: http://localhost:8000

Documentação interativa (Swagger): http://localhost:8000/docs

RabbitMQ Management: http://localhost:15672 (guest/guest)

  Como Testar

1. Cadastrar Corrida (Gera Evento Assíncrono)

POST /corridas

Payload de exemplo:

{
  "id_corrida": "run_001",
  "passageiro": {
    "nome": "João",
    "telefone": "99999-1111"
  },
  "motorista": {
    "nome": "Carla",
    "nota": 4.8
  },
  "origem": "Centro",
  "destino": "Inoã",
  "valor_corrida": 50.00,
  "forma_pagamento": "Pix"
}


Comportamento: A API retorna HTTP 202 (Accepted). O Producer envia para o RabbitMQ. O Consumer lê, salva no Mongo e atualiza o saldo no Redis.

2. Consultar Saldo do Motorista

GET /saldo/Carla

Resposta esperada (se for a primeira corrida de 50.00):

{
  "motorista": "Carla",
  "saldo": 50.0
}


3. Consultar Corridas

GET /corridas ou /corridas/pagamento/Pix
Retorna os documentos persistidos no MongoDB.

⚙️ Variáveis de Ambiente

As configurações padrão estão no arquivo .env para execução local, mas são sobrescritas pelo docker-compose.yml para execução em contêineres.

Variável

Descrição

MONGO_URI

Connection string do MongoDB

REDIS_URL

URL de conexão do Redis

RABBITMQ_URL

URL de conexão do Broker AMQP

Desenvolvido para avaliação técnica TransFlow.
