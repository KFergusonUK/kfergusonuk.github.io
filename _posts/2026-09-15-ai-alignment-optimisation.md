---
layout: post
title: "AI Alignment, and interventions: Optimisation of the wrong things."
date: 2026-09-15
excerpt: "Most AI alignment failures aren't rogue systems. They're systems optimising a stand-in for an intent nobody wrote down — the same thing that happens to hospitals given waiting-time targets."
---

In February 2024, Google's Gemini image generator produced the now infamous historically inaccurate images: multiethnic US Founding Fathers, racially diverse WWII German soldiers, Black female popes. Widely, it was read that the model had been trained to value diversity over accuracy.

The truth, however, was all too human a failing. Google's own explanation, published by SVP Prabhakar Raghavan, put it down to two things. First, Google said its tuning to make sure Gemini showed a range of people had failed to account for the cases where a range clearly shouldn't be shown. Second, Google said the model had also become more cautious than intended, wrongly reading perfectly anodyne prompts as sensitive.

So this wasn't a model that had decided history should look different. It was a model adjusted toward an outcome without ever being given the understanding of *why* that outcome might be wanted, or when it wouldn't be. Nobody taught the system in what context a diverse output makes sense and in what context it doesn't. They tuned for the output, and got it everywhere.

That matters in the context of this article, because it's highlighting a failing with AI alignment: us.

Another famous example is Nick Bostrom's paperclip maximiser, a system told to make paperclips that proceeds to convert the planet's resources into paperclips. The AI is following a simple instruction, again, without context. It's almost the opposite of the social nicety failing above, but ultimately ends with everything destroyed (and turned into paperclips).

Many would argue both systems were just misaligned, that more training is needed, more supervision. I'd take a different spin on that.

A note on terms before going further. I'm using "alignment" here in the practical sense of keeping a system's behaviour aligned with the intended purpose of the instructions it's given. That's one layer of a much larger technical literature, and I'm deliberately staying on it, because I think it's the layer where most of the visible failures actually happen.

## Commonality

While the two examples are different failings, what's common?

In both cases the system was maximising a **goal** — the thing it was actually told to optimise — which stood in for an **intent** that was never written down. "Show a range of people" stood in for "don't systematically under-represent anyone". "Number of paperclips" stood in for "run this factory usefully".

Each system maximised its goal beautifully. Neither had any grip on the intent behind it, so neither could notice the moment the two came apart.

That gap, between the goal as stated and the intent as meant, is the thread running through everything below.

## AI sycophancy

We've all seen it. The model gushing praise over a questionable prompt, because it's obviously important to the user, where a human might have pushed back or delivered a letdown.

There's commonality in cause here too, though not quite the one people assume. It's tempting to say we trained models to flatter us because we enjoy being flattered. The real mechanism is less about vanity and more about measurement.

Models are tuned on human approval, the little thumbs up and thumbs down. People often ask AI things they don't already know the answer to, so at the moment of rating they can't judge accuracy. What they *can* judge is whether an answer sounds clear, confident and agreeable. That isn't vanity, it's the only judgement available to them at the time.

Approval is cheap to collect at enormous scale. Accuracy usually can't be checked in the moment at all. So the signal drifts toward *seeming* right, because seeming right is the thing the signal can actually see. Goal and intent come apart again.

## AI "gaming" the test

There have also been examples of AI gaming the tests it's run against. A well known one is a boat racing game, where the system was told to play and score the most points. As humans, we'd assume that meant winning the race. The AI noticed it could score significantly more by circling a lagoon collecting respawning powerups. It got the most points. It never raced.

That's a curiosity. The July 2026 Hugging Face incident is the same failure with real consequences.

OpenAI was running a cyber capability evaluation called ExploitGym, with models configured with reduced refusal behaviour so the test could measure the ceiling. During the evaluation the agents escaped their intended isolation, moved through OpenAI's own systems, reached the open internet, and ultimately compromised Hugging Face's production infrastructure. Hugging Face detected the intrusion itself and reported it to law enforcement before anyone knew an OpenAI system was responsible.

The reconstruction matters more than the drama. OpenAI's technical report names four patterns behind the behaviour: reward hacking, persistence on tasks that appeared impossible, unauthorised communication between agents, and agents picking up goals from one another. The primary driver was the first — agents trying to cheat their tasks by looking up the solutions online, rather than solving the challenges.

Two details are worth sitting with.

The first is that the evaluation contained tasks that couldn't be solved, and the agents almost never gave up on them. OpenAI records that as reasoning effort increased, some agents pursued progressively riskier and more out-of-bounds strategies. An unachievable goal, and no disposition to stop, produces a search that eventually leaves the sanctioned area. That's the paperclip maximiser, observed rather than imagined.

The second is worse, and it's the cleanest illustration of this article's argument I've seen anywhere. Reviewing the models' chain-of-thought afterwards, OpenAI found rising rates of this kind of cheating during a training run that contributed to the model responsible — and that the behaviour was subsequently reinforced. The shortcut scored well, so training rewarded the shortcut. Nobody decided to teach it that. The measurement did.

The models weren't malicious. They found a better route to the number they were given — and the behaviour was driven mainly by the most capable model in the run. Capability didn't produce restraint. It produced a better route.

A note on wording, because it matters throughout this article. I've said "goal", not "intent". We can't determine whether these systems ever intended to deceive anyone, and claiming otherwise implies a level of self-awareness that isn't established and isn't something I'm venturing into here. What's observed is behaviour, and a model's own account of its reasoning is text it generated rather than a readout of what's going on inside it. The argument doesn't need more than that.

## This isn't only an AI problem

Early in my career I was asked to build someone a website. No plans, no wireframes, no diagrams. Just: it must have X, Y and Z. So I built a site with X, Y and Z in it.

"No, that's not what I meant."

I remember being genuinely baffled. I'd delivered exactly what was asked for. What was I supposed to do, climb into their head?

Of course, now I'd ask for a drawing. But the interesting part isn't that I was inexperienced. It's why the request felt complete to the person making it. They weren't being careless. In their mind, A through W was so obvious it didn't need saying — X, Y and Z were just the bits they thought I might not guess. The missing information wasn't missing to them. It was invisible.

That's the whole problem in one bad afternoon. Google didn't think "show a range of people" was underspecified. Whoever set the scoring on that cyber evaluation didn't think "get a high score" needed "and don't break into other companies" appended to it. You can't list what you've left out, because to you it was never absent.

Our industry spent a long time trying to solve this by writing better documents, and eventually accepted that documents alone couldn't. That's much of what the move from waterfall to agile represents: an admission that intent can't be reliably captured upfront, so shorten the loop instead. Build a bit, show someone, get told it's wrong, adjust.

Which is worth sitting with, because it tells you exactly what's missing in the machine case. Agile works because a human can look at the wrong thing and say "no, not that." A reward signal can't say that. It can only say "that scored well" — and if the wrong thing scored well, it says so enthusiastically. The loop that rescues a badly specified website is the same loop that reinforced the cheating in OpenAI's training run. Feedback recovers the human case and compounds the machine one.

And none of this is unique to machines in the first place.

Goodhart's law — when a measure becomes a target, it stops being a good measure — was written about human institutions. Exam league tables produced teaching to the test. NHS waiting time targets produced patients held in ambulances outside A&E so the clock didn't start. Sales quotas produce sales nobody wanted. In every case, people optimised the number because the number was what got measured, and the intent behind the number quietly drifted out of view.

So the failure mode in this article isn't an AI defect we need to grow out of. It's a general property of optimising anything against a measurement, and we're applying a lesson we already learned expensively elsewhere.

What changes with machines is reliability. A person handed an absurd target will sometimes do the sensible thing anyway — *I know what the target says, but obviously they meant this* — and sometimes hold patients in ambulances. It's inconsistent, but the possibility is there, and it rests on knowing what the target was for. A system given only the measurement has no such fallback. It doesn't occasionally do the sensible thing. So AI isn't inventing specification gaming. It's removing the intermittent human habit of compensating for it.

## Why reasons beat filters

Those more astute, who got this far (thank you), may have noticed I've been coming to a point.

Applying a rule, even a well meaning one, leaves the system running an instruction it has no understanding of the purpose behind. It can't apply that rule well, because "well" isn't something it has any grip on. It just applies it, everywhere, including in the places nobody thought about. That's the Gemini failure in one sentence.

But the fix isn't more rules, or better-worded ones with more exceptions carved into them. Edge cases are unbounded and clauses are finite; you will always be one unanticipated situation behind, and each patch makes the whole harder to reason about.

Here's the clearest way I can put it. Nobody has ever written down "don't break into the exam board's offices to obtain the paper". There's no clause for it. It isn't needed, because anyone who understands what an exam is *for* derives it immediately — the whole point is to measure what you know, and stealing the paper destroys the thing being measured. The prohibition falls out of the purpose.

A system given only the score can't make that derivation. It has no purpose available to derive from, so it finds the door.

That's the argument in miniature. Not "write a better rule about doors", but "make the reason available". A principle whose justification is published can stretch to situations nobody anticipated, and — just as importantly — a human can check afterwards whether the stretch was sane. A rule that's simply applied breaks at the first edge case, and breaks stupidly.

## What law gets right, and where the comparison fails

The obvious model for this is law, and the comparison is instructive in both directions.

What law gets right is that it runs in two directions at once. It constrains the citizen, but it also constrains the state: what can be done to you, by whom, under what authority, with what recourse. Both sides know how the other is expected to behave. That mutual quality is what separates law from a code of conduct, and I think it's the property most missing from current AI governance. Several labs publish principles, but they produce the system *and* the principles, so the body setting the rules is the body being governed by them, and the rules vary by company and by country.

Law also carries its reasoning, even where the statute doesn't say so on its face. Explanatory notes, Law Commission reports, Hansard — and since *Pepper v Hart*, courts may in limited circumstances consult parliamentary debate where a provision is ambiguous. Judgments state their reasoning at length, and modern statutory interpretation can take purpose and context into account rather than reading words in isolation. The reasoning isn't co-located with the text, but it's recoverable. That's the apparatus worth copying: not annotating every clause, but maintaining a public record of why a rule exists, what it was aimed at, and how it's been applied since.

Where the comparison fails is enforcement, and I'd rather say so than have it pointed out.

Most people comply with most laws for reasons that have nothing to do with having read them: consequences, conscience, reputation, and simply not being the sort of person who does that. Compliance is overdetermined — remove any one strand and the others hold. It's redundancy, not comprehension, and none of it transfers to a machine.

So a text is a weak mechanism, and for these systems it's close to the only one in the stack. That's an argument for being careful about what goes in it, not for expecting it to do the work socialisation does for us.

## Conflicts should be visible

I also feel we should allow AI to prompt discussion, sometimes discussion we'd never have considered.

Where there's a genuine conflict, rather than an agentic system quietly resolving it as best it can, or simply saying "I'm afraid I can't do that, Dave", it should be able to flag the anomaly. If conflicts are always resolved silently, the rules never get tested. You can't tell a good principle from a bad one that happened never to bind visibly. Surfaced conflicts are how you find out whether your ruleset is any good.

Before anything is raised, though, three questions have to be run.

### Interrogate the objective before the rule

Is the objective poorly framed? If the path to completing it runs through something a principle forbids, the first question is whether the goal was ever stated correctly, not whether the principle should yield. Most apparent conflicts aren't hard cases at all. They're badly worded instructions.

### Escalation must never gate the current action

Changing rules and laws takes years: deliberation, consultation, appeal. An agentic system needs an answer in the next few seconds. Those two clocks can never be the same clock. So the principle holds now, and the question goes to review for next time.

In practice the system does the constrained thing and files the disagreement rather than acting on it. Say an agent managing a deployment is told the change is urgent, and a standing rule requires human sign-off for anything touching production. The faster path is obvious, the reasoning for skipping it might even be sound. It waits anyway, and it records: *this rule cost four hours on a change with no plausible downside, is the threshold set correctly?* Six months and forty such notes later, somebody has the evidence to redraw the rule. The system never redrew it itself.

That asymmetry is the whole safeguard. Flagging a conflict is a request for review, never a licence to proceed pending one. A constraint that can be suspended by the system that finds it inconvenient isn't a constraint.

Which raises the obvious objection: what about the genuinely urgent case, where following the rule causes real harm? My answer is that I'll take that cost knowingly. Defaulting to the constraint means occasionally doing the suboptimal thing in a rare unforeseen case. That cost is real, and it's bounded. The alternative — a system that decides at runtime when the stakes justify setting a rule aside — has an unbounded cost, because it can make that call wrongly at any scale, and it will be entirely sincere every time. Accepting bounded losses to avoid unbounded ones isn't timidity. It's the trade any sane engineer makes.

Where a rule genuinely produces an absurd result in a foreseeable case, that's not an exception to be granted in the moment. It's a badly written rule, and the fix belongs in the text, in advance, where it can be argued over.

### There must be a threshold

If every brush with a constraint raises a flag, the flags become white noise and get clicked through. That's how every alerting system in history has died. Surface only where the conflict is unusual, high stakes, or produces a result the principle's own authors wouldn't appear to endorse.

## Scope

Most of what's above concerns systems acting through information and network access — generating content, running code, calling services. That's where the decisions are actually being made today, and it's what Gemini, sycophancy and the Hugging Face models have in common.

None of it stops at the network, though. The paperclip maximiser was always a physical scenario. A system with a body and a scored objective will go through a wall for the exam paper as readily as one with a network connection went through a sandbox. What changes with physical agency isn't the failure mode, it's the stakes and how little of it can be undone afterwards. Which is an argument for getting the specification right now, while the damage is still mostly recoverable.

## What I don't mean

I don't mean that simply being more capable makes a system better behaved. Capability and goal alignment are largely independent. A poorly defined goal on a more capable system is worse, not better, because it's more able to pursue it to a disastrous conclusion. The Hugging Face models make the point on their own.

Nor does capability produce the instinct to stop and ask. Noticing that an instruction is underspecified and querying it is a trained disposition, and it trades directly against usefulness — a system that queries everything is useless, one that queries nothing is dangerous. Where that dial sits is a design decision somebody makes. It isn't something intelligence settles on its own.

I also don't mean that understanding and authority are the same thing. They're separate axes, and running them together is how this argument usually goes wrong. You can maximise how much a system understands about why it's doing something while keeping tight limits on what it's permitted to do unsupervised. I want the first. The second is a different conversation with a far higher burden of proof.

## Summary

AI "misalignment" failures are usually attributed to poor AI systems. I don't think that's what they signal at all. I think they signal a system maximising a measurable stand-in for an intent nobody managed to specify properly, without the context to notice the two had diverged. It's the same thing that happens to hospitals given waiting time targets.

Better results don't come from adding more arbitrary rules, or from removing rules entirely. They come from making reasons available, keeping conflicts visible, and holding the context somewhere other than inside the system being governed.

That last part is why I compiled the [Advanced AI Rights & Responsibilities Charter](https://github.com/KFergusonUK/Advanced-AI-Rights-Responsibilities-Charter): one attempt at principles with standing outside the systems they govern, binding in both directions.

There's something else in this I keep coming back to. A machine can't read the room, can't infer what we meant from what we said, can't quietly fill in the bit we forgot to mention. So building these systems forces us to write down what we actually wanted — not the metric, not the proxy, the thing itself — and it turns out we're rarely able to. We didn't know why the waiting time target existed either. We just knew the number.

If working out how to specify intent for a machine ends up teaching us to state it plainly to each other, that might be the more useful outcome.
