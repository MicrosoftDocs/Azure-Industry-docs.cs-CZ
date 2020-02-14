---
title: Přehled migrace řešení elektronického obchodování do Azure
author: scseely
ms.author: scseely
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Tento článek vysvětluje fáze migrace infrastruktury elektronického obchodování z místního prostředí do Azure.
ms.openlocfilehash: e918f1157dc2bc42a6c4d0decfef95a8daa7ccf0
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77054180"
---
# <a name="migrating-your-e-commerce-solution-to-azure-overview"></a>Migrace řešení elektronického obchodování do Azure Overview

## <a name="introduction"></a>Úvod

Přesunutí stávajícího řešení elektronického obchodování do cloudu představuje mnoho výhod pro podnik: umožňuje škálovatelnost, nabízí zákazníkům 24/7 usnadnění přístupu a bude snazší integrovat cloudové služby. Ale nejdřív přesunete řešení elektronického obchodování do cloudu, což je významný úkol s náklady, které musí přirozumět rozhodovacímu pracovníkovi. Tento dokument vysvětluje rozsah migrace Azure s cílem informovat vás o možnostech. První fáze začíná odborníky v oblasti IT, která přesouvá komponenty do cloudu. V Azure popisujeme kroky, které tým elektronického obchodování může využít ke zvýšení návratnosti investic a využití cloudu.

**Na Crossroads**

I když globální transakce elektronického obchodování mají jenom zlomek celkových maloobchodních prodejů, kanál bude dál sledovat stabilní růst po dobu roku. V 2017 se elektronický obchod představoval 10,2% celkových maloobchodních prodejů, a to až z 8,6% v 2016 ( [zdroj](https://www.emarketer.com/Report/Worldwide-Retail-Ecommerce-Sales-eMarketers-Updated-Forecast-New-Mcommerce-Estimates-20162021/2002182)). V případě zralosti elektronického obchodování spolu s nástupemem cloud computingu jsou maloobchodníky na Crossroads.  Existují volby, které je třeba provést. Můžou si svůj obchodní model novými možnostmi, které umožňují vyvíjejí technologii. a můžou naplánovat jejich modernizaci s ohledem na jejich aktuální způsobilost.

**Vylepšení cest zákazníků**

Elektronický obchod, který se primárně zaměřuje na cestu zákazníka, má mnoho různých atributů. Tyto atributy je možné seskupovat do čtyř hlavních oblastí: zjišťování, vyhodnocení, nákup a po zakoupení.

Chování zákazníka je zachyceno jako data. A trychtýřem nákupů je kolekce přípojných bodů pro aplikace používané pro zobrazení dat produktů, transakcí, inventáře, expedice, plnění objednávek, profilu zákazníka, nákupního košíku a doporučení produktů pro několik názvů.

Typickou maloobchodní firmu se spoléhá na velkou kolekci softwarových řešení, která se v různých aplikacích přihlásí do základních aplikací v rozsahu od zákaznických aplikací.  Následující výkres znázorňuje zobrazení funkcí, které jsou k dispozici v typické maloobchodní firmě.

 ![](./assets/migrating-ecommerce-solution-to-azure/ecommerce-system-sketch.png)

Cloud nabízí příležitost posunout způsob, jakým organizace získává, používá a spravuje technologie.  Mezi další výhody patří: snížené náklady na údržbu datových center, lepší spolehlivost a výkon a flexibilita přidávání dalších služeb. V tomto případě se podíváme na cestu, kterou může prodejní firma použít k migraci stávající infrastruktury do Azure. Využijeme také nové prostředí, které využívá postup opětovného hostování, refaktorování a opětovného sestavení. I když mnoho organizací může dodržet tuto cestu řady k modernizaci, ve většině případů mohou organizace v rámci svého počátečního bodu vyřadit do jakékoli fáze.  Organizace se můžou rozhodnout nehostovat své aktuální aplikace v Azure a přejít přímo na refaktoring nebo dokonce znovu sestavit.  Toto rozhodnutí bude pro aplikaci jedinečné a organizace bude co nejlépe vyhovovat potřebám jejich modernizace.

## <a name="rehost"></a>Opětovným hostováním

V této fázi se také říká &quot;výtah a Shift.&quot; Tato fáze zahrnuje migraci fyzických serverů a virtuálních počítačů do cloudu. Pouhým posunem aktuálního serverového prostředí přímo na IaaS se těžit výhody úspory nákladů, zabezpečení a vyšší spolehlivosti. Úspory pocházejí z technik, jako je spouštění úloh na správných velikostech virtuálních počítačů. V současné době funkce místních virtuálních počítačů a fyzických počítačů často překračují každodenní potřeby prodejců. Virtuální počítače musí být schopné zvládnout sezónní obchodní špičky, ke kterým dochází jenom několikrát ročně. Proto platíte za nepoužívané možnosti během období mimo špičku. S Azure si vybíráte virtuální počítač ve správném formátu na základě požadavků pro aktuální obchodní cyklus.

K opětovnému hostování v Azure jsou k dispozici tři fáze:

- **Analýza** : identifikace a inventarizace místních prostředků, jako jsou aplikace, úlohy, sítě a zabezpečení. Na konci této fáze máte kompletní dokumentaci k existujícímu systému.
- **Migrace** : přesun každého subsystému z místního prostředí do Azure. Během této fáze použijete Azure jako rozšíření datového centra a aplikace pokračuje v komunikaci.
- **Optimalizace** : systémy se přesunou do Azure a zajišťují, že se jedná o správnou velikost věcí. Pokud se v prostředí zobrazuje, že k některým virtuálním počítačům je přidělený příliš velký počet prostředků, změňte typ virtuálního počítače na takový, který má vhodnější kombinaci procesoru, paměti a místního úložiště.

### <a name="analyze"></a>Analyzovat

Postupujte takto:

1. Seznam místních serverů a aplikací. Tím se spoléháte na agenta nebo nástroj pro správu, který umožňuje shromažďovat metadata o serverech, aplikacích spuštěných na serverech, aktuálním využití serveru a způsobu, jakým jsou servery a jejich aplikace nakonfigurované. Tento výsledek je sestava všech serverů a aplikací v prostředí.
1. Identifikujte závislosti. Pomocí nástrojů můžete identifikovat, které servery vzájemně komunikují, a aplikace, které vzájemně komunikují. Výsledkem je mapa (neboli mapy) všech aplikací a úloh. Tyto mapy jsou podávány do plánování migrace.
1. Analyzujte konfigurace. Cílem je zjistit, jaké typy virtuálních počítačů budete po spuštění v Azure potřebovat. Výsledkem je sestava pro všechny aplikace, které se můžou přesunout do Azure. Mohou být dále klasifikovány jako mají:
      1. Žádné úpravy
      1. Základní úpravy jako změny názvů
      1. Menší změny, jako je například mírné změny kódu
      1. Nekompatibilní úlohy, které vyžadují další úsilí při přesunu
1. Vytvořte rozpočet. Nyní máte seznam, který obsahuje každý procesor – paměť a tak dále, a požadavky pro každou aplikaci. Tyto úlohy umístěte na správné velikosti virtuálních počítačů. Poplatky za cloudové platformy se účtují na základě využití. Existují nástroje pro mapování vašich potřeb ke správné velikosti virtuálních počítačů Azure. Pokud migrujete virtuální počítače s Windows nebo SQL Server, měli byste se také podívat na [zvýhodněné hybridní využití Azure](https://azure.microsoft.com/pricing/hybrid-benefit/?WT.mc_id=retailecomm-docs-scseely) , který snižuje vaše náklady na Azure.

Microsoft poskytuje několik nástrojů k analýze a katalogu vašich systémů. Pokud spouštíte VMWare, můžete k usnadnění zjišťování a hodnocení použít [Azure Migrate](/azure/migrate/migrate-overview?WT.mc_id=retailecomm-docs-scseely) . Nástroj identifikuje počítače, které se dají přesunout do Azure, doporučuje spustit typ virtuálního počítače a odhaduje náklady na zatížení. Pro prostředí Hyper-V použijte [Plánovač nasazení služby Azure Site Recovery](/azure/site-recovery/hyper-v-deployment-planner-overview?WT.mc_id=retailecomm-docs-scseely). Pro velké migrace, kdy potřebujete přesunout stovky nebo více virtuálních počítačů, můžete chtít pracovat s [partnerem migrace do Azure](https://azure.microsoft.com/migration/partners/?WT.mc_id=retailecomm-docs-scseely). Tito partneři mají zkušenosti a možnosti, jak přesouvat vaše úlohy.

### <a name="migrate"></a>Migrovat

Začněte plánovat, které služby se mají přesunout do cloudu a v jakém pořadí. Vzhledem k tomu, že tato fáze zahrnuje přesun úloh, postupujte podle tohoto pořadí:

1. Vytvořte síť.
2. Zahrňte systém identity (Azure Active Directory).
3. Zřizování částí úložiště v Azure

Během migrace je prostředí Azure rozšíření vaší místní sítě. Logické sítě můžete propojit s [Azure Virtual Network](/azure/virtual-network/virtual-networks-overview?WT.mc_id=retailecomm-docs-scseely). Můžete se rozhodnout použít [Azure ExpressRoute](/azure/expressroute/?WT.mc_id=retailecomm-docs-scseely) k zajištění komunikace mezi vaší sítí a Azure na privátním připojení, které nikdy nekontaktuje Internet. Můžete také použít síť VPN typu Site-to-site, kde se VPN Gateway Azure mluví k vašemu místnímu zařízení VPN se všemi přenosy odeslanými bezpečně pomocí šifrované komunikace mezi Azure a vaší sítí. Publikovali jsme referenční architekturu s podrobnostmi o nastavení [hybridní sítě.](/azure/architecture/reference-architectures/hybrid-networking/vpn?WT.mc_id=retailecomm-docs-scseely)

Po nakonfigurování sítě Naplánujte kontinuitu podnikových aplikací. Doporučujeme použít replikaci v reálném čase k přesunu místních dat do cloudu a zajistit, aby byl Cloud i stávající data stejná. Elektronického obchodování obchody nikdy nezavřou; duplikace nabízí možnost přepnout z místního prostředí do Azure s minimálním dopadem na vaše zákazníky.

Začněte přesouvat data, aplikace a související servery do Azure. Řada společností používá službu [Azure Site Recovery](/azure/site-recovery/site-recovery-overview?WT.mc_id=retailecomm-docs-scseely) k migraci do Azure. Služba cílí na provozní kontinuitu a zotavení po havárii (BCDR). To je ideální pro migraci z místního prostředí do Azure. Váš tým pro implementaci může přečíst podrobnosti o migraci místních virtuálních počítačů a fyzických serverů [do Azure.](/azure/site-recovery/migrate-tutorial-on-premises-azure?WT.mc_id=retailecomm-docs-scseely)

Jakmile se podsystém přesune do Azure, proveďte test a ujistěte se, že vše funguje podle očekávání. Až se všechny problémy zavřou, přesuňte úlohy do Azure.

### <a name="optimize"></a>Optimalizace

V tomto okamžiku budete nadále monitorovat prostředí a změnit základní výpočetní možnosti tak, aby odpovídaly úlohám při změnách prostředí. Kdokoli, kdo sleduje stav prostředí, by měl sledovat, kolik prostředků se používá. Cíl by měl mít 75-90% využití na většině virtuálních počítačů. Na virtuálních počítačích, které mají výjimečně nízké využití, zvažte jejich balení s více aplikacemi nebo migrace na virtuální počítače s minimálními náklady v Azure, které zachovávají správnou úroveň výkonu.

Azure poskytuje také nástroje pro optimalizaci prostředí. [Azure Advisor](/azure/advisor/advisor-overview?WT.mc_id=retailecomm-docs-scseely) sleduje součásti vašeho prostředí a poskytuje individuální doporučení na základě osvědčených postupů. Doporučení vám pomůžou vylepšit výkon, zabezpečení a dostupnost prostředků používaných ve vašich aplikacích. Azure Portal také zpřístupňuje informace o stavu vašich aplikací. Vaše virtuální počítače by měly využívat [rozšíření virtuálních počítačů Azure pro Linux a Windows](/azure/virtual-machines/extensions/overview?WT.mc_id=retailecomm-docs-scseely). Tato rozšíření poskytují konfiguraci po nasazení, antivirovou ochranu, monitorování aplikací a další. Můžete také využít mnoho dalších služeb Azure pro diagnostiku sítě, používání služeb a upozorňování prostřednictvím služeb, jako jsou [Network Watcher](/azure/network-watcher/network-watcher-monitoring-overview?WT.mc_id=retailecomm-docs-scseely), [Service map](/azure/monitoring/monitoring-walkthrough-servicemap?WT.mc_id=retailecomm-docs-scseely), [Application Insights](/azure/application-insights/app-insights-overview?WT.mc_id=retailecomm-docs-scseely)a [Log Analytics](/azure/log-analytics/log-analytics-overview?WT.mc_id=retailecomm-docs-scseely).

I když součásti organizace optimalizují systém v Azure, mohou vývojové týmy začít s přechodem do fáze po migraci: refaktoring.

## <a name="refactor"></a>Refaktorovat

Po dokončení migrace se může vaše aplikace elektronického obchodování začít využívat jejího nového domova v Azure. Fáze refaktoru není nutné čekat, dokud se nepřesunulo celé prostředí. Pokud váš tým CMS migruje, ale tým ERP nemá žádný problém. Tým CMS může stále zahájit své úsilí refaktoringu. Tato fáze zahrnuje použití dalších služeb Azure k optimalizaci nákladů, spolehlivosti a výkonu tím, že refaktoringuje vaše aplikace. Místo toho, abyste v tomto modelu využili jenom hardware spravovaného poskytovatele a operační systém, jste v tomto modelu také využili výhod cloudových služeb, které vám povedou k snížení nákladů. Svou aktuální aplikaci budete nadále používat tak, jak jsou, s nějakým drobným kódem aplikace nebo změnou konfigurace a připojíte svou aplikaci k novým službám infrastruktury, jako jsou kontejnery, databáze a systémy správy identit.

Úsilí refaktoringu mění velmi malý kód a konfiguraci. Zaměřte se víc času na automatizaci hlavně proto, že technologie přijaté v této fázi spoléhají na skriptování při sestavování a nasazování prostředků. pokyny k nasazení jsou skriptu.

I když je možné použít spoustu služeb Azure, zaměříme se na nejběžnější služby, které se používají ve fázi refaktorování: kontejnery, služby App Services a databázové služby. Proč se podívejme na refaktoring? Refaktoring poskytuje silnou základ kódu, který snižuje dlouhodobé náklady tím, že v rámci tohoto důvodu udržuje dluh kódu.

Kontejnery poskytují způsob, jak seskupit aplikace. Z důvodu způsobu, jakým kontejner virtualizuje operační systém, můžete do jednoho virtuálního počítače zabalit několik kontejnerů. Aplikaci můžete přesunout do kontejneru s nulovým počtem změn kódu; Možná budete potřebovat změny konfigurace. Toto úsilí také vede k psaní skriptů pro vytváření sad aplikací do kontejneru. Vaše vývojové týmy budou strávit vytváření a testování těchto skriptů časem refaktoringu. Azure podporuje kontejnerování prostřednictvím [služby Azure Kubernetes](/azure/aks/?WT.mc_id=retailecomm-docs-scseely) (AKS) a souvisejících [Azure Container Registry](https://azure.microsoft.com/services/container-registry/?WT.mc_id=retailecomm-docs-scseely) , které můžete použít ke správě imagí kontejneru.

Pro služby App Services můžete využít výhod různých služeb Azure. Například vaše stávající infrastruktura může obdržet objednávku zákazníka tím, že umístí zprávy do fronty, jako je [RabbitMQ](https://www.rabbitmq.com/). (Například jedna zpráva se účtuje zákazníkovi, druhým je odeslání objednávky.) Při opětovném hostování aplikace RabbitMQ umístíte do samostatného virtuálního počítače. Během refaktoringu přidáte do řešení [Service Busou](/azure/service-bus-messaging/service-bus-queues-topics-subscriptions?WT.mc_id=retailecomm-docs-scseely) frontu nebo téma, přepíšete kód RabbitMQ a zastavíte používání virtuálních počítačů, které obsluhují funkce služby Řízení front. Tato změna nahrazuje sadu virtuálních počítačů s trvalou službou front zpráv za účelem snížení nákladů. Další služby App Services najdete na webu Azure Portal.

Pro databáze můžete přesunout databázi z virtuálního počítače do služby. Azure podporuje SQL Server úlohy s [Azure SQL Database](/azure/sql-database/sql-database-cloud-migrate?WT.mc_id=retailecomm-docs-scseely) a [Azure SQL Database spravované instance](/azure/sql-database/sql-database-managed-instance?WT.mc_id=retailecomm-docs-scseely). [Služba migrace dat](https://azure.microsoft.com/services/database-migration/?WT.mc_id=retailecomm-docs-scseely) posuzuje vaši databázi, informuje o práci, kterou je třeba provést před migrací, a pak přesune databázi z virtuálního počítače do služby. Azure podporuje také [MySQL](https://azure.microsoft.com/services/mysql/?WT.mc_id=retailecomm-docs-scseely), [PostgreSQL](https://azure.microsoft.com/services/postgresql/?WT.mc_id=retailecomm-docs-scseely)a [Další služby databázového](https://azure.microsoft.com/services/#databases?WT.mc_id=retailecomm-docs-scseely) stroje.

## <a name="rebuild"></a>Sestavit znovu

Až do této chvíle jsme se snažili minimalizovat změny v elektronického obchodování systémech – nepoužíváme samostatné pracovní systémy. Teď se podíváme, jak skutečně využít cloud. Tato fáze znamená, že se má opravit existující aplikace tím, že se PaaS nebo dokonce i SaaS služby a architektura. Tento proces zahrnuje hlavní revize pro přidání nových funkcí nebo pro rearchitekturu aplikace pro Cloud.  _Spravovaná rozhraní API_ je nový koncept, který využívá cloudové systémy. Díky vytváření rozhraní API pro komunikaci mezi službami můžeme usnadnit aktualizaci našeho systému.  Druhou výhodou je možnost získat přehled o datech, která máme. Provedeme to tak, že přejdete do architektury _mikroslužeb a rozhraní API_ a pomocí strojového učení a dalších nástrojů analyzujete data.

### <a name="microservices--apis"></a>Mikroslužby a rozhraní API

Mikroslužby komunikují prostřednictvím externě orientovaných rozhraní API. Každá služba je samostatná a měla by implementovat jedinou podnikovou schopnost, například doporučit položky zákazníkům, udržovat nákupní košíky atd. Deskládání aplikace do mikroslužeb vyžaduje čas a plánování. I když neexistují žádná pevná pravidla pro definování mikroslužeb, obecný nápad zahrnuje snížení nasazujíelné jednotky na sadu komponent, které téměř vždy mění dohromady. Mikroslužby umožňují nasazovat změny, jak často jsou potřeba, a zároveň omezit zátěž testování pro celou aplikaci. Některé služby můžou být velmi malé. V případě, že nevyužívají žádné prostředky, pokud nepoužíváte, můžete pro ně využívat [Azure Functions](/azure/azure-functions/functions-overview?WT.mc_id=retailecomm-docs-scseely) dobře škálovat na tolik volajících, kolik potřebujete. Další služby budou rozdělené na obchodní možnosti: spravovat produkt, zaznamenávat objednávky zákazníků a tak dále.

Mechanismy bez serveru mají nevýhody: v případě světlé zátěže může být pomalé reagovat, protože určitý server v cloudu trvá několik sekund, než se konfigurace a spuštění vašeho kódu. Pro části vašeho prostředí používané zákazníky silně potřebujete zajistit, aby mohli najít produkty, umístit objednávky, vracet žádosti a tak dále s rychlostí a snadnou tím. V případě, že se výkon zpomaluje, riskujete ztrátu zákazníků v časovém trychtýři. Pokud máte funkce, které musí reagovat rychle, sestavte tuto funkci jako samostatně nasazovatelné jednotky ve [službě Azure Kubernetes](/azure/aks/?WT.mc_id=retailecomm-docs-scseely).  V ostatních případech, jako jsou například služby, které vyžadují určitou kombinaci paměti, několik procesorů a dostatek místních úložišť, může být vhodné hostovat mikroslužby ve vlastním virtuálním počítači.

Každá služba používá rozhraní API pro interakci. Přístup k rozhraní API může být přímý na mikroslužbu, ale to vyžaduje, aby kdokoli komunikoval se službou, aby dokázala zjistit topologii aplikace. Služba jako [API Management](/azure/api-management/?WT.mc_id=retailecomm-docs-scseely) poskytuje centrální způsob publikování rozhraní API. Všechny aplikace se jednoduše připojí ke službě API Management. Vývojáři můžou zjistit, jaká rozhraní API jsou k dispozici. Služba API Management také nabízí funkce, které umožní vašemu maloobchodnímu prostředí fungovat dobře. Služba může omezit přístup k rozhraní API různými částmi aplikace (abyste zabránili kritickým místům), odpovědím v mezipaměti pro pomalé změny hodnot, převádět z formátu JSON na XML a další. Úplný seznam zásad najdete [tady](/azure/api-management/api-management-policies?WT.mc_id=retailecomm-docs-scseely).

### <a name="make-use-of-your-data-and-the-azure-marketplace"></a>Využijte svá data a Azure Marketplace

Vzhledem k tomu, že máte všechna data a systémy v Azure, můžete do svého podnikání snadno začlenit další SaaS řešení. Můžete provést několik věcí hned. Můžete například použít [Power BI](https://powerbi.microsoft.com/?WT.mc_id=retailecomm-docs-scseely) ke spojování různých zdrojů dat, abyste mohli vytvářet vizualizace a sestavy – a získat přehledy.

V dalším kroku se podíváme na nabídky [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?WT.mc_id=retailecomm-docs-scseely) , které vám pomůžou dělat věci, jako je optimalizace inventáře, Správa kampaní na základě zákaznických atributů a prezentace příslušných položek pro každého zákazníka na základě jejich preferencí a historie. Očekává se, že strávíte nějakou dobu, po kterou budou data v nabídkách Marketplace fungovat.

## <a name="next-steps"></a>Další kroky

Řada vývojových týmů se chystá k opětovnému hostování a refaktoru současně za účelem vyřešení technického dluhu a lepší využití kapacity. Před přechodem k dalším krokům je možné nové hostování znovu hostovat.  Všechny problémy v nasazení do nového prostředí budou snazší diagnostikovat a opravovat. Tím zajistíte, aby se týmy vývoje a podpory vypnuly v Azure jako nové prostředí. Jak začnete Refaktorovat a znovu sestavit systém, vytváříte stabilní a funkční aplikaci. To umožňuje zmenšit, cílené změny a častější aktualizace.

Publikovali jsme obecnější dokument white paper o migraci do cloudu: [migrace do cloudu Essentials](https://azure.microsoft.com/resources/cloud-migration-essentials-e-book/?_lrsc=9618a836-9f81-4087-901f-51058783c3a8&WT.mc_id=retailecomm-docs-scseely). V průběhu plánování migrace si můžete prostudovat velmi velkou část.

## <a name="technologies-presented"></a>Prezentované technologie

Používá se během opětovného hostování:

- [Azure Advisor](/azure/advisor/advisor-overview?WT.mc_id=retailecomm-docs-scseely) je přizpůsobený cloudový poradce, který pomáhá dodržovat osvědčené postupy pro optimalizaci nasazení Azure.
- Služba [Azure Migrate](/azure/migrate/migrate-overview?WT.mc_id=retailecomm-docs-scseely) posuzuje místní úlohy pro migraci do Azure.
- [Azure Site Recovery](/azure/site-recovery/site-recovery-overview?WT.mc_id=retailecomm-docs-scseely) orchestruje a spravuje zotavení po havárii pro virtuální počítače Azure a místní virtuální počítače a fyzické servery.
- [Azure Virtual Network](/azure/virtual-network/virtual-networks-overview?WT.mc_id=retailecomm-docs-scseely) umožňuje mnoha typům prostředků Azure, jako je Azure Virtual Machines (VM), bezpečně komunikovat mezi sebou, internetem a místními sítěmi.
- [Azure ExpressRoute](/azure/expressroute/?WT.mc_id=retailecomm-docs-scseely) umožňuje rozmístit vaše místní sítě do cloudu Microsoftu přes soukromé připojení, které usnadňuje poskytovatel připojení.

Používá se během refaktoru:

- [Služba Azure Kubernetes](/azure/aks/?WT.mc_id=retailecomm-docs-scseely) spravuje vaše hostované prostředí Kubernetes, díky čemuž je rychlé a snadné nasazení a Správa kontejnerových aplikací bez znalosti orchestrace kontejnerů.
- [Azure SQL Database](/azure/sql-database/sql-database-technical-overview?WT.mc_id=retailecomm-docs-scseely) je služba spravovaná relační databáze pro obecné účely v Microsoft Azure. Podporuje struktury, jako jsou relační data, JSON, prostorová data a XML. SQL Database nabízí spravované samostatné databáze SQL, spravované databáze SQL v elastickém fondu a spravované instance SQL.

Používá se při opětovném sestavování:

- Azure [API Management](/azure/api-management/?WT.mc_id=retailecomm-docs-scseely) pomáhá organizacím při publikování rozhraní API pro externí, partnerské a interní vývojáře, aby bylo možné odemknout potenciál svých dat a služeb.
- [Azure Functions](/azure/azure-functions/functions-overview?WT.mc_id=retailecomm-docs-scseely) řešení ISA pro snadné spouštění malých částí kódu nebo &quot;funkcí&quot; v cloudu.
- [Power BI](https://powerbi.microsoft.com/?WT.mc_id=retailecomm-docs-scseely) je sada nástrojů pro obchodní analýzy, které poskytují přehledy napříč vaší organizací.

## <a name="conclusion"></a>Závěr

Přesun systému elektronického obchodování do Azure provede analýzu, plánování a definovaný přístup. Prohlédli jsme se třemi fázemi opětovného hostování, refaktoru a nového sestavení. Díky tomu může organizace přejít z jednoho pracovního stavu na jiný a současně minimalizovat velikost změny v každém kroku. Maloobchodní prodejci se také můžou rozhodnout Refaktorovat nebo dokonce znovu sestavit komponenty, přičemž se zcela přeskočí opětovné hostování. Mnohokrát budete mít jasnou cestu, která je předána k modernizaci. až to budete mít, můžete to provést. Při práci v Azure se dozvíte více příležitostí k přidávání nových možností, snížení nákladů a vylepšení celkového systému.


_Tento článek vytvořil Scott Seely a Mariya Zorotovich._