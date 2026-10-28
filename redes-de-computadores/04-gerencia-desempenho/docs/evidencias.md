# Índice das evidências

[← Voltar ao laboratório](../README.md)

As 21 capturas abaixo foram preservadas com os nomes e o conteúdo originais. Os números identificam os arquivos, não a quantidade de testes independentes.

| Nº | Captura | O que documenta |
| --- | --- | --- |
| 01 | [01-roteador-ip-interface.png](../imagens/01-roteador-ip-interface.png) | Configuração IPv4, ativação e salvamento da G0/0 do roteador. |
| 02 | [02-pc0-configuracao-ip.png](../imagens/02-pc0-configuracao-ip.png) | PC0 com IPv4 estático 192.168.1.2/24 e gateway .1. |
| 03 | [03-pc1-configuracao-ip.png](../imagens/03-pc1-configuracao-ip.png) | PC1 com IPv4 estático 192.168.1.3/24 e gateway .1. |
| 04 | [04-pc2-configuracao-ip.png](../imagens/04-pc2-configuracao-ip.png) | PC2 com IPv4 estático 192.168.1.4/24 e gateway .1. |
| 05 | [05-servidor-configuracao-ip.png](../imagens/05-servidor-configuracao-ip.png) | Server0 com IPv4 estático 192.168.1.100/24 e gateway .1. |
| 06 | [06-servidor-http-ativo.png](../imagens/06-servidor-http-ativo.png) | Serviço HTTP ativado e arquivos presentes no servidor. |
| 07 | [07-pc0-ping-servidor.png](../imagens/07-pc0-ping-servidor.png) | Ping do PC0: quatro respostas e nenhuma perda. |
| 08 | [08-pc1-acesso-http.png](../imagens/08-pc1-acesso-http.png) | Página HTTP do servidor recebida pelo PC1. |
| 09 | [09-pc1-ping-servidor.png](../imagens/09-pc1-ping-servidor.png) | Ping do PC1: quatro respostas e nenhuma perda. |
| 10 | [10-pc2-ping-servidor.png](../imagens/10-pc2-ping-servidor.png) | Ping do PC2: quatro respostas; primeira 3 ms, demais <1 ms. |
| 11 | [11-simulacao-icmp-pc2-servidor.png](../imagens/11-simulacao-icmp-pc2-servidor.png) | Intercâmbio ICMP de 0,000 a 0,007 s no modo Simulation. |
| 12 | [12-diagnostico-tcp-descarte.png](../imagens/12-diagnostico-tcp-descarte.png) | Inspeção do TCP: RST+ACK com sequência antiga descartado como duplicado. |
| 13 | [13-simulacao-http-inicio.png](../imagens/13-simulacao-http-inicio.png) | Primeiro evento HTTP local do PC2 em 0,011 s. |
| 14 | [14-simulacao-http-resposta.png](../imagens/14-simulacao-http-resposta.png) | Resposta HTTP ao PC2 em 0,023 s e eventos TCP até 0,033 s. |
| 15 | [15-http-detalhes-pacote-ip.png](../imagens/15-http-detalhes-pacote-ip.png) | Cabeçalhos da resposta: IPs, portas e tamanho total IP de 183 bytes. |
| 16 | [16-http-tamanho-conteudo.png](../imagens/16-http-tamanho-conteudo.png) | Content-Length de 62 bytes, destacado na captura enviada. |
| 17 | [17-pc2-pagina-http-recebida.png](../imagens/17-pc2-pagina-http-recebida.png) | Arquivo helloworld.html exibido no PC2 em verificação posterior. |
| 18 | [18-switch-fa04-estatisticas-parte1.png](../imagens/18-switch-fa04-estatisticas-parte1.png) | Primeira parte dos contadores da Fa0/4, com paginação. |
| 19 | [19-switch-fa04-estatisticas-completas.png](../imagens/19-switch-fa04-estatisticas-completas.png) | Contadores completos, velocidade, duplex e erros da Fa0/4. |
| 20 | [20-switch-tabela-mac.png](../imagens/20-switch-tabela-mac.png) | Tabela MAC dinâmica com roteador, PC2 e servidor. |
| 21 | [21-topologia-completa.png](../imagens/21-topologia-completa.png) | Topologia completa com enlaces ativos. |
