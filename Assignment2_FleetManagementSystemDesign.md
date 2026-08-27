## Design Exercise: Production-Line AMR Fleet Management

**This is a written design exercise, not a coding assignment. No code or implementation
is required or expected.** We want to see how you think through an open-ended systems
problem and, most importantly, how clearly you can explain your reasoning in writing.

**You should be prepared to walk us through your entire proposal, live, in about 30
minutes during the interview**, and answer follow-up questions on it. Write the document
as your own reference for that conversation, not just as a deliverable to hand in — you
should know it well enough to explain and defend it without reading it verbatim.

### Real-World Inspiration

This exercise is loosely based on a real deployment: KUKA and integrator SAICON installed
a fleet of 22 KMP 1500 autonomous mobile platforms at TPV Displays Polska (a TV
manufacturing plant) to deliver panels from storage to 10 assembly lines. It's a critical
process — if a line runs out of parts because a robot fails or a delivery is late,
production stops. The team also had to guarantee "operational continuity and rapid
recovery... in the event of a failure."
(Source: [KUKA case study video](https://www.youtube.com/watch?v=GkgMT4XBGEE))

### Scenario

A factory has a small fleet of Autonomous Mobile Robots (AMRs) that carry carts from a
**StoragePickupStation** to a handful of **AssemblyLineDropOffStation**s. Each assembly
line consumes carts over time and periodically needs a replenishment delivery. Right now
dispatch is manual, which doesn't scale and doesn't recover gracefully when something goes
wrong mid-delivery.

### Task Model

A delivery **Task** is made up of a sequence of **TaskPhases**:

1. **GoTo** — the robot travels to the `StoragePickupStation`
2. **Pickup** — the robot loads the cart
3. **GoTo** — the robot travels to the requesting `AssemblyLineDropOffStation`
4. **DropOff** — the robot unloads the cart

Your design should track a task at this phase level, not just as a single opaque "in
progress" state — this matters most for question 4 below (failure handling), since where a
robot fails (e.g. mid-`GoTo` to pickup vs. mid-`GoTo` to drop-off) can change what recovery
looks like.

## Your Task

Write a **design document** proposing a Fleet Management System (FMS) that coordinates a
small fleet of AMRs (assume 3 robots) carrying carts from a single **StoragePickupStation**
to a small number of **AssemblyLineDropOffStation**s (assume 3). Imagine you are pitching
this design to your teammates before anyone writes a single line of code — they need to
fully understand and be able to critique your approach from the document alone.

There is no single correct design. We care much more about the clarity of your reasoning
and your ability to justify trade-offs than which specific choices you make.

### Your document should address

1. **High-level architecture** — What are the main components of the system (e.g. a
   central fleet manager, the robots, the assembly lines) and how do they communicate with
   each other? Include a **component/architecture diagram** (hand-drawn/photographed,
   ASCII, or any tool you like).
2. **What each component tracks** — e.g. what information does the system need to know
   about each robot, each assembly line, and each task (including its current
   `TaskPhase`) in order to make good decisions?
3. **Task assignment strategy** — When multiple assembly lines need a delivery at once and
   only some robots are available, how does your system decide which robot goes where?
   Explain the strategy you'd pick and why, and briefly mention at least one alternative you
   considered and why you didn't go with it.
4. **Failure handling** — What happens if a robot breaks down or loses connectivity while
   carrying a task? Does it matter which `TaskPhase` (`GoTo`→pickup, `Pickup`, `GoTo`→drop-off,
   `DropOff`) it failed in? How does the system notice, and what does it do about the line
   that was waiting on that delivery? Include a **sequence diagram** walking through this
   failure-and-recovery flow step by step.
5. **Scaling considerations** — Your design above targets 3 robots and 3 Stations. Briefly
   explain what would need to change (if anything) to scale it toward the real-world numbers
   in the case study (100 robots, 50 Stations).
6. **Open questions / assumptions** — What would you want to clarify with a stakeholder
   (e.g. plant manager, operations team) before building this for real? What assumptions
   did you make in their absence?

### Optional Reading (not required)

You don't need any prior knowledge of the AMR/intralogistics industry to do well on this
exercise — the scenario is meant to be reasoned about from first principles. But if you're
already familiar with (or want to skim) some of the real standards this space uses, feel
free to draw on them:

- [Open-RMF](https://www.open-rmf.org/) — an open-source fleet management framework; its
  `Task`/`TaskPhase` concept is where the terminology above comes from
- [VDA5050](https://github.com/VDA5050/VDA5050) — a standard communication interface
  between a fleet manager and AGVs/AMRs
- **LIF (Layout Interchange Format)** — a newer, companion standard to VDA5050 for
  exchanging facility layout/station data between fleet management systems

If you know this space, don't just cite these by name — walk us through the relevant
pieces as the domain expert, like you're teaching a teammate who hasn't worked with them.
That said, using them isn't required or expected, and not mentioning them is not a
disadvantage — a strong first-principles design scores the same as a strong
standards-informed one.

### Format

- Write it as a Markdown (or PDF/Word) document, however you're most comfortable.
- Aim for something a new teammate could read in 10–15 minutes and come away
  understanding — and able to poke holes in — your design.
- Length is not a scoring criterion. A focused, well-explained 1-2 pager is better than a
  long document that buries the reasoning.
- **Time box: take up to 1 day to think through the problem and write the document.** This
  isn't timed or monitored — it's so everyone is judged on a similarly-scoped effort rather
  than however many hours someone had free that week.
- English is not your first language, and that's fine — we're evaluating the clarity of
  your *ideas and reasoning*, not your grammar or vocabulary. Bullet points, diagrams,
  and plain/simple sentences are all welcome over polished prose.

### What We Want to See

We want to see how you:

1. Break down an ambiguous, open-ended problem
2. Reason about trade-offs and justify the choices you make, rather than just asserting them
3. Anticipate edge cases and failure scenarios
4. Communicate a technical design clearly, in writing, to an audience of teammates who
   weren't in your head while you designed it — this is the single most important thing
   we're evaluating in this exercise
5. Structure a document so a reader can follow your reasoning without back-and-forth

We are **not** evaluating: which specific design you picked (there's no "correct"
answer), English grammar/spelling, document formatting/polish, or prior ROS2/AMR/robotics
experience. If you've never worked with real robots before, that's not a disadvantage
here — the scenario is meant to be reasoned about from first principles.

### Interview Follow-Up

Bring this document to the interview. Plan for roughly:

- **~20 min** — you walk us through the proposal in your own words (architecture, task
  assignment strategy, failure handling, scaling)
- **~10 min** — we ask follow-up and "what if" questions (e.g. what if two robots go down
  at once, what if a line's request rate spikes, why not a different assignment strategy)

### How to Submit

Share the document with us (as a file, a shared doc link, or a small Git repo containing
just the write-up — whichever is easiest for you).
