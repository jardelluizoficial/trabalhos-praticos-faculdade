# Como reproduzir a prática

[← Voltar ao laboratório](../README.md)

Abra a [simulação salva](../topologia/rede-gerencia-desempenho.pkt) no Cisco Packet Tracer ou monte o cenário abaixo. Os tempos e os contadores podem variar em uma nova execução; as imagens documentam a coleta original.

## Montagem e endereços

Use um Cisco 1941, um 2960-24TT, três PC-PT e um Server-PT, com cabos Copper Straight-Through.

| Origem | Interface | Destino | Porta |
| --- | --- | --- | --- |
| Router0 | G0/0 | Switch0 | Gi0/1 |
| PC0 | FastEthernet0 | Switch0 | Fa0/1 |
| PC1 | FastEthernet0 | Switch0 | Fa0/2 |
| PC2 | FastEthernet0 | Switch0 | Fa0/3 |
| Server0 | FastEthernet0 | Switch0 | Fa0/4 |

No Router0, abra a CLI e execute:

```text
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
end
copy running-config startup-config
```

Aceite o nome do arquivo de destino com Enter. Depois confira:

```text
show ip interface brief
```

A G0/0 deve mostrar `192.168.1.1` e `up/up`.

Em cada PC e no servidor, use `Desktop > IP Configuration > Static`. Configure PC0 `.2`, PC1 `.3`, PC2 `.4` e Server0 `.100`, todos com prefixo `192.168.1`, máscara `255.255.255.0` e gateway `192.168.1.1`. DNS não é necessário para estes testes por IP.

## Conectividade e aplicação

1. No servidor, abra `Services > HTTP` e confira `On` e a presença de `helloworld.html`.
2. Em cada PC, abra `Desktop > Command Prompt` e execute `ping 192.168.1.100`.
3. Registre solicitações, respostas, perdas e tempos. Se houver falha, confira endereços, máscara, cabeamento e estado das portas antes de concluir a causa.
4. No PC1, abra `Desktop > Web Browser`, digite `http://192.168.1.100` e pressione `Go` uma vez. Confira o recebimento da página. Depois acesse `http://192.168.1.100/helloworld.html` no PC1 e registre o conteúdo “Hello, world!”, como na [captura 22](../imagens/22-pc1-arquivo-http-recebido.png).

## ICMP no modo Simulation

1. Entre em `Simulation`, habilite ICMP nos filtros e envie uma PDU simples do PC2 para Server0.
2. Use `Capture/Forward` para observar as etapas, ou `Auto Capture/Play` para avançar automaticamente.
3. Registre o evento inicial no PC2 e o retorno da resposta ao PC2. Na coleta original foram `0,000 s` e `0,007 s`.
4. Conte as solicitações separadamente das linhas de eventos: uma mesma PDU aparece em vários equipamentos.

## HTTP no modo Simulation

1. Conclua o teste anterior. Prepare uma nova captura e filtre `HTTP` e `TCP`. Na coleta documentada, `Constant Delay` estava desmarcado.
2. No PC2, mantenha o navegador aberto e solicite `http://192.168.1.100/helloworld.html` uma única vez.
3. Avance a simulação. Os eventos TCP aparecem antes do HTTP; a página não termina de carregar enquanto os eventos necessários não forem processados.
4. Registre a primeira ocorrência HTTP local no PC2 e a chegada da resposta HTTP ao PC2. Na captura original: `0,011 s` e `0,023 s`.
5. Abra o evento HTTP recebido pelo PC2 e consulte `Inbound PDU Details`. Confira IPs, portas e o campo `Content-Length` da resposta.
6. Guarde a captura da lista de eventos e dos detalhes da PDU antes de reiniciar a simulação.
7. Confira o conteúdo no navegador. A captura 17 registra uma verificação posterior em Realtime, sem atribuir a ela os tempos de outra execução.

Se aparecerem somente eventos TCP, primeiro confira o filtro e avance os eventos. Na ocorrência documentada nesta prática, a inspeção revelou um RST+ACK duplicado; após reabrir o navegador e enviar uma nova solicitação única, o HTTP foi registrado. Isso não deve ser tratado como diagnóstico universal para qualquer falha HTTP.

## Contadores e análise

No Switch0:

```text
enable
show interfaces fastEthernet 0/4
show mac address-table
```

Pressione Espaço se aparecer `--More--`. Registre estado, velocidade, duplex, pacotes, bytes, erros e colisões. A tabela MAC relaciona o servidor `00e0.8fcd.c25e` à Fa0/4 na captura original.

Consulte a [comparação das duas leituras](analise-desempenho.md#verificação-dos-contadores). Nesta execução, os contadores permaneceram iguais após novos acessos HTTP e não foram usados para calcular utilização. Em uma coleta por contadores, é necessário verificar que eles acompanham o tráfego e registrar um intervalo conhecido antes de calcular a diferença de bytes por direção.

Salve o projeto `.pkt` e mantenha as imagens com nomes numerados. O [índice de evidências](evidencias.md) mostra os registros desta execução.

## Experimento complementar com ping periódico

1. Em `Simulation`, remova a PDU simples anterior e configure `Add Complex PDU` no PC2. Use aplicação `PING`, destino `192.168.1.100`, origem `192.168.1.4`, TTL `32`, TOS `0`, Sequence Number `1` e Size `1000`.
2. Marque `Periodic`, informe intervalo `1` segundo e crie a PDU. Mantenha somente essa PDU periódica na lista.
3. Prepare a captura com `Reset Simulation`, filtros ARP e ICMP e `Constant Delay` desmarcado.
4. Avance com `Capture/Forward` até **3.000 s**, que significa três segundos. Não confunda com `0.003 s`, equivalente a três milissegundos.
5. Registre os três ciclos completos com envio em 0, 1 e 2 s. Exclua o início do quarto ciclo em 3 s da janela `0 ≤ t < 3 s`.
6. Abra um evento no Server0 e confira `Inbound PDU Details` e `Outbound PDU Details`. Registre o Total Length do pacote IP recebido e da resposta enviada: na coleta, ambos são 1028 bytes.
7. Calcule volume e taxa por sentido usando os pacotes que atravessam a porta do servidor, sem contar cada linha de encaminhamento como um novo pacote. Compare a taxa IP com 100 Mb/s e declare a exclusão do custo Ethernet e dos demais tráfegos.
8. Salve um arquivo separado, preservando a primeira simulação. A [versão complementar](../topologia/rede-gerencia-desempenho-complemento.pkt), o [método detalhado](utilizacao-fluxo-icmp.md) e as capturas 25 a 28 registram este experimento.
