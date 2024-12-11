This repository documents my understanding of Multi-agentic LLM systems, mostly my notes are from Deep Random Thoughts. 

1. [Introduction](#1)
   1. [Part 1: The 9 Commandments: How to Build LLM Products Successfully](#2)  
      1. [Data is most probably your only moat](#3)
      2. [Follow validation-driven development](#4)
      3. [Get your product in the hands of your (ideally paying) users asap](#5)
      4. [Separate the data and interface layers, and be prepared to invest in data engineering ](#6)
   2. [Part 2: LLM Agents](#3)
   3. [Part 3: Design Patterns](#4) 

<a name="1"></a>
## Introduction

<a name="2"></a>
### Part 1: **<a href="https://aisc.substack.com/p/llm-agents-part-1-the-9-commandments">The “9” Commandments: How to Build LLM Products Successfully</a>**

The principles of building the most sophisticated multi-agent LLM products is the same as the ones for any LLM product and ultimately the same as the ones for any data-powered software product.

<a name="3"></a> 
#### Data is most probably your only moat

Moat = competitive advantage 

- Your data is the secret sauce that enables you to build AI systems that can perform tasks and provide insights your competitors can only dream of.
- When it comes to preparing your data for training models, it's important to weigh the benefits of annotating data versus relying solely on unsupervised methods. While unsupervised learning can be appealing due to its potential to reduce manual labor, annotated data often leads to better model performance and faster convergence. Investing in data annotation can pay off in the long run by improving the accuracy and reliability of your AI systems.

<a name="4"></a> 
#### Follow validation-driven development

- A very common problem in naively built agentic systems is **compounding error in these systems that quickly leads into systems falling in endless loops or producing nonsensical results**. The only way to avoid these problems is having reliable and granular metrics throughout the system that act as feedback or reward mechanisms keeping the components and overall system in check.
- Start by defining clear, quantitative metrics that capture what "good" looks like for your product - whether that's accuracy, user engagement, task completion rate, or some combination of these. This has to be done for the **components of your architecture as well as the overall performance of the system.**
- The key is to have a **solid experimental setup** where you're constantly shipping new arrangements of components, measuring their impact on your core metrics, and doubling down on the most promising ideas. This is also particularly important for LLM agent systems since the landscape of potential improvements is so vast that a thorough investigation of all possibilities with limited resources is simply impractical. 

<a name="5"></a> 
#### Get your product in the hands of your (ideally paying) users asap

- One of the biggest pitfalls in AI development is getting bogged down in endless technical tweaks before getting any feedback from real users. This is especially tempting with LLMs, where there's always another parameter to tune or dataset to incorporate. But the reality is, you'll never know if you're building something people actually want until you put it in their hands. The feedback gets even more real if they are paying you (or at least they anticipate having to pay you to use the product).
- The antidote is simple (but not always easy): Build the simplest viable version of your product and get it in front of users as quickly as humanly possible. This might mean starting with a bare-bones MVP (Minimum viable product) that only does one thing, or even launching a "fake" version powered by human labor behind the scenes. The point is to start collecting real feedback and data from day one, so you can validate your core assumptions and start iterating in the right direction. Doing this can also help regulate your understanding of the right metrics to track as per last commandment. It is easy to lose sight of what really matters to the user quantitatively by hiding behind technical metrics like accuracy.
- Another important aspect of this is deployment. It is great that your product works on your laptop, but if the user can’t interact with it you have significant friction in getting the feedback that you need.

<a name="6"></a> 
#### Separate the data and interface layers, and be prepared to invest in data engineering 

Do not count on LLMs beyond linguistic interfaces 

Create Robust Feedback Modules

Actually Improve User’s Productivity 

Think deeply about integrations into users' tools and workflows 

Design for humans! 


<a name="3"></a>
### Part 2: **<a href="https://aisc.substack.com/p/llm-agents-part-2-what-the-heck-are">LLM Agents</a>**


<a name="4"></a>
### Part 3: **<a href="https://aisc.substack.com/p/llm-agents-part-3-multi-agent-llm">Design Patterns</a>**

Going to the basics of RL is important in thinking through agentic workflows, as we discussed before. Also going back to software design principles is the way to go about creating multi-agent systems. Let's discuss how the below established software design principles can be applied to the emerging trend of multi-agent large language model (LLM) systems:
- Domain-Driven Design (DDD), 
- Service-Oriented Architecture (SOA), and 
- Microservices architecture

Traditional design patterns provide a robust framework for software development. By integrating machine learning (ML) into these patterns, we can introduce a new dimension to software architecture. ML enables **probabilistic routing** between software components, replacing **pre-programmed deterministic routing**. This integration not only enhances the functionality of individual components but also introduces new capabilities. Both LLMs and specialized ML models, and often a combination of the two, can be utilized to achieve these improvements.

### Communication methods

Communication methods between components, previously services and more recently agents, remain consistent with traditional approaches, using 
- REST, 
- GraphQL, 
- JSON, and 
- DSLs. 

However, the introduction of natural language as an interface adds a new layer of complexity, with its own set of advantages and challenges. These hybrid systems, combining predetermined and probabilistic behavior, may become the new standard in software development.

### Domain-Driven Design (DDD)

DDD emphasizes modeling software around the **core domain of a business**. It advocates for a common language shared by developers and domain experts, ensuring everyone speaks the same language. DDD breaks down the domain into bounded contexts, areas with well-defined and segregated responsibilities, and often minimal dependency on other areas.

Bounded contexts ensure that complexity is manageable by focusing on specific aspects of the domain. This focus also promotes better communication and understanding between developers and domain experts. **By breaking down the domain into bounded contexts, we lay the groundwork for introducing agents with specialized capabilities, each responsible for a specific bounded context within the larger multi-agent LLM system**. Just as bounded contexts promote modularity and focus within the domain, agents with bounded responsibility will do the same.

Each bounded context has its own data entities, business rules, and common language. This modular approach allows developers to focus on specific areas of functionality without getting overwhelmed by the complexity of the entire system.

### Service-Oriented Architecture (SOA)

SOA takes the concept of bounded contexts from DDD and maps them to services. Each service encapsulates a specific domain functionality and exposes a well-defined interface. This promotes loose coupling, allowing services to evolve independently without impacting others.

### Microservices architecture

Microservices architecture takes SOA a step further by creating even smaller, more focused services. Unlike SOA, in microservice architectures the services focus as narrowly as possible, often only on a single function. This approach offers greater agility, scalability, and resilience. Each microservice owns its data and logic, promoting independent development and deployment.

**You might notice that some of the things that you’ve imagined as “multi-agent” systems could be achieved simply by a well designed software system.**

### Multi-Agent LLMs: A New Design Pattern

#### Beyond Microservices: Active Agents vs. Passive Data Handlers

Microservices excel in building modular, scalable software systems. However, they primarily function as **passive data handlers, responding to requests and manipulating data.** Multi-agent LLMs, on the other hand, take a leap forward by introducing **“active” components inside these services, effectively allowing them to “make decisions” in scenarios without being deterministically programmed to do so.** These agents can:

- Continuously monitor the situation, analyze data, and identify potential issues or opportunities.
- Take initiative and perform actions without explicit instructions. This can involve initiating communication with other agents, retrieving information, or even triggering predefined workflows.
- Collaborate and negotiate with each other to achieve a common goal. This allows for dynamic decision-making and adaptation to unforeseen circumstances.

This shift from passive data handling to active agents unlocks new possibilities:

- Complex Task Automation: Multi-agent LLMs can automate complex tasks that require reasoning, planning, and collaboration across different domains. Imagine a system with a service constantly monitoring traffic patterns, augmented with an agent analyzing weather data, and a second one making decisions about rerouting deliveries to avoid congestion – a scenario beyond the pre-determined nature of microservices.

- Emergent Behavior:  LLMs themselves show emergent properties; they can classify text, extract entities, and more although they are only explicitly trained on predicting the next most likely token. When LLM-powered agents that are fine-tuned to strengthen any of these properties or are augmented with tools that give them specialized capabilities can interact with each other, non-trivial collective behavior might appear. The semantic flexibility, although less controllable in nature, combined with the reliability of JSON based communication between various software services, agentic or otherwise, could result in systems that work in ways that are more “expected” by human operators, for example by adapting and responding to situations in ways that might not be explicitly programmed.

- Continuous Improvement: The modular, and to some extent potentially redundant, nature of multi-agent systems can make them less constrained by improvements in a single component as the only opportunity for improvements in the overall system. For example, an agent that is fine-tuned to do task decomposition effectively can help other agents do that task well by providing examples in a few shot setup. In a more well setup system, each component inside agents, potentially small LM or non-LM models, can have a feedback loop continuously being restrained and improved. This could include models that are involved in the policies of individual agents or the overall system. 


### Contracts, Languages, and Communication

Microservices architectures thrive on clear and well-defined communication. This communication relies on **predefined API contracts, essentially agreements that dictate how services interact with each other.** These contracts act like sheet music for an orchestra, ensuring each microservice plays its part seamlessly.

REST APIs and JSON are the cornerstones of these contracts. 

- REST (Representational State Transfer) defines a standardized architecture for requesting and receiving data between services. 
- JSON (JavaScript Object Notation) acts as the “language” for transmitting data, offering a lightweight and human-readable format for exchanging information.

Agentic systems use these existing mechanisms and will also introduce a new dimension to communication, adding two more communication types:

- Domain-Specific Languages (DSLs): These are custom languages tailored to a specific domain or purpose. Imagine a trading agent responsible for capital market transactions using a combination of statistics, machine learning, and business logic rules. Communicating this info in natural language is too complex and error-prone, and in JSON is too limited. However, using a DSL, imagine a set of pseudocode snippets describing the logic of the rules, as a communication contract between the controller agent and the executor agent can be the most efficient channel. DSLs offer more expressiveness and efficiency compared to generic JSON data, but require specialized knowledge to understand and implement.

- Natural Language (NL): This is the most human-like form of communication. Agents could potentially communicate and share information using natural language processing (NLP) techniques. **However, natural language is inherently ambiguous and prone to misinterpretations.** While offering the most flexibility, NL communication is also the least robust and requires advanced NLP capabilities to manage effectively.

Even in the realm of multi-agent systems, **the established approach of API calls and JSON data exchange remains the most reliable and robust communication method**. It provides a clear and well-defined path for information exchange. DSLs offer a middle ground, balancing expressiveness with control. Finally, natural language communication, while offering the most flexibility, comes with the greatest risk of misunderstandings and requires significant development effort to implement effectively. In all likelihood, a product that you design would tap into all these different communication channels between services and therefore agents to achieve the best balance between performance and control.

### Shared Benefits and Challenges multi-agent LLMs and microservices architectures

Both multi-agent LLMs and microservices architectures offer several advantages:

- Modularity: Break down complex tasks into smaller, manageable units.
- Scalability: Scale individual agents or services independently based on needs.
- Resilience: If designed right, given the adaptability of agent policies that leads to some redundancy, failure of one agent or service doesn't cripple the entire system.
- Independent Deployment: Deploy and update individual agents/services without affecting others.

However, both approaches also come with challenges:

- Increased Complexity: Managing interactions and dependencies between agents/services requires careful planning.
- Testing and Debugging: Debugging issues that span multiple agents/services can be intricate. Also, the probabilistic nature of agents can make systems built with them considerably harder to debug.
- Distributed System Management: Distributing resources and ensuring consistent behavior across agents/services adds complexity.

Therefore, multi-agent systems, unlike what vendors tell you, are not a silver bullet for everything and choosing to approach solving a business problem with them comes down to a careful pros/cons analysis.

### How to design multi-agent architectures?

1. Map the workflow humans execute to achieve a particular objective including people, processes, and tools involved.
2. Draw context boundaries around parts of the process that are self-contained.
    - You want each of these areas to have **minimal data dependency** on another one (if they share / exchange a lot of data they might have to be merged).
    - You want each of these areas to have **minimal functional dependencies** on another one (if most of the time a change in one requires a change in another one, they should be merged into one context).
3. Decide if each of these contexts are a software services or if they need to become “agentic” and mark them as such (“Payment Processing Service”, “Data Analyzer Agent”)
     - Note that each agent might contain microservices (“PDF Parser microservice”, “info retrieval microservice”)
4. Add any other services necessary that your architecture doesn’t explicitly contain (“User Management Service”, “Shared Memory Service”)
5. Determine how data flows through the system (“PDF goes from File Upload service to parsing service”, “JSON containing rewritten query and search filters goes from query analyzer service to info retrieval service). 
     - Revise the system modularity (context boundaries) to minimize data movement.
6. Determine and document **communication protocols between different services and agents**.
     - **In most interfaces the protocol should be REST-based and the data load should be JSON for robustness purposes.**
     - If that fails to meet your requirement, then try to use DSLs, only if that fails also, use natural (or formal) language.
     - It is ok to use natural language for most of your interfaces in a quick and dirty prototype implementation, but you should remind yourself that, in all likelihood, it will **not meet the reliability threshold for user facing production deployment.**
7. Determine how you would unit test each microservice.
     - If conceptualizing a unit test for a microservice is too complex, it might be a sign of a need to break it into smaller pieces.
     - The unit test for a service would be all component unit tests passing. The unit test for an agent might be a less trivial heuristic on how all component unit tests behaved (in principle agents should be able to recover from some of the failing components; for example it can choose a document search action instead if google search action is failing).




Skills I need to develop/already have before the bootcamp:
- Programming
- LLM & AI concepts
- Dev Techstack
- Ops Techstack 
- Product Dev
- Build something

## What are AI agents?

https://www.youtube.com/watch?v=F8NKVhkZZWI

https://www.youtube.com/watch?v=IivxYYkJ2DI


Learning path: https://miro.com/app/board/uXjVLLfHFis=/


https://aisc.substack.com/p/llm-agents-part-3-multi-agent-llm

