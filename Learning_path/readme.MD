## LLM Agents - Part 3: **<a href="https://aisc.substack.com/p/llm-agents-part-3-multi-agent-llm">Design Patterns</a>**

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

