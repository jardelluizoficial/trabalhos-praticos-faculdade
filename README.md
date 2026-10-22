# Trabalhos práticos da faculdade

Atividades da graduação em **Computação em Nuvem — Anhanguera**, documentadas por Jardel Luiz. Cada prática reúne o objetivo, o ambiente, os procedimentos e os resultados observados.

[Perfil](https://github.com/jardelluizoficial) · [Suporte N1](https://github.com/jardelluizoficial/laboratorio-suporte-n1) · [Cisco](https://github.com/jardelluizoficial/cisco-networking-labs) · **Faculdade**

## Redes de Computadores

Os laboratórios abaixo foram realizados no **Cisco Packet Tracer**. A sequência vai da comunicação dentro de uma rede local à comunicação entre duas sub-redes.

| Laboratório | O que foi praticado | Resultado documentado |
| --- | --- | --- |
| **[01 · Rede local com dois PCs e um switch](redes-de-computadores/01-lan-packet-tracer/README.md)** | Cabeamento, IPv4, máscara de sub-rede e teste com ping. | Quatro respostas, sem perda de pacotes. |
| **[02 · Duas sub-redes com TCP/IP](redes-de-computadores/02-tcpip-sub-redes/README.md)** | Interfaces do roteador, gateway, rotas conectadas, ARP e ICMP. | 16 testes finais sem perdas e um incidente de gateway corrigido. |

## Escolha o ponto de partida

- **Primeiro contato com redes:** comece pela [rede local](redes-de-computadores/01-lan-packet-tracer/README.md).
- **Diagnóstico de falhas:** veja o [caso do gateway incorreto](redes-de-computadores/02-tcpip-sub-redes/docs/incidente-gateway.md), com comparação antes e depois da correção.
- **Reprodução da prática:** consulte o [roteiro das duas sub-redes](redes-de-computadores/02-tcpip-sub-redes/docs/roteiro-pratico.md) e abra a [base funcional `.pkt`](redes-de-computadores/02-tcpip-sub-redes/topologia/u2-a2-tcpip-02-base-validada.pkt) no Packet Tracer.
- **Conferência dos resultados:** acesse a [matriz de testes](redes-de-computadores/02-tcpip-sub-redes/resultados/testes-conectividade.csv) e o [índice de evidências](redes-de-computadores/02-tcpip-sub-redes/docs/evidencias.md).

## Como os registros estão organizados

Cada laboratório tem seu próprio README com a explicação principal e uma pasta `imagens/` com as capturas. A prática de duas sub-redes também contém `docs/` para os detalhes, `topologia/` para os arquivos do simulador e `resultados/` para os testes.

## Continue explorando

As práticas da [Cisco Networking Academy](https://github.com/jardelluizoficial/cisco-networking-labs) ficam em um repositório próprio. Os exercícios com Windows e Active Directory estão nos [laboratórios de Suporte N1](https://github.com/jardelluizoficial/laboratorio-suporte-n1).
