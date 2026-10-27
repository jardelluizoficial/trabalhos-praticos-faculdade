# Roteiro de reprodução da rede IPv6

[← Voltar ao laboratório](../README.md)

Este roteiro descreve a montagem e os testes realizados em 26/10/2026. Para consultar a rede já montada, baixe [rede-ipv6.pkt](../topologia/rede-ipv6.pkt) e abra no Cisco Packet Tracer. Aguarde os enlaces ficarem ativos antes de repetir os pings.

## 1 Montar a topologia

Adicione um roteador 1941, dois switches 2960-24TT e quatro PCs. Use os nomes Router0, Switch0, Switch1 e PC0 a PC3. Para reproduzir o cabeamento, use Copper Straight-Through:

| Dispositivo e porta | Destino e porta |
| --- | --- |
| PC0 · FastEthernet0 | Switch0 · FastEthernet0/1 |
| PC1 · FastEthernet0 | Switch0 · FastEthernet0/2 |
| PC2 · FastEthernet0 | Switch1 · FastEthernet0/1 |
| PC3 · FastEthernet0 | Switch1 · FastEthernet0/2 |
| Switch0 · GigabitEthernet0/1 | Router0 · GigabitEthernet0/0 |
| Switch1 · GigabitEthernet0/1 | Router0 · GigabitEthernet0/1 |

## 2 Configurar o Router0

Abra a CLI. Se o diálogo de configuração inicial aparecer, responda `no`. No prompt `Router>`, execute:

```text
enable
configure terminal
ipv6 unicast-routing
interface gigabitEthernet 0/0
ipv6 address 2001:DB8:1::1/64
no shutdown
exit
interface gigabitEthernet 0/1
ipv6 address 2001:DB8:2::1/64
no shutdown
exit
end
copy running-config startup-config
```

Confirme com Enter o nome de destino sugerido para salvar. Depois, execute:

```text
show ipv6 interface brief
```

A G0/0 deve apresentar `2001:DB8:1::1`; a G0/1, `2001:DB8:2::1`. Ambas devem indicar `[up/up]`. A Vlan1 não participa desta atividade. Os endereços link-local podem variar entre montagens.

`ipv6 unicast-routing` permite o encaminhamento IPv6; `ipv6 address` atribui o endereço à interface; `no shutdown` ativa a interface. O comando de cópia salva a configuração do roteador, mas também é necessário salvar o projeto `.pkt` pelo menu do Packet Tracer.

## 3 Configurar os computadores

Em cada PC, abra **Desktop > IP Configuration > IPv6 Configuration > Static**. Preencha:

| PC | IPv6 Address | Prefix Length | Default Gateway |
| --- | --- | --- | --- |
| PC0 | 2001:DB8:1::2 | 64 | 2001:DB8:1::1 |
| PC1 | 2001:DB8:1::3 | 64 | 2001:DB8:1::1 |
| PC2 | 2001:DB8:2::2 | 64 | 2001:DB8:2::1 |
| PC3 | 2001:DB8:2::3 | 64 | 2001:DB8:2::1 |

Mantenha o endereço link-local gerado automaticamente. O DNS pode ficar vazio, pois os testes usam endereços IPv6 diretamente. Não é necessário configurar IPv4 para esta prática.

## 4 Repetir os testes

Em **Desktop > Command Prompt**, execute os dois comandos de cada computador:

| Origem | Mesmo prefixo | Outro prefixo |
| --- | --- | --- |
| PC0 | `ping 2001:DB8:1::3` | `ping 2001:DB8:2::2` |
| PC1 | `ping 2001:DB8:1::2` | `ping 2001:DB8:2::3` |
| PC2 | `ping 2001:DB8:2::3` | `ping 2001:DB8:1::2` |
| PC3 | `ping 2001:DB8:2::2` | `ping 2001:DB8:1::3` |

Nas capturas desta prática, cada teste apresentou quatro linhas `Reply from`, quatro pacotes recebidos e 0% de perda. Em uma nova execução, registre o resultado observado. Uma falha inicial isolada não confirma a causa do problema; repita e confira as configurações se necessário.

## 5 Guardar as evidências

Salve uma captura da topologia, uma da verificação das interfaces e uma dos dois testes em cada PC. Os nomes e os resultados estão no [índice de evidências](evidencias.md). Salve a simulação em **File > Save As** com o nome `rede-ipv6.pkt`.

## Se a comunicação falhar

1. Compare endereço, prefixo /64 e gateway com a tabela.
2. Verifique cabos e portas, além do estado `up/up` das interfaces do roteador.
3. Confira a configuração `ipv6 unicast-routing`.
4. Compare o resultado para o vizinho local com o resultado para a outra rede.
5. Registre o erro e valide novamente após a correção.

Essa sequência é uma orientação de diagnóstico. Não foi realizado um experimento de falha proposital nesta prática.
