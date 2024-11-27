# Introduction

Bitcoin was introduced to the world as a peer-to-peer electronic payments network.
Prior to its introduction, many other forms of digital payment networks came and went:
[Digicash](https://en.wikipedia.org/wiki/DigiCash), [e-gold](https://en.wikipedia.org/wiki/E-gold),
[b-money](https://en.bitcoin.it/wiki/B-money), [beenz](https://en.wikipedia.org/wiki/Beenz.com), and
many others.

Since bitcoin there have been tens of thousands of new digital currencies created. What makes bitcoin
so special? Why did it earn a premium in the market?

Bitcoin was designed and optimized for security. When it was launched in January 2009 it was sent into
a hostile world where governments and central banks had strong incentives to shut it down. Because of
this it needed to make considerable tradeoffs to protect itself and ensure its survival. How can you
protect a monetary network from highly capable and motivated attackers? Make it difficult to come under
the control of a small group of people. As a consequence of the decision to prioritize security above
all else, usage as an every day payments system is less than ideal.

Settlement times for a transaction are 10 minutes on average: roughly enough time for the most recent
block to spread globally and the network to reach consensus; any faster and you run the risk of
the operations centralizing in a small region.

Fees are market driven: if there is a lot of demand for using bitcoin, then users pay higher fees
to make transactions. This is a necessary cost to prevent a denial of service and to impose some
amount of discipline on the users. Often the fees are quite managable, but as bitcoins become more
valuable even a "small fee" could mean a lot in purchasing power.

Miners use proof-of-work to compete for who gets to write to the blockchain but this means fewer
entities are able to do it. Because of this you have to submit transactions and wait for the miners
to pick it up and write to the blockchain. There are no guarantees a miner will pick your transaction
for the next block or even at all: if you submit a transaction with 0 fees, it's valid, but because
it costs miners energy to show proof of work they need to be compensated and likely won't pick your
transaction.

So bitcoin can't scale? We're doomed to a suboptimal payment network? No!

Enter the [lightning network](https://lightning.network/).

# Payment Channels

Imagine going to a bar and setting up a tab. Instead of paying for every drink as you order, the tab
keeps track of how much you owe. When you're ready to leave, you settle the total amount in one transaction.
Similarly, a payment channel lets users make many small transactions off the main Bitcoin blockchain,
only recording the final balance on the blockchain when they're done, which makes transactions faster and
cheaper.

How does this work? You create a single on-chain transaction where the balance is divided in some way between
two users. Those users can then update the balance between themselves as often as they'd like for free
and when they're ready to part ways they broadcast a final transaction to the bitcoin network that distributes
the funds into a unique output ([UTXO](https://en.bitcoin.it/wiki/UTXO) in bitcoin terms) for each user.

What's useful about this setup is that if Alice and Bob have a payment channel, and Bob and Carol have one,
then Alice can send Carol a payment without having to open a channel to her. She can send Bob a request
to pay Carol by paying him through their channel and he can then pay Carol through their channel. You can
repeat this by adding more and more channels between people.

Payments solved, right?! Well, it's complicated. This certainly works and fairly well at that! However,
if in our example above Alice has sent all of her balance to Bob's end of the channel then she can't
make any more payments. She can still receive money, but she'll need to open a new channel if she wants
to spend. This is an area of active research on how to optimize these channels. At the moment it can
be daunting for beginners to manage their channels effectively and since each channel requires 2 on-chain
transactions it can be difficult for users with small balances to justify the fees.

So only the bitcoin "power users" and software engineers can use payment channels? No!

Enter [ecash](https://cashu.space/).

# Ecash

In 1982 David Chaum conceived of ["ecash"](https://en.wikipedia.org/wiki/Ecash) and implemented in Digicash.
Ecash is the digital analog of physical cash. If you have a bill you can hand it to someone there's no record
of you having done so. There are no intermediaries standing between you and the person you're handing the cash to.
The cash is a liability of some bank somewhere but for the purposes of the transaction only the two people
doing business are directly involved. This is great for privacy and for minimizing fees. How do we do that
with bitcoin and what's the catch?

Wouldn't it be great if you knew someone who already has established payment channels and is willing to do the
work to maintain them and possibly open new ones when necessary? And they are responsible for keeping their
server up and running so payments can flow? That can be a part time job and a barrier for complete beginners.
With [Cashu](https://cashu.space/) someone with payment channels setup can run a service called a "mint" that
allows the balance to be broken up into "bills" (or bearer tokens for the finance nerds) and sent between any
two people whenever they choose. The bills are redeemable from the mint via payment over their existing payment
channels (or perhaps opening a new one).

The catch? You're trusting that the mint will honor their liabilities. The mint operator, the person who owns
the payment channels, can decide at any point to keep the funds. Cashu was designed so that if the mint does
this, they can't selectively choose who to steal from: they have to steal from everyone or noone. Ecash users
don't create an account when interacting with the mint so the mint itself has no idea who has what balance and
therefore can't deny anyone in particular.

Still, it's very important to remember that an ecash mint ultimately holds the keys for the bitcoin so be careful
which mints you trust. If you have a trusted friend capable of maintaining a lightning network node then consider
using them for a mint. You can also ask trusted friends or forums for recommendations on established mints.

Ecash is infinitely scalable in theory and great for privacy but is still in its infancy in the bitcoin ecosystem.
It's considered risky to trust a mint. Treat ecash just like regular cash in your wallet. It's ok to have enough to
spend that day or the next week but you could lose your wallet or be robbed (ie. the mint runs off with your funds).
Eventually this will mature into a reliable system with less reliance on trust, but we're not there yet.
