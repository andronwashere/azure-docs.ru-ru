---
title: Краткое руководство. Блокировка доступа в случае обнаружения угроз сеанса при помощи условного доступа Azure Active Directory | Документация Майкрософт
description: В этом учебнике описано, как настроить политику условного доступа Azure Active Directory (Azure AD) для блокировки входов в систему, связанных с рисками сеанса.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 12/14/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdbdd0253478200d39501444ae649b87b77e65a4
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509050"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-conditional-access"></a>Краткое руководство. Блокировка доступа в случае обнаружения угроз сеанса с помощью условного доступа Azure Active Directory  

Чтобы обеспечить защиту среды, может потребоваться блокировка входа подозрительных пользователей в систему. [Azure Active Directory (Azure AD) Identity Protection](../active-directory-identityprotection.md) анализирует каждый вход в систему и вычисляет вероятность того, что попытка входа не была выполнена законным владельцем учетной записи пользователя. Вероятность (низкая, средняя, высокая) указывается в виде вычисляемого значения, называемого [уровни риска при входе](conditions.md#sign-in-risk). Задавая условие риска при входе, можно настроить политику условного доступа так, что она будет реагировать на определенные уровни риска при входе в систему.

В этом учебнике показано, как настроить [политику условного доступа](../active-directory-conditional-access-azure-portal.md), чтобы блокировать вход в систему при обнаружении настроенного уровня риска при входе в систему.

![Создание политики](./media/app-sign-in-risk/1000.png)

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Для выполнения сценария в этом руководстве вам понадобится следующее.

- **Доступ к Azure AD Premium P2**. Хотя в Azure AD Premium P1 доступна функция условного доступа, все же вам необходим выпуск P2, так как для сценария, описанного в этом учебнике, требуется защита идентификации.
- **Защита идентификации**. Сценарий в этом руководстве требует включенной функции защиты идентификации. Если вы не знаете, как включить защиту идентификации Azure, см. раздел [Включение защиты идентификации Azure Active Directory](../identity-protection/enable.md).
- **Обозреватель Tor**. [Обозреватель Tor](https://www.torproject.org/projects/torbrowser.html.en) предназначен для сохранения конфиденциальности в Интернете. Служба защиты идентификации воспринимает вход в систему с помощью обозревателя Tor как входы с анонимных IP-адресов, которые имеют средний уровень риска. Дополнительные сведения см. в статье [События риска Azure Active Directory](../reports-monitoring/concept-risk-events.md).  
- **Тестовая учетная запись Alain Charon**. Сведения о создании тестовой учетной записи см. в разделе о [Add new users to Azure Active Directory](../fundamentals/add-users-azure-active-directory.md#add-a-new-user) (Добавление новых пользователей в Azure Active Directory).

## <a name="test-your-sign-in"></a>Проверка входа

Цель этого шага — убедиться, что тестовая учетная запись имеет доступ к клиенту с использованием обозревателя Tor.

**Чтобы проверить вход в систему, выполните следующие действия.**

1. Выполните вход в [портал Azure](https://portal.azure.com) как **Alain Charon**.
1. Выполните выход.

## <a name="create-your-conditional-access-policy"></a>Создание политики условного доступа

В этом сценарии используется вход в систему с помощью обозревателя Tor для создания обнаруженных событий риска — **входы с анонимных IP-адресов**. Уровень риска этого события средний. Для ответа на такое событие риска установите условие риска при входе на "Средний". В рабочей среде необходимо установить условие риска при входе на "Высокий" или "Средний и высокий".

В этом разделе показано, как создать необходимую политику условного доступа. В политике задайте следующие значения.

| Параметр | Значение |
| --- | --- |
| Пользователи и группы | Alain Charon  |
| Облачные приложения | Все облачные приложения |
| Риск при входе | Средний |
| Предоставление доступа | Заблокировать доступ |

![Создание политики](./media/app-sign-in-risk/130.png)

**Чтобы настроить политику условного доступа, выполните следующие действия:**

1. Войдите на [портал Azure](https://portal.azure.com) с правами глобального администратора, администратора безопасности или администратора условного доступа.
1. На портале Azure на панели навигации слева щелкните **Azure Active Directory**.

   ![Azure Active Directory](./media/app-sign-in-risk/02.png)

1. На странице **Azure Active Directory** в разделе **Безопасность** щелкните **Условный доступ**.

   ![Условный доступ](./media/app-sign-in-risk/03.png)

1. На странице **Условный доступ** на панели инструментов сверху нажмите кнопку **Добавить**.

   ![ИМЯ](./media/app-sign-in-risk/108.png)

1. На странице **Создать** в поле **Имя** введите **Блокировать доступ для среднего уровня риска**.

   ![ИМЯ](./media/app-sign-in-risk/104.png)

1. В разделе **Назначение** щелкните **Пользователи и группы**.

   ![Пользователи и группы](./media/app-sign-in-risk/06.png)

1. На странице **Пользователи и группы** выполните следующие действия.

   ![Условный доступ](./media/app-sign-in-risk/107.png)

   1. Щелкните **Выбор пользователей и групп**, а затем выберите **Пользователи и группы**.
   1. Нажмите кнопку **Выбрать**.
   1. На странице **Выбор** выберите **Alain Charon** и нажмите кнопку **Выбрать**.
   1. На странице **Пользователи и группы** нажмите кнопку **Готово**.
1. Щелкните **Облачные приложения**.

   ![Облачные приложения](./media/app-sign-in-risk/08.png)

1. На странице **Облачные приложения** выполните следующие действия.

   ![Условный доступ](./media/app-sign-in-risk/109.png)

   1. Нажмите **Все облачные приложения**.
   1. Нажмите кнопку **Done**(Готово).
1. Щелкните **Условия**.

   ![Элементы управления доступом](./media/app-sign-in-risk/19.png)

1. На странице **Условия**:

   ![Уровень риска при входе](./media/app-sign-in-risk/21.png)

   1. Щелкните **Риск при входе**.
   1. В **Настройке** нажмите **Да**.
   1. В поле "Уровень риска входа" выберите **Средний**.
   1. Нажмите кнопку **Выбрать**.
   1. На странице **Условия** нажмите **Готово**.
1. В разделе **Элементы управления доступом** щелкните **Предоставить**.

   ![Элементы управления доступом](./media/app-sign-in-risk/10.png)

1. На странице **Предоставление** выполните следующие действия.

   ![Условный доступ](./media/app-sign-in-risk/105.png)

   1. Выберите **Заблокировать доступ**.
   1. Нажмите кнопку **Выбрать**.
1. В разделе **Включение политики** щелкните **Вкл**.

   ![Включение политики](./media/app-sign-in-risk/18.png)

1. Нажмите кнопку **Создать**.

## <a name="evaluate-a-simulated-sign-in"></a>Оценка смоделированного входа

Теперь, когда вы настроили политику условного доступа, вероятно, вы захотите узнать, работает ли она так, как ожидалось. В качестве первого шага используйте инструмент **What If для политик условного доступа**, чтобы смоделировать вход тестового пользователя. При имитации оценивается влияние входа на политики и создается отчет об имитации.  

При запуске **средства политики "Что — если"** для этого сценария в список **Применяемые политики** должен быть внесен пункт **Блокировать доступ для среднего уровня риска**.

![Пользователь](./media/app-sign-in-risk/117.png)

**Чтобы оценить политику условного доступа, выполните следующие действия:**

1. На странице [Conditional Access — Policies](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) (Условный доступ — политики) в меню в верхней части щелкните **What If**.  

   ![What If](./media/app-sign-in-risk/14.png)

1. Нажмите кнопку **Пользователь** на странице **Пользователи**, выберите **Alan Charon** и нажмите **Выбрать**.

   ![Пользователь](./media/app-sign-in-risk/116.png)

1. В поле **риск при входе** выберите **Средний**.

   ![Пользователь](./media/app-sign-in-risk/119.png)

1. Щелкните **What If**.

## <a name="test-your-conditional-access-policy"></a>Проверка политики условного доступа

В предыдущем разделе вы узнали, как оценить смоделированный вход. Помимо имитации необходимо проверить политику условного доступа, чтобы убедиться, что она работает должным образом.

Чтобы протестировать политику, попробуйте войти в [портал Azure](https://portal.azure.com) как **Alan Charon** при помощи обозревателя Tor. Попытка входа должна быть заблокирована политикой условного доступа.

![Многофакторная Идентификация](./media/app-sign-in-risk/118.png)

## <a name="clean-up-resources"></a>Очистка ресурсов

Если тестовый пользователь, обозреватель Tor и политика условного доступа больше не нужны, удалите их:

- Сведения об удалении пользователя Azure AD см. в разделе об [удалении пользователей из Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- Чтобы удалить политику, выберите ее и нажмите кнопку **Удалить** на панели инструментов быстрого доступа.

   ![Многофакторная Идентификация](./media/app-sign-in-risk/33.png)

- Инструкции по удалению обозревателя Tor см. в разделе [Uninstalling](https://tb-manual.torproject.org/uninstalling/) (Удаление).

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [Обеспечение принятия условий использования](require-tou.md)
> [Require MFA for specific apps with Azure Active Directory conditional access](app-based-mfa.md) (Требование MFA для определенных приложений с использованием условного доступа Azure Active Directory)
