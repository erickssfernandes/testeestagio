# 🧠 Teste de Estágio 

## Instruções
- Realize o git clone deste repositório
- Responda as perguntas no próprio Readme.md
- Suba o código no repositório
- Insira a mensagem retornada do servidor
- Nos envie o link do seu repositório

### Parte 1 – Conceito

Pesquise e explique com suas palavras:

- O que são WebSockets?
  
  R: WebSockets são uma tecnologia usada para aplicações em tempo real, bidirecional entre cliente e servidor que utiliza conexão TCP. São muito utilizadas para aplicações que precisam de um tempo menor de resposta entre cliente e servidor, além de facilitar o proccesso de transferência de dados com um desempenho melhor.

- Como funcionam?
  
  R: WebSockets funcionam estabelecendo uma conexão do cliente e servidor através de um soquete TCP, depois de conectados podem enviar e receber dados em tempo real. Para seu funcionamento existem duas etapas:  1° O handshake HTTP que seria a conexão WebSocket, criando uma solicitação HTTP ao servidor especificando o protocolo WebSocket no cabeçalho Upgrade e o servidro responde com uma resposta HTTP que incluiu um cabeçalho Upgrade, que indica sua migração para o protocolo WebSocket.
2° O protocolo WebSocket começa após terminado o handshake HTTP permitindo a comunicação em tempo real do cliente e servidor. O protocolo WebSocket é simples, baseado em mensagens, que permitem a comunicação bidirecional. As mensagens são enviadas em frames, que consistem em um cabeçalho (possui informações do tipo de mensagem, comprimento e se é o ultimo frame da mensagem) e um playload (possui os dados reais da mensagem).

- Quando é melhor usar WebSockets em vez de uma API REST tradicional?
  
 R: É melhor utilizar WebSockets para aplicativos que exigem comunicação em tempo real, bidirecional e de baixa latência. Exemplos de sua utilização mais comuns são: Chats online, jogos, ferramentas colaborativas em tempo real e atualização em tempo real de placares.

### Parte 2 – Prática

Você deve criar um pequeno script que se conecta ao **servidor WebSocket** que criamos e descobrir **qual mensagem ele está enviando**.

#### ✅ O que você deve fazer:
1. Criar um pequeno código na linguagem que preferir
2. Estabelecer a conexão com o servidor WebSocket
3. Ler a mensagem recebida >>  **Mensagem recebida do servidor: Olá mundo! LTIS 2025**
  
URL do servidor: websocket-fh6l.onrender.com

Código em Python:
import asyncio
import websockets

async def conectar():
    uri = "wss://websocket-fh6l.onrender.com"
    print(f"Conectando ao servidor WebSocket: {uri}")
    
    try:
        async with websockets.connect(uri) as websocket:
            print(" Conectado! Enviando 'ping'...")

            await websocket.send("ping")
            print(" Mensagem enviada: ping")

            while True:
                mensagem = await websocket.recv()
                print(f" Mensagem recebida: {mensagem}")
    except Exception as e:
        print(f" Erro ao conectar ou receber: {e}")

asyncio.run(conectar())

 Mensagem enviada pelo cliente: ping
 **Mensagem recebida do servidor: Olá mundo! LTIS 2025**



### Parte 3 - 🔎 Desafio teórico: Comunicação em tempo real entre usuários
Você precisa projetar um sistema simples de mensagens em tempo real para usuários logados.

---

#### 🧩 Cenário

O sistema permite que usuários escolham um **nome de usuário** ao entrar.

As mensagens podem ser:

- **Públicas**: todos os usuários conectados recebem.
- **Privadas**: enviadas para um **usuário específico** (por exemplo: `/msg joao oi`).

Outros requisitos:

- Um mesmo usuário pode estar conectado em **vários dispositivos ou abas ao mesmo tempo**.
- Se um usuário **cair e voltar**, ele deve continuar recebendo as mensagens normalmente.

---

#### ❓ Sua tarefa (teórica)

1. Que tipo de tecnologia de comunicação você usaria para esse cenário?
   
   R: Utilizaria a tecnologia de WebSockets para um chat em tempo real, e também usaria a ferramenta redis para gerenciamento dos usuarios e compartilhamentos de mensagens entre servidores, e para identifcação ou reconexão de usuarios alguma ferramenta de autenticação como JWT ou OAuth.

2. Como garantiria o envio correto para:
   - Todos os usuários?
     R: Manter todas as conexões WebSocket em uma Redis que guarda as conexões ativas, por usuário. Assim a mensagem é enviada a todas as conexões ativas no sistema.
     
   - Apenas um usuário específico?
     R: O usuário possui um Id especifico, o sistema busca esse Id no memória e envia a mensagem para esse Id.
     
   - Todas as sessões abertas de um mesmo usuário?
     R: Nesse caso a mensagem seria enviada para todas as conexões de um mesmo Id, por exemplo um usuário tem 3 conexões, o Id é o mesmo para as 3 que ficam agrupadas ao mesmo Id.

3. Existe alguma ferramenta ou biblioteca que facilitaria esse tipo de comunicação?  
   Se sim, **qual?** E **por quê?**
R:
   Sim, existem Socket.io (Facilita o uso de WebSockets, suporta múltiplas conexões por usuário, reconexão automática e pode ser integrado com Redis), Redis(Gerencia conexões e entrega de mensagens entre servidores), se houver necessidade Kafka (caso mensagens precisem ser persistidas, com garantia de entrega), e uma ferramenta para uso em larga escala seria também Pusher (Possui serviços prontos para Websockets com fallback automático e autenticação embutida).
Há mais ferramentas que podem ser utilizadas, porém essas são algumas que possuem escalabilidade, robustez e são mais simples de usar.

---

#### 🎯 Objetivo

Entender se você consegue identificar os desafios da comunicação em tempo real e pensar em soluções viáveis e escaláveis para eles.


## Boa sorte! 💻
