---
title: 'Tutorial: integração de SSO (logon único) do Azure Active Directory com CyberSolutions CYBERMAILΣ | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o CyberSolutions CYBERMAILΣ.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/09/2020
ms.author: jeedes
ms.openlocfilehash: 41c0bdf3eaac1a78d4cdef09ba418155a5a0c919
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92455022"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cybersolutions-cybermail"></a>Tutorial: integração de SSO (logon único) do Azure Active Directory com CyberSolutions CYBERMAILΣ

Neste tutorial, você aprenderá como integrar o CyberSolutions CYBERMAILΣ ao Azure AD (Azure Active Directory). Ao integrar o CyberSolutions CYBERMAILΣ com o Azure AD, você pode:

* Controlar quem tem acesso ao CyberSolutions CYBERMAILΣ no Azure AD.
* Permitir que seus usuários façam login automaticamente no CyberSolutions CYBERMAILΣ com contas do Azure AD deles.
* Gerenciar suas contas em um local central: o portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura habilitada para SSO do CyberSolutions CYBERMAILΣ.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O CyberSolutions CYBERMAILΣ dá suporte ao SSO iniciado por **SP e IDP**

## <a name="adding-cybersolutions-cybermail-from-the-gallery"></a>Adicionar o CyberSolutions CYBERMAILΣ da galeria

Para configurar a integração do CyberSolutions CYBERMAILΣ no Azure AD, você precisa adicionar o CyberSolutions CYBERMAILΣ da galeria à sua lista de aplicativos SaaS gerenciados.

1. Entre no portal do Azure usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar da galeria** , digite **CyberSolutions CYBERMAILΣ** na caixa de pesquisa.
1. Selecione **CyberSolutions CYBERMAILΣ** no painel de resultados e adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.


## <a name="configure-and-test-azure-ad-sso-for-cybersolutions-cybermail"></a>Configurar e testar o SSO do Azure AD para CyberSolutions CYBERMAILΣ

Configure e teste o SSO do Azure AD com o CyberSolutions CYBERMAILΣ usando um usuário de teste chamado **B.Fernandes**. Para que o SSO funcione, você precisa estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no CyberSolutions CYBERMAILΣ.

Para configurar e testar o SSO do Azure AD com o CyberSolutions CYBERMAILΣ, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do CyberSolutions CYBERMAIL](#configure-cybersolutions-cybermail-sso)** – para definir as configurações de logon único no lado do aplicativo.
    1. **[Criar usuário de teste do CyberSolutions CYBERMAIL](#create-cybersolutions-cybermail-test-user)** – para ter um equivalente de B.Fernandes no CyberSolutions CYBERMAILΣ que esteja vinculado à representação do usuário no Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No portal do Azure, na página de integração do aplicativo **CyberSolutions CYBERMAILΣ** , encontre a seção **Gerenciar** e selecione **logon único**.
1. Na página **Selecionar um método de logon único** , escolha **SAML**.
1. Na página **Configurar o logon único com o SAML** , clique no ícone de edição/caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na seção **Configuração Básica do SAML** , caso deseje configurar o aplicativo no modo iniciado por **IDP** , digite os valores dos seguintes campos:

    a. No **identificador** caixa de texto, digite uma URL usando o seguinte padrão: `https://<SUBDOMAIN>.cybercloud.jp/saml/module.php/saml/sp/metadata.php/m2k_generic_sp`

    b. No **URL de resposta** caixa de texto, digite uma URL usando o seguinte padrão: `https://<SUBDOMAIN>.cybercloud.jp/cgi-bin/saml_login/saml2-acs/m2k_generic_sp`

1. Clique em **Definir URLs adicionais** e execute o passo seguinte se quiser configurar a aplicação no modo **SP** iniciado:

    Na caixa de texto **URL de logon** , digite um URL usando o seguinte padrão: `https://<SUBDOMAIN>.cybercloud.jp`

    > [!NOTE]
    > Esses valores não são reais. Atualize esses valores com o Identificador, a URL de Resposta e a URL de Logon reais. Entre em contato com a [Equipe de suporte ao cliente do CyberSolutions CYBERMAILΣ](mailto:tech@cybersolutions.co.jp) para obter esses valores. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

1. Na página **Configurar o logon único com o SAML** , na seção **Certificado de Autenticação SAML** , localize **XML de Metadados de Federação** e selecione **Baixar** para baixar o certificado e salvá-lo no computador.

    ![O link de download do Certificado](common/metadataxml.png)

1. Na seção **Configurar CyberSolutions CYBERMAILΣ** , copie as URLs apropriadas com base em suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará um usuário de teste no portal do Azure chamado B.Fernandes.

1. No painel esquerdo do portal do Azure, escolha **Azure Active Directory** , **Usuários** e, em seguida, **Todos os usuários**.
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades do **Usuário** , siga estas etapas:
   1. No campo **Nome** , insira `B.Simon`.  
   1. No campo **Nome de usuário** , insira username@companydomain.extension. Por exemplo, `B.Simon@contoso.com`.
   1. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa **Senha**.
   1. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure, concedendo acesso ao CyberSolutions CYBERMAILΣ.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, selecione **CyberSolutions CYBERMAILΣ**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.
1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.
1. Na caixa de diálogo **Usuários e grupos** , selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando que uma função seja atribuída aos usuários, escolha-a na lista suspensa **Selecionar uma função**. Se nenhuma função tiver sido configurada para esse aplicativo, você verá a função "Acesso Padrão" selecionada.
1. Na caixa de diálogo **Adicionar atribuição** , clique no botão **Atribuir**.

## <a name="configure-cybersolutions-cybermail-sso"></a>Configurar SSO do CyberSolutions CYBERMAIL

Para configurar o logon único no lado do **CyberSolutions CYBERMAILΣ** , você precisa enviar o **XML de Metadados de Federação** baixado e as URLs copiadas adequadas do portal do Azure para a equipe de suporte do [CyberSolutions CYBERMAILΣ](mailto:tech@cybersolutions.co.jp). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-cybersolutions-cybermail-test-user"></a>Criar usuário de teste do CyberSolutions CYBERMAIL

Nesta seção, você cria um usuário chamado Brenda Fernandes no CyberSolutions CYBERMAILΣ. Trabalhe com a [equipe de suporte do CyberSolutions CYBERMAILΣ](mailto:tech@cybersolutions.co.jp) para adicionar os usuários na plataforma do Cyber Solutions CYBERMAILΣ. Os usuários devem ser criados e ativados antes de usar o logon único.

## <a name="test-sso"></a>Testar o SSO 

Nesta seção, você testará a configuração de logon único do Azure AD com as opções a seguir. 

#### <a name="sp-initiated"></a>Iniciado por SP:

* Clique em **Testar este aplicativo** no portal do Azure. Isso redirecionará para a URL de logon do CyberSolutions CYBERMAILΣ, na qual você poderá iniciar o fluxo de logon.  

* Acesse a URL de logon do CyberSolutions CYBERMAILΣ diretamente e inicie o fluxo de logon nela.

#### <a name="idp-initiated"></a>Iniciado por IdP:

* Clique em **Testar este aplicativo** no portal do Azure, e você entrará automaticamente no CyberSolutions CYBERMAILΣ para o qual configurou o SSO 

Use também o Painel de Acesso da Microsoft para testar o aplicativo em qualquer modo. Ao clicar no bloco CyberSolutions CYBERMAILΣ no Painel de Acesso, se estiver configurado no modo SP, você será redirecionado para a página de logon do aplicativo para iniciar o fluxo de logon e, se estiver configurado no modo IdP, você será conectado automaticamente ao CyberSolutions CYBERMAILΣ para o qual configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Próximas etapas

Depois de configurar oCyberSolutions CYBERMAILΣ, você poderá impor o controle de sessão, que fornece proteção contra exfiltração e infiltração dos dados confidenciais da sua organização em tempo real. O controle da sessão é estendido do acesso condicional. [Saiba como impor o controle de sessão com o Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).