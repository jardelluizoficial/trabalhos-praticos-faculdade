# Índice das evidências

[← Laboratório de duas sub-redes](../README.md) · [Todos os trabalhos](../../../README.md) · [Perfil](https://github.com/jardelluizoficial)

Capturas reais da montagem, configuração, testes e diagnóstico. Os arquivos originais foram preservados; o histórico de falhas e repetições permanece visível nas capturas.

| Captura | Evidência |
|---|---|
| [01-topologia-inicial.png](../imagens/01-topologia-inicial.png) | 01 topologia inicial |
| [02-topologia-cabeada.png](../imagens/02-topologia-cabeada.png) | 02 topologia cabeada |
| [04-router0-interfaces.png](../imagens/04-router0-interfaces.png) | 04 router0 interfaces |
| [05-router0-rotas.png](../imagens/05-router0-rotas.png) | 05 router0 rotas |
| [06-pc0-enderecamento.png](../imagens/06-pc0-enderecamento.png) | 06 pc0 enderecamento |
| [07-pc1-enderecamento.png](../imagens/07-pc1-enderecamento.png) | 07 pc1 enderecamento |
| [08-pc2-enderecamento.png](../imagens/08-pc2-enderecamento.png) | 08 pc2 enderecamento |
| [09-pc3-enderecamento.png](../imagens/09-pc3-enderecamento.png) | 09 pc3 enderecamento |
| [10-pc0-testes-conectividade-b.png](../imagens/10-pc0-testes-conectividade-b.png) | 10 pc0 testes conectividade b |
| [10-pc0-testes-conectividade.png](../imagens/10-pc0-testes-conectividade.png) | 10 pc0 testes conectividade |
| [11-pc1-testes-conectividade.png](../imagens/11-pc1-testes-conectividade.png) | 11 pc1 testes conectividade |
| [12-pc2-testes-conectividade.png](../imagens/12-pc2-testes-conectividade.png) | 12 pc2 testes conectividade |
| [13-pc3-testes-conectividade.png](../imagens/13-pc3-testes-conectividade.png) | 13 pc3 testes conectividade |
| [14-topologia-final-validada.png](../imagens/14-topologia-final-validada.png) | 14 topologia final validada |
| [15-simulation-arp.png](../imagens/15-simulation-arp.png) | 15 simulation arp |
| [16-simulation-icmp.png](../imagens/16-simulation-icmp.png) | 16 simulation icmp |
| [17-pc0-gateway-incorreto.png](../imagens/17-pc0-gateway-incorreto.png) | 17 pc0 gateway incorreto |
| [18-pc0-incidente-testes.png](../imagens/18-pc0-incidente-testes.png) | 18 pc0 incidente testes |
| [19-pc0-gateway-restaurado.png](../imagens/19-pc0-gateway-restaurado.png) | 19 pc0 gateway restaurado |
| [20-pc0-conectividade-restaurada.png](../imagens/20-pc0-conectividade-restaurada.png) | 20 pc0 conectividade restaurada |

A configuração do roteador está visível na captura 04; não foi necessário um arquivo 03 separado. Para PC0, a captura 10 inicial registra as primeiras tentativas e a captura 10-b comprova a repetição sem perdas para os destinos remotos.

Na captura 15, o ARP registrado é broadcast e tem 192.168.1.2 como IP de origem e IP alvo. Na captura 16, o pedido ICMP de PC0 para PC2 é encaminhado entre as duas interfaces do roteador.

---

[← Voltar ao laboratório](../README.md) · [Evidências](evidencias.md) · [Roteiro](roteiro-pratico.md) · [Incidente](incidente-gateway.md)
