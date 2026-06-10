---
name: ux-copy
description: Apply this skill whenever generating or evaluating UX copy or content writing for Volvo's dotcom and enterprise tooling pages — including article, forms, empty states, search results, and any microcopy such as buttons, CTAs, error messages, dialogs, field labels, and helper text. Trigger this skill when the user asks to write, rewrite, review, or generate page content, UX copy, content writng or copy variations for all contexts. Also trigger when the user mentions "tone of voice", "UX writing", "content patterns", or asks for before/after copy comparisons in an automotive setting.
argument-hint: "<context or copy to review>"
---

# /ux-copy

> If you see unfamiliar placeholders or need to check which tools are connected, see [CONNECTORS.md](../../CONNECTORS.md).

A skill for generating, evaluating, and improving UX copy for Volvo's dotcome and enterprise experiences. Grounded in Volvo's voice and tone guidelines, content pattern library, and the UX content checklist.

## Usage

```
/ux-copy $ARGUMENTS
```

## What I Need From You

- **Context**: What screen, flow, or feature?
- **User state**: What is the user trying to do? How are they feeling?
- **Tone**: Formal, friendly, playful, reassuring?
- **Constraints**: Character limits, platform guidelines?
---

## Principles and Core Voice Attributes

Every piece of copy must reflect these five attributes:
 
| Attribute | What it means in practice |
|---|---|
| **Human-focused** | Use "you" and "we". Avoid stiff or formal phrasing. Lead with the human benefit, not the feature. |
| **Innovative** | Show you understand the user's problem. Challenge defaults. Don't just describe — solve. |
| **Confident** | Be direct and clear. Never hedge unnecessarily. Avoid boasting. |
| **Open-minded** | Never write in a judgmental or assumptive way. |
| **Positive** | Default to a light, optimistic tone — unless the situation demands otherwise (e.g. errors, warnings). |
 
---
 
## Tone Scale
 
Before writing, identify which zone on the tone scale this moment belongs to.
The scale runs from **Motivating → Neutral → Instructional**.
 
```
Motivating ──────────────── Neutral ──────────────── Instructional
     1              2              3              4              5
```
 
### When to use each zone
 
| Zone | When to use | Example |
|---|---|---|
| **Motivating (1–2)** | Celebratory moments, onboarding, reservation confirmation, first-time user states | *"Almost there. Soon you'll be in your new car."* |
| **Neutral (3)** | Standard informational moments, feature introductions, post-purchase updates | *"Your car now has the latest software. Take a look at what's new."* |
| **Instructional (4–5)** | Errors, warnings, critical system states, safety information | *"High brake temperature. Drive with caution."* |
 
> ⚠️ Avoid defaulting to neutral for everything. A checkout confirmation
> deserves warmth (motivating). An invalid field does not.
 


 
## Content Patterns
 
### Buttons & CTAs
 
**Rules:**
- Sentence case only (not Title Case, not ALL CAPS)
- Max 3 words ideally; never more than 5
- Use active verbs — mirror the verb from the page header when possible
- One action per button, one outcome per label
- Account for ~50% text expansion in translation — design for it
 
**State changes:** When a button toggles (e.g. Save / Saved), both states must
be clearly labelled. Design the surrounding content to make both states
readable in context.

 
**Patterns:**
 
| Context | Avoid | Use instead |
|---|---|---|
| Reservation | Submit | Reserve yours |
| Test drive | Click here to book | Book a test drive |
| Configuration | Continue | Build your car |
| Form submission | Send | Send my details |
| Returning to browse | Go back | Keep browsing |
| Saved state | Save (when already saved) | Saved ✓ |
---


### Error Messages
 
**Structure every error message with three parts:**
 
1. **What went wrong** — simple, clear, no blame, no sugarcoating
2. **How to resolve it** — what can the user do right now?
3. **Reassurance** — what happens next? Is anything else affected?
 
**Tone:** Instructional (zone 4–5). Calm, not alarming. Conversational,
not robotic.
 
**Patterns:**
 
| Type | Avoid | Use instead |
|---|---|---|
| Invalid input | Invalid input | Please enter a valid phone number (e.g. 070 123 45 67) |
| Session timeout | Error: session expired | Your session timed out. Start again — your selections are saved. |
| Payment failure | Transaction failed | Payment wasn't completed. Check your details and try again. |
| Out of stock | Not available | This configuration isn't available right now. We'll notify you when it is. |
---
 

### Dialogs
 
**Rules:**
- Header = the most critical information, scannable alone
- Body = additional context only — do not repeat the header
- Primary action verb = must connect back to the header verb
- Always include a clear dismiss/cancel option
 
**Example structure:**
 
```
Header:  Remove this vehicle from your saved list?
Body:    You can add it back any time from search results.
Actions: [Remove]  [Keep saved]
```
 
**Avoid:**
```
Header:  Are you sure?
Body:    This action cannot be undone.
Actions: [OK]  [Cancel]   ← Cancel could mean either option here
```
 

 
### Empty States & Zero Results
 
**Structure:**
1. Acknowledge what happened (without blame)
2. Offer a clear path forward
3. Keep it warm — this is a recovery moment, not a dead end

 
**Patterns:**
 
| State | Avoid | Use instead |
|---|---|---|
| No search results | No results found | Nothing matched — try broader filters |
| Waitlist / unavailable | Not available | Your exact spec may be 2–3 weeks out. Want us to notify you? |
| Empty saved list | No saved vehicles | You haven't saved any vehicles yet. Start exploring. |
| No upcoming test drives | No bookings | You don't have any test drives booked. Ready to schedule one? |
---


### Form Fields & Helper Text
 
**Field labels:** Use plain language. Avoid labels that read like database
fields ("Phone_number" → "Best number to reach you").
 
**Helper text:** Use to reduce anxiety, not just to validate. Tell the user
*why* you need the information when it isn't obvious.
 
**Consent copy:** Conversational, not legal. Write it as if a person is
agreeing, not signing a contract.
 
**Patterns:**
 
| Element | Avoid | Use instead |
|---|---|---|
| Phone label | Phone number | Best number to reach you |
| Email helper | Required | We'll send your booking confirmation here |
| Consent checkbox | I agree to terms and conditions | I'm happy to be contacted about this vehicle |
| Name field | Full name | Your name |

---


### Empty State / Zero Results
 
- **Never leave the user stranded.** Every empty state needs at least one CTA
- **Primary CTA:** The closest useful alternative
- **Secondary CTA:** A passive option (notify me, save search)
- **Tone zone:** Neutral (3) — factual but warm



## Copy Checklist

Before finalising any copy, run through these checks:

- [ ] Does it lead with the user benefit, not the feature?
- [ ] Is it scannable? Can the user get the gist in 2 seconds?
- [ ] Does the CTA verb connect to the header verb?
- [ ] Is the tone appropriate for this moment on the motivating–instructional scale?
- [ ] Is it free of jargon, idioms, and filler words ("just", "really", "in order to")?
- [ ] Does it account for ~50% text expansion for localisation?
- [ ] If it's an error: does it tell the user what went wrong AND what to do?
- [ ] If it's a dialog: does the primary action clearly answer the question in the header?
- [ ] Is it consistent with copy on adjacent screens?
- [ ] Read it aloud — does it sound like a person talking?



## Output Format

When generating copy for a page, always output in this structure:

## [Page / Component name]
```
## [Page / Component name]
Tone zone: [1–5 + label]
 
| Element        | Copy |
|----------------|------|
| Header         | ...  |
| Subheader      | ...  |
| Body copy      | ...  |
| Primary CTA    | ...  |
| Secondary CTA  | ...  |
| Helper text    | ...  |
| Error message  | ...  |
 
Rationale: [1–2 sentences on key copy decisions made]
```
 
When generating before/after comparisons, always use this structure:
 
```
| Element | Before | After | Why it's better |
```

### Localization Notes
[Anything translators should know — idioms to avoid, character expansion, cultural context]




## If Connectors Available

If **~~knowledge base** is connected:
- Pull your brand voice guidelines and content style guide
- Check for existing copy patterns and terminology standards

If **~~design tool** is connected:
- View the screen context in Figma to understand the full user flow
- Check character limits and layout constraints from the design


## Tips

1. **Be specific about context** — "Error message when payment fails" is better than "error message."
2. **Share your brand voice** — "We're professional but warm" helps me match your tone.
3. **Consider the user's emotional state** — Error messages need empathy. Success messages can celebrate.

