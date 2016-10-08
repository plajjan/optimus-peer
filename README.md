Optimus Peer
============

**This is currently just an idea. Please get in touch if you find it
interesting and we'll turn this thing into running code!**

Optimus Peer (OP) sets out to enable automatic management of peering
relationships between networks on the Internet. It ranges from initial
establishment through the regular lifecycle of adding additional peering
interconnections and potential depeering of networks no longer meeting your
policy.

Why?
----
Many networks on the Internet manage hundreds or thousands of peering
connections and this requires time. While a great deal of interconnects happen
through human interaction at various events there is just as many, if not more,
that happen through a few emails. It's often a faceless process with a few
simple steps, yet requiring a human touch on either end. Such a process is a
prime target for automation.

Structured peering policies
---------------------------
Today peering policies are written in free text intended for human consumption.
This allows many degrees of freedom and can therefore be used to express
essentially anything in the policy. I could require a peering applicant to send
me pictures of peeled bananas before accepting a peering request. In real life
however, most policies include a few common requirements, such as the minimum
number of interconnects, the geographic spread of said interconnects and total
amount of exchanged traffic. This information can easily be expressed in a
structured format so that it is possible to automatically evaluate a policy.

A structured peering policy could be published, just like its free text
counterpart is today, or it could be kept private, only so that the local OP
system can evaluate incoming requests according to the structured peering
policy.

Route-servers
-------------
Route-servers, you might say. There's a route-server on every IX and so I don't
need automatic peering because setting up a BGP single session to a route-server
isn't very time consuming. Sure, they are around and through them a network can
quickly receive lots of routes and have its own advertisement reach many other
networks.

There are however some drawbacks to route-servers and a few examples are listed
here:
 - Route-servers are SPOFs with a potential of heavy impact on your exchanged
   traffic
 - Doesn't work for PNI or mixed IX / PNI peers
 - Per network policies for filtering are significantly more difficult to
   implement
 - Religion (the Church of BGP frowns upon route-servers)

There are certainly advantages as well, like lower CPU usage on your router as
it doesn't have to maintain as many BGP session or process as many incoming
routes. With todays fast CPUs in RE / RP cards, this is usually of less concern
though.

If you are happy with route-serves you can probably stop reading right now but
if you find them lacking or are sometimes worried about the implications on
your network if one fails or want to handle PNIs, do read on.


The process
-----------
The process of setting up a peer is typically manual. You send an email to
peering@, some guy at the other end accepts or rejects your proposal for
peering and will configure his end. You configure your end and voila, peering
is established.


Now, a lot of the process of evaluating a potential peering partner towards
your established policy can be done in an automated fashion and this is
naturally what OP is all about. It will primarily assist in responding to
incoming peeing requests but could also, with some input, assist in finding new
peering partners and requesting peering with those partners.

Different networks apply different policies to peering, such as minimum amount
of exchanged traffic, a minimum number of interconnects or similar.

It's also fairly common with requirements on operational practices, financial
stability or other information that can be difficult to verify for a system.


* NETCONF vs RESTCONF or both!?
* Express peering policy in YANG
 - Publish or keep it private
 - Augment with local modifications if needed (but be sure to support get-schema)
* YANG model for RPC action to request peering
* A policy could be implemented in a programmatic fashion and be evaluated
  directly when a peering request is received
  - 
* An audit trail can be implemented to allow an operator to follow the process
  and potentially fine tweak the automatic policy
- 

Should it be synchronous or asyncronous? Likely both right? 


== WILL NOT SOLVE ==
- Actual configuration of your devices, that is the job of a different system
- Won't automatically yield peering with Tier1 networks ;)


Processing incoming peering requests
------------------------------------
Processing a request follows these steps:
* Authenticate incoming request
* Validate request data
* Enrich request with local data, typically in the form of statistics
* Evalute request based on policy
* Respond

These steps might be more or less standardised with certain functions being
completely standardised whereas others are custom per deployment.

The processing of an incoming request starts with authentication the requesting
OP system to make sure it is authoritative for the autonomous system stated in
the request. This is an example of a standard function since it should be
applied everywhere and the procedure to authenticate the requesting OP system
should be standardised.

After this first check we can enrich the request with various data. For
example, the amount of traffic from/to the requesting AS can be extracted from
a local statistical system. As there many types of stat systems, this needs to
be written on a local basis. Over time a number of de facto stats-extractors
might emerge but regardless this can be considered a custom function.

The main function is that of actually evaluating whether to peer with a network
or not. While large parts of peering policy are standard and can be handled by
a standard function, one might wish to augment the model and execute a custom
function


Sanity checks
-------------
* Authenticate that requesting OP system is authoritative for the AS in the
  request
* Validate that the IP addresses in a request/proposal are correct. Imagine a
  peering request for a PNI where the requesting party assigns the IP addresses
  to the link. What prevents them from saying the link is 8.8.8.8/31? You would
  configure that on your router and effectively blackhole Googles DNS...
* TODO validate more!?!?!?

Policy requirements
-------------------
We have requirements, such as:
 - minimum of X interconnects
 - minimum of X Gbps of traffic
 - party must operate network of X Gbps backbone capacity
 - ratio of in:out traffic. some networks just care about a relatively even
   ration regardless of direction where others care about both the ratio and
   direction - how to model?

These requirements typically apply to a geographical area. Most networks apply
a global scope and only evaluate whether the two networks, on total, exchange
more than X Gbps but some (typically larger) networks look on a regional basis
to determine whether it makse sense to peer in that specific region.


Regional peering
----------------
Larger networks might choose to implement policies that allow them to peer with
other networks on a regional basis, as a peering partner of such a network one
would only receive routes for that region. Similarly the network would only propagate the 

