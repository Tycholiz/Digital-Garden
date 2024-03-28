This Dendron vault of tech knowledge is organized according to domains and their sub-domains, along with specific implementation of those domains.

For instance, Git itself is a domain. Sub-domains of Git would include things like `commit`,
`tags`, `reflog` etc. implementations of each of those could be `cli`, `strat`
(strategies), `inner`, and so on.

The goal of the wiki is to present data in a manner that is from the perspective
of a querying user. Here, a user is a programmer wanting to get key information
from a specific domain. For instance, if a user wants to use postgres functions
and hasn't done them in a while, they should be able to query
`postgres.functions` to see what information is available to them.

This wiki has been written with myself in mind. While learning each of these
domains, I have been sensitive to the "aha" moments and have noted down my
insights as they arose. I have refrained from capturing information that I
considered obvious or otherwise non-beneficial to my own understanding.

As a result, I have allowed myself to use potentially arcane concepts to explain other ones. For example, in my note on [[unit testing|testing.method.unit]], I have made reference to the [[microservices|general.arch.microservice]] note. If these notes were made with the public in mind, this would be a very bad strategy, given that you'd have to understand microservices to be able to draw that same parallel that I've already drawn. Since these notes are written for myself, I have been fine with taking these liberties.

What I hope to gain from this wiki is the ability to step away from any
given domain for a long period of time, and be able to be passably useful for
whatever my goals are within a short period of time. Of course this is all
vague sounding, and really depends on the domain along with the ends I am
trying to reach.

To achieve this, the system should be steadfast to:
- be able to put information in relatively easily, without too much thought
	required to its location. While location is important, Dendron makes it easy
	to relocate notes, if it becomes apparent that a different place makes more
	sense.
- be able to extract the information that is needed, meaning there is a
	high-degree in confidence in the location of the information. The idea is
	that information loses a large amount of its value when it is unfindable.
	Therefore, a relatively strict ideology should be used when determining
	where a piece of information belongs.
	- Some concepts might realistically belong to multiple domains. For instance, the concept of *access modifiers* can be found in both `C#` and `Typescript`. Therefore, this note should be abstracted to a common place, such as [[OOP|paradigm.oop]].

This Dendron vault is the sister component to the [General Second Brain](https://tech.kyletycholiz.com).

### Tags
Throughout the garden, I have made use of tags, which give semantic meaning to the pieces of information.

- `ex.` - Denotes an *example* of the preceding piece of information
- `spec:` - Specifies that the preceding information has some degree of *speculation* to it, and may not be 100% factual. Ideally this gets clarified over time as my understanding develops.
- `anal:` - Denotes an *analogy* of the preceding information. Often I will attempt to link concepts to others that I have previously learned.
- `mn:` - Denotes a *mnemonic*
- `expl:` - Denotes an *explanation*

## Resources
### UE (Unexamined) Resources
Often, I come across sources of information that I believe to be high-quality. They may be recommendations or found in some other way. No matter their origin, I may be in a position where I don't have the time to fully examine them (and properly extract notes), or I may not require the information at that moment in time. In cases like these, I will add reference to a section of the note called **UE Resources**. The idea is that in the future when I am ready to examine them, I have a list of resources that I can start with. This is an alternative strategy to compiling browser bookmarks, which I've found can quickly become untenable.

### E (Examined) Resources
Once a resource has been thoroughly examined and has been mined for notes, it will be moved from *UE Resources* to *E Resources*. This is to indicate that (in my own estimation), there is nothing more to be gained from the resource that is not already in the note.

### Resources
This heading is for inexhaustible resources. 
- A prime example would be a quality website that continually posts articles.  - Another example would be a tool, such as software that measures frequencies in a room to help acoustically treat it.
