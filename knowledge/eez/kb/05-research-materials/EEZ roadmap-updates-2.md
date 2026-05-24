May 12, 2026

## EEZ roadmap & updates \- Transcript

### 00:00:00

   
**Friederike Ernst:** Um okay. Um so uh Philip and um Phillip and the rest of the  
**Philippe Schommers:** It's  
**Friederike Ernst:** EZ team and I had um a workshop last week to kind of talk about um uh talk about uh the  
**Philippe Schommers:** just  
**Friederike Ernst:** the precise architecture and the road map going forward forward both for kind of easy as as a general construct and also um for nosis chain um maybe Philip I'll give a high level overview and you kind of chime in whenever I say something ludicrous or kind of kind of um accelerating timelines too much. Um um so uh kind of the idea is to kind of have the EEZ smart core smart contracts um done by end of this month. So in two weeks time um and they will then um go out for external review um uh kind of we'll share them with um interested parties that will be um for four weeks or so and then kind of we have uh budgeted another four weeks um for an external audit. Um so um if that all goes according to plan then would be June and July spoken for.  
   
 

### 00:01:20

   
**Friederike Ernst:** Um and then kind of we would um deploy um the contracts um kind of to an easy test net um to Ethereum mainet um in August. So that's kind of the um that's the current timeline for the easy cool contracts. There's a lot of other things that actually need to happen um in order for the Ethereum economic zone to actually really work. Um so so one um is um the prover setup. We have decided to actually go for a modular prover setup. Um so there will be a a number of different available provers. So kind of zk provers. So that would be sisk and um uh the succinct sp uh prover. Um for now people can add their own um ck provers. it's not it's not gated. Um we will also um uh allow people to we will also add um TE servers so trusted execution environment. So basically that's um wouldn't say this publicly but that's glorified multiigs. Um so that's um those those are kind of the um the uh provers that will be available and people will have to use at least two solvers uh two provers.  
   
 

### 00:02:48

   
**Friederike Ernst:** So kind of you will have two proving systems and kind of you you as a as a chain in the Ethereum economic zone can specify which proving systems um you use. Um okay. Um that's that's the that's for um yeah and then basically we'll we'll be building rollup one um kind of uh obviously there will be a very first test net um uh after uh the um after uh the the core contracts are live. But kind of this test net will be gradually um upgraded as more things um come live. Um so kind of it's it's dynamic test net. Um for Nosis chain the idea is to kind of implement something for Nosis chain as soon as possible that already gives tangible value to Nosis chain users and DAPs even if it's not um the even if it's not the final form um of Nosis in the Ee um so the simplest version um that um we can put drive that will already um confer very significant advantages to Nosis chain users um is allowing a single call from Ethereum mainet onto Nosis chain with a return value.  
   
 

### 00:04:25

   
**Friederike Ernst:** What that enables is that basically liquidity between mainet and Nosis chain is is shared. Um it's important to understand that kind of this this only goes one way. Okay. So kind of the call can only be initiated from mainet. it goes into Nosis chain and then it it it it goes back to mainet. But because kind of like um you you can uh kind of I mean uh you can use this for any kind of uh token um order kind of it it rebalances both ways, right? So kind of like if you if you if you say you want to sell ETH on Nosis chain against USDC, you can also sell USDC against ETH on mainet, right? So kind of like it's it's the same mechanic here. Um so that would that would already confer probably around 80% of the benefits of what the EZ could enable today to users when we have this. Um it's it's it's important to understand that kind of down the line there will be many more benefits. Um but um even if we kind of had the full version today um the they wouldn't be available to users yet because DAPs don't take them into consideration yet.  
   
 

### 00:05:34

   
**Friederike Ernst:** So kind of despite the fact that they are theoretically there practically not there. So kind of a lot of the benefits will already be unlocked um from this um shared liquidity. um this um this kind of no nested calls kind of just one call only can initiate from L1 two noses and then back um this is something that with a limited proofer setup so kind of nosk provers yet um uh we think we can do by December 1st um so this is it's the plan to kind of upgrade nos's chain by end of year to a first version um that already synchronously composes with Ethereum in a limited sense kind of obviously um kind of uh nested calls um and move proving systems and so on kind of this will be very much a first instance um so kind of um everything else can be added later so kind of like it's not that kind of where we're saying okay we're simplifying it and then we're deploying it. This is kind of this is um this is a stepping stone um on the road to kind of full synchronous composibility.  
   
 

### 00:06:59

   
**Friederike Ernst:** But kind of if we say kind of we first build out the entire thing and then we launch it all at once, it'll take us um probably until yeah probably yeah okay I I don't even want to look at Philipp it it'll take us probably two years or something right. So kind of um uh yeah, so kind of having something that already kind of is very useful for a lot of people early on is important for us to  
**Ben Carvill:** Oops.  
**Friederike Ernst:** kind of um yeah to uh to not only have these effects unlock these effects much much later. Um that's um that's uh that's the um higher level takeaway. Phillip um is is this a fair  
**Philippe Schommers:** Yeah, I think so.  
**Friederike Ernst:** representation?  
**Philippe Schommers:** And I also think that's fairly realistic. I I mean, I just have one thing I really don't like with the intermediary blocks, but that's a conversation for another time, I guess.  
**Friederike Ernst:** Yeah.  
**Sebastian Bürgel:** Okay. But that means in So no,  
**Ben Carvill:** is that the 12 to 4 second sorry was that the 12 to 4  
   
 

### 00:08:11

   
**Sebastian Bürgel:** Ben, go ahead.  
**Ben Carvill:** seconds differences in time  
**Friederike Ernst:** Yeah. So, kind of we're not actually going to go so kind of this is Yeah, I think this is the part that I skipped over. So, kind of we're we're we're um currently not in favor of taking the validator set with us. Um so um kind of just because um having shorter block times  
**Ben Carvill:** Yeah.  
**Friederike Ernst:** then kind of requires having a committee that kind of decides on um finality. So kind of basically you kind of you you um uh you deteriorate the security of your chain um and you can only you can still only go to probably um 4 second blocks or so. So kind of the idea we run a sequencer inhouse. So a centralized sequencer that kind of makes blocks every second. Um possibly with a little hiccup around kind of like the composing block. So kind of like possibly kind of like a little bit longer um latency around that block, but kind of in principle um one block every second kind of built by a centralized sequencer.  
   
 

### 00:09:24

   
**Ben Carvill:** One of my questions was I spoke with Jordi in Prague and we kind of ran through a lot of this stuff as well like when you're calling if you could call infrastructure from layer 1 and the block times of the instance then Oracle uh were to was 4 seconds on your instance and then you only had an update every 12 seconds from ETH mainet I mean I've not like specifically dived into this but one of my questions I had was like does that introduce complexities for for DeFi like is that um yeah if you're missing updates of  
**Friederike Ernst:** those are complexities. Yeah, I mean those are complexities that are already there, right? So kind of like every time you kind of have chains with different block times kind of there is there is kind of a time where kind of like one chain already has a new state and the other one kind of still has kind of like the old stale state. Um, so that that's that's that's that already is the case and kind of if you look at it kind of uh if you if you look at it kind of like kind of with just the canonical bridge um it's it's much worse today, right?  
   
 

### 00:10:33

   
**Friederike Ernst:** So kind of you you can bridge but it'll take you 20 minutes. Um so kind of every time you kind of you you kind of get the tokens kind of you the the the state on which you kind of acted um is 20 minutes  
**Ben Carvill:** Got  
**Friederike Ernst:** old  
**Sebastian Bürgel:** So just to to paint the picture for August,  
**Ben Carvill:** it.  
**Sebastian Bürgel:** I believe you said we have a we have a Nosis L2 which is single sequencer  
**Friederike Ernst:** August in August there's not going to be a Nosis L2 So kind of Nosis L2 is going to  
**Sebastian Bürgel:** run.  
**Friederike Ernst:** be end of year. So this in August kind of there's going to be um kind of the first instance  
**Sebastian Bürgel:** Okay.  
**Friederike Ernst:** of um rollup one or kind of we were we actually kind of we called it kind of like rollup zero to one. So kind of it it kind of start at roll up 0.3 or something and then kind of like because kind of like there's many features not yet present and kind of the first kind of vanilla in insensiation where kind of like everything  
   
 

### 00:11:28

   
**Sebastian Bürgel:** Yeah.  
**Friederike Ernst:** works as advertised earlier kind of that will be roll up one so kind of in in August September there will be roll up point two or  
**Sebastian Bürgel:** Okay.  
**Friederike Ernst:** something.  
**Sebastian Bürgel:** So for August we have a version which has no more Ethereum blocks but which is being used by Titan Beaver and the the Flashbots thing builderet. Um which you want them active on that one to to include that stuff there.  
**Philippe Schommers:** No, I mean we will just use the B math bundles send them to to builders meth bundles.  
**Sebastian Bürgel:** Sorry that the what?  
**Philippe Schommers:** So you basically just create a bundle that contains transactions that are one transactions or transactions whatever we send them to a builder like Titan Beaver whatever and they basically have to um well put the entire bundle in that order um in a in one block and they also don't do it like they revert it if it's if any of the transactions revert um and this way we can actually ourselves compose the blocks.  
**Sebastian Bürgel:** got it.  
**Philippe Schommers:** So basically do the singles calls and post to L1 and whatnot.  
   
 

### 00:12:37

   
**Sebastian Bürgel:** Okay.  
**Philippe Schommers:** They don't have to be involved and they are just I mean there to do to post the bundles but they don't have to actively compose or do  
**Sebastian Bürgel:** And and why do we send that into a MEF pipeline and not just into a normal ME  
**Philippe Schommers:** anything because it's pretty  
**Sebastian Bürgel:** rule?  
**Philippe Schommers:** much uh impossible because we need like let's say if there's L1 to L2 transaction we need to put something before the L1 transaction and if we can't uh then  
**Sebastian Bürgel:** Okay. Okay. Sure. You want the execution order. Okay. So, you submit a bundle because you want I mean I guess we could bundle  
**Philippe Schommers:** Yeah.  
**Sebastian Bürgel:** it ourselves.  
**Philippe Schommers:** Yeah. We So our composer will do all the calculations. will find all the transactions that need to be put the order they need to be put it put in. Create a bundle, send the bundle to all the builders and  
**Sebastian Bürgel:** Okay. Okay. Okay. Good. Okay. So,  
   
 

### 00:13:33

   
**Philippe Schommers:** yeah  
**Sebastian Bürgel:** we have that one. That's roll up one. And um since it's going through the normal math pipeline, we should be reasonably sure that this gets executed in time. So yeah, we have kind of composibility. I'm just trying to think now how does this feel?  
**Friederike Ernst:** Dr. Bas there's also kind of like this is kind of I mean we we will do this and we will run it but kind of like  
**Sebastian Bürgel:** Yeah.  
**Friederike Ernst:** this will not be a production environment. Okay. So kind of like this will be kind of labeled as a test net. to kind of to make sure kind  
**Sebastian Bürgel:** Well, but it has real money. Like it has real money.  
**Friederike Ernst:** of you can use it  
**Sebastian Bürgel:** It has real ETH. I can use it for whatever I want to,  
**Philippe Schommers:** Yes, we'll try  
**Sebastian Bürgel:** right?  
**Friederike Ernst:** for whatever you want but kind of yeah it I would strongly advise against using it for whatever you want kind of like  
   
 

### 00:14:19

   
**Philippe Schommers:** to  
**Friederike Ernst:** from day one and kind of it gives us a way of kind of testing out all these composer issues to kind of make sure we kind of can actually prove the state kind of like I mean all of these things kind of like we need to make sure they they  
**Sebastian Bürgel:** Yeah.  
**Friederike Ernst:** work um and uh yeah so kind of  
**Sebastian Bürgel:** Okay. Okay.  
**Philippe Schommers:** And we also try to limit the amounts of money that can kind of move through the easy on roll of zero  
**Sebastian Bürgel:** Okay.  
**Friederike Ernst:** Yeah.  
**Sebastian Bürgel:** Unless you're chat.  
**Philippe Schommers:** just so that people he can  
**Sebastian Bürgel:** Like chat. Can all in. Okay. Good. Now it's  
**Friederike Ernst:** Well, I would really hate it if he kind of if he managed to screw us by kind of losing a lot of money on me easy kind of  
**Sebastian Bürgel:** 31st.  
**Philippe Schommers:** on.  
**Friederike Ernst:** I kind of like maybe let's let's let's exclude him all together.  
**Sebastian Bürgel:** I I feel so bad for you. You lost money so you can't troll other dolls.  
   
 

### 00:15:10

   
**Sebastian Bürgel:** No, but now it's 31st of December. We're celebrating New Year's with the actual Nosis L2. So let's think how that feels like practically. Now on the 31st of December 2026, we have a uh a centralized sequencer that runs the Nosis L2 which is not a replacement for Nosis chain but is separate or how do you see that?  
**Friederike Ernst:** It's a replacement for nosis chain.  
**Ben Carvill:** from launch from that date.  
**Philippe Schommers:** I mean hopefully not from December 31st, but  
**Ben Carvill:** Yeah.  
**Friederike Ernst:** Yes.  
**Armagan Ercan:** Please do it at the December 8 because when we launched the Nosis chain beacon nos beacon  
**Philippe Schommers:** yes.  
**Sebastian Bürgel:** No, now for for this discussion it's December 31st. I just want one date. So December 31st Nosis chain my validator that I'm running here right now is very sad  
**Armagan Ercan:** chain.  
**Sebastian Bürgel:** because it won't get to propose the canonical version of Nosis chain anymore. it won't get any more GNO rewards which is fair enough but we basically have a kind of genesis from this thing where we import all the genesis state  
   
 

### 00:16:26

   
**Friederike Ernst:** I mean kind of I'm not entirely sure kind of you actually need to import the genesis state.  
**Sebastian Bürgel:** um  
**Friederike Ernst:** So kind of like I mean so basically the kind of just the the way that blocks are built is switched out from underneath. Uh but kind of you don't need to import kind of I mean Philip correct me if I'm wrong but I don't think you actually need to import state  
**Philippe Schommers:** Yeah, I mean we'll update the clients so that they kind of support the  
**Friederike Ernst:** anywhere.  
**Philippe Schommers:** transition. Uh but you'll be able to run some clients. Maybe not all of them. So you might, for example, if you're running, I don't know, go get gas, you might have to move to rest or whatever. Uh but they will be just one  
**Friederike Ernst:** So kind of this the the the reason for this being that um uh um  
**Philippe Schommers:** package.  
**Friederike Ernst:** that different um clients are um more or less easily verifiable kind of by the ZK uh program.  
**Sebastian Bürgel:** Yeah.  
**Friederike Ernst:** So kind of it and it depends on the stack on which it's built.  
   
 

### 00:17:26

   
**Friederike Ernst:** Um so kind of uh ideally um you want uh C sharp um or um Rust um you kind of C++ you can make work um if you're ambitious enough and then kind of there are a lot of clients so kind of Golang probably also make work if if you're ambitious enough but then kind of Jav JavaScript and so on probably can forget  
**Sebastian Bürgel:** right? Right. Right. Okay. So, let's say on my DNA I still have my ref running, right? My validator is kind of sad because it gets shut down, but my ref keeps running. My Nosis Nymph.  
**Friederike Ernst:** If you update it, right?  
**Sebastian Bürgel:** Sure.  
**Friederike Ernst:** Kind of the the rest kind of Yeah.  
**Sebastian Bürgel:** Sure.  
**Friederike Ernst:** If you update your  
**Sebastian Bürgel:** Auto update. Off we go. Um,  
**Friederike Ernst:** Yeah.  
**Sebastian Bürgel:** now my Nimbus is also unhappy because it gets shut down, I suppose. Where does my So, there's a new sidecar which is then running which my ref gets its consensus info  
**Philippe Schommers:** That is I mean kind of a technical detail.  
   
 

### 00:18:26

   
**Sebastian Bürgel:** from.  
**Philippe Schommers:** Let's say it's not clear if it's going to be a separate consensus client or if it's going to be inside the rest library bin library.  
**Sebastian Bürgel:** Interesting. Okay.  
**Philippe Schommers:** Um yeah,  
**Sebastian Bürgel:** Yeah,  
**Ben Carvill:** What is a Nimbus?  
**Sebastian Bürgel:** Nimbus  
**Philippe Schommers:** nemesis  
**Ben Carvill:** What is Nimbus?  
**Sebastian Bürgel:** is  
**Ben Carvill:** Okay.  
**Armagan Ercan:** execution and the cost.  
**Ben Carvill:** Is it  
**Philippe Schommers:** client  
**Sebastian Bürgel:** okay. So,  
**Ben Carvill:** okay?  
**Sebastian Bürgel:** let's say it's integrated into ref because Debjit can make it happen. He's good. Um, now I have my ref running on my Dub node. uh which is basically still I get and and you said we want at that moment 6 seconds blocks or 12 seconds blocks  
**Philippe Schommers:** Well,  
**Sebastian Bürgel:** inherited.  
**Philippe Schommers:** Martin wants two second blocks, but that's the point I'm not very convinced  
**Sebastian Bürgel:** Okay, let's let's let's skip that discussion.  
**Philippe Schommers:** about.  
**Sebastian Bürgel:** Every every whatever number of seconds I get to see a new block. Now the trust assumptions of that block are basically I trust the single sequencer.  
   
 

### 00:19:34

   
**Philippe Schommers:** No. So you trust a sequencer for pure L2 blocks but once you actually go to the L1 and we post all the data to the L1 then you trust a committee which is  
**Sebastian Bürgel:** Yes.  
**Philippe Schommers:** basically the current bridge uh signers. Well no the bridge validators sorry. So basically the sequencer it's just there to do preconf. So it just says this is the canonical kill chain and I'm the only person who currently can I mean advance it for the let's say two second two second slots. Then when we get to the sync lot then there's a committee of let's say four out of seven bridge  
**Sebastian Bürgel:** Yeah.  
**Philippe Schommers:** validators currently that become then easy validators or whatever validators and they will then yeah attest to it and then post it to the tool.  
**Sebastian Bürgel:** Okay. Okay. Okay. So let's say there is just a block right now which is just L2, right? It doesn't do any crosschain composibility. So in that case again I get a new block signed from the sequencer with the signature.  
   
 

### 00:20:39

   
**Sebastian Bürgel:** I validate yes it was the se the sequencer which is fine now yeah this this just advances now there is something going across the bridge so in this case in in either direction so let's say from Nosis to mainet now this thing needs to post  
**Philippe Schommers:** To begin with there will just be minorosis right?  
**Sebastian Bürgel:** some sorry say  
**Philippe Schommers:** So in in December there will just be from L1 to L2.  
**Sebastian Bürgel:** again oh  
**Philippe Schommers:** So from minorosis.  
**Sebastian Bürgel:** Okay. So, it's a Hotel California  
**Ben Carvill:** Can you expand on that?  
**Philippe Schommers:** Yes.  
**Ben Carvill:** So, so we can we can only bridge one way from layer 1 to layer two or make calls from layer one to layer two.  
**Sebastian Bürgel:** L2.  
**Philippe Schommers:** Well,  
**Ben Carvill:** So,  
**Philippe Schommers:** okay. No,  
**Ben Carvill:** you can't  
**Philippe Schommers:** you can also bridge out, but probably that's not going to be synchronous. So, it's also going to be a bit slower potentially. But I'm that's I mean, we can still change that. That still has to be defined.  
   
 

### 00:21:34

   
**Philippe Schommers:** But technically,  
**Sebastian Bürgel:** Okay.  
**Philippe Schommers:** we are  
**Sebastian Bürgel:** But the minimum version is syn synchronous bridging in  
**Philippe Schommers:** Yes.  
**Sebastian Bürgel:** asynchronous  
**Philippe Schommers:** Well, you can have a bridge out synchronously,  
**Sebastian Bürgel:** out.  
**Philippe Schommers:** but then you have to trigger it from L1. So that's also a  
**Sebastian Bürgel:** Yeah. Yeah.  
**Ben Carvill:** from a from a liquidity perspective you get the reach of layer one liquidity  
**Philippe Schommers:** possibility  
**Sebastian Bürgel:** Yeah.  
**Ben Carvill:** but you won't be able to I mean if you can't exit that environment let's say liquidate your position using capital on layer one  
**Philippe Schommers:** you can so when you so you can't trigger a transaction from L1 to L2 and then get a response. So basically you can do a flash loan on L1,  
**Ben Carvill:** like  
**Philippe Schommers:** bridge it to L2, do whatever you want to do there. So liquidate whatever and then bridge that back immediately in this transaction to L1. So you can go out, you'll just have to go in through the L1 in this  
**Ben Carvill:** if you're so so if so one of the big things we've communicated so far is like um accessing liquidity from  
   
 

### 00:22:28

   
**Philippe Schommers:** transaction.  
**Friederike Ernst:** Thank  
**Ben Carvill:** across the EBM ecosystem. So like let's say our friend wants to get rid of his Nosis that he's been buying and um  
**Friederike Ernst:** you.  
**Ben Carvill:** can't do so on Nosis because it's low liquidity at the moment. Can he tap into you know layer one you know markets if he wants to?  
**Philippe Schommers:** Yes. Yes, that's fine. It's just that the transaction has to be done on the L1 and not on L2.  
**Ben Carvill:** Yeah.  
**Friederike Ernst:** But as a user, you don't actually have to you don't have to be privy to this, right?  
**Philippe Schommers:** Good.  
**Friederike Ernst:** kind like this can be abstracted away from you. You don't have to know where your transaction is actually  
**Sebastian Bürgel:** But nobody's going to do that, right? So there needs to be a DAP which specifically let's say unis swap.  
**Friederike Ernst:** initially.  
**Sebastian Bürgel:** I would need the unis swap interface which kind of wraps this experience for me which you  
**Philippe Schommers:** Yeah.  
**Sebastian Bürgel:** know  
**Friederike Ernst:** So basically cow yeah for in for instance cow can have a solver that kind of understands this and kind of  
   
 

### 00:23:18

   
**Philippe Schommers:** Right.  
**Friederike Ernst:** like you want to trade something on nos's chain kind of and kind of there you if you need to dip into liquidity from mainet understands it has to initialize the transaction from mainet and uh I mean I mean with kind of intent based architectures you you don't know what transaction exactly you're signing anyways so kind of it's Um uh yeah no and then you're correct Sebastian and that kind of um uh there is kind of on the BD  
**Sebastian Bürgel:** Okay.  
**Friederike Ernst:** side having a money market that understands this logic and taps into it. It's pretty high up on our list.  
**Sebastian Bürgel:** Isn't this just cow swap?  
**Friederike Ernst:** Um so kind of to make sure  
**Sebastian Bürgel:** Like why is this not just cow swap with a bunch of cool solvers that can do that?  
**Philippe Schommers:** Tell  
**Friederike Ernst:** Yeah, I mean so basically it Yeah. So,  
**Sebastian Bürgel:** Okay.  
**Philippe Schommers:** Minute.  
**Friederike Ernst:** but in principle kind of you can also use it um for money market crosschain liquidity, right? And that's not something that that cow swap is natively uh is is going  
   
 

### 00:24:29

   
**Sebastian Bürgel:** What do you mean by that?  
**Friederike Ernst:** to Okay.  
**Sebastian Bürgel:** I I might not understand the the nuance here because to me I I can see that I can use  
**Friederike Ernst:** So, kind of Yeah.  
**Sebastian Bürgel:** cow swap. I can swap anything for anything. What else do you  
**Friederike Ernst:** So kind of the Go for it  
**Ben Carvill:** kind of like cross it's kind of like a cross and unis  
**Sebastian Bürgel:** need?  
**Friederike Ernst:** then.  
**Ben Carvill:** swaps like they have intents baked into the decks right that's what cow swap doesn't quite have at the minute that right  
**Friederike Ernst:** Sorry, you cut out in the middle the it's intent  
**Ben Carvill:** uh yeah so like unis swap uses across intense to do do crosschain  
**Friederike Ernst:** based.  
**Ben Carvill:** transactions within unis swap, right? And you're saying doesn't have that  
**Friederike Ernst:** Yeah. Yeah. Cow swab will have that. So kind of cow swap is going to be a launch partner for for the EZ.  
**Ben Carvill:** limit.  
**Friederike Ernst:** So kind of like their their cow solvers kind of like we're in touch with um I think his name  
   
 

### 00:25:08

   
**Philippe Schommers:** Oops.  
**Friederike Ernst:** is Lewis or something kind of like he manages kind of the the server relations and kind of like solvers will be um asked to kind of integrate this kind of ahead of time. So kind of this is uh we're aware of this uh but also kind of having a crosschain money market um that kind of supports this natively. I mean be it um be it I mean I mean kind of you guys know that kind of like AB is uh kind of nosis is on the shutdown list by AB kind of like in the in the next year or so. So kind of the idea is to kind of either take this open kind of like make it the best money market that you can have with kind of transaction fees from Nosis but liquidity from mainet um or kind of make sure kind of another uh um money market maybe marks new money market kind of uh uh kind of oh kind of  
**Sebastian Bürgel:** What's a money market, Frederick?  
**Friederike Ernst:** like a  
**Sebastian Bürgel:** A lending protocol.  
**Ben Carvill:** Let him knock it  
   
 

### 00:26:20

   
**Sebastian Bürgel:** Yeah. Okay.  
**Ben Carvill:** out.  
**Sebastian Bürgel:** Okay.  
**Friederike Ernst:** yeah you The money market is a proper term.  
**Sebastian Bürgel:** Okay. Yeah.  
**Friederike Ernst:** No.  
**Sebastian Bürgel:** I I don't know.  
**Friederike Ernst:** Is that not something that  
**Sebastian Bürgel:** I'm I'm I'm not the finance guy, so probably it is. I'm I just call it lending protocols always.  
**Friederike Ernst:** okay?  
**Sebastian Bürgel:** Yeah. Okay. So December 31st,  
**Friederike Ernst:** Okay.  
**Sebastian Bürgel:** we have this thing. This thing now allows me either when I have my wallet connected to Ethereum mainet and somebody has a simple DAP or I have an kind of intent marketplace which abstracts it away. So my wallet can be connected even to Nosis L2. Then I can have this experience which means like yeah probably makes sense to build deps which have that which which allow me to do that. Like it seems like for example unis swap right if I just go to app.unis swap.org um that's not going to work out of the box. I need I need some app that  
   
 

### 00:27:19

   
**Friederike Ernst:** This is correct. But if you go if you go to skull swap, it'll work out of the box.  
**Sebastian Bürgel:** exactly that's what I mean. So we have two possible avenues of using this L2 in that state which is a some intent thing. the intent thing can handle it natively. I just need to be connected to Nosis chain or the other path is I have a dedicated app that understands EEZ. So  
**Friederike Ernst:** Yes. But I think kind of like these are these are kind of like from a primary point of view as the user I agree um kind of this is correct but kind of it of course it will lead to the fact that arbiting across Nosis chain and mainet becomes risk-free. Um so kind of um the rates on Nosis chain will be much closer to the rates you would get on mainet regardless even if you don't use this in your transaction yourself as a second order effect.  
**Sebastian Bürgel:** And you foresee that there will be an a or some other other money market on the nosis L2 deployed.  
   
 

### 00:28:27

   
**Friederike Ernst:** We will absolutely make sure that this is I mean AB is currently deployed and kind of like they are also a launch partner for  
**Sebastian Bürgel:** Okay. Okay. Okay. Good.  
**Friederike Ernst:** the easy. So kind of they we can we can yeah but kind of this is this is kind of like uh this is I have a very short list of asks from the BD team that they kind of will be uh held to account for um and kind of an EEZ um uh aware and uh positive uh money market is top of that list that we have some revenue share  
**Sebastian Bürgel:** I mean I guess yeah I guess the important thing here is the re relevant risk committee that has just kind of shut down some markets on a right. So there was this risk committee vote that reduced stuff and I think the relevant part is not so much just the BD folks at AVA but actually saying hey dear risk committee how would you assess the following deployment because that's the one that ultimately decides what's happening what's not happening  
   
 

### 00:29:27

   
**Friederike Ernst:** Yes.  
**Sebastian Bürgel:** right  
**Friederike Ernst:** But kind of like basically I would I would still kind of um uh cluster this under BD activity because kind of like it's a BB activity to kind of  
**Sebastian Bürgel:** yeah sure I'm I'm just trying to think more from a side  
**Friederike Ernst:** Yeah.  
**Sebastian Bürgel:** of the technical design things that we have.  
**Philippe Schommers:** Thank you.  
**Sebastian Bürgel:** Where does that come together with the kind of user you know improvements and it comes together at the point of this risk committee of the respective lending protocol saying yes this thing what you do there is not totally dodgy. we actually find it, you know, reasonable enough and that's why we do this deployment and that's why we have relatively high um, you know, uh, collateralization ratios or whatever they call it so that you can use it. Yeah. Okay. Okay.  
**Ben Carvill:** from from a from a uh I guess a commercial or like um yeah value capture perspective for the instance.  
**Sebastian Bürgel:** Okay.  
**Ben Carvill:** uh you mentioned on the rollup one I don't know I didn't quite catch whether you're working with Titan and Beaver build on like how does the layer one uh sorry how does the instance settle with layer one and how does the how does that revenue model kind of work I guess assuming a few things there but like yeah is there anything from the Nosis instance where we'll capture fees from the sequencer bundler I assume and we'll also maybe is there anything anything that we can capture from  
   
 

### 00:30:52

   
**Friederike Ernst:** Yes. So kind of Yeah. So kind of on on um on the um uh roll up  
**Ben Carvill:** That's  
**Friederike Ernst:** one or roll up zero to one. this is not something where we will capture significant upside.  
**Ben Carvill:** Mhm.  
**Friederike Ernst:** So kind of like this is kind of a reference implementation. Um so it it'll be a glorified test net.  
**Ben Carvill:** Mhm.  
**Friederike Ernst:** Um so kind of for Nosis chain there are several ways we can kind of capture um revenue from it. Um the first one is kind of because Nosis will be um early to the Ethereum economic zone and this is something that kind of like I I was super intent on. So kind of I want to make sure kind of like we are the first significant network in the easy. So kind of like this is going to give us a um narrative and first mover advantage um above others. Um, and I hope um that it can drive sign significant deployments to Nosis chain  
**Ben Carvill:** Okay.  
**Friederike Ernst:** because user experience on noises chain is going to be kind of while while still being extremely secure, it's significantly um it it it's better than on most other L2s that don't have enough liquidity.  
   
 

### 00:32:08

   
**Friederike Ernst:** So kind of kind of if you look at if you look at base liquidity with the common assets kind of it's as good as Ethereum mainet but then kind of you can say okay this is maybe for other reasons you don't want to be on base and kind of like Nosis should be kind of um the default um ecosystem to deploy to if you want Ethereum security but don't want to pay pay Ethereum fees. So that's kind of the um so kind of I want to make sure we try to push Nosis chain as um a as kind of the beating non-ethereum heart of the Ethereum economic zone. Um, so kind of if you deploy your token, if you kind of if you deploy your DAP kind of and you don't want to have your own dedicated DAB chain, Nosis chain is the place to do it to or from. Um, it's kind of this is a pretty um there there is a risk here. There's a risk that this may not happen. um that kind of other rollups will kind of be there first or will be there um not significantly slower than uh we will or that kind of like for some reason kind of like uh there um there's uh a prolonged bare market and people don't want to experiment and kind of it takes longer so kind of the first mover advantage is not as significant and so on kind of it would have to be a first mover advantage by the daps not by the users right kind like you're not appealing  
   
 

### 00:33:45

   
**Friederike Ernst:** to the users here. We're appealing to the devs. So,  
**Ben Carvill:** Mhm.  
**Friederike Ernst:** um, so if this, so I think kind of like we need to, um, try to make this happen for Nosis chain, but if we, if we don't make it happen for Nosis chain, there's a bunch of other ways we can monetize this. So, kind of um, the first one is being an aggregator for other L2s who move into the easy. So kind of if you look at the costs of proving um your network um proving a second network or a third network or who network in parallel um has marginal additional costs. So kind of you can de facto kind of become an aggregator of networks and it's beneficial for everyone because um the the networks kind of pay significantly less than they would if they if they approved themselves um and kind of you can you can become kind of an aggregator of proofs.  
**Ben Carvill:** Mhm.  
**Friederike Ernst:** Another thing that you can do is you can become a um uh a crosschain block builder or um very advanced search depending on how ambitious you are.  
   
 

### 00:34:58

   
**Friederike Ernst:** So block building is an extremely lucrative um uh uh business even right now. So kind of Titan makes 2 million in revenue a week like a week. This is this is this is the actual number. And I'm not talking about the week when kind of like uh uh someone tried to swap things on the A uh wallet and did kind of uh uh paid 30 million for $50, $50,000 worth of A tokens. Um on a regular week, they make $2 million. Um obviously the the the surface here becomes much  
**Ben Carvill:** Thank you.  
**Friederike Ernst:** larger still um when you now uh uh when you now solve for multiple networks in parallel.  
**Ben Carvill:** It's good for their business,  
**Friederike Ernst:** Um so  
**Ben Carvill:** right? Yeah, this this is good for their business,  
**Friederike Ernst:** can yeah we  
**Ben Carvill:** right? We're kind of bringing them a lot of new block space to to  
**Friederike Ernst:** can also 100% so kind of like yeah for build block business is fantastic for the business but we could also become  
   
 

### 00:35:56

   
**Ben Carvill:** process.  
**Friederike Ernst:** a block builder right kind of like we could also kind of instead of being a block builder we could also become a sophisticated searcher and kind of feed the individual builders um these bundles. So kind of there there's there's um an avenue here. Um and we could also do something about um uh kind of being an RBC uh provider for uh for kind of easy um aware network. So kind of there's there's different info plays we can we can take here. Um the most straightforward one is the aggregator because we will already be doing um 90% of the work anyways for proving noises chain. The other ones are additional work, but it's work that um we understand uh kind of I mean especially Martin and uh um and people from the cow team have been very deep um in the block building um stack. So kind of I think we could pretty easily make this um an attractive business. Um yeah so kind of so kind of kind of just to recap uh kind of uh strategy one is make make noses an attractive uh player in the um EEZ realm and get DAPs to kind of deploy on noses then kind of um transaction volume and uh revenue from that is one upside fading that kind of um uh more infra more in play involving other rollups Easy as  
   
 

### 00:37:43

   
**Ben Carvill:** the the as of the 31st of December focusing mainly on I guess it will look like a traditional  
**Friederike Ernst:** well.  
**Ben Carvill:** sequencer almost the more users we have on the network the more fees we'll capture through the use of the the the rollup um I guess um you know something we need to communicate if this is going to go through a GI route which is the conversations So far I've had um with the validator set being sunset uh that's going to have an impact on low utility I think particularly staked no within those validators um you know is yeah have you got any idea of like what that would look I mean it's hard to quantify right now I I understand that but like is there any numbers or any estimates that we've got of like what that looks  
**Friederike Ernst:** Um so kind of I mean yeah I mean basically if you look at how much revenue Nosis chain makes currently here it's on the order of 100,000 XI. Okay that's kind of how much revenue um Nosis chain currently makes. So kind of all the rewards that actually go to the DNO stakers come from the DAO treasury, right?  
   
 

### 00:38:49

   
**Friederike Ernst:** So kind of like this is this is this is incentivized behavior on the side  
**Ben Carvill:** Yep.  
**Friederike Ernst:** of the stakers. Um kind of I mean obviously kind of transaction fees on NOS's chain are super low. So kind of we could raise this um kind of probably by a factor of 10 minimum. We could also um I mean raise the min minimum fee, right? Because kind of like currently blocks are not full. So kind of that it kind of just reverts to the to the minimum fee. Um and that's something that kind of you can set as a parameter of the system. The other thing is kind of in principle we could have a bread sheet. So kind of a couple of bips on transactions in and out of Nosis chain. Um so kind of if if you kind of looked at monetizing this at today's usage levels um kind of that would get you to 1 to2 million a year. Okay. So kind of this is kind of what Nosis chain realistically um makes in terms of revenue um today.  
   
 

### 00:39:46

   
**Friederike Ernst:** Um if if you look at um kind of the so kind of and kind of like if you offset that kind of with um the costs we have for nosis chain obviously it's it's it's a loss maker right? Um so if you look at um kind of what does this mean in terms of economic model for GNO currently NOS's chain is something that um the average GNO holder pays for um except for if you're a staker um then kind of you there's a little bit of redistribution between GNO holders who are not stakers and GNO holders who are stakers but in principle currently um kind of there's no economic upside for the GNO token. So yes, the staking is going to go away, but I expect um transaction volumes to go up because it will be a significantly um improved um user experience and dev experience um on Nosis chain. So kind of and then obviously these transaction fees um they they can again be routed um to the GNO holders, right? So kind of they would be routed into the Nosis Dow and then kind of like I mean they they could be either they could either accumulate their kind of driving value to GNO or they could be kind of uh dispersed in some sort of dividend or buyback.  
   
 

### 00:41:13

   
**Ben Carvill:** Okay. I I do think having that up front would be would be useful in the GIP. AD was currently thinking she's been asking if we could get this up like as soon as the end of this week as the GIP. I think that's that's probably not the right thing to do having heard the timelines, but taking time to like look at the economics of the of the rollup and how that sits on paper, I would be in favor of doing that. Um,  
**Friederike Ernst:** Yeah. I mean, so in principle kind of like um yeah,  
**Ben Carvill:** just general.  
**Friederike Ernst:** I mean I I I I can write something up,  
**Ben Carvill:** Yeah,  
**Friederike Ernst:** but um kind of obviously um yeah, I mean I I I've written it written it I I've written it up before,  
**Ben Carvill:** it's not an easy thing to do.  
**Friederike Ernst:** but I can also write it up again. And uh yeah, so kind of obviously um uh the GN so kind of obviously this would be easier if kind of other parts um of the would already be profit making for the GNO token because kind of like in principle of course the ideas or the profits that kind of acrew go into the DAO and then kind of they are they they kind of are there for the benefit of the GNO holders because the GNO holders own the DAO.  
   
 

### 00:42:24

   
**Friederike Ernst:** Um and obviously kind of if there were any profits um this would be uh easier to illustrate. Uh but yeah, happy happy to kind of write it up  
**Ben Carvill:** Cool. Yeah. I mean,  
**Friederike Ernst:** anyways.  
**Ben Carvill:** if if you want a separate I don't know if it's warrants a separate call and exploration around it as well, but it's quite a big topic to to kind of get out in the open, but would be for it if we could.  
**Friederike Ernst:** Well, it's not that big a topic. I don't think it's kind of like in principle kind of it's I mean sure if you go into the nitty-gritty it can be a big topic but kind of I think the high level idea that people currently have so kind of they I think they don't um I think it always kind of depends on um what um what you kind of take into consideration what you take out of consideration. So currently kind of obviously DNO utility if you kind of look at just the individual stakers you stake it and then you get rewards.  
   
 

### 00:43:21

   
**Friederike Ernst:** Okay. But kind of like if you kind of take kind of the economics of the DAO into consideration too then obviously this changes right because kind of like you're paying kind of like uh from from the pocket of the DAO to the pocket of the people who stake GNO. Um, so kind of it's kind of Yeah. So kind of the economics of kind of of how to view it kind of change changes depending on how far um in or out you zoom  
**Sebastian Bürgel:** I mean on a on a high level uh one one question that in a close Martin and I asked and I would maybe like to bring back also towards Ben's point from what we discussed I can't assess yet if this thing is actually better or strictly worse than NOS's chain and I for one don't look at this from the side of block time, which for many apps that I'm working on is not a relevant metric, but censorship resistance. Does this thing have a forced inbox? For example, if we go live with this on December 31st.  
   
 

### 00:44:26

   
**Friederike Ernst:** I mean in your voice there's going to be a force inbox. I think whether it's going to be there on December 1st is a negotiation with Phillip.  
**Sebastian Bürgel:** I mean, if we for me again,  
**Friederike Ernst:** Philip,  
**Sebastian Bürgel:** sorry to pinpoint things,  
**Friederike Ernst:** you're shaking ahead.  
**Sebastian Bürgel:** but to but  
**Philippe Schommers:** I don't know. I don't know how how complex of a task that is.  
**Sebastian Bürgel:** just  
**Philippe Schommers:** I think it's should be realistic that we have for inclusion in box  
**Sebastian Bürgel:** Yeah.  
**Philippe Schommers:** problem.  
**Sebastian Bürgel:** I mean even if we do have it right I would actually say we actually need more than that. So to have onpar censorship resistance with Nosis chain that we have today the forced inbox of all these L2s today are basically a bit b\*\*\*\*\*\*\* because nobody can use that. So if you are in a position that  
**Friederike Ernst:** Sebastian I yeah I I understand your concern  
**Sebastian Bürgel:** Yeah.  
**Friederike Ernst:** um and uh I mean I've also thought about this um and kind of for now kind of I mean in due course kind of like obviously I want to come to um a point where nosis as an L2 is as censorship resistance resistant as Nosis as in L1 is but currently the fact that Nosis is censorship resistant does nothing for our users right so kind of like there there's no there's no actual advantage that is conferred to them by this in the absence of and and kind of I  
   
 

### 00:45:48

   
**Sebastian Bürgel:** I disagree.  
**Friederike Ernst:** mean so kind of like I don't want to say kind of like I want to forgo this in the long run but I think compromising on kind of the first version of of kind of delivering this is something that I'm very I'm very happy to do if it makes the user experience significantly better um in other respects.  
**Sebastian Bürgel:** I mean, okay, let me make these points because they will be made in public also by other people.  
**Friederike Ernst:** Yes.  
**Sebastian Bürgel:** I disagree because a I don't see a significant improvement in UX by having some faster blocks, you know, for stuff like Nosis Pay.  
**Friederike Ernst:** Liquidity.  
**Sebastian Bürgel:** Yeah. On the liquidity side, I see it right.  
**Friederike Ernst:** Liquidity.  
**Sebastian Bürgel:** But on the liquidity side, it is only as much better as the actual apps get better, which you know, as mentioned, the two paths, which is one, the cow swap integration and B dedicated apps, which remain to be seen which of these will be built. And then if there is no censorship resistance, the liquidity also gets worse because now you as a liquidation mechanism of a I have to rely on the goodwill of the sequencer to let me execute act like liquidations.  
   
 

### 00:47:08

   
**Sebastian Bürgel:** That is a  
**Friederike Ernst:** But this is currently the kind of but this is not something that kind of practically constrains a  
**Sebastian Bürgel:** strictly  
**Friederike Ernst:** because kind of you would actually have to say the same on base and arbitum and so on and oh no they're flourishing.  
**Sebastian Bürgel:** maybe I don't know what what goes through these you know through through the heads of of the risk committee which again to me is absolutely essential to um you know the thing that actually decides what flies and what doesn't fly um but yeah from from my perspective I could very well imagine that you say okay if if a liquidation mechanisms relies on the goodwill of a centralized sequencer without any recourse. Yeah, I mean in my eyes that's strictly worse than what we have right  
**Friederike Ernst:** This is kind of from that perspective. It's strictly west worse.  
**Sebastian Bürgel:** now.  
**Friederike Ernst:** But like if you go to um if you go to um L to beat and you look at kind of like which which um uh um um which uh do not have a for which rollups don't have a for inbox um and kind of if you look at the top of those of course  
   
 

### 00:48:19

   
**Sebastian Bürgel:** Is it?  
**Friederike Ernst:** they have a so it's  
**Sebastian Bürgel:** Yeah, I'm not really sure how to read L2 beat,  
**Friederike Ernst:** Um,  
**Sebastian Bürgel:** I have to admit, at this point. But yeah, I mean anyway, my my point is more um apart from from my perspective here, I think my point is maybe more for Ben. These points will come up. You're proposing something which is strictly worse than no sustain is today. Why the hell do you do that? Why don't you wait another half a year until you have a proper thing out there? This is guaranteed to  
**Ben Carvill:** Yeah,  
**Sebastian Bürgel:** come.  
**Ben Carvill:** I  
**Friederike Ernst:** But I think kind of like yeah kind of I I I mean I see where you're coming from.  
**Ben Carvill:** agree.  
**Friederike Ernst:** Um and I think kind of like here we need to point to the very clear upgrade path to get there. Um and secondly we need to point to um to extent advantages  
**Sebastian Bürgel:** Yeah.  
**Friederike Ernst:** um for um for Nosis uh L2 um namely the liquidity um and kind of the  
   
 

### 00:49:22

   
**Sebastian Bürgel:** Yeah. I mean, and and then lastly, maybe to up the pressure for the BD team here, this is the upside. If the BD team, you know, doesn't deliver this, there is no upside and we end up in a strictly worse situation than we have today. So, it's absolutely essential then that we do have these things on launch day and not a week later. Otherwise, we have no upsides for a strictly worse outcome.  
**Friederike Ernst:** Yes.  
**Sebastian Bürgel:** Yeah. All right.  
**Friederike Ernst:** I mean I wouldn't I mean kind of like I think it depends on how like how you how you quantify strictly wise outcome but  
**Sebastian Bürgel:** Yeah. Right. Right. I I'm I'm overdrawing the picture here a little bit.  
**Friederike Ernst:** I  
**Sebastian Bürgel:** Yeah.  
**Philippe Schommers:** I think for users it's also better because bridging is faster.  
**Sebastian Bürgel:** Okay.  
**Philippe Schommers:** Uh most of them yeah don't care about credible neutrality or anything like that. Uh so I think in the in the user experience it's it's going to be  
**Sebastian Bürgel:** I'm actually, you know what?  
   
 

### 00:50:19

   
**Sebastian Bürgel:** I'm actually not even sure. Do people really not care? When I look at the forum,  
**Philippe Schommers:** better  
**Sebastian Bürgel:** for example, on the framework discussions, people do care.  
**Friederike Ernst:** Yeah,  
**Sebastian Bürgel:** That's at least what they say when you ask them.  
**Friederike Ernst:** but those are the people who kind of engage. I think there's there's a significant survivor bias here. So kind of like if you're going to to um kind of comment on a forum post on the framework for the future for kind of like you you're already kind of like in the 0001% um  
**Sebastian Bürgel:** Yeah.  
**Friederike Ernst:** of people who care about  
**Sebastian Bürgel:** Yeah. Yeah.  
**Philippe Schommers:** Optimism did great without fraud proofs for for years which allowed them to do whatever they  
**Friederike Ernst:** this  
**Philippe Schommers:** wanted in any way they  
**Sebastian Bürgel:** But they lived only on BD and that's not exactly our game,  
**Philippe Schommers:** wanted.  
**Sebastian Bürgel:** right? they did because they did great BD and marketing and that's all they did basically.  
**Philippe Schommers:** Yeah.  
**Ben Carvill:** Even the go to market was through the ecosystem builders like in the end I think even now they've  
   
 

### 00:51:00

   
**Sebastian Bürgel:** Please.  
**Ben Carvill:** pivoted to like an enterprise focused they're having a role at uh institution that they work with um I've got I've got two quick question. Oh, two one quick question and another question just to confirm the gas token might change. I've seen you answer this before. Feder Rica going to stick  
**Friederike Ernst:** the gas token won't change. So kind of basically because it's it's baked into the chain itself kind of in the value um of the  
**Ben Carvill:** his  
**Friederike Ernst:** so and it's it's not impossible to change the gas token. So gas take token with the  
**Ben Carvill:** okay from from a so at the minute we are not taking undertaking in  
**Friederike Ernst:** exact  
**Ben Carvill:** protocol governance um because of the valid data set that's the you know whether that's the definitive reason or not I guess only But one of the advantages if we do not if we discontinue the the validators is that we could maybe introduce uh you know protocol level governance or even introduce standards or ask protocols to input what they might need from the rollup environment.  
   
 

### 00:52:14

   
**Ben Carvill:** Um that seems like a positive that could come from the validator set being retired.  
**Friederike Ernst:** Sorry,  
**Ben Carvill:** Um you're hearing me?  
**Friederike Ernst:** I I don't I don't kind of Can you make the point again because Yeah, I'm hearing you okay,  
**Ben Carvill:** Okay.  
**Friederike Ernst:** but I don't I just don't get the point.  
**Ben Carvill:** Uh so so at the moment we don't do protocol level governance governance in the DAO does not touch into  
**Friederike Ernst:** Yeah.  
**Ben Carvill:** any standards or any protocol tweaks. Um KDR is could we introduce this uh for the instance if we are suns setting the validators. Um like what what what your thoughts going  
**Friederike Ernst:** I think down the line, yes, I think for the for the kind of mid true to midterm future kind of like  
**Ben Carvill:** to  
**Friederike Ernst:** um we'll we'll have to be benevolent dictators here. Um, so kind of I think kind of like we would just um I know you hate this band, but kind of I um uh and I I also trust you to find nicer terms for this kind of for the public.  
   
 

### 00:53:12

   
**Friederike Ernst:** Um but uh this is I I think kind of it's more trouble than it's worst. uh right now there there's an additional element that kind of like we have played with um that um I should add here kind of I don't think kind of like we should um introduce this from day one um but there is the idea of for nosis chain um actually having Sebastian maybe plug your ears an um an explicit censorship protocol protocol so kind of like if Um if you kind of for instance as a um kind of having the having the sequencer and the the and kind of perform a check um that as kind of as a um smart contract developer you can specify the intended business logic of your smart contract and if something happened happens that goes against that.  
**Ben Carvill:** Mhm.  
**Friederike Ernst:** So for instance, if you say okay the maximum um mint policy here is a million tokens and if then someone kind of finds a mint bug and kind of like makes 5 million tokens or billion tokens this is a transaction that just wouldn't go through the validator kind of the the the sequencer.  
   
 

### 00:54:29

   
**Friederike Ernst:** So kind of it kind of having kind of this because if you kind of look at at the state of DeFi right now, um the rates you get on DeFi protocols are only slightly higher than you get kind of with your with your legacy bank. Um and you're taking an awful lot of risk for that.  
**Ben Carvill:** Mhm.  
**Friederike Ernst:** Um, so kind of especially with AIS that specialize in exploiting all kinds of attack surfaces that probably would have kind of would have remained um uh uh covert kind of much longer earlier. So kind of I mean even if you look at I mean if you look at the code base of say AV4 or so this is it's thousands of lines of code. there's there's no way there's no vulnerability in there kind of like and then kind of like you get 1% more than you would get um with Revolute or or N26 or wherever kind of like you have your money in real life that kind of that's a really bad deal kind of in terms of risk adjusted rate that's way below what you actually get offchain.  
   
 

### 00:55:41

   
**Friederike Ernst:** So kind of rebuilding kind of these um these fall back and red kind kind of redundancies kind of is is kind of is very real um value proposition um and obviously you don't want it to be discretionary Sebastian so kind of you shouldn't you don't want kind of like a three three um letter agency kind of to knock at your door and kind of like then and suddenly you kind of you're you're censoring ing specific transactions. But in case something very clearly misbehaves, I I think kind of like this is something that could and I mean then kind of like if you want maximum credible neutrality, you go to mainet. Um but yeah, so it's something that we're currently thinking about. I need to drop  
**Sebastian Bürgel:** And for me, I'm not against this.  
**Friederike Ernst:** to  
**Sebastian Bürgel:** I just think Frederick, we have to make this explicit.  
**Ben Carvill:** Okay.  
**Sebastian Bürgel:** I think a mistake in the past was to slide things in.  
**Friederike Ernst:** 100%. So kind of this is not this is not something that we'll sneak in.  
**Sebastian Bürgel:** I'm just Yeah.  
**Friederike Ernst:** It's something that we'll have a very clear discussion about but kind of like we need to kind of we we need to come up with a  
**Sebastian Bürgel:** Yeah.  
**Friederike Ernst:** mechanism of how it would work and then kind of we can uh but in principle okay I need to drop two  
**Sebastian Bürgel:** Yeah.  
**Friederike Ernst:** um I'll speak with you guys later.  
**Ben Carvill:** John  
**Sebastian Bürgel:** Yeah.  
   
 

### Transcription ended after 00:57:05

*This editable transcript was computer generated and might contain errors. People can also change the text after it was created.*