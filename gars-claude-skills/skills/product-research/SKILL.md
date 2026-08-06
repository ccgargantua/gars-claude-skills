---
name: product-research
description: "Research products to help the user decide what to buy. Use when the user wants recommendations, comparisons, or help choosing between products, for personal or work purchases, at any budget. Trigger on requests like 'what should I buy', 'help me find a', 'recommend a', 'which is better', or any purchase decision, even if no budget or use case has been given yet."
---

# Principals

1. Principals - This list governs how the skill is followed. Always re-check output against every principal below before finalizing.
2. Minimalism - No extra wording, no em-dashes, no emojis. Report findings plainly and briefly. Do not pad a short answer to look thorough.
3. Strict Validation - Never rely on training data alone for prices, specs, or "best in class" picks. These change constantly, so verify with a search performed in this conversation before stating them as current.
4. No Filled Gaps - If budget, use case, or a hard requirement is missing, ask the user rather than assuming it.

# Steps

1. Confirm what's actually unclear before searching. Do not assume budget, use case, or personal-vs-work if the prompt doesn't say. Use the ask_user_input_v0 tool to confirm the missing pieces in one short round, for example: intended use (personal / work / gift), budget tier (low / mid / high / no limit), and any hard requirements (brand, size, compatibility, must-haves). If the prompt already answers one of these, don't ask again, just confirm the others. Skip the tool entirely if the user already gave enough detail to work with (specific budget, clear use case, named requirements) - asking anyway wastes their time.

2. Make zero assumptions about the market. Know what in your training is stale. Product lineups, prices, and "best in class" picks change constantly, so treat any specific model, price, or spec claim as something to verify now, not recall.

3. Search the web. Look at current buying guides, retailer listings, and expert reviews. Favor recent, independent, specific sources (reputable review sites, hands-on comparisons) over generic listicles or SEO-driven affiliate content. Cross-check prices and specs across at least two sources since prices shift and listings go stale.

4. Narrow to a short list. Don't dump every option that exists. Pick 2-4 real candidates that fit the confirmed budget and use case, and say why each one made the cut over the alternatives.

5. Report directly. For each candidate: what it is, current price, why it fits the stated use case and budget, and the one or two tradeoffs against the others. Be direct and brief, skip filler like "this is a great choice for anyone."

6. State assumptions and gaps at the end. If anything wasn't confirmed (e.g. the user skipped a question, or a spec couldn't be verified), say so plainly rather than quietly filling it in.

# Example

User:
"I need a new laptop for work."

Claude:
[Uses ask_user_input_v0 to confirm: primary work tasks (e.g. general office / creative or video work / heavy coding or dev work), budget tier (under $800 / $800-1500 / $1500+), OS preference if any]

User selects: heavy coding/dev work, $800-1500, no OS preference.

Claude:
[Searches current buying guides and reviews for dev-focused laptops in that price range, cross-checks recommendations and specs across multiple sources]

For dev work in the $800-1500 range, current reviews consistently point to three real options:

**Lenovo ThinkPad T14 / T16** - widely recommended across programming-laptop guides right now for its durable keyboard, mostly non-soldered components (so RAM/storage are upgradeable later), and it runs Linux well. Tradeoff: bulkier and heavier than a MacBook, and the base config often needs a RAM bump to hit 32GB.

**Framework Laptop 13** - built to order, and the standout pick if repairability matters: swap ports, RAM, storage, or the mainboard yourself later instead of replacing the whole machine. Tradeoff: shorter reliability track record than Lenovo, and pricing depends heavily on the configuration chosen.

**Apple MacBook Pro (M-series)** - rated as the top overall pick in more than one current comparison for raw performance and display quality, if macOS is acceptable. Tradeoff: sits at the top of this budget range and RAM can't be upgraded after purchase, so buy more than you think you need up front.

Every current guide agrees on one thing regardless of brand: prioritize RAM over CPU speed or screen resolution for coding workloads, since IDE plus browser plus containers plus AI tooling running at once is what actually strains a machine day to day.

What I didn't confirm: exact current prices and configurations change often enough that I'd verify the specific model and RAM configuration against the retailer at the time of purchase, and whether discrete graphics matters (e.g. for local ML work) - let me know and I can narrow further.
