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
4. No PC1, abra `Desktop > Web Browser`, digite `http://192.168.1.100` e pressione `Go` uma vez. Confira o recebimento da página.

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

Consulte a [análise](analise-desempenho.md) para distinguir contadores acumulados de taxas. Para uma futura medição de utilização, faça duas leituras em intervalo conhecido e calcule a diferença de bytes por direção; essa coleta adicional não faz parte dos resultados publicados.

Salve o projeto `.pkt` e mantenha as imagens com nomes numerados. O [índice de evidências](evidencias.md) mostra os registros desta execução.
