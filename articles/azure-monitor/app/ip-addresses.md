---
title: IP-адреса, используемые Application Insights и Log Analytics | Документация Майкрософт
description: Исключения брандмауэра сервера, требуемые для Application Insights
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 44d989f8-bae9-40ff-bfd5-8343d3e59358
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: mbullwin
ms.openlocfilehash: 3d2aefa7040ca2082e0e584d250d99f1ca00edf7
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326427"
---
# <a name="ip-addresses-used-by-application-insights-and-log-analytics"></a>IP-адреса, используемые Application Insights и Log Analytics
Служба [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) использует несколько IP-адресов. Вам могут понадобиться эти адреса, если отслеживаемое приложение расположено за брандмауэром.

> [!NOTE]
> Эти адреса статические, но время от времени мы можем их менять. Весь трафик Application Insights представляет исходящий трафик, за исключением сценариев с отслеживанием доступности и веб-перехватчиками, для которых требуются правила брандмауэра для входящего трафика.
> 
> 

> [!TIP]
> Подпишитесь на эту страницу как на RSS-канал, добавив https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/azure-monitor/app/ip-addresses.md.atom в используемое средство чтения RSS-каналов или ATOM, чтобы получать уведомления о последних изменениях.
> 
> 

## <a name="outgoing-ports"></a>Порты для исходящего трафика
Чтобы разрешить пакету SDK службы Application Insights и/или монитору состояния отправлять данные на портал, в брандмауэре сервера нужно открыть несколько портов для исходящего трафика.

| Цель | URL | IP-адрес | порты; |
| --- | --- | --- | --- |
| Телеметрия |dc.services.visualstudio.com<br/>dc.applicationinsights.microsoft.com |40.114.241.141<br/>104.45.136.42<br/>40.84.189.107<br/>168.63.242.221<br/>52.167.221.184<br/>52.169.64.244<br/>40.85.218.175<br/>104.211.92.54<br/>52.175.198.74<br/>51.140.6.23<br/>40.71.12.231<br/>13.69.65.22<br/>13.78.108.165<br/>13.70.72.233<br/>20.44.8.7<br/>13.86.218.248<br/>40.79.138.41<br/>52.231.18.241<br/>13.75.38.7<br/>102.133.162.117<br/>40.73.171.20<br/>102.133.155.50<br/>52.162.110.67 | 443 |
| Динамический поток метрик |rt.services.visualstudio.com<br/>rt.applicationinsights.microsoft.com |23.96.28.38<br/>13.92.40.198 |443 |

## <a name="status-monitor"></a>Монитор состояния
Настройка монитора состояния (требуется только для внесения изменений).

| Цель | URL | IP-адрес | порты; |
| --- | --- | --- | --- |
| Конфигурация |`management.core.windows.net` | |`443` |
| Конфигурация |`management.azure.com` | |`443` |
| Конфигурация |`login.windows.net` | |`443` |
| Конфигурация |`login.microsoftonline.com` | |`443` |
| Конфигурация |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| Конфигурация |`auth.gfx.ms` | |`443` |
| Конфигурация |`login.live.com` | |`443` |
| Установка | `globalcdn.nuget.org`, `packages.nuget.org` ,`api.nuget.org/v3/index.json` `nuget.org`, `api.nuget.org`,`dc.services.vsallin.net` | |`443` |

## <a name="availability-tests"></a>Тесты доступности
Ниже приведен список адресов, которые используются для [проверки доступности веб-сайтов](../../azure-monitor/app/monitor-web-app-availability.md) . Если вам нужно запустить веб-тесты в приложении, а веб-сервер обслуживает только определенные клиенты, следует разрешить входящий трафик от наших серверов тестирования доступности.

Откройте порты 80 (HTTP) и 443 (HTTPS) для входящего трафика с этих адресов (IP-адреса сгруппированы по расположению):

```
Australia East
20.40.124.176/28
20.40.124.240/28
20.40.125.80/28

Brazil South
191.233.26.176/28
191.233.26.128/28
191.233.26.64/28

France Central (Formerly France South)
20.40.129.96/28
20.40.129.112/28
20.40.129.128/28
20.40.129.144/28

France Central
20.40.129.32/28
20.40.129.48/28
20.40.129.64/28
20.40.129.80/28

East Asia
52.229.216.48/28
52.229.216.64/28
52.229.216.80/28

North Europe
52.158.28.64/28
52.158.28.80/28
52.158.28.96/28
52.158.28.112/28

Japan East
52.140.232.160/28
52.140.232.176/28
52.140.232.192/28

West Europe
51.144.56.96/28
51.144.56.112/28
51.144.56.128/28
51.144.56.144/28
51.144.56.160/28
51.144.56.176/28

UK South
51.105.9.128/28
51.105.9.144/28
51.105.9.160/28

UK West
20.40.104.96/28
20.40.104.112/28
20.40.104.128/28
20.40.104.144/28

Southeast Asia
52.139.250.96/28
52.139.250.112/28
52.139.250.128/28
52.139.250.144/28

West US
40.91.82.48/28
40.91.82.64/28
40.91.82.80/28
40.91.82.96/28
40.91.82.112/28
40.91.82.128/28

Central US
13.86.97.224/28
13.86.97.240/28
13.86.98.48/28
13.86.98.0/28
13.86.98.16/28
13.86.98.64/28

North Central US
23.100.224.16/28
23.100.224.32/28
23.100.224.48/28
23.100.224.64/28
23.100.224.80/28
23.100.224.96/28
23.100.224.112/28
23.100.225.0/28

South Central US
20.45.5.160/28
20.45.5.176/28
20.45.5.192/28
20.45.5.208/28
20.45.5.224/28
20.45.5.240/28

East US
20.42.35.32/28
20.42.35.64/28
20.42.35.80/28
20.42.35.96/28
20.42.35.112/28
20.42.35.128/28

```  

## <a name="application-insights-api"></a>API Application Insights
| Цель | URI | IP-адрес | порты; |
| --- | --- | --- | --- |
| API |api.applicationinsights.io<br/>api1.applicationinsights.io<br/>api2.applicationinsights.io<br/>api3.applicationinsights.io<br/>api4.applicationinsights.io<br/>api5.applicationinsights.io |23.96.58.253<br/>13.78.151.158<br/>40.74.59.40<br/>40.70.42.246<br/>40.117.198.0<br/>137.116.226.91<br/>52.163.88.44<br/>52.189.210.240<br/>13.77.201.34<br/>13.78.149.206<br/>52.232.28.146<br/>52.175.241.170<br/>20.36.36.66<br/>52.147.29.101<br/>40.115.155.252<br/>20.188.34.152<br/>52.141.32.103 |80, 443 |
| Документация по API |dev.applicationinsights.io<br/>dev.applicationinsights.microsoft.com<br/>dev.aisvc.visualstudio.com<br/>www.applicationinsights.io<br/>www.applicationinsights.microsoft.com<br/>www.aisvc.visualstudio.com |23.96.58.253<br/>13.78.151.158<br/>40.74.59.40<br/>40.70.42.246<br/>40.117.198.0<br/>137.116.226.91<br/>52.163.88.44<br/>52.189.210.240<br/>13.77.201.34<br/>13.78.149.206<br/>52.232.28.146<br/>52.175.241.170<br/>20.36.36.66<br/>52.147.29.101<br/>40.115.155.252<br/>20.188.34.152<br/>52.141.32.103 |80, 443 |
| Расширение "заметки конвейера Azure" |aigs1.aisvc.visualstudio.com |Динамический|443 |

## <a name="log-analytics-api"></a>API Log Analytics

| Цель | URI | IP-адрес | порты; |
| --- | --- | --- | --- |
| API |api.loganalytics.io<br/>*.api.loganalytics.io |23.96.58.253<br/>13.78.151.158<br/>40.74.59.40<br/>40.70.42.246<br/>40.117.198.0<br/>137.116.226.91<br/>52.163.88.44<br/>52.189.210.240<br/>13.77.201.34<br/>13.78.149.206<br/>52.232.28.146<br/>52.175.241.170<br/>20.36.36.66<br/>52.147.29.101<br/>40.115.155.252<br/>20.188.34.152<br/>52.141.32.103 |80, 443 |
| Документация по API |dev.loganalytics.io<br/>docs.loganalytics.io<br/>www.loganalytics.io |23.96.58.253<br/>13.78.151.158<br/>40.74.59.40<br/>40.70.42.246<br/>40.117.198.0<br/>137.116.226.91<br/>52.163.88.44<br/>52.189.210.240<br/>13.77.201.34<br/>13.78.149.206<br/>52.232.28.146<br/>52.175.241.170<br/>20.36.36.66<br/>52.147.29.101<br/>40.115.155.252<br/>20.188.34.152<br/>52.141.32.103 |80, 443 |

## <a name="application-insights-analytics"></a>Аналитика Application Insights

| Цель | URI | IP-адрес | порты; |
| --- | --- | --- | --- |
| Портал аналитики | analytics.applicationinsights.io | Динамический | 80, 443 |
| CDN | applicationanalytics.azureedge.net | Динамический | 80, 443 |
| Мультимедиа CDN | applicationanalyticsmedia.azureedge.net | Динамический | 80, 443 |

Примечание. *Домен applicationinsights.io принадлежит команде Application Insights.

## <a name="log-analytics-portal"></a>Портал Log Analytics

| Цель | URI | IP-адрес | порты; |
| --- | --- | --- | --- |
| Портал | portal.loganalytics.io | Динамический | 80, 443 |
| CDN | applicationanalytics.azureedge.net | Динамический | 80, 443 |

Примечание. Домен *.loganalytics.io принадлежит команде Log Analytics.

## <a name="application-insights-azure-portal-extension"></a>Расширение портала Azure для Application Insights

| Цель | URI | IP-адрес | порты; |
| --- | --- | --- | --- |
| Расширение для Application Insights | stamp2.app.insightsportal.visualstudio.com | Динамический | 80, 443 |
| Расширение CDN для Application Insights | insightsportal-prod2-cdn.aisvc.visualstudio.com<br/>insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com<br/>insightsportal-cdn-aimon.applicationinsights.io | Динамический | 80, 443 |

## <a name="application-insights-sdks"></a>Пакеты средств разработки Application Insights

| Цель | URI | IP-адрес | порты; |
| --- | --- | --- | --- |
| Пакеты средств разработки CDN JS Application Insights | az416426.vo.msecnd.net | Динамический | 80, 443 |
| Пакеты средств разработки Java Application Insights | aijavasdk.blob.core.windows.net | Динамический | 80, 443 |

## <a name="alert-webhooks"></a>Веб-перехватчик оповещений

| Цель | IP-адрес | порты;
| --- | --- | --- |
| Оповещение | 23.96.11.4 | 443 |

## <a name="profiler"></a>Profiler

| Цель | URI | IP-адрес | порты; |
| --- | --- | --- | --- |
| Агент | agent.azureserviceprofiler.net<br/>*.agent.azureserviceprofiler.net | 20.190.60.38<br/>20.190.60.32<br/>52.173.196.230<br/>52.173.196.209<br/>23.102.44.211<br/>23.102.45.216<br/>13.69.51.218<br/>13.69.51.175<br/>138.91.32.98<br/>138.91.37.93<br/>40.121.61.208<br/>40.121.57.2<br/>51.140.60.235<br/>51.140.180.52<br/>52.138.31.112<br/>52.138.31.127<br/>104.211.90.234<br/>104.211.91.254<br/>13.70.124.27<br/>13.75.195.15<br/>52.185.132.101<br/>52.185.132.170<br/>20.188.36.28<br/>40.89.153.171<br/>52.141.22.239<br/>52.141.22.149<br/>102.133.162.233<br/>102.133.161.73 | 443
| Портал | gateway.azureserviceprofiler.net | Динамический | 443
| Хранилище | *.core.windows.net | Динамический | 443

## <a name="snapshot-debugger"></a>Snapshot Debugger

> [!NOTE]
> Profiler и Snapshot Debugger используют один и тот же набор IP-адресов.

| Цель | URI | IP-адрес | порты; |
| --- | --- | --- | --- |
| Агент | ppe.azureserviceprofiler.net<br/>*.ppe.azureserviceprofiler.net | 20.190.60.38<br/>20.190.60.32<br/>52.173.196.230<br/>52.173.196.209<br/>23.102.44.211<br/>23.102.45.216<br/>13.69.51.218<br/>13.69.51.175<br/>138.91.32.98<br/>138.91.37.93<br/>40.121.61.208<br/>40.121.57.2<br/>51.140.60.235<br/>51.140.180.52<br/>52.138.31.112<br/>52.138.31.127<br/>104.211.90.234<br/>104.211.91.254<br/>13.70.124.27<br/>13.75.195.15<br/>52.185.132.101<br/>52.185.132.170<br/>20.188.36.28<br/>40.89.153.171<br/>52.141.22.239<br/>52.141.22.149<br/>102.133.162.233<br/>102.133.161.73 | 443
| Портал | ppe.gateway.azureserviceprofiler.net | Динамический | 443
| Хранилище | *.core.windows.net | Динамический | 443
