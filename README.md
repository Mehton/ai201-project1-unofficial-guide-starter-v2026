# The Unofficial Guide

Bhupinder jit Mehton; I picked city_guides corppus

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

This project is a small document-based question answering system. I chose the city_guides corpus, which contains practical travel and local-guide information about places in the region. When someone asks a question like “how do I get to Kestrelford?”, the software searches the corpus for the most relevant chunks, ranks them by similarity to the question, and then uses the best matches to generate a helpful answer based on the actual guide text. In other words, it does not just guess from general knowledge; it looks through the saved city guide documents and answers using those sources.

## Chunking Strategy

**Chunk size: 300-500**
**Overlap: 20% to 30%**

<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

     I chose my chunk size and overlap based on the structure of the documents in this corpus. The advice posts are short and self-contained, while the city guides are longer and organized into sections, so a single chunk size would either split important facts apart or merge unrelated material. I used a moderate chunk size so each chunk could hold one clear idea without losing context between nearby paragraphs. After testing, I adjusted the overlap because some answers were split across section boundaries, and the overlap helped keep related information together.

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: thread_bike_commute.txt#0 `— produced by: chunker.py::fallback_split`

```
THREAD: Is a bike worth it for a 20 minute walk commute?

--- reply 1 (14 votes) ---
Yeah. Cuts an 18 minute walk to about 6. The thing nobody mentions is storage — covered bike parking exists at three buildings and is full by 9am at all three.

--- reply 2 (9 votes) ---
Counterpoint, I sold mine. Between November and March the paths are either icy or salted and salt destroys a drivetrain in one season.

--- reply 3 (22 votes) ---
Both true. I keep a cheap bike for September to November and walk the rest of the year. Total cost was about $120for the bike and I don't care what happens to it.

--- reply 4 (5 votes) ---
If you do get one, the campus does free registration and it's the only reason I got mine back after it was taken.

For each one, ask: could someone answer a question using only this,
without reading what came before or after?

```

**Chunk 2** — source: thread_meal_plan_tier.txt#0 `— produced by: chunker.py::fallback_split`

```
THREAD: Which meal plan tier is right?

--- reply 1 (24 votes) ---
Depends entirely on whether your building has a kitchen. Fenwick has kitchenettes, so people there go down a tierand cook two or three nights. Everywhere else, get the middle tier.

--- reply 2 (19 votes) ---
The highest tier only makes sense if you eat three meals a day in the halls every single day, which basically nobody does past October.

--- reply 3 (11 votes) ---
Remember you can only change it once and only in the first ten days. I waited and got stuck on a plan I didn't use.

--- reply 4 (7 votes) ---
Declining balance rolls within the semester but not between them. Spend it in December or lose it.


```

**Chunk 3** — source: thread_parking.txt#0 `— produced by:chunker.py::fallback_split`

```
THREAD: Worth getting a parking permit?

--- reply 1 (15 votes) ---
West lots sell out in about three days in August. East lot never sells out but it's a 12 minute walk, at which point you might as well have parked on the street.

--- reply 2 (21 votes) ---
Street parking on Verrill is legal and free and unmarked, which is why half the upper years do it.

--- reply 3 (8 votes) ---
If you're commuting daily, the west permit is worth the August scramble. Otherwise don't bother.

```

**Chunk 4** — source: thread_printing.txt#0 `— produced by:chunker.py::fallback_split`

```
THREAD: Is the printing quota enough?

--- reply 1 (17 votes) ---
For most people yes. $30 is about 600 pages black and white. It's the colour printing that eats it — eight times the cost per page.

--- reply 2 (11 votes) ---
Doesn't roll over between semesters. Print your readings in December rather than losing it.


```

**Chunk 5** — source: thread_roommate_conflict.txt#0 `— produced by: chunker.py::fallback_split`

```
THREAD: Roommate situation isn't working. What now?

--- reply 1 (28 votes) ---
Talk to your RA early, and frame it as 'we need help sorting this out' rather than 'move me'. Room changes are possible but the process starts with mediation and skipping that step slows it down.

--- reply 2 (14 votes) ---
Room changes happen at the semester boundary almost always, and mid-semester only in fairly serious cases.

--- reply 3 (33 votes) ---
Write down specifics before the meeting. 'It's not working' is hard to act on; 'guests four nights a week past 2am' is not.

```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question: What is the bus schedule between Brightwater and Kestrelford?**

\*\*Answer:
(best distance 0.274, cutoff 0.6)

Based on the documents, buses run from Brightwater to Kestrelford roughly hourly on weekdays, every two hours on Saturdays, and do not run on Sundays.

Sources:

- `guide_kestrelford.md`
- `guide_regional_transport.md`

Sources retrieved: guide_accessibility.md, guide_brightwater.md, guide_kestrelford.md, guide_regional_transport.md

1 model calls this session, 1191 tokens (1132 in, 59 out)
\*\*

```
(best distance 0.331, cutoff 0.6)

Based on the provided documents, the Kestrelford bus service runs hourly on weekdays and two-hourly on other times (though Sunday service is minimal to non-existent outside the Brightwater town routes).

Source: `guide_regional_transport.md`

Sources retrieved: guide_marchwood.md, guide_regional_transport.md, guide_seasons.md, guide_walking.md

1 model calls this session, 448 tokens (395 in, 53 out)

```

**My relevance cutoff: 0.6**

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question                                                     | In corpus? | Best distance |
| ------------------------------------------------------------ | ---------- | ------------- |
| What is the bus schedule between Brightwater and Kestrelford | city_guide | 0.274         |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1. I asked AI to explain me the prompts in simple words.**

**2. Write me the command for all these steps. I will run them myself**

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.


     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion                              | Target | Run 1  | Run 2  | Run 3  | Verdict |
| -------------------------------------- | ------ | ------ | ------ | ------ | ------- |
| 1. Retrieved chunk contains the answer | 4 of 5 | 4 of 5 | 4 of 5 | 4 of 5 | MET     |
| 2. Every answer names a source         | 5 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | Met     |
| 3. Gate stops out-of-corpus questions  | 4 of 5 | 5 of 5 | 5 of 5 | 5 of 5 | Met     |
| 4.                                     |        |        |        |        |         |
| 5.                                     |        |        |        |        |         |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

### How do I get to Kestrelford? — run 1

- Best distance: 0.3516 (passed the gate)
- Sources retrieved: guide_accessibility.md, guide_kestrelford.md, guide_regional_transport.md, guide_walking.md

```
To get to Kestrelford, you can drive, as it is located an hour inland from Brightwater (guide_kestrelford.md). The final eight minutes of the approach road are single-track with passing places (guide_regional_transport.md). However, the approach road is not gritted above the second village and can become impassable in snow during winters (guide_walking.md).
```

### How do I get to Kestrelford? — run 2

- Best distance: 0.3516 (passed the gate)
- Sources retrieved: guide_accessibility.md, guide_kestrelford.md, guide_regional_transport.md, guide_walking.md

```
To get to Kestrelford, you drive an hour inland from Brightwater (guide_kestrelford.md). The final eight minutes of the approach road are single-track with passing places (guide_regional_transport.md). Note that the approach road is not gritted above the second village and becomes impassable in snow (guide_walking.md).
```

### How do I get to Kestrelford? — run 3

- Best distance: 0.3516 (passed the gate)
- Sources retrieved: guide_accessibility.md, guide_kestrelford.md, guide_regional_transport.md, guide_walking.md

```
To get to Kestrelford, you drive an hour inland from Brightwater (`guide_kestrelford.md`). The final eight minutes of the approach road are single-track with passing places (`guide_regional_transport.md`), and the road is not gritted above the second village, making it impassable in snow (`guide_walking.md`).
```

### How long does it take to drive from Brightwater to Kestrelford? — run 1

- Best distance: 0.3169 (passed the gate)
- Sources retrieved: guide_pellew_sands.md, guide_regional_transport.md, guide_seasons.md, guide_walking.md

```
Based on the provided documents, there is no mention of how long it takes to drive from Brightwater to Kestrelford. Therefore, I don't have enough information to answer the question.
```

### How long does it take to drive from Brightwater to Kestrelford? — run 2

- Best distance: 0.3169 (passed the gate)
- Sources retrieved: guide_pellew_sands.md, guide_regional_transport.md, guide_seasons.md, guide_walking.md

```
Based on the provided documents, there is no mention of how long it takes to drive from Brightwater to Kestrelford. Therefore, I do not have enough information to answer your question.
```

### How long does it take to drive from Brightwater to Kestrelford? — run 3

- Best distance: 0.3169 (passed the gate)
- Sources retrieved: guide_pellew_sands.md, guide_regional_transport.md, guide_seasons.md, guide_walking.md

```
Based on the provided documents, there is no mention of how long it takes to drive from Brightwater to Kestrelford. Therefore, I do not have enough information to answer your question.
```

### Is there a railway station in Kestrelford? — run 1

- Best distance: 0.3990 (passed the gate)
- Sources retrieved: guide_accessibility.md, guide_kestrelford.md, guide_regional_transport.md, guide_walking.md

```
Based on the provided documents, there is no mention of whether or not there is a railway station in Kestrelford, though there is a reference to a "Kestrelford trackbed" that follows a "railway line closed in 1963" (`guide_walking.md`).

I do not have enough information to confirm if an active railway station currently exists there.
```

### Is there a railway station in Kestrelford? — run 2

- Best distance: 0.3990 (passed the gate)
- Sources retrieved: guide_accessibility.md, guide_kestrelford.md, guide_regional_transport.md, guide_walking.md

```
Based on the provided documents, there is no mention of an active railway station in Kestrelford, though there is a "Kestrelford trackbed" that follows a railway line closed in 1963 (`guide_walking.md`).
```

### Is there a railway station in Kestrelford? — run 3

- Best distance: 0.3990 (passed the gate)
- Sources retrieved: guide_accessibility.md, guide_kestrelford.md, guide_regional_transport.md, guide_walking.md

```
Based on the provided documents, there is no direct mention of an active railway station in Kestrelford, though the text does mention that the "Kestrelford trackbed follows the railway line closed in 1963" (`guide_walking.md`) and references a "Kestrelford service" that is "hourly on weekdays" (`guide_regional_transport.md`).

Files used: `guide_walking.md`, `guide_regional_transport.md`
```

### What is the bus schedule between Brightwater and Kestrelford? — run 1

- Best distance: 0.3308 (passed the gate)
- Sources retrieved: guide_marchwood.md, guide_regional_transport.md, guide_seasons.md, guide_walking.md

```
Based on the provided documents, the Kestrelford service is hourly on weekdays and two-hourly on other days (though the exact frequency for non-weekdays is implied to be less, with Sunday service being minimal to non-existent outside Brightwater town routes).

Source: `guide_regional_transport.md`
```

### What is the bus schedule between Brightwater and Kestrelford? — run 2

- Best distance: 0.3308 (passed the gate)
- Sources retrieved: guide_marchwood.md, guide_regional_transport.md, guide_seasons.md, guide_walking.md

```
Based on the provided documents, the Kestrelford service runs hourly on weekdays and two-hourly (though Sunday service is minimal to non-existent outside Brightwater town routes).

Source: `guide_regional_transport.md`
```

### What is the bus schedule between Brightwater and Kestrelford? — run 3

- Best distance: 0.3308 (passed the gate)
- Sources retrieved: guide_marchwood.md, guide_regional_transport.md, guide_seasons.md, guide_walking.md

```
Based on the provided documents, the Kestrelford service runs hourly on weekdays and two-hourly (though Sunday service is minimal to non-existent outside Brightwater town routes).

This information came from `guide_regional_transport.md`.
```

### What is the road like on the approach to Kestrelford? — run 1

- Best distance: 0.3149 (passed the gate)
- Sources retrieved: guide_accessibility.md, guide_kestrelford.md, guide_regional_transport.md, guide_walking.md

```
Based on the provided documents, the approach road to Kestrelford is not gritted above the second village and is impassable in snow (*guide_walking.md*). Additionally, the final eight minutes of the approach are single-track with passing places (*guide_regional_transport.md*).
```

### What is the road like on the approach to Kestrelford? — run 2

- Best distance: 0.3149 (passed the gate)
- Sources retrieved: guide_accessibility.md, guide_kestrelford.md, guide_regional_transport.md, guide_walking.md

```
Based on the provided documents:

* The approach road is not gritted above the second village and is impassable in snow, which can cut the town off for a day or two most winters (*guide_walking.md*).
* The final eight minutes of the approach is single-track with passing places, and the approaches overall have poor transport (*guide_regional_transport.md*).
```

### What is the road like on the approach to Kestrelford? — run 3

- Best distance: 0.3149 (passed the gate)
- Sources retrieved: guide_accessibility.md, guide_kestrelford.md, guide_regional_transport.md, guide_walking.md

```
The Kestrelford approach road is single-track with passing places for the final eight minutes (*guide_regional_transport.md*). Additionally, it is not gritted above the second village and becomes impassable in snow, which can cut the town off for a day or two most winters (*guide_walking.md*).
```

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| #   | Criterion | Verdict | How I decided |
| --- | --------- | ------- | ------------- |
| 1   |           |         |               |
| 2   |           |         |               |
| 3   |           |         |               |
| 4   |           |         |               |
| 5   |           |         |               |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion                              | Target | Run 1 | Run 2 | Run 3 | Verdict |
| -------------------------------------- | ------ | ----- | ----- | ----- | ------- |
| 1. Retrieved chunk contains the answer | 4 of 5 |       |       |       |         |
| 2. Every answer names a source         | 5 of 5 |       |       |       |         |
| 3. Gate stops out-of-corpus questions  | 4 of 5 |       |       |       |         |
| 4.                                     |        |       |       |       |         |
| 5.                                     |        |       |       |       |         |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
