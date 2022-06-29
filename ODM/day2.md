

# **3.1.1.3.2.2** StudyEventRef

- 연구의 특정 버전 내에서 발생하는 StudyEventRef에 대한 참조
- StudyEventRefs list : 연구 내에서 발생할 수 있는 연구 StudyEvent유형을 식별. 프로토콜 내의 StudyEventRefs에는 중복된 OrderNumbers나 StudyEventOIDs가 없어야 합니다.
    
    Contained in : Protocol
    
    ## Attributes
    
    | Attributes | type | description |
    | ----- | --- | -------- |
    | StudyEventOID | oidref | StudyEventDef의 OID를 참조한다. |
    | OrderNumber | integer | StudyEventDefs의 목록이 사용자에게 표시될 때마다 사용할 수 있도록 StudyEventDefs에 대한 순서를 제공합니다. event 일정, 작업시간, 데이터 정확성을 의미하는 것은 아니다. |
    | Mandatory | Yes \ No |  Yes일때, MetaDataVersion을 포함하는 임상 데이터가 Study event 인스턴스가 없으면 불완전하다는 것을 나타낸다. 불완전한 ODM 임상 데이터 파일은 연구 검토 및 분석하기에 불완전한 것으로 볼 수 있다.  |
    | CollectionExceptionConditionOID | oidref | Study event에 대한 데이터를 수집하면 안되는 상황을 설명한다. ConditionDef를 참조한다. |
    
    ### 💚예제
    
    ```xml
    <Protocol>
    	<StudyEventRef StudyEventOID="SE.1" Mandatory="No"/>
    	<StudyEventRef StudyEventOID="SE.2" Mandatory="No"/>
    	<StudyEventRef StudyEventOID="SE.3" Mandatory="No"/>
    </Protocol>
    <StudyEventDef OID="SE.1" Name="Baseline (T0)" Repeating="No" Type="Common">
    	<Description>
    		<TranslatedText xml:lang="en">Baseline (T0)</TranslatedText>
                <TranslatedText xml:lang="de">Vorbefragung (T0)</TranslatedText>
       </Description>
       		<FormRef FormOID="F.1" Mandatory="No"/>
                <FormRef FormOID="F.2" Mandatory="No"/>
    </StudyEventDef>
    ```
    

# **3.1.1.3.4   FormDef**

- FormDef는 스터디에서 발생할 수 있는 양식 유형을 설명합니다.
    
    ## Body
    
    - Description : FormDef에 대한 설명
    - ItemGroupRef  : 아래 참고
    - ArchiveLayout : 폼에서 데이터를 수집할때 사용하는 화면의 레이아웃 사진. Study를 저장할때 유용하다.
    - Alias : 별칭
    
    Contained in : MetaDataVersion
    
    ## Attributes
    
    | Attributes | type | description |
    | ----- | --- | -------- |
    | OID | oid |  |
    | Name | name |  |
    | Repeating | Yes or No |  현재 종류의 폼이 현재 studyevent안에서 반복적으로 발생할 수 있다.</br> study event안에서 Formref를 작성할때 하위의 FormDef의 OID를 참조하는데, event안에서 반복적으로 FormOid를 참조할 경우 Yes, 아닐때는 No |
    
    ### 💚예제
    
    ```xml
    <FormDef OID="F.1" Name="Basis data" Repeating="No">
    	<Description>
    		<TranslatedText xml:lang="en">Basis data</TranslatedText>
    		<TranslatedText xml:lang="de">Basisdaten</TranslatedText>
    	</Description>
    	<ItemGroupRef ItemGroupOID="IG.1" Mandatory="No"/>
    	<ItemGroupRef ItemGroupOID="IG.2" Mandatory="No"/>
    </FormDef>
    <FormDef OID="F.2" Name="Medical history" Repeating="No">
    	<Description>
    		<TranslatedText xml:lang="en">Medical history</TranslatedText>
    		<TranslatedText xml:lang="de">Vorerkrankungen</TranslatedText>
    	</Description>
    	<ItemGroupRef ItemGroupOID="IG.3" Mandatory="No"/>
    	<ItemGroupRef ItemGroupOID="IG.4" Mandatory="No"/>
    </FormDef>
    ```
    

## **3.1.1.3.4.1   ItemGroupRef**

- 특정 FormDef 내에서 발생하는 ItemGroupDef에 대해 참조한다.
- ItemGroupRef의 목록은 Form의 유형 내에서 발생할 수 있는 ItemGroup의 유형을 식별한다.
- 하나의 FormDef 내의 ItemGroupRefs는 중복 ItemGroupOID나 OrderNumber가 없어야한다.

Contained in : FormDef

## Attributes

| Attributes | type | description |
| ----- | --- | -------- |
| ItemGroupOID | oidref | ItemGroupDef 참고 |
| OrderNumber | integer |  ItemGroupDefs의 목록이 사용자에게 표시될 때마다 사용할 수 있도록 ItemGroupDefs이나 FormDef에 대한 순서를 제공한다. event 일정, 시간 순서나 데이터 정확성에 관한 내용는 아니다.  |
| Mandatory | Yes \ No | Yes일때, Mandatory의 item Group 인스턴스가 없으면 이 유형의 인스턴스에 대한 임상 데이터가 불완전하다는 것을 나타냅니다. 그래서 불완전한 ODM 임상 데이터 파일은 연구 검토 및 분석 목적으로 불완전 한 것으로 간주될 수 있다.  |
| CollectionExceptionConditionOID | oidref | 주어졌을 때, ItemGroup에 대한 데이터를 수집해서는 안 되는 상황을 설명하는 ConditionDef를 참고한다. |

### 💚예제

```xml
<FormDef OID="F.5" Name="Placeholder" Repeating="No">
	<Description>
		<TranslatedText xml:lang="de">Formular noch zu benennen ...</TranslatedText>
		<TranslatedText xml:lang="en">Form to be named ...</TranslatedText>
	</Description>
	<ItemGroupRef ItemGroupOID="IG.8" Mandatory="No"/>
</FormDef>
```

## 💚****데이터 형식****

| format Name | Schema Datatype | Allowed String Pattern |
| ---- | --- | ------ |
| oidref | xs:string | any sequence of characters\</br>(minLength="1") |
| name | xs:string | any sequence of characters\</br>(minLength="1") |
| oid | xs:string | any sequence of characters\</br>(minLength="1") |

## **3.1.1.3.11 ConditionDef**

- **ConditionDef는 참, 거짓의 조건을 정의한다.**
- Description은 반드시 제공되어야하고 자유롭게 기록해야한다.ConditionDef의 표준 내용이다.(산문체 → 자유롭게 기록으로 의역)
- CondictionDef는 조건에 의해 정의된 상황, 즉 FormalExpression이 참으로 평가될 때 생략될 수 있는 study metadata 구성요소 내의 CollectionExceptionConditionOID에 의해 참조된다.
- FormalExpression :  참, 거짓으로 계산하는 기계판독이 가능한 표현을 반드시 포함해야한다. 각각 다른 Context속성을 가진 경우 여러개의 FormalExpression을 제공할 수 있어서 동일한 식이 여러 시스템에 적합한 형태로 표현될 수 있다.
- 앱이 FormalExpression을 해석할 수 없거나 boolean 데이터를 지원하지 않을때는, 참조 study metadata 구성 요소에 대한 데이터는 조건을 지정하지 않은 것처럼 수집되어야 한다.

Contained in : MetaDataVersion

## Body

- Description : ConditionDef의 대한 설명
- FormalExpression : True나 False로 사용. 3.1.1.3.11.1 FormalExpression 참고
- Alias : 별칭

## Attributes

| Attributes | type |
| --- | --- |
| OID | oid |
| Name | name |

### 💚예제

```xml
<ConditionDef OID="C.1" Name="CountryOfBirthNotOther">
	<Description>
		<TranslatedText xml:lang="en">CountryOfBirthNotOther</TranslatedText>
	</Description>
	<FormalExpression Context="OpenEDC">!(CountryOfBirth == "Other")</FormalExpression>
</ConditionDef>
<ConditionDef OID="C.2" Name="GenderNotFemale">
	<Description>
		<TranslatedText xml:lang="en">GenderNotFemale</TranslatedText>
	</Description>
	<FormalExpression Context="OpenEDC">!(Gender == "Female")</FormalExpressio
</ConditionDef>
```

## 3.1.1.3.11.1 FormalExpression

- ConditionDef 또는 RangeCheck에서 사용되는 FormalExpression은 True 또는 False로 평가해야 한다.
- 유형변환(Type Inputation)을 가지는 MethodDef 내에서 참조되는 FormalExpression은 Method를 사용하여 귀속되거나 계산될 수 있는 항목에 대한 올바른 DataType으로 평가해야 합니다.
- Computation or Transpose must evaluate to the correct DataType for an Item that may be imputed or computed using the Method.
- 바꾸거나(Transpose) 계산은 귀속되거나 계산할 수있는 메소드 항목을 올바른 데이터 형식인지 반드시 평가해야한다.
    
    ## Attributes
    
    | Attributes | type | description |
    | ---- | --- | -------- |
    | Context | text | FormalExpression 내용을 평가할 때 사용할 적절한 컴퓨터 언어를 제안하는 자유 형식 한정자(qualifier)입니다. |
