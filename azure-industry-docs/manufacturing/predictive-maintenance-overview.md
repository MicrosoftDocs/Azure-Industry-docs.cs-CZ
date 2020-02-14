---
title: Úvod do prediktivní údržby v výrobě
author: scseely
ms.author: scseely
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Přehled vývoje prediktivní údržby (PdM) pro výrobní zákazníky v Azure.
ms.openlocfilehash: 14ef249685c9ee90846dbfeba993742f7502037b
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77053823"
---
# <a name="predictive-maintenance-in-manufacturing-overview"></a>Prediktivní údržba v přehledu výroby

## <a name="introduction"></a>Úvod

Prediktivní Údržba (PdM) předpokládá, že údržba se musí vyhnout nákladům spojeným s neplánovanými výpadky. Když se připojíte k zařízením a sledujete data, která zařízení vytváří, můžete identifikovat vzory, které vedou k potenciálním problémům nebo selháním. Tyto přehledy pak můžete použít k vyřešení problémů dřív, než k nim dojde. Tato možnost předpovídá, kdy zařízení nebo prostředky potřebují údržbu, vám umožní optimalizovat životnost zařízení a minimalizovat prostoje.

PdM extrahuje přehledy z dat vytvořených v zařízeních v dílenském a působí na tyto přehledy. Nápad PdM se vrátí k brzkému 1990&#39;s a rozšiřuje pravidelně naplánovanou a preventivní údržbu. V brzkém případě nedostatečná dostupnost senzorů generujících data a také nedostatek výpočetních prostředků pro shromažďování a analýzu dat, která obtížně implementují PdMy. V současné době se pokrok v Internet věcí (IoT), Cloud Computing, analýza dat a Machine Learning (ML) povolují prediktivní údržba.

PdM vyžaduje, aby zařízení poskytovalo data od senzorů, která sledují zařízení, i další provozní data. Systém PdM analyzuje data a ukládá výsledky. Lidé působí na základě analýzy.

Po zavedení nějakého pozadí v tomto článku probereme, jak implementovat různé části řešení PdM s využitím kombinace místních dat, služby Azure Machine Learning a využití modelů strojového učení. PdM spoléhá na data při rozhodování, takže začneme tím, že si vyhledáme shromažďování dat. Data musí být shromažďována a pak použita k vyhodnocení toho, co se děje, a také k sestavování lepších prediktivních modelů v budoucnu. Nakonec vysvětlete, co vypadá jako řešení pro analýzu, včetně vizualizace výsledků analýz v nástroji pro vytváření sestav, jako je [Power BI](https://docs.microsoft.com/power-bi/).

## <a name="maintenance-strategies"></a>Strategie údržby

Přes historii výrobního odvětví se vyznamenalo několik strategií údržby. Reaktivní údržba opraví problémy po jejich výskytu. Preventivní údržba opravuje problémy dřív, než k nim dojde, podle plánu údržby na základě předchozího selhání. PdM také řeší problémy dřív, než k nim dojde, ale zvažuje skutečné využití tohoto vybavení místo toho, aby se naplánovala pevná. Pro tři, PdM bylo nejobtížnější dosáhnout z důvodu historických omezení pro shromažďování, zpracování a vizualizaci dat. Pojďme&#39;se podívat na každou možnost v trochu více detailů.

Reaktivní údržba zahrnuje &quot;, pokud není&#39;poškozená,&#39;neopraví&quot;ou duševníost. Aktivuje Asset pouze v případě, že dojde k chybě. Například motor z 5. CNC strojového centra pro zpracování se bude obsluhovat pouze v případě, že přestane fungovat. Reaktivní údržba maximalizuje životnost součásti, která nakonec neproběhne úspěšně. Reaktivní údržba také zavádí neznámou dobu výpadků, neočekávané škody způsobené součástmi poškozenými neúspěšnými součástmi a dalšími problémy.

Preventivní údržba vyžaduje, abyste službu Asset v předem určených intervalech. Interval je typicky založený na historii zkušeností selhání pro daný prostředek. Tyto intervaly vycházejí z historických výsledků, simulací, statistického modelování atd. Výhodou této strategie je, že zvyšuje dobu provozu, má za následek méně chyb a může být plánovaná údržba. Nevýhodou v mnoha případech je nahrazená součást assetu, která mohla zůstat po nějakou dobu. Výsledkem je vyšší údržba a odpad. Na překlopení můžou některé části ještě selhat před naplánovanou údržbou. Pravděpodobně víte o dobrém preventivní údržbě: po každé nastavené hodinové operaci (nebo některé jiné metriky) ukončete používání počítače a zkontrolujte počítač. Nahradí všechny části, které jsou v důsledku nahrazení.

Prediktivní údržba monitoruje využití prostředků pomocí modelů k předpovídání selhání komponenty v případě, že se prostředek bude pravděpodobná. Tato součást pak má naplánovanou údržbu pro &quot;&quot;údržby za běhu. PdM vylepšuje předchozí strategie tím, že maximining dobu provozu i životnost prostředků. Vzhledem k tomu, že se zařízení zařadí blízko maximální životnosti komponenty, strávíte méně peněz při nahrazování pracovních částí. Nevýhodou je to, že povaha PdMí za běhu je obtížnější, protože vyžaduje větší rychlost a flexibilitu organizace služeb. Zpátky na motor z 5 CNCch strojového zpracování, naplánujete jeho údržbu &quot;řádným&quot; (tj. plánovaným způsobem, aniž by došlo k narušení výroby), pokud prediktivní model předpovídá, že motor má za následek 75% pravděpodobnost selhání během příštích 24 hodin (na základě informací pocházejících ze senzorů v počítači).

 ![](./assets/pdm-assets/maintenancestrategies.png)


## <a name="different-ways-pdm-can-be-offered"></a>Různé způsoby, jak se PdM dá nabídnout

Řešení PdM může použít přímo výrobce a monitorovat data přicházející z vlastních výrobních operací. Existují i jiné způsoby, které znamenají nové obchodní příležitosti a toky příjmů pro jiné organizace. Příklad:

- Výrobce přidá hodnotu pro své zákazníky tím, že nabízí služby prediktivní údržby pro své produkty.
- Výrobce nabízí své produkty v rámci modelu produktu jako služby – kde se zákazníci &quot;odběrem&quot; k produktu, místo abyste si ho koupili přímo. V rámci tohoto modelu chce výrobce maximalizovat dobu provozu produktu; Pokud produkt nepracuje, produkt nebude generovat výnosy.
- Společnost poskytuje prediktivní údržbu produktů a služeb pro produkty, které vyrábí jeden nebo více výrobců.

## <a name="building-a-predictive-maintenance-solution"></a>Vytvoření řešení prediktivní údržby

Pokud chcete vytvořit řešení PdM, začneme s daty; v ideálním případě data, která zobrazují normální provoz, a data, která ukazují, jak se toto zařízení vyhledalo jako předtím, během a po selhání. Data pocházejí ze senzorů, poznámek udržovaných provozovateli, údajů o životním prostředí, specifikací počítačů atd. Systémy záznamů mohou zahrnovat historians, systémy spouštění výroby, ERP atd. Data jsou dostupná pro analýzy mnoha různými způsoby. [Vědecký proces týmového zpracování dat](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/), který je znázorněný níže a přizpůsobený pro výrobu, má vynikající úkol, který vysvětluje různé obavy, které má při sestavování a provádění modelů strojového učení.

 ![](./assets/pdm-assets/DataScienceDiagram.png)


První úkol bude identifikovat typy selhání, které chcete předpovědět. Díky tomu je pak možné identifikovat zdroje dat, které mají zajímavé údaje kolem tohoto typu selhání. Kanál získá data do systému z vašeho prostředí. Odborníci na data budou k přípravě dat používat své oblíbené nástroje Machine Learning. V tuto chvíli jsou připravené k vytváření a výukové modely, které mohou identifikovat nejrůznější typy problémů. Mezi modely odpovídají dotazy:

- _V případě assetu je to pravděpodobnost, že k selhání dojde během příštích X hodin?_ Odpověď: 0-100%
- _Jaká je zbývající životnost assetu?_ Odpověď: X hod.
- _Chová se tento prostředek neobvyklým způsobem?_ Odpověď: Ano nebo ne
- _Který prostředek vyžaduje obsluhu co nejnaléhavější?_ Odpověď: Asset X

Po rozvinutí se modely můžou zasedat na samotném zařízení pro vlastní diagnostiku, a to na hraničním zařízení, které je v produkčním prostředí, nebo v Azure. Také budete nadále posílat data z primárních zdrojů do centrálního úložiště, abyste mohli pokračovat v sestavování a údržbě řešení PdM.

Výkonná technologie Azure umožňuje vyškolit a testovat modely podle vaší technologie, kterou si vyberete. Můžete využít GPU, FPGA, CPU, velké paměťové počítače a tak dále. Jednou z skvělých věcí k Azure je, že platforma plně využívá Open Source nástroje používané odborníky přes data (například R a Python). Po dokončení analýzy se můžou výsledky zobrazit v dalších aspektech řídicího panelu nebo v jiných sestavách. Tyto sestavy se můžou zobrazit ve vlastních nástrojích nebo můžete využít výhod nástrojů pro vytváření sestav, jako je [Power BI](https://docs.microsoft.com/power-bi/) nebo [Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/).

Bez ohledu na to, co vaše PdM potřebuje, Azure nabízí nástroje, škálu, možnosti, které potřebujete pro sestavení plného řešení.

## <a name="getting-started"></a>Začínáme

Množství zařízení, které bylo nalezeno v dílenském výrobě, shromažďuje data a poskytuje mechanismy pro shromažďování dat ze zařízení. Zahajte shromažďování těchto dat co nejrychleji. Když dojde k selhání, mají odborníci na data analyzovat data k vytvoření modelů, které mohou detekovat budoucí selhání. Jako znalostní báze o detekci selhání se přesunete do prediktivního režimu, ve kterém během plánovaného výpadku opravíte součásti. [Průvodce modelováním prediktivní údržby](https://gallery.azure.ai/Collection/Predictive-Maintenance-Modelling-Guide-1) poskytuje plný návod, jak sestavovat Machine Learningé části řešení.

Pokud chcete zobrazit ukázkové řešení, přečtěte si řešení, průvodce a PlayBook pro [PDM ve službě Aerospace](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace). Pokud potřebujete rozpracovat na sestavách vytváření modelů, doporučujeme navštívit si [školu AI](https://aischool.microsoft.com/). [Úvod do Machine Learning s kurzem Azure ml](https://aischool.microsoft.com/learning-paths/4ZYo4wHJVCsUSAKa2EoAk8) vám pomůže seznámit se s našimi nástroji.

## <a name="technologies-presented"></a>Prezentované technologie

[Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) ukládá stovky až miliard objektů na horké, studené nebo archivní úrovni v závislosti na tom, jak často jsou data potřeba.

[Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) je databáze pro extrémně nízkou latenci a rozsáhle škálovatelné aplikace kdekoli na světě, s nativní podporou pro NoSQL.

[Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/) zahrnuje všechny funkce potřebné k tomu, aby vývojáři, odborníci na data a analytici mohli snadno ukládat data libovolné velikosti, tvaru a rychlosti a provádět všechny typy zpracování a analýzy napříč platformami a jazyky.

[Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) je služba ingestování telemetrie Hyper-Scale, která shromažďuje, transformuje a ukládá miliony událostí. Jako platforma pro distribuované streamování nabízí nízkou latenci a konfigurovatelné časové uchovávání, které umožňuje příchozí přenos dat do cloudu a načítání dat z více aplikací pomocí sémantiky publikování a odběru.

[Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) je služba Internet věcí (IoT), která je postavená na IoT Hub. Tato služba je určená pro zákazníky, kteří chtějí analyzovat data na zařízeních, jinými slovy &quot;na hraničním&quot;, a ne v cloudu. Díky přesunu částí úlohy do hraničních zařízení můžou vaše zařízení šetřit čas strávený odesíláním zpráv do cloudu a rychleji reagovat na změny stavu.

[Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) je plně spravovaná služba, která umožňuje spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení IoT a back-endem řešení.

[Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/) umožňuje počítačům učit se z dat a prostředí a působit bez explicitního programu. Zákazníci můžou vytvářet aplikace umělých inteligentních funkcí (AI), které inteligentně vydávaly, zpracovávají a chovají informace – rozšiřují lidské možnosti, zvyšují rychlost a efektivitu a pomáhají organizacím dosáhnout víc.

[Azure Service Bus](https://docs.microsoft.com/azure/service-bus/) je zprostředkovatelských komunikační mechanismus. Základní komponenty infrastruktury zasílání zpráv Service Bus jsou fronty, témata a odběry.

[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) je inteligentní, plně spravovaná relační cloudová databázová služba, která poskytuje nejširší kompatibilitu s modulem SQL Server, takže můžete migrovat databáze SQL Server bez změny vašich aplikací.

[Power BI](https://docs.microsoft.com/power-bi/) je sada nástrojů pro obchodní analýzy, které poskytují přehledy napříč vaší organizací. Připojte se ke stovkám zdrojů dat, Zjednodušte přípravu dat a proveďte analýzu ad hoc.

[Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/) je plně spravovaná služba pro analýzy, ukládání a vizualizace, která slouží ke správě dat časových řad ve službě IoT-Scale v cloudu.

## <a name="conclusion"></a>Závěr

PdM vyžaduje, aby počítače měly určitou úroveň instrumentace a připojení, které nám umožňují sestavovat systémy, které mohou předpovídat problémy a umožňují nám reagovat, než dojde k selhání. PdM rozšiřuje preventivní plány údržby tím, že identifikuje konkrétní součásti pro kontrolu a opravu nebo nahrazení. Existuje mnoho prostředků, které vám pomůžou začít. Infrastruktura&#39;Microsoftu vám může pomáhat s vytvářením řešení, která běží na zařízení, na hraničních zařízeních a v cloudu. 

Začněte tím, že zadáte horních 1-3 chyb, které chcete zabránit, a zahájíte proces zjišťování s těmito položkami. Pak určete, kde se data nacházejí, která pomáhají identifikovat selhání. Zkombinujte tato data s dovednostmi, které jste si vybrali, od [úvodu do Machine Learning s](https://aischool.microsoft.com/learning-paths/4ZYo4wHJVCsUSAKa2EoAk8) využitím kurzu Azure ml k sestavování modelů PDM.