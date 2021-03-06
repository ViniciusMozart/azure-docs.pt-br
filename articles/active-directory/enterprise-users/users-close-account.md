---
title: Fechar uma conta corporativa ou de estudante em uma organização não gerenciada do Azure AD
description: Como fechar sua conta corporativa ou de estudante em um Azure Active Directory não gerenciado.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.subservice: enterprise-users
ms.topic: how-to
ms.workload: identity
ms.date: 11/20/2020
ms.author: rolyon
ms.reviewer: ''
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 142143f96314539051ea44c00c6ff95cb2650566
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94650199"
---
# <a name="close-your-work-or-school-account-in-an-unmanaged-azure-ad-organization"></a>Fechar sua conta corporativa ou de estudante em uma organização não gerenciada do Azure AD

Se você for um usuário em uma organização não gerenciada do Azure Active Directory (Azure AD) e não precisar mais usar aplicativos dessa organização ou manter qualquer associação com ele, poderá fechar sua conta a qualquer momento. Uma organização não gerenciada não tem um administrador global. Os usuários em uma organização não gerenciada podem fechar suas contas por conta própria, sem precisar entrar em contato com um administrador.

Os usuários em uma organização não gerenciada geralmente são criados durante a inscrição de autoatendimento. Um exemplo pode ser um operador de informações em uma organização que se inscreve em um serviço gratuito. Para obter mais informações sobre a inscrição de autoatendimento, consulte [o que é inscrição de autoatendimento para Azure Active Directory?](directory-self-service-signup.md).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="before-you-begin"></a>Antes de começar

Antes de poder fechar sua conta, você deve confirmar os seguintes itens:

* Verifique se você é um usuário de uma organização não gerenciada do Azure AD. Você não poderá fechar sua conta se você pertencer a uma organização gerenciada. Se você pertencer a uma organização gerenciada e quiser fechar sua conta, deverá entrar em contato com o administrador. Para obter informações sobre como determinar se você pertence a uma organização não gerenciada, consulte [excluir o usuário de um locatário não gerenciado](/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant).

* Salve os dados que você deseja manter. Para obter informações sobre como enviar uma solicitação de exportação, consulte [acessando e exportando logs gerados pelo sistema para locatários não gerenciados](/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants).

> [!WARNING]
> Fechar sua conta é irreversível. Quando você fechar sua conta, todos os dados pessoais serão removidos. Você não terá mais acesso à sua conta e aos dados associados à sua conta.

## <a name="close-your-account"></a>Fechar sua conta

Para fechar uma conta corporativa ou de estudante não gerenciada, siga estas etapas:

1. Entre para [fechar sua conta](https://go.microsoft.com/fwlink/?linkid=873123), usando a conta que você deseja fechar.

1. Em **minhas solicitações de dados**, selecione **fechar conta**.

    ![Minhas solicitações de dados-fechar conta](./media/users-close-account/close-account.png)

1. Examine a mensagem de confirmação e selecione **Sim**.

    ![Minhas solicitações de dados-confirmar fechamento](./media/users-close-account/confirm-close.png)

## <a name="next-steps"></a>Próximas etapas

- [O que é inscrição por autoatendimento no Azure Active Directory?](directory-self-service-signup.md)
- [Excluir o usuário do locatário não gerenciado](/flow/gdpr-dsr-delete#delete-the-user-from-unmanaged-tenant)
- [Acessando e exportando logs gerados pelo sistema para locatários não gerenciados](/power-platform/admin/powerapps-gdpr-dsr-guide-systemlogs#accessing-and-exporting-system-generated-logs-for-unmanaged-tenants)