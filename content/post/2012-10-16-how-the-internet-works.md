---
title: How the Internet works
author: joefkelley
layout: post
date: 2012-10-16
url: /10
sfw_pwd:
  - lefFFMoEh5Ld
categories:
  - Tech

---
Let's talk about the internet. Everyone knows how to use it, but almost nobody actually knows how it functions. If you think about it, there's definitely some magic going on here. When you type a website into your browser, e.g. "www.joefkelley.com", how does it know which server it needs to talk to? How does your data get sent there? How does it get sent back? There's a bunch of intermediate computers that all of this has to hop over; how do they know where to route the traffic to? It's a complicated process, and one that even I, being a pretty tech-savvy guy, was ignorant to until recently. But it is an interesting topic, and if you've ever wondered how it works, I'm going to try to write this in pretty basic terms but still cover the main important parts. Even still, there's a lot of layers, so strap in...

**Part 1: Packets**

This is a simple thing, but one that is crucial to understand. All traffic on the internet is in the form of "packets." These are small chunks of information that are transmitted as a whole. They contain a lot of meta-data, such as where they are going, where they are coming from, and a variety of other fields depending on the type of the packet. It also contains some "payload," that is, the actual data that is being transmitted. The specifics aren't too important here, but it's important to understand that data is always sent in small chunks, not as a continuous stream, even in the case of applications that we think of as "streaming", such as video, or a game. Think of it like mailing a letter, not like a telephone call.

**Part 2: IP**

This is the one fundamental part that I feel is most commonly understood, but there are still some subtleties. Every computer that is connected to the internet has something called an "IP address." Here, "IP" stands for "Internet Protocol," to give you a sense for how crucial this is to the internet. An IP address is usually a series of four numbers, each 0-255. For example, the IP address of the computer hosting this blog is 54.243.253.248. The basic idea behind the IP address is that it's how computers recognize each other. They are, with some caveats, unique, so if your computer wants to talk to this server, it attaches the "destination IP address" to all of its outgoing packets. It also attaches its own IP address as a "source IP address" so that its recipients know who to respond to.

The one thing that is less commonly-known about IP addresses is that they are hierarchical and regional. That is, IP addresses that match on their first three numbers are very likely to be located next to each other. It's similar to if we wrote addresses like this: "USA.NY.New York City.5th Avenue.350". The more IP addresses match, the closer they are. But matches later in the address only count if the rest matches too. It's the same with physical addresses. We wouldn't expect "Canada.British Columbia.Vancouver.5th Avenue.350" to be close to the New York address, because not even its first part, "Canada" matches "USA," even though the last parts match.

This is not by accident. When IP addresses are given out to internet service providers (ISPs), they are given out in chunks. So an ISP might have all of the addresses of the form "12.34.56.[0-255]". We write this group as "12.34.56.00/24" where the "/24" means that every address in the group (called a subnet) matches on the first 24 bits. The exact meaning of of the number of bits isn't important; just know that the higher the number, the more specific the group is. Getting back to our physical address analogy, the USA would be a "/8" group, New York City would be a "/16", and 5th Avenue would be a "/24". The ISPs are responsible for laying out their network such that computers in the same group are close to each other. There's no law that says they have to, but it behooves them to do so as it will make their network more efficient.

**Part 3: Routing**

So when your computer wants to send a packet to "54.243.253.248," it can't do it directly, because it doesn't have a direct connection to that IP address. In fact, it's likely that your computer is only directly connected to one other computer, your home router. There's a vast network of routers of various sizes that are all directly connected to only a few others, but indirectly connected to every other computer. When a router receives a packet, it looks at the packet's destination address and decides which of its neighbors to pass it on to. It does this by maintaining something called a "routing table." This is a series of entries, each of which contain a minimum of two pieces of information: a subnet address and a next hop - the neighbor to pass the packet on to. Let's walk through an example. Let's say a router called "A" is directly connected to routers B, C, and D. Its routing table might look like:

  * 10.20.30.0/24 - B
  * 10.20.0.0/16 - C
  * 20.55.0.0/16 - C
  * 0.0.0.0/0 - D

You might notice that it's possible for a given IP address to match more than one subnet. For example, 10.20.30.1 matches the first, second, and fourth entries in this routing table. When this occurs, the packet is sent on to the most specific subnet, in this case 10.20.30.0/24, so it is sent to router B. With this in mind, you can see that the last entry, 0.0.0.0/0 - D, is very useful. If the router has no idea where to send a packet, it just hands it off to router D, which is likely to be more widely-connected to the internet. This is common for routers near the "edge" of the internet.

So how do routers get their routing tables? They periodically send messages back and forth containing the subnets that they know how to route to. So looking at our above example, it's reasonable to assume that, at some point, B sent a message to A saying "I know how to get to 10.20.30.0/24". There are a lot of different and complicated policies that routers can implement to decide which routes they share, which they prefer, how often they share them, etc. but the basic idea is that all computers on the internet tend to talk to each other to gradually understand how to route packets around. Additionally, each computer does not need to know the entire route; it only needs to know the next hop.

Again, the physical address analogy holds here. The New York City post office might have no idea where a specific address in San Francisco is, but it knows where the SF post office is, and the SF post office has assured the NYC post office that it knows where the address is. Same idea here, but with more hops and more individual computers.

So now we have enough to understand how a packet gets from one point to another. When your computer wants to talk to this blog's web server, it sends a packet out that contains the data, it's own source IP address, and the server's destination IP address. Your router gets the packet, checks the destination address, looks up that address in its routing table, and sends it off to the next hop. This continues, getting closer and closer, and getting to more and more specific subnets, until it eventually reaches a router in front of the web server, which sees a packet for an IP address that it knows how to route to directly, and the packet arrives at the server. You might expect as many as twenty or thirty machines between your computer and any other computer it is sending a packet to, although something closer to ten or fifteen is more common.

This is essentially the core of the internet, and it is because of how this works that the internet is reliable, redundant, and uncontrollable. If any individual router stops working, there are many, many other paths through the internet. Even if a whole ISP stops working, there are enough others that it is unlikely anyone except that ISP's customers would notice it. There is no single way that the core of the internet could ever be completely shut down.

**Part 4: TCP**

This is all a fantastically complicated process already, with a number of things that could go wrong. What if a router along the way is broken? What if a packet is lost? Or what if you have to split your data across multiple packets, and they take different routes and get to the destination in the wrong order? It is for all of these reasons that most internet traffic uses something called "transmission control protocol" or "TCP." It is basically a sequence of messages between two machines that establish known information about a persistent connection, and makes sure everyone knows exactly what data has and hasn't been received by each computer. Here's a simplified example of the type of messages that might get sent between two computers, A and B:

  * A sends to B: "I'd like to start a TCP connection with you"
  * B sends to A: "OK, let's start a TCP connection"
  * A sends to B: "I have received your last message and we have a connection"
  * A sends to B: "Here's data packet #1: [data]"
  * B sends to A: "I have received data packet #1"
  * A sends to B "Here's data packet #2: [data]"
  * B sends to A "I have received data packet #2"
  * ... more data exchanged ...
  * A sends to B "OK, let's finish this connection"
  * B sends to A "OK, closing"
  * A sends to B "OK, closed."

If, for example, data packet #2 were to be lost on the way to B, A would realize that B never acknowledged the receipt of that packet and could re-send it. There are a lot of subtleties and optimizations to the actual protocol, but the basic idea is to provide more reliability on top of the basic packet routing that goes on. TCP is an immensely complicated protocol in reality, and has gone through many versions, but it serves a pretty simple problem: handle it gracefully when things go wrong.

It's worth noting that not all internet traffic uses TCP. There are applications that use other, less-reliable, or even more reliable protocols. For example, if streaming a movie, you might not care if one individual frame of that movie doesn't quite go through, so it's not worth resending a single packet if it gets dropped. But for most common applications, TCP is used.

**Part 5: DNS**

There's one more important piece of the puzzle: how to go from a human-readable address, like "www.joefkelley.com" to a machine-routable IP address. The answer is the "domain name system" or DNS. It works through a similar idea to packet routing: each machine doesn't need to know all the answers, but it needs to get you closer to someone who does. So there are a handful of "top-level" domain name servers that your computer knows of by default. Your computer sends a message to one of them, aksing "where is www.joefkelley.com?" and they won't know the answer directly, but they might respond "I don't know, but 12.34.56.78 handles .com domains." So your computer contacts 12.34.56.78, asks again, and it would respond "I don't know, but 98.76.54.32 handles joefkelley.com domains." So you contact 98.76.54.32 and it _does_ know where www.joefkelley.com is, so it gives you the IP address.

One interesting thing to note here, is that the large name-servers are the closest thing that the internet has to a "single point of failure." If just those handful of machines go down, the whole domain name system would stop working, and nobody would be able to visit websites unless they knew the IP address already. Related to this: DNS is the only part of the internet that the U.S. government controls directly. If a site is doing something illegal, the department of homeland security can and will remove their DNS entry, effectively taking the site offline without even touching their actual servers.

**Part 6: Other topics**

...and that's it! That's the whole internet! Kidding, of course; there are tons of other protocols and issues to tackle - link-layer protocols like WiFi and ethernet, transport protocols other than TCP like UDP, what the heck a NAT does, how everything is cached, and then of course, security. There's a million different malicious things a hacker could do with knowledge of how the internet works. But hopefully this covers the basics and gives you a sense for just how complicated the whole process is. Even after understanding most of how it works, I am more amazed by it than ever. The internet is an incredible feat of engineering and it's a remarkable human achievement.

If you're interested in another post going deeper into one of these topics or into something else that's related, let me know and I'd be happy to write one.
