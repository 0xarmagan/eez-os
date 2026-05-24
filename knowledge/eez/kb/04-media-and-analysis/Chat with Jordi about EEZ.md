---
title: Chat with Jordi about EEZ

---

Speaker 1: Armagan
Speaker 2: Jordi
Speaker 3: Ben


Speaker 1  (00:01)
Record with your Di yes was about E. Alright, the I have prepared prepared some questions to understand easy and then into the developer perspective. Firstly, like what is the path to deploying ease? 

Speaker 1  (00:19)
On the other chance, or what will be the first step in deploying? Google. 

Speaker 2  (00:26)
And the first thing is that What's a change chain very much is From easy perspective is state, which is represented by a state road. And Well, this is a state just evolves in the chain customer, 2 customer sections, and these estate change to a new state. Okay? 

Speaker 2  (00:54)
So first thing. So, and But I mean, first I'm going to integrate with easier, first thing is that this state needs to be in easy. So that's my contract of easy needs to have access to these sustain. 

Speaker 2  (01:11)
Of course, is a community smart contract. This is this is taken not be modified. Then, he's modified according to the rules. 

Speaker 2  (01:17)
So It's it's in here. Great, it's one of the first unit is chain. That's Here, okay, now the state the second is this a state. 

Speaker 2  (01:28)
I mean, this chain needs to support So in this state needs to accept calls from allerg from what are chains Maybe able to do calls to other change. So You are such Ching, you need to define hold this calls are handled and Under which circumstances? You make calls to other two other changes. 

Speaker 2  (02:05)
So the second thing we need to do is okay you have a chain, but if you want to connect to other change, you need to define in your state and decision function in your in your definition of your chain, how you handle the change or in what circumstances you are sending this change. If your chain is an EDM you can follow more or less the rules that we are Taking in no one with approxicant drags and so on or the ones that maybe applying lossi chain and prior for EV M is going to be kind of a standard like calling and to move, but you are not limited to I mean, naturally, you can have a change that's something that's not even an E. 

Speaker 2  (02:47)
B. M and just accept calls that comes from an Eb M chain or from another chain, and do calls to other to other places. 

Speaker 3  (02:58)
I guess I would not even BM change in access in the state will looks quite different to Eb M change. It was like for a proxy contract perspective changes to deploy. 

Speaker 2  (03:09)
So what it's commonates, the When will be the message passing? The message passing is a call on the record synchronosis. The what is shaco in the college destination address, and which is an federal madress, and old madda, and naturally a value which is there, but I would say in most of special change this value is one of these 0. 

Speaker 2  (03:32)
For example, loss chain, natiff token is not even so, the value is going to be 0 okay, but So it's kind of a message. Okay, so somebody says I call and you will get a ritory and the other way wrong. Okay, it was like so met, so Janes are able to Immediate calls on receive course. 

Speaker 1  (03:57)
So it works on any chain with the like sufficient improve infrastructure. 

Speaker 2  (04:04)
Yeah, okay. So this is so this is very much about what I was connecting to using the state needs to be there. You need to define these state demonstration function. 

Speaker 2  (04:15)
So this needs to be well-defined it. I mean, you'll change you need to 

Speaker 3  (04:19)
I'm thinking of visitors like standards like easy standards. 

Speaker 2  (04:22)
Yeah, but I mean, this the way you define your chain is up to you. The only thing that's definite easy is called these calls on. This returns should look like, but what you do in your chain, I mean, what, again, have an Liv, you can have a wazom, or you can have something that That's not execut nothing at all. 

Speaker 2  (04:45)
I mean, just doing some Special dot that's like some tradings or I mean whatever or some whatever application mean what you do in your state is your I mean this is your business. It's not a business. Okay. 

Speaker 2  (05:04)
So The third thing is, well you need. So, this estate music function when you are, I mean, when you are dissustained change, you need to verify this state in order for easy. I mean it's like easy, we'll not accept. 

Speaker 2  (05:23)
Stay changed, so you need to verify according to this state position feature that been closed may include this calls back-and-forth. Yeah, so while you need to then to Can I get them a very fire? Okay? 

Speaker 2  (05:42)
I mean, if this is a standard chain problem, it's going to be some very fires that you don't have to build a new borrifier, but if you are building something that's really innov, you need to some whole implemented, this is your full children. 

Speaker 3  (05:54)
Some of the new one would be non EV unchained that have different state communications a little bit different verificance. 

Speaker 2  (06:01)
Would say that most of the change they would take a while that standarder, and maybe some tweaks, so specifics to your chain. But. Again? 

Speaker 2  (06:11)
Hmm, you will have to You may have to adapt especially. I mean, depending on the chain if you have some other meant bridging things, I mean, or some special recomiles, I mean, you can have like different specific things. Specific to your chain now? 

Speaker 2  (06:38)
So then you to which roll up. So when you define a roll up, you'll define very much. Um, my goodness is, I mean this It's like these estate and decision function, which is very much, is which verifer and for each varifier, is which verification key. 

Speaker 2  (06:57)
Which version is like? It's a hush of the programme of these estate and situation function. 

Speaker 3  (07:03)
And partly independent to channel, right? 

Speaker 2  (07:07)
Yes, so there is another thing that needs to be the definite we need to check with. So what I'm saying here is not For definitely bad day there I use. Do you have a chain 80? 

Speaker 2  (07:21)
Yeah, they there that we have in mind, but I get, we need to finish talking about that. But they don't have your mind is that you are, we are creating a new chain and new chain that is will be assigned automatically to you. That's it. 

Speaker 2  (07:33)
Okay but for Seeing diabetes and these one reserve range that's not been used school in then it would be just incremental giving to you, but for chains that they are really half a chain idea they want to migrate, maybe Maybe here is one of the exceptions that maybe here we had some Seaco I mean temporary Mody, just for adding for adding this chat and then we will follow in the standard. Well, thanks there. Okay. 

Speaker 2  (08:10)
But again, it's just to avoid conflicts on the generian. This is something that at some point, we will remove Okay and then any case, you will not be able to repeat chain av, and you will not be able to take one of these reserved chinas that are going to be given. But again we need to see how this mega news for generity will happen, but this is just a small detail. 

Speaker 2  (08:38)
And other so it's roll up. So this Pole is this and call this hope this verifier, which actually in the verifiers. It's important to know that it can be different. 

Speaker 2  (08:52)
We are proving systems. We are approving system can be Jesus, for example, we have a proving system. But very sp, one, we have a optimal system, but maybe it's going to be some proving systems at all. 

Speaker 2  (09:03)
A Dev gaste brilliant system, or maybe I would the sick proving single system. What I mean brewing system is something that's just checking that the state decision function is correct. Okay, and actually you can define maybe no order for a citizenship of which you correct? 

Speaker 2  (09:20)
Maybe you can check, okay, but you need 2 out of 3. Improving systems to make sure she has to do to accept it. So you can be find these things, but easily as not married with any closing systems, so you can define as many clothing systems as you want, and bro ups can decide which brewing system you accept. 

Speaker 2  (09:45)
Okay, so we know here and so on a draw ups. I mean, one draw up it's going to be represented by you when the smart contracts. So when you want, you will perhaps a smart contract and that's mark, this is smart contract is what we'll have the rules for your world up. 

Speaker 3  (09:59)
Yeah, we've massive toxic. 

Speaker 2  (10:01)
Yeah, this is it's not approximately. This is the role up cultural, it's not. It's not for the representation of our contracts in one chain. 

Speaker 2  (10:10)
That's but in the easy context, these are wall up contract. And I mean, these are records, there can be something that it's I mean, you cannot change it. We need something, okay. 

Speaker 2  (10:21)
This is the state teacher function, and it's again of a naughty flow up and you cannot change it and sorry or it's something that's very making that, okay, it's like this is a private roll up. So you can change you can put whatever you want. You can set the state that you want you put the rules is the one thing you can be more flexible. 

Speaker 2  (10:38)
But again on the So we draw up, we'll have the wrong rules. These rules will be in this given by this. Oh, a smart country, but important to understand that. 

Speaker 2  (10:53)
It's roll up, it's associing as this as Marco drachis, it can be fully decentralised maybe there's Marco contract where we can do it, or it can be something that know that 

Speaker 3  (11:07)
So it was just like questions about 

Speaker 1  (11:10)
I was one following question. And then became The basic one like a formal data and change perspective, but what does easy require? Do this. 

Speaker 1  (11:22)
One of the core requirements 

Speaker 2  (11:24)
From yeah, I mean easier we have Home, you can define Bordering systems yep, in general, each pollaring system is going to have its own by the 8 minutes. They don't okay I mean, balled data is just give you a hush, which is a hush of a lot of a lot of statusions, and so and you'll say you'll get this is okay or not, but data can be made some singator can be some zk can be some proof. 

Speaker 1  (11:56)
It should be a parallel 

Speaker 2  (11:58)
Should be? What should 

Speaker 1  (11:59)
I don't put in paralympics, black construction like blood building. 

Speaker 2  (12:03)
Meanwhile, when you are blocked, when you are building means when you are building a block for a roll up mainly. But what you are saying is that you are doing your statusation function, so you need to burify this statusian function. Actually, this is a statusian function may include some goals to other change or receive some calls from other change. 

Speaker 2  (12:23)
And all this so and this is going to be borrified by your by the burrifier. Okay, and the verrifier will not only burify your chain, but will verify only the or the change that. That affects to you and this going to be done Ah, atomically, and if you have 4 change that they are talking one each other. 

Speaker 2  (12:45)
So You need to submit like the statums of futures of the fort chains and it's going to be And the basically we have a look like one proof or many proof one prooving system that altogether will prove according to all the rules that this is the transitional futures of these 4 chains are Yeah, yeah. But honestly. 

Speaker 1  (13:11)
Torby have a proo what about the sequence? 

Speaker 2  (13:15)
Um. You can sequent series. Well, who selects these transactions? 

Speaker 2  (13:24)
I hope that together, but at the end, if you want to haffer so sequencer, if you are, if you are, if you are, if you want to create a statistical function, that's only sections that affect your chain, and you change your Catholic sequence or you just put the sections. And then Yeah, I see it to the block. But if you want to connect different chains, I mean, the sequencing needs to be Sharon, what's coordinated kind of a sharot sequence, and then this coordinator maybe what it does is it's why it takes those to it, execute them altogether at the same time. 

Speaker 2  (14:01)
It generates a blue 

Speaker 1  (14:03)
Linear execution ordered across chains. 

Speaker 2  (14:07)
Well, it's linear across change. I mean, it's like take on the transactions. It takes you one after the other, but these times such as matuch other change. 

Speaker 2  (14:16)
So it takes all this and atomically excuse so that and generates a proof or or a proof yeah, for that, and all this needs to happen into our sequence in one block. 

Speaker 3  (14:30)
You were just talking about the real. So when we were at cans, if they came up, which was when the blocks of R$, that layer one 

Speaker 2  (14:37)
All of the 

Speaker 3  (14:38)
Connect to chains, need to agree to having their sequence updated according to the real goal layer one I think Frederic mentioned something around each new chance is to your first question. A mine like needs to agree to basically. Respond to that real bit there. 

Speaker 3  (14:59)
One I didn't actually have the solution in my head of what 

Speaker 2  (15:02)
Yeah, for the R$1, you can understand the R$1 like me if it was like another change, generate some calls to other chains, yeah, through the proxy contract and receive some calls. Okay, there is a trick that we are doing it easy for this to happen because we need to keep things actually in your one, but it's I mean, from the at least, from that, at least from the theoretical perspective. You can understand like another earlier. 

Speaker 2  (15:29)
Okay, so it's just okay and then all the, you know? These 2 questions, sections, including the ones that goes to layer 1 day are executed like attonically following approof of course, there is an implementation of the data. This is only layer one. 

Speaker 2  (15:45)
This is something that's very specific some things, but from the user perspective, it's you should not not each of these, but you're saying, is this you're right, I mean, if you are We got it, we call it composer these general, the composer we generate this this Bandal with all the transactions of the layer one and layer 2 that they touch one each other, okay that they won each other. We take all those and such as and generate the proof for that. I mean, this is, and this is composer, generally a Bandal. 

Speaker 2  (16:25)
And of course, is Bandal. But we sent probably to a block builder that would include broad 

Speaker 1  (16:32)
So nice sum arrived. We have. We will have a problem, infrastructure and the sequence of coordination. 

Speaker 1  (16:38)
Basically 

Speaker 2  (16:40)
Yeah, so once or I mean, there's all depends on chamber chain, but, yes, I mean, the change would. Be responsible for for creating mister sections of four coordinating with these composers to include all of sections in the right way if you are creating a change, for example, if you are creating a chain with a very specialistial function that nobody knows it's imposable that Yeah, I think I was actually nobody's got to call you on and not sexuals. I wasn't include it because it's not. 

Speaker 2  (17:16)
But if this is public and the people know, and there are, there is a demand for tone structures, mean, it's going to be some economic incentives for these composer to include austan sections. But do you need to make them public and 

Speaker 3  (17:31)
So let's say how it's going out. And looking at a new role up to connect and so easy, maybe they only have one arc provider on that chain, or they have any Oracle was questionable. Let's say like from an assess, but like, maybe it's a risk assessment or like you. 

Speaker 3  (17:48)
Appropriate assessment like, is there anything that 

Speaker 2  (17:52)
I mean, that hears this is a solution from the security perspective. It's a is solution between chains 

Speaker 3  (17:59)
Because I haven't gold. It's no 

Speaker 2  (18:02)
Exactly. I mean, of course, hacker roll up from me, whatever, saying that roll up, it's hacking roll up. I mean, if roll up has a statement function that's more issues I mean and you are it that roll up. 

Speaker 2  (18:13)
I mean that's your problem. That's I think, but they there is that one. Money's roll up cannot affect other wallups, especially if they don't touch this. 

Speaker 1  (18:25)
So it allows us to composibility like him. Can I compose in the easy yeah, illegal yeah? 

Speaker 2  (18:33)
Yeah, yeah, this is the complexity is that we're working by days that you can't call these. And well you can call up a can Carl roll up B but in the middle of a call can call rulap C and then you return from B and then in the B you return to a and maybe in a you reverse and then you need to cancel everything I mean this will this And no I mean, this is our just calls, so it's going to be some limits, but limits the same way that you have you normal theraput transaction. You have limits of many times you can nest. 

Speaker 2  (19:11)
Yeah, goals well 

Speaker 1  (19:13)
I think nasta was British amazing. 

Speaker 2  (19:16)
Yeah, so here maybe I maybe I need to follow with the question, but do you want nesting? Okay other thing is that your roll up can include another easy insight in having thing that way, I don't know if this makes sense, but at the end this is more contract. So you can deploy this is my contract. 

Speaker 2  (19:35)
Whatever you want, other thing is, hold a bundles we call the builders, will borick, and to think a little bit, but it be, and it's just a smart contract. 

Speaker 1  (19:46)
And this, it is effectively 

Speaker 3  (19:48)
So you can deploy it another easy instance on the easy instance. 

Speaker 2  (19:52)
Not? I mean, probably yes, but I mean, we are not thinking we are not thinking, is it and that with that in mind? 

Speaker 3  (20:02)
There's 6 minutes. 

Speaker 1  (20:03)
It costs in the like obviously like a mascope in the one single block which is increase the problem time. Or bringing time will be same? Let me do a nice oh, necessarily. 

Speaker 2  (20:20)
Improving time, I mean the thing is at the proving time. The proviece thank you. Squad is very much, for example, so it does not the pain. 

Speaker 2  (20:27)
So it depends more on the resources you have. So, and this will limit the number of transactions that you can include again, especially for meaning, of course, in layer one, you still have the gas limits. Okay, so you cannot By pasta gas leaves another one, but you can do whatever you want in now, to. 

Speaker 2  (20:47)
So now those you could have a tools at half infinite gas limit. And The only limitation is the proving capacity of the capacity is very much limited to the hardware with the quantity of GPU's. You have a premises. 

Speaker 2  (21:05)
Of course, there are some I mean there, so proving time, it's very much product. It's not like 100%. Percent, there is always more part. 

Speaker 3  (21:17)
Wasn't 100%? 

Speaker 2  (21:18)
But a lite double Para, so if you have the thing is that the day is that generating the proof yeah, it should take the same time to improve what transction that to prove what millions on sections, maybe for approving, what's section you need one GPU for proving 1 million sections you may need? I don't know what GPU's okay, but theoretically the time should be the same. Okay, this is would be if it's 100I mean, it's almost, but there is some it's like a look at so it's like If you are putting a lot of sections, maybe it's slidely I mean, that's a point these growing a bit. 

Speaker 3  (22:01)
Psychology's chamber will have to set up GPU power or GPU and this is 

Speaker 2  (22:07)
Well again these, yeah, you are the you see from the cause we'll see what A transaction to be included is to be proven in order to be prevent, we need to generate this is proof so the provin cost pertual section. This will be probably the limiting limiting factor here, but we expect that if the restore sections that are willing to pay this proving cost. 

Speaker 3  (22:38)
Connected to if existing theormanators at all is that the stams join the dots therm I had 

Speaker 2  (22:44)
This is not brilliant valley doors. There are just deciding which walk the GPU power is this is for more from them is the composer that maybe linked to the block builder. But it's like mid. 

Speaker 2  (22:59)
This is the limits. Well, of course, the limits is going to be, of course, the market. Okay, so the demand and The proving capacity, which is 

Speaker 3  (23:14)
Yeah 

Speaker 1  (23:16)
What is the actual gascross right down in the call easy? What is the difference between like A 

Speaker 2  (23:26)
The sortique I mean, I mean, hearing here. He's robbery running. There's kind of a fixed cost. 

Speaker 2  (23:35)
You measure that you have a one single easy chain, and has one such chopper rock this section probes? We're going to be very expensive, because we need to generate section with a proof in everybody blog and just forward to sexual. This is like problem's gonna be a lot. 

Speaker 2  (23:52)
But At certain points, fixed cost is going to fix cost is going to be Negligible, looking out with a very budget's going to be the very well causes the very well cost at the end at the end is the energy she fortunating the proof which is Okay, but it is not 0 yeah. There are other limitations but the availability limitations that the effect here. Yeah, but I don't know. 

Speaker 2  (24:23)
I also, did they are brewing? Talking about improving? But we don't probably cost, this is quite paraly Sabo, but you need to have process intersections processing these transctions. 

Speaker 2  (24:41)
Sometimes it's not they need as well, and even processing that this may become a this may become a limitation here, okay, sort of like, but here we're limiting that are much, much higher, what we probably have. 

Speaker 3  (24:56)
I guess some of the weather latter part of what you just mentioned. 

Speaker 2  (25:01)
Oh, yeah, I mean we had not defined in easy. Medical specific Nepal, which is chain, it? Chain should have the meppool, but of course, for these shareder sections, it should be some sort of mentals, but I think that could only, of course, maybe if everybody that is sabul, but it's kind of ample that the water used a bit water uses. 

Speaker 2  (25:28)
It's it's a lot of people. Maybe a lot of touch is just to go to the polay they just go directly to about that, so I expect that this very, very much will suffer with With it with this, maybe that's a boy we decide to put them a pool for this shared on sections. It's a possibility. 

Speaker 2  (25:46)
It's not Decided that there is whole you could communicate then this whole communicate with these composers. We can be through pair to be a mempool but can be directly to the list of composer, so not bad list of composers. 

Speaker 3  (26:05)
And that would be something that they could have roll up connecting would choose on set up boy, and I guess that have it preestablished locally. 

Speaker 2  (26:12)
You haven't decided yet. But in many cases, it's I would say that We've been define something, but it's something that Perhaps will coordinate. We were the going to impose, we have. 

Speaker 2  (26:29)
We got to impose where the users should set that was actually the user should set the sections to that. Opposer, to whoever supposed to build this profession will establish the meganism. It could be even more than one So are not at this point, we are not thinking in imposing anything, but of course, I don't want to close the doors. 

Speaker 3  (26:54)
I guess there's something to communicate right. Is there something you should be aware of what designed choice for you? 

Speaker 1  (27:02)
Okay, we have A Contract, so once I'm I deployed, the easy contract. Can I upgrade it? 

Speaker 2  (27:11)
They, there is I don't know what's going to come in the first version, but there is that this will not be upgradable. Maybe I used to there is going to be a new version of easy. But I am not saying that. 

Speaker 2  (27:25)
Maybe the first word should we make it cabulator because it's intestant. Whatever? But the I east Easy should look. 

Speaker 2  (27:38)
There is that easy years fix as well should not be Uh, update it will come to move that 

Speaker 1  (27:48)
Like, for example, if the nested calls fail, what's what will happen? Probably The old coat executed 

Speaker 2  (27:57)
Yeah, cases scenario. Is that nothing excuse. In the full balloon is, but the easily warrant is that everything is consistent. 

Speaker 1  (28:09)
Um. Is there any upgrade path in the future? 

Speaker 2  (28:14)
For easy, no no idea is that so idea this to be I mean, there's more good like fix with another say, is that we create like easy to then roll ups migrate there. But It should. It should be like another thing. 

Speaker 2  (28:33)
It's not 

Speaker 3  (28:34)
Switching a contract as a mutable deployment, you deploy 

Speaker 2  (28:38)
Reminder, nazi, good future. Well, I mean, is that I'm not, of course, this may happen, they gay, is that They dare that at least that we have an accordingly is not to you have something relatively simple. And we outdate that we check it very well, and we let it Uh, I see. 

Speaker 2  (29:04)
That's why that's the idea that we have cooling abatory deploy, does mean we are creating this is my contraction. So we'll see the reason probably. I mean, we want to run some I want to run Maybe before a while get enough, we worked on the running debay it with the race to do all these tests it made it, we will not declare the beginning like a final production ready, but we want to get as much experience. 

Speaker 2  (29:41)
So that we can the spot ready, and this bobbies, what it should be fully not. I'm glad about fix this is what it is. 

Speaker 3  (29:50)
The right right away. 

Speaker 2  (29:52)
Yeah, that's what we're with the intentions. But again to say 

Speaker 3  (29:56)
I guess the zeka improving will continue to be you know? And 

Speaker 2  (30:01)
Yeah, but for example, that cica I mean the ciga proving this is going to be proving system proving system, we'll have their own smark on track and again. They mean, here I'm talking more from disease perspective that is therefore, for this is to be a little bit like easy, it is like they is to have a small smark contract. That's not upgradable and if there is a new version of Z maybe we create another broom system, and then just wall ups, they just decide to switch to 

Speaker 3  (30:34)
My sense. 

Speaker 1  (30:35)
I have one more 11 pop generate question like, what is the one thing you want? Developers understand a lot easy before they start doing. It's an impremitive. 

Speaker 1  (30:46)
It's a new design. What developers should know? 

Speaker 2  (30:51)
Thank you. Thank you with robers should see as Let's get off an extension of Italian or a way of extending. Set you so it's important to understand this. 

Speaker 2  (31:07)
What's what this signals could possibility means is that? Well, you say, maybe if you understand that for you can call Uh, one, the smart contract from another smart contract. He decided for you was here now you can do exactly the same, but crosschain, so he's like from one brought up or from maybe you're going to call to another chain, and And triatory, you compose with from one the other, so that's sorry to be quite quickly through economic zone, because it's it's like set of roll ups. 

Speaker 2  (31:45)
Oh, it's a state. So members that they got So it's sacked a topically heavy synchronously between them, like if they were inside. This is, I think this is the most important things for understanding. 

Speaker 2  (32:05)
While this. This will be so we are talking from users of absolute systems. They need to see that. 

Speaker 2  (32:13)
Okay, they are in a little Middle buffic, aller that okay nowithera be speaker, so they spent the things that they could go, though it's not only if you don't go truck, they can go on now all ups contracts, and if you are a developer that's planning to build your own chain. And while you need to understand that while this is kind of an extension of theory that you need to accept this and accept and send this calls that you're going, it's a very simple interaction. 

Speaker 3  (32:48)
I don't know on an easy instance. So you've got AI p's it on a fair number one. I'm assuming you'll inherit the food starting to be implemented. 

Speaker 3  (32:59)
What would be governable if you wanted it to be on a easy instance. Like, could you let's say you wanted to have quick? A block time or I do whatever you wanted to do for your certain incidents like 

Speaker 2  (33:12)
Your you're free to do whatever you want when you are so of course you write a smartphone truck, it's not going to you put the rules, but you're free to put on the rules you want your chain. 

Speaker 3  (33:24)
Chronics. 

Speaker 2  (33:26)
The things are limited that you have is that medithrium block time is what it is. We cannot change that so right now is 12 seconds. So that means that I mean you can define block time you want. 

Speaker 3  (33:42)
Michael, maybe you'll be customising the proofer okay? 

Speaker 2  (33:48)
Well, of course, the problem you need to the prover you need to tell what are the rules? Okay, so you need to the probably to specify what what your state needs and if you put 3 blocks per a 3 block because you want a probing time or 4 seconds, you need to explain you need to tell the state position that you have these superworks or these optimisations there. But from the easy, it's just a state that is change based case widespread what spread 3 block. 

Speaker 2  (34:26)
But these estate, maybe this is theory of state. Maybe these Evolves 3. So one transition in L1, this may involve 3 changes 3 blocks in, but this is this is how you defined. 

Speaker 2  (34:45)
This is a specific to your chain. Is that we can not update these estate once everythings because there is only who our block every 2 or seconds. So your chain, you may maintain some temporary thing, but this is going to be centralised me. 

Speaker 2  (35:07)
We cannot give you the warranties so tomorrow, not warranty because is more contract, so easy will not warranty, whatever it's not commit to everyone. But what's not coming to our one. You're change you. 

Speaker 2  (35:22)
Do what you 

Speaker 3  (35:24)
So it just so not everything from your sequence to will have to actually go to well on that right. 

Speaker 2  (35:29)
Depending of independent here, and depending the here, there is different options. Then, you can have a chain where the sequencer is kind of a centralied sequencer and sequence, quite important piece, and then the users of the chain trust a lot sequence. And it's The secretary just the dating the state once in a while. 

Speaker 2  (35:54)
Do what's the world can be able to second tour you what I want? Okay? Ab users of the chain they need to yeah accept that, but if they want to interact with other chains or the interactions happen at Uh, the thraugh block, so Do not expect. 

Speaker 2  (36:17)
So if you're in one chain, and you are calling another chain, and this goal Of this case will happen in the next a few o'clock. If there is 2 sequences, they agree on whatever okay, but once you want in deep for busy is what what's committed, and what's competed is either you know? 

Speaker 1  (36:38)
Oh, we have hand on the road stuck. What if the ones take change the invalidate execution all the 

Speaker 2  (36:47)
Tax birds mean the truth is what the theme says, so it's a reorder is a reyork of everything. The transaction happens if there is York, and of course changed the to take in account that real xinder, one Ah, they happen. So of course here. 

Speaker 2  (37:10)
Jane scale Handal basic That would take 3 ways. So what these? Okay except that he did us would be good may have they have reorics too? 

Speaker 2  (37:23)
Another is Well, you need to lock the chain, so it's like you're waiting for the one, so we did what you want to be final. So 

Speaker 3  (37:35)
He was just second. 

Speaker 2  (37:36)
You know, white for some time. And just to of course, more your white, more want this you have that so want to be a reorcan you are free to decide if you go forward at the risk of having some York, I mean, if you want to have 0 reason you need to wait for finality in a one which is a pain. And There is Things that are in the middle. 

Speaker 2  (38:03)
That's like the sense of walking the full chain Your baylock, parts of your chain, or parts of Bay, yeah, something more specific in there. Or Here you haven't a study yet. But. 

Speaker 2  (38:20)
Maybe? For changes that need to where it is and finality, maybe there is maybe there are some architectures we need to check there super architectures of kind of a double change, a change that it's like secret news and that's connect, and then no lot of that's more the one that runs. Then, because you control because sequencer controls both changes that you can set up some rules for the 6, but again we desar architectures that all right there. 

Speaker 2  (38:50)
So the architectures that we need to see. 

Speaker 1  (38:52)
And the components that what we can use a sprint confirmation 

Speaker 2  (38:57)
The appliconfirmation, served by the Domino too. It's like pre confirmation is just that somebody is competing that something will happen. Well, this is where up to you what you trust. 

Speaker 2  (39:10)
But at the end, the final, the final word easy, I mean, this is my contract for the final word is what the theory says. If there is record informations in the wall ups while you're free to do to accept the confirmations. If there is pro confirmation with your free to take this pro confirmations for your free to trust these progressions with economics that are in there. 

Speaker 2  (39:37)
But What goes, what goes from is I mean, this is a small contract, and what goes is? What's in the What did that smoking? 

Speaker 3  (39:50)
Was like absolutely quitting again? 

Speaker 1  (39:52)
I'm no question, please ask 

Speaker 3  (39:54)
You mentioned that the live one block time is 12 seconds, and when something is called from one of the easiestnesses, it'll need to be settled in the blocktime 12 seconds. So I'm thinking promote, let's say notic chain instance, the infrastructure, yeah, I mean, let's say you're calling an Oracle. I mean, there's other examples of an Oracle but will there be latency in that call, if you're retrieving, that's say the price of a theory. 

Speaker 3  (40:18)
From there one, so your instance 

Speaker 2  (40:20)
The share probe, we need to talk more with marketing able to work. And we've got us 

Speaker 3  (40:25)
If you're not at that internal 

Speaker 2  (40:26)
Yeah, that's go, but it's very specific for losses. So it's like a but I hear they want to see it's like, I mean, if you divide the theorblock of the secret 4 so blocks of 4 seconds, so that we maybe 4 for So everybody so everybody, 3 blocks is going to be 2, that's going to be only those chain transactions. Yeah, and another that's going to be the shared one. 

Speaker 2  (40:54)
So if you are doing a section that's touching values. Chains, you will have to write to this. You've sector's fourth was actually to be included. 

Speaker 2  (41:05)
But again, this is something that 

Speaker 3  (41:08)
They have a call on Monday with one. 

Speaker 2  (41:09)
How did you ask? You'll better ask you better ask Martin because of this is what I know that's thinking about. But again, this is he's considering different options, so something closed yeah. 

Speaker 3  (41:23)
Minister would right if your original price was lacked by 12 second. And that's not great. 

Speaker 2  (41:31)
5 seconds oh well, yeah, loses, but there's already happening right now in this advitage kitches. 

Speaker 3  (41:39)
Yeah, true. Yeah, I got a yeah. Promise you just think about 

Speaker 2  (41:44)
Are you? 

Speaker 3  (41:45)
A limit? Um. No questions on my side, I guess like we're going to be dog feeding this in the node for the lack of a better word from a nosis's point of view. 

Speaker 3  (41:57)
So 

Speaker 2  (41:58)
That will be losses a specific specific. I think 

Speaker 3  (42:02)
We kind of figuring out how we do this first then, online we'll take 

Speaker 2  (42:05)
Yeah, I see the first version. I mean, but again I mean all this, what I'm telling is everything that I'm saying you about losses. I have 0 Uh. 

Speaker 2  (42:18)
So I've got decided yeah, yeah, actually, yeah, when those season, but they tell you what I mean, what then saying you is what I hear? But they hear Martin, he stops, but it's not even a finalisation that things will go able to in that direction. 

Speaker 3  (42:34)
I believe they were working on it. The week just gone, and then we got a call next week where they were going to share their professions. 

Speaker 2  (42:40)
But it is a little bit the first version, for example, they want they want to do something meaning more, maybe just that they don't want that. For they don't want to accept their sections, they want to accept all the cause from they want to a tool. So they want to. 

Speaker 2  (42:56)
Have some feedback, some feedb. Very, very minimal. But it's something that unlocks maybe like 80%, with 90% of the value of liquidity. 

Speaker 2  (43:08)
People swaps. Okay, so The blandie, I'm going to be daddy's like, okay, what can unlucky 10% of the functionality? Yeah, for what's the minimal unlocking functionality that we need so that we had 1880% of the moule we went from there. 

Speaker 2  (43:26)
We will go. That's a little bit by the reason be fine, but again, yes, that's something. 

Speaker 3  (43:34)
Yeah, we'll pick up with the next week. Um, I think that quarter as well yeah. 

Speaker 2  (43:40)
Yeah, bought some exciting. But I think you have like, you know thank you yeah, we've got that. 

Speaker 3  (43:47)
To do this together, but yeah. 
