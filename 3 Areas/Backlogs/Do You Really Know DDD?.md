---
difficulty: Easy
---
- so you wanna use DDD in your project. why? if you're like how I was when I started learning DDD and answer "I just wanna do things the right way", I will present you a quote by a statistician George Box "all models are wrong, but some are useful". DDD won't prevent you from wrong modeling. Far from it.
- is hexagonal architecture the same/ an implementation of DDD? no, hexarch only decouples technical complexity and business complexity, it doesn't say anything about how to design/ manage the business complexity (we can model the hexarch domain using DDD.) still, it's a popular choice to pair with DDD due to its relative ease of replacing vendor technologies used within the applications without affecting the domain logic.
- is DDD the same as separating the data models into aggregates and entities? sort of, but data models are more of a byproduct of DDD (or any architectural design, really).
- is it just a set of technical jargons? while this is a massive oversimplification, from my experience it is the most helpful way to think/ talk about it. when every backend engineer knows what the entities (with invariants and value objects) and context boundaries are in a particular software project, you can talk about the behaviors of the application in a more productive way, not limited to data modeling for your particular database/ class/ function/ whatever.
- is X a thing or many things? Design homophones:  see people using the same word/s for different meanings
	- a "claim" or "claims" -> https://www.youtube.com/watch?v=Y0txTmT3k7M at 4:38
	- overloaded terms, e.g. a "category" or "categories" -> personal exp in Agriaku