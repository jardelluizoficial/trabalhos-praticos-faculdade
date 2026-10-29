# Trabalhos práticos da faculdade

Atividades da graduação em **Computação em Nuvem — Anhanguera**, documentadas por Jardel Luiz. Cada prática reúne o objetivo, o ambiente, os procedimentos e os resultados observados.

[Perfil](https://github.com/jardelluizoficial) · [Suporte](https://github.com/jardelluizoficial/laboratorios-praticos-de-suporte) · [Cisco](https://github.com/jardelluizoficial/cisco-networking-labs) · **Faculdade**

## Redes de Computadores

Os laboratórios abaixo foram realizados no **Cisco Packet Tracer**. A sequência reúne comunicação em rede local, roteamento entre sub-redes com IPv4 e IPv6 e análise de desempenho com ICMP e HTTP.

| Laboratório | O que foi praticado | Resultado documentado |
| --- | --- | --- |
| **[01 · Rede local com dois PCs e um switch](redes-de-computadores/01-lan-packet-tracer/README.md)** | Cabeamento, IPv4, máscara de sub-rede e teste com ping. | Quatro respostas, sem perda de pacotes. |
| **[02 · Duas sub-redes com TCP/IP](redes-de-computadores/02-tcpip-sub-redes/README.md)** | Interfaces do roteador, gateway, rotas conectadas, ARP e ICMP. | 16 testes finais sem perdas e um incidente de gateway corrigido. |
| **[03 · Duas sub-redes com IPv6](redes-de-computadores/03-ipv6-duas-sub-redes/README.md)** | Endereçamento /64, gateway, ativação de interfaces e roteamento IPv6. | Oito testes nos quatro PCs, com 32 respostas e 0% de perda. |
| **[04 · Gerência de desempenho em uma rede IPv4](redes-de-computadores/04-gerencia-desempenho/README.md)** | Tráfego ICMP e HTTP, tempos de resposta, taxa útil e contadores do switch. | 12 respostas sem perdas, acesso HTTP e estimativa da utilização por um fluxo ICMP periódico. |

## Escolha o ponto de partida

- **Primeiro contato com redes:** comece pela [rede local](redes-de-computadores/01-lan-packet-tracer/README.md).
- **Diagnóstico de falhas:** veja o [caso do gateway incorreto](redes-de-computadores/02-tcpip-sub-redes/docs/incidente-gateway.md), com comparação antes e depois da correção.
- **Reprodução da prática:** consulte o [roteiro das duas sub-redes](redes-de-computadores/02-tcpip-sub-redes/docs/roteiro-pratico.md) e abra a [base funcional `.pkt`](redes-de-computadores/02-tcpip-sub-redes/topologia/u2-a2-tcpip-02-base-validada.pkt) no Packet Tracer.
- **Conferência dos resultados:** acesse a [matriz de testes](redes-de-computadores/02-tcpip-sub-redes/resultados/testes-conectividade.csv) e o [índice de evidências](redes-de-computadores/02-tcpip-sub-redes/docs/evidencias.md).

- **Prática com IPv6:** veja o [laboratório de duas sub-redes /64](redes-de-computadores/03-ipv6-duas-sub-redes/README.md), com [simulação `.pkt`](redes-de-computadores/03-ipv6-duas-sub-redes/topologia/rede-ipv6.pkt) e testes locais e entre redes.

- **Desempenho da rede:** veja o [laboratório de ICMP e HTTP](redes-de-computadores/04-gerencia-desempenho/README.md), com [cálculos e limites da medição](redes-de-computadores/04-gerencia-desempenho/docs/analise-desempenho.md).

## Como os registros estão organizados

Cada laboratório tem seu próprio README com a explicação principal e uma pasta `imagens/` com as capturas. As práticas mais detalhadas também contêm `docs/` para os detalhes, `topologia/` para os arquivos do simulador e `resultados/` para os testes.

## Continue explorando

As práticas da [Cisco Networking Academy](https://github.com/jardelluizoficial/cisco-networking-labs) ficam em um repositório próprio. Os exercícios com Windows e Active Directory estão nos [laboratórios práticos de suporte](https://github.com/jardelluizoficial/laboratorios-praticos-de-suporte).

Para comparar com o incidente de gateway do Packet Tracer, veja o [diagnóstico de DNS no Windows](https://github.com/jardelluizoficial/laboratorios-praticos-de-suporte/tree/main/diagnostico-dns): o servidor responde por IP, mas a consulta de nome falha quando o cliente utiliza um DNS incorreto.
