---
title: AI pro implementaci podrobného plánu zdravotnictví pomocí služeb Azure
author: dstarr
ms.author: dastarr
ms.date: 11/20/2019
ms.topic: article
ms.service: industry
description: Tento článek poskytuje pokyny pro Microsoft Azure podrobný plán pro AI.
ms.openlocfilehash: 40919ffde2c2cac11339b40348cba7a5e0e0e16d
ms.sourcegitcommit: 3b175d73a82160c4cacec1ce00c6d804a93c765d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052633"
---
# <a name="implementing-the-azure-blueprint-for-ai"></a>Implementace Azure detailu pro AI

## <a name="introduction"></a>Úvod

Organizace zdravotnictví mají za to, že AI (umělá inteligentní funkce) a ML (Machine Learning) můžou být cennými nástroji pro mnoho částí jejich podnikání, od vylepšení výsledků pacientů po zjednodušení každodenních operací. Zdravotnické organizace často nemají technologické pracovníky k implementaci systémů AI/ML. Společnost Microsoft vytvořila podrobný [plán Azure zdravotnictví AI](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr), aby se zlepšila Tato situace a rychle získala řešení AI a ml běžící v Azure. Pomocí podrobného plánu ukážeme, jak rychle začít s AI a ML v bezpečném, kompatibilním, zabezpečeném a spolehlivém způsobu.

Stavový plán pro AI zahájí v organizaci AI/ML do vaší organizace pomocí Azure. Tento článek popisuje, jak nainstalovat podrobný plán, jeho komponenty a jak ho použít ke spuštění experimentu AI/ML, který předpovídá dobu pobytu pacienta.

### <a name="benefits"></a>Výhody

Byl vytvořen podrobný plán, který poskytuje pokyny k zdravotním organizacím a rychlý Start na správné architektury PaaS (platforma jako služba) pro podporu AI/ML ve vysoce regulovaných zdravotnických prostředích, včetně toho, aby systém zachovává požadavky na dodržování předpisů HIPAA a HITRUST.

Technologické pracovníky v rámci zdravotnických organizací mají často málo času pro nové projekty, zejména ty, ve kterých se musí naučit novou a složitou technologii. Podrobný plán může pomáhat technickým pracovníkům seznámit se s Azure a několika jeho službami rychle a ušetřit tak náklady na výukovou křivku. Technický personál se může seznámit z podrobného plánu jako referenční implementace po instalaci portálu a využít ho k rozšiřování svých schopností nebo k vytvoření nového řešení AI a ML po podrobném plánu.

Podrobný plán vám pomůže začít pracovat s novými možnostmi AI a ML, a to rychle. S AI a ML je technický personál připravený na spouštění experimentů AI/ML pomocí dat shromažďovaných prostřednictvím různých zdrojů. Data mohou například existovat na předchozích instancích Sepsis a mnoho z doprovodných proměnných, které byly sledovány pro jednotlivá pacienty s podmínkou. Pomocí těchto dat v anonymním formátu může technický personál Hledat indikátory potenciálních Sepsis v pacientech a pomáhat měnit provozní postupy pro lepší zamezení této situaci.

Podrobný plán poskytuje data a ukázkový kód pro učení, jak předpovědět dobu pobytu pacienta. Toto je vzorový případ použití, který se dá použít k získání informací o komponentách řešení AI/ML.

### <a name="platform-or-infrastructure-as-a-service"></a>Platforma nebo infrastruktura jako služba

Microsoft Azure nabízí nabídky PaaS i SaaS a volba pravého pro vaše potřeby se u každého případu použití liší. Podrobný plán je navržený tak, aby používal služby PaaS, které se řeší pro předpověď délky pobytu pacienta v nemocnicích. V podrobném plánu Azure pro zdravotní péče AI najdete vše potřebné k vytvoření instance zabezpečeného a kompatibilního řešení AI/ML předem nakonfigurovaného pro organizace zdravotnictví. Model PaaS, který používá tento podrobný plán, nainstaluje a nakonfiguruje podrobný plán jako kompletní řešení.

### <a name="paas-option"></a>PaaS – možnost

Výsledkem použití modelu PaaS Services je snížení celkových nákladů na vlastnictví, protože neexistuje žádný hardware, který by bylo možné spravovat. Organizace nemusí kupovat a udržovat hardware nebo virtuální počítače. Podrobný plán používá výhradně služby PaaS Services.

Tím se sníží náklady na údržbu místního řešení a bezplatné technické pracovníky se soustředit na strategické iniciativy namísto infrastruktury. Může se taky přesunout za výpočetní výkon a úložiště od rozpočtů velkých výdajů až po rozpočty provozních výdajů. Náklady na spuštění tohoto scénáře podrobného plánu jsou založené na využití služeb a náklady na úložiště dat.

### <a name="iaas-option"></a>IaaS – možnost

I když se v podrobném plánu a tomto článku zaměřuje na implementaci PaaS, existuje [rozšíření Open Source](https://github.com/Azure/Azure-Health-Extension) , které umožňuje jeho použití v prostředích infrastruktury jako služba (IaaS).

V modelu hostování IaaS zákazníci platíte za dobu provozu hostovaných virtuálních počítačů Azure a jejich výpočetní výkon. IaaS poskytuje vyšší úroveň řízení, protože zákazník spravuje své vlastní virtuální počítače, ale obvykle se zvyšuje náklady, protože se k virtuálním počítačům účtuje doba provozu a využití. Zákazník je dál zodpovědný za údržbu virtuálních počítačů pomocí oprav, ochrany proti malwaru a tak dále.

Model IaaS překračuje rámec tohoto článku, který se zaměřuje na PaaS nasazení podrobného plánu.

## <a name="the-healthcare-aiml-blueprint"></a>Podrobný plán zdravotní péče AI/ML

Podrobný plán vytvoří výchozí bod pro použití této technologie v kontextu zdravotnictví. Při instalaci podrobného plánu do Azure se vytvoří všechny prostředky, služby a několik uživatelských účtů, aby podporovaly scénář AI/ML s příslušnými aktéry, oprávněními a službami.

Podrobný plán obsahuje experiment AI/ML, který předpovídá dobu pobytu pacienta, což může pomáhat při prognózování pracovníků, počtů lůžek a dalších logistikách. Balíček obsahuje instalační skripty, ukázkový kód, testovací data, podporu zabezpečení a ochrany osobních údajů a další.

## <a name="blueprint-technical-resources"></a>Technické zdroje podrobného plánu

Níže uvedené prostředky se nacházejí v tomto úložišti GitHubu.

Primární prostředky jsou:

1. [Powershellové](https://docs.microsoft.com/powershell/scripting/powershell-scripting?WT.mc_id=ms-docs-dastarr) skripty pro nasazení, konfiguraci a další úlohy.
2. [Podrobné pokyny k instalaci](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/deployment.md) , které obsahují informace o použití instalačního skriptu.
3. [Komplexní Nejčastější dotazy](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/faq.md).

Mezi důležité aspekty tohoto modelu patří identita a zabezpečení, které jsou obzvláště důležité při práci s daty o pacientech. Na tomto obrázku vidíte součásti kanálu ML.

![Kanál ML](assets/sg-healthcare-ai-blueprint-assets/ml-pipeline.png)

Na následujícím obrázku vidíte produkty Azure, které jsou nainstalované. Každý prostředek nebo služba nabízí součást řešení pro zpracování AI/ML, včetně otázek pro průřezy identity a zabezpečení.

![Zóny součástí](assets/sg-healthcare-ai-blueprint-assets/component-zones.png)

Implementace nového systému ve regulovaném zdravotním prostředí je složitá. Například zajištění, aby všechny aspekty systému byly kompatibilní s HIPAA a HITRUST certifikovaný využívá více než vývoj zjednodušeného řešení. Podrobný plán nainstaluje oprávnění k identifikaci a prostředkům k usnadnění těchto komplexních prostředků.

Podrobný plán taky poskytuje další skripty a data, která se používají k simulaci a studiu výsledků admitting nebo vyměřování pacientů. Tyto skripty umožňují pracovníkům okamžitě začít učit se, jak implementovat AI a ML pomocí řešení v bezpečném izolovaném scénáři.

### <a name="additional-blueprint-resources"></a>Další zdroje podrobného plánu

Podrobný plán obsahuje mimořádné doprovodné materiály a pokyny pro technické pracovníky a také obsahuje artefakty, které vám pomůžou vytvořit plně funkční instalaci. Mezi tyto další artefakty patří:

1. [Model hrozeb](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=01828de2-9555-4bac-a2a0-44e9ed2eeeaf&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr) pro použití s [Microsoft Threat Modeling Tool](https://www.microsoft.com/download/details.aspx?id=49168&WT.mc_id=ms-docs-dastarr). Tento model hrozeb zobrazuje komponenty řešení, data toků mezi nimi a hranice vztahu důvěryhodnosti. Tento nástroj se dá použít k modelování hrozeb, které by bylo možné využít k rozšíření základního podrobného plánu nebo získání informací o architektuře systému z hlediska zabezpečení.

2. [Matice zodpovědnosti zákazníka HiTRUST](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=eab85244-b9ab-490a-9e2a-611153f7d3af&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)v excelovém sešitu.  Zobrazí se informace o tom, co vám (zákazník) musí poskytnout a co Microsoft poskytne pro každý požadavek v matici. Další informace o této matrici odpovědnosti najdete v tomto článku v části zabezpečení a dodržování předpisů > podrobného plánu odpovědnosti v tomto dokumentu.

3. [Data o stavu HiTRUST a](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=ffc32e44-665e-46c5-b753-163d55a17d27&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr) dokument white paper o kontrole AI prozkoumají podrobný plán požadavků, který se má splnit pro certifikaci HiTRUST.

4. [HIPAA data o stavu a kontrola AI](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=d5ce675c-3e83-45db-98a6-ae77fc439436&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr) . dokument White Paper kontroluje architekturu s ohledem na HIPAAické předpisy.

Tyto prostředky jsou [tady na GitHubu](https://github.com/Azure/Health-Data-and-AI-Blueprint).

## <a name="installing-the-blueprint"></a>Instaluje se podrobný plán.

Tato řešení podrobného plánu vám umožní začít pracovat s malým množstvím času. Doporučuje se trochu znalosti o skriptování prostředí PowerShell, ale podrobné pokyny jsou k dispozici, které vám pomůžou s instalací, takže technologists bude úspěšným nasazením tohoto podrobného plánu bez ohledu na jejich znalosti skriptování.

Technický personál může očekávat instalaci podrobného plánu s malým množstvím zkušeností za použití Azure během 30 minut až po hodinu.

### <a name="the-installation-script"></a>Instalační skript

Podrobný plán poskytuje výjimečné doprovodné materiály a pokyny k instalaci. Poskytuje také skriptování pro instalaci a odinstalaci služeb a prostředků podrobného plánu. Volání skriptu nasazení prostředí PowerShell je jednoduché. Před instalací podrobného plánu je třeba shromáždit určitá data a použít je jako argumenty skriptu Deploy. ps1, jak je znázorněno níže.

```powershell
.\deploy.ps1 -deploymentPrefix <prefix> `
            -tenantId <tenant id> ` # also known as the AAD directory
            -tenantDomain <tenant domain> `
            -subscriptionId <subscription id> `
            -globalAdminUsername <user id> ` # ID from your AAD account
            -deploymentPassword <universal password> ` # applied to all new users and service accounts
            -appInsightsPlan 1 # we want app insights set up
```

### <a name="the-installation-environment"></a>Instalační prostředí

**Významná!** Neinstalujte plán z počítače mimo Azure. Pokud v Azure vytvoříte čistý Windows 10 (nebo jiný virtuální počítač s Windows) a z něj spustíte instalační skripty, instalace je mnohem pravděpodobnější. Tato technika používá cloudový virtuální počítač k omezení latence a k vytvoření hladké instalace.

Během instalace skript volá jiné balíčky, aby se načetly a používaly. Při instalaci z virtuálního počítače v Azure bude prodleva mezi instalačním počítačem a cílovými prostředky mnohem nižší. Nicméně některé stažené balíčky skriptů jsou stále zranitelné kvůli latenci jako balíčky skriptů živě mimo prostředí Azure, což může vést k výpadkům při vypršení časového limitu.

### <a name="install-failure-dont-panic"></a>Instalace se nezdařila! (nenouzové)

Instalační program stáhne během instalace některé externí balíčky. Někdy vyprší časový limit požadavku prostředku skriptu z důvodu prodlevy mezi instalačním počítačem a balíčkem. Pokud k tomu dojde, máte dvě možnosti:

1. Spusťte instalační skript znovu beze změn. Instalační program zkontroluje již přidělené prostředky a nainstaluje pouze ty, které jsou potřeba. I když tato technika může fungovat, je riziko, že instalační skript zkusí přidělit prostředky, které už jsou na svém místě. To může způsobit chybu a instalace se nezdaří.

2. Pořád spouštíte skript Deploy. ps1, ale předáte různé argumenty pro odinstalaci služeb podrobného plánu.

```powershell
.\deploy.ps1 -clearDeploymentPrefix <prefix> `
             -tenantId <value> `
             -subscriptionId <value> `
             -tenantDomain <value> `
             -globalAdminUsername <value> `
             -clearDeployment
```

Po dokončení odinstalace změňte předponu v instalačním skriptu a zkuste instalaci zopakovat. K potížím s latencí nemusí dojít znovu. Pokud se instalace při stahování balíčků skriptů nezdařila, spusťte skript odinstalačního programu a znovu spusťte instalační program.

Po spuštění odinstalačního skriptu dojde k následujícímu:

- Uživatelé nainstalovaní instalačním skriptem
- Skupiny prostředků a jejich příslušné služby se odešlou, včetně úložiště dat.
- Aplikace zaregistrovaná v AAD (Azure Active Directory)

Všimněte si, že Key Vault se drží jako "obnovitelné odstranění" a i když se na portálu nezobrazuje, nezíská se po dobu 30 dnů navráceno. To umožňuje v případě potřeby reKey Vault. Další informace o důsledcích tohoto a způsobu, jak ho zpracovat, najdete v části technické problémy > Key Vault v tomto článku.

### <a name="reinstall-after-an-uninstall"></a>Po odinstalaci znovu nainstalovat

Pokud po odinstalaci potřebujete znovu nainstalovat plán, musíte změnit předponu v dalším nasazení, protože odinstalovaná Key Vault způsobí chybu, pokud předponu neměňte. Další informace najdete v části _technické problémy > Key Vault_ v tomto článku.

### <a name="required-administrator-roles"></a>Požadované role správce

Osoba instalující plán musí být v AAD role Globální správce. Účet pro instalaci musí být také správcem předplatného Azure pro použití předplatného. Pokud uživatel, který provádí instalaci, není v obou těchto rolích, instalace se nezdaří.

![Instalační program podrobného plánu](assets/sg-healthcare-ai-blueprint-assets/blueprint-installer.png)

Instalace navíc není navržená tak, aby fungovala s předplatnými MSDN kvůli těsné integraci s AAD. Musí se použít standardní účet Azure. V případě potřeby [získáte bezplatnou zkušební verzi](https://azure.microsoft.com/free/?WT.mc_id=ms-docs-dastarr) s kreditem na instalaci řešení podrobného plánu a spuštění jeho ukázek.

## <a name="adding-other-resources"></a>Přidávání dalších prostředků

Instalace podrobného plánu Azure neobsahuje více služeb, než je potřeba k implementaci případu použití AI/ML. Do prostředí Azure se ale dá přidat víc prostředků nebo služeb, což je dobrá zkušební plocha pro další iniciativy nebo výchozí bod pro produkční systém. Například jedna může přidat další služby PaaS nebo IaaS prostředků do stejného předplatného a AAD.

Do řešení se můžou přidat nové prostředky, například [Cosmos DB](/azure/cosmos-db/introduction?WT.mc_id=ms-docs-dastarr) nebo nové [Azure Functions](/azure/azure-functions/functions-overview?WT.mc_id=ms-docs-dastarr), protože je potřeba víc možností Azure. Při přidávání nových prostředků nebo služeb se ujistěte, že jsou nakonfigurované tak, aby splňovaly zásady zabezpečení a ochrany osobních údajů, aby byly v souladu s předpisy a zásadami.

Nové prostředky a služby můžou být vytvořené pomocí [rozhraní Azure REST API](https://docs.microsoft.com/rest/api/?view=Azure&WT.mc_id=ms-docs-dastarr), [Azure PowerShell skriptování](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-6.6.0&WT.mc_id=ms-docs-dastarr)nebo pomocí webu [Azure Portal](https://portal.azure.com/?WT.mc_id=ms-docs-dastarr).

## <a name="using-machine-learning-with-the-blueprint"></a>Použití strojového učení s podrobným plánem

Podrobný plán byl navržený tak, aby předvedl scénář ML s regresním algoritmem použitým v modelu, který předpovídá [dobu pobytu pacienta](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr#example-use-case). Toto je běžná předpověď pro to, aby poskytovatelé zdravotnictví mohli běžet, protože pomáhá plánovat personální oddělení a další provozní rozhodnutí. V průběhu času je možné zjistit anomálie v době, kdy se průměrná doba pobytu pro danou podmínku zvyšuje nebo odmítá.

### <a name="ingesting-training-data"></a>Ingestování školicích dat

S nainstalovaným plánem a všemi službami fungují i data, která se mají analyzovat. 100 000 [záznamů o pacientech je k dispozici pro](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr#ingest) ingestování a práci s modelem. Ingestování záznamů o pacientech je prvním krokem při použití [Azure Machine Learning Studio](/azure/machine-learning/studio/what-is-ml-studio?WT.mc_id=ms-docs-dastarr) ke spuštění délky experimentu, jak je znázorněno níže.

![Ingestování](assets/sg-healthcare-ai-blueprint-assets/ingest.png)

Podrobný plán obsahuje experiment a nezbytná data pro spuštění úlohy ML v Machine Learning Studio (MLS). V příkladu se používá školený model v experimentu k předpovědi délky pacienta v závislosti na mnoha proměnných.

V tomto ukázkovém prostředí jsou data ingestovaná do služby Azure SQL Database volná z jakýchkoli vad nebo chybějících datových elementů. Tato data jsou čistá. Často se nečistá data ingestují a musí je "vyčistit", aby je bylo možné použít ke krmení školicích algoritmů Machine Learning, nebo před použitím dat v úloze ML. Chybějící data nebo nesprávné hodnoty v datech mají negativní vliv na výsledky analýzy ML.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

Mnoho organizací zdravotnictví nemá technické pracovníky soustředit se na projekty v ML. To často znamená, že cenná data se nepoužívají nebo jsou drahé konzultanti přihlášeni k vytváření řešení v ML.

Odborníci na AI a ML i tyto učení o AI/ML můžou použít Azure Machine Learning Studio k návrhu experimentů. MLS je webové prostředí pro návrh, které se používá k vytváření experimentů ML. Pomocí MLS můžete vytvářet, vyvíjet, vyhodnocovat a hodnotit modely a ušetřit tak úžasné čas při použití různých nástrojů k vývoji modelů.

MLS nabízí kompletní sadu nástrojů pro úlohy ML. To znamená, že lidé novinkou do ML můžou začít s použitím tohoto nástroje a rychleji vydávat výsledky než jiné nástroje ML. Díky tomu může vaši pracovníci IT soustředit se na poskytnutí hodnoty jinde a bez uvedení do odborníka ML. Tato funkce ve vaší vlastní organizaci zdravotnictví znamená, že je možné testovat různé hypotézy a výsledná data analyzovaná pro užitečné poznatky, jako jsou třeba zásahy na pacienty, nabízejí [předem zapsané moduly](/azure/machine-learning/studio-module-reference/index?WT.mc_id=ms-docs-dastarr#help-for-machine-learning-modules) , které se použijí na plátně pro přetahování, a vizuálně sestavovat ucelené pracovní postupy pro datové vědy jako experimenty.

Existují předem zapsané moduly, které zapouzdřují konkrétní algoritmy, jako jsou například stromy rozhodování, doménové struktury, clustering, časová řada, detekce anomálií a další.

Vlastní moduly lze přidat do libovolného experimentu. Ty jsou napsané v [jazyce R](/azure/machine-learning/studio/extend-your-experiment-with-r?WT.mc_id=ms-docs-dastarr) nebo v [Pythonu](/azure/machine-learning/studio/execute-python-scripts?WT.mc_id=ms-docs-dastarr). To umožňuje použití předem připravených modulů a vlastní logiky pro vytvoření propracovanější experiment.

MLS umožňuje [vytváření a používání](/azure/machine-learning/studio-module-reference/machine-learning-initialize-model?WT.mc_id=ms-docs-dastarr) výukových modelů a poskytuje sadu předem navržených experimentů pro použití v běžných aplikacích. Kromě toho je možné přidat nové experimenty do MLS beze změny jakýchkoli prostředků podrobného plánu.

Pokud chcete ušetřit čas, přejděte na [Azure AI Gallery](https://gallery.azure.ai/industries/healthcare?WT.mc_id=ms-docs-dastarr) a vyhledejte řešení připravená k použití ml pro konkrétní odvětví, včetně zdravotní péče. Galerie obsahuje například řešení a experimenty pro detekci duplicit v případě, kdy je předpověď srdcových nemocí.

## <a name="security-and-compliance"></a>Zabezpečení a dodržování předpisů

Zabezpečení a dodržování předpisů jsou dvě nejdůležitější věci, které je potřeba mít na vědomí při vytváření, instalaci nebo správě softwarových systémů v lékařském prostředí. Investice na základě přijetí softwarového systému můžou být nižší než požadavky na požadované zásady zabezpečení a certifikace.

I když je tento článek a podrobný plán zdravotnictví zaměřen na technické zabezpečení, jsou důležité i další typy zabezpečení, včetně fyzického zabezpečení a zabezpečení správy. Tato témata zabezpečení jsou nad rámec tohoto článku, který se zaměřuje na technický bezpečnostní plán.

### <a name="principle-of-least-privilege"></a>Princip nejnižších oprávnění

Podrobný plán nainstaluje pojmenované uživatele s rolemi k podpoře a omezí jejich přístup k prostředkům v řešení. Tento model se označuje jako "princip nejnižší úrovně oprávnění" a přístup k prostředkům v návrhu systému. Princip uvádí, že služba a uživatelské účty by měly mít přístup jenom k systémům a službám, které jsou potřeba pro oprávněný účel.

Tento model zabezpečení zajišťuje kompatibilitu systému s požadavky HIPAA a HITRUST a odstraňuje riziko pro organizaci.

### <a name="defense-in-depth"></a>Důkladná ochrana

Návrhy systémů, které používají více abstraktních vrstev ovládacích prvků zabezpečení, jsou zabezpečeny s využitím důkladné obrany. Důkladná ochrana zajišťuje redundanci zabezpečení na více úrovních. Znamená to, že nezáleží na jedné vrstvě obrany. Zajišťuje, aby účty uživatelů a služeb měly odpovídající přístup k prostředkům, službám a datům. Azure poskytuje prostředky zabezpečení a monitorování v každé úrovni systémové architektury, aby poskytovala důkladnou hloubku pro celou šířku technologie.

V softwarovém systému, jako je ten nainstalovaný v podrobném plánu, se uživatel může přihlásit, ale nemá oprávnění ke konkrétnímu prostředku. Tento příklad obrany ochrany zajišťuje funkce RBAC (Access Control na základě rolí) a AAD, která podporuje princip nejnižších oprávnění.

Dvojúrovňové ověřování je také podrobná formou technické obrany a může být volitelně zahrnuto v případě, kdy je plán nainstalován.

### <a name="azure-key-vault"></a>Azure Key Vault

Služba Azure Key Vault je kontejner – trezor, který slouží k ukládání tajných klíčů, certifikátů a dalších dat používaných aplikacemi. Mezi ně patří databázové řetězce, adresy URL koncového bodu REST, klíče rozhraní API a další vývojáři, kteří nechtějí do aplikace v souboru. config zavádět kód ani je distribuovat.

Kromě toho jsou trezory přístupné pro identity služby Application Service nebo jiné účty v rámci s oprávněními AAD. To umožňuje, aby k tajným klíčům byl za běhu přistup aplikace, které potřebují obsah trezoru.

Klíče uložené v trezoru můžou být šifrované nebo podepsané a používání klíče se dá monitorovat pro jakékoli obavy ze zabezpečení.

Pokud se Key Vault odstraní, neodstraní se hned z Azure. Dopad tohoto problému je popsaný v části _technické problémy > Key Vault_ tomto článku.

### <a name="application-insights"></a>Application Insights

Organizace zdravotnictví často mají zásadní a životně důležité systémy, které musí být spolehlivé a odolné. Anomálie nebo výpadky ve službě se musí zjistit a opravit co nejdříve. [Application Insights](/azure/application-insights/app-insights-proactive-application-security-detection-pack?WT.mc_id=ms-docs-dastarr) je technologie APM (Application Performance Management), která monitoruje aplikace a odesílá výstrahy, když se něco nepovede. Monitoruje aplikace za běhu z důvodu chyb nebo anomálií aplikací. Je navržená pro práci s více programovacími jazyky a poskytuje bohatou sadu funkcí, které vám pomůžou zajistit, aby byly aplikace v pořádku a běžely bezproblémově.

Například aplikace může mít nevrácenou paměť. Application Insights může pomáhat najít a diagnostikovat problémy pomocí bohatých sestav a klíčových ukazatelů výkonu, které monitoruje. Application Insights je robustní služba APM pro vývojáře aplikací.

Tato [interaktivní ukázka](https://analytics.applicationinsights.io/demo#/discover/home?WT.mc_id=ms-docs-dastarr) ukazuje klíčové funkce a funkce Application Insights, včetně kompletního řídicího panelu monitorování, který může technologists v organizaci pro zdravotnictví použít k monitorování stavu aplikace a stavu.

#### <a name="azure-security-center"></a>Azure Security Center

Zabezpečení v reálném čase a sledování klíčových ukazatelů výkonu je nezbytné v důležitých aplikacích. Azure Security Center (ASC) pomáhá zajistit zabezpečení a ochranu vašich prostředků Azure. ASC je služba pro správu zabezpečení a rozšířenou ochranu před internetovými útoky. Dá se použít k uplatnění zásad zabezpečení napříč vašimi úlohami, omezení vystavení hrozbám a detekci a reakci na útoky.

Security Center Standard poskytuje následující služby.

- **Hybridní zabezpečení** – Získejte jednotný přehled o zabezpečení napříč všemi vašimi místními a cloudových úlohami. To je užitečné hlavně v hybridních cloudových sítích používaných organizacemi pro zdravotní péče s Azure.
- **Rozšířená detekce hrozeb** – ASC používá pokročilou analýzu a [Microsoft Intelligent Security Graph](http://cloud-platform-assets.azurewebsites.net/intelligent-security-graph/?WT.mc_id=ms-docs-dastarr) k získání hraničních zařízení, která vám pomohou vyvíjejí útoky a zmírnit je okamžitě.
- **Řízení přístupu a aplikací** – zablokuje malware a další nežádoucí aplikace pomocí doporučení pro přidávání na seznam povolených pro vaše konkrétní úlohy a využívá Machine Learning.

V souvislosti s plánem stavu AI analyzuje ASC součásti systému a poskytuje řídicí panel, který zobrazuje ohrožení zabezpečení v rámci služeb a prostředků v předplatném. Samostatné prvky řídicího panelu poskytují přehled o potížích řešení následujícím způsobem.

- Zásady a dodržování předpisů
- Hygiena zabezpečení prostředků
- Ochrana před hrozbami

Níže je příklad řídicího panelu, který identifikuje 13 návrhů na vylepšení slabých míst ohrožení zabezpečení systému. Zobrazuje také pouhých 46% dodržování předpisů pomocí HIPAA a zásad.

![Ochrana před hrozbami](assets/sg-healthcare-ai-blueprint-assets/threat_protection.png)

Podrobnější informace o potížích se zabezpečením s vysokou závažností ukazují, které prostředky jsou ovlivněné, a náprava potřebná pro jednotlivé prostředky, jak je znázorněno níže.

Zaměstnanci IT můžou strávit spoustu hodin, kteří se pokoušejí ručně zabezpečit všechny prostředky a sítě. Pomocí ASC k identifikaci ohrožení zabezpečení v daném systému může být čas strávený v jiných strategických činnostech. V případě mnoha identifikovaných chyb může ASC automaticky použít akci oprava a zabezpečit prostředek, aniž by museli mít oprávnění k tomu, aby se mohl dig hluboko do problému.

![Vysoká rizika](assets/sg-healthcare-ai-blueprint-assets/high_risks.png)

Služba ACS ještě více zajišťuje možnosti detekce hrozeb a výstrah. Pomocí služby ACS můžete monitorovat sítě, počítače a cloudové služby pro příchozí útoky a činnost po porušení zabezpečení vašeho prostředí.
ASC automaticky shromažďuje, analyzuje a integruje informace o zabezpečení a protokoly z nejrůznějších prostředků Azure. 

Funkce ML v ASC umožní IT zjišťování ručních přístupů, které neodhalí hrozby. Seznam výstrah zabezpečení s určením priorit se zobrazuje v ASC společně s informacemi potřebnými k rychlému prozkoumání problému spolu s doporučeními, jak útok napravit.

### <a name="rbac-security"></a>Zabezpečení RBAC

[Access Control na základě rolí](/azure/role-based-access-control/role-assignments-portal?WT.mc_id=ms-docs-dastarr) (RBAC) poskytuje nebo odmítá přístup k chráněným prostředkům, někdy s konkrétními právy na prostředek. Tím se zajistí, že k určeným systémovým součástem budou mít přístup jenom odpovídající uživatelé. Správce databáze může například mít přístup k databázi obsahující šifrovaná data pacienta, zatímco poskytovatel zdravotní péče může mít přístup pouze k záznamům příslušného pacienta přes aplikaci, která je zobrazuje. Většinou se jedná o elektronický zdravotní záznam nebo elektronický systém záznamů o zdravotním stavu. Zdravotní sestry nepotřebuje přístup k databázím a správce databáze nemusí vidět data záznamů o stavu pacienta.

K tomu je potřeba, aby byla RBAC součástí zabezpečení Azure a aby bylo možné přesně zaměřit správu přístupu pro prostředky Azure. Jemně odstupňovaná nastavení pro každého uživatele umožňují správcům zabezpečení a systémů být velmi přesné v právech, která dávají každému uživateli.

### <a name="blueprint-responsibility-matrix"></a>Tabulka odpovědnosti podrobného plánu

Matice zodpovědnosti zákazníka HITRUST je excelový dokument, který podporuje zákazníky, kteří implementují a zdokumentují ovládací prvky zabezpečení pro systémy založené na Azure. Sešit obsahuje seznam důležitých požadavků HITRUST a vysvětluje, jak společnost Microsoft a zákazníci zodpovídají za každou z nich.

Pro zákazníky, kteří vytvářejí systémy v Azure, je nutné pochopit sdílenou zodpovědnost při implementaci ovládacích prvků zabezpečení v cloudovém prostředí. Implementace konkrétního řízení zabezpečení může být zodpovědností společnosti Microsoft, zodpovědností zákazníků nebo sdílené zodpovědnosti mezi společností Microsoft a zákazníky. Různé implementace cloudu mají vliv na způsob sdílení zodpovědnosti mezi společností Microsoft a zákazníky.

Příklady najdete v následující tabulce zodpovědnosti.

|Odpovědnosti Azure  |Odpovědnosti zákazníků  |
|---------|---------|
|Azure zodpovídá za implementaci, správu a monitorování metod a mechanismů programu Information Protection, které se vztahují k příslušnému prostředí poskytování služeb. | Zákazník je zodpovědný za implementaci, konfiguraci, správu a monitorování metod a mechanismů programu Information Protection pro prostředky řízené zákazníky, které se používají pro přístup ke službám Azure a jejich využívání. |
|Azure zodpovídá za implementaci, konfiguraci, správu a monitorování metod a mechanismů správy účtů ve vztahu k příslušnému prostředí poskytování služeb. |Zákazník je také zodpovědný za správu účtů nasazených instancí virtuálních počítačů Azure a rezidentních komponent aplikace.|

Toto jsou jenom dva příklady mnoha zodpovědností, které je potřeba vzít v úvahu při nasazování cloudových systémů. Matice zodpovědnosti zákazníka HITRUST je navržená tak, aby podporovala dodržování předpisů HITRUST v organizaci s implementací systému Azure.

## <a name="customization"></a>Přizpůsobení

Po instalaci je běžné upravit plán. Důvody a techniky pro přizpůsobení prostředí se liší.

Plán může být před instalací přizpůsoben úpravou instalačních skriptů. I když je to možné, je vhodné vytvořit nezávislé skripty PowerShellu, které se spustí po dokončení počáteční instalace. Po počáteční instalaci je možné do systému přidat také nové služby prostřednictvím portálu.

Přizpůsobení mohou zahrnovat některé z následujících možností.

- Přidání nových experimentů k Machine Learning Studio
- Přidání dalších nesouvisejících služeb do prostředí
- Úprava příjmu dat a výstupu testu ML na použití jiného zdroje dat než databáze Azure SQL patientdb
- Poskytování produkčních dat do experimentu ML
- Vyčištění všech speciálních dat, která se ingestují, aby odpovídala tomu, co experiment vyžaduje

Přizpůsobení instalace se neliší od práce s jakýmkoli řešením Azure. Je možné přidat nebo odebrat služby nebo prostředky a poskytovat nové funkce. Při přizpůsobování podrobného plánu se postará o změnu celkového kanálu ML, abyste zajistili, že implementace pokračuje v práci.

## <a name="technical-issues"></a>Technické problémy

Následující problémy mohou způsobit selhání instalace podrobného plánu nebo instalace v nežádoucí konfiguraci.

### <a name="key-vault"></a>Key Vault

Trezory klíčů jsou při odstraňování prostředku Azure jedinečné. Trezory uchovává Azure pro účely obnovení. Při každém spuštění instalačního skriptu se musí do instalačního skriptu předat jiná předpona, jinak se instalace nezdaří kvůli kolizi se starým názvem trezoru. Trezory klíčů a všechny další prostředky jsou pojmenovány pomocí předpony, kterou zadáte do instalačního skriptu.

Key Vault vytvořeného instalačním skriptem se zachová jako "obnovitelné odstranění" po dobu 30 dnů. I když není aktuálně přístupná přes portál, je možné odstraněné [trezory klíčů spravovat z PowerShellu](/azure/key-vault/key-vault-soft-delete-powershell?WT.mc_id=ms-docs-dastarr)a můžete je dokonce odstranit ručně.

### <a name="azure-active-directory"></a>Azure Active Directory

Důrazně doporučujeme nainstalovat podrobný plán do prázdného AAD, nikoli do produkčního systému. Vytvořte novou instanci AAD a použijte její ID tenanta během instalace, abyste se vyhnuli přidávání účtů podrobného plánu do vaší aktivní instance AAD.

## <a name="technologies-presented"></a>Prezentované technologie

- Přečtěte si další informace o [datech o stavu Azure a](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr)podrobném plánu AI.
- Tady si můžete stáhnout, klonovat nebo rozvětvit [úložiště GitHub](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/deployment.md).
- [Machine Learning Studio](/azure/machine-learning/?WT.mc_id=ms-docs-dastarr) je pracovní prostor a nástroj pro práci s daty k vytváření Machine Learning experimentů. Umožňuje používat integrované algoritmy, widgety pro zvláštní účely a skripty Pythonu a R.
- Tajné klíče, certifikáty a další soukromá data se nacházejí v [Azure Key Vault](/azure/key-vault/key-vault-whatis?WT.mc_id=ms-docs-dastarr).
- Skriptovací jazyk PowerShell je instrumentací pro nastavení podrobného plánu, i když jsou v pokynech k instalaci uvedeny potřebné příkazy.
- [Azure AI Gallery](https://gallery.azure.ai/) poskytuje pole receptu pro řešení AI a ml užitečná pro zákazníky podle jejich odvětví. K dispozici je několik řešení publikovaných odborníky přes data spolu s dalšími odborníky na zdravotní péči.
- [Azure Security Center](/azure/security-center/?WT.mc_id=ms-docs-dastarr) poskytuje přehledy o chování vaší aplikace, chybách zabezpečení a technikách zmírnění rizik.

## <a name="wrapping-up"></a>Zabalení

[Data o stavu Azure pro AI](/azure/security/blueprints/azure-health?WT.mc_id=ms-docs-dastarr) je kompletní řešení ml a dá se použít jako studijní nástroj pro technologists, který vám umožní lépe pochopit Azure a zajistit, aby systémy splňovaly zákonné požadavky na zdravotní péči. Dá se použít taky jako výchozí bod pro produkční systém, který používá Azure Machine Learning Studio jako ústřední bod.

[Stáhněte](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/deployment.md) si podrobný plán, abyste mohli začít s implementací v řádu hodin, ne dnů nebo týdnů. Pokud máte problémy s instalací nebo dotazy ohledně podrobného plánu, přejděte [na stránku Nejčastější dotazy](https://github.com/Azure/Health-Data-and-AI-Blueprint/blob/master/faq.md).

Stáhněte si podpůrné materiály, které vám pomůžou lépe pochopit implementaci podrobného plánu nad rámec instalace a ML experimentu. Tato záruka zahrnuje následující.

1. [HITRUST matice odpovědnosti zákazníka](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=eab85244-b9ab-490a-9e2a-611153f7d3af&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)
2. [Komplexní model hrozeb](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=01828de2-9555-4bac-a2a0-44e9ed2eeeaf&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)
3. [HITRUST data o stavu a dokument white paper o kontrole AI](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=ffc32e44-665e-46c5-b753-163d55a17d27&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics&WT.mc_id=ms-docs-dastarr)
4. [HIPAA data o stavu a revize AI](https://servicetrust.microsoft.com/ViewPage/HIPAABlueprint?command=Download&downloadType=Document&downloadId=d5ce675c-3e83-45db-98a6-ae77fc439436&docTab=d7c399a0-2b92-11e8-9910-13dc07d708f7_Data_Analytics?WT.mc_id=ms-docs-dastarr)

Bez ohledu na to, jestli používáte účely podrobného plánu pro učení nebo pro základní řešení AI a ML pro vaši organizaci, nabízí výchozí bod pro práci s AI/ML v Azure se zaměřením na zdravotní péči.