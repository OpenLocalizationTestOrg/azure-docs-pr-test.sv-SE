## <a name="what-are-service-bus-queues"></a>Vad är Service Bus-köer?
Service Bus-köer stöder kommunikation med hjälp av en **asynkron meddelandetjänst**. När du använder köer kommunicerar komponenter i ett distribuerat program inte direkt med varandra. De utbyter istället meddelanden via en kö som fungerar som en mellanhand (asynkron meddelandekö). En meddelandeproducent (avsändare) lämnar en meddelandekö toohello och fortsätter sedan dess bearbetning. Asynkront, en meddelandekonsument (mottagare) hämtar hello-meddelande från hello kön och bearbetar det. hello producenten inte har toowait på ett svar från konsumenten hello i ordning toocontinue tooprocess och skicka fler meddelanden. Köer erbjuder **First In, First Out (FIFO)** meddelande leverans tooone eller flera konkurrerande konsumenter. Meddelanden är det innebär vanligtvis emot och bearbetas av hello mottagarna i hello ordning som de har lagts till toohello kön, och varje meddelande tas emot och bearbetas av bara en meddelandekonsument.

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Service Bus-köer är en mångsidig teknologi som kan användas för en mängd olika scenarier:

* Kommunikation mellan webb- och arbetsroller i ett Azure-program med flera nivåer.
* Kommunikation mellan lokala appar och appar med Azure som värd, i en hybridlösning.
* Kommunikation mellan komponenter i ett distribuerat program som körs lokalt i olika organisationer eller avdelningar i en organisation.

Med hjälp av köer kan du tooscale dina program enklare och aktivera flera återhämtning tooyour arkitektur.


