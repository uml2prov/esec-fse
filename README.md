# ESEC/FSE - SUPLEMENTARY Material

In this page, we present supporting material of the paper entitled "UML2PROV: Automating Provenance Capture in Software Engineering" submitted to the 11th Joint Meeting of the European Software Engineering Conference and the ACM SIGSOFT Symposium on the Foundations of Software Engineering PADERBORN, GERMANY, September 04 – 08.

* [Translation rules] (https://github.com/uml2prov/esec-fse/blob/master/README.md#translation-rules)
* [Evaluation dataset] ()






##Translation rules

Here we present a complete description of the rules used to mapping UML Sequence Diagrams (SqD) and UML State Diagrams (SMD) to provenance templates. Such rules have been divided into those that deal with SqD, and those that tackle SMD. 

### From Sequence Diagrams to templates

* SeqR1. The lifeline representing the sender of a message is mapped to a PROV agent, whose type is given by the name of its class. 

* SeqR2. Each message is mapped to a PROV activity, whose type is given by the messag e's name, ana a start time, given by the message invocation time. In addition, the PROV activity representing the message is related to the PROV agent representing the sernder lifeline (given by SeqR1) through the PROV reslationship prov:wasAssociatedWith.

* SeqR3. Each input argument within an asynchronous or synchronous message is mapped to a PROV entity identified by the variable var:inputN, where N refers to the argument's position, ana a PROV attribute prov:value. Since input arguments are "used" by messages to perform their particular operations, we utilize the relationship prov:used to relate the activity, created fot ehe message (by SeqR2), with each entity, created for each input argument. Besides, the relationship used has a PROV attribute prov:role with the name of the input argument. 

* SeqR4. Each output argument within a reply message is mapped to a PROV entity identified by the variable var:outputN, where N refers to the argument's position, and the attribute prov:value. Since it could be said that each output argument is "generated" by a reply message, we use the PROV association prov:wasGeneratedBy to relate the activity, created for the message (by SeqR2), with the entity, created for each output argument. Furthermore, wasGeneratedBy has a PROv attribute prov:role with the name of the output argument.

* SeqR5. In PROV two relationships with the form (B, prov:used, A) and (C, prov:wasGeneratedBy, B) (see SeqR3 and SeqR4, respectively), have to be enriched with a new direct relationship between (C, prov:wasDerivedFrom, A) to express the dependency of C and A. This structure refers to a provenance construction called Use-generate-derive triangle, which encloses the three elements involvedsadf

### From State Machine Diagrams to templates

* StR1. The object of a transition is mapped to a PROV agent, with the type attribute given by the name of the object's class. Furthermore, the object's state machine is represented as a PROV entity (we note that it has a similar tranlation to a SqD's lifeline). We create an entity related to the object's state machine to have an element representing the umbrella for the object's concret states. Both the PROV agent representing the object and the PROV entity representing the object's state machine are related by means of prov:wasAttributedTo.

* StR2. The event in a transition is mapped to a PROV activity, whose type is given by the transition name, and start time, given by the transition invocation tieme. 

* Str3. Each state (simple or composite) included in the SMD is mapped to a PROV entity. If the state constitutes the source of the transition, the new PROV entity is identified by var:source, and is related to the PROV activity which represents the transition through the relationship prov:used. In addition, in order to represents the fact that the triggered of a transition change the object's activate state from a source state to a target state, the source state PROV entity is related to the PROV activity translation of the transition by means of prov:wasInvalidatedBy relationship. In contrast if the state corresponds to the target of the transition, the new PROV entity is identified by var:target, and is related to the PROV activity which represents the transition by means of the prov:wasGeneratedBy relationship. In both cases, the PROV entity is complemented by the attributes prov:type and u2p:state: while prov:type is given by the name of the object modelled by the SMD, u2p:state is given by the name of the translated state. 

* Str4. As in SeqR5, we can identify the Use-generate-derive triangle among the PROV entity representing the source state, the PROV activity representing the transition and the PROV entity representing the target state. Taking this into account, we define a direct relationship between both the source PROV entity and the target PROV entity by means of the prov:wasDerivedFrom relationship, representing the fact that the target state is a consequence of the triggered of the transition from the source state. 

* Str5. The relationship between the object whose behaviour is modelled and its states depend on whether the transition is enclosed in a composite state or not. If the transition is enclosed, in addition to create a PROV entity representing the composite state (StR3), we register such a state as belonging to the object's state machine by means of the prov:specializationOf relationship. Additionally, each PROV entity representing an enclosed state is directly related with the PROV entity representing the composite state through prov:hadMember. It allows us to assert that each substate "is a member of" the composite State. In contrast, if the transition is not enclosed, we register each state which composes the transition as belonging to the object's state machine by means of the prov:specializationOf relationship. 












