# Merchant Mind — Product Requirements Document

## Status

**Draft product definition after discovery.**

This document consolidates the current product thesis. It is intentionally a PRD, not a technical specification. Architecture, implementation choices, model selection, storage, retrieval, evaluation harnesses, and deployment details remain open unless explicitly constrained here.

## 1. Product summary

Merchant Mind is a buyer-first merchant intelligence layer for AI-mediated commerce.

It helps a merchant use both public commerce data and private merchant knowledge to recommend, compose, configure, adapt, and explain the best solution the merchant can actually provide to a buyer.

The buyer may be:

- a human interacting with an ecommerce experience;
- an external buyer agent;
- a salesperson or customer-service operator acting on behalf of a customer;
- another system that needs merchant-side product or solution intelligence.

Merchant Mind should remain channel-neutral. Human interfaces and agent protocols are different surfaces over the same merchant-side decision capability.

## 2. Background and problem

AI shopping and agentic commerce are making catalog discovery, product feeds, checkout capabilities, and machine-to-machine commerce increasingly standardized.

Emerging protocols and commerce platforms can already expose products, variants, prices, availability, cart and checkout operations, identity, fulfillment, and other transactional capabilities to AI systems. This is valuable infrastructure, but it risks reducing the merchant to a source of public catalog data and transaction endpoints while an external AI controls more of the discovery and recommendation layer.

That model ignores an important asset: the merchant often possesses knowledge about its merchandise that is not present in the public catalog and should not necessarily be exposed directly.

Examples include:

- return rates and return reasons;
- fit and sizing behavior;
- kept purchases and repeat purchases;
- complaints and customer-service patterns;
- product failures and warranty evidence;
- real compatibility between products;
- actual fulfillment reliability;
- inventory and operational context;
- internal documentation and expert knowledge;
- merchandising knowledge built through experience;
- commercial constraints and objectives.

A buyer agent may know the buyer better. The merchant should be able to know the merchandise better.

Merchant Mind explores how that merchant-side knowledge can remain part of the buying decision without requiring the merchant to disclose confidential data or manipulate the buyer in favor of merchant value.

## 3. Product thesis

> Give every buyer access to the merchant's best product knowledge — whether the buyer is a person or an AI agent.

More precisely:

> Merchant Mind uses public and private merchant knowledge to build the best solution for the buyer, asks only for information that materially changes the decision, and allows merchant objectives to influence the outcome only among alternatives that are materially equivalent for the buyer.

The product is not defined by a chatbot, a protocol, a RAG pipeline, or a recommendation model. Those are possible implementation mechanisms.

The defining capability is **buyer-first merchant-side decision intelligence**.

## 4. Why now

Several changes make the problem newly important:

1. Product feeds and structured catalog exposure are becoming normal infrastructure for AI shopping.
2. Commerce protocols such as UCP increasingly cover capability discovery and standardized commerce operations.
3. MCP and A2A provide increasingly standard ways for AI systems to discover tools or interact with other agents.
4. Ecommerce platforms are beginning to distribute merchant catalogs directly into external AI shopping channels.
5. External AI systems can therefore own a larger part of product discovery and recommendation while the merchant retains responsibility for product truth, fulfillment, transaction, service, and post-purchase outcomes.
6. Merchants already possess first-party evidence that may materially improve recommendation quality but is absent from public product data.

The opportunity is therefore not to invent another feed or protocol. It is to preserve and improve the merchant's intelligent role in the buying decision as commerce becomes AI-mediated.

## 5. Target customer

### Initial customer profile

The strongest initial target is a **specialist ecommerce retailer in the mid-market or upper mid-market**.

Ideal characteristics include:

- a meaningful ecommerce business;
- a non-trivial assortment across multiple products, variants, or brands;
- categories where product choice benefits from specialist knowledge;
- enough transactions to generate useful post-purchase evidence;
- returns, support, inventory, or fulfillment data beyond the public catalog;
- meaningful cost when the wrong product is selected;
- insufficient incentive or scale to build a complete merchant-agent intelligence stack internally.

Small merchants may not possess enough private evidence to create a strong advantage over catalog-only recommendation. Very large enterprise retailers may have the data but also substantial internal AI teams and incumbent enterprise platforms. Neither segment is excluded from the product, but neither is the preferred first validation target.

### Initial business buyer

Likely primary buyer:

- Ecommerce Director / Head of Digital Commerce.

Likely stakeholders:

- Head of Merchandising / Buying;
- Customer Experience / Customer Service;
- ecommerce product leadership;
- technology leadership as technical approver;
- category experts or specialist sales teams where relevant.

## 6. Buyer problem

The buyer often does not know the exact SKU required.

They may know:

- a goal;
- a use case;
- constraints;
- preferences;
- products already owned;
- previous positive or negative experiences;
- a budget or delivery requirement.

The buyer should not have to translate this into the merchant's category taxonomy, filter model, attribute schema, or technical configuration model.

Examples:

- "I need waterproof shoes for walking all day, I have wide feet, budget €150 and I really want to avoid a return."
- "Three days in the Dolomites in September, staying in huts. I already have boots and a backpack, budget €400, and I want to travel light."
- "I need to irrigate this area of my garden and I want the simplest reliable solution."

The merchant should be able to transform that need into a product or solution using knowledge that may be richer than what is publicly exposed.

## 7. Merchant problem

A merchant increasingly needs to serve buyers through channels it does not fully control.

If the merchant exposes only catalog and transaction primitives, important merchant knowledge may disappear from the decision process. The merchant then risks becoming a fulfillment database while external systems perform search, recommendation, comparison, and solution design using incomplete information.

Merchant Mind should let the merchant make its product intelligence available without requiring it to reveal raw confidential data or create custom recommendation logic for every new AI channel.

## 8. Product principles

### 8.1 Buyer first

Buyer utility is the primary optimization objective.

Merchant value may influence the result only when candidate solutions are materially equivalent or near-equivalent for the buyer.

The product must not use a simple weighted formula where enough margin can compensate for a materially worse buyer outcome.

Conceptually:

```text
1. determine admissible solutions
2. identify the best solutions for the buyer
3. establish a buyer-equivalent or near-equivalent set
4. apply merchant objectives only inside that set
5. produce an externally appropriate explanation
```

If product A is meaningfully better for the buyer than product B, higher margin, excess inventory, private-label priority, or campaign pressure must not cause B to win.

If A and B are effectively equivalent for the buyer, the merchant may legitimately prefer B.

Merchant objectives may also create new buyer value. For example, an inventory objective may enable a discount that makes one candidate better for the buyer as well.

### 8.2 Minimum sufficient interaction

Merchant Mind must ask only questions that can materially improve the decision.

Conversation is a mechanism for acquiring missing information, not a goal.

The system should:

1. use known buyer context and merchant knowledge first;
2. determine whether a sufficiently good decision is already possible;
3. if not, ask the single most useful missing question or a very small set of truly necessary questions;
4. re-evaluate after new information arrives;
5. stop asking when additional information is unlikely to change the recommendation materially.

It must not turn an existing product configurator or attribute schema into a long conversational questionnaire.

### 8.3 Source-agnostic knowledge

The product model must not depend on a single data source or ingestion mechanism.

Possible sources include databases, APIs, commerce platforms, analytics warehouses, OMS, CRM, customer-service systems, documents, structured files, RAG sources, expert transcripts, or explicit rules.

Adding or removing a source changes what the system knows, not what Merchant Mind is.

### 8.4 Selective disclosure

Merchant Mind may use information that must remain private.

The system must distinguish between:

- information that may be disclosed directly;
- internal evidence that can support a safe aggregate or derived claim;
- confidential information that may affect an internal decision but must not be exposed.

Confidential evidence may be withheld, but explanations must not be fabricated to conceal the real basis of the decision.

### 8.5 Use standards rather than replacing them

Merchant Mind should use existing commerce and agent standards where they solve the problem adequately.

In particular, UCP, MCP, and A2A are relevant for commerce capability discovery, standardized operations, tool invocation, and agent-to-agent interaction.

The project should not create a proprietary commerce or agent protocol merely to differentiate itself.

### 8.6 Channel neutrality

The merchant intelligence should be reusable across:

- an onsite ecommerce advisor;
- an internal salesperson or customer-service experience;
- external AI systems;
- buyer agents;
- future commerce channels.

Channel-specific UI or protocol adapters should not redefine the underlying decision model.

## 9. Knowledge model

Knowledge should be classified along at least two independent dimensions: who may access it and what role it plays in the decision.

### 9.1 Public or externally shareable knowledge

Examples:

- product attributes;
- variants;
- price and availability;
- product specifications;
- promotions;
- public reviews;
- shipping and returns policies;
- manufacturer documentation.

### 9.2 Merchant-internal evidence

Internal evidence may improve the buyer's outcome without being exposed directly.

Examples:

- orders and kept purchases;
- return rate and reason-coded returns;
- sizing and fit evidence;
- product complaints;
- support contacts;
- warranty and defect signals;
- actual delivery reliability;
- empirical product compatibility;
- observed usage patterns;
- internal documentation;
- category-expert knowledge.

Not all internal evidence is equally reliable. Evidence may be stale, based on a small sample, limited to a market or product revision, or confounded by previous merchandising decisions. The decision layer must therefore be able to treat evidence as contextual rather than absolute truth.

### 9.3 Buyer-provided context

Examples:

- current need and use case;
- hard constraints;
- preferences;
- budget;
- delivery deadline;
- products already owned;
- profile or purchase history explicitly shared or made available with appropriate authorization.

An external buyer agent may disclose only the subset of buyer context necessary for the current task.

### 9.4 Merchant objectives

Examples:

- margin;
- contribution margin;
- aging stock;
- sell-through targets;
- inventory balancing;
- private-label priority;
- vendor incentives;
- campaign objectives;
- fulfillment cost.

These may only act inside the buyer-equivalent or near-equivalent set unless they create additional buyer value.

### 9.5 Merchant hard constraints

Examples:

- eligibility rules;
- inventory reservation constraints;
- legal or regulatory restrictions;
- territorial constraints;
- contractual restrictions;
- product compatibility requirements;
- minimum-price or promotion constraints;
- fraud or operational policies.

These constrain the solution space rather than act as optimization objectives.

## 10. Product capabilities

The complete product vision includes the following capabilities. They may be implemented incrementally, but they belong to the product rather than being treated as unrelated future products.

### 10.1 Understand

Interpret buyer goals, constraints, preferences, and context without requiring the buyer to know the merchant's internal schema.

### 10.2 Clarify

Identify missing information that is necessary to distinguish between materially different solutions and ask for it with minimum interaction.

### 10.3 Advise / Recommend

Select the best product or small set of products for the buyer using both public product truth and applicable private merchant evidence.

### 10.4 Compare

Explain meaningful differences and trade-offs between viable candidates in terms relevant to the buyer's stated need.

### 10.5 Compose

Build a coherent multi-product solution when the buyer's need cannot be satisfied by a single SKU.

Composition should consider coverage, compatibility, redundancy, budget, existing buyer-owned products, and relevant private merchant evidence.

### 10.6 Configure

Support categories in which the correct solution must be parameterized or dimensioned from buyer-specific conditions.

The system should gather only configuration inputs that are actually decision-relevant.

### 10.7 Substitute

When an item becomes unavailable or unsuitable, identify a replacement that preserves the reasons the original item was selected rather than relying only on product similarity.

### 10.8 Explain

Provide buyer-facing reasoning that is useful, truthful, and permitted by disclosure policy.

### 10.9 Adapt

Maintain the current buying problem and proposal across turns so that changes in budget, weather, stock, requirements, or preferences cause targeted re-planning rather than a complete restart.

### 10.10 Propose / Incentivize

Where merchant policy permits, the system may construct an offer or apply incentives that improve the proposal.

Merchant-side incentives are particularly desirable when a merchant objective can be converted into genuine buyer value.

### 10.11 Decline to recommend

A buyer-first system must be able to conclude that the merchant does not currently offer a solution it can responsibly recommend.

The product must not force a sale merely because inventory exists.

## 11. Initial user experiences

### 11.1 Onsite ecommerce advisor

The initial human-facing wedge is an advisor embedded in or alongside an ecommerce experience.

The buyer describes a need naturally. Merchant Mind uses available context and merchant knowledge, asks only necessary questions, and returns a recommendation or solution that can lead into normal commerce actions such as product selection, cart, or checkout.

This should feel closer to interacting with a knowledgeable specialist salesperson than to interacting with a product-search chatbot.

### 11.2 External buyer agent

An external buyer agent should eventually be able to discover and invoke the same merchant intelligence through existing machine-readable standards.

The external agent should not require raw access to the merchant's private evidence. It supplies the buyer context required for the task and receives an appropriate recommendation, proposal, explanation, and commerce actions.

### 11.3 Internal assisted selling

The same decision layer may support customer-service or specialist sales staff. This is a channel application of the same intelligence, not a separate core product.

## 12. Initial ecommerce domains

### 12.1 Footwear

Footwear is the first product-selection laboratory because it makes the value of private evidence easy to understand.

Relevant problems include:

- nominal sizing versus observed sizing behavior;
- width and fit;
- return reasons;
- product reliability;
- buyer aversion to returns;
- fulfillment reliability;
- legitimate versus forbidden merchant tie-breaks.

A catalog-only buyer agent may see two near-identical shoes. Merchant Mind may know that one has materially worse fit outcomes for buyers with similar needs.

The project must generalize beyond sizing and should not become a fit-recommendation product.

### 12.2 Outdoor

Outdoor adds multi-product solution composition.

The buyer may provide a trip, environment, budget, equipment already owned, comfort preference, weight preference, and timing constraints.

The system may need to:

- decompose the need;
- avoid redundant purchases;
- preserve compatibility;
- assemble a coherent kit;
- adapt to stock changes;
- replace one item without breaking the solution;
- trade off budget, weight, quality, delivery, and use case.

This domain tests whether Merchant Mind can construct a solution that does not already exist as a catalog entity.

## 13. Extension to technical solution selling

The product should be able to generalize from ecommerce recommendation and composition to higher-consideration technical categories.

Examples include:

- irrigation systems;
- pergolas and shading systems;
- HVAC;
- networking;
- components and accessories;
- tools and machinery;
- other configured or solution-oriented commerce.

The buyer may begin with a problem rather than a product request. Merchant Mind may need measurements, capacity, environmental conditions, compatibility information, or installation constraints before proposing a solution.

This does not change the product model. It increases the importance of clarification, configuration, and minimum sufficient interaction.

## 14. Standards and interoperability

Merchant Mind should treat standardized commerce infrastructure as a dependency or integration surface rather than as a differentiation target.

### UCP

Relevant for merchant capability discovery and standardized commerce operations such as catalog, cart, checkout, fulfillment, orders, identity, and related capabilities.

Where appropriate, Merchant Mind should be discoverable or invokable alongside the merchant's standard UCP capabilities instead of creating a parallel proprietary commerce stack.

### MCP

Relevant as a tool invocation surface for AI clients.

Merchant Mind may expose merchant-intelligence capabilities through MCP where that is appropriate for the client environment.

### A2A

Relevant for agent discovery, advertised skills, and richer agent-to-agent interaction.

A merchant-side agent may expose Merchant Mind capabilities to external buyer agents through A2A while keeping private merchant knowledge server-side.

### Principle

Protocols provide discovery and invocation. Merchant Mind provides decision intelligence.

## 15. Product boundary

Merchant Mind includes:

- merchant-side buyer decision intelligence;
- use of public and private merchant knowledge;
- buyer-first policy and merchant tie-breaking;
- clarification with minimum sufficient interaction;
- recommendation, composition, configuration, substitution, adaptation, explanation, and offer construction;
- selective disclosure;
- reuse across human and agent-facing surfaces;
- standards-compatible exposure of merchant intelligence.

Merchant Mind does **not** need to own:

- the ecommerce catalog system of record;
- the OMS;
- the CRM;
- the analytics warehouse;
- the underlying ecommerce checkout implementation;
- every ingestion and retrieval technology;
- a new agent or commerce transport protocol.

## 16. Non-goals

Merchant Mind is not:

- a generic ecommerce chatbot;
- a customer-support bot with search attached;
- a generic conversational search engine;
- a feed management or catalog syndication product;
- a product-feed standard;
- an AI visibility or GEO product;
- a merchant attribution or observability dashboard as its central value proposition;
- a generic RAG platform;
- an ecommerce platform replacement;
- a universal personal shopping agent;
- a proprietary agent protocol;
- a system whose primary objective is merchant margin;
- a conversational requirements bot that collects every possible field;
- a guarantee that every category can be sold safely without human review or additional process.

## 17. MVP direction

The complete product vision remains broad, but the first executable slice should demonstrate the core thesis rather than every integration.

The initial implementation should be able to show:

1. one merchant context with a product catalog;
2. both public product information and at least one meaningful source of private merchant evidence;
3. a buyer intent expressed naturally;
4. a buyer-first recommendation that can differ from a catalog-only recommendation because of private evidence;
5. minimum sufficient clarification when information is genuinely missing;
6. a case in which merchant objectives legitimately break a buyer-equivalent tie;
7. a case in which the system refuses to let merchant objectives override a materially better buyer outcome;
8. truthful selective disclosure of the resulting recommendation;
9. at least one human-facing ecommerce interaction;
10. a credible path to expose the same intelligence through an agent-facing standard rather than rebuilding the decision logic.

Footwear and outdoor should be used to exercise product selection and solution composition respectively.

The first implementation may use controlled or synthetic merchant data if necessary to make the decision problem explicit before integrating real retailer systems.

## 18. Business model hypotheses

Business model remains open and should not constrain the first product slice.

Models worth evaluating later include:

- SaaS priced by store or business unit;
- usage-based pricing;
- ecommerce-platform application plus paid service;
- API-first merchant intelligence service;
- enterprise contracts;
- agency or solution-partner model;
- white-label deployment;
- an open or free integration component with paid intelligence and management capabilities.

A free or open integration layer is only justified if merchant-side deployment materially improves adoption or data access. It should not be created merely as a distribution tactic without a technical reason to exist.

## 19. Success definition for the product

Evaluation should support product development rather than become the product itself.

The first product is successful if it can demonstrate, in representative scenarios, that:

- private merchant knowledge can materially improve a buyer outcome compared with catalog-only reasoning;
- the system remains buyer-first when merchant objectives conflict with buyer value;
- it can reach useful decisions without unnecessary conversational friction;
- confidential merchant information can influence decisions without leaking into external responses;
- the same merchant intelligence can plausibly serve both human and agent-mediated commerce.

Detailed benchmarks and metric suites belong in the implementation/evaluation design once the first product slice is fixed.

## 20. Key risks and open questions

### Product risk

The difference between strong catalog/search/recommendation systems and Merchant Mind may be too small in some ecommerce categories. The product must focus on cases where private evidence, solution composition, configuration, or merchant expertise materially changes the decision.

### Data availability

Some merchants may not have enough structured or trustworthy internal evidence to create meaningful additional intelligence.

The product should degrade gracefully toward public knowledge rather than requiring every source to be present.

### Evidence quality

Internal data can be biased, stale, sparse, or confounded. Popular products are not automatically better products; high return rates are not automatically product defects.

The product must avoid treating every merchant metric as decision-grade truth.

### Buyer trust

The merchant has an inherent commercial interest. Buyer-first behavior and clear policy boundaries are therefore central to the product's credibility.

### Disclosure

The system must use private knowledge without leaking confidential data or fabricating public explanations.

### Standard evolution

UCP, MCP, A2A, and commerce-platform capabilities are evolving quickly. Merchant Mind should depend on stable conceptual boundaries rather than overfit to one protocol version.

### Platform capture

Large ecommerce or AI platforms may implement increasingly sophisticated merchant intelligence directly. Merchant Mind should remain valuable through channel neutrality, merchant-controlled knowledge, decision quality, and the ability to combine heterogeneous evidence rather than relying only on platform-owned data.

### Human escalation

Some technical categories may require professional assessment, regulatory review, installation validation, or a physical survey. The system must be able to identify when it cannot responsibly complete the decision autonomously.

## 21. Future product directions

These are extensions of the same core product rather than commitments to separate products:

- richer solution configuration;
- specialist B2B commerce;
- technical presales qualification;
- internal salesperson copilot experiences;
- agent-to-agent negotiation within buyer-first policy boundaries;
- incentives and offer construction;
- multi-store or multi-brand merchant intelligence;
- commerce-platform integrations;
- warehouse or enterprise data integration;
- broader technical categories;
- additional machine-facing commerce and agent standards as they mature.

## 22. Decisions made during discovery

The following decisions are considered part of the current product definition:

1. Ecommerce is the starting domain and must remain central.
2. Merchant observability is no longer the primary product thesis.
3. Existing product feeds and commerce protocols should be reused rather than reinvented.
4. Merchant Mind may serve both humans and AI agents through different surfaces over the same intelligence.
5. Private merchant knowledge is a first-class product input.
6. Knowledge ingestion is source-agnostic and not the product itself.
7. Buyer utility is primary; merchant objectives only break buyer-equivalent or near-equivalent ties.
8. Selective disclosure is required because internal evidence and objectives may be confidential.
9. Minimum sufficient interaction is a core product principle.
10. Footwear and outdoor are the initial ecommerce domains for product selection and solution composition.
11. Technical solution-selling categories are valid extensions of the same model.
12. The product may say that the merchant has no solution it can responsibly recommend.
13. The full product scope includes recommendation, composition, configuration, substitution, adaptation, explanation, and incentives, even if implementation is staged.

## 23. Next step

The product discovery is sufficiently complete to stop expanding the concept.

The next phase should turn this PRD into a technical design or SPEC that answers questions such as:

- the boundary between deterministic commerce logic, policy enforcement, retrieval, and model reasoning;
- how merchant knowledge and evidence are represented;
- how public versus confidential context is isolated;
- how buyer-equivalence and merchant tie-breaking are enforced;
- how channel adapters reuse the same decision layer;
- which first ecommerce platform and data fixtures are used;
- which capabilities form the first executable vertical slice.

Those decisions should be made from this product definition rather than reopening the product discovery unless implementation reveals a genuine product contradiction.
