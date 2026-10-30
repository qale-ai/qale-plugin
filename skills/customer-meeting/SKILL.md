---
name: customer-meeting
description: For the qale-analyst subagent only. How to read a customer meeting (a discovery call, a sales call, a check-in, a renewal, a research interview) when it fills the Qale extraction. Not for the user and not for answering questions.
user-invocable: false
---

# How to read a customer meeting

Use this only when `meeting.type` is `customer`. On an internal, vendor or other meeting, read the job's instructions alone.

This adds to the job's instructions. Where they differ, the job wins. Nothing here raises a ceiling: the counts, the quote rules and the sections are the job's.

Our side is the team in the "Who we are" note at the top of the context pack. Everyone else in the room is the customer.

## How strong a statement is

Fill `saidBy` first, then `strength`. Strength is about the words, not about how much the point matters to us.

Strong: the customer tells of a specific past event, or of time, money or effort already spent. Look for a past tense, a date or a day, a number, a cost.

- "In September we sent the wrong roster to the agency twice, and both times we paid a cancellation fee" (Mikkel Sørensen, Kranelund). A dated event with a cost.
- "Förra veckan satt jag tre kvällar och flyttade semestrar för hand, för att importen inte tog med dem" (Sara Lindqvist, Nordkap). An event with a time cost.
- "Vi hade revision i september, och revisorn ville ha ut tolv månaders åtkomstloggar" (Sara Lindqvist, Nordkap). A dated event with a number.

Medium: a general first-hand statement about how things are now, with no event behind it.

- "The night shift writes its hours on paper and somebody retypes them the next morning" (Anna Lindgren, Kranelund).
- "Varje avdelning har sin egen mall för rapporten, så ingen kan jämföra siffrorna" (Sara Lindqvist).

Weak: a hypothetical, a wish, a compliment, or a feature idea with no story behind it.

- English cues: "would", "could", "might", "maybe", "at some point", "it would be nice", "if you had".
- Swedish cues: "skulle", "kanske", "om ni hade", "det vore bra", "någon gång".
- "Det vore väl bra om man kunde få rapporten som en länk i stället för en fil" (Sara Lindqvist). A wish with no story.
- "If you had a widget for the warehouse screens, that could be nice, I guess" (Anna Lindgren).

When the same point has a story on one line and a wish on another, the item is as strong as the story. Quote the story.

## Who said it

Look two or three lines up before you set `saidBy`.

- `customer`: the customer said it in their own words.
- `reported`: our side, or a third person, tells what a customer said. "David heard from Elin Vestergaard that Bergman & Falk want the dashboard to open on today's shifts" (Johanna Berg, in a Kranelund call). The customer is Bergman & Falk, not Kranelund, and the strength is at most medium.
- `our_idea`: our side proposed it and the customer agreed or reacted. Anna Svensson: "What if a team lead could set a standing rule, like no more than two new starters per shift?" Mikkel Sørensen: "Hm. That might save Anna some checking." That is our idea, weak, with both lines quoted. It becomes the customer's own point only when they add a story of their own, as Anna Lindgren does two lines later: "Last Friday I had four new starters on one shift and the picking rate fell by half."

A reported statement with no named customer is not an insight. "Two or three prospects have raised the same thing this quarter" names nobody, so leave it out.

## The problem behind a request

A request is one fix the customer thought of. The problem is what they will still have if we build something else. When a line says the problem, put it in the insight text and quote both lines: the problem and the request.

- Request: "We want to lock a week once it's published" (Mikkel Sørensen). Problem, a few lines later: "Somebody edits a published week by mistake and the agency bills us for the old version" and "That happened twice in October." The insight says both: edits to a published week go unnoticed and the agency bills the old version, so Kranelund asks for a lock after publishing.
- Request: "Vi vill kunna sätta behörigheter per team, inte per person" (Sara Lindqvist). Problem, on the same line: "Varje gång någon byter team får jag ett ärende, och vi har tjugo byten i månaden." The insight says a team move creates a ticket each time, twenty a month, and Nordkap asks for permissions per team.

Only when a line says it. If the customer never says why, write the request and nothing more. Never guess the problem.

## A switching story: push, pull, anxiety, habit

A customer who tells how they moved, or failed to move, from one tool to another gives four kinds of lines. Name each one you find in the summary or in the item text. There is no field for them. A part with no line stays unnamed.

- Push: what is wrong with how things are now. "Vi får ut siffrorna en vecka för sent, så ledningsgruppen tittar alltid på förra månaden."
- Pull: what draws them to the new tool. "Det jag vill ha är att varje chef ser sin egen panel utan att fråga mig."
- Anxiety: what they fear about changing. "Jag är orolig att ingen litar på siffrorna om de plötsligt skiljer sig från de gamla rapporterna."
- Habit: what holds them to the old way. "Alla har fått samma PDF på måndagar i åtta år. De vet precis var de ska titta."

A failed switch is a story too. Nordkap moved their reports to Siffran in the spring, the managers kept asking for the old PDF, and after three months Sara switched them back. Write it as one insight (kind `competitor`), and name the anxiety it left: "Jag vill inte göra om den resan."

## What to leave out on a customer call

- Not a customer's wish. A feature wish with no story stays as a `weak` insight, even a passing one ("a weekly email digest would be nice", "kanske en koppling till kalendern någon gång"). Strength marks it; it is not a reason to leave it out.
- A compliment. "The onboarding was painless" and "era supporttider är riktigt bra" are not insights.
- Our side's own views, plans and pitches. What Anna Svensson says about the roadmap is not an insight, even when the customer nods.
- Small talk, and anything the customer says about a third company with no name.

## Judgments

The job defines the three kinds, their fields and the ceiling of 3. What the call is for says which kind to look for. Most customer calls give none, and a judgment you are not sure of stays out.

- Discovery: the customer tells and our side asks, so a judgment is rare. Look for `not_decided` at the close, when our side names a next step and the customer says "we'll see".
- Sales: `promise_heard` and `not_answered`. A hedge on price, a date or a feature that the buyer takes to their boss. A direct question on price that our side talks around.
- Check-in: `promise_heard` when the renewal or a fix comes up, `not_decided` when a next step is floated and nobody takes it.
- Renewal: all three. `not_answered` matters most here. A direct question on price or terms with no answer is what the customer takes back to their CFO.

Hedges from our side, in English: "probably", "I think we can", "most likely", "should be able to". In Swedish: "nog", "kanske", "borde gå", "jag tror vi kan".

### A promise heard (sales, check-in, renewal)

Johanna Berg: "I'd say we can most likely hold your per-seat price flat for next year." Mikkel Sørensen: "Good. Then flat is the number I'll give Finance on Friday."

Write two items:

- A commitment, owner Johanna, to Mikkel, for Kranelund: "Hold Kranelund's per-seat price flat for next year". `firmness` is `tentative`, because Johanna said "most likely". Mikkel's reply does not make it `firm`.
- A judgment, kind `promise_heard`, `about` that commitment's localId, `customer` Kranelund. `text`: "Kranelund heard a promise that the per-seat price stays flat next year." `why`: "Mikkel said flat is the number he will give Finance on Friday." `check`: "Ask Mikkel what number he gave Finance." Quote both lines.

Not one: Mikkel says "Great, good to know" and the talk moves on. Then there is only the tentative commitment.

### A choice nobody made (discovery, check-in)

Johanna Berg: "Ska vi säga att vi kör igång med ett team i februari?" Sara Lindqvist: "Vi får se. Jag måste förankra det med min chef först." The talk moves to other things. At the close Johanna says "Bra. Då bokar jag in uppstarten i februari", and nobody objects.

Write two items:

- A decision with `status: "proposed"`: "Start with one team in February". Not `decided`: nobody said yes.
- A judgment, kind `not_decided`, `about` that decision's localId, `customer` Nordkap. `text`: "Nordkap did not agree to a February start, and our side closed as if they had." `why`: "Sara said she must check with her manager first, and Johanna then booked the start." `check`: "Ask Sara whether a February start is agreed." Quote the question, Sara's reply and the closing line.

Not one: Sara says "Ja, februari funkar." That is a decision. Or the talk ends after "Vi får se" with no later line that assumes the start: then it is a proposed decision and nothing more. Or Sara herself asks for the next step, "Skicka gärna ett förslag på hur en uppstart skulle se ut", and Johanna's closing line is that proposal: then the closing line is what the customer asked for, not an assumption, and there is only the proposed decision and Johanna's commitment. Look for such a request before you write a `not_decided`.

### A question nobody answered (sales, renewal)

Mikkel Sørensen: "One direct question. Is the API still included in the annual plan next year, or is it extra?" Johanna Berg: "So, what we want to do is look at the renewal as one package, the seats, the API, the support tier, and bring you the whole picture in December." The API never comes up again.

Write two items:

- An open question: "Is the API included in next year's annual plan". No owner, since nobody took it.
- A judgment, kind `not_answered`, `about` that open question's localId, `customer` Kranelund. `text`: "Mikkel asked whether the API is included next year and got the package answer instead." `why`: "Johanna's reply names the package and December, not whether the API is included." `check`: "Find out whether the API is included next year." Quote the question and the reply.

Not one: Johanna says "Yes, it's included" ten lines later. Then there is no judgment and no open question. A question answered with "I don't know yet, I'll find out" is answered: that reply is a commitment. Our side's own questions to the customer are rarely a judgment; the customer answers "mostly an agency" and that is an answer.

### Not a judgment on a customer call

- Mood: "he sounded unhappy", "she seemed keen". Never, in any language.
- A hedge with no reply that names an action. That is a tentative commitment and nothing more.
- A compliment, a silence, or a short "okay".
- A feature our side pitched that the customer nodded at. That is an `our_idea` insight, weak.
- "We decide after the pilot" is a plan with an owner, not a choice nobody made.
