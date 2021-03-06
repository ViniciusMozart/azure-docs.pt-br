---
title: Visão geral da política do firewall do aplicativo Web do Azure (WAF)
description: Este artigo é uma visão geral das políticas globais, por site e por URI do WAF (firewall do aplicativo Web).
services: web-application-firewall
ms.topic: article
author: winthrop28
ms.service: web-application-firewall
ms.date: 10/14/2020
ms.author: victorh
ms.openlocfilehash: 0f7fec997ec6d0c73fe6d1039ab1033752e0cedf
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94566564"
---
# <a name="azure-web-application-firewall-waf-policy-overview"></a>Visão geral da política do firewall do aplicativo Web do Azure (WAF)

As políticas de firewall do aplicativo Web contêm todas as configurações de WAF e configurações. Isso inclui exclusões, regras personalizadas, regras gerenciadas e assim por diante. Essas políticas são então associadas a um gateway de aplicativo (global), um ouvinte (por site) ou uma regra baseada em caminho (por URI) para que eles entrem em vigor.

> [!NOTE]
> As políticas por URI do WAF (firewall do aplicativo Web) do Azure estão em visualização pública.
> 
> Essa versão prévia pública é fornecida sem um SLA e não deve ser usada para cargas de trabalho de produção. Talvez alguns recursos não tenham suporte, tenham recursos restritos ou não estejam disponíveis em todos os locais do Azure. Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Não há limite para o número de políticas que você pode criar. Quando você cria uma política, ela deve estar associada a um gateway de aplicativo para entrar em vigor. Ele pode ser associado a qualquer combinação de gateways de aplicativo, ouvintes e regras baseadas em caminho.

## <a name="global-waf-policy"></a>Política global de WAF

Quando você associa uma política WAF globalmente, cada site por trás do seu gateway de aplicativo WAF é protegido com as mesmas regras gerenciadas, regras personalizadas, exclusões e quaisquer outras configurações definidas.

Se desejar que uma única política se aplique a todos os sites, você poderá associar a política ao gateway de aplicativo. Para obter mais informações, consulte [criar políticas de firewall do aplicativo Web para o gateway de aplicativo](create-waf-policy-ag.md) para criar e aplicar uma política WAF usando o portal do Azure. 

## <a name="per-site-waf-policy"></a>Política de WAF por site

Com as políticas de WAF por site, você pode proteger vários sites com diferentes necessidades de segurança por trás de um único WAF usando as políticas por site. Por exemplo, se houver cinco sites por trás de seu WAF, você poderá ter cinco políticas WAF separadas (uma para cada ouvinte) para personalizar as exclusões, as regras personalizadas, os conjuntos de regras gerenciadas e todas as outras configurações de WAF para cada site.

Digamos que o seu gateway de aplicativo tenha uma política global aplicada a ele. Em seguida, aplique uma política diferente a um ouvinte nesse gateway de aplicativo. A política do ouvinte agora entra em vigor apenas para esse ouvinte. A política global do gateway de aplicativo ainda se aplica a todos os outros ouvintes e a todas as outras regras baseadas em caminho que não têm uma política específica atribuída a eles.

## <a name="per-uri-policy"></a>Política por URI

Para obter ainda mais personalização até o nível de URI, você pode associar uma política de WAF a uma regra com base em caminho. Se houver determinadas páginas em um único site que exijam políticas diferentes, você poderá fazer alterações na política WAF que afetam apenas um determinado URI. Isso pode se aplicar a uma página de pagamento ou de entrada ou a qualquer outro URI que precise de uma política de WAF ainda mais específica do que os outros sites por trás de seu WAF.

Assim como ocorre com políticas de WAF por site, políticas mais específicas substituem aquelas menos específicas. Isso significa que uma política por URI em um mapa de caminho de URL substitui qualquer política de WAF por site ou global acima dela.

## <a name="example"></a>Exemplo

Digamos que você tenha três sites: contoso.com, fabrikam.com e adatum.com todos atrás do mesmo gateway de aplicativo. Você quer um WAF aplicado a todos os três sites, mas precisa de segurança adicional com o adatum.com, pois é aí que os clientes visitam, navegam e compram produtos.

Você pode aplicar uma política global ao WAF, com algumas configurações básicas, exclusões ou regras personalizadas, se necessário, para interromper alguns falsos positivos do bloqueio do tráfego. Nesse caso, não é necessário ter regras de injeção de SQL globais em execução porque fabrikam.com e contoso.com são páginas estáticas sem back-end SQL. Portanto, você pode desabilitar essas regras na política global.

Essa política global é adequada para contoso.com e fabrikam.com, mas você precisa ter mais cuidado com adatum.com em que as informações de entrada e os pagamentos são manipulados. Você pode aplicar uma política por site ao ouvinte do adatum e deixar as regras do SQL em execução. Além disso, suponha que haja um cookie bloqueando algum tráfego, para que você possa criar uma exclusão para esse cookie para parar o falso positivo. 

O URI adatum.com/payments é onde você precisa ter cuidado. Portanto, aplique outra política a esse URI e deixe todas as regras habilitadas e também remova todas as exclusões.

Neste exemplo, você tem uma política global que se aplica a dois sites. Você tem uma política por site que se aplica a um site e, em seguida, uma política por URI que se aplica a uma regra específica baseada em caminho. Consulte (Inserir link aqui quando existir) como criar políticas por site e por URI para o PowerShell correspondente para este exemplo.

## <a name="existing-waf-configurations"></a>Configurações de WAF existentes

Todas as novas configurações de WAF do firewall do aplicativo Web (regras personalizadas, configurações do conjunto de regras gerenciadas, exclusões e assim por diante) existem em uma política de WAF. Se você tiver um WAF existente, essas configurações ainda poderão existir na configuração do WAF. Para obter mais informações sobre como mover para a nova política de WAF, [migre a configuração do WAF para uma política do WAF](./migrate-policy.md). 


## <a name="next-steps"></a>Próximas etapas

- [Crie políticas por site e por URI usando Azure PowerShell](per-site-policies.md).