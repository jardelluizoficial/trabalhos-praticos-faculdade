# Fundamentos de redes para Suporte e evolução em cloud

Este laboratório desenvolve a habilidade de observar uma configuração, prever o caminho de um pacote e conferir a previsão por meio de testes. Isso ajuda a explicar um incidente e escolher a próxima verificação.

## Conceitos usados na montagem

| Conceito | Significado nesta prática | Pergunta para conferir a compreensão |
| --- | --- | --- |
| IPv4 | Endereço lógico atribuído à interface de rede | Qual IP identifica PC0? |
| Máscara e prefixo | Definem a parte do endereço que identifica a rede | Por que /24 corresponde a 255.255.255.0? |
| Sub-rede | Conjunto de endereços delimitado pelo prefixo | PC0 e PC2 pertencem à mesma rede? |
| Gateway padrão | Próximo salto usado pelo PC quando não existe uma rota mais específica para o destino remoto | Qual interface recebe o pacote de PC0 destinado a PC2? |
| Switch de camada 2 | Encaminha quadros na rede local com base em endereços MAC | PC0 precisa passar pelo roteador para alcançar PC1? |
| Roteador | Encaminha pacotes entre redes segundo sua tabela de rotas | Por que Router0 precisa de uma interface em cada rede? |
| ARP | Resolve um IPv4 para um endereço MAC no enlace local | Ao falar com PC2, PC0 procura o MAC de PC2 ou o do gateway? |
| ICMP | Protocolo de controle usado pelo ping tradicional | O que uma resposta de ping permite afirmar? |

Um IP privado é apropriado para esta simulação. Endereço privado não é uma política de segurança: a permissão de tráfego depende também de controles configurados.

## Arquitetura TCP/IP e relação com OSI

Usaremos inicialmente a organização de TCP/IP em quatro camadas. Alguns materiais didáticos a apresentam em cinco, separando enlace e física. Se a faculdade usar essa variante, ajustar os nomes mantendo as funções.

| TCP/IP em quatro camadas | Relação aproximada com OSI | Exemplo para estudar |
| --- | --- | --- |
| Aplicação | Aplicação, apresentação e sessão | HTTP e DNS são exemplos; não serão configurados na base |
| Transporte | Transporte | TCP e UDP; o ping desta prática não testa seus serviços |
| Internet | Rede | IP e ICMP; encaminhamento pelo roteador |
| Acesso à rede | Enlace e física | Ethernet, quadros, MAC, portas e cabos |

ARP será observado como mecanismo de apoio à entrega local entre IP e Ethernet, evitando prender a aprendizagem a uma classificação didática única. Referência da arquitetura: [RFC 1122](https://www.rfc-editor.org/info/rfc1122/).

## Aplicação imediata em Suporte

Chamado simulado: “Consigo me comunicar com um computador próximo, mas não com o computador de outro setor”. Antes de alterar configurações, registrar origem, destino, impacto e mensagem recebida. Conferir IP e máscara, comunicação local, gateway e caminho de retorno. Anotar a evidência de cada hipótese, aplicar a menor correção adequada e validar novamente.

A sequência praticada aqui é: sintoma, hipótese, teste, resultado, correção e validação. Se a causa exigir acesso ou conhecimento fora do atendimento, o registro ajuda no encaminhamento. Um ping com sucesso não comprova, por exemplo, que um site ou uma aplicação está funcionando.

## Relação com Azure

Na evolução dos estudos, vamos relacionar os intervalos IP desta prática a VNets e subnets do Azure. O entendimento de endereços e rotas ajuda a investigar comunicação entre recursos, mas a plataforma usa componentes e regras próprios.

Uma diferença concreta: o Azure reserva os quatro primeiros IPs e o último de cada sub-rede. Uma /24 tem 251 endereços disponíveis para atribuição a recursos. Por isso os IPs .2 e .3 exigidos neste Packet Tracer não devem ser copiados para VMs Azure. Aqui, cada rede possui 254 endereços de host utilizáveis, dos quais três são ocupados pelo roteador e pelos dois PCs. [Microsoft Learn](https://learn.microsoft.com/pt-br/azure/virtual-network/virtual-networks-faq).

Esta etapa é uma comparação conceitual. Não é necessário criar assinatura, VNet ou VM para concluir o laboratório.

## Relação com Cloud Security

Duas sub-redes organizam endereços e caminhos. Na montagem base, o roteador permite encaminhamento entre elas, sem uma política de filtragem configurada. Para estudar proteção, precisamos perguntar quais origens podem acessar quais destinos, usando quais protocolos e portas.

No Azure, grupos de segurança de rede, chamados NSGs, usam regras para permitir ou negar tráfego. Vamos relacionar esse conceito ao que foi observado aqui; uma eventual prática de ACL no Packet Tracer será outro exercício, com critérios próprios. [Visão geral dos NSGs](https://learn.microsoft.com/pt-br/azure/virtual-network/network-security-groups-overview).

Ter conectividade e ter autorização são questões diferentes. ICMP respondendo não comprova que a rede está segura ou que uma porta de aplicação está acessível.

## O que explicar ao final

1. O caminho de PC0 para PC1 e o caminho de PC0 para PC2.
2. Como a máscara determina a necessidade de usar o gateway.
3. Por que as interfaces do roteador usam .1 em redes diferentes.
4. Por que o ping usa ICMP e não testa diretamente uma sessão TCP.
5. Como um gateway incorreto pode preservar comunicação local e impedir a remota nesta topologia.
6. Qual evidência diferencia uma hipótese de uma conclusão.
7. Qual regra de endereçamento muda quando o cenário passa para Azure.
