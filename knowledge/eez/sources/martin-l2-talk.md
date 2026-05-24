all right yeah so this will be um a  
controversial presentation um and I kind  
of expect uh to make  
uh well kind of everyone angry because  
uh I'm  
uh criticizing to some extent and the  
existing uh2 and to some extent the  
existing ethereum um core um road  
map but um I think it needs to be done  
so in this in this role uh I'm I'm  
founder of nosis but in in in this  
capacity I'm just speaking as someone  
who has built for over 10 years now on  
ethereum so we started building on  
ethereum when it was still a test net um  
deployed one of the first apps couple of  
weeks after ethereum was released and  
have uh continuously been builders in  
the  
space  
um  
so let me make a few comments on on on  
on the previous presentation I have the  
absolutely highest respect for for Jesse  
for um what Bas uh is doing what  
coinbase is doing in general but I do  
think uh the claim to say uh um we are  
bringing people to  
ethereum we bringing the next billion  
people to ethereum is wrong we bringing  
the next people billion people to base  
and that is a significant difference so  
just a few examples so Jesse was also  
talking about the 30% uh um fee that  
centralized platforms uh can charge from  
you of course referring to Apple that is  
something for example Bas can absolutely  
do as well so if you are app on base  
it's absolutely in their hands to  
control um how much fees you're paying  
for a transaction and of course they can  
start uh charging a 30% uh cut and this  
is typically something that doesn't  
happen early on that happens much later  
once a platform is established uh a lot  
of kind of lock in effects happen and it  
often even doesn't happen with the first  
generation so ultimately base is  
coinbase is a is a regular company and  
yeah there are great people I  
absolutely again have highest respect  
for for for Jesse and and Brian and but  
they are employees at the company and at  
some point they might um leave they  
might retire they might do something  
else and then another set of people  
comes and ultimately it's a company  
controlled by shareholders um there's a  
fiary duty to to maximize shareholder  
value so oh we have this asset base oh  
we can Spike up fees uh we can take more  
fees from from those applications and in  
a way it's even our duty to do it  
because we have to maximize uh value for  
our shareholders so my claim is bringing  
people to those L2 ecosystems is quite  
different from uh being on ethereum the  
claim of course is always rollups  
inherit the security uh  
of ethereum  
and we see labels like it's secured by  
ethereum you kind of get what you get uh  
that you would get from ethereum but the  
reality is absolutely none of those um  
that uh top l2s actually yeah kind of  
truly inherits the security of ethereum  
so if you look into the detail funds can  
be stolen here can funds can be stolen  
here funds can be stolen here funds can  
be stolen here funds can be stolen  
um and then on top of that now we are  
saying okay let's do chain abstraction  
so let's make it invisible to the user  
um on what chain they actually are but  
in a world where the underlying security  
is quite well different and opaque chain  
abstraction actually means in that case  
risk abstraction there was nice talk uh  
two two days ago so if you're  
abstracting the chain you're abstracting  
the risk and that's well that's uh  
that's dangerous so uh at some point  
funds might be lost and then you can no  
longer abstract the abstract the chain  
and you need to tell the user oh sorry  
um your funds are um lost but there are  
even more uh risks that even L2 beat  
does not cover uh yesterday there was um  
a great presentation by by James uh  
showing and again just referring to the  
um to the presentation showing that on  
any chain with a centralized sequencer  
that's again for example based arbitrum  
optimism if you have funds as a user on  
a money market like AA and uh compound  
the sequencer can indefinitely and at  
zero cost prevent you from accessing  
your funds from withdrawing your funds  
and and you might think yeah but you can  
Force include a transaction via L1 no it  
that uh won't help so roughly the idea  
is you create this withdraw request um  
and because the sequencer ultimately  
creat uh can control the exact  
transaction ordering they can alter the  
state of that money market so they can  
just for for a subc borrow uh too many  
funds out of that money market so that  
your request to to get funds out or your  
own funds out fails and then kind of  
directly after you repay so they pay  
zero interest because it's it's it's the  
same time it's in the same same time  
stamp so just kind of those kind of  
things uh yeah in my view again truly  
show if you if you uh build on one of  
those two uh systems you far from  
inheriting or getting um the security um  
that ethereum promises so what does  
ethereum security actually mean it means  
things it's a culture it's a process  
it's not it's it's much more than code  
so uh ethereum security means public  
core develop calls public discussions  
multiple client implementations rigorous  
testing highest level of of Promise um  
of backwards compatibility and kind of  
those things cannot be um derived or  
exported automatically to tol2 just  
because you um yeah kind of deploy some  
some code on ethereum that process and  
that culture cannot be derived of course  
there can be efforts to kind of promote  
that culture and to some extent ethereum  
is doing that and kind of talic is going  
around and say yeah if you don't do  
those things then I won't call you an L2  
anymore so yeah that culture can we can  
try to promote this culture and L2 be is  
of course doing a great great job in in  
kind of trying to export that culture  
but again it cannot be derived  
um um with code so looking at another  
data point  
um again the claim uh assets are secured  
by ethereum but the reality is um only  
or it's it's it's the part that is  
actually of assets that is actually  
coming from ethereum is is getting  
lesser uh or the fraction is getting  
smaller and smaller so most assets again  
this is for example base but it's very  
similar on on uh on on other chains and  
on on smaller Chains It's even much  
worse most aets are actually not coming  
from ethereum so this is here the  
canonical  
um uh the oh yeah so so back here in  
time uh n the canonical Bridge was still  
the dominant so that that is kind of the  
one the assets that are actually kind of  
secured by ethereum now uh the dominant  
part are native assets so those are  
assets issued on the  
on the L2 or on on that other chain  
directly and they do not uh inherit  
there's not a concept of exiting um  
those to ethereum  
um and there are reasons for that so if  
you use the canonical Bridge uh and you  
currently and and you want to derive  
that security and you currently want to  
say let's say send a message from uh  
from arbitrum uh to optimism and and get  
a message back  
that mes that transfer took takes two  
weeks so kind of if you actually use  
economical Bridge it would take you two  
weeks to do that so A pigeon is faster  
in transporting a message for most parts  
of the world so yes no one is using that  
mechanism instead we're using uh new  
kind of bridges or external bridges that  
don't inherit the security um of  
ethereum so if most assets  
um are not natively bridged and  
sequencing is also not done by ethereum  
so as it's not bridged means not from  
ethereum not secured by ethereum  
sequencing not done by ethereum then the  
role of ethereum kind of um yeah uh is  
reduced to being kind of this  
checkpointing uh um yeah checkpointing  
um systems so here um again rollups  
could choose to uh yeah how how they how  
they uh build blocks and they can either  
choose to use their own  
sequencing um which means they can do  
super fast confirmations uh they capture  
their own me  
um and but the disadvantage is that they  
can only um not synchronously read uh  
into ethereum and that is a rational  
choice to do if you optimize for  
connectiveness to other chain or for um  
uh yeah for for treadly or you can go  
kind of what's called based um where you  
let ethereum do the block building and  
and the sequencing and here you are  
really optimizing for connectiveness to  
ethereum um but that's only rational if  
it's more important for you to be uh  
yeah as closely connected to the kind of  
economic zone that ethereum is but the  
reality is only 1% of um of of of value  
um is is is choosing um uh to do  
that so here is my my proposal for  
ethereum to uh to fix that and to  
address those issues and the proposal is  
that ethereum itself  
should develop and deploy uh ZK uh  
proven EVMS rollups and deploy 128 equal  
instances of that that are highly oper  
interoperable and that are truly kind of  
if you built on those you're truly built  
on ethereum so what would that mean  
being built by ethereum it would mean  
that don't even think about introducing  
multii that would be Unthinkable that  
ethereum deploys something and it's it  
has some some multis um as the upgrade  
mechanism it would mean that we at least  
have well we want to have multi client  
so we would have at least uh two  
independent implementations of let's say  
yeah the proof systems that's the most  
important part part here rigorous  
testing thousands of eyes that look at  
uh at the stuff and actually care about  
all the details that um are are are yeah  
often Pulled Under the rck um in  
uh so the idea is yes we can still have  
l2s wide range of designs within that we  
have based L2 so l2s that use ethereum  
for Block production and for well um and  
for yeah sequencing SL block production  
but even a subset of those would be  
native L2 so truly built by ethereum  
governed by ethereum and again we can  
here see uh some some things so  
um uh reads into L2 are synchronous here  
um and but of course also the economic  
perspective is important so uh so just  
it only in a native rollup essentially  
all value of that rollup is is captured  
by well ethereum  
itself now one question could be is is  
is that actually um sharding what what  
I'm I'm I'm proposing here no not  
exactly so sharding would be a design  
where you would have um in a way  
multiple L2 instances and that was kind  
of still the promise a couple of years  
ago that we would have  
1,24 uh shards so essentially instances  
of ethereum L1 uh and and they would all  
be kind of live on the same layer what  
I'm proposing is is not that but instead  
but still something that comes tries to  
come as as close as possible have the L1  
and then have those yeah 128 equivalent  
um l2s but still um in in this hierarchy  
and looking more at it  
um yeah some some uh things  
so so on an L1 you can do synchronous  
reads and writes and that's what we all  
love about about ethereum this uh  
composability the composability  
uh within this this larger system would  
still be pretty strong so from an L2 the  
idea is you can still do synchronous  
reads into ethereum so meaning you can  
execute some code on the L2 you can  
actually read a contract uh on L1 in  
that execution so let's say in price  
Oracle or or any any anything that lives  
on ethereum and you can kind of even  
yeah read a function continue with that  
result in the in the L2 process um and  
and do things so kind of on the L2 can  
do things dependent on the state of the  
L1 so it has immediate transparent  
access the other way around from the L1  
you cannot read immediately the state of  
the l2s that is asynchronous reads but  
wres are actually synchronous so if you  
can very well do a transaction um on L1  
that in the same transaction so  
atomically also creates a right into the  
L2 and affects the state you will just  
not in the context of L1 be able to um  
to immediately get kind of the result of  
that state change so again the read will  
be um  
asynchronous same uh in communication  
between l2s so you can synchronously  
make a transaction that affects the  
state of two l2s  
but again you cannot read from one L2 uh  
to the other so the general idea is  
stuff that everyone should be able to  
read um in kind of contentious State  
might live on on on the L1 and on all  
the l2s they can access the state  
synchronously um but there can be much  
more um non-contentious  
uh um state so all the things I have  
proposed so far would be possible today  
on ethereum without any any um kind of  
upgrades or changes uh to ethereum it  
would essentially just take the the um  
yeah kind of the ethereum community to  
decide um to build that of course if we  
do that we can actually make those l2s  
um even more powerful so few things or  
two things I'm proposing here that could  
can be done  
um if we make those L2 native we can and  
make them also native to the economics  
of ethereum so ethereum of course let's  
say issues eserve to reward um  
validators to participate in the  
consensus in the same way it could also  
kind of redirect or direct this issuance  
uh towards um proving the correctness um  
uh of all uh of all those um  
l2s another one is um that I'm  
suggesting uh  
that those l2s should have distinct name  
spaces so meaning an address should be  
clearly um attributable to to one L2 or  
to to one uh kind of in a way chain  
either L1 and L2 uh and in my view that  
is a big problem that right now you have  
an address and it can and the same  
address on on various chains can mean  
very different things so for example if  
you have a safe um then actually it it  
can the same address can exist on  
multiple chains but it can have  
completely different owners so what I'm  
suggesting here is that we shouldn't  
have this address uh collusion so each  
uh L2 um kind of uh uh yeah uses an  
additional salt to uh to to have its own  
address space and if we have that um we  
could we could allow sending a message  
from the L2 with its unique address into  
the L1 and actually have um  
have the message sender um uh on the  
transaction be that unique address that  
only exists um on on on the L1 so that  
allows you to do things like let's say  
you hold a token on L1 by an address or  
a contract that is only lives um on an  
L2 but yeah um some technical details  
so here's kind of the Spiral um I I  
expect or I see already happening uh um  
if we don't go this route so if we don't  
go this route it means um the economic  
zone that is ethereum and ethereum block  
building um becomes less relevant and  
therefore it already makes it less um  
less attractive to use ethereum as a  
block Builder or as a sequencer and  
again we already see this today 99 of  
the economic value of L2 choose not to  
be based and not to use ethereum as um a  
block builder at the same time more and  
more um Native or assets are not coming  
from ethereum and are not kind of really  
secured by ethereum either they are  
natively issued or they use external  
Bridges so for those the economic  
security of ethereum also matters less  
and less so in uh in total that means  
that um the relationship between rollups  
and ethereum really becomes weak weaker  
and weaker and yeah I I'm going so far  
and say it just becomes a meme so now so  
essentially in my view um that's the  
crossroad we are uh in front of with  
Native rollups we can kind of continue  
to have ethereum the most relevant or  
economic zone I would I would claim that  
should be the goal to be the most  
relevant economic zone even in the world  
in in in the world that's a place where  
prices  
are there's also the other perspective  
some believe ethereum should be a meme  
or is a meme um and they promote this  
idea of um yeah let's say iser is money  
and their perspective might be um if we  
do Native rollups we kind of damage the  
meme we damage our Collective um maybe  
religion or or kind of and because we we  
create tension within this uh uh nice  
family that we are that we all are so we  
disenfranchise those rollups so  
therefore the East meme gets weaker and  
all all that economic stuff doesn't  
really matter it's just about saying  
is's money and spreading eer and  
therefore it's better to uh uh to not do  
Native uh  
rollups all right so final uh final  
slide  
um for whom is it to decide uh what  
route we go absolutely for for all of us  
um that are here for the for everyone  
who is who is ethereum and we are all  
ethereum it's not just um let's say the  
core develop some core developers or or  
the ethereum foundation um essentially  
um it is um yeah ethereum anyone can  
essentially affect changes um uh to  
ethereum well thank you very  
much thank you so much and uh while  
we're still on that note we have some  
questions from everyone here and the  
first one is there areund 28 identical  
rollups which one should I deploy my dap  
on yeah um  
so um so I mean the first important part  
would be that right now you if you want  
to to to uh to deploy somewhere right  
now well you either use L1 ethereum or  
you have to choose one of those many  
ecosystems be it super chain or egg  
layer or or something and this would  
give you the opportunity to actually  
just say Okay I want to be um on on  
ethereum now um if you um uh if you um  
have to choose a particular one then  
again reads and writes within a  
particular um L2 are are synchronous so  
if there are other applications you want  
to regularly interact with then yes it  
would be wise to um to choose that one  
if if you're fine to say okay I just  
need um I just need to uh uh access I I  
I want this close access to the L1 um  
they are all equally close to the L1  
then you really should just choose the  
rollup that is used the least um and and  
because that will be um um the  
cheapest why not implement this on  
nosis yeah I mean um maybe but but but I  
think really I mean the um well ethereum  
is is is is is the big thing so ethereum  
is that um uh has that credible chance  
to be this most important um um uh  
economic  
zone as as much as I would like nosis to  
become that um we we are not there we I  
mean ethereum is 100 times bigger and um  
and and again I mean um saying that  
again so there are all those ecosystems  
and NOS this is one of them that that  
that also tries to do uh valuable things  
and again I think that's totally valid  
I'm just saying I'm not saying if you  
come to nosis you come to ethereum no  
you come to nosis and we try to do  
valuable um things there and in the same  
way uh I would say other ecosystems  
again uh all the they are great  
ecosystems and they bring uh Great Value  
but you should understand then you are  
building on that ecosystem and not  
necessarily um on  
ethereum all right why 128 Why not start  
with four or 16 yeah I mean um the the  
idea is really to uh to make it clear  
that building um on ethereum or building  
here um is is a long-term viable and  
that um and and and and that of course  
addresses costs so I do think well I  
absolutely also want to share the vision  
to bring a billion people to uh on chain  
and um and and and for that we ethereum  
needs to be much more uh ambitious and I  
think 100x in in effect increase in  
effective block Space is really what  
what should be aimed for on a timeline  
for let's say two years I I I would uh  
say could be realistic for that um and  
not not a Forex or something like  
that all right the next one wasn't  
necessarily a question it was more a  
compliment they just agree completely  
and they're thanking you for stating the  
hard truth um but then we've got do you  
you consider this as rugging existing  
rollups yeah I mean uh to well rugging  
yeah  
um so I think I think existing rollups  
um well already have big time Advantage  
they alive um they are already built so  
that that will take at least yeah again  
I think let's say another two years so  
but but then yes if existing rollups  
don't will will not offer anything other  
than kind of just provide evm space then  
they will have a strong competition but  
I am uh I do absolutely think that all  
those uh all those yeah existing rollups  
are great people innov in Innovative  
people and I think they will be able to  
uh to kind of evolve and and um and  
create something or or essentially have  
additional Innovation on top and if they  
do that then yeah there can just be much  
more uh kind of Base uh basic evm block  
space as we know and love it since 10  
years that's what I'm proposing here to  
just create much more of that and then  
there can be those l2s that are more  
Innovative that do additional things  
that do Cutting Edge maybe buil-in  
privacy may maybe some form of other  
sequencing that is is much faster there  
are all kind of ways um uh to to  
innovate so I think yeah again both both  
absolutely can uh can live all right  
unfortunately we are out of time thank  
you everyone for your questions and  
thank you once again Martin for your  
phenomenal talk

All

From Ethereum Foundation

Ethereum  
