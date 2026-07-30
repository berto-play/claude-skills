# Ghost Taxonomy: Job Posts

A ghost is a hiring signal that looks real but isn't. Run this check on every
company that appears in results. Ghosts are not accusations — they are flags
that warrant verification before investing application effort.

Use the inline table in SKILL.md Part D for fast field triage. This file
contains the full detection signal set for each ghost type.

---

## 1. Phantom Role `[PHANTOM]`

**Definition:** Role is posted and looks active but has already been filled
internally, via referral, or through a retained search. Company hasn't removed the post.

**Primary signal:** LinkedIn shows a new hire in that function within the last 30 days.

**Secondary signals:**
- Post is >21 days old with no visible activity or update
- Same role posted and removed multiple times in the past 12 months
- Job post has a "Promoted" tag on LinkedIn — paid to stay visible, not necessarily active
- Glassdoor or Blind has comments like "they already have someone in mind"
- ATS shows the role but the company's careers page has removed it

**Recommended action:** Check LinkedIn People tab filtered to the target function.
If someone joined in the last 60 days with a matching title, skip unless you have
a direct internal contact who can confirm the role is genuinely open again.

---

## 2. Evergreen Role `[EVERGREEN]`

**Definition:** Company posts the same role continuously regardless of actual hiring intent.
Used as a talent pipeline builder. Real hire may happen once a year or never.

**Primary signal:** Same or near-identical job description appears in historical searches
(Wayback Machine, Google cache) from 90+ days ago.

**Secondary signals:**
- Post has been live >60 days without modification
- Multiple identical postings across boards with staggered dates
- Role is generic enough to fit many people ("Senior Designer" with no project or team context)
- No interview activity reported on Glassdoor for this role in the past 6 months
- Company has been "hiring" for this function for multiple years consecutively

**Recommended action:** Light-touch outreach to gauge real intent before investing
in a full application. Applying is low-risk but low-probability. Ask a recruiter
or hiring manager directly: "Is this an active search with a hire target date?"

---

## 3. Approval Ghost `[APPROVAL-GHOST]`

**Definition:** Role was approved in planning but a headcount freeze stopped actual hiring.
Post remains live because no one formally cancelled it in the ATS.

**Primary signal:** Post is stale (>21 days) with no reported interview activity, despite
appearing on a live ATS board.

**Secondary signals:**
- Company announced cost controls, "efficiency focus", or slowed growth in recent earnings/press
- LinkedIn headcount flat or declining while the role stays posted
- Glassdoor reviews mention hiring slowdowns in the target function
- Role has been "posted" but no recruiter has reached out to strong-fit candidates in the market
- Company recently had leadership changes in the hiring function

**Recommended action:** Before applying, try to confirm the role is unfrozen. Look for
a recruiter associated with the role on LinkedIn. If the recruiter profile shows no
recent activity on this search, the approval ghost flag stands.

---

## 4. Pipeline Post `[PIPELINE]`

**Definition:** Company is collecting resumes for a future round, not an immediate hire.
No specific start date or budget confirmed.

**Primary signal:** Job description uses "building our talent pool", "always looking for",
or has a vague or missing start date.

**Secondary signals:**
- Role is posted at a company that recently announced a funding round (not yet closed)
- JD describes a team "we're building" rather than one that exists
- Company is pre-revenue or pre-product-market-fit — headcount plan is aspirational
- Recruiter response is enthusiastic but timeline is described as "a few months out"
- Same role has been in "pipeline" status for 60+ days without offers

**Recommended action:** Flag in the Watch section rather than Apply. Suitable for
a warm introduction now so you're positioned when the role activates. Do not invest
full application effort until the role has a confirmed hire target.

---

## 5. Syndicated Ghost `[SYNDICATED]`

**Definition:** Role was posted once by the employer, then auto-syndicated across multiple
boards. Each board shows a different "posted X days ago" timestamp based on when it was
scraped, not when the original post went live. Creates false freshness signals.

**Primary signal:** Same role title and description appears on 3+ boards with meaningfully
different dates (e.g. LinkedIn says 3 days ago, Indeed says 22 days ago, Glassdoor says 8 days).

**Secondary signals:**
- Job description is identical word-for-word across all boards
- Company careers page shows a date that differs from every board
- Role appears on agency boards (not direct employer) with no attribution to original source
- The "freshest" posting date is from a low-trust aggregator (Indeed, ZipRecruiter)

**Recommended action:** Trust the company careers page date above all boards.
If the careers page date is >30 days, treat as MEDIUM-HIGH staleness regardless
of what the boards show. If no careers page date is available, use the oldest
board date as the conservative estimate.

---

## 6. Zombie Post `[ZOMBIE]`

**Definition:** Role was posted by a company that has since had a hiring freeze, layoff,
or pivot, but the post was never removed from the ATS. Company is not actively hiring
for this role — or possibly for anything.

**Primary signal:** Company announced a layoff, hiring freeze, or restructure in the
last 90 days and the post predates that announcement.

**Secondary signals:**
- LinkedIn headcount has dropped since the post date
- Crunchbase or press shows runway is short, last funding round was 12+ months ago
- No other recent posts from the company; the one you found is isolated
- Glassdoor reviews from recent months mention layoffs or hiring stops
- Company blog or LinkedIn page has gone quiet (no posts in 60+ days)

**Recommended action:** Cross-reference the post date with any news about the company's
financial position. If a layoff announcement postdates the job post, treat as zombie.
Flag as Skip in triage. Do not apply without verifying the company is still operational
and the role is genuinely open.

---

## 7. Stealth Fill `[STEALTH]`

**Definition:** Role is being filled via retained recruiter or direct LinkedIn outreach
while the public post stays live. Company is actively interviewing but through a
hidden channel — the public post is a formality or backup.

**Primary signal:** A recruiter is actively reaching out to candidates for this role
on LinkedIn while the public post remains live and open.

**Secondary signals:**
- The role has been posted for >14 days with no update, but a recruiter at the company
  recently connected with multiple candidates matching the profile
- Company LinkedIn page shows the hiring manager "actively recruiting" badge
- The role JD was posted early in a search that is now in late-stage interviews
- A contact in the market has received outreach for this exact role but it's still "open"

**Recommended action:** A stealth fill doesn't mean you can't get the role — it means
the primary channel is a recruiter. Direct outreach to the recruiter or hiring manager
is more effective than applying through the ATS. If you apply through ATS, flag your
application with a direct message to the recruiter on LinkedIn.

---

## Cross-Ghost Detection: When Multiple Flags Apply

Some roles show signals from multiple ghost types simultaneously.

**Syndicated + Phantom:** Same stale listing appearing on multiple boards with fresh dates,
while LinkedIn shows the role was recently filled. Skip.

**Evergreen + Pipeline:** Long-running post at a company that's been "building the team"
for 12 months. Watch rather than apply; treat as relationship-building.

**Zombie + Approval-Ghost:** Post predates a hiring freeze AND the role was approved but
never had a confirmed hire date. Very high staleness risk — Skip unless you have insider
confirmation the role survived the freeze.

When 2+ ghost types flag simultaneously, move the role to Skip in triage unless the
operator has specific inside information contradicting the flags.
