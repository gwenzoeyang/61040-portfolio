# Project Ideation — Problem Framing

## Domains (brainstorm → pick 3)

**Ten domains (short phrases):**
1. School course planning  
2. Collectibles (Pokémon cards, etc.) tracking  
3. Collectibles trading  
4. Textbooks for school (trading specifically)  
5. Roommate/block finders  
6. Budgeting/spending tracking  
7. Getting Pokémon cards at retail prices / restock tracking  
8. Splitting several bills in large groups  
9. NYTimes games that are now paywalled  
10. Old items thrown away at the end of the school year

**Three most promising & why (2–3 sentences each):**
- **School course planning.** Wellesley moved the course browser behind login, so third-party tools like Coursicle no longer work for us. There’s a clear gap for a student-friendly planner that lets you build schedules quickly, find classes that fit open windows, and consider cross-registration travel. The demand is immediate and local.
- **Textbook trading.** Textbooks are expensive and marketplaces take cuts/fees, so students lose value on both ends. A campus-scoped, trust-preserving swap/sell model (school-only identity + meet-in-person verification) could reduce cost and waste with a straightforward workflow.
- **Splitting several bills in large groups.** Trips and group events create lots of back-and-forth payments and mistakes. A tool that nets everyone’s balances down to one clean transfer per person reduces friction and confusion.

---

## Problems (3 per chosen domain → pick 3 to pursue)

### A) School course planning — three problems (short phrases)
- Good UI & actual planning workflow needed  
- Accounting for transportation to MIT  
- Richer decision info (beyond blurb)

### B) Textbooks for school — three problems (short phrases)
- Money lost to third-party services  
- Low-income students disproportionately impacted  
- Trust is important (fear of scams)

### C) Splitting several bills — three problems (short phrases)
- Multiple payments back and forth  
- Chasing after people for each purchase  
- One person unfairly fronting everything

#### Selected problems (title + 2–3 sentences each)
- **Wellesley Course Planner: Simple, Powerful UI.** Workday is overcomplicated for “planning,” and Coursicle no longer supports Wellesley—students need fast add-to-schedule flows, time-fit search, and cross-reg support. Solving this core planning workflow also enables travel buffers and richer information in one place.
- **Campus Textbook Swap Without Platform Fees.** Students lose money to marketplace fees/markups. A school-only, in-person-verified swap/sell flow removes middleman fees and increases trust while keeping the implementation simple (no in-app payments).
- **Group Trip Net-Settler (No More Ping-Pong Payments).** Instead of dozens of reimbursements, netting reduces everyone to one payment they owe or are owed. It’s widely applicable and produces a clear, tangible benefit.

#### Unselected problems (1–2 sentences each on why excluded)
- **Accounting for transportation to MIT.** Valuable, but it’s naturally addressed inside the selected planning app with travel-time buffers.
- **Richer decision info.** Also folds into the planner (link to RMP, syllabi), so it doesn’t need to be a separate problem.
- **Low-income students disproportionately impacted.** It’s a motivation for the textbook solution rather than a separate problem to solve.
- **Trust is important.** This is human/behavioral; we’ll mitigate through school-only identity and in-person inspection within the main textbook solution.
- **Chasing after people for each purchase.** Behavioral; the netting tool reduces occasions to chase but can’t eliminate human follow-through issues.
- **One person fronting everything.** Allowing multi-payers is useful, but it increases reconciliation complexity—better solved by the main netting feature.

---

## Stakeholders (3 per selected problem; then 2–3 sentences on impacts)

### 1) Wellesley Course Planner: Simple, Powerful UI
**Stakeholders:**
- Group A — Wellesley students who used Coursicle before (lost a tool they liked)  
- Group B — Wellesley students who never used Coursicle (struggle with Workday UI/permissions)  
- Group C — Wellesley administrators/registrar staff (care about policy, privacy, and data access)

**Impacts (2–3 sentences):** Groups A and B get easier planning (fit-by-time, cross-reg travel buffers, richer info) and less frustration. Group C benefits from fewer complaints but may worry about access control; the app should respect login-gated data via the student’s own credentials and avoid public redistribution.

### 2) Campus Textbook Swap Without Platform Fees
**Stakeholders:**
- Group A — Low-income students (most harmed by costs/fees)  
- Group B — Third-party textbook sellers (profit today from fees/markups)  
- Group C — Wealthier students (minimally affected; convenience-oriented)

**Impacts (2–3 sentences):** Group A gains cheaper, local access and keeps more resale value. Group B may lose volume/fees; Group C remains neutral to mildly positive since participation is optional.

### 3) Group Trip Net-Settler
**Stakeholders:**
- Group A — People who owe others (want clarity and fewer actions)  
- Group B — People who are owed (want confidence they’ll be made whole)  
- Group C — Third-party payment methods (transaction volume may fall)

**Impacts (2–3 sentences):** A and B reduce errors and time spent coordinating. C is neutral unless their revenue depends on many small transfers; then volume could dip slightly.

---

## Evidence & Comparables (about 10 links per selected problem; 1–2 sentences each)

### 1) Wellesley Course Planner: Simple, Powerful UI
1. Reddit thread where students ask how to make class schedules… shows persistent demand and ad-hoc workarounds like spreadsheets. <https://www.reddit.com/r/college/comments/8k101h/what_do_you_use_to_make_a_class_schedule_plan_for/>  
2. Comment noting manual scheduling is labor-intensive and error-prone, underscoring poor tooling. <https://www.reddit.com/r/college/comments/8k101h/comment/dz4557d/>  
3. Comment citing Coursicle as a top option historically, highlighting the gap now that it doesn’t support Wellesley. <https://www.reddit.com/r/college/comments/8k101h/comment/dzjg8l7/>  
4. Wellesley’s course browser itself requires login/VPN off-campus, which breaks unauthenticated third-party planners. <https://courses.wellesley.edu/>  
5. Workday is confirmed as Wellesley’s system of record for student data and registration—any planner must align with it. <https://www1.wellesley.edu/workday>  
6. Workday Student page shows how registration actually happens, guiding planner UI/flow alignment. <https://www1.wellesley.edu/workday/student>  
7. Official Wellesley–MIT Exchange Bus schedule enables adding realistic travel buffers for cross-reg classes. <https://www.wellesley.edu/about-us/offices-departments/transportation/shuttle-bus-schedule>  
8. Private Wellesley Q&A forum post: Above-average upvotes vs typical baseline → signals authentic local demand. <https://github.com/gwenzoeyang/61040-portfolio/blob/main/assignments/assignment1/IMG_41C6D0EE21B1-1.jpeg>
9. Personal communication: “Yes, I do think that Wellesley could do with a better course planner, since Workday is actually so hard to use” -Tongtong Ye, Wellesley '26.
10. Personal communication: "I would definitely use something like that, especially if it could account for travel times on the loco. I have lab in the middle of the day a lot, so I would definitely be interested in something like that." -Joyce Nishimwe, Wellesley '27.

### 2) Campus Textbook Swap Without Platform Fees
1. Students vent about textbook costs… anecdotal but representative of widespread pain. <https://www.reddit.com/r/college/comments/rw1s5h/textbooks_are_ungodly_expensive/>  
2. EducationData.org summarizes survey data (~$285/yr on materials; some skip meals/work more), quantifying the problem. <https://educationdata.org/average-cost-of-college-textbooks>  
3. Campus op-ed argues for cheaper options and documents student experiences, showing grassroots demand. <https://calvinchimes.org/2023/09/18/textbooks-are-too-expensive-students-should-have-cheaper-options/>  
4. Library blog cites cost stress and typical spending, adding institutional perspective. <https://www.bentley.edu/library/in-the-know/yelling-floors-students-stressing-about-textbook-costs>  
5. Analysis of why textbooks stay expensive (professor choice, bundling/access codes), explaining structural causes. <https://bookscouter.com/blog/why-are-textbooks-so-expensive/>  
6. Reddit shows where students actually shop and how price drives behavior (Amazon/ThriftBooks, etc.). <https://www.reddit.com/r/college/comments/rw1s5h/textbooks_are_ungodly_expensive/>  
7. Reddit experiences with Chegg rentals—reduce upfront cost but owners don’t recoup value; trading/resale still needed. <https://www.reddit.com/r/college/comments/rw1s5h/comment/hraum31/>  
8. Reddit reports FB Marketplace/PDF scam concerns—school-only identity + in-person inspection can mitigate this. <https://www.reddit.com/r/college/comments/rw1s5h/comment/hrao31o/>  
9. Reddit notes library borrowing constraints (limited copies; re-borrowing to cover a full term is hard). <https://www.reddit.com/r/college/comments/rw1s5h/comment/hr92f4z/>  
10. Quora explainer on high pricing (even for digital) reinforces why students seek fee-free, local alternatives. <https://www.quora.com/Why-are-college-textbooks-super-expensive-even-for-the-digital-version>

### 3) Group Trip Net-Settler (No More Ping-Pong Payments)
1. HerMoney roundup compares popular split apps and their trade-offs—useful to position a net-settling alternative. <https://hermoney.com/connect/friends/best-apps-for-splitting-bills-with-friends/>  
2. Reddit r/apps thread “tired of Splitwise” signals appetite for simpler flows or different features. <https://www.reddit.com/r/apps/comments/1izw2zx/im_tired_of_splitwise_any_other_suggestions_for/>  
3. Reddit comment flags Chipp’s currency limitations—international trips need better support. <https://www.reddit.com/r/apps/comments/1izw2zx/comment/mf7asrr/>  
4. Reddit r/travel thread shows recurring pain splitting many small travel costs over time. <https://www.reddit.com/r/travel/comments/1gade4w/how_to_split_small_costs_during_travel_with/>  
5. Reddit (Kochi) captures the social awkwardness of splitting with friends—automation reduces friction. <https://www.reddit.com/r/Kochi/comments/1db7k79/how_you_all_deal_with_splitting_expense_among/>  
6. SplitBillCalc is a simple single-bill calculator—fine for one bill, not for long, multi-expense trips. <https://splitbillcalc.com/>  
7. Tricount subscription note (comment) shows sensitivity to paywalls for full feature access. <https://www.reddit.com/r/travel/comments/1gade4w/comment/ltcu4nh/>  
8. Common-fund approach can leave leftovers/shortfalls—net-settling avoids this. <https://www.reddit.com/r/travel/comments/1gade4w/comment/ltcxmeq/>  
9. Kittysplit demonstrates low-friction, account-less sharing, but doesn’t optimize minimal transfers across many expenses. <https://kittysplit.com/>  
10. Spliito emphasizes simplicity and multi-currency but is still more calculator-style than full net-settlement with reminders/proof-of-payment. <https://spliito.com/>

---

## Features (3 per selected problem; title + 2–3 sentences each)

### 1) Wellesley Course Planner: Simple, Powerful UI
- **Live course import per term (via your own login).** Pull the current term’s sections/times each semester using the student’s credentials. This keeps data accurate without redistributing gated data, and it enables saved planning states that evolve over time.  
- **Cross-reg travel buffers (“loco-aware”).** Let students block out transit windows between campuses from chosen departure/arrival stops/times. The planner prevents accidental overlaps and suggests realistic sequencing of classes across campuses.  
- **Richer decision tiles (RMP links + past syllabi if available).** Surface RMP links and any shared syllabi/past materials alongside times and credits. This helps students evaluate workload/fit beyond a short description.

### 2) Campus Textbook Swap Without Platform Fees
- **Location-first search + in-person verification.** Filter by distance/campus and let buyers inspect items before committing. This reduces fraud risk and eliminates the need for fee-taking intermediaries.  
- **School-only trading mode.** Restrict listings to verified campus emails to raise baseline trust and make meetups/logistics easier. A lighter trust framework also means simpler moderation.  
- **No in-app payments or fees.** The product only connects buyers and sellers; all payments occur outside the app. That keeps costs at zero for students and avoids complex payment compliance.

### 3) Group Trip Net-Settler (No More Ping-Pong Payments)
- **Auto netting across many expenses.** Members log who paid what; the app computes a minimal set of transfers so each person pays or receives once. This eliminates dozens of small reimbursements.  
- **Multi-payer aware (no single “designated payer”).** Multiple people can front costs throughout the trip, and the system still nets correctly at the end. This spreads burden without creating reconciliation chaos.  
- **Due-by reminders with proof-of-payment check.** Gentle nudges and a simple “mark paid” with optional receipt/photo reduce chasing and confusion. Exports to your preferred payment app keep the tool payment-agnostic.

**GPT use: used to put my bullets and ideas into full sentences!**
