# A brief on Digital Object Architecture

The Digital Object (DO) Architecture (also known as the DO Architecture or simply the DOA) is a logical extension of the Internet architecture that addresses the need to support information management more generally than just conveying information in digital form from one location in the Internet to another. The DOA enables interoperability across participating information systems, whether in the Internet or not. It is a non-proprietary architecture and is publicly available [[DONA - DOA](https://www.dona.net/digitalobjectarchitecture)].
DOA introduces the concept of a digital object, which forms the basis for the architecture. It also specifies three core components and two protocols. The three components are:
1. the identifier/resolution system,
2. the repository system, and
3. the registry system. 
One interface protocol is defined for the identifier/resolution system, and a second interface protocol is defined for use by the repository and the registry system. In practice, the repository and registry components may be combined.

## Digital Object
A digital object (DO) is a sequence of bits, or a set of sequences of bits, incorporating a work or portion of a work or other information in which a party has rights or interests, or in which there is value, each of the sequences being structured in a way that is interpretable by one or more of the computational facilities, and having as an essential element an associated unique persistent identifier. 
For all practical purposes, the concept of a digital object is substantially similar to the notion of “digital entity” as defined in ITU-T Recommendation X.1255. An “entity”, in that recommendation, is defined as anything that can be separately and uniquely identified. It also describes a “digital entity” (DE) as an entity that is represented as, or converted to, a machine-independent data structure consisting of one or more elements that may be parsed by different information systems.

## Protocol 1. Digital Object Interface Protocol (DOIP)
The Digital Object Interface Protocol (DOIP) is a simple, but powerful conceptual protocol for software applications (“clients”) to interact with “services” which could be either the digital objects or the information systems that manage those digital objects [[DONA Specifications](https://www.dona.net/specsandsoftware#block-digitalobjectinterfaceprotocol)]. 

## Protocol 2. Identifier/Resolution Protocol (IRP)
The Identifier/Resolution Protocol (IRP) is a rapid-resolution protocol for creating, updating, deleting, and resolving identifiers that are globally managed and allotted. Each identifier is associated with a record that clients can resolve to using this protocol.

## Core Component 1. Identifier/Resolution System
The identifier/resolution system is one of the three components comprising the Digital Object Architecture. This system enables: 
* allotment of unique identifiers to information in digital form structured as digital objects regardless of the location of such information or the technology used to serve such information, and subsequently 
* the resolution of the identifiers to current state information about the corresponding digital object, e.g., its location(s), access, and usage policies, timestamps, and/or public keys. 
The state information is stored in the form of a digital object; and rapid resolution is enabled using the IRP. 

## Core Component 2. Repository System
The repository system manages digital objects including the provision of access to such objects based on the use of identifiers, and with integrated security. Through the use of identifiers in the access protocol, the repository system abstracts away the details of the storage technologies from the clients enabling a long-lived mechanism for depositing and accessing digital objects. Access to this system is enabled using the DOIP. 

## Core Component 3. Registry System
The registry system is a specialized repository system intended to store metadata about digital objects rather than the digital information itself, and typically stores metadata of digital objects that are managed by one or more repository systems. Access to this system is enabled using the DOIP as well.
