# ESEC/FSE - SUPLEMENTARY Material

In this page, we present supporting material of the paper entitled "UML2PROV: Automating Provenance Capture in Software Engineering" submitted to the 11th Joint Meeting of the European Software Engineering Conference and the ACM SIGSOFT Symposium on the Foundations of Software Engineering PADERBORN, GERMANY, September 04 – 08.

* [Translation rules] (https://github.com/uml2prov/esec-fse/blob/master/README.md#translation-rules)
* [Evaluation dataset] (https://github.com/uml2prov/esec-fse/blob/master/README.md#evaluation-dataset)


##Translation rules

Here we present a complete description of the rules used to mapping UML Sequence Diagrams (SqD) and UML State Diagrams (SMD) to provenance templates. Such rules have been divided into those that deal with SqD, and those that tackle SMD. 

### From Sequence Diagrams to templates

* SeqR1. The _lifeline_ representing the sender of a _message_ is mapped to a PROV __agent__, whose type is given by the name of its class. 

* SeqR2. Each _message_ is mapped to a PROV activity, whose type is given by the message's name, and a start time, given by the _message_ invocation time. In addition, the PROV activity representing the _message_ is related to the PROV agent representing the sernder _lifeline_ (given by SeqR1) through the PROV relationship prov:wasAssociatedWith.

* SeqR3. Each _input argument_ within an _asynchronous_ or _synchronous_ _message_ is mapped to a PROV entity identified by the variable var:inputN, where N refers to the argument's position, and a PROV attribute prov:value. Since _input arguments_ are "used" by _message_ to perform their particular operations, we utilize the relationship prov:used to relate the activity, created for the _message_ (by SeqR2), with each entity, created for each _input argument_. Besides, the relationship used has a PROV attribute prov:role with the name of the _input argument_. 

* SeqR4. Each _output argument_ within a _reply_ _message_ is mapped to a PROV entity identified by the variable var:outputN, where N refers to the argument's position, and the attribute prov:value. Since it could be said that each _output argument_ is "generated" by a reply _message_, we use the PROV association prov:wasGeneratedBy to relate the activity, created for the _message_ (by SeqR2), with the entity, created for each _output argument_. Furthermore, prov:wasGeneratedBy has a PROV attribute prov:role with the name of the _output argument_.

* SeqR5. In PROV two relationships with the form (B, prov:used, A) and (C, prov:wasGeneratedBy, B) (see SeqR3 and SeqR4, respectively), have to be enriched with a new direct relationship between (C, prov:wasDerivedFrom, A) to express the dependency of C on A. This structure refers to a provenance construction called _Use-generate-derive triangle_, which encloses the three elements involved.

### From State Machine Diagrams to templates

* StR1. The _object_ of a _transition_ is mapped to a PROV agent, with the type attribute given by the name of the _object_'s class. Furthermore, the _object_'s _state machine_ is represented as a PROV entity (we note that it has a similar translation to a SqD's _lifeline_). More specifically, we create an entity related to the _object_'s _state machine_ to have an element representing the umbrella for the _object_'s concrete _states_. Both the PROV agent representing the _object_ and the PROV entity representing the _object_'s _state machine_ are related by means of prov:wasAttributedTo.

* StR2. The _event_ in a _transition_ is mapped to a PROV activity, whose type is given by the _transition_ name, and start time, given by the _transition_ invocation time. 

* Str3. Each _state_ (_simple_ or _composite_) included in the SMD is mapped to a PROV entity. If the _state_ constitutes the _source_ of the _transition_, the new PROV entity is identified by var:source, and is related to the PROV activity which represents the _transition_ through the relationship prov:used. In addition, in order to represent the fact that the triggered of a _transition_ changes the _object_'s active _state_ from a _source_ _state_ to a _target_ _state_, the _source_ _state_ PROV entity is related to the PROV activity translation of the _transition_ by means of the prov:wasInvalidatedBy relationship. In contrast, if the _state_ corresponds to the target of the _transition_, the new PROV entity is identified by var:target, and is related to the PROV activity which represents the _transition_ by means of the prov:wasGeneratedBy relationship. In both cases, the PROV entity is complemented by the attributes prov:type and u2p:state. While prov:type is given by the name of the _object_ modelled by the SMD, u2p:state is given by the name of the translated _state_. 

* Str4. As in SeqR5, we can identify the _Use-generate-derive triangle_ among the PROV entity representing the _source_ _state_, the PROV activity representing the _transition_ and the PROV entity representing the _target_ _state_. Taking this into account, we define a direct relationship between both the source PROV entity and the target PROV entity by means of the prov:wasDerivedFrom relationship, representing the fact that the _target_ _state_ is a consequence of the triggered of the _transition_ from the _source_ _state_. 

* Str5. The relationship between the _object_ whose behaviour is modelled and its states depend on whether the _transition_ is enclosed in a _composite_ _state_ or not. If the _transition_ is enclosed, in addition to create a PROV entity representing the _composite_ _state_ (StR3), we register such a _state_ as belonging to the _object_'s _state machine_ by means of the prov:specializationOf relationship. Additionally, each PROV entity representing an enclosed state is directly related with the PROV entity representing the _composite_ _state_ through prov:hadMember. It allows us to assert that each substate "is a member of" the _composite_ _state_. In contrast, if the _transition_ is not enclosed, we register each _state_ which composes the _transition_ as belonging to the _object_'s _state machine_ by means of the prov:specializationOf relationship. 


##Evaluation dataset

In this section we provide the dataset which includes the relevant documents related to the case studies used in "Analysis and Discussion" section. 

* [Seminar] (https://github.com/uml2prov/esec-fse/tree/master/Seminar)
* [Water](https://github.com/uml2prov/esec-fse/tree/master/Water)
* [Model View Controller (MVC)] (https://github.com/uml2prov/esec-fse/tree/master/MVC) 
* [PhoneCall](https://github.com/uml2prov/esec-fse/tree/master/PhoneCall)
* [Elevator]()










