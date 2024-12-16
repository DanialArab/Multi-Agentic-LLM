This repository documents my understanding of Multi-agentic LLM systems, mostly my notes are from Deep Random Thoughts. 

1. [Introduction](#1)
   1. [Part 1: The 9 Commandments: How to Build LLM Products Successfully](#2)  
      1. [Data is most probably your only moat](#3)
      2. [Follow validation-driven development](#4)
      3. [Get your product in the hands of your (ideally paying) users asap](#5)
      4. [Separate the data and interface layers, and be prepared to invest in data engineering ](#6)
      5. [Do not count on LLMs beyond linguistic interfaces](#7)
      6. [Create Robust Feedback Modules](#8)
      7. [Actually Improve User’s Productivity](#9)
      8. [Think deeply about integrations into users' tools and workflows](#10)
      9. [Design for humans!](#11)
   2. [Part 2: LLM Agents](#12)
      1. [Agents in Reinforcement Learning](#13)
         1. [Properties of RL agents](#14)
      2. [Agents in Natural Language Processing](#15)
      3. [ Agents and Autonomy](#16)
      4. [Agents and Control Flow](317)

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

- Using LLMs doesn't give you a free pass to ignore established software and data engineering best practices. In fact, as LLM-based systems grow in complexity and capability, it becomes even more critical to architect your systems in a **modular, maintainable way.**
- Designing your system in a way that the LLM itself becomes the source of knowledge is a risky and ill-advised approach. Instead, strive to architect your system, craft your prompts, and provide the relevant context to the LLM to ensure it relies solely on the information you supply to it when generating a response. While this may evolve in the future, cleanly decoupling your data from your interfaces gives you greater control, allows you to layer on additional security and privacy measures, and makes your system more robust to changes in the underlying models. Retrieval-augmented generation (RAG) techniques provide a powerful way to achieve this decoupling while still harnessing the full power of LLMs.
- It is tempting to think that you just fine-tune one model using your data and it will work as expected with all the controls that you need. The reality is that LLMs are not well-behaved enough to achieve any granular level of control necessary for real world applications and use cases. It is best to separate the data layer (knowledge base documents, structured data, etc) in already well established structured (aka databases) with all the necessary controls (eg. identity and access management) that come with those. This also makes it easier for you to **pre- / post- process that data before feeding it to the model in retrieval augmented generation (or equivalent) setups.**
- Separating the data layer also gives you the ability to build all the logic necessary for processing, storing, and retrieving the data used to train and run smaller models you use in your control flows or to fine-tune your large models. This includes data ingestion pipelines, data cleaning and transformation steps, feature engineering, and data versioning. Your interface layer, on the other hand, should focus solely on exposing the capabilities of your models to end-users, whether that's via APIs, chatbots, or interactive GUIs. Of course, LLM itself can act as a linguistic interface by providing conversational interactions with the user. 

<a name="7"></a> 
#### Do not count on LLMs beyond linguistic interfaces 

- As the name suggests, LLMs are language models - they excel at generating statistically plausible sequences of words, but struggle with many other desirable capabilities like reasoning, analysis, and grounding in real-world facts.
- They are notoriously prone to **"hallucinations" - confidently generating false or nonsensical information that can be hard to detect.** They struggle with maintaining coherence over long time horizons or complex multi-step tasks.

<a name="8"></a> 
#### Create Robust Feedback Modules

- In academia, scientific papers undergo peer review, where different experts independently critique the work before publication. Borrowing from this process, a powerful paradigm for building self-improving AI systems is to train multiple models that play distinct roles akin (similar) to authors and reviewers.
- In this setup, you might use a generative model to produce some output, like a dialogue response, a document summary, or a piece of code. You then have to use a separate "critic" model to evaluate the quality of that output along various dimensions like factual accuracy, logical coherence, style and tone. Crucially, these models are trained independently, so the critic acts as an objective assessor, not just a rubber stamp.
- This is particularly important in agentic systems where the goal is for the system to continuously monitor its performance, reflect on the outcome, and try again with improved likelihood of better performance. This is a crucial ingredient for the level of autonomy we seek in agents. Therefore implementing highly reliable, accurate, and trustworthy feedback sub-systems (aka “reward mechanism” in the context of RL) is a big part of success in building an agentic product.  You can even equip the agents with ensembles of critic tools (including but not limited to occasionally asking for human input) to cover different facets of evaluation, like long-term coherence vs. individual response quality.

<a name="9"></a> 
#### Actually Improve User’s Productivity 

-  Productivity does not equal saving time only, but rather it implies saving unwanted effort, therefore, your product has to address workflows that your users:
      - Spend significant time on, AND 
      - They do not want to spend that time doing that task. 
- Approach every new capability through the lens of "how does this concretely make my user's job efficient and effective?" **If you can't quantify the impact, chances are it's not worth building.**
- Another important nuance here is that builders are sometimes excited about taking away the parts of the job that people actually enjoy spending time on rather than parts that they hate doing. While that product might theoretically improve productivity, the psychological barrier of using it will backfire

<a name="10"></a> 
#### Think deeply about integrations into users' tools and workflows 

- To drive successful adoption, your AI product needs to fit seamlessly into users' existing workflows and tool chains. No matter how impressive your models are under the hood, if using your product feels like a clunky, disjointed experience, people simply won't bother. On the flip side, if your product slots nicely into the tools and processes users are already using day-to-day, you'll dramatically lower the barriers to adoption and make your AI feel like a natural extension of users' workflows. The last thing people want is yet another siloed app to switch back and forth from. Instead, look for opportunities to embed your AI capabilities right within the apps users already live in day-to-day, whether that's their email client, messaging platform, note-taking tool, or code editor.
- To get this right, you need to invest significant time upfront to deeply understand how your target users currently work and what their key pain points are. This means going beyond surface-level interviews and surveys to really immerse yourself in their world. Shadow them as they go about their tasks, paying close attention to all the tools, systems, and collaborators they interact with along the way. Map out their end-to-end workflows to identify bottlenecks, inefficiencies, and opportunities for AI to streamline the process.
- Armed with this deep understanding, architect your AI product to integrate with the specific tools your users depend on, with seamless bridges for importing and exporting data, triggering actions, and collaborating with teammates. In many cases, this means delivering your AI capabilities as **plugins or add-ons right within users' primary tools, instead of forcing them to switch to a separate app.**
- When well executed, this deep integration approach makes your AI product feel less like a tool and more like an intelligent assistant that's always there in the flow of work, ready to lend a hand. Users don't have to disrupt their normal processes or learn new interfaces - they can simply tap into the power of AI whenever and wherever they need it. And that frictionless experience is the key to making AI an indispensable part of people's daily lives.

<a name="11"></a> 
#### Design for humans! 

- Pretty model performance numbers in a lab setting are meaningless if they don't translate into tangible benefits for humans in the messy real world.
- Prioritizing the human element means investing deeply in thoughtful UX design, extensive user testing, and rapid iteration based on feedback. It means providing robust, accessible user education to help people understand both the capabilities and limitations of the AI systems you're putting in their hands. And it means proactively considering and mitigating the potential risks and unintended negative consequences your product could have in people's lives.
- If we keep humans at the center and measure success by the positive impact we have **in their lives, not just our model metrics,** we can build an AI-powered future that brings out the best in both machines and people. 

<a name="12"></a>
### Part 2: **<a href="https://aisc.substack.com/p/llm-agents-part-2-what-the-heck-are">LLM Agents</a>**

An Intelligent Agent (IA) is an **autonomous** entity that observes and acts upon an environment to achieve specific goals. These agents can range from simple systems, such as thermostats or basic control mechanisms, to highly complex AI-powered systems. Most IAs possess some or all of the following key properties:

- Autonomous operations
- Reactive to the environment 
- Proactive (goal-directed) 
- Interactive with other agents (via the environment)

Some points:

- Open-source LLMs have also reached a point where they can effectively drive agentic workflows. For example, the integration of LLMs into systems where they can **call tools** has further enhanced their capabilities, allowing them to perform more complex and diverse tasks.
- Do I need multi-agentic LLM: Yes, if:
    - Your task is more complex and involves multiple steps.
    - Needs the ability to remember past interactions and context.
    - Benefits from accessing and interacting with external tools or resources.
    - Requires a level of autonomy in completing the task.
    
    Examples:
  
    - A virtual assistant that manages your schedule, checks weather data, and books appointments.
    - A system that analyzes customer reviews and recommends product improvements.
    - A chatbot that can answer complex questions by searching the web and integrating information from different sources.
    - A content creation tool that understands your previous creative decisions and generates content that aligns with your overall vision.


- A bit of history: The 21st century has witnessed a remarkable surge in the development and deployment of intelligent agents across various domains. The evolution of powerful machine learning algorithms, coupled with the exponential growth in computing power and data availability, has enabled the creation of highly sophisticated autonomous systems. One of the most significant breakthroughs in this era has been the emergence of **Reinforcement Learning (RL) as a key approach for training intelligent agents.**

- The progress in RL highlights the potential of RL in enabling agents to learn and adapt to complex environments through trial-and-error learning and reward maximization.

<a name="13"></a>
### Agents in Reinforcement Learning

Reinforcement Learning (RL) is a subfield of machine learning that focuses on training agents to make sequential decisions in an environment to maximize a cumulative reward signal. In RL, an agent interacts with its environment by taking actions, observing the resulting state, and receiving rewards or penalties based on its actions. The goal here is to learn a policy, which is a mapping from states to actions, that maximizes the expected cumulative reward over time.

Note that while reward has a significant impact on how the agent behaves, it is not an internal property of the agent. It is instead a property determined by the designer of the system (part art, part science) to help it learn the desired goal seeking behavior. In other words, the reward in this case is externally driven (as opposed to human behavior, for example, where the incentives could be internal or external). 

<a name="14"></a>
#### Properties of RL agents

We can explore the key properties we mentioned at the beginning of this article for RL agents. 

- Autonomy: RL agents can make decisions independently based on their policy, without requiring explicit instructions or supervision.
- Interactivity: Agents continuously interact with their environment, including other agents, by taking actions and receiving feedback in the form of rewards or penalties.
- Adaptability: Through trial-and-error learning, RL agents can adapt their behavior based on the feedback they receive, allowing them to improve their performance over time.
- Goal-orientation: RL agents are driven by the objective of maximizing cumulative rewards, which enables them to learn optimal strategies for achieving specific goals.


<a name="15"></a>
#### Agents in Natural Language Processing

LLM agents are a new class of AI systems that combine large language models (LLMs) with the ability to make informed decisions, take actions, and work towards specific goals. They can be described as a system that uses an LLM to reason through a problem, create a plan to solve the problem, and execute the plan with the help of a set of tools. LLM agents are typically characterized by 3 properties:

- Memory (equivalent to environment in the context of RL)
- Tool usage (equivalent to actions in the context of RL)
- Planning (equivalent to policies in RL mapping states to actions based on the current state to maximize reward)  

This concept enables LLMs to analyze information they encounter and choose the most appropriate tool for the task at hand based on their available policies. This empowers them to make informed decisions and achieve their goals. This is exactly what we humans do. When we have a task to solve, we gather information, we look for ways and tools that help us to solve this task as easily as possible. Memory and tool usage are relatively well established, but planning has still significant room for debate and improvements. 

<a name="16"></a>
#### Agents and Autonomy

One of the important characteristics of agents is their **level of autonomy characterized by their ability to execute increasingly complex tasks with little to no supervision (correlated with the complexity of their policies).** A software system at a low level of autonomy might resemble tools and one with a high level of autonomy might behave like an agent. While there is no clear cut differentiation, comparison to autonomous driving can be illuminating.

<a name="17"></a>
### Agents and Control Flow

In the near term, overcoming these challenges hinges (depends) on robust control flow mechanisms. **Control flow dictates how the LLM agent navigates interactions, makes decisions, and ultimately achieves its goals.** Without it, LLM agents risk producing **nonsensical outputs, deviating from intended tasks, or simply becoming unstable.**

Imagine an LLM agent designed to write customer service emails.  Control flow ensures it understands the situation (e.g., complaint, inquiry), retrieves relevant information (e.g., customer details, order history), and crafts a professional and appropriate response.  This might involve routing the user's request to the appropriate department or dynamically generating different email templates based on the issue.  Control flow keeps the agent on track, preventing irrelevant tangents or factual errors.

Effective control flow also addresses the challenges of measuring progress and integrating with existing systems. By establishing clear decision points and expected behaviors, developers can create metrics to track the LLM agent's performance and identify areas for improvement.  Furthermore, control flow allows for the integration of human oversight and intervention.  Developers can design control mechanisms that allow humans to guide the LLM towards desired outcomes or step in when the agent encounters unexpected situations. In essence, control flow acts as the bridge between the raw power of LLMs and the need for stability, reliability, and human oversight in real-world applications. See also:

How to do retrieval augmented generation (RAG) right!
How to do retrieval augmented generation (RAG) right!
Amir Feizpour (ai.science)
·
May 22
Read full story
You can think of control flows as generalized policies that come from a deep understanding of the workflow the agent is trying to automate. They could include things like:

Guardrails: Type of control flow specifically designed to restrict the LLM's behavior in certain ways. They act like safety rails, preventing the LLM from venturing into undesirable areas or generating harmful outputs. For instance: preventing offensive language, staying on topic, and fact-checking.

Routing: LLMs can be used to make decisions about how to respond to a user's query. This can involve classifying the query type (e.g., question, request, instruction) and then directing the response accordingly. For instance, an LLM might predict the best response to a question is a factual summary, while a request might require completing an action (like booking a flight).

Error Handling and Recovery:  When an agentic system encounters an issue, control flow mechanisms allow it to diagnose the problem and take corrective actions. This might involve prompting the user for clarification, reformulating a request, or attempting alternative strategies to complete the task.

Prioritization and Decision Making:  Agentic systems often juggle multiple tasks or goals. Control flow structures help them prioritize based on urgency, importance, or available resources.  For instance, a virtual assistant might prioritize responding to an urgent message over completing a less time-sensitive task.

State Management:  Many agentic systems track their internal state (e.g., conversation history, user preferences) to provide a more consistent and personalized experience. Control flow dictates how the system updates its state based on new information and uses it to inform future actions. Imagine a chatbot remembering your previous order preferences while recommending a new product.

Learning and Adaptation:  Advanced agentic systems can learn and adapt their behavior over time. Control flow allows them to integrate newly acquired knowledge into their decision-making process. For instance, a recommendation system might adjust its suggestions  based on your past interactions and positive feedback.

Most common implementations involve training small, specialized models (not necessarily language models) that carry out tasks and provide information or constraints to the overall system including crafting prompts that maximize the likelihood of the desired LLM response. 







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

