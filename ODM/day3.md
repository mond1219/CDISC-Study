## 3.1.1.3.6.1 Question

- 문서나 화면에 항목에 관한 데이터를 제공하라는 메세지가 표시, 사용자에게 제공하는 라벨이다.

    Contained in : ItemDef

    ## Body

    - TranslatedText : 질문, 안내 등 사람일 읽을 수 있는 문장들   

    ### 💚예제
    ```xml
    <Question>
        <TranslatedText xml:lang="en">What is your gender?</TranslatedText>
        <TranslatedText xml:lang="de">Welches Geschlecht haben Sie?</TranslatedText>        
    </Question>   
    ```

## 3.1.1.3.6.2   ExternalQuestion

- 외부에서 정의된 질문에 대한 참조   

    Contained in : ItemDef

    ## Attributes

    | Attributes | type | description |
    | ---- | --- | -------- |
    | Dictionary | text | 사전적 이름 |
    | Version | text | Externalquestion사전의 버전 지정자(작명자) |
    | Code | text | externalQuestion에서 특정 질문을 선택하는 코드 |

## 3.1.1.3.6.3   MeasurementUnitRef

- 측정 단위 정의의 대한 참조

    Contained in : ItemData, ItemDef, RangeCheck

    ## Attributes
    | Attributes | type | description |
    | ---- | --- | -------- |
    | MeasurementUnitOID | oidref | 측정단위 정의의 대한 참조 |

    ### 💚예제
    ```xml
    <MeasurementUnitRef MeasurementUnitOID="MU.5"/>
    ```

## 3.1.1.3.6.4   RangeCheck

- 항목의 범위 값에 대한 제약 조건을 정의한다.
- Itemdata 값이 유효하면 True, 잘못된 경우 False로 평가되는 식을 나타낸다.
- 식은 비교연산자(Comparator)와 CheckValue를 사용하거나 FormalExpression을 사용하여 지정한다.


    ### 비교연산자나 CheckValue를 사용한 RangeCheck
    
    - 비교연산자나 CheckValue를 사용한 RangeCheck는 치우친(한쪽으로) 제약 조건을 나타낸다.
    - 여러 RangeCheck를 사용하여 복잡한 제약조건을 지정할 수 있다.

        ex) 이상, 이하를 사용하려면 두개의 RangeCheck가 필요하다.
    
    - 제약조건
        - 실제 데이터 값이 제약 조건을 충족하지 못하면 거부(Hard조건)되거나, 경고(Soft조건)가 생긴다.

            #### 💙비교 연산자 

            | 기호 | 설명 |
            | :---: | :---: |
            | LT | 미만 |
            | LE | 이하 |
            | GT | 초과 |
            | GE | 이상 |
            | EQ | 동일 |
            | NE | 같지 않음 |

            #### 💙CheckValue   
        
            | 기호 | 설명 |
            | :---: | :---: |
            | IN | 목록 중 하나만 |
            | NOTIN | 목록 값에 존재하지 않는다. |

        - 측정 단위가 지정된 경우, 해당 항목 값에는 바꿀 수 있는 측정 단위가 기본적으로 있어야 한다. RangeCheck의 일부로 단위를 올바르게 바꿔야한다.
        - 측정 단위가 정해져 있지 않을때, 해당 항목 값은 측정단위가 기본적으로 없어야한다.


        ### 💚예제

        - value > 0

        ```xml
        ## GT : 초과 
        <RangeCheck Comparator="GT" >
            <CheckValue>0</CheckValue>
        </RangeCheck>
        ```

        - 18 <= value <= 65 

        ```xml
        ## GE : 이상 , LE : 이하
        <RangeCheck Comparator="GE" >
            <CheckValue>18</CheckValue>
        </RangeCheck>
        <RangeCheck Comparator="LE" >
            <CheckValue>65</CheckValue>
        </RangeCheck>
        ```

        - 1,5,7 중 하나일 때

        ```xml
        <RangeCheck Comparator="IN" >
            <CheckValue>1</CheckValue>
            <CheckValue>5</CheckValue>
            <CheckValue>7</CheckValue>
        </RangeCheck>
        ```    

    ### FormalExpression을 사용한 RangeCheck
    
    - FormalExpression을 사용하면 모든 범위를 나타낼 수 있다.   
    - FormalExpression 자체로 RangeCheck를 표현할 수 있기 때문에 비교연산자나 MeasurementUnitRef를 쓰면 안된다.
    - ItemData 요소의 값을 사용하고 식의 결과를 논리(boolean)값으로 반환한다.
    - 멀티 FormalExpression은 각가 다른 Context 속성을 가진 경우 제공될 수 있고, 이를 통해 동일한 식이 여러 시스템에 적합한 형식으로 표현될 수 있다.
    - 서로 다른 의미를 가진 여러개의 다른 식은 별도의 RangeCheck로 표시되어야 한다.

        ### 💚예제
        ```xml
        <ConditionDef OID="C.2" Name="GenderNotFemale">
            <Description>
                <TranslatedText xml:lang="en">GenderNotFemale</TranslatedText>
            </Description>
            <FormalExpression Context="OpenEDC">!(Gender == "Female")</FormalExpression>
        </ConditionDef>
        ``` 

## 3.1.1.3.6.4.1   CheckValue
- RangeCheck에 사용되는 비교값   
- CheckValue가 속한 ItemDef에 선언된 값의 type과 동일해야 한다.   

    Contain in : RangeCheck

    ### Body
    - value :

## 3.1.1.3.6.4.2 ErrorMessage
- RangeCheck를 위반할때 사용하는 에러 메세지

    Contained in : RangeCheck

    ### Body
    - TranslatedText : 질문, 안내 등 사람일 읽을 수 있는 문장들   

## 3.1.1.3.6.5 CodeListRef
- CodeList 정의 참조
- CodeList를 참조하는 데이터 타입과 포함된 ItemDef는 동일해야 한다.   

    Contained in : ItemDef

    ### Attributes

    | Attributes | type | description |
    | :---: | :---: | :---: |
    | CodeListOID | oidref | CodeList 정의를 참조한다. |

    ### 💚예제
    ```xml
    <ItemDef OID="WHO.1" Name="WHO.1" DataType="integer">
        <Description>
            <TranslatedText xml:lang="en">I have felt cheerful and in good spirits</TranslatedText>
            <TranslatedText xml:lang="de">… war ich froh und guter Laune</TranslatedText>
        </Description>
            <Question>
                <TranslatedText xml:lang="en">I have felt cheerful and in good spirits</TranslatedText>
                <TranslatedText xml:lang="de">… war ich froh und guter Laune</TranslatedText>
            </Question>
        <CodeListRef CodeListOID="CL.3"/>
    </ItemDef>
    ```
