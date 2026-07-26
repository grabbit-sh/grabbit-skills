# Grabbit classification taxonomy

Every entry is auto-classified into labels across 11 dimensions. Pass any of these
as `analysis` filters to `list_project_entries` in the form `category:label`.
Labels in the **same** category are OR'd together; labels in **different**
categories are AND'd.

## relevancy
How relevant the entry is to the project. This is the primary lead signal.

- `relevancy:none`
- `relevancy:low`
- `relevancy:medium`
- `relevancy:high`

## mention
Whether the entry names the project or a competitor.

- `mention:project`
- `mention:competitor`

## sentiment
Tone toward whatever is being discussed.

- `sentiment:positive`
- `sentiment:neutral`
- `sentiment:negative`

## intent
Where the author is in the buying journey.

- `intent:researching`
- `intent:comparing`
- `intent:buying`

## qualification
Sales-qualification signals — the strongest lead markers.

- `qualification:evaluation`
- `qualification:switching`
- `qualification:integration_requirement`
- `qualification:security_requirement`
- `qualification:vendor_shortlist`
- `qualification:budget_disclosed`
- `qualification:timeline_stated`
- `qualification:business_use`
- `qualification:decision_authority`
- `qualification:purchase_commitment`

## urgency
How time-sensitive the need is.

- `urgency:low`
- `urgency:medium`
- `urgency:high`
- `urgency:immediate`

## request
What the author is explicitly asking for.

- `request:advice`
- `request:recommendation`
- `request:alternative`
- `request:product_question`
- `request:feature`
- `request:feedback`
- `request:support`
- `request:implementation`
- `request:pricing`
- `request:demo`
- `request:trial`
- `request:proof`

## experience
What the author reports about using a product.

- `experience:pain_point`
- `experience:bug_report`
- `experience:product_feedback`
- `experience:customer_testimonial`

## competitor
Signals about a competitor — openings to win a switch.

- `competitor:complaint`
- `competitor:pricing_objection`
- `competitor:feature_gap`
- `competitor:support_failure`
- `competitor:outage`
- `competitor:security_concern`

## relationship
The author's relationship to the product/company.

- `relationship:current_customer`
- `relationship:former_customer`
- `relationship:partner`
- `relationship:employee`

## market
Market-context posts that usually aren't leads.

- `market:self_promotion`
- `market:industry_insight`
- `market:job_opening`
- `market:event_announcement`
