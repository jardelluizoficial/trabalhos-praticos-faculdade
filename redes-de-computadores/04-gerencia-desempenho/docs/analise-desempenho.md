# Análise dos indicadores de desempenho

[← Voltar ao laboratório](../README.md)

## Conectividade e tempo ICMP

Cada um dos três PCs enviou quatro solicitações ao servidor e recebeu quatro respostas. Total: `3 × 4 = 12` solicitações e 12 respostas. Perda: `(12 − 12) ÷ 12 × 100 = 0%`.

PC0 e PC1 apresentaram respostas abaixo de 1 ms. No PC2, a primeira foi de 3 ms e as demais abaixo de 1 ms. O resumo exibiu média de 0 ms; a apresentação em milissegundos inteiros não comprova atraso literalmente nulo.

Na [captura ICMP do modo Simulation](../imagens/11-simulacao-icmp-pc2-servidor.png), o retorno ocorreu em `0,007 s`, a partir de `0,000 s`: **7 ms de ida e volta**. As cinco linhas representam etapas do intercâmbio PC2 → switch → servidor → switch → PC2.

## Intervalo HTTP e taxa útil

Na [lista de eventos](../imagens/14-simulacao-http-resposta.png), o primeiro evento HTTP local do PC2 ocorre em `0,011 s`; a resposta HTTP chega ao PC2 em `0,023 s`. O intervalo adotado é:

```text
0,023 − 0,011 = 0,012 s = 12 ms
```

Esse intervalo abrange a solicitação e a resposta. Não é apenas o tempo de serialização do arquivo no fio e exclui o estabelecimento TCP anterior. A captura ainda registra eventos TCP até `0,033 s`.

Os [detalhes da resposta](../imagens/16-http-tamanho-conteudo.png) mostram:

| Campo | Valor observado |
| --- | --- |
| IP de origem | 192.168.1.100 |
| IP de destino | 192.168.1.4 |
| Porta TCP de origem | 80 |
| Porta TCP de destino | 1029 |
| IP Total Length | 183 bytes |
| HTTP Content-Length | 62 bytes |
| HTTP Connection | close |

Para considerar somente o conteúdo útil da aplicação:

```text
62 bytes × 8 = 496 bits
496 ÷ 0,012 = 41.333,3 bit/s ≈ 41,3 kbit/s
```

O resultado é **goodput estimado no intervalo definido**. Não inclui todos os cabeçalhos, confirmações ou quadros. O valor de 183 bytes se refere ao pacote IP completo e não substitui os 62 bytes do corpo HTTP no cálculo de conteúdo útil.

Uma resposta de apenas 62 bytes, observada uma vez, não serve como benchmark de transferência sustentada nem demonstra a vazão máxima da rede. A página recebida na [captura 17](../imagens/17-pc2-pagina-http-recebida.png) é uma verificação posterior; não se atribuem automaticamente a ela os tempos da captura de simulação.

## Capacidade e utilização

A [Fa0/4](../imagens/19-switch-fa04-estatisticas-completas.png) foi apresentada como `up/up`, full-duplex e 100 Mb/s. Isso é a capacidade nominal do enlace observado, não a taxa de dados transferida pela aplicação.

Uma forma de medir taxa por contadores seria:

```text
taxa por direção (bit/s) = diferença de bytes × 8 ÷ intervalo em segundos
utilização por direção (%) = taxa medida ÷ capacidade do enlace × 100
```

Os contadores publicados são uma leitura acumulada, sem zeragem e sem segunda leitura em intervalo controlado. Portanto, **a utilização total não foi medida de forma confiável**. A relação entre a taxa do corpo HTTP e 100 Mb/s ignoraria o restante do tráfego e não representaria a utilização total da porta.

Na leitura, as médias de cinco minutos apareceram como zero, apesar de 956 pacotes/193.351 bytes recebidos e 2.357 pacotes/263.570 bytes enviados. Os erros de entrada, saída e colisões foram zero. Dez reinicializações da interface estavam registradas, sem investigação de causa; não devem ser omitidas nem interpretadas como falhas atuais comprovadas.

A [tabela MAC](../imagens/20-switch-tabela-mac.png) mostrou entradas dinâmicas na VLAN 1: roteador `0000.0c7a.7701` em Gi0/1, PC2 `0004.9a6d.8143` em Fa0/3 e servidor `00e0.8fcd.c25e` em Fa0/4.

## Diagnóstico TCP observado

Antes da captura bem-sucedida, houve tentativas em que apareceram eventos TCP sem HTTP. Na [inspeção da camada 4](../imagens/12-diagnostico-tcp-descarte.png), o Server0 recebeu do PC2 um segmento RST+ACK com número de sequência antigo e o descartou como duplicado. A captura documenta esse comportamento, mas não estabelece sua causa inicial.

Depois de reabrir o navegador, solicitar a página uma única vez e avançar os eventos, a resposta HTTP foi observada. Não há evidência suficiente para classificar a ocorrência como congestionamento ou problema de cabo.

## Limites e conclusão

Os testes comprovaram conectividade local e recebimento de conteúdo HTTP. Não avaliaram DNS, segurança, roteamento entre sub-redes ou saturação. Tempos de Realtime e Simulation foram mantidos como observações separadas. A análise distingue resultados medidos, valores acumulados e cálculos derivados, sem transformar limitações da coleta em resultados inventados.
