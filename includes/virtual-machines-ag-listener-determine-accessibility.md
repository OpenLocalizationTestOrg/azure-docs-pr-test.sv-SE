Det är viktigt toorealize att det finns två sätt tooconfigure en tillgänglighetsgruppslyssnare i Azure. hello sätt skiljer sig åt i hello typ av Azure belastningsutjämnare som du använder när du skapar hello-lyssnaren. hello i den följande tabellen beskrivs hello skillnader:

| Typer av belastningsutjämnare | Implementering | Använd när: |
| --- | --- | --- |
| **Externa** |Använder hello *offentliga virtuella IP-adressen* för hello Molntjänsten som är värd för hello virtuella maskiner (VMs). |Du måste tooaccess hello lyssnare från utanför hello virtuellt nätverk, inklusive från hello Internet. |
| **Internt** |Använder en *intern belastningsutjämnare* med en privat adress för hello-lyssnaren. |Du kan komma åt hello-lyssnaren endast från hello samma virtuella nätverk. Den här access innehåller plats-till-plats VPN för hybridscenarion. |

> [!IMPORTANT]
> För en lyssnare som använder hello molnbaserad tjänst offentliga VIP (extern belastningsutjämnare), så länge som hello klienten, lyssnare och databaser finns i Hej samma Azure-region, kommer du inte betalar utgång avgifter. Annars returnerade data via hello lyssnare anses utgång och den debiteras med normal dataöverföring priser. 
> 
> 

En ILB kan konfigureras bara på virtuella nätverk med ett regionalt omfång. Befintliga virtuella nätverk som har konfigurerats för en tillhörighetsgrupp kan inte använda en ILB. Mer information finns i [översikt över intern belastningsutjämnare](../articles/load-balancer/load-balancer-internal-overview.md).

