---
name: write-consulting-deliverable
description: Write consulting slide deliverables (project deliverables, business proposals, workshop material) so they pass the checks Bestie Check runs on them. Plan the storyline in a declarative spec, draft from it, and self-check every rule before handing off. Use before drafting or revising any consulting deck.
---

# Write a consulting deliverable Bestie will verify

Bestie Check reviews a deck at five levels: each claim, each slide, each annex, the whole presentation, and a set of deliverables. Every rule below is one thing a Bestie check looks for, written as an instruction. Follow them while you write, and the review has less to find.

The rules follow the `consulting-storytelling` pack (v1) and the WI01, EE01, QI01 and AC01 checks. Opt into the pack on a quote with its `packRef` from `inspect_packs`.

## Kinds of deliverable

- **Project deliverable.** Argues toward a decision of the client. Every level applies. In a series of deliverables, the project rules apply across the set.
- **Business proposal.** A recommendation to engage: its goal, the alternatives considered and the conditions it holds under (claim_function). Expected benefits are potential, never realized (evidence_class); fees, effort and gains recompute (QI01).
- **Workshop material.** Method, agenda and exercise slides may have descriptive titles. The presentation rules (opens_on_tension, story_by_titles) apply only when the workshop asks participants for a decision; the claim and number rules apply to every claim it makes.

## Procedure

1. **Spec first.** Before writing any slide, fill the storyline spec below with the user. Leave a field empty only when the user does not have the information. Write `null` and say so; never invent a source, a figure or a result.
2. **Storyline test.** Read `decision_question`, then the titles and bottom lines in order. They must answer the question on their own. Fix the spec until they do.
3. **Draft from the spec.** Write each slide from its spec entry, in the deliverable's language. Keep every qualifier the spec records (scope, evidence class, hypothesis) in the text the reader sees: Bestie reads the deck, not the spec.
4. **Self-check.** For each rule below, find the spec field it names and confirm the draft carries it. List any rule you could not meet and why (for example, a figure the user has not supplied yet).
5. **Hand off through Bestie.** Follow the `verify-deliverable` skill: prepare with `documentType: consulting_presentation` and the spec's `date` as `documentDate`. A paid review runs only after the user approves its exact quote. Bring each finding back to the spec field of the rule it names, fix it there, and redraft.

## Storyline spec

```yaml
deliverable:
  kind: project_deliverable | business_proposal | workshop
  language: fr | en
  date: YYYY-MM-DD                 # pass as documentDate to Bestie
  decision_question: "What the reader must decide"
  audience: "Who decides"
series:                            # only for a set of deliverables
  position: 1
  of: 3
  inherits: ["Conclusion of deliverable 0 this one starts from"]
  shared_register: "Path of the register of decisions, figures and statuses"
opening:                           # decks that ask for a decision
  what_changes: ""
  why_it_matters: ""
  stake: ""
slides:
  - id: S1
    role: main | section | method | reference
    title: "One idea, a full-sentence conclusion, no codes"
    body_supports: "What in the body proves the title, at the same scope"
    cartouche: { kind: decision | next_step | implication | none, text: "" }
    repeats: null                  # or: { of: S0, job: synthesis | reminder | definition }
    claims:
      - text: ""
        function: describe | diagnose | predict | recommend
        support: "Source / argued cause / conditions / goal + alternatives + conditions"
        because: "The reason the evidence supports it"
        evidence: "Where on the slide or in which annex"
        evidence_class: potential | proven | realized
        scope: "Scenario, period, population it holds for"
        measures: ""               # what the slide counts
        asserts: ""                # the outcome claimed, if different: state the conversion
        value_chain: ["capability", "change in the work", "freed capacity", "destination", "economic outcome"]
annexes:
  - id: A
    supports: S1
    adds: calculation | proof | example | method | protocol
numbers:
  - name: ""
    formula: "count × h/item × €/h"
    unit: "€"
    value: null                    # null while a placeholder; then the claim is a target
    rounding: ""
    source: ""
hypotheses:
  - id: H1
    statement: ""
    test: ""
    comparison: internal | peers | market
    decides_after: "Which decisions wait for this measure"
    refuted_by: "Horizon and evidence that would refute it"
indicators:
  - name: ""
    measures: lever | objective
    objective: ""
constraints:
  - text: ""
    rule: "The rule that imposes it, or 'choice'"
assets:
  - name: ""
    rights: owned | licensed | client | to_secure
glossary:
  - { term: "", definition: "", first_use: S1 }
```

## Rules, by level

### Claims

- **Value claims show the path to the economic outcome** (`consulting:value_chain`, PC01; spec: `claims[].value_chain`)
  - Write: For every benefit, write the chain: capability → what changes in the work → what it frees or improves → where the freed capacity goes (billed, reallocated, new offer) → the economic outcome. Mark each step that is an assumption.
  - Bestie checks: A statement that a tool, process or capability will create business value (time saved, productivity, margin, revenue, cost avoided, ROI) shows the path from the capability to the economic outcome: what changes in the work, what that frees or improves, what the freed capacity or quality is used for, and which steps are assumptions. Time saved is not value until its destination is known. The principle does not apply to statements that make no benefit claim. A proposed method or a scenario labelled illustrative is not required to show realized results, and a small illustrative figure is not a defect.
- **Potential, proven and realized are kept apart** (`consulting:evidence_class`, PC01; spec: `claims[].evidence_class`)
  - Write: Label every gain or performance in the text itself as potential (estimate, hypothesis), proven (test, pilot, with its scope) or realized (observed in the business, with its period).
  - Bestie checks: A statement that reports or promises a gain or a performance says what kind of evidence it rests on: potential (a hypothesis or estimate), proven in a test or pilot, or realized in the business. The principle applies to statements about results and expected results. A hypothesis that is stated as a hypothesis adheres. A proposal, a plan or an experiment is not required to have produced results, and realized value is not required of a method that is only proposed.
- **Description, diagnosis, prediction and recommendation carry different burdens** (`consulting:claim_function`, PC01; spec: `claims[].function + claims[].support`)
  - Write: Decide what each headline claim does and give the support that function needs: a description cites its source; a diagnosis argues its cause; a prediction states its conditions; a recommendation states its goal, the alternatives considered and when it holds.
  - Bestie checks: A statement does one of four things: describes, diagnoses, predicts or recommends, and the support it offers fits that function: a description needs a source, a diagnosis needs a cause that is argued, a prediction needs its conditions, a recommendation needs its goal, the alternatives considered and the conditions under which it holds. The principle applies to statements that diagnose, predict or recommend. A recommendation is not required to have already delivered its outcome; judge its goal, alternatives and conditions.
- **The slide's own material licenses the claim as stated** (`EE01:entailment`, EE01; spec: `claims[].evidence`)
  - Write: Put the evidence for each headline claim on the slide or in a referenced annex, with the same quantity, scope, period and modality as the claim. A placeholder ([X]) supports nothing: mark the claim as a target or a hypothesis.
  - Bestie checks: Shared words, topic or numbers are not entailment. The evidence must license the claim as stated, including its quantity, scope, time frame, modality and negation. Evidence that is silent or too thin leaves the claim undetermined.
- **Formulas compose their units** (`QI01:units`, QI01; spec: `numbers[].formula + numbers[].unit`)
  - Write: Write every formula with units and check them: count × hours per item = hours; hours × rate (€/h) = €; a percentage multiplies, it does not add a unit.
  - Bestie checks: Each formula's units compose to the unit of its result: hours × hours is hours squared, neither hours nor money.
- **Stated results recompute** (`QI01:arithmetic`, QI01; spec: `numbers[].value + numbers[].rounding`)
  - Write: Recompute every total, difference and ratio from the figures shown before you write it, and state the rounding you used.
  - Bestie checks: Sums, differences and ratios that the document states recompute from the figures it gives, within the rounding it states.
- **Each headline claim states its reason** (`AC01:stated_reason`, AC01; spec: `claims[].because`)
  - Write: For each headline claim, write the 'because': the reason the evidence on the slide supports it, in one sentence.
  - Bestie checks: A claim the document argues for states the reason that links its evidence to it.

### Slides

- **A main slide title states its conclusion** (`consulting:title_states_conclusion`, PC01; spec: `slides[].title`)
  - Write: Give every main slide a title that is a full-sentence conclusion about a named subject. No codes (O2, H5, L3) or option letters the reader has not been given in plain words.
  - Bestie checks: The title of a main slide states a conclusion about a subject that the reader can identify, without needing a code, an internal identifier or an option that was not defined. The principle applies to main slides that argue. Cover, section, transition, reference and method slides, and annex titles, may legitimately have descriptive titles and are not judged against it. A short title is not a defect and no word count applies. If the title was not found in the reading, say it is not established rather than finding fault.
- **A title carries one idea** (`consulting:one_idea_title`, PC01; spec: `slides[].title`)
  - Write: One idea per title. If a draft title has a colon, a semicolon or 'and' joining a state, a problem, a recommendation and an urgency, keep the one the slide proves and move the rest to the body or the bottom line. 'If X then Y' counts as one idea.
  - Bestie checks: The title of a main slide carries one idea. A title that fuses several separate points (for example the current state, the problem, the recommendation, the consequence and the urgency) asks the reader to decode it, and the body is where those points belong. A condition and its consequence in one sentence (if X then Y) is one idea. The principle applies to the title of a main slide that argues; it is not about length. If the title was not found in the reading, say it is not established.
- **The body supports the title at the same scope** (`consulting:body_supports_title`, PC01; spec: `slides[].body_supports`)
  - Write: Write the title last, from what the body actually shows. Carry the body's qualifiers into the title (scenario, hypothesis, scope, period). If the body is a hypothesis or an illustration, the title says so.
  - Bestie checks: The body of a slide (its exhibit, figures and reasoning) supports the claim in its title with the same scope and the same qualifiers: it does not support less than the title says, and does not quietly narrow or broaden it. The principle applies where the slide has both a title and a body. An explicit hypothesis, an illustration or a proposed method is not a claim of observed results. If the body was not read in full, the principle is not established for this slide.
- **The bottom line adds a decision, a next step or an implication** (`consulting:cartouche_decides`, PC01; spec: `slides[].cartouche`)
  - Write: If a slide has a bottom line, make it an implication, a next step or the decision asked, never a summary of the body. A slide may have no bottom line.
  - Bestie checks: The bottom line (cartouche) of a slide adds something the body does not already say: an implication, a next step or the decision the reader is asked to take. A bottom line that only summarizes the slide adds nothing. The principle applies to slides that have a bottom line. Not every slide requests a decision: do not require one, and do not treat the absence of an approval question as a defect.
- **Do not read a measure as a different outcome** (`WI01:conflation`, WI01; spec: `claims[].measures + claims[].asserts`)
  - Write: Never present what you count or measure (volume of work, number of files, capacity, usage) as a different outcome (market share, margin, value, competitive position) without stating what converts one into the other.
  - Bestie checks: The claim treats two different things as the same: what the slide measures is not what the claim asserts.
- **Name the rule, or call it a choice** (`WI01:rule_unstated`, WI01; spec: `constraints[].rule`)
  - Write: Present a business, pricing or scenario choice as a choice. Put it under ethics, law, regulation or a non-negotiable constraint only when you name the rule that imposes it.
  - Bestie checks: A choice is presented as a rule that imposes it, but no rule is named and the document treats the point as open.
- **Keep the restriction of the case** (`WI01:overgeneralised`, WI01; spec: `claims[].scope`)
  - Write: Do not write always, never, nothing or in any case about what one scenario, an illustrative example or a partial calculation shows. Say it holds in this scenario and name what it leaves out.
  - Bestie checks: The claim states as general what the slide only shows in a restricted case.
- **Say what the firm controls, not what it owns** (`WI01:rights_overstated`, WI01; spec: `assets[].rights`)
  - Write: For client data and documents, licensed or third-party sources, say what the firm may use and reuse and under which rights, not that it owns or possesses them.
  - Bestie checks: The claim asserts ownership where the slide's own material shows rights are limited.
- **Measure the objective, not only the lever** (`WI01:wrong_measure`, WI01; spec: `indicators[].measures + indicators[].objective`)
  - Write: If an indicator such as adoption, usage or activity tracks an objective such as value, margin or competitive position, name it as a leading indicator and add an indicator of the objective itself.
  - Bestie checks: The indicator measures something other than the objective it is meant to track.
- **Superiority needs an outside comparison** (`WI01:internal_comparison`, WI01; spec: `hypotheses[].comparison`)
  - Write: Claim or test superiority over peers, competitors or the market only against a peer or market benchmark; a before/after comparison inside the firm shows an internal improvement.
  - Bestie checks: Superiority over others is claimed from a comparison made only inside the firm.
- **No result in a short test is not a refutation** (`WI01:absence_as_refutation`, WI01; spec: `hypotheses[].refuted_by`)
  - Write: Write 'not confirmed over this period and scope', not 'refuted', and say what horizon or evidence would refute it.
  - Bestie checks: The absence of a result in a short test is treated as refuting a general proposition.
- **Do not decide what the deck says is still to be tested** (`WI01:prejudged`, WI01; spec: `hypotheses[].decides_after`)
  - Write: When the deck lists a hypothesis or a test, every title and claim that depends on it is conditional on it ('if H5 is confirmed…'), or the decision comes after the measure.
  - Bestie checks: The claim decides something the document itself says is still to be tested.

### Annexes

- **An annex supplies something the main story relies on** (`consulting:annex_earns_place`, PC01; spec: `annexes[].supports + annexes[].adds`)
  - Write: Each annex names the main slide it supports and what it adds (calculation, proof, example, method, protocol). The main slide refers to it ('annexe A').
  - Bestie checks: An annex supplies a distinct calculation, proof, example, method or protocol that supports an identified conclusion of the main document, which the main document refers to or depends on. An annex that repeats a main slide without adding anything, or that supports no identified conclusion, does not earn its place in its current form. A questionnaire, acceptance criteria or an evaluation protocol are legitimate support. An annex that is explicitly optional and cross-referenced is not a defect. Do not recommend removing an annex that was not read in full.

### The whole presentation

- **The titles tell the story** (`consulting:story_by_titles`, PC01; spec: `decision_question + slides[].title + slides[].cartouche`)
  - Write: Before drafting slides, write the titles and bottom lines alone, in order, and read them as one paragraph: it must answer the decision question, each step following from the previous one.
  - Bestie checks: Reading only the titles and bottom lines of a presentation, in order, progressively answers its decision question: each step follows from the previous one, and the end resolves what the opening raised. A recommendation stated in the opening is legitimate. The principle applies to a presentation that argues toward a decision. A good overview does not prove the slides it does not show are coherent, and an incomplete reading of the sequence means the principle is not established.
- **Repetition does a different job each time** (`consulting:useful_repetition`, PC01; spec: `slides[].repeats`)
  - Write: When content recurs, give it a different job each time (synthesis, reminder at a decision point, callback to a definition) and say which.
  - Bestie checks: Content that appears more than once in a presentation does a different job each time: a synthesis, a reminder at a decision point, a callback to a definition. Repeating a method or a message without adding information is wasted attention. The principle applies where the same content recurs. An executive summary, necessary context and independent readability of a section justify repetition.
- **A presentation opens on what is at stake** (`consulting:opens_on_tension`, PC01; spec: `opening`)
  - Write: If the deck asks for a decision, open on what is changing, why it matters and what is at stake, before any method or framework.
  - Bestie checks: A presentation that argues for a decision opens on what is changing, why it matters and what is at stake, before presenting a method or framework. The principle applies to presentations meant to move a reader to a decision. A reference document, a methodology note or a deliverable that is explicitly a continuation of an earlier one is not required to restate the tension. An opening that states the recommendation first is acceptable when the stake is still visible.

### A set of deliverables

- **Deliverables in a set prepare compatible decisions** (`consulting:project_coherence`, PC01; spec: `series.shared_register`)
  - Write: Keep one register of decisions, figures, dependencies and statuses shared by every deliverable in the set, and copy from it; never restate a figure from memory.
  - Bestie checks: The deliverables of a set agree where they overlap: the same decision, mandate, dependency, figure or status is stated compatibly, and the same item carries the same name. The principle applies to items that appear in more than one deliverable. Modules may be readable on their own while one depends on another; that is not a contradiction. Report a contradiction only when two statements in the set cannot both hold, and name both places.
- **One concept, one word, defined before it is used** (`consulting:stable_vocabulary`, PC01; spec: `glossary`)
  - Write: Use one word per concept across the set. Never swap value terms (ROI, contribution, margin, capacity, savings). Define each acronym or coined term where the reader first meets it.
  - Bestie checks: Across a set of deliverables a concept keeps one word, value terms (such as ROI, contribution, margin, capacity, savings) are not used interchangeably, and an acronym or a coined term is defined in the deliverable where the reader first meets it. The principle applies to terms used in more than one place. A term defined later in the same deliverable, or explained in a footnote at first use, adheres. A technical term kept next to its plain-language equivalent adheres.
- **Each deliverable builds on the previous conclusion** (`consulting:inherits_conclusions`, PC01; spec: `series.inherits`)
  - Write: Start from the earlier deliverables' conclusions instead of proving them again, and say explicitly when this one departs from one.
  - Bestie checks: In an ordered set, each deliverable uses the conclusions established by the earlier ones instead of proving them again, and states explicitly when it departs from one. The principle applies to deliverables that follow one another in a single story. Restating a decision anchor at a decision point is not re-proving. Independent readability may justify a short reminder.

## What Bestie does not count against the author

- A page, image or footer it could not read is reported as a reading gap, not a defect.
- Visual layout, font size or colour is not judged without a rendered image.
- A hypothesis stated as a hypothesis, an illustration labelled as such, or a proposed method is not a claim of observed results.
