# PSET 2

### Q1

1.1.  In simple terms, a context is just the bucket where we keep track of which short
codes are already used. In a URL shortener, the bucket is the base domain (like
tinyurl.com, sho.rt, or your custom domain). Each domain has its own bucket, so
the code /abc123 can exist on different domains without clashing. This makes it
easy to guarantee uniqueness per domain and lets us expire and reuse codes
inside one domain without affecting others. Without contexts, every code would
have to be unique across the entire system, which is harder to manage.

1.2.  The spec says we keep a set of used strings so we know what’s been taken and
can preserve the uniqueness of URLs. But if we use a counter, we don’t actually
need to store all those strings. The counter itself represents the set: every
number from 1 up to the counter can be turned into a unique string. So the set in
the spec is like the exact values, but if we go “in numeric order,” then we only
need to keep track of our last used nonce.

1.3.  One advantage is that the link is easy to remember, since it will just be a single
word, thus making it easier to remember than if we went with numeric order.
However, this also makes it easier for people to stumble across your link, which,
if it is private, could pose security issues. For modification, give each context a
word list. The generate action should make a code by picking 1–3 random words,
joining them with “-”, retrying if it’s taken, then recording it as used. Expose
simple knobs (addWords/blockWords, word count, separator); if combos run low,
increase the word count or fall back to the counter/random scheme.

### Q2

2.1.  Because the generate step only needs to know which domain/base it’s
generating a code for. NonceGeneration cares about the context (the
shortUrlBase) to avoid collisions; the targetUrl doesn’t matter for picking a unique
suffix, so it’s omitted there. In the register step, we’re actually creating the
mapping from that suffix (under the base) to the targetUrl, so we need both the
targetUrl and the shortUrlBase (and the generated nonce). The when-clause lists
only the arguments each step needs to bind and pass along.

2.2.  Because it only works when the action’s parameter/result name and your local
variable name are identical and there’s no risk of confusion. In real syncs you
often need to rename things—e.g., bind NonceGeneration.generate()’s result
nonce to shortUrlSuffix for UrlShortening.register, or map an expireResource
result named resource to a local shortUrl. You may also have name clashes (two
different actions both have a url), or want to document the data flow clearly
across concepts that use different field names. Writing the full argName:
varName makes the mapping explicit, avoids ambiguity and accidental capture,
and is safer if action signatures change later. So we omit names only when it’s
trivially clear; otherwise, we keep them for clarity and correctness.

2.3.  The first two syncs include the request because they need data from it to do their
job: generate needs the shortUrlBase to pick a unique code in the right context,
and register needs both the targetUrl and shortUrlBase (plus the generated
nonce) to create the mapping. The third sync, setExpiry, doesn’t need anything
from the original request—only the shortUrl returned by register—so it listens to
the completion of UrlShortening.register() and uses that result directly.

2.4.  Make the domain implicit. Drop shortUrlBase from the request and from the
“when” parts of the first two syncs, since users no longer choose a base.
Internally, treat the domain as a fixed constant (e.g., “bit.ly”): the generate step
calls nonce generation for that single default context, and the register step pairs
the returned nonce with the same fixed domain to create the short URL. The
setExpiry sync is unchanged because it only needs the shortUrl returned by
register.

2.5.  **sync** expireAndDelete
      **when** ExpiringResource.expireResource(): (resource: shortUrl)
      **then** UrlShortening.delete() (shortUrl: shortUrl)

### Q3

3.1. 

**concept** ShortLinkOwnership [User]
- **purpose** record who owns each short URL so analytics can be private
- **principle** after a short URL is claimed, that user is its owner until the short URL
  is deleted/expired
- **state**
  - a set of Claims with
    - shortUrl String
    - owner User
- **actions**
  - claim(shortUrl, owner: User)
    - **requires** no claim exists for shortUrl
    - **effect** saves (shortUrl, owner) as a Claim
  - isOwner(shortUrl, user: User): (yes: Boolean)
    - **requires** shortUrl exists
    - **effect** returns whether user owns shortUrl
  - forget(shortUrl)
    - **requires** a claim exists for shortUrl
    - **effect** removes the claim (used on delete/expire)



**concept** AccessAnalytics [User]

- **purpose** count accesses for each short URL; only the owner can view/reset
  counts
- **principle** for any shortUrl, the counter equals the number of successful lookups
  since creation/reset; only its owner may read or reset it
- **state**
  - a set of Counters with
    - shortUrl String
    - count Number
- **actions**
  - recordAnAccess(shortUrl)
    - **requires** a shortening exists for shortUrl
    - **effect** creates a counter if missing and increments count by 1
  - getCount(shortUrl, requester: User): (count: Number)
    - **requires** ShortLinkOwnership.isOwner(shortUrl, requester)
    - **effect** returns the count for shortUrl
  - resetCount(shortUrl, requester: User)
    - **requires** ShortLinkOwnership.isOwner(shortUrl, requester)
    - **effect** sets count to 0
  - deleteCounter(shortUrl)
    - **requires** a counter exists for shortUrl
    - **effect** removes it (used on delete/expire)


3.2.  **sync** claimOwnership

- **when**
  - Request.shortenUrl (user)
  - UrlShortening.register (): (shortUrl)
- **then** ShortLinkOwnership.claim (shortUrl, owner: user)


**sync** countOnLookup

- **when** UrlShortening.lookup (shortUrl): (targetUrl)
- **then** AccessAnalytics.recordAccess (shortUrl)


**sync** viewAnalytics

- **when** Request.viewAnalytics (shortUrl, user)
- **then** AccessAnalytics.getCount (shortUrl, requester: user)


3.3.  Features:

3.3.1.  Letting users pick their own short URLs is a good idea because it allows
users to customize and organize their links better. You can implement this
by adding an action that takes the user, their proposed shortened URL,
and the target URL, and run it through a tiny “suffix rules” checker (no bad
words, right length, etc.). If a user asks for .../my-brand, validate it
and, if OK, register it. Ownership + expiry work exactly as before. This is
a valid idea because it allows for more autonomy for the user and more
specificity, so they can understand what they are clicking when they click
it!

3.3.2.  Using words instead of gibberish codes is a good idea because it makes it
easier for the user to pass the URL along and repeat it to others. Add a
word-based generator with a dictionary (and a blocklist for inappropriate
words). Then, instead of using consecutive numbers or creating random
codes, use that generator to make a short phrase (happy-bear-birthday,
or the like, basically 2–3 words joined) and then register it.

3.3.3.  Showing analytics grouped by target URL is a good idea because it
allows for owners to compare and contrast. However, in order to protect
user information, you must make sure that the shortened URLs are only
grouped into URLs with the same owner user. In the state of
AccessAnalytics, add a targetURL to the Counters. Then, on each lookup,
you can either look up the individual shortened links associated with your
user, or the targetURL and all the shortened links for it on your account.
Making sure only that owner can view it is important. (This avoids leaking
stats across different people who shorten the same target.) But this could
be highly useful if you are trying different marketing strategies, and using
different shortened links to see which strategies led to the most clicks, or
similar scenarios.

3.3.4.  Making links hard to guess is not necessarily a good idea. Previously in
this assignment, I said that it is useful for security reasons to have a
hard-to-guess link. However, if someone is that concerned about security,
it is a given that they would already have additional security measures
that prevent random people from accessing the targetURL. Thus, this only
creates undue pain for the people access these links, as the entire point
of a shortened link is that it is easier for the user to recognize and type
into their computer. However, by making them “hard to guess,” they
become more inaccessible and lose their original purpose.

3.3.5.  Letting users see link analytics without making an account is not a good
idea. This will result in a breach of privacy and trust between the User and
website. Say, for instance, that one User is a company, running an
experiment on various marketing strategies for users (measuring success
by their frequency of clicks). A rival company, if allowed to see link
analytics, could then steal this information about their marketing
strategies and use it for their own betterment, which is obviously a breach
of trust between the original user and the website. There just really is not
good reason for non-owners to need to see the analytics of shortened
URLs that are not theirs. Also, the point of having this website is
supposedly to make it profitable, meaning that we would want to
incentivize people to create accounts. Allowing them to view analytics
without an account would disincentivize them.


I used AI in this assignment to again, reword my bulleted answers and to help me brainstorm.
I also used it to create skeletal outlines of concept designs so I could fill them in.
