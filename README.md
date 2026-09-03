# Merchant Mind

> Buyer-first merchant intelligence for AI-mediated commerce.

Merchant Mind explores how a merchant can remain an intelligent participant in the buying decision when product discovery and shopping are increasingly mediated by AI.

The core idea is simple: a buyer agent may know the buyer very well, but the merchant often knows the merchandise better than anyone else. That knowledge is not limited to the public catalog. It may include returns, fit and sizing evidence, product issues, fulfillment reliability, compatibility, inventory context, customer-service knowledge, internal documentation, merchandising expertise, and other signals that a buyer agent should not necessarily receive directly.

Merchant Mind uses that knowledge to help a buyer — human or agentic — reach the best solution the merchant can actually provide.

## Product thesis

A product feed, structured catalog, or commerce API can expose products and transactional capabilities. They do not by themselves expose the merchant's decision intelligence.

Merchant Mind is intended to provide a buyer-first decision layer that can:

- understand a buyer's need and constraints;
- ask only the missing questions that can materially change the decision;
- recommend or compare individual products;
- compose multiple products into a coherent solution;
- configure or adapt a solution when the category requires it;
- substitute unavailable or unsuitable items while preserving the original intent;
- explain the proposal without leaking confidential merchant information;
- use merchant objectives only when alternatives are effectively equivalent for the buyer;
- expose the same merchant intelligence to human-facing and agent-facing channels.

The initial product wedge is ecommerce. Footwear and outdoor are the first domains considered because they cover both high-volume product recommendation and multi-product solution composition. More technical solution-selling categories — for example irrigation, pergolas, HVAC, components, tools, or other configurable products — are natural extensions rather than a change of product.

## Buyer-first policy

Merchant Mind does **not** optimize a weighted mixture of buyer value and merchant value.

The intended policy is lexicographic:

1. satisfy hard constraints and factual requirements;
2. maximize the quality of the outcome for the buyer;
3. identify alternatives that are materially equivalent or near-equivalent for the buyer;
4. only inside that set may merchant objectives act as tie-breakers;
5. disclose only information and explanations that are appropriate to expose externally.

A higher-margin, overstocked, private-label, or strategically preferred product must not beat a materially better product for the buyer merely because it is more valuable to the merchant.

Merchant objectives can, however, create buyer value. For example, excess inventory may allow the merchant to offer an incentive that makes an otherwise equivalent product better for both parties.

## Knowledge, not just catalog data

Merchant Mind is intentionally source-agnostic.

Possible sources include:

### Public or externally shareable knowledge

- catalog and product data;
- variants, prices, inventory, promotions;
- product specifications;
- public policies and documentation;
- public reviews and Q&A;
- manufacturer information.

### Merchant-internal knowledge

- orders and kept purchases;
- returns and return reasons;
- fit and sizing evidence;
- complaints, support tickets and warranty signals;
- product reliability and compatibility evidence;
- fulfillment performance;
- inventory and operational context;
- internal documentation;
- merchandising or sales expertise;
- structured databases, APIs, warehouse data, RAG sources, or other merchant-owned evidence.

### Buyer context

- current intent and constraints;
- preferences explicitly provided by the buyer;
- authenticated history or profile data when available and permitted;
- context shared by an external buyer agent.

### Merchant objectives and constraints

- margin and contribution margin;
- stock and sell-through objectives;
- fulfillment cost or inventory balancing;
- private-label or assortment priorities;
- vendor or campaign constraints;
- eligibility, regulatory, territorial, contractual, or operational rules.

The way knowledge is acquired is an implementation concern. Adding a database, document collection, transcript corpus, RAG pipeline, or expert session changes the evidence available to the system, not the product model itself.

## Minimum sufficient interaction

Merchant Mind must not become an agent that chats until the buyer gives up.

Conversation is an acquisition mechanism, not the product.

The system should first use the knowledge and context it already has, then ask only questions whose answers can materially change the recommendation or configuration. If the current evidence is already sufficient for a good decision, it should propose a solution instead of collecting more information for its own sake.

This matters even more in technical ecommerce categories, where a traditional configurator can easily become a long questionnaire disguised as chat.

## Selective disclosure

The system may use information that should not be exposed to the buyer or buyer agent.

For example, a recommendation may be influenced by aggregated return evidence or fulfillment reliability without exposing raw return rates, internal margins, stock pressure, vendor agreements, or confidential operational data.

Merchant Mind therefore separates:

- evidence used internally for decision-making;
- information that may be expressed as an externally useful claim;
- confidential merchant context that must remain private.

Explanations must remain truthful: confidential evidence may be withheld, but it must not be replaced with invented public reasoning.

## Human and agent channels

The same merchant intelligence should not be rebuilt separately for every channel.

A human may interact through an onsite ecommerce advisor. A salesperson or customer-service operator may use an internal interface. An external AI may invoke the merchant through machine-readable commerce and agent protocols.

Conceptually:

```text
                 Merchant Intelligence
                        │
        ┌───────────────┼────────────────┐
        │               │                │
  Ecommerce advisor   Internal UI    Agent interfaces
      (human)       sales/support     UCP / MCP / A2A
```

The project should use emerging standards where they already solve discovery, commerce primitives, tool invocation, or agent-to-agent communication instead of inventing a new protocol unnecessarily.

Current standards of particular interest include:

- Universal Commerce Protocol (UCP) for commerce capability discovery and standardized commerce operations;
- Model Context Protocol (MCP) for exposing callable tools to AI systems;
- Agent2Agent (A2A) for agent discovery and agent-to-agent interaction.

Merchant Mind is intended to add merchant-side decision intelligence **above** those primitives, not replace them.

## Initial ecommerce domains

### Footwear

The simplest strong case for private merchant intelligence.

Public catalog data may make two products appear similar, while internal evidence can reveal meaningful differences in fit, sizing stability, return reasons, reliability, delivery performance, or post-purchase satisfaction.

The system should use that evidence to improve the buyer's outcome while preventing merchant objectives from overriding materially better recommendations.

### Outdoor

A step from product selection to solution composition.

A buyer may describe a trip, existing equipment, budget, preferences, weather constraints, and other requirements. The merchant must determine what is actually needed, avoid redundant products, preserve compatibility, adapt to changes or stockouts, and compose a coherent solution rather than returning a ranked list of SKUs.

## Beyond ecommerce recommendation

The same model can support higher-consideration technical selling.

Examples include irrigation systems, pergolas, HVAC, networking, components, machinery, tools, and other categories where the buyer describes a problem rather than a SKU.

In these cases the system may need to clarify a small number of decision-critical facts — dimensions, operating conditions, compatibility, capacity, installation constraints, and similar variables — before proposing a solution.

The principle remains unchanged: use all available merchant knowledge, minimize buyer effort, and produce the best solution the merchant can responsibly provide.

## What Merchant Mind is not

Merchant Mind is not intended to be:

- a generic ecommerce chatbot;
- a customer-service bot with a product-search tool;
- a new product-feed format;
- a feed management or catalog syndication platform;
- an AI visibility/GEO dashboard;
- a merchant observability or attribution dashboard as its primary product;
- a generic RAG product;
- a replacement ecommerce platform;
- a new commerce or agent protocol;
- a margin-maximizing sales agent that can sacrifice buyer value;
- a conversational questionnaire that insists on completing a schema before helping the buyer.

## Current status

Product discovery is complete enough to move into product definition.

The project originated as an exploration of **Agentic Commerce / Merchant Observability** and evolved toward merchant-side decision intelligence after examining the parts of catalog exposure, commerce capability discovery, checkout, and agent interoperability that emerging standards and platforms are already addressing.

The current product definition is documented in [`docs/PRD.md`](docs/PRD.md).

Implementation architecture is intentionally not fixed yet. The next stage should turn the product requirements into a technical design and an executable first slice without prematurely building a generic commerce-agent framework.
