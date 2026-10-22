# Roteiro prático de montagem e configuração

[← Laboratório de duas sub-redes](../README.md) · [Todos os trabalhos](../../../README.md) · [Perfil](https://github.com/jardelluizoficial)

Executar uma etapa por vez, conferindo o resultado antes de avançar. Todos os comandos a seguir pertencem aos dispositivos simulados no Packet Tracer. O guia apresenta a sequência completa para consulta durante o acompanhamento.

## E01 Inserir dispositivos e salvar

1. Criar uma atividade nova no espaço Logical do Packet Tracer.
2. Em Network Devices e Routers, inserir um 1941. Confirmar as interfaces GigabitEthernet0/0 e GigabitEthernet0/1. Se o modelo disponível tiver nomes diferentes, conferir antes de adaptar os comandos.
3. Em Switches, inserir dois 2960.
4. Em End Devices, inserir quatro PC-PT.
5. Conferir os nomes: Router0, Switch0, Switch1, PC0, PC1, PC2 e PC3. Se o programa numerar de outra forma, ajustar os nomes de exibição.
6. Dispor Router0 acima dos dois switches; dois PCs abaixo de cada switch. Manter espaço para ler os nomes e ver os cabos.
7. Em File > Save As, salvar `topologia/u2-a2-tcpip-01-montagem.pkt`, relativo à raiz deste repositório.
8. Capturar `01-topologia-inicial.png`.

Conferência: exatamente sete dispositivos. Pergunta: qual é a função do switch e qual é a função do roteador neste desenho?

## E02 Conectar os cabos

Em Connections, selecionar **Copper Straight-Through**, clicar no dispositivo de origem, escolher a porta e repetir no destino. Usar este cabo nas seis ligações.

| Origem | Porta | Destino | Porta |
| --- | --- | --- | --- |
| PC0 | FastEthernet0 | Switch0 | FastEthernet0/1 |
| PC1 | FastEthernet0 | Switch0 | FastEthernet0/2 |
| PC2 | FastEthernet0 | Switch1 | FastEthernet0/1 |
| PC3 | FastEthernet0 | Switch1 | FastEthernet0/2 |
| Switch0 | GigabitEthernet0/1 | Router0 | GigabitEthernet0/0 |
| Switch1 | GigabitEthernet0/1 | Router0 | GigabitEthernet0/1 |

As portas dos PCs e as duas portas do roteador seguem o desenho exigido. A escolha das portas dos switches organiza a montagem. Conferir o nome da interface ao clicar.

Salvar o projeto e capturar `02-topologia-cabeada.png`. Os enlaces do roteador podem aparecer vermelhos antes de ativar suas interfaces. As portas dos switches podem passar por um período de inicialização antes de ficar verdes. Conferir configuração e estado antes de interpretar uma cor como falha permanente.

## E03 Entender o endereçamento

Usar a tabela do README. Todos os dispositivos configurados usam máscara 255.255.255.0, equivalente a /24.

Com essa máscara, PC0 192.168.1.2 e PC1 192.168.1.3 pertencem à rede 192.168.1.0. PC2 192.168.2.2 e PC3 192.168.2.3 pertencem à rede 192.168.2.0. Para alcançar outra rede, cada PC utiliza a interface do roteador conectada à sua própria rede como gateway.

Para cada /24: 32 - 24 = 8 bits restantes; 2^8 = 256 endereços; 256 - 2 = 254 endereços de host utilizáveis. Reservar .0 como identificação da rede e .255 como broadcast. A interface do roteador usa .1; os dois PCs usam .2 e .3; há 251 endereços de host ainda não atribuídos em cada rede.

Perguntas: por que PC0 não usa 192.168.2.1 como gateway? A máscara do PC0 permite considerar PC2 um vizinho da mesma sub-rede?

## E04 Configurar Router0

Abrir Router0 > CLI. Aguardar a inicialização. Se aparecer a pergunta do assistente inicial de configuração, responder `no`. Pressionar Enter quando solicitado.

Digitar na ordem, conferindo cada mudança de modo:

```text
enable
configure terminal
interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
interface gigabitEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
end
```

| Comando | Propósito |
| --- | --- |
| `enable` | Acessar o modo privilegiado |
| `configure terminal` | Entrar no modo de configuração global |
| `interface ...` | Selecionar a porta que será configurada |
| `ip address ...` | Atribuir IP e máscara à interface selecionada |
| `no shutdown` | Retirar o desligamento administrativo da interface |
| `exit` | Sair do modo atual para o nível anterior |
| `end` | Voltar ao modo privilegiado |

Capturar `03-router0-configuracao.png`, mostrando os comandos e os IPs utilizados. Em seguida, verificar:

```text
show ip interface brief
show ip route
```

O primeiro comando deve mostrar G0/0 com 192.168.1.1 e G0/1 com 192.168.2.1, ambas `up/up` quando os enlaces estiverem operacionais. Outras interfaces não utilizadas podem estar desligadas. Capturar `04-router0-interfaces.png`.

O segundo deve mostrar as redes 192.168.1.0/24 e 192.168.2.0/24 como diretamente conectadas, normalmente com código `C`. Dependendo da versão, também aparecem entradas locais `L` para os endereços das interfaces. Capturar `05-router0-rotas.png`.

As redes estão diretamente conectadas ao mesmo roteador; esta montagem não exige rota estática, protocolo de roteamento dinâmico nem rota padrão para a Internet. O IOS instala rotas conectadas quando as interfaces estão utilizáveis. [Referência Cisco](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipaddr/command/ipaddr-cr-book/ipaddr-r1.html).

Salvar a configuração do roteador:

```text
copy running-config startup-config
```

Se aparecer `Destination filename [startup-config]?`, pressionar Enter para aceitar. Salvar também o arquivo `.pkt` em File > Save: o salvamento da configuração do equipamento e o salvamento do projeto são ações diferentes.

Pergunta: por que o roteador tem dois endereços IP nesta atividade?

## E05 Configurar os quatro PCs

Em cada PC, abrir Desktop > IP Configuration e selecionar Static. Preencher IP Address, Subnet Mask e Default Gateway conforme a tabela do README. DNS não é necessário para os testes por endereço IP desta atividade; não adicionar um servidor DNS fictício.

| PC | IP Address | Subnet Mask | Default Gateway | Print |
| --- | --- | --- | --- | --- |
| PC0 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 | `06-pc0-enderecamento.png` |
| PC1 | 192.168.1.3 | 255.255.255.0 | 192.168.1.1 | `07-pc1-enderecamento.png` |
| PC2 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 | `08-pc2-enderecamento.png` |
| PC3 | 192.168.2.3 | 255.255.255.0 | 192.168.2.1 | `09-pc3-enderecamento.png` |

Salvar cada print com a identificação do PC visível. Conferir os doze campos, sem repetir um IP em dois dispositivos. Salvar o projeto.

## E06 Testar a conectividade

Voltar ao modo Realtime para os testes iniciais. Em cada PC, abrir Desktop > Command Prompt. Usar `ipconfig` para conferir os dados e executar os pings abaixo, um por vez. Não copiar os comentários do PDF como parte do comando.

| PC de origem | Gateway | Vizinho local | PCs da outra rede | Print |
| --- | --- | --- | --- | --- |
| PC0 | `ping 192.168.1.1` | `ping 192.168.1.3` | `ping 192.168.2.2` e `ping 192.168.2.3` | `10-pc0-testes-conectividade.png` |
| PC1 | `ping 192.168.1.1` | `ping 192.168.1.2` | `ping 192.168.2.2` e `ping 192.168.2.3` | `11-pc1-testes-conectividade.png` |
| PC2 | `ping 192.168.2.1` | `ping 192.168.2.3` | `ping 192.168.1.2` e `ping 192.168.1.3` | `12-pc2-testes-conectividade.png` |
| PC3 | `ping 192.168.2.1` | `ping 192.168.2.2` | `ping 192.168.1.2` e `ping 192.168.1.3` | `13-pc3-testes-conectividade.png` |

Essa cobertura testa todos os pares de PCs nos dois sentidos e acrescenta os gateways. Os dois pings explicitamente pedidos no PC0 estão incluídos.

Esperado: respostas `Reply from` dos destinos e, depois da estabilização, testes repetidos sem perdas. Registrar o observado em `resultados/testes-conectividade.csv`. A matriz desta execução registra os resultados observados. Em uma reprodução, registrar separadamente os novos resultados.

Uma resposta de ping comprova a ida do pedido ICMP e a volta da resposta naquele teste. Um primeiro timeout pode decorrer da resolução ARP; repetir e investigar antes de concluir. Falha persistente exige diagnóstico, e um ping sem resposta sozinho não determina a causa. [Referência Cisco](https://www.cisco.com/c/en/us/support/docs/ios-nx-os-software/ios-software-releases-121-mainline/12778-ping-traceroute.html).

Para os prints, ampliar a janela e manter comandos, origem e respostas legíveis. Se os testes não couberem, dividir em arquivos com sufixos `-a` e `-b`, registrando ambos na matriz. Preservar também a evidência de uma falha quando ela gerar uma investigação.

Depois da validação:

1. Capturar `14-topologia-final-validada.png` com nomes, redes e enlaces visíveis.
2. Salvar como `topologia/u2-a2-tcpip-02-base-validada.pkt`.
3. Reabrir essa cópia, conferir IPs e interfaces e repetir um ping entre sub-redes para verificar que o projeto foi salvo corretamente.

### Investigação se algum teste falhar

1. Identificar origem, destino, comando e mensagem recebida.
2. Conferir cabos, portas e estado dos enlaces.
3. Conferir IP, máscara e duplicidade de endereços.
4. Testar um vizinho local e o gateway da origem.
5. Conferir as duas interfaces e as rotas do Router0.
6. Conferir IP, máscara e gateway do PC de destino: a resposta também precisa encontrar o caminho de volta.
7. Corrigir a causa identificada e repetir os mesmos testes.

Pergunta: se PC0 alcança PC1, mas não PC2, quais verificações ajudam a localizar o problema?

## E07 Observar os pacotes como exercício extra

Trabalhar em uma cópia da base validada, salva como `topologia/u2-a2-tcpip-03-simulacao.pkt`.

1. Abrir Simulation e ajustar os filtros para ARP e ICMP, conforme a interface disponível.
2. Gerar um ping de PC0 para PC2.
3. Avançar os eventos com Capture/Forward e abrir os detalhes das PDUs.
4. Se o ARP já estiver em cache e não aparecer, conferir a tabela ARP e preparar uma nova execução acompanhada. Não interpretar ausência de novo ARP como falha.
5. Capturar `15-simulation-arp.png`, quando houver a resolução, e `16-simulation-icmp.png` com o percurso e os detalhes relevantes.

Observar que PC0 usa o MAC do gateway para enviar o quadro destinado a outra rede. O roteador encaminha o pacote por outra interface e monta um novo quadro para a rede de destino. Os endereços IP de origem e destino permanecem os dos PCs neste cenário sem NAT; os endereços MAC mudam entre os enlaces.

O ping usa ICMP sobre IP; ele não utiliza TCP nem UDP. O nome da arquitetura TCP/IP não significa que todo tráfego da atividade use TCP.

## E08 Diagnosticar gateway incorreto como exercício extra

Seguir o [estudo de incidente](incidente-gateway.md). Começar somente depois da base validada e usar uma cópia. Ao terminar, restaurar o valor original e repetir os testes.

## E09 e E10 Concluir as entregas

Preencher o relatório acadêmico com os resultados observados e revisar todas as exigências do enunciado. No GitHub, atualizar o README, registrar a versão usada, conferir os links das evidências e incluir apenas os extras efetivamente executados. Comparar os novos resultados com as evidências desta execução.

---

[← Voltar ao laboratório](../README.md) · [Evidências](evidencias.md) · [Roteiro](roteiro-pratico.md) · [Incidente](incidente-gateway.md)
