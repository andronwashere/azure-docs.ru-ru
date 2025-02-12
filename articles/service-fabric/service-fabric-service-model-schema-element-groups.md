---
title: Группы элементов схемы XML для модели службы Azure Service Fabric | Документация Майкрософт
description: Эта статья содержит сведения о группах элементов схемы XML для модели службы Service Fabric.
services: service-fabric
documentationcenter: na
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/10/2018
ms.author: atsenthi
ms.openlocfilehash: 26915380364c3f011a1f56d607a7e06ec28c81cf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60837578"
---
<!-- This article was generated by the Python script found in the service-fabric-service-model-schema.md file -->

# <a name="service-model-xml-schema-elements"></a>Элементы схемы XML для модели службы

## <a name="partitiondescriptiongroup-group"></a>Группа PartitionDescriptionGroup
Определяет схему секционирования для службы.

|Атрибут|Значение|
|---|---|
|Содержимое|3 элемента|

### <a name="xml-source"></a>Источник XML
```xml
<xs:group xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="PartitionDescriptionGroup">
        <xs:annotation>
            <xs:documentation>Defines the partitioning scheme for a service.</xs:documentation>
        </xs:annotation>
        <xs:choice>
            <xs:element name="SingletonPartition">
                <xs:annotation>
                    <xs:documentation>Declares that this service has only one partition.</xs:documentation>
                </xs:annotation>
                <xs:complexType/>
            </xs:element>
            <xs:element name="UniformInt64Partition">
                <xs:annotation>
                    <xs:documentation>Describes a uniform partitioning scheme based on Int64 keys.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:attribute name="PartitionCount" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>Total number of partitions (positive integer). Each partition is responsible for a non-overlapping subrange of the overall partition key range.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="LowKey" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>Inclusive low range of the partition key (long).</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="HighKey" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>Inclusive high range of the partition key (long).</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                </xs:complexType>
            </xs:element>
            <xs:element name="NamedPartition">
                <xs:annotation>
                    <xs:documentation>Describes a named partitioning scheme based on names for each partition.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:sequence maxOccurs="unbounded">
                        <xs:element name="Partition">
                            <xs:annotation>
                                <xs:documentation>Describes a partition by name.</xs:documentation>
                            </xs:annotation>
                            <xs:complexType>
                                <xs:attribute name="Name" use="required">
                                    <xs:annotation>
                                        <xs:documentation>The name of the partition</xs:documentation>
                                    </xs:annotation>
                                    <xs:simpleType>
                                        <xs:restriction base="xs:string">
                                            <xs:minLength value="1"/>
                                        </xs:restriction>
                                    </xs:simpleType>
                                </xs:attribute>
                            </xs:complexType>
                        </xs:element>
                    </xs:sequence>
                </xs:complexType>
            </xs:element>
        </xs:choice>
    </xs:group>
    
```
### <a name="content-element-details"></a>Сведения об элементе Content

#### <a name="singletonpartition"></a>SingletonPartition
Объявляет, что служба имеет только один раздел.

|Атрибут|Значение|
|---|---|
|name|SingletonPartition|

##### <a name="xml-source"></a>Источник XML
```xml
<xs:element xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="SingletonPartition">
                <xs:annotation>
                    <xs:documentation>Declares that this service has only one partition.</xs:documentation>
                </xs:annotation>
                <xs:complexType/>
            </xs:element>
            

```

#### <a name="uniformint64partition"></a>UniformInt64Partition
Описывает единую схему секционирования на основе ключей Int64.

|Атрибут|Значение|
|---|---|
|name|UniformInt64Partition|

##### <a name="xml-source"></a>Источник XML
```xml
<xs:element xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="UniformInt64Partition">
                <xs:annotation>
                    <xs:documentation>Describes a uniform partitioning scheme based on Int64 keys.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:attribute name="PartitionCount" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>Total number of partitions (positive integer). Each partition is responsible for a non-overlapping subrange of the overall partition key range.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="LowKey" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>Inclusive low range of the partition key (long).</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="HighKey" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>Inclusive high range of the partition key (long).</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                </xs:complexType>
            </xs:element>
            

```

#### <a name="namedpartition"></a>NamedPartition
Описывает схему секционирования по именам для каждого раздела.

|Атрибут|Значение|
|---|---|
|name|NamedPartition|

##### <a name="xml-source"></a>Источник XML
```xml
<xs:element xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="NamedPartition">
                <xs:annotation>
                    <xs:documentation>Describes a named partitioning scheme based on names for each partition.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:sequence maxOccurs="unbounded">
                        <xs:element name="Partition">
                            <xs:annotation>
                                <xs:documentation>Describes a partition by name.</xs:documentation>
                            </xs:annotation>
                            <xs:complexType>
                                <xs:attribute name="Name" use="required">
                                    <xs:annotation>
                                        <xs:documentation>The name of the partition</xs:documentation>
                                    </xs:annotation>
                                    <xs:simpleType>
                                        <xs:restriction base="xs:string">
                                            <xs:minLength value="1"/>
                                        </xs:restriction>
                                    </xs:simpleType>
                                </xs:attribute>
                            </xs:complexType>
                        </xs:element>
                    </xs:sequence>
                </xs:complexType>
            </xs:element>
        

```

## <a name="scalingpolicymechanismgroup-group"></a>Группа ScalingPolicyMechanismGroup
Определяет механизм масштабирования.

|Атрибут|Значение|
|---|---|
|Содержимое|2 элемента|

### <a name="xml-source"></a>Источник XML
```xml
<xs:group xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ScalingPolicyMechanismGroup">
        <xs:annotation>
            <xs:documentation>Defines the scaling mechanism.</xs:documentation>
        </xs:annotation>
        <xs:choice>
            <xs:element name="InstanceCountScalingMechanism" minOccurs="0" maxOccurs="1">
                <xs:annotation>
                    <xs:documentation>Defines a mechanism for increasing the instance count of a partition. This is for stateless services only.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:attribute name="MinInstanceCount" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The minimum number of instances.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="MaxInstanceCount" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The maximum number of instances..</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="ScaleIncrement" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The number of instances by which to perform the upshift/downshift.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                </xs:complexType>
            </xs:element>
            <xs:element name="AddRemoveIncrementalNamedPartitionScalingMechanism" minOccurs="0" maxOccurs="1">
                <xs:annotation>
                    <xs:documentation>Defines a mechanism for adding or removing named partitions.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:attribute name="MinPartitionCount" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The minimum number of partitions.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="MaxPartitionCount" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The maximum number of partitions.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="ScaleIncrement" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The number of partitions by which to perform the upshift/downshift.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                </xs:complexType>
            </xs:element>
        </xs:choice>
    </xs:group>
    
```
### <a name="content-element-details"></a>Сведения об элементе Content

#### <a name="instancecountscalingmechanism"></a>InstanceCountScalingMechanism
Определяет механизм для увеличения числа экземпляров секции. Только для служб без отслеживания состояния.

|Атрибут|Значение|
|---|---|
|name|InstanceCountScalingMechanism|
|minOccurs|0|
|maxOccurs|1|

##### <a name="xml-source"></a>Источник XML
```xml
<xs:element xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="InstanceCountScalingMechanism" minOccurs="0" maxOccurs="1">
                <xs:annotation>
                    <xs:documentation>Defines a mechanism for increasing the instance count of a partition. This is for stateless services only.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:attribute name="MinInstanceCount" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The minimum number of instances.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="MaxInstanceCount" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The maximum number of instances..</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="ScaleIncrement" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The number of instances by which to perform the upshift/downshift.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                </xs:complexType>
            </xs:element>
            

```

#### <a name="addremoveincrementalnamedpartitionscalingmechanism"></a>AddRemoveIncrementalNamedPartitionScalingMechanism
Определяет механизм для добавления или удаления именованных секций.

|Атрибут|Значение|
|---|---|
|name|AddRemoveIncrementalNamedPartitionScalingMechanism|
|minOccurs|0|
|maxOccurs|1|

##### <a name="xml-source"></a>Источник XML
```xml
<xs:element xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="AddRemoveIncrementalNamedPartitionScalingMechanism" minOccurs="0" maxOccurs="1">
                <xs:annotation>
                    <xs:documentation>Defines a mechanism for adding or removing named partitions.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:attribute name="MinPartitionCount" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The minimum number of partitions.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="MaxPartitionCount" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The maximum number of partitions.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="ScaleIncrement" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The number of partitions by which to perform the upshift/downshift.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                </xs:complexType>
            </xs:element>
        

```

## <a name="scalingpolicytriggergroup-group"></a>Группа ScalingPolicyTriggerGroup
Определяет триггер масштабирования.

|Атрибут|Значение|
|---|---|
|Содержимое|2 элемента|

### <a name="xml-source"></a>Источник XML
```xml
<xs:group xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="ScalingPolicyTriggerGroup">
        <xs:annotation>
            <xs:documentation>Defines the scaling trigger.</xs:documentation>
        </xs:annotation>
        <xs:choice>
            <xs:element name="AveragePartitionLoadScalingTrigger" minOccurs="0" maxOccurs="1">
                <xs:annotation>
                    <xs:documentation>Defines a scaling mechanism based on the average load of replicas of a partition.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:attribute name="MetricName" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The name of metric which should be used for scaling operations.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="LowerLoadThreshold" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The lower limit of the load value of a metric.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="UpperLoadThreshold" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The upper limit of the load value of a metric.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="ScaleIntervalInSeconds" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The time interval in seconds to be considered for scaling.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                </xs:complexType>
            </xs:element>
            <xs:element name="AverageServiceLoadScalingTrigger" minOccurs="0" maxOccurs="1">
                <xs:annotation>
                    <xs:documentation>Defines a scaling mechanism based on the average load of partitions of a service.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:attribute name="MetricName" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The name of metric which should be used for scaling operations.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="LowerLoadThreshold" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The lower limit of the load value of a metric.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="UpperLoadThreshold" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The upper limit of the load value of a metric.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="ScaleIntervalInSeconds" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The time interval in seconds to be considered for scaling.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="UseOnlyPrimaryLoad" type="xs:string" use="optional">
                        <xs:annotation>
                            <xs:documentation>Use load of primary replica as average load of partition.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                </xs:complexType>
            </xs:element>
        </xs:choice>
    </xs:group>
    
```
### <a name="content-element-details"></a>Сведения об элементе Content

#### <a name="averagepartitionloadscalingtrigger"></a>AveragePartitionLoadScalingTrigger
Определяет механизм масштабирования на основе средней нагрузки реплик секции.

|Атрибут|Значение|
|---|---|
|name|AveragePartitionLoadScalingTrigger|
|minOccurs|0|
|maxOccurs|1|

##### <a name="xml-source"></a>Источник XML
```xml
<xs:element xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="AveragePartitionLoadScalingTrigger" minOccurs="0" maxOccurs="1">
                <xs:annotation>
                    <xs:documentation>Defines a scaling mechanism based on the average load of replicas of a partition.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:attribute name="MetricName" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The name of metric which should be used for scaling operations.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="LowerLoadThreshold" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The lower limit of the load value of a metric.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="UpperLoadThreshold" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The upper limit of the load value of a metric.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="ScaleIntervalInSeconds" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The time interval in seconds to be considered for scaling.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                </xs:complexType>
            </xs:element>
            

```

#### <a name="averageserviceloadscalingtrigger"></a>AverageServiceLoadScalingTrigger
Определяет механизм масштабирования на основе средней нагрузки секций службы.

|Атрибут|Значение|
|---|---|
|name|AverageServiceLoadScalingTrigger|
|minOccurs|0|
|maxOccurs|1|

##### <a name="xml-source"></a>Источник XML
```xml
<xs:element xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="AverageServiceLoadScalingTrigger" minOccurs="0" maxOccurs="1">
                <xs:annotation>
                    <xs:documentation>Defines a scaling mechanism based on the average load of partitions of a service.</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:attribute name="MetricName" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The name of metric which should be used for scaling operations.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="LowerLoadThreshold" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The lower limit of the load value of a metric.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="UpperLoadThreshold" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The upper limit of the load value of a metric.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="ScaleIntervalInSeconds" type="xs:string" use="required">
                        <xs:annotation>
                            <xs:documentation>The time interval in seconds to be considered for scaling.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                    <xs:attribute name="UseOnlyPrimaryLoad" type="xs:string" use="optional">
                        <xs:annotation>
                            <xs:documentation>Use load of primary replica as average load of partition.</xs:documentation>
                        </xs:annotation>
                    </xs:attribute>
                </xs:complexType>
            </xs:element>
        

```

