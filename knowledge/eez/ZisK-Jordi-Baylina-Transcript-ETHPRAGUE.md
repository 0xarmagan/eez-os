---
title: 'ZisK: Pushing the Limits of Real-Time Proving - Jordi Baylina'

---

0:099 secondsThank you very much for having me here.
0:1212 secondsIt's an amazing venue by the way and amazing conference. So let's start this.
0:1919 secondsBefore going a little bit deep uh in what we are doing in Z, I would like to share with you how I see uh modern
0:2828 secondsblockchains. I mean how So what why tend to think about uh blockchains in the
0:3636 secondsfuture? Okay. So blockchains um since the last years we're separating what's the data and what's the execution part.
0:4747 secondsSo if we understand blockchains just as data uh we can just check I mean it's a
0:5656 secondsledger it's a public ledger it's a humanity ledger if you want is a place where you can register logs I mean you cannot go back as an as a ledger so you
1:051 minute, 5 secondsjust register those things okay and this itself this becomes like a a true you can understand a database that's
1:121 minute, 12 secondsincreasing its data its data only And probably that's very much what you need.
1:211 minute, 21 secondsSo on these ledgers, I mean if you take all these uh all these data for example in in in a normal in normal blockchain,
1:291 minute, 29 secondsthese data used to be transactions and we can do picture mean we can do uh pictures we can do queries on this data.
1:401 minute, 40 secondsOkay. Okay. So these normal queries mean we can uh and for example from these queries we can deduke the state we if we take all the trans all the Ethereum
1:481 minute, 48 secondstransactions we process them deterministically from the beginning and actually we could calculate I mean what's the current what's the current
1:551 minute, 55 secondsbalance of my account or what's the uh whatever it's the we know a unique things of the state okay there were at
2:032 minutes, 3 secondsthe beginning of the blockchain I remember some some chains some I mean the old people some some of them some of them There was one that was called facto
2:122 minutes, 12 secondssomething like that that was very much that just you just put data and then the way you uh read this data the way that
2:192 minutes, 19 secondsyou interpret this data that's not it just needs to be like a consensus but it's like it's something that's already
2:262 minutes, 26 secondsknown and if we agree in how we read this data we don't need we don't need much more than that. Okay. So these queries we can actually calculate the
2:352 minutes, 35 secondsqueries. The problem of a blockchain like that that like that is that everybody that connects to a blockchain and wants to know the data they need to synchronize from the beginning and they need to interpret all this data.
2:462 minutes, 46 secondsCurrently with uh zero knowledge we can do those queries much easy. I mean we
2:532 minutes, 53 secondscan do the query somebody maybe computes that data but they can create a proof that the concrete
2:592 minutes, 59 secondsum result of this query is correct. And not only that, but I can do queries of a queries and queries of a queries of
3:073 minutes, 7 secondsqueries. We have this recussion. I mean this tool of I mean you're doing a query of a state you prove the state and then you get create another proof that
3:153 minutes, 15 secondsverifies the query of a query. Okay. So if every time that there is a new block we just compute the new state and we can compute a query and we have the this
3:243 minutes, 24 secondsstate. Okay. So this is this idea of we can separate the idea of um we call that dynamic consensus and the static consensus. So static consensus is what
3:333 minutes, 33 secondsokay we agree we don't uh doesn't change it's something that's fixed it's something that we are we this the way that we read this data the way that we
3:423 minutes, 42 secondsquery this data this agreement that we have that this data means this okay and dynamic consensus would be like the the
3:483 minutes, 48 secondsmean is the data that in every block is happening okay so we need the blockchain when we want to change this uh uh this
3:583 minutes, 58 secondsconsensus okay so when there is something that something that changes. We need to reach a new consensus here.
4:034 minutes, 3 secondsOkay. Now, forex, well, we need blockchains for forex because it's kind of a dynamic. There is a part that changes. So, we need to somehow signal
4:124 minutes, 12 secondsuh somewhere that this is the new the new state. Okay. And uh moving forward to this concept, I mean we have transactions, but we can have other
4:204 minutes, 20 secondsthings. One example, for example, is linking this with a with identity. I mean, I can have transactions, but I can have attentions or information, other
4:284 minutes, 28 secondsthings that are related. And we can read this data in a in a in a specific way.
4:344 minutes, 34 secondsOkay. And all this is possible thank you to this zero knowledge because we can prove these things. And when this uh and
4:424 minutes, 42 secondswhen we can generate these queries that are complex and we can generate them very fast, this model becomes more useful. Okay. And this is the the the
4:504 minutes, 50 secondsthings the the vision that I'm I'm we are seeing uh um on that. For example,
4:584 minutes, 58 secondsanother example is I mean we we we are applying a little bit this this concept in uh in in eit in Ethereum economic zone. But the idea is when we have many rollups that interact with each other.
5:095 minutes, 9 secondsHere we are talking only about rollups.
5:115 minutes, 11 secondsUm so if we just put the transactions of all the rollups in this chain we can query I mean we can deterministically
5:195 minutes, 19 secondsum execute those transactions even if they touch different rollups. And of course we will not have one state. We
5:265 minutes, 26 secondswill have one state per rollup. But the the uh the the result the state of each rollup is going to be deterministic. Of
5:355 minutes, 35 secondscourse maybe one transaction is going to touch one of these difference. But the only thing we need in order to do synchronous composability between rollups is just put the transactions
5:435 minutes, 43 secondsthere and read them. So you see that the how powerful this uh this this can be.
5:505 minutes, 50 secondsOf course this is more complex because we have also layer one and of course this state they can change but as idea
5:575 minutes, 57 secondsuh I think it's interesting to see to see this okay and finally well I want to mention I don't want to go deep here I
6:046 minutes, 4 secondswant to mention that this um model also works with privacy the difference here is that well uh the difference here is that this state is somehow encrypted and
6:136 minutes, 13 secondsthen you need to have a way to encrypt the crypt here there are some technologies is but they are not
6:216 minutes, 21 secondsavailable. I think the next next next talk is about offuscation that's something that is very promising but using offuscation and so you can have
6:286 minutes, 28 secondsall this all this uh model but with privacy in mind okay so
6:366 minutes, 36 secondswhy and then let's talk a little bit about this well zisk is just a zkm it's
6:426 minutes, 42 secondszkm uh it's just a um well how you create this proof I mean how you do these queries you do these queries
6:506 minutes, 50 secondsmeans that you need to execute I mean you get some data you need to execute something deterministically and and then you need to generate a proof okay so
6:596 minutes, 59 secondsthis is zisk is it very much helps a lot here so zisk is just a program that where you can write any program in rust
7:057 minutes, 5 secondsor in C or any normal language and you just uh generate a zero knowledge proof of this uh uh of this computation okay
7:147 minutes, 14 secondsand it's open source it's fast it's secure it's 128 bit security um it's quantum resistance
7:227 minutes, 22 secondsuh the idea is to audit and to formally verifi verify that and um I mean it
7:307 minutes, 30 secondsscales very well I would say it's one of the top levels um libraries in in in in in the current day but probably the the
7:407 minutes, 40 secondsmain I mean the main property of ZISK is that it was designed with latency in
7:477 minutes, 47 secondsmind I mean the idea is to be able to generate these proofs as fast as we can.
7:527 minutes, 52 secondsOf course, also as cheap as we can and with as performance as we can, but the main priority was to do it fast. And in
7:597 minutes, 59 secondsthis presentation, I'm going to talk a lot about how so all the things that we did in Zeiss to make this proof as fast
8:078 minutes, 7 secondsas possible because when we want to have information, we want to know fast and we want to have this information fast. When you have a when a block is minet, we
8:168 minutes, 16 secondswant to have a proof of that block as soon as as possible. We will see that for many applications this is this is important. Of course this has a lot of
8:248 minutes, 24 secondsother applications but all of them they have in common that you need to do them fast. Also mentioned that um in ZISK is
8:348 minutes, 34 secondsuh I mean it's not a new it's not a new project or at least it's not a new team. It's a team that we have
8:418 minutes, 41 secondsbeen working uh now about five years already in zero knowledge. Um I mean we built Hermes, we build
8:508 minutes, 50 secondsPolygon, ZKVM. We have been running uh we are of the few one of the few teams that have been running uh uh ZK systems
8:578 minutes, 57 secondsin productions for many times. So and we have a lot I mean we collect a lot of experience in the last years. Okay. So
9:059 minutes, 5 secondsDanisk is the evolution. I mean it's the evolution of this team. That's I mean it's like the cutting edge technology uh
9:119 minutes, 11 secondsuh of the of the of all the learnings of the last uh those those years. Okay.
9:199 minutes, 19 secondsZISK is um I mean if you check at the architecture of of of ZISK of course ZISK is a is a RS uh it's a RIS 5 uh VM.
9:279 minutes, 27 secondsOkay. But this risk 5 VM is built on top of a of a a generic ZK system is the the
9:349 minutes, 34 secondsproofman and this is all all the elementization is written in in P2. a specific language for writing arithmetization that allows us to make
9:439 minutes, 43 secondsso all the secure things I mean all the arithmetization is compact in a very few um lines of course that can be easily
9:529 minutes, 52 secondsaudited and it's very very specific to that and on top of that we have the recursor that's a a package I mean it's
9:599 minutes, 59 secondsa a framework that allows us to uh aggregate um proofs and to just generate a proof of proof or a query for another
10:0710 minutes, 7 secondsquery This allows you to do it automatically and in a very easy way on top of on top of Zisk. Okay. Um if we go
10:1610 minutes, 16 secondsa little bit deep how how what's the the core part of ZISK. So the idea is that the idea of the main the important part
10:2510 minutes, 25 secondsof ZISK is that when we have an import a huge query I mean I have I imagine a program that's quite long. We have a way to divide the problem in many problems.
10:3410 minutes, 34 secondsactually it's a big proof and we divide it in in sub proofs or in in chunks of proofs. Okay. And uh so and then we we
10:4210 minutes, 42 secondswe can do those subroofs like in parallel and then we aggregate those proofs in a big one. Actually they are
10:4910 minutes, 49 secondsnot regular proofs because all these proofs are built in parallel in the same transcript. This allows us to connect
10:5710 minutes, 57 secondsone each other but this is what allows us to to to distribute it. Okay. And um we have a we we we separate for so we
11:0711 minutes, 7 secondscan for example have an execution and we can divide the execution in chunks.
11:1111 minutes, 11 secondsOkay. But also we we we separate that functionally functionally mean we have a a specific u uh circuits that are for
11:1811 minutes, 18 secondsthe RAM others are for arithmetic operations for binary operations for catch for hashes. So we have a very specialized uh uh circuits that you can do bunch of operations of a single kind.
11:3011 minutes, 30 secondsOkay. And then we join them together in what we call a kind of a bus. Okay. So the idea is that all these circuits they connect one each other. I mean they from
11:3811 minutes, 38 secondsone circuit to the other they can send like message. Actually they are not really message. They are things that are
11:4411 minutes, 44 secondsassume in one side uh that you prove uh uh in the other side. So you have only one circuit for example in the main machine that you are assuming that one
11:5311 minutes, 53 secondsmultiplication is correct and then you have another circuit that actually is proving multiplications. So that and you have this boss that says okay the
12:0012 minutesmultiplication that you assume that was correct here actually are proving that's correct in this other circuit and this all these steam machines they connect
12:0812 minutes, 8 secondsthrough this boo this through this boss and at the end we just check that everything is is is is proven okay well
12:1512 minutes, 15 secondsthis allows us to do u different uh different this bus allows us to do different techniques
12:2312 minutes, 23 secondsum going to jump this a little bit but uh yeah This is the the core of the of the bus. Okay. But let's go to the
12:3212 minutes, 32 secondsdifferent techniques that we are using in order to make it fast uh with low
12:3912 minutes, 39 secondslatency. That's very much the goal. So the main goal of this uh project in the last two years that I have we have been
12:4812 minutes, 48 secondsworking currently. Okay. So when we have um um a proof I mean generation a proof.
12:5412 minutes, 54 secondsWell actually we we we have an execution. Okay. probably in this execution sometimes we have to uh
13:0013 minutesprepare uh that execution. For example, if we want to proof an Ethereum block, well, first there is a preparation. I
13:0713 minutes, 7 secondsmean maybe we want to extract from an Ethereum client. We want to extract uh
13:1413 minutes, 14 secondsall the stateless uh uh witness. I mean it's alo all you need to all you need to do to proof a block in a stateless
13:2213 minutes, 22 secondsmanner. Okay. So this would be like a pre preprocess. Okay. we have the execution itself. I mean if you want to prove an execution actually we need to
13:3013 minutes, 30 secondsexecute. We cannot prove an execution without executing it. Okay. So that's one of the places is executing it. Okay.
13:3713 minutes, 37 secondsAnd u this execution in the case of disk is in risk five. So the first one of the first problems that we have is that okay
13:4513 minutes, 45 secondsum one of the b first bottleneck is like okay if it's risk five I need to execute code in in ris 5 and I will not be able
13:5413 minutes, 54 secondsto prove faster that the time it takes to prove currently the system is mono thread there are techniques for doing multi
14:0214 minutes, 2 secondsthread but there's mono thread so this give us a limitation so the first thing is how we can uh uh execute
14:1014 minutes, 10 secondsrisk five code uh fast. Okay. Once we have this code uh executed
14:1714 minutes, 17 secondswell the idea is that we divide this work. So we have a kind of a content plan is do we we do a strategy. Okay. We are going to do so many mains so many
14:2614 minutes, 26 secondsbinaries. So we are doing a plan of how many proofs proof we're going to build.
14:3014 minutes, 30 secondsOkay. And then we in parallel we build all those proofs. Okay. These proofs are has in general two parts. One is the what we call the witness computation.
14:3814 minutes, 38 secondsThis is a part that's if you want is a part that's done in the CPU and another part that's on the GPU that's more cryptographic mainly entities and and
14:4614 minutes, 46 secondsmercalization for a start that's very much we're using and once we have all those proof well there is a final stage that we need to
14:5314 minutes, 53 secondsaggregate them so we need to join them so we take two proof and we convert in a single proof it's a and it's a recursive circuit that it's a proof that's proving
15:0215 minutes, 2 secondsto proof so we we start with two proof we have one and we just do kind of a tree and we have a final proof and a lot
15:1015 minutes, 10 secondsof times is this is a stark if you want to proof it on chain we may have a last state that's it converted to a stark I
15:1815 minutes, 18 secondsmean we just it's a proof it's a it's a normal plon or a normal gross 16 proof that actually is proving a stark okay so the end we have a a proof that
15:2615 minutes, 26 secondsconcentrates everything here okay so what can we do here okay well one of the techniques that we are doing is for
15:3415 minutes, 34 secondsexample well in the in the first two stages the first thing is Okay, we can do all this preparation. We go to the
15:4215 minutes, 42 secondsEthereum block and so on and then start the execution or we can do it a little bit more smart. So the idea is that we
15:4915 minutes, 49 secondscan do those things in in in a pipeline way. So we we are preparing and while we are preparing u the proof we already start executing
15:5815 minutes, 58 secondsthat proof. Okay. So this pipelining the inputs. This is one thing for example that if we are planning to do latency we we can do we can use this technique.
16:0716 minutes, 7 secondsOkay. And here we have like a good reduction uh for for proving blocks in real time. This is an
16:1616 minutes, 16 secondsimportant well this is one of the things that you can you can do. Another thing that another technique that we can do is
16:2316 minutes, 23 secondswell rising execution we have a when you're executing one of those things in in a
16:3116 minutes, 31 secondsthe serial way you have like a lot of pieces of the code that are quite repetitive. Okay. So for example we call
16:3916 minutes, 39 secondsit pre-ompiles. here we can't have hashes or we can have uh uh signature uh verifications or things that are very
16:4816 minutes, 48 secondsconcrete and and very parallel. Well, one of the things that we can do is okay, so instead of executing everything
16:5516 minutes, 55 secondsin the risk 5 execution, the idea is that we can have a native execution done in in in intel
17:0317 minutes, 3 secondsthat's just um doing this um it's like kind of helping the ru 5 execution. So
17:1017 minutes, 10 secondsthe idea is okay you are precomputing all these u I don't know this u is it
17:1717 minutes, 17 secondsrecovers or these hashes or these uh pings things okay and uh when the the
17:2417 minutes, 24 secondsrisk 5 is having to execute that it will give him the result already I mean it will preprocess it will help them
17:3117 minutes, 31 secondsbecause then at the end this is going to be maybe proven in another pre-ompile so the risk five does not need to execute there so they they they can hint them
17:3917 minutes, 39 secondsthey can send hints and and so overall then the execution can go uh faster.
17:4417 minutes, 44 secondsOkay, another thing to do if I mean if you see here you see that most of the work in a proving system can be parallels. We can
17:5417 minutes, 54 secondsbuild this thing in parallel but there is this execution part that's harder to paralyze. So the idea is that we want to um start paralyzing as soon as possible.
18:0518 minutes, 5 secondsSo we want to have an execution and we we need to split this execution in multiple executions so that we can start using multiprocess as soon as as soon as
18:1318 minutes, 13 secondspossible. So one of the techniques that we are using well first this execution one of the things that we are doing is
18:2018 minutes, 20 secondswe are transpiling this execution to uh so we we convert the ris 5 assembly to Intel assembly the code that we're
18:2718 minutes, 27 secondsexecuting exactly the same. And this can I mean this transpolation this allows us to execute this five in around 2 GHz
18:3518 minutes, 35 secondswhich is very close to the uh clock speeds of Intel uh uh by there okay so it's a very optimal code okay but we
18:4518 minutes, 45 secondswant to parallelize we want to parallelize that how we parallelize well the idea is we execute that normally
18:5318 minutes, 53 secondsokay but what we are doing is um we are storing I mean the idea is that Every time that we are doing a read of a memory, we are just um creating a lock.
19:0319 minutes, 3 secondsEvery time that we do a read, we store this lock. So the idea is that then we can execute all these chunks again, but
19:1219 minutes, 12 secondsinstead of having memory, you have this log. So when you need to to read something from the memory, you get this log. Writing to a memory, I don't care about the memory. I don't have memory.
19:1919 minutes, 19 secondsOkay, I already did it the first time.
19:2119 minutes, 21 secondsOkay, so this allows us to do a first pass a first execution of the code creating this lock and from there I can
19:3019 minutes, 30 secondshave different chunks and I can run in multiple processes and running it in in in in in parallel. Okay. And from there
19:3719 minutes, 37 secondsI can do the winners computation and the and the global. So this allows me so I'm executing even when I execute the first
19:4419 minutes, 44 secondschunk I can already start computing the finished computation of that chunk. Even while I'm executing the first time I'm
19:5119 minutes, 51 secondsalready uh starting to uh compute the proofs for this uh given chunk uh uh in
19:5919 minutes, 59 secondsparallel. Okay. So so from here we go in parallel. Okay. But this is this works
20:0820 minutes, 8 secondsin a single machine. Okay. But the idea is that we can do the idea in multiple servers. So because
20:1720 minutes, 17 secondswe want to to build that fast. So we want to parallel not in one single server but in many servers. So the strategy here is that this execution
20:2620 minutes, 26 secondswe are not doing once and then distributing to the servers. We are executing the same thing in all the
20:3320 minutes, 33 secondsservers. So all the servers they know what they're going to do. And then the parallel part each server they already know in a deterministic way they don't
20:4120 minutes, 41 secondseven have to communicate which are in this strategy which which piece they are
20:4720 minutes, 47 secondsgoing to they are going to uh uh have to execute I mean the first server is going to do maybe the first uh chunk the second chunk and maybe this first chunk
20:5620 minutes, 56 secondsof the binaries and whatever and this is a deterministic uh uh distribution. So without communications all the servers
21:0421 minutes, 4 secondscan be done in in parallel. Okay. So we can have this is uh one of our clusters that we have in in Giron. So we can have
21:1221 minutes, 12 secondslike many machines working together. All of them they are ex doing the execution but each one is doing each each of the each of the each of the of the pieces.
21:2421 minutes, 24 secondsOkay. And maybe one of those or one of those is they are finally uh aggregating all of them. Actually the aggregation
21:3221 minutes, 32 secondsthe technique for aggregation they don't need to follow a specific pattern on the tree they can you can aggregate to proof no matter the first comes first
21:4021 minutes, 40 secondsaggregate and you can use different servers so the aggregation is really fast actually the aggregation circuit is very optimized it takes less than I
21:4721 minutes, 47 secondsthink it's around 80 85 milliseconds one single aggregation and can be done in parallel so aggregation is very very optimized in the case of uh in the case
21:5621 minutes, 56 secondsof Cisco okay and it's important to see that the interactions
22:0322 minutes, 3 secondsuh between the servers and if you want what would be a coordinator are absolutely minimal because I mean you had the input you get the input of
22:1122 minutes, 11 secondscourse you need to broadcast the input to all of them all of them execute each one already knows what they have to execute there is a couple of u there is
22:1822 minutes, 18 secondsa couple of interactions a pocket of back and forth um because I mean it's this as I told you these proofs are built in parallel so there is that they
22:2722 minutes, 27 secondsneed to have a a single point is single challenge that needs to be broadcast and then the aggregation part. So we have a lot of proofs at the end have a lot of
22:3522 minutes, 35 secondsproof at the end and then all this needs to be aggregated at the end. But the the important part here is that the the
22:4222 minutes, 42 secondsbandwidth between the coordinator and the server is very low and this allows us for example so this is very good if
22:5022 minutes, 50 secondsyou want to build this proof in a decentralized way. I mean you can have um uh three servers in Asia, another in
22:5722 minutes, 57 secondsuh Europe, another in America. Uh and the latencies will affect minimally in the proof uh generation. And this is
23:0623 minutes, 6 secondsbecause you don't really need a cluster to bleed. You need a set of computers coordinated but minimally coordinated
23:1323 minutes, 13 secondsfrom the network. Okay. So this is one of the important properties of uh uh of this. Okay. And of course at the end we
23:2023 minutes, 20 secondshave a this plunk wrapper which is optional but if you want to prove this on chain then this is uh uh well uh uh
23:2823 minutes, 28 secondswe have this piece already done currently is uh it takes less than two seconds and there are some margin here to to improve proving that on chain is
23:3623 minutes, 36 secondsabout 250k it's so it's not gross you don't need a trusted setup but you need a universal system but it's already
23:4423 minutes, 44 secondsthere so you don't need to worry about building a ceremony and so on and it goes relatively fast and it's 100%
23:5223 minutes, 52 secondsavailable as I as I told you before is everything is open source okay and on top of that we have also the recursor
24:0024 minutesthis is we are working hard currently on documenting and making it more available but everything is written already so the idea is that you can have two
24:0924 minutes, 9 secondsindependent proofs or three independent proofs for example you have a proof of two blocks or three blocks and you can aggregate them in a single block but this is it's a different concept it's one thing is like a inner aggregation.
24:1924 minutes, 19 secondsSo if we have a building a single proof that's splitted in there but once you have the proof you can take another proof and and aggregate them. Okay. So
24:2724 minutes, 27 secondsyou can uh have two real proofs and then technique is very similar but you aggregate real proofs and you can aggregate them using the using the the the recursor. Okay, some benchmarks.
24:4024 minutes, 40 secondsWell, you can you can check it in f proof. For example, there we have some clusters running. Uh but uh I mean what's clear is that proving blocks in real time is is is is a fact actually.
24:5324 minutes, 53 secondsThese are the numbers. U 97 96% this is with u I think this is uh uh
25:0225 minutes, 2 secondsJirona cluster is 16 GPUs in CIA we have 24 GPUs. This can go lower especially for the clients. Clients are getting
25:0925 minutes, 9 secondsbetter and there is some margin to improve uh with the clients but what's clear is that they can we are talking
25:1725 minutes, 17 secondsabout in the order of few seconds to generate a proof actually for for easy for Ethereum economic zone there is one
25:2525 minutes, 25 secondstime that's really important I mean it's like so if you are building we have this composer this bundle mean we are taking transactions of different chains and we
25:3425 minutes, 34 secondswe need to build a kind of a a bundle with all these transactions that do all this crosschain communication. Well, the
25:4125 minutes, 41 secondsidea is that while you are building this, you can already start building this proof and the idea is this time I mean since since you finish the bundle
25:5025 minutes, 50 secondsuntil you get you get the proof available how long this takes. Okay. And this is very and this is it's below uh
25:5725 minutes, 57 secondsbelow uh three below 3 seconds currently and there is margin and this is what allow us to to make is easy possible
26:0526 minutes, 5 secondsthis low latency. you can you you need to you can only build those proofs when you already have a block and you have only 12 seconds to do all this. So
26:1426 minutes, 14 secondsthat's why you need this low latency and this becomes super important. Okay, last thing uh uh well we are current state of
26:2426 minutes, 24 secondsZISK is I mean it's uh everything is I would say it's finished. We are currently doing some clean up uh of the
26:3326 minutes, 33 secondscode. We expect to have a version 1.0 zero. Um the first week in the next next weeks um it's going to be probably it's
26:4126 minutes, 41 secondsgoing to be a 1.0 zero beta because of course here there is a lot of security and things that needs to be run and uh
26:4826 minutes, 48 secondsalso we are working hard on the formal verification but currently um if you are a user you would not notice anything
26:5726 minutes, 57 secondsfrom what would be like the final uh version for for at least for for the recursor here there is some work that we
27:0527 minutes, 5 secondsare still working but if you are just doing normal proof I fully recommend to test it it's really easy we simplify it a lot all
27:1127 minutes, 11 secondsuh installing and all the user experience uh for that. Okay. And that's very much uh what I wanted to explain today. Thank you.

