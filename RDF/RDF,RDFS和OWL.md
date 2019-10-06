<!-- TOC -->

- [RDF](#RDF)
  - [知识图谱的基石](#知识图谱的基石)
  - [RDF的三种类型](#RDF的三种类型)
  - [RDF序列化方法](#RDF序列化方法)
- [RDF的“衣服”——RDFS/OWL](#RDF的“衣服”——RDFS/OWL)

  
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
