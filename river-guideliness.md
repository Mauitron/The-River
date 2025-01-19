# The River: Core Guidelines for Software Development

> I mark my insertions and comments as quotes. Which is unfortunately a somewhat inverse usage of the mark up.  
This is not meant as a start of a discussion, but as feedback on the text. I avoided questions. Do with this feedback as you wish: Ignore it, think about it, be convinced by it, understand it as being out of scope. It is up to you. Your essay and these guidelines clearly show, that you not only have thought about this in depth, but also invested a lot of effort into expressing your thoughts. 

## Fundamental Principles

### 1. Flow-Based Organization
Code should be organized around complete flows of functionality rather than arbitrary technical boundaries. Each significant feature should be treated as its own "river", a coherent path from beginning to end. The goal is to maintain locality, visibility and comprehensibility of the entire process.

> If those technical boundaries are indeed *arbitrary* I fully agree. Often they are required by the techniques used. If those are strictly necessary to achieve the goal is another discussion.

### 2. Complete Responsibility
Functions should aim to handle their entire responsibility from start to finish, regardless of length. Rather than fragmenting logic across multiple small functions, allow each function to tell its complete story. The key metric is not size but clarity, isolation and coherence of flow.

> Fully agree

### 3. Natural Boundaries
Systems should be divided along natural feature boundaries, like a watershed contains multiple rivers. Each feature flow should maintain its independence while connecting with others at well-defined points. These boundaries should emerge from the problem domain rather than being imposed by technical considerations.

> Feature flow is just one aspect on how do decompose/structure/model a complex system. Computer Science has to deal with the challenge to adhere to multiple decompositions at once. That is why aspect-oriented programming became a thing and why we move elements (realization of concepts) from code to annotations.

> You speak of *well-defined points* of interactions. Research will have to spend time, and experience will show what make a point *well-defined* with regard to feature flow.

> I agree that staying close to the problem domain is a favorable goal. I believe object orientation started to address that challenge. I rather see `Student` as a class name than `IMainDBObject`.   
One kind of abstraction (My [professor](https://www.se-rwth.de/staff/Manfred.Nagl/) identified three kinds, which I no longer remember) leads from the problem domain (very abstract) to the computer implementation (less abstract). Once the implementation is on those less abstract layers the line between the modeled problem domain and the computer domain blurs.

## Implementation Guidelines

### Code Structure
1. Keep the entire flow of a feature visible and comprehensible in one place
2. Allow functions to be as long as needed to handle their complete responsibility
3. Maintain linear, easy-to-follow progression through each function
4. Use clear section comments to guide readers through longer flows
5. Keep feature flows independent of each other where possible

> If *flow of a feature* is the only or main principle of structure, I fully agree. But, as I hopefully have commented in the main essay, I believe other principles are needed as well. 

### Abstraction Rules
1. Prefer local abstractions that serve immediate context over global utilities
2. Allow patterns to emerge naturally rather than creating abstractions upfront

> This is so true! I saw over-engineered code which added interfaces to every class or using a visitor pattern with only one visitor and one very concrete collection to traverse.

3. Abstract only when it genuinely improves understanding, performance  or maintainability. And do it only after reflecting on the potential cost vs benefit of doing so. 

> I believe the before mentioned visitor pattern was included to *improve* readability. If you are best friends with certain patterns and all other team members are as well, then using such patterns might indeed improve readability. At the time, at least one team member (me) found it way harder to understand.

4. Keep abstractions close to where they're used, preserving context
5. Accept similar patterns in different contexts rather than forcing reuse

> *Forcing* reuse does not make sense, agreed. If you scale the complexity of the problem domain, you will encounter the true *need* for reuse, because you will have to check 99 flows, if they also contain the bug you just fixed in 1 of 100 flows (I am aware that there are draw-backs on the reuse which might be harder to handle). 

### Error Handling
1. Handle errors within the flow where they occur

> In my experience error handling is hard. This is partially due to functional decomposition and abstraction, but will remain true if I have to handle an error after the river split up; if I have to pass the error over through a "well-defined point of connection" between flows.

2. Provide rich context in error messages by leveraging local knowledge
3. Design flows to contain and isolate failures
4. Make error states and recovery paths visible within the main flow

> I think the last two points have not been shown/discussed in the essay. If we were to discuss them, examples of what that means for you were required. 

### Cross-Cutting Concerns
1. Treat logging, security, and monitoring as part of the local environment

> This might hide the flow. How do you see/mitigate that?

2. Allow each flow to handle cross-cutting concerns in context-appropriate ways

> Nice, but I have *no* idea how to achieve that.

3. Accept that similar patterns may repeat across different flows
4. Focus on clarity and completeness within each flow rather than global consistency

> Agreed, but: Global consistency can in turn increase readability across team members and thus maintainability. Following some idioms is ok.  
If global consistency is required (e.g. You have to do C before A before B) to make things work, maintenance becomes hard (because people will "forget" to do C or A). Avoiding such global consistency completely, might impose abstraction (I hide doing C and A, from whoever does B) and might obscure the flow. Where to settle for compromise between those extremes is a matter of actual situation.

### Growth and Scaling
1. Add new capabilities as new rivers rather than complicating existing ones

> Good and important point. I might otherwise believe to follow the above guidelines by adding some if-statements to an existing flow.

2. Maintain constant complexity by isolating it within each flow while the system grows

> I am not sure I understand: Does it mean all the statements in flow should be on the same level of abstraction, e.g. the problem domain?

3. Allow each flow to evolve independently based on its specific needs
4. Focus on isolation and fault containment between flows

## Code Review Guidelines

### What to Look For
1. Clear progression of logic from start to finish
2. Appropriate handling of edge cases and errors within the flow
3. Rich context in error messages and logging
4. Natural and necessary abstractions that serve the local context
5. Proper isolation between different feature flows

### Questions to Ask
1. Can you understand the complete flow without jumping between files?
2. Are abstractions serving the immediate context or forcing premature generalization?
3. Is similar code truly duplicate, or serving different contexts?

> Rather: Do unified code segments server readability or just avoid code duplication?   
It seems to me you assume, that the reviewer found code duplication and likes to criticize it. 

4. Are boundaries between flows natural and appropriate?
5. Does the code tell a clear, coherent story?

## Anti-Patterns to Avoid

1. Breaking flows into small functions - unless with clear and worthwhile benefits
2. Creating global abstractions - unless absolutely necessary

> I propose: Creating global abstractions - unless for a concrete, well known cause.

3. Forcing reuse between unrelated contexts - unless absolutely needed

> I propose: Forcing reuse between unrelated contexts.   
I wouldn't know when reuse between *unrelated* contexts would be *absolutely* needed. I guess never.   
As these Anti-Pattern should only be avoided and are not strictly forbidden, I don't think an exception is required.

4. Obscuring flow with unnecessary indirection - unless with clear benefit

> I propose: Obscuring flow with indirection - unless with clear benefit   
A clear benefit makes the indirection necessary and the Anti-Pattern would no longer apply.

5. Separating related functionality for the sake of "clean code" - is always bad

## Decision Framework

When organizing code, ask:
1. Does this organization make the flow more or less visible?

> If my problem is realizing a flow, that is a valid question. If the flow is not the dominant problem, this question does not help.

2. Does this abstraction serve the immediate context or are its benefits so large it is warranted?
3. Is this repetition isolated or truly harmful?
4. Are these boundaries emerging from the domain or being forced?
5. Does this structure help or hinder understanding the complete story?

Remember: The goal is not theoretical purity but practical comprehensibility. Let the natural flow of the problem guide the structure of your solution.
