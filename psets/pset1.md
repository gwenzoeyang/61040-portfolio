# Problem Set 1

---

## Exercise 1

1. One invariant of the state is that the number of items in the registry is not negative, or not overselling an item (ie. `count >= 0`). The other invariant is that every purchase must have a corresponding request within the same registry for the item that is purchased. Most important invariant: **never oversell** — remaining count must never go below zero. The purchase action is most affected by this invariant, so it must check there’s enough left and subtract in the same atomic step (and use an idempotency key for retries) so two buyers can’t both succeed. The other rule — that every purchase links to an existing request — is less critical; database rules or soft-delete keep it clean, and if it breaks you can fix the records later without hurting users.  

2. The purchase action can break “no oversell” if two people try to buy the last item at the same time — both pass the check, both subtract, and you end up below zero. Fix it by doing the check and the subtract in one step so only one buyer wins; if the step fails, show “no longer available.” Also ignore duplicate clicks/retries, only allow positive quantities, and never let the database store a negative count.  

3. An owner of a registry may want to edit the items they have on it or some other action before they put it back up for others to buy from. This way, they can do that without buyers simultaneously changing the list as well.  

4. Yes, in practice you would want to have an action to delete registries in order to minimize clutter, ensure user data integrity, etc. However, on the user side, they could just close the registry to get it out of the public domain.  

5. The registry owner may execute a query in order to find out what has been bought off the registry and what remains, ie. “Show me the status of my registry.” It would take the `registry_id` and match the owner’s user to the request’s user, then return the set of requests associated with that registry, as well as the set of purchases. The giver of a gift may execute a query to find out what they can still buy, ie. “Show me the unbought items on the registry.” It would also take the `registry_id` and return the set of remaining requests associated with that registry.  

6. We add a **“surprise mode”** setting to each registry (off / hide until close / hide until a date) and an optional reveal date. A simple rule (`canReveal`) says who can see bought items, which should only be the owner when the mode’s condition is met. Also, we create an action to let the owner change the mode/date. The owner’s view follows the rule — show bought items only when allowed; otherwise show all item requests, bought or otherwise. All core rules stay the same.  

7. First, SKUs are unique and do not change. Item names, descriptions, and prices, however, can change depending on the manufacturers and sellers. Also, it is unnecessary for the registry to store more information past “what” and “how many.” Thus, it is better to use SKUs and then refer to the item details in a different catalog. Finally, it prevents confusion over items that are named the same but have different descriptions, or items that are the same but are sold by different sellers and have different descriptions. Essentially, it keeps the data clean.  

---

## Exercise 2

**state**  

a set of Users with
a username
a password

**actions**  

- **register (username: String, password: String): (user: User)**  
  - requires username is available/not taken by another User already  
  - effects creates a new User with given username and password and adds to the set of Users  

- **authenticate (username: String, password: String): (user: User)**  
  - requires username and password pair exist in set of Users  
  - effects treat the login User as the corresponding User to the username  

The essential invariant is that **only a single username and the corresponding password identify the same User every time**. This invariant is preserved because our User set starts empty, so everything is true. The register action first checks that the username isn’t taken, then adds exactly one user and a matching password, so uniqueness and matching stay true. The authenticate action doesn’t change anything — it only succeeds if the username exists and the password matches — so all invariants remain true.  

**state**  

a set of Users with
    a username
    a password
    a confirmation Boolean (False until changed)


**actions**  

- **register (username: String, password: String): (user: User, token: String)**  
  - requires username is available/not taken by another User already  
  - effects creates a new User with given username and password and adds to the set of Users  
  - returns a secret token that will be emailed to the User  

- **authenticate (username: String, password: String): (user: User)**  
  - requires username and password pair exist in set of Users  
  - effects if confirmation is True, then treat the login User as the corresponding User to the username; otherwise, tell the User that they must confirm their account by email first  

- **confirm (username: String, token: String)**  
  - effects asks User to input their username and token. If token inputted matches the token corresponding to the given username, then change confirmation to True and redirect the User to login; otherwise, tell the User that they have the wrong token  

---

## Exercise 3

Passwords are a single, account-wide secret for interactive website sign-in (not for Git over HTTPS). Classic PATs are bearer tokens you can create many of for API/CLI and Git over HTTPS; each can be scoped, expirable, revoked individually, and may require org SSO authorization. In short: **passwords prove “you” in the UI; PATs are limited-power keys for automation that authenticate by the token alone.**  

The GitHub page could be clearer by starting with a tiny “Concepts at a glance” box (Password vs PAT classic vs fine-grained), then two copy-paste examples (`git push` using a PAT; `curl` with Authorization), and a one-line best-practice note: “use minimal scopes and short expirations.”  

### Concept Specification  

**concept** PersonalAccessToken [User, Scope]

**purpose**
enable API/CLI and Git-over-HTTPS access as a user without using the account password;
let users make per-tool secrets they can limit and revoke

**principle**
a user creates a secret token with chosen scopes (and optional expiration);
the token’s value is shown once and must be saved by the user;
later, presenting that token authenticates as the same user
for operations allowed by its scopes, while it is unexpired and not revoked.

**state**

a set of Tokens with
    an owner User
    a value Secret
    a set of scopes Scope
    an expiration Time? // none means no expiration
    a revoked Flag


**actions**  
- **createToken (owner: User, scopes: set Scope, expiration?: Time): (token: Secret)**  
  - requires User is signed in and authenticated  
  - effects create a new token for this owner with the given scopes and expiration (or none), set revoked to false, and return the token’s secret value (shown once)  

- **revokeToken (token: Secret)**  
  - requires a token with this value exists  
  - effects set that token’s revoked flag to true  

- **authenticate (token: Secret): (user: User)**  
  - requires a token with this value exists, is not revoked, and (has no expiration or now < expiration)  
  - effects no change; returns user = the owner of that token  

---

## Exercise 4

### URL Shortener

**concept** URLShortener [User, URL]

**purpose**
map long URLs to short, user-friendly links that can later be expanded back into the original URL

**principle**
a user provides a long URL and requests a shortened version;
they may supply a custom suffix or let the system generate one;
the system ensures each suffix is unique and stores the mapping;
when someone visits the short link, it redirects them to the original URL.

**state**  

a set of ShortLinks with
    an owner User
    an original URL
    a suffix String
    a creation Timestamp


**actions**  
- **createAuto (owner: User, original: URL): (shortlink: ShortLink)**  
  - effects create a new ShortLink with this owner and original URL, assign a unique autogenerated suffix, and store the mapping  

- **createCustom (owner: User, original: URL, suffix: String): (shortlink: ShortLink)**  
  - requires no existing ShortLink uses this suffix  
  - effects create a new ShortLink with this owner, original URL, and given suffix  

- **lookup (suffix: String): (url: URL)**  
  - requires a ShortLink with this suffix exists  
  - effects return the original URL associated with this suffix and route a given User there  

- **delete (owner: User, suffix: String)**  
  - requires a ShortLink exists with this suffix and this owner  
  - effects remove the ShortLink mapping  

- **listLinks (owner: User): (links: Set of ShortLinks)**  
  - requires owner has created at least one ShortLink  
  - effects return the set of all ShortLinks belonging to this owner  

**notes**  
- Because of the use of suffixes in the URLs, they can never overlap, i.e. a given shortened URL can never refer to more than one longer URL. However, one long URL can have multiple shortened URLs, although that is inefficient. This is an invariant (suffixes must be unique).  
- Both user-defined and autogenerated suffixes are supported.  

---

### Conference Room Booking

**concept** ConferenceRoomBooking [User, Room]

**purpose**
allow users to reserve conference rooms for specific times, preventing conflicting bookings

**principle**
a user selects a conference room and a desired time interval;
the system checks availability of the room;
if the room is free, the booking is created and stored;
users can later view or cancel their bookings;
other users can see which times are already taken.

**state**  

a set of Rooms with
    an identifier (Room)
    a capacity Number
    a location String

a set of Bookings with
    a room Room
    a user User
    a start Time
    an end Time

**actions**  
- **createBooking (user: User, room: Room, start: Time, end: Time): (booking: Booking)**  
  - requires room exists and no overlapping booking for that room in [start, end)  
  - effects create a new booking for this user, room, start, and end  

- **cancelBooking (user: User, booking: Booking)**  
  - requires booking exists and belongs to this user  
  - effects remove the booking from the system  

- **listBookings (room: Room): (bookings: Set of Booking)**  
  - requires room exists  
  - effects return all bookings for this room, sorted by time  

- **listAvailableBookings (room: Room): (bookings: Set of Booking)**  
  - requires room exists  
  - effects return all available bookings for this room, sorted by time (times that the room is not currently booked)  

- **listUserBookings (user: User): (bookings: Set of Booking)**  
  - requires user is the logged in user and exists  
  - effects return all bookings made by this user  

- **listBookingsByTime (start: Time, end: Time): (bookings: Set of Booking)**  
  - effects return all bookings that overlap the interval [start, end)  

- **listAvailableBookingsByTime (start: Time, end: Time): (bookings: Set of Booking)**  
  - effects return all available bookings that encompass the interval [start, end) (for all rooms that are not booked during the entirety of this time)  

**notes**  
- Preventing overlaps is the most important constraint, because it creates a real-life issue for people booking.  
- Recurring bookings are not accounted for/enabled here.  
- There is no sign-in feature here, since this is assumed to be on a company/library/university website, where the User would already be signed in with their credentials.  

---

### Billable Hours Tracking

**concept** BillableHoursTracking [User, Project]

**purpose**
record employee work sessions on projects for accurate client billing

**principle**
an employee starts a session by selecting a project and entering a short description;
the system records the start time and marks the session active;
the employee ends the session later, which sets the end time and computes duration;
the system prevents overlapping sessions for the same employee;
forgotten sessions can be ended automatically and flagged for later review.

**state**  

a set of Projects with
    an identifier Project
    a name String

a set of Sessions with
    an employee User
    a project Project
    a description String
    a start Time
    an optional end Time
    an active Flag
    a needsReview Flag

**actions**  
- **startSession (employee: User, project: Project, description: String): (session: Session)**  
  - requires project exists and employee has no active Session  
  - effects create a new active Session with employee, project, description, start set to now, end unset, needsReview set to false  

- **endSession (employee: User, session: Session)**  
  - requires session exists, belongs to employee, and is active  
  - effects set session.end to now and set active to false  

- **autoEndStale (session: Session, end: Time)**  
  - requires session exists, is active, and end > session.start  
  - effects set session.end to end, set active to false, and set needsReview to true  

- **markReviewed (employee: User, session: Session)**  
  - requires session exists, belongs to employee, and needsReview is true  
  - effects set needsReview to false  

- **adjustTimes (employee: User, session: Session, newStart: Time, newEnd: Time)**  
  - requires session exists, belongs to employee, newEnd > newStart, and session is not active  
  - effects update session.start and session.end to the provided times and set needsReview to false  

- **listSessionsByUser (employee: User): (sessions: Set of Session)**  
  - requires employee exists  
  - effects return all Sessions for this employee  

- **listSessionsByProject (project: Project): (sessions: Set of Session)**  
  - requires project exists  
  - effects return all Sessions for this project  

- **listActiveSessions (): (sessions: Set of Session)**  
  - effects return all Sessions where active is true  

**notes**  
- Exactly one active session per employee prevents overlap.  
- `adjustTimes` provides a controlled correction path after review.  
- The `autoEndStale` action would presumably be used by an admin, so there is no “hours limit,” it is just a manual action that will be taken if an admin notices that an employee’s session has gone on too long. It will flag them for review and allow the admin to talk to the employee to come to a consensus.


LLM Usage: I used them to restate my words more succintly and eloquently. I also used them to formulate the skeletons of the concept specifications
