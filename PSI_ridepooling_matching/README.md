# PSI ridepooling matching

Explore Private Set Intersection (PSI) with an inputless third party which could improve the standard two-party PSI (2P PSI) and is more practical in some usecases like ridepooling matching. The matching checks if the cardinality of route (a set of connecting edges) intersection is over a threshold. Traditionally, a rider/driver generate the route and provide it to the trusted third party. There is also other method for matching like point-based matching but it is not the focus here. A Privacy breaches can lead to identity theft, phishing, or malicious misuse by employees for personal gain. A solution to this is 2P PSI ride pooling. However, it suffers data leakage between parties because it runs matching on every pairs then sends all the possible matches for each party——each rider knows all matched driver and vice versa. Moreover, variants of 2P PSI like the standard 2P PSI and 2P threshold PSI has high computational or performance overhead. An interesting solution is to use a PSI with a third party (modeled as a semi-honest adversary). Work from https://eprint.iacr.org/2024/1109 proposed a protocol with security analysis and performance benchmarks. Our contribution is to extend to handle time of the ride, flexibility, route flexibility. Also, we create a simple demo to show how this can work with real data (route based on Openstreetmap's road network). A full system of multiple riders, drivers and a third party server with practical interactions are not implemented though a possible system is presented as an idea.

This project consists of 2 parts: modification of QuickPool's code, a triples generation code.
- User can act as a driver or rider to input their start and end position to create route triples to the database(mongoDB)
- Modified QuickPool's code fetch these triples from the db into its PSI with a third party (third party PSI).
    - Note that, this is just to mock input from rider and driver not real system.

Further explaination can be found in slides: https://docs.google.com/presentation/d/1HIrOMiZK4FcA88VesIeRQKzkMVmp_dyIA_9d5xN4dH4/edit?usp=sharing

## Team Information

**Project Members**

- Name: Waritwong Sukprasongdee
  - Discord Username: pngwaritwg
  - Devfolio Username: pngwaritwg
  - Github Username: pngwaritwg
  - Role: Developer

## Technical Approach

- **Components** (Select all that apply)
  - [ ] Frontend
  - [x] Backend
  - [ ] Smart Contracts
  - [ ] ZK Circuits
  - [ ] Machine Learning (ML)

- MPC: 2 party Private Set Intersection with a third party. First each rider and driver compute its route using its start and end position given a road network. Every rider-driver pair then perform just simple key exchange (not Diffie–Hellman key exchange). Each party (e.g. a rider A) use this key to mask its route Pseudo-Random Function(key, route) (the implementation use Pseudo-Random Permutation (PRP) based on the AES)for every other parties (e.g. driver D, E, F, ..). The masked routes send to the third party to check if the intersection length is above threshold.

## Sponsors (if applicable)

If you are applying for a sponsor project idea or grant, select the sponsors below.

- [ ] Push Protocol
- [ ] Polygon
- [ ] Chainlink
- [ ] Brevis
- [ ] Orbiter
- [ ] ZKM
- [ ] Nethermind
- [ ] PSE
- [ ] AltLayer

## What do you plan to achieve with your project?

What is the plan for this project from now on? Do you plan to continue to work on it? Do you want some help? How could we help you?
Design full system for real rider, driver and third party server interactions. Extend to handle group of riders to match a driver.

## Lessons Learned (For Submission)

- What are the most important takeaways from your project?
- 
Computational and performance overhead and security assumptions in MPC need to be considered. I find PSI with a third party simple somehow it  solve a question in my mind about the limitation of 2P PSI for ride pooling. The simplicity of the protocol make it easy for me to learn building components of it i.e key exchanging, masking inputs, PRF concept, OPRF concept in regular PSI. I also find some implementation tricks which is helpful for extension from the original paper.
- Are there any patterns or best practices that you've learned that would be useful for other projects?
- 
Batch processing for requests to minimize the overhead of multiple PSI operations. EMP toolkit's block data structure (a block of data, often 128 bits, which is a common size for cryptographic operations) allows for secure operations on cipher and improve secure computation e.g. PRP.
- Highlight reusable code patterns, key code snippets, and best practices - what are some of the ‘lego bricks’ you’ve built, and how could someone else best use them?
- 
Implementation of creating route triples {(node_i, node_(i+length of intersection threshold),time_(i+flexibility))} for time and flexibility of time. These triples will be compared against the PRF output from the other party. Since the triples already account for the intersection threshold, the intersection process only needs to check for a match——one match is sufficient. This approach enables the generation of multiple routes for each party; for instance, traveling from A to B can have several shortest paths, enhancing the flexibility of route matching.

## Project Links (For Submission)

Slides: https://docs.google.com/presentation/d/1HIrOMiZK4FcA88VesIeRQKzkMVmp_dyIA_9d5xN4dH4/edit?usp=sharing
Github repo: https://github.com/pngwaritwg/PSI_ridepooling_matching
## Video Demo (For Submission)
https://www.canva.com/design/DAGVoNaZqKY/1ZyA_9XGpIC5J3RggIXvng/watch?utm_content=DAGVoNaZqKY&utm_campaign=designshare&utm_medium=link&utm_source=editor
