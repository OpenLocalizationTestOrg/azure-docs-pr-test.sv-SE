En Azure belastningsutjämnare är en Layer 4-belastningsutjämnare (TCP, UDP). hello belastningsutjämnare ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri tjänstinstanser i molntjänster eller virtuella datorer i en belastningen belastningsutjämnaren. Azure belastningsutjämnare kan även presentera dessa tjänster på flera portar, flera IP-adresser eller både och.

Du kan konfigurera en belastningsutjämnare för att:

* Läsa belastningsutjämna inkommande Internet-trafik toovirtual datorer (VM). Vi refererar tooa belastningsutjämnare i det här scenariot som en [Internetriktade belastningsutjämnare](../articles/load-balancer/load-balancer-internet-overview.md).
* Belastningsutjämna trafik mellan virtuella datorer i ett virtuellt nätverk (VNet), mellan virtuella datorer i molntjänster eller mellan lokala datorer och virtuella datorer i ett virtuellt nätverk mellan olika platser. Vi refererar tooa belastningsutjämnare i det här scenariot som en [intern belastningsutjämnare (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Vidarebefordra externa trafiken tooa specifika VM-instans.
