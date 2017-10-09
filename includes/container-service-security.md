# <a name="securing-docker-containers-in-azure-container-service"></a>Skydda Docker-behållare i Azure Container Service

Den här artikeln innehåller överväganden och rekommendationer som hjälper dig att skydda Docker-behållare som distribueras i Azure Container Service. Många av dessa överväganden gäller vanligtvis tooDocker behållare som distribueras i Azure eller andra miljöer. 

## <a name="image-security"></a>Avbildningssäkerhet

Behållare skapas från avbildningar som lagras i en eller flera databaser. Dessa databaser kan höra toopublic eller privata behållare register. Ett exempel på ett offentligt register är [Docker Hub](https://hub.docker.com/). Ett exempel på ett privat register är hello [Docker betrodda registret](https://docs.docker.com/datacenter/dtr/2.0/), som kan installeras lokalt eller i ett virtuellt privat moln. Det finns också molnbaserade tjänster för privata registerbehållare, t.ex. [Azure Container Registry](../articles/container-registry/container-registry-intro.md).

### <a name="public-and-private-images"></a>Offentliga och privata avbildningar
Normalt, som med andra offentligt publicerade programvarupaket, är säkerheten i en offentligt tillgänglig behållaravbildning inte garanterad. Behållaravbildningar består av flera programvarulager och varje programvarulager kan medföra säkerhetsrisker. Det är viktiga toounderstand hello ursprung hello behållaren image, inklusive hello ägare till hello bild (toodetermine om det är en pålitlig källa eller inte), hello programlager består av och hello programvaruversioner. 

Till exempel om du går toohello officiella [nginx databasen](https://hub.docker.com/_/nginx/) i Docker hubb och navigera toohello **taggar** fliken färgkodade säkerhetsproblem i varje bild visas. Varje färg visar hello sårbarhet ett Programsteg till hello bild. Mer information om hur du granskar säkerhetsrisker i Docker Hub finns i [Understanding official repos on Docker Hub](https://blog.docker.com/2015/06/understanding-official-repos-docker-hub/) (Så här fungerar officiella databaser i Docker Hub).

![Nginx-avbildningar i Docker Hub](./media/container-service-security/docker-hub-nginx.png)

Företag djupt hand om säkerhet och tooprotect sig från säkerhet attacker bör lagra och hämta bilder från ett privat register, till exempel Azure Container registret eller Docker betrodda registret. I tillägg tooproviding ett hanterade privata register, stöd för Azure-behållare registret [service principal-baserad autentisering](../articles/container-registry/container-registry-authentication.md) via Azure Active Directory för grundläggande autentisering, inklusive rollbaserad åtkomst för skrivskyddad, Skriv- och ägare.

### <a name="image-security-scanning"></a>Säkerhetsgenomsökning för avbildningar

Även om du använder ett privat register är en bra idé toouse avbildning genomsökning lösningar för ytterligare säkerhet verifiering. Varje programvara lager i en behållare bild är potentiellt känsliga toovulnerabilities oberoende av andra lager i hello behållaren bild. Eftersom allt företag börja distribuera sina produktionsarbetsbelastningar baserat på behållaren tekniker, blir skanningen viktigt tooensure förebyggande av säkerhetshot mot deras organisationer. 

Övervakning och genomsökning av lösningar som [Twistlock](https://www.twistlock.com/2016/11/07/twistlock-supports-azure-container-registry) och [turkos säkerhet](http://blog.aquasec.com/image-vulnerability-scanning-in-azure-container-registry), bland annat kan använda tooscan behållare bilder i ett privat register och identifiera potentiella säkerhetsrisker. Det är viktigt att ange toounderstand hello djup att genomsöka de hello olika lösningarna. Till exempel kanske vissa lösningar bara verifierar avbildningslager mot kända säkerhetsrisker. Dessa lösningar kanske inte kan tooverify avbildningen layer programvara inbyggda via vissa package manager-program. Andra lösningar har djupare genomsökningsintegration och kan hitta sårbarheter i all paketerad programvara.

### <a name="production-deployment-rules-and-audit"></a>Regler för och granskning av distributioner i produktionsmiljön
När ett program har distribuerats i produktion, är det grundläggande tooset några regler tooensure som bilder används i produktionsmiljöer är säker och innehåller ingen säkerhetsproblem.

* Som regel ska bilder med säkerhetsrisker, även mindre inte tillåtas toorun i en produktionsmiljö. Dessutom bör alla bilder som har distribuerats i produktionsmiljön helst sparas i ett privat registret tillgänglig tooa Välj få. Det är också viktigt tookeep hello antalet produktion bilder små tooensure att de kan hanteras effektivt.

* Eftersom det hårda toopinpoint hello ursprung programvara från en avbildning av offentligt tillgängliga behållare, är det en bra idé toobuild bilder från källan tooensure kunskap om hello ursprung hello lager. När en säkerhetsrisk hämtar i en egen inbyggd behållaren bild kan kunder hitta en snabbare sökvägen tooa lösning. Med en offentlig avbildning kunder behöver toofind hello roten av en offentlig avbildningen toofix den eller skaffa en annan säker bild från hello utgivare.

* En noggrant skannad bild som distribuerats i produktionsmiljön garanteras inte toobe in toodate under hello programmet hello livstid. Säkerhetsproblem kan rapporteras för lager i hello avbildning som inte tidigare känd eller införts efter hello Produktionsdistribution. Regelbunden granskning av avbildningar som distribuerats i produktionsmiljön är nödvändiga tooidentify bilder som har upphört att gälla eller har inte uppdaterats om en stund. En kan använda blå-grön distribution metoder och löpande uppgradering mekanismer tooupdate behållare bilder utan driftavbrott. Skanningen kan göras med verktyg som beskrivs i föregående avsnitt hello. 

* En kontinuerlig integration (KO) pipeline toobuild bilder och integrerad säkerhet genomsökning hjälper underhålla säker privata register med bilder för säkra behållare. hello säkerställer säkerhetsproblem genomsökning inbyggd hello CI lösning att bilder som klarar alla tester i hello push toohello privata registret från vilken produktion arbetsbelastningar har distribuerats. Ett CI pipeline-fel säkerställer att sårbara bilder inte pushas till hello privata register används för arbetsbelastningen Produktionsdistribution. Dessutom körs säkerhetsgenomsökningar av avbildningarna automatiskt om det finns ett stort antal avbildningar. Att manuellt söka efter säkerhetsrisker i avbildningar kan vara en enormt tidskrävande och felbenägen uppgift.

## <a name="host-level-container-isolation"></a>Behållarisolering på värdnivå
När en kund distribuerar behållarprogram på Azure-resurser distribueras de på en prenumerationsnivå i resursgrupper och är inte konfigurerade för flera klientorganisationer. Det innebär att om en kund delar en prenumeration med andra, det finns inga gränser som kan byggas mellan två distributioner i hello samma prenumeration. Det betyder i sin tur att det inte går att garantera säkerhet på behållarnivå. 

Det är också viktig toounderstand som behållare delar hello kernel och hello resurser av hello-värden (som i Azure Container Service är en Azure-dator i ett kluster). Av den anledningen måste behållare som körs i en produktionsmiljö köras i icke-privilegierat användarläge. Kör en behållare med rotprivilegier kan äventyra hello hela miljön. Med rotnivå åtkomst i en behållare, kan en hackare komma åt toohello fullständiga rotprivilegier på hello värden. Det är dessutom viktigt toorun behållare med skrivskyddad filsystem. Detta förhindrar att någon som har åtkomst toohello komprometteras behållaren toowrite skadliga skript toohello filsystemet och komma åt tooother filer. På samma sätt är det viktigt toolimit hello resurserna (till exempel minne, CPU och nätverkets bandbredd) tooa behållare. Detta förhindrar att hackare att ta upp resurser och olaglig verksamhet, till exempel kreditkort bedrägeri eller bitcoin utvinningsmodellen som kan förhindra att andra behållare körs på hello-värd eller kluster.

## <a name="orchestrator-considerations"></a>Orkestreringsöverväganden

Varje orkestrering som är tillgänglig i Azure Container Service har sina egna säkerhetsöverväganden. Exempelvis bör du begränsa direkt SSH åtkomst tooorchestrator noder i Container Service. I stället bör du använda Användargränssnittet och kommandoradsverktyg för varje orchestrator (exempelvis `kubectl` för Kubernetes) toomanage hello behållaren miljö utan att öppna hello värdar. Mer information finns i [gör en anslutning till tooa Kubernetes, DC/OS eller Docker Swarm-kluster](../articles/container-service/kubernetes/container-service-connect.md).

Ytterligare säkerhet för orchestrator-specifik information finns i hello följande resurser:

* **Kubernetes**: [Security Best Practices for Kubernetes Deployment](http://blog.kubernetes.io/2016/08/security-best-practices-kubernetes-deployment.html) (Säkerhetsrekommendationer för Kubernetes-distributioner)

* **DC/OS**: [Securing Your Cluster](https://dcos.io/docs/1.8/administration/securing-your-cluster/) (Skydda ditt kluster)

* **Docker Swarm**: [Docker Security](https://www.docker.com/docker-security) (Säkerhet i Docker)

## <a name="next-steps"></a>Nästa steg

* Mer information om Docker-arkitektur och en behållare säkerhet finns i [introduktion tooContainer säkerhet](https://www.docker.com/sites/default/files/WP_IntrotoContainerSecurity_08.19.2016.pdf).

* Information om säkerhet i Azure-plattformen finns hello [Azure Security Center](https://www.microsoft.com/en-us/trustcenter/cloudservices/azure).
