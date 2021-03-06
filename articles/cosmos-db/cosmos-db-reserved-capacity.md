---
title: De kosten van Azure Cosmos DB resources met gereserveerde capaciteit optimaliseren
description: Meer informatie over het kopen van Azure Cosmos DB gereserveerde capaciteit om uw reken kosten op te slaan.
author: bandersmsft
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: banders
ms.reviewer: sngun
ms.openlocfilehash: 0ee43fe0996c05f4e59f6107ba52fac19b83cdef
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756959"
---
# <a name="optimize-cost-with-reserved-capacity-in-azure-cosmos-db"></a>Kosten optimaliseren met gereserveerde capaciteit in Azure Cosmos DB

Azure Cosmos DB gereserveerde capaciteit helpt u geld te besparen door over te slaan op een reserve ring voor Azure Cosmos DB resources voor een jaar of drie jaar. Met Azure Cosmos DB gereserveerde capaciteit kunt u een korting krijgen op de door Voer die is ingericht voor Cosmos DB resources. Voor beelden van resources zijn data bases en containers (tabellen, verzamelingen en grafieken).

Azure Cosmos DB gereserveerde capaciteit kan uw Cosmos DB kosten aanzienlijk verminderen&mdash;tot 65 procent op reguliere prijzen met één jaar of drie jaar vooraf. Gereserveerde capaciteit biedt een facturerings korting en heeft geen invloed op de runtime status van uw Azure Cosmos DB-resources.

Azure Cosmos DB gereserveerde capaciteit heeft betrekking op de door Voer ingericht voor uw resources. De opslag- en netwerkkosten vallen hier niet onder. Zodra u een reserve ring koopt, worden de doorvoer kosten die overeenkomen met de reserverings kenmerken niet meer in rekening gebracht tegen de betalen naar gebruik-tarieven. Zie het artikel over [Azure-reserve ringen](../billing/billing-save-compute-costs-reservations.md) voor meer informatie over reserve ringen.

U kunt Azure Cosmos DB gereserveerde capaciteit kopen via de [Azure Portal](https://portal.azure.com). Betaal [vooraf of per maand](../billing/billing-monthly-payments-reservations.md) voor de reservering. Gereserveerde capaciteit kopen:

* U moet de rol van eigenaar zijn voor minstens één bedrijf of een afzonderlijk abonnement met betalen per gebruik-tarieven.  
* Voor Enterprise Agreements moet de optie **Gereserveerde instanties toevoegen** zijn ingeschakeld in de [EA Portal](https://ea.azure.com). Als deze instelling is uitgeschakeld, moet u een EA-beheerder zijn voor het abonnement.
* Voor het programma Cloud Solution Provider (CSP) kunnen alleen beheer agenten of verkoop medewerkers Azure Cosmos DB gereserveerde capaciteit kopen.

## <a name="determine-the-required-throughput-before-purchase"></a>De vereiste door Voer voor aankoop bepalen

De grootte van de reserve ring moet worden gebaseerd op de totale hoeveelheid door Voer die wordt gebruikt door de bestaande of binnenkort gedistribueerde Azure Cosmos DB resources. U kunt de vereiste door Voer op de volgende manieren bepalen:

* De historische gegevens ophalen voor de totale ingerichte door voer over uw Azure Cosmos DB accounts, data bases en verzamelingen in alle regio's. U kunt bijvoorbeeld de dagelijkse gemiddelde ingerichte door Voer evalueren door uw dagelijkse gebruiks overzicht te downloaden van `https://account.azure.com`.

* Als u een klant van Enterprise Agreement (EA) bent, kunt u uw gebruiks bestand downloaden om de Azure Cosmos DB doorvoer gegevens op te halen. Raadpleeg de waarde **Service type** in het gedeelte **aanvullende informatie** van het gebruiks bestand.

* U kunt de gemiddelde door Voer berekenen voor alle werk belastingen op uw Azure Cosmos DB-accounts die u voor de volgende of drie jaar verwacht uit te voeren. U kunt dat aantal vervolgens gebruiken voor de reserve ring.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Azure Cosmos DB gereserveerde capaciteit kopen

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).  

2. Selecteer **alle services** > **reserve ringen** > **toevoegen**.  

3. Kies in het deel venster **aankoop reserveringen** **Azure Cosmos DB** om een nieuwe reserve ring te kopen.  

4. Vul de vereiste velden in zoals beschreven in de volgende tabel:

   ![Het formulier voor de gereserveerde capaciteit invullen](./media/cosmos-db-reserved-capacity/fill-reserved-capacity-form.png)

   |Veld  |Beschrijving  |
   |---------|---------|
   |Scope   |   Optie die bepaalt hoeveel abonnementen het facturerings voordeel kunnen gebruiken dat is gekoppeld aan de reserve ring. Het bepaalt ook hoe de reserve ring wordt toegepast op specifieke abonnementen. <br/><br/>  Als u **gedeeld**selecteert, wordt de reserverings korting toegepast op Azure Cosmos DB exemplaren die worden uitgevoerd in een abonnement binnen uw facturerings context. De facturerings context is gebaseerd op de manier waarop u zich hebt geregistreerd voor Azure. Voor zakelijke klanten is het gedeelde bereik de inschrijving en omvat alle abonnementen binnen de inschrijving. Voor betalen naar gebruik-klanten is de gedeelde Scope alle afzonderlijke abonnementen met betalen per gebruik-tarieven die door de account beheerder zijn gemaakt.  <br/><br/>  Als u **één abonnement**selecteert, wordt de reserverings korting toegepast op Azure Cosmos DB exemplaren in het geselecteerde abonnement. <br/><br/> Als u **één resource groep**selecteert, wordt de reserverings korting toegepast op Azure Cosmos DB exemplaren in het geselecteerde abonnement en de geselecteerde resource groep in dat abonnement. <br/><br/> U kunt het reserverings bereik wijzigen nadat u de gereserveerde capaciteit hebt gekocht.  |
   |Abonnement  |   Het abonnement dat wordt gebruikt om te betalen voor de Azure Cosmos DB gereserveerde capaciteit. De betalings wijze voor het geselecteerde abonnement wordt gebruikt bij het opladen van de kosten. Het abonnement moet een van de volgende typen zijn: <br/><br/>  Enterprise Agreement (nummers van aanbiedingen: MS-AZR-0017P of MS-AZR-0148P): voor een Enter prise-abonnement worden de kosten in rekening gebracht op basis van de monetaire toezeg ging van de inschrijving of worden aangerekend als overschrijding. <br/><br/> Individueel abonnement met betalen per gebruik-tarieven (aanbiedings nummers: MS-AZR-0003P of MS-AZR-0023P): voor een afzonderlijk abonnement met betalen per gebruik-tarieven, worden de kosten in rekening gebracht op de credit card-of factuur betalings methode voor het abonnement.    |
   | Resourcegroep | De resource groep waarop de gereserveerde capaciteits korting wordt toegepast. |
   |Termijn  |   Eén jaar of drie jaar.   |
   |Type door Voer   |  De door Voer is ingericht als aanvraag eenheden. U kunt een reserve ring voor de ingerichte door voer kopen voor zowel de installatie van één regio als voor meerdere schrijf bewerkingen in regio's. Het doorvoer type heeft twee waarden waaruit u kunt kiezen: 100 RU/s per uur en 100 multi-master RU/s per uur.|
   | Gereserveerde capaciteits eenheden| De hoeveelheid door Voer die u wilt reserveren. U kunt deze waarde berekenen door de door voer te bepalen die nodig is voor al uw Cosmos DB resources (bijvoorbeeld data bases of containers) per regio. Vervolgens vermenigvuldigt u deze met het aantal regio's dat u koppelt aan uw Cosmos-data base. Bijvoorbeeld: als u vijf regio's met 1.000.000 RU per seconde in elke regio hebt, selecteert u 5.000.000 RU per seconde voor de aanschaf van de reserve ring capaciteit. |


5. Nadat u het formulier hebt ingevuld, wordt de prijs berekend die is vereist om de gereserveerde capaciteit aan te schaffen. De uitvoer toont ook het percentage van de korting die u krijgt met de gekozen opties. Klik vervolgens op **selecteren**

6. Controleer in het deel venster **aankoop reserveringen** de korting en de prijs van de reserve ring. Deze reserverings prijs is van toepassing op Azure Cosmos DB resources waarbij de door Voer is ingericht in alle regio's.  

   ![Samen vatting van gereserveerde capaciteit](./media/cosmos-db-reserved-capacity/reserved-capacity-summary.png)

7. Selecteer **beoordelen + kopen** en **Nu kopen**. U ziet de volgende pagina wanneer de aankoop slaagt:

Nadat u een reserve ring hebt gekocht, wordt deze onmiddellijk toegepast op bestaande Azure Cosmos DB resources die overeenkomen met de voor waarden van de reserve ring. Als u geen bestaande Azure Cosmos DB resources hebt, wordt de reserve ring toegepast wanneer u een nieuwe Cosmos DB instantie implementeert die overeenkomt met de voor waarden van de reserve ring. In beide gevallen wordt de periode van de reserve ring direct na een succes volle aankoop gestart.

Wanneer de reserve ring verloopt, blijven uw Azure Cosmos DB-exemplaren actief en worden de normale tarieven voor betalen naar gebruik gefactureerd.

## <a name="cancel-exchange-or-refund-reservations"></a>Annulering, omwisseling of terugbetaling van reserveringen

Zie [begrijpen hoe de reserverings korting wordt toegepast op Azure Cosmos DB](../billing/billing-understand-cosmosdb-reservation-charges.md)voor hulp bij het identificeren van de juiste gereserveerde capaciteit.

Annulering, ruiling of terugbetaling van reserveringen is mogelijk met bepaalde beperkingen. Zie [Selfservice voor ruiling en terugbetaling van Azure-reserveringen](../billing/billing-azure-reservations-self-service-exchange-and-refund.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

De reserverings korting wordt automatisch toegepast op de Azure Cosmos DB resources die overeenkomen met het reserverings bereik en de kenmerken. U kunt het bereik van de reserve ring bijwerken via de Azure Portal, Power shell, Azure CLI of de API.

*  Zie [inzicht in de korting voor Azure-reserve ringen](../billing/billing-understand-cosmosdb-reservation-charges.md)voor meer informatie over hoe gereserveerde capaciteits kortingen worden toegepast op Azure Cosmos db.

* Raadpleeg de volgende artikelen voor meer informatie over Azure-reserveringen:

   * [Wat zijn Azure-reserve ringen?](../billing/billing-save-compute-costs-reservations.md)  
   * [Azure-reserveringen beheren](../billing/billing-manage-reserved-vm-instance.md)  
   * [Inzicht in het gebruik van reserveringen voor Enterprise-inschrijvingen](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [Inzicht in het gebruik van reserveringen voor uw abonnement met betalen per gebruik](../billing/billing-understand-reserved-instance-usage.md)
   * [Azure-reserve ringen in het Partner Center CSP-programma](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>Hebt u hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [kunt u een ondersteuningsaanvraag maken](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
