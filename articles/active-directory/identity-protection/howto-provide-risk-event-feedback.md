---
title: Предоставьте отзыв о событиях риска в защита идентификации Azure AD Azure Active Directory
description: Как и зачем предоставлять отзыв о событиях риска защиты идентификации.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 07/19/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6bd0984a78860192f507323491952e895c8de8bf
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68370204"
---
# <a name="how-to-give-risk-feedback-in-azure-ad-identity-protection"></a>Как Предоставление отзывов о рисках в защита идентификации Azure AD

Защита идентификации Azure AD позволяет отправить отзыв о своей оценке рисков. В следующем документе перечислены сценарии, в которых вы хотите отправить отзыв о оценке рисков защита идентификации Azure AD и о том, как мы его включили.

## <a name="what-is-a-detection"></a>Что такое обнаружение?

Обнаружение защиты идентификации — это индикатор подозрительных действий с точки зрения риска идентификации. Эти подозрительные действия называются событиями риска. Эти обнаружения на основе удостоверений могут основываться на эвристике, машинном обучении или в продуктах партнеров. Эти обнаружения используются для определения риска при входе и риска для пользователей,

* Риск для пользователей представляет вероятность компрометации удостоверения.
* Риск входа представляет вероятность компрометации входа в систему (например, если вход не разрешен владельцем удостоверения).

## <a name="why-should-i-give-risk-feedback-to-azure-ads-risk-assessments"></a>Зачем предоставлять отзыв о рисках для оценки рисков Azure AD? 

Существует несколько причин, по которым необходимо предоставить отзыв о рисках Azure AD:

- Обнаружено неверное средство **оценки рисков для пользователя или входа Azure AD**. Например, вход, показанный в отчете "рискованные входы", был неопасным, и все обнаружения на этом входе были ложными положительными.
- **Вы проверили правильность оценки риска для пользователя или входа Azure AD**. Например, вход, показанный в отчете "рискованные входы", действительно является вредоносным, и вы хотите, чтобы Azure AD знал, что все обнаружения на этом входе были настоящими положительными.
- Вы исправляете **риск для этого пользователя за пределами Защита идентификации Azure AD** и хотите обновить уровень риска пользователя.

## <a name="how-does-azure-ad-use-my-risk-feedback"></a>Как Azure AD использует отзыв о рисках?

Azure AD использует ваши отзывы для обновления риска базового пользователя и (или) входа и точности этих событий. Этот отзыв помогает защитить конечного пользователя. Например, после подтверждения того, что вход в систему будет скомпрометирован, Azure AD немедленно увеличит риск и совокупное значение риска пользователя, а также статистический показатель входа (не риск в реальном времени). Если этот пользователь включен в политику риска пользователя для принудительного сброса паролей пользователями с высоким уровнем риска, он автоматически исправит себя при следующем входе в систему.

## <a name="how-should-i-give-risk-feedback-and-what-happens-under-the-hood"></a>Как следует предоставлять отзывы о рисках и что происходит внутри нее?

Ниже приведены сценарии и механизмы, позволяющие предоставить отзыв о рисках для Azure AD.

| Сценарий | Как дать отзыв? | Что происходит внутри? | Примечания |
| --- | --- | --- | --- |
| **Вход не скомпрометирован (ложный положительный результат)** <br> В отчете "рискованные входы" показан вход в систему [состояние риска = под угрозой], но этот вход не был скомпрометирован. | Выберите вход и щелкните "подтвердить безопасность входа". | Azure AD переместит составной риск входа в None [состояние риска = подтверждено безопасно; Уровень риска (aggregate) =-] и отменяет влияние на риск пользователя. | В настоящее время параметр "подтверждение безопасного входа" доступен только в отчете "рискованные входы". |
| **При входе скомпрометировано (истинный положительный результат)** <br> В отчете "рискованные входы" показан вход в систему [состояние риска = под угрозой] с низким риском [уровень риска (агрегатная) = низкая], и этот вход действительно скомпрометирован. | Выберите вход и щелкните "подтвердить несанкционированный вход". | Azure AD переместит статистический показатель входа в систему, и пользователь подвергается высокой опасности [состояние риска = подтверждено скомпрометировано; Уровень риска = высокий]. | В настоящее время параметр "подтвердить несанкционированный вход" доступен только в отчете "рискованные входы". |
| **Скомпрометированный пользователь (истинный положительный результат)** <br> В отчете "рискованные пользователи" отображается пользователь с риском [состояние риска = под угрозой] с низким риском [уровень риска = низкий], и этот пользователь действительно скомпрометирован. | Выберите пользователя и нажмите кнопку "подтвердить безопасность пользователя". | Azure AD переместит риск для пользователя в высокий уровень [состояние риска = подтверждено скомпрометировано; Уровень риска = высокий] и добавит новое обнаружение "администратор подтвердил" скомпрометированный пользователь ". | В настоящее время параметр "подтверждать безопасность пользователя" доступен только в отчете "рискованные пользователи". <br> Обнаружение "администратор подтвердил безопасность пользователя" отображается на вкладке "события риска, не связанные с входом" в отчете "рискованные пользователи". |
| **Пользователь исправляется за пределами защита идентификации Azure AD (true положительный и исправленный)** <br> В отчете "рискованные пользователи" отображается пользователь с риском, и он затем исправлен пользователем за пределами защита идентификации Azure AD. | 1. Выберите пользователя и нажмите кнопку "подтвердить безопасность пользователя". (Этот процесс подтверждает в Azure AD, что пользователь действительно скомпрометирован.) <br> 2. Дождитесь, пока "уровень риска" пользователя переходит в режим "высокий". (На этот раз Azure AD предпримет необходимое время на отзыв о подсистеме риска.) <br> 3. Выберите пользователя и нажмите кнопку "закрыть риск для пользователя". (Этот процесс подтверждает в Azure AD, что пользователь больше не скомпрометирован.) |  Azure AD перемещает пользователя в состояние "нет". Уровень риска =-] и закрывает риск для всех существующих входов, имеющих активный риск. | Нажатие кнопки "закрыть риск пользователя" приведет к закрытию всех рисков для пользователя и прошлых входов. Это действие необратимо. |
| **Пользователь не скомпрометирован (ложный положительный результат)** <br> Отчет "рискованные пользователи" показывает пользователя с риском, но он не скомпрометирован. | Выберите пользователя и нажмите кнопку "закрыть риск для пользователя". (Этот процесс подтверждает в Azure AD, что пользователь не скомпрометирован.) | Azure AD перемещает пользователя в состояние "нет". Уровень риска =-]. | Нажатие кнопки "закрыть риск пользователя" приведет к закрытию всех рисков для пользователя и прошлых входов. Это действие необратимо. |
| Я хочу закрыть риск для пользователя, но не уверен, что он скомпрометирован или является надежным. | Выберите пользователя и нажмите кнопку "закрыть риск для пользователя". (Этот процесс подтверждает в Azure AD, что пользователь больше не скомпрометирован.) | Azure AD перемещает пользователя в состояние "нет". Уровень риска =-]. | Нажатие кнопки "закрыть риск пользователя" приведет к закрытию всех рисков для пользователя и прошлых входов. Это действие необратимо. Рекомендуется исправить пользователя, щелкнув "Сброс пароля" или запросите у пользователя безопасный сброс или изменение учетных данных. |

Отзывы о событиях риска для пользователей в защите идентификации обрабатываются в автономном режиме, а обновление может занять некоторое время. В столбце состояние обработки рисков будет содержаться текущее состояние обработки отзывов.

![Состояние обработки риска для отчета о рискованных пользователях](./media/howto-provide-risk-event-feedback/risky-users-provide-feedback.png)

## <a name="next-steps"></a>Следующие шаги

[Справочник по событиям риска Защита идентификации Azure Active Directory](risk-events-reference.md)