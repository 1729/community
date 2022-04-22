In podcasts and other venues, a16z's [Chris Dixon](https://twitter.com/cdixon) has described the initial wave of web3 apps as "skeuomorphic." What he means is that these web3 apps imitate the design of earlier, monolithic web2 apps, in much the way that some early PC apps visually imitated the design of real-world objects (e.g., a notepad app that looks like a yellow notepad).

I have a bit of a love-hate relationship with Chris's adaptation of "skeuomorphism" – I love it because it's evocative and gets at something real about the current state of web3, but my problem with it is that it only tells us what not to build. It doesn't offer any forward guidance.

Chris's assumption is that the ecosystem will eventually move on from skeuomorphism as it matures via the magic of the market, and he's surely right. But I think we can also give that maturation process a little push by actively looking out across the computing landscape for positive, constructive clues to the kinds of apps that [web3's new digital primitives](https://future.a16z.com/tokens-are-a-new-digital-primitive/) make possible.

Because all of computing is fundamentally cyclical at all levels of abstraction, it makes some sense to look back to an earlier turn of the wheel for a positive vision of what should come next with web3. Specifically, I think we should go back to Unix, and the philosophies that it embodied.

Eric S. Raymond's _The Art of Unix Programming_ is the classic text on the philosophical underpinnings of the Unix operating system. I actually haven't read the whole thing, but as a member of the Slashdot generation, I've read enough ESR and been exposed to enough of his ideas to have a good sense of the book's basic ideas. Twitter user [@brandur](https://twitter.com/brandur) has put together a really great [summary of ESR's writing](https://brandur.org/small-sharp-tools) on Unix, and really you should pause here and read the whole thing before proceeding.

I've often heard the Unix philosophy summarized as "small pieces, loosely joined," but @brandur reminds me that there's an alternate description from ESR that we also used to quote back in the day: "small, sharp tools." Here's the full quote:

> One strain of Unix thinking emphasizes small sharp tools, starting designs from zero, and interfaces that are simple and consistent. This point of view has been most famously championed by Doug McIlroy. Another strain emphasizes doing simple implementations that work, and that ship quickly, even if the methods are brute-force and some edge cases have to be punted. Ken Thompson’s code and his maxims about programming have often seemed to lean in this direction.

In other words, the power of the Unix command line is that it offers users a suite of compact, single-purpose programs like`ls`, `cat`, `ps`, and `grep` can be composed together on-the-fly via pipe operators to perform specific tasks. These programs all share an execution context – a logged-in user with a role, a permissioned filesystem, a stdout buffer, and so on – and because of that shared context a user can compose these tools in different combinations to perform different tasks.

As long as the maintainers of, say, `ps` continue to adhere to certain OS conventions, they don't really need to think too much about what the maintainers of grep are doing, especially under the hood. So a user like me can compose a command like, `ps aux | grep postgres` to see if Postgresql is running on my Mac or if I need to restart it.

## What about Martin Folwer's dragons?
Before I translate this into a vision for web3, I have to address the dragons that are already in the room. I'm talking, of course, about a famous essay by Martin Folwer: [MonolithFirst](https://martinfowler.com/bliki/MonolithFirst.html).

![image](https://user-images.githubusercontent.com/340258/164806163-e2b4d8e9-17e9-4926-8d7c-b0ca13abfaba.png) _Folwer's diagram, with the "here be dragons" jump to microservices_

All of us who've done web-scale programming in any professional capacity have developed a healthy aversion to "microservices first" – I know I sure have. Best practice is usually to start with a monolith, and then break it apart at service boundaries as that becomes cost-effective and feasible. Or, maybe you just never break it up; really big monoliths work great for some use cases.

The problem with microservices, at least in my experience (this isn't so much drawn directly from Folwer's essay), is twofold:
1. **Managing state** between services is a pain. Every service is going to have its own private datastore, so there are sync and cache invalidation problems inherent in the microservices approach. Sync and cache invalidation are Hard Problems, and you don't want them if you can avoid them.
2. **API design** is an annoying time-suck and a source of nerd fights. I'm a big fan of [JSONAPI](https://jsonapi.org/), but I have fought people over this because most nerds have big hot opinions about such things. Also, the maintainers of a group of related microservices all have to know about each others' APIs, so that's another flavor of sync problem that exists in the realm of API knowledge and documentation. This is "head state" rather than "app state," but do not discount the overhead of managing "head state" across multiple teams.

You don't have to worry about syncing state and doing API design if you can just make all the contexts internal to the application, such that everyone can share a database schema and a migration history. In a single-dB app, we nerds can limit our fights to data relations and naming things, and once the migration is run in production, even the nerds who lost the fight are not keen to roll it back and re-do it their way. Everyone moves on with their jobs.

But I'm not convinced that the above really applies to web3. Specifically, I think the blockchain has the potential to do away with a lot of the state management and API-related issues by providing a kind of shared execution context that's reminiscent of what the Unix command line provides for a logged-in user. Much of the complexity in both of these items is typically linked to the fact that many linked microservices share user and policy objects, but both of these things have the potential to be very different in a world where users are wallets, roles are NFTs, and the blockchain is the filesystem.

## A DAO is not an app, so it doesn't have to be a monolith
I've seen a decent amount of DAO tooling screenshots and mockups in the past months, some released and some unreleased. I have also written some DAO tooling, and am in the process of writing some more. This [proposed prototype screen](https://forum.bankless.community/t/dao-map-transparency-accountability/1937) that I dug up on Google, for Bankless DAO, is typical of some of the more ambitious tools I've encountered:

![image](https://user-images.githubusercontent.com/340258/164806530-07362bb6-2837-4fed-aa72-2ff5de6b72dc.png)

I'm most interested in that little navigation strip on the left, which indicates that there's a home screen (probably a dashboard), then a search view, an inbox of some kind for messages, an address book tab, and so on.

This screenshot says to me, "there are a bunch of `users` in a database, and each user has many `projects` that you can sort by `projects.status`…” So far, so skeuomorphic.

The question I have, though, is: if we take the [Billion User Table](https://1729.com/the-billion-user-table) seriously, and all those `users` are just wallets on-chain that have tokens in them, then **why are all these things part of one app?**

The internet is full of tables of milestones and Kanban boards, so why write a new one and tuck it all the way inside a "DAO tooling" monolith? If all my DAO users are just blockchain wallets containing certain tokens, and some of those tokens grant roles in the DAO, then even if I'm writing a brand new project management system for these users to collaborate via, does it _really_ have to be inside a larger app with a dashboard and all the bells and whistles?

A Trello clone that supports login with Ethereum and token gating, and that also acts as a blockchain oracle that can write events to the chain (X user completed Y project on Z date) would be usable by any DAO member with some tokens and a browser, could be developed in isolation by a separate team on whatever stack they like, and could have the on-chain part of its output aggregated in other apps and dashboards across a larger ecosystem.

A DAO's **dashboard** could also be its own app on its own stack. This app just scans the chain for state changes that signify different goals being hit, and anyone with the correct token can log in and view it.

Or, consider the **address book**. Does this really have to live inside the same app as the Bounty Board? A standalone DAO address book app might let anyone with a DAO token create some database entries that include their name, profile picture, location for meetups, social links, and other info. Anyone else who wants to search that address book can connect a wallet and, if that wallet has the right token with the right access, can see whatever it is they're allowed to see, or export entries in various common formats (JSON, vcard, CSV).

How about a **map** of where members are for meetups? A token-gated map could pull on-chain data that was written by the DAO's contacts app – or maybe ask the app directly for export by showing it a token – and display a map of wallet locations to whoever logs in with the right token. It would be very simple, and the team could use whatever tech stack they're most fluent in. The URL for it could be published on a token-gated web page for the DAO, so members can find it.

Given that the users and roles are all stored on-chain in the form of token-holding wallets, and that some kinds of app output can be written to the chain by treating it as a sort of `stdout` buffer (depending on the chain and the size of the state) or (probably more appropriately) a permissioned filesystem that any app can read from the chain, it just doesn't seem to me that a bunch of competing DAO back-office monoliths are necessary.

Instead of jamming a search function, an address book, a user map, an organizational map, a bounty board, and other tools of different kinds into a single monolith behind a single web interface, what if we thought in a Unix way with the following primitives:

- Users are wallets
- Roles are NFTs
- the filesystem is the blockchain (probably in combination with a distributed filesystem)
- `stdio` is... TBD? Most blockchains are a poor fit for this given how [costly and finite block space](https://twitter.com/balajis/status/1479863075301834755) is, but there may be some kind of more ephemeral, non-space-constrained, off-chain structure (like lightning channels?) that works as a stdout/stdin buffer where all the app output visible to a token holder can be dumped. 
- Maybe pipe operators aren't necessary because the apps are polling the chain for data.

Instead of a DAO management monolith, maybe what we need more of are the web3 equivalents of `ls`, `cat`, `grep`, `ps`, and the like – small, sharp tools that read from a stdin and write to a `stdout`, and that execute in a common context of users, roles, and permissions.

## Conclusion: things that are needed for the above to work
In order for the above vision to become reality, we may need a few more primitives. We probably actually already have these and I just don't know about them – if this is the case, please point me in the right direction.

First, we need the kind of **public NFT role-granting table** I described in a previous talk: [The Billion Role Table](https://docs.google.com/presentation/d/1Ao-_0Zaan4YMBC8ulwhD5EetwA_8jmnvqFe7-u7VcxM/edit). There's been [some discussion](https://engineerdao.notion.site/engineerdao/RBAC-With-NFTs-23542717d8a345eba0d7d8d3f4a403fb) of how this may be possible to implement with the existing ERC 721 standard. We should keep exploring this, and/or figure out how to do it on Solana.

Second, implicit at a number of points in the above discussion is the existence of a **parent NFT** that can be used to **mint valid child NFTs**, where we can cryptographically verify that a particular child was issued by the holder of a particular parent. So for the Feedback App example, I'd want to be able to issue (and also revoke, if necessary) a set of parent 1729 Feedback NFTs that different competing Feedback Apps can use to mint valid 1729 Feedback NFTs for hosts and attendees.

Finally, the blockchain – or even the blockchain plus a filesystem like Arweave – might replace the Unix filesystem in this picture, but it's probably not the best replacement for stdio. The chain is meant to be permanent, but what's really needed here is an **ephemeral buffer**. So I could envision a kind of blockchain-based version of Amazon SQS or even something simpler that just provides public, ephemeral buffers where apps can write an encrypted text payload that can only be unlocked with a specific bearer token or key of some kind.

I'd be shocked if all the above doesn't already exist in some form or another in the ecosystem. But internally to 1729, it would be good if we could bring all these elements together and begin to experiment with them in order to build a set of small, sharp tools for managing our community.

## Further Reading
- [Idea Legos](https://www.notboring.co/p/idea-legos?s=r)
- [Zodiac: The expansion pack for DAOs](https://gnosisguild.mirror.xyz/OuhG5s2X5uSVBx1EK4tKPhnUc91Wh9YM0fwSnC8UNcg)
- [Organization Legos: The State of DAO Tooling](https://medium.com/1kxnetwork/organization-legos-the-state-of-dao-tooling-866b6879e93e)
- [What is Composability?](https://blog.aragon.org/what-is-composability/)
- [daomasters.xyz](https://daomasters.xyz) 

