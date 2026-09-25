# Building Tax Technology: Lessons from the Front Line

**Event:** Tax Technology Bite LIVE! — KPMG Meijburg, Amstelveen, 7 October 2026
**Slot:** 10:00–10:45 (opening keynote, 45 min)
**Audience:** Tax directors, heads of tax

**Rough timing:** opening 3 min · seven lessons ~5 min each (35) · takeaways 3 min · buffer 3 min

**Principle:** slides carry high-level insights that apply to any tax technology; concrete examples live in the speaker notes.

---

## Slide 1 — Title

**On slide**
- Building Tax Technology: Lessons from the Front Line
- Aleksandra Bal, Global Indirect Tax Technology Lead, Stripe

---

## Slide 2 — Setting the scene

**On slide**
- What we built: an in-house tax engine for 100+ countries and 13,000+ US jurisdictions, embedded in the Stripe ecosystem — checkouts, subscriptions, invoices
- What we learnt applies to any tax technology you build, buy or implement

**Speaker notes**
- This is the example I'll come back to throughout the talk. I'll refer to others along the way, but this is the main one.
- Personal background: trained as a tax lawyer, advised on VAT, then studied computer science to build the systems I used to advise on.
- Tax is part of the product, not an add-on — that's why the engine is embedded rather than bolted on (sets up lesson 3).
- Building a tax engine in-house is uncommon, which is why these lessons are worth sharing.
- Most of you will buy or configure rather than build. The lessons are the same: they're about what the technology has to cope with, not about who writes the code.

---

## Slide 3 — Lessons that apply to any tax technology

**On slide**
1. Global ambition meets local reality
2. What you need to unlearn as a tax specialist
3. Getting started right
4. The data model decides everything
5. AI is a tool, not a destination
6. Measure outcomes, not activity
7. The job with no job description

**Speaker notes**
- The order is a project lifecycle: understanding the situation → changing how you think → planning → building → running → proving → staffing.
- Lessons 1 and 2 are the mindset. Lessons 3 and 4 are before and during the build. Lessons 5 and 6 are operating it. Lesson 7 closes on the people who do all of the above.

---

## Slide 4 — Lesson 1: Global ambition meets local reality

**On slide**
- You can't research everything up front — design for being wrong, and make it cheap to be wrong by keeping decisions reversible
- Clarity won't come, so make an educated guess and move fast
- Prioritize by revenue, volume and enforcement exposure, and document the gaps you accept

**Speaker notes**
- You have to start building before you have certainty. Every design rests on unverified assumptions — you can't research 100 countries up front, and speed matters.
- Once you accept that some assumptions will be wrong, make it cheap to be wrong: keep decisions reversible, and make rules configurable where possible.
- For some things you will never have clarity — possibly for many years. You can't wait, so you make an educated guess.
- Coming from EU VAT, I thought Europe was complex. US sales tax changed that: SaaS is taxed differently from state to state.
- Vagueness that won't resolve:
  - Whether US marketplace facilitator laws apply to certain transactions.
  - How to identify a business customer: a tax ID in some countries, a certificate of registration in others.
  - California taxing SaaS from 1 January 2027, with guidance still open. (Keep details general; CDTFA guidance is still being developed.)
- What we did: focused on the jurisdictions that matter most to the business. Everything else is a non-prioritized gap with a documented risk assessment.
- Enforcement exposure is not only about the tax administration acting against the company. It can also mean personal exposure for the people who sign.
- Example: you've started selling to the UAE but haven't registered yet. You now file a registration, and the question is whether to disclose the earlier sales and possibly pay a penalty. As a tax director, how comfortable would you be signing a registration application certifying that there were no prior sales — knowing how severe the penalties are there, and what it could mean for you personally the next time you travel through Dubai?
- The point: when you decide whether to comply, look at the full picture, not only penalties and interest.
- Bridge to next slide: the hardest part of this wasn't technical. It was unlearning instincts.

---

## Slide 5 — Lesson 2: What you need to unlearn as a tax specialist

**On slide**
*Good tax lawyers make bad tax software* (italic line above the list)

- Don't surface every exception in the product
- Don't aim for zero risk
- Count the cost of compliance, not only the cost of non-compliance
- Don't wait for certainty — decide with ~70% of the information

**Speaker notes**
- Good tax lawyers make bad tax software. As a consultant, an opinion that didn't list every exception felt like malpractice. In a product, listing every exception is the malpractice.
- Every exception: a checkout or API that surfaces every exception is unusable; the decision tree never terminates. Build the patterns that carry the overwhelming majority of volume, and give the long tail a conservative default or a manual override.
- Zero risk: a tax engine across many countries and thousands of jurisdictions will not capture every nuance; at that scale it's a permanent condition. Triage by materiality and exposure, accept what falls below the line, monitor what sits near it. Refusing to ship until nothing can go wrong means shipping nothing — a bigger risk nobody puts on the register.
- Cost of compliance: every increment of coverage is a rule to build and maintain, a field to collect, a person to hire, friction for the customer. Ask two questions: what does it cost if it goes wrong, and what does it cost to prevent? When the last mile costs more than the exposure it removes, it's a bad trade that feels virtuous.
- Certainty: much of what a tax product handles isn't settled law (e.g., platform economy, deemed supplier rules). Bezos: decide with around 70% of the information you wish you had. Make decisions flexible and reversible.
- The legal training isn't wasted. The rigor moves upstream: it belongs in the architecture, not on the screen.

---

## Slide 6 — Lesson 3: Getting started right

**On slide**
- Think slow, act fast — get the scope and the timeline right
- Don't make good projects look bad — an optimistic timeline is how a healthy project acquires a bad reputation
- Understand the commercial reality: know which constraints you can influence and which you cannot
- A seat at the table is not impact at the table
- Understand the total cost, not just the price of the solution

**Speaker notes**
- Planning is cheap, so do not save money there. Planning and prototyping cost almost nothing; the real costs are incurred once you start building. "Think slow, act fast" is the core advice of *How Big Things Get Done* (Bent Flyvbjerg and Dan Gardner).
- What happens when you rush planning: you discover problems during implementation — functionality and features that still need to be built. Either the project takes much longer, or it gets abandoned after time and effort have already been spent building. Backtracking once you've started is expensive.
- Not a contradiction of lesson 1: lesson 1 is about researching hundreds of jurisdictions before you build a global solution — there is no time for that. This lesson is about planning the implementation and the technical architecture, because once you start building you commit resources.
- Don't make good projects look bad. Projects often appear to go wrong only because we created that impression. Say three months when six or seven is realistic, and after three months people start asking why it wasn't delivered. It's no surprise that it wasn't — but the project acquires a bad image, and so do the people on it. Sometimes it doesn't end well for them: they're blamed for non-delivery, purely because someone was too optimistic in the planning.
- Commercial reality: the product architecture sets your most important constraints, and some can't be removed — no amount of tax reasoning will change them.
  - Tax would say the checkout should always collect a full address. Product will say it hurts conversion, and they will not do it. So we collect a full address only where absolutely needed (the US); elsewhere the country, plus province or postal code for Canada, the UAE and India. Tax IDs are validated at the point of entry.
  - Some fields are needed to calculate tax (Canada, India), others only for reporting (UAE emirate).
  - Build without thinking about tax and you get situations like this: the marketplace operator is legally required to collect and remit tax but never receives the money, because it goes directly to the seller. We had to implement additional money movements to fix it. Once the product is built, its constraints are almost fixed — that's why tax has to be in the room when it's designed.
- Seat at the table: tax teams are always told to get involved as early as possible. But being in the room is not the same as being heard. [To develop: how tax professionals get their message across — communicating with impact.]
- Total cost: not just what you pay the vendor. Count the cost of running it — your own infrastructure (AWS, data sitting in S3 buckets), the people, the maintenance. Know the number, because when the answer is "there is no budget", knowing your current costs is how you find room: reduce what you already spend instead of asking for more.

---

## Slide 7 — Lesson 4: The data model decides everything

**On slide**
- A wrong data model creates workarounds on workarounds, until nobody understands the data
- Design for problems you've never seen, but build for the ones you have — your scope drives your complexity
- Test any system against your hardest jurisdictions, not your easiest

**Speaker notes**
- Problems you've never seen: what if jurisdiction boundaries change? What if one transaction carries several taxes instead of one? In the US this is normal — boundaries change, and one transaction can carry state, county, city and district taxes at once. As a European tax professional, you wouldn't even know this is a problem until you meet it.
- Example: a customer in San Francisco. On top of the state and the local (city/county) tax, you collect at least three different district taxes. (San Francisco is a consolidated city and county, so say "local tax" or "city-county tax", not two separate taxes. Confirm the current district list with CDTFA's rate-by-address lookup before the talk.)
- Similar surprises closer to home — country ≠ tax territory: the Canary Islands and Réunion are in the EU but outside EU VAT. Bonaire, Sint Eustatius and Saba are part of the Netherlands but outside the EU, with their own tax (ABB) instead of VAT.
- One transaction ≠ one tax: in Canada, federal and provincial taxes can apply together, and you may need two customer tax IDs to stop charging tax at both levels. (Say "provincial", not "state"; check "reverse charge" wording with a Canadian specialist.)
- US: an address isn't a jurisdiction. We built a geolocation system: geocode the address to latitude and longitude, then place the point inside the polygon of the right taxing jurisdiction. Boundaries change, so the polygons need ongoing maintenance.
- Optional: ZIP codes are postal routes, not tax boundaries. Ask your vendor whether it uses rooftop geolocation or ZIP codes.
- If you don't support cross-border sales of goods, you don't need to model for them. With a broad scope like ours, you do.

---

## Slide 8 — Lesson 5: AI is a tool, not a destination

**On slide**
- Rule #1 of machine learning: don't always use machine learning
- Match the model to the task; routine work rarely needs a frontier model
- Every agent adds risk; prefer workflows where the task allows
- AI amplifies mediocrity: everyone can produce anything, but not everything should be produced

**Speaker notes**
- Our tax engine has no AI. It's purely deterministic, by design: the same answer every time, and explainable.
- We use AI to build the engine: coding and bug resolution.
- Monitoring hundreds of websites: a frontier model would be too expensive. Detecting a change doesn't need a model at all; a light model judges whether the change matters. Light models suit data manipulation, monitoring and frequent routine operations.
- We built a tax bot to help colleagues with tax questions, and many customer support bots. Agents are extremely hard to test. Three agents at 70% success each give about 34% for the whole system (0.7³).
- Activity is not accomplishment. Everyone can now produce anything, and some people mistake volume for value.
- Someone still has to be able to explain and defend every output. AI makes producing work cheap and checking it expensive.
- Some people want to use AI for everything. The result is AI slop: memos that use legislative references as decoration; AI-generated SQL queries that break at some point, and no one knows why.
- Bridge to lesson 6: mistaking activity for accomplishment is exactly what bad metrics reward.

---

## Slide 9 — Lesson 6: Measure outcomes, not activity

**On slide**
- Much of tax's value is invisible: work that prevents or deprioritizes something leaves no trace
- When a measure becomes a target, it stops being a good measure
- Even "accuracy" is ambiguous: accurate against the law, or against your own decisions?
- Start with two or three outcome metrics, and watch what people optimize for

**Speaker notes**
- Invisible work: we spend a lot of time tracking legislative developments. Some never materialize. Some materialize, but we deliberately don't implement them because they're deprioritized. That is value we provide, but something not enacted or not implemented doesn't count anywhere. It's invisible, and very difficult to show.
- So although we are part of a revenue-generating product, we still regularly have to answer the question: "What are you actually doing here?"
- The first metric we were asked to report: usage of AI tools. It measures activity, not value. Judge the work, not the tools used to produce it.
- McNamara fallacy: measure what's easy, disregard the rest, assume it's unimportant, then assume it doesn't exist. Preventive work (legislation analyzed but never enacted, a deadline prepared for and then postponed) disappears.
- Goodhart's Law: Wells Fargo's "products per customer" target led to millions of unauthorized accounts and billions in fines. In tax: "returns filed on time" without an accuracy counterweight.
- Accuracy: if we knowingly make an assumption we know is incorrect, is the engine inaccurate — or is inaccuracy only when we're surprised by the result? Measure two things separately: accuracy against the law (risk exposure) and accuracy against your decisions (engineering quality).
- Outcome metrics to consider: audit assessment outcomes, post-filing correction rates, penalties and interest, time to deliver guidance for a new product or market, and whether material regulatory changes reached the business before they had operational impact.
- Optional: judgment isn't binary (adverse audit outcomes in grey areas); tax teams don't control their inputs; the cost of measurement itself.

---

## Slide 10 — Lesson 7: The job with no job description

**On slide**
- Work changes faster than job descriptions
- Functional homes own recurring work; missions handle temporary, cross-functional work
- Work can be fluid; accountability can't
- Staff missions with flexible people who are willing to learn

**Speaker notes**
- Open with my story: hired to build a tax engine for EMEA. My first job was implementing Canada, my second was helping with US geography boundaries.
- Global expansion: in Stripe's *Indexing the AI economy*, the median top-100 AI company sold into 55 foreign countries in year one and 79 in year two (SaaS: 25 and 42). Small, selected sample — present it as the direction, not a benchmark.
- Work without jobs (Jesuthasan and Boudreau), adapted for tax as a hybrid model:
  - Functional homes: filing and tax engine maintenance, which never finish.
  - Missions: e.g., a market cluster launch. A mission needs a measurable outcome, a senior sponsor, allocated capacity, and an end point with a named receiver.
- Borrowed specialists fail because the filing deadline wins. Mission hires arrive after the launch. You need generalists with range, who are flexible and willing to learn.
- The boundary keeps moving: a market that was a mission last year is a filing obligation this year.

---

## Slide 11 — Key takeaways

**On slide**
- Don't chase 100% accuracy — decide which risks you accept
- Unlearn the instincts that make you a good adviser but a bad builder
- Plan before you build: scope, timeline, constraints and total cost
- Get the data model right early
- AI is a transformation tool, not an end state
- Measure outcomes, not activity
- Your job is to solve problems, and problems will keep changing, so your job will too

---

## Open points

- Confirm with Stripe what is cleared for external talks.
- Verify Canadian "reverse charge" wording with a Canadian specialist.
