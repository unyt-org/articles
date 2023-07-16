<!--
	{
		description: "Introduction to unyt.org's digital voting system",
		preview: "res/header-digital-voting.png",
		date: ~2023-07-18~,
		tag: "Security",
		author: "unyt.org",
		authorRef: https://unyt.org
	};
-->


# Introduction to unyt.org's digital voting system

Traditional paper-based voting systems, like those employed in most goverment elections (*US Presidential Election* / *German federal election*), are not only expensive and intransparent, but also prone to inefficiencies and [potential fraud](https://de.wikipedia.org/wiki/Bundestagswahl_2021#Unregelm%C3%A4%C3%9Figkeiten_in_Berlin_und_Einspruch_des_Bundeswahlleiters). 

The management and counting of votes require extensive human resources and can cost a stack of bucks... a big stack. With [unyt.org](https://unyt.org) we are introducing a better, cheaper, fully transparent and more secure alternative to traditional systems: **unyt.org's digital democratic blockchain-backed voting system**. 

By leveraging the power of blockchain technology, we can significantly reduce costs while ensuring the integrity of the democratic process and enabling anonymous transparency. In this article, we will dig into the basics of such a system and explain how it works within the [unyt.org](https://unyt.org) Supranet.

The process of paper-based voting typically involves several steps:

1. *Identification*: Voters go through an identification process to receive their ballot. A poll worker marks their name on a list to ensure they receive only one ballot.
2. *Voting*: Voters fill out their ballot and deposit it into a ballot box.
3. *Vote Counting*: After the voting period ends, poll workers manually count the votes.

While this method has its merits, it also possesses some vulnerabilities: Bad poll workers can **manipulate the process** by omitting certain ballots from the count or allowing malicious actors to vote multiple times. Moreover, the sheer volume of human work involved makes the system **susceptible to errors** (*yeah humans error... we are all kinda dumb, so why not use AI to run the world? /s*). Another drawback in those traditional systems is that as a voter, you cannot track whether your vote was actually counted or not. How can you be sure?


As it has always been - technology provides solutions to these issues: [unyt.org's](https://unyt.org) digital voting system combines cryptographical approaches with the use of a **distributed ledger** (blockchain). Our [HELIX](https://docs.unyt.org)-blockchain serves as a public shared and immutable database of transactions, allowing anyone to track the voting process in real-time and identify any malicious behavior. 

When an individual casts their vote in this system, their unique endpoint identifier linked to their human identity is recorded on the [HELIX](https://docs.unyt.org)-blockchain. The system checks whether the voter is eligible to vote and if they have already voted, making double voting impossible. Additional the immutability of the blockchain prevents the removal of votes since all this information is public shared accross the whole system at any time.

By implementing these measures, digital voting becomes more secure than traditional paper-based methods. The immutability and transparency of the blockchain enable full verifiability during the whole election process providing a more convenient alternative to in-person voting.

---
**But what about anonymity?** *How can this system maintain the same level of anonymity as paper-based voting?* - Okay, so then... let's talk about [Zero-Knowledge Proofs](https://en.wikipedia.org/wiki/Zero-knowledge_proof) (ZKPs).

ZKPs allow individuals to prove something without revealing any information about it. In the case of anonymous voting, a voter generates a public token and a secret token. After identification, they submit the public token to the system, which records it on the blockchain. When casting their vote, they utilize the secret token along with a ZKP, proving that the secret token corresponds to one of the public tokens without revealing which one. This ensures that each registered voter can vote only once while maintaining their anonymity. Importantly, this anonymity is mathematically verifiable through zero-knowledge proofs.


1. **Public Token Submission**: Instead of receiving a physical ballot, voters send their public token to the voting authority.
2. **Voter Identification**: The voting authority verifies the voter's identity and record the public token on the blockchain alongside the voter's unique identifier.
3. **Casting the Vote**: Voters use their secret token to cast their vote, which cannot be linked to their public token, thereby preserving their anonymity.


---

**Okay... but isn't anonymous voting impossible?** *With this method nobody can interlink votes with human identities - would't this allow bad actors to vote multiple times?* - **No!**

The legder stores 3 public lists to: 
1. **Vote-Request List** - Trusted endpoints (endpoint linked to their human identity) that want to participate in the election, send a vote request to an authority including the encrypted vote.
2. **Ballot List** - Response to 1. signed by authority to ensure each endpoint has only voted once.
3. **Vote List** - Published (Blind / Ring signature) of actual vote that is unlinkable but verifiable.

---


Combining those technologies we can form a digital system that allows for a fully decentralized, transparent, anonymous and trustless election:
* **democratic** - Every human eligible to vote can vote once
* **fully decentralized** - no need for trusted authorities
* **transparent and full of integrity** - each participant can verify that every single vote is real
* **immutable** - once voted nobody can ever change the election result or any single vote of this process
* **anonymous and unlinkable** - Nobody can link votes to the real entity of the voter

As demonstrated, it is entirely feasible to create a highly secure and cost-effective online voting system for the [unyt.org](https://unyt.org) Supranet. Such a system can be employed for government elections or any other scenario where democratic voting is required.