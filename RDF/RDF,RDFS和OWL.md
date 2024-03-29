<!-- TOC -->

- [RDF](#RDF)
  - [知识图谱的基石](#知识图谱的基石)
  - [RDF的三种类型](#RDF的三种类型)
  - [RDF序列化方法](#RDF序列化方法)
- [RDF的“衣服”——RDFS/OWL](#RDF的“衣服”——RDFS/OWL)
- [RDFS](#RDFS)
- [OWL](#OWL)
  - [数据建模能力](#数据建模能力)
  - [推理能力](#推理能力)
<!-- /TOC-->
--------------------------------------------------
## RDF
### 知识图谱的基石
- 知识图谱是由一些相互连接的**实体**和他们的**属性**构成的。
- 知识图谱是由一条条知识组成，每条知识表示为一个SPO三元组(Subject-Predicate-Object)。
- 在知识图谱中，我们用RDF形式化地表示这种三元关系。
- RDF(Resource Description Framework)，即资源描述框架，是W3C制定的，用于描述实体/资源的标准数据模型。

<div align="center"><img src="./picture/spo.jpg" height="" /></div>

### RDF的三种类型
- RDF图中一共有三种类型，International Resource Identifiers(IRIs)，blank nodes 和 literals。
- 下面是SPO每个部分的类型约束：
    - Subject可以是IRI或blank node。
    - Predicate是IRI。
    - Object三种类型都可以。
- IRI我们可以看做是URI或者URL的泛化和推广，它在整个网络或者图中唯一定义了一个实体/资源，和我们的身份证号类似。
- literal是字面量，我们可以把它看做是带有数据类型的纯文本。
- blank node简单来说就是没有IRI和literal的资源，或者说匿名资源。
- 示例：
    - 罗纳尔多的中文名是罗纳尔多·路易斯·纳扎里奥·达·利马”这样一个三元组用RDF形式来表示：

<div align="center"><img src="./picture/罗纳尔多spo.jpg" height="" /></div>

>"www.kg.com/person/1"
>是一个IRI，用来唯一的表示“罗纳尔多”这个实体。
>"kg:chineseName"也是一个IRI，用来表示“中文名”这样一个属性。"kg:"是RDF文件中所定义的prefix，如下所示。
>>@prefix kg: <http://www.kg.com/ontology/>
>>>kg:chineseName其实就是"http:// www.kg.com/ontology/chineseName"的缩写。

<div align="center"><img src="./picture/罗纳尔多.jpg" height="" /></div>

- 我们其实可以认为知识图谱就包含两种节点类型，资源和字面量。借用数据结构中树的概念，字面量类似叶子节点，出度为0。

### RDF序列化方法
- RDF/XML，顾名思义，就是用XML的格式来表示RDF数据。之所以提出这个方法，是因为XML的技术比较成熟，有许多现成的工具来存储和解析XML。然而，对于RDF来说，XML的格式太冗长，也不便于阅读，通常我们不会使用这种方式来处理RDF数据。
- N-Triples，即用多个三元组来表示RDF数据集，是最直观的表示方法。在文件中，每一行表示一个三元组，方便机器解析和处理。开放领域知识图谱DBpedia通常是用这种格式来发布数据的。
- Turtle, 应该是使用得最多的一种RDF序列化方式了。它比RDF/XML紧凑，且可读性比N-Triples好。
- RDFa, 即“The Resource Description Framework in Attributes”，是HTML5的一个扩展，在不改变任何显示效果的情况下，让网站构建者能够在页面中标记实体，像人物、地点、时间、评论等等。也就是说，将RDF数据嵌入到网页中，搜索引擎能够更好的解析非结构化页面，获取一些有用的结构化信息。
- JSON-LD，即“JSON for Linking Data”，用键值对的方式来存储RDF数据。
- 我们结合罗纳尔多知识图的例子，给出其N-Triples和Turtle的具体表示。
>Example1 N-Triples:

><http://www.kg.com/person/1> <http://www.kg.com/ontology/chineseName> "罗纳尔多·路易斯·纳萨里奥·德·利马"^^string.  
><http://www.kg.com/person/1> <http://www.kg.com/ontology/career> "足球运动员"^^string.  
><http://www.kg.com/person/1> <http://www.kg.com/ontology/fullName> "Ronaldo Luís Nazário de Lima"^^string.  
><http://www.kg.com/person/1> <http://www.kg.com/ontology/birthDate> "1976-09-18"^^date.  
><http://www.kg.com/person/1> <http://www.kg.com/ontology/height> "180"^^int.  
><http://www.kg.com/person/1> <http://www.kg.com/ontology/weight> "98"^^int.  
><http://www.kg.com/person/1> <http://www.kg.com/ontology/nationality> "巴西"^^string.  
><http://www.kg.com/person/1> <http://www.kg.com/ontology/hasBirthPlace> <http://www.kg.com/place/10086>.  
><http://www.kg.com/place/10086> <http://www.kg.com/ontology/address> "里约热内卢"^^string.  
><http://www.kg.com/place/10086> <http://www.kg.com/ontology/coordinate> "-22.908333, -43.196389"^^string.  

- 用Turtle表示的时候我们会加上前缀（Prefix）对RDF的IRI进行缩写。
>Example2 Turtle:  

>@prefix person: <http://www.kg.com/person/> .  
>@prefix place: <http://www.kg.com/place/> .  
>@prefix : <http://www.kg.com/ontology/> .  

>person:1 :chineseName "罗纳尔多·路易斯·纳萨里奥·德·利马"^^string.  
>person:1 :career "足球运动员"^^string.  
>person:1 :fullName "Ronaldo Luís Nazário de Lima"^^string.  
>person:1 :birthDate "1976-09-18"^^date.  
>person:1 :height "180"^^int.   
>person:1 :weight "98"^^int.  
>person:1 :nationality "巴西"^^string.   
>person:1 :hasBirthPlace place:10086.  
>place:10086 :address "里约热内卢"^^string.  
>place:10086 :coordinate "-22.908333, -43.196389"^^string.  

- 同一个实体拥有多个属性（数据属性）或关系（对象属性），我们可以只用一个subject来表示，使其更紧凑。我们可以将上面的Turtle改为：
>Example3 Turtle:  

>@prefix person: <http://www.kg.com/person/> .  
>@prefix place: <http://www.kg.com/place/> .  
>@prefix : <http://www.kg.com/ontology/> .  

>person:1 :chineseName "罗纳尔多·路易斯·纳萨里奥·德·利马"^^string;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:career "足球运动员"^^string;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:fullName "Ronaldo Luís Nazário de Lima"^^string;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:birthDate "1976-09-18"^^date;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:height "180"^^int;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:weight "98"^^int;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:nationality "巴西"^^string;   
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:hasBirthPlace place:10086.  
>place:10086 :address "里约热内卢"^^string;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:coordinate "-22.908333, -43.196389"^^string.  
- ***将一个实体用一个句子表示（这里的句子指的是一个英文句号“.”）而不是多个句子，属性间用分号隔开。***
-----------------------------------------------

## RDF的“衣服”——RDFS/OWL
之所以说RDFS/OWL是RDF的“衣服”，因为它们都是用来描述RDF数据的。为了不显得这么抽象，我们可以用关系数据库中的概念进行类比。用过Mysql的读者应该知道，其database也被称作schema。这个schema和我们这里提到的schema language十分类似。我们可以认为数据库中的每一张表都是一个类（Class），表中的每一行都是该类的一个实例或者对象（学过java等面向对象的编程语言的读者很容易理解）。表中的每一列就是这个类所包含的属性。如果我们是在数据库中来表示人和地点这两个类别，那么为他们分别建一张表就行了；再用另外一张表来表示人和地点之间的关系。RDFS/OWL本质上是一些预定义词汇（vocabulary）构成的集合，用于对RDF进行类似的类定义及其属性的定义。   
>Notice: RDFS/OWL序列化方式和RDF没什么不同，其实在表现形式上，它们就是RDF。其常用的方式主要是RDF/XML，Turtle。另外，通常我们用小写开头的单词或词组来表示属性，大写开头的表示类。数据属性（data property，实体和literal字面量的关系）通常由名词组成，而对象数据（object property，实体和实体之间的关系）通常由动词（has，is之类的）加名词组成。剩下的部分符合驼峰命名法。为了将它们表示得更清楚，避免读者混淆，之后我们都会默认这种命名方式。读者实践过程中命名方式没有强制要求，但最好保持一致。

-------------------------------------------------

## RDFS
- RDFS，即“Resource Description Framework Schema”，是最基础的模式语言。
- 还是以罗纳尔多知识图为例，我们在概念、抽象层面对RDF数据进行定义。
- 下面的RDFS定义了人和地点这两个类，及每个类包含的属性。

>@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .  
>@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .  
>@prefix : <http://www.kg.com/ontology/> .  

- 这里我们用词汇rdfs:Class定义了“人”和“地点”这两个类。
>:Person rdf:type rdfs:Class.  
>:Place rdf:type rdfs:Class.  

- rdfs当中不区分数据属性和对象属性，词汇rdf:Property定义了属性，即RDF的“边”。
>:chineseName rdf:type rdf:Property;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .    
 
>:career rdf:type rdf:Property;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .    
        
>:fullName rdf:type rdf:Property;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .    
        
>:birthDate rdf:type rdf:Property;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:date .    

>:height rdf:type rdf:Property;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:int .    
        
>:weight rdf:type rdf:Property;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:int .    
        
>:nationality rdf:type rdf:Property;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .    
        
>:hasBirthPlace rdf:type rdf:Property;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range :Place .    
        
>:address rdf:type rdf:Property;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Place;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .      
        
>:coordinate rdf:type rdf:Property;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Place;    
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .    

- rdfs:Class. 用于定义类。
- rdfs:domain. 用于表示该属性属于哪个类别。
- rdfs:range. 用于描述该属性的取值类型。
- rdfs:subClassOf. 用于描述该类的父类。比如，我们可以定义一个运动员类，声明该类是人的子类。
- rdfs:subProperty. 用于描述该属性的父属性。比如，我们可以定义一个名称属性，声明中文名称和全名是名称的子类。

------------------------------------------------------------------

## OWL
- RDFS本质上是RDF词汇的一个扩展。后来人们发现RDFS的表达能力还是相当有限，因此提出了OWL。
- 我们也可以把OWL当做是RDFS的一个扩展，其添加了额外的预定义词汇。
- OWL，即“Web Ontology Language”，语义网技术栈的核心之一。OWL有两个主要的功能：
    - 提供快速、灵活的数据建模能力。
    - 高效的自动推理。
    
### 数据建模能力
>@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .  
>@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .  
>@prefix : <http://www.kg.com/ontology/> .  
>@prefix owl: <http://www.w3.org/2002/07/owl#> .  

- 这里我们用词汇owl:Class定义了“人”和“地点”这两个类。
>:Person rdf:type owl:Class.  
>:Place rdf:type owl:Class.  

- owl区分数据属性和对象属性（对象属性表示实体和实体之间的关系）。**词汇owl:DatatypeProperty定义了数据属性，owl:ObjectProperty定义了对象属性**。
>:chineseName rdf:type owl:DatatypeProperty;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .  

>:career rdf:type owl:DatatypeProperty;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .  
        
>:fullName rdf:type owl:DatatypeProperty;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .  
        
>:birthDate rdf:type owl:DatatypeProperty;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:date .  

>:height rdf:type owl:DatatypeProperty;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:int .  
        
>:weight rdf:type owl:DatatypeProperty;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:int .  
        
>:nationality rdf:type owl:DatatypeProperty;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .  
        
>:hasBirthPlace rdf:type owl:ObjectProperty;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Person;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range :Place .  
        
>:address rdf:type owl:DatatypeProperty;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Place;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .  
        
>:coordinate rdf:type owl:DatatypeProperty;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:domain :Place;  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rdfs:range xsd:string .  

- 罗纳尔多这个例子不能展现OWL丰富的表达能力，我们这里简单介绍一下常用的词汇：

- 描述属性特征的词汇
    - owl:TransitiveProperty. 表示该属性具有传递性质。例如，我们定义“位于”是具有传递性的属性，若A位于B，B位于C，那么A肯定位于C。
    - owl:SymmetricProperty. 表示该属性具有对称性。例如，我们定义“认识”是具有对称性的属性，若A认识B，那么B肯定认识A。
    - owl:FunctionalProperty. 表示该属性取值的唯一性。 例如，我们定义“母亲”是具有唯一性的属性，若A的母亲是B，在其他地方我们得知A的母亲是C，那么B和C指的是同一个人。
    - owl:inverseOf. 定义某个属性的相反关系。例如，定义“父母”的相反关系是“子女”，若A是B的父母，那么B肯定是A的子女。

- 本体映射词汇（Ontology Mapping）
    - owl:equivalentClass. 表示某个类和另一个类是相同的。
    - owl:equivalentProperty. 表示某个属性和另一个属性是相同的。
    - owl:sameAs. 表示两个实体是同一个实体。

- 本体映射主要用在融合多个独立的Ontology（Schema）。
    - 举个例子，张三自己构建了一个本体结构，其中定义了Person这样一个类来表示人；李四则在自己构建的本体中定义Human这个类来表示人。当我们融合这两个本体的时候，就可以用到OWL的本体映射词汇。
><http://www.zhangsan.com/ontology/Person> rdf:type owl:Class .  
><http://www.lisi.com/ontology/Human> rdf:type owl:Class .  
><http://www.zhangsan.com/ontology/Person> owl:equivalentClass <http://www.lisi.com/ontology/Human> .

### 推理能力
- 知识图谱的推理主要分为两类：基于**本体**的推理和基于**规则**的推理。
>我们这里谈的是基于本体的推理。读者应该发现，上面所介绍的属性特征词汇其实就创造了对RDF数据进行推理的前提。此时，我们加入支持OWL推理的推理机（reasoner），就能够执行基于本体的推理了。RDFS同样支持推理，由于缺乏丰富的表达能力，推理能力也不强。举个例子，我们用RDFS定义人和动物两个类，另外，定义人是动物的一个子类。此时推理机能够推断出一个实体若是人，那么它也是动物。OWL当然支持这种基本的推理，除此之外，凭借其强大的表达能力，我们能进行更有实际意义的推理。想象一个场景，我们有一个庞大数据库存储人物的亲属关系。里面很多关系都是单向的，比如，其只保存了A的父亲（母亲）是B，但B的子女字段里面没有A，如下表。

<div align="center"><img src="./picture/表格.png" height="" /></div>

>如果在只有单个关系，数据量不多的情况下，我们尚能人工的去补全这种关系。如果在关系种类上百，人物上亿的情况下，我们如何处理？当进行关系修改，添加，删除等操作的时候，该怎么处理？这种场景想想就会让人崩溃。如果我们用inversOf来表示hasParent和hasChild互为逆关系，上面的数据可以表示为：

<div align="center"><img src="./picture/父子推理.jpg" height="" /></div>

- 绿色的关系表示是我们RDF数据中真实存在的，红色的关系是推理得到的。

