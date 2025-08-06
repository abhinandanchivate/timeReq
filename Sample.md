

# **Deep Case Study: SOLID Failures at Scale – Rewriting a Billing Engine in a Unicorn Fintech**

---

## 🏢 Context

* **Company**: Large fintech unicorn (\~100M users)
* **Module**: Global billing & invoicing microservice
* **Tech Leads**: All 15–20+ yrs experienced, ex-bank/enterprise veterans
* **Problem**: Business logic was unmanageable, brittle, unscalable
* **Goal**: Migrate from a tightly-coupled monolithic billing processor to a scalable, maintainable service

---

## 🔍 Where Senior Engineers Got It Wrong

* **Architectural Inversion**: Applied DI everywhere, causing accidental complexity
* **Misapplied SRP**: Split classes by domain, not responsibility
* **Wrong Abstractions**: Built abstractions for non-volatile parts
* **God-Config Anti-pattern**: 100+ feature flags in 1 class
* **Avoided Interfaces**: Over-optimized for performance/readability

---

## 💥 Real Breakdown: The "RateEngine.java" Disaster

**Initial Design had 300+ lines of logic combining tax, FX, discounts, region, etc.**
→ Completely violated SRP, OCP, DIP, LSP, and was impossible to test or extend.

---

## 🧰 What the Fix Looked Like (Architecturally)

Refactored into:

* `RateEngine`: Orchestrator only
* `PricingStrategyProvider`: OCP-compliant strategies
* `DiscountPolicyChain`: LSP & DIP
* `FXRateService`: Interface-based
* `InvoiceBuilder`: SRP

---

## 👎 Behavioral + Organizational Failures from 15+ Yrs Engineers

* Favored central god classes due to mainframe background
* Avoided interfaces
* Used feature flags to simulate strategies
* Tied domain models to service layers

---

## 🧠 Coaching / Transformation Methods

* Use runtime failure simulations
* Refactoring dojos
* Guardrails (max responsibility per class)
* Plugin-based rule engines
* DDD/event storming for modularity

---

## 🧬 Final Refactor Outcome

* From 1 monolith to 17 components
* Unit test coverage jumped from 20% to 85%
* Plug-in architecture reduced deployment risks

---

## ✅ Architectural Reasoning (Layer by Layer)

**1. RateEngine as Orchestrator (SRP)**
→ Clear separation of coordination logic, now mockable/testable.

**2. PricingStrategy (OCP)**
→ Strategy pattern allows extension per region/customer.

**3. TaxCalculator (DIP + LSP)**
→ Easily plug new tax policies, no core changes.

**4. FXRateService (DIP)**
→ Adapter pattern supports switching providers.

**5. DiscountPolicyChain (SRP + OCP)**
→ New rules can be added/removed independently.

---

## ✅ Benefits Summary

* Class sizes dropped from 3000+ to < 250 LOC
* Test coverage: 85%+
* New rule integration time dropped from 3 days to < 1 day
* Regression dropped significantly

---

## 🧠 Key Insight for Mentoring Seniors

**They don’t need principles — they need consequences.**
Instead of saying "follow SRP/OCP", show how a change breaks the system, causes regressions, or blocks scalability.

---

## 📘 TL;DR (for Architects & Leads)

| Layer           | SOLID Principle | Pattern                 | Why                     |
| --------------- | --------------- | ----------------------- | ----------------------- |
| RateEngine      | SRP             | Orchestrator            | Keeps workflow clear    |
| PricingStrategy | OCP             | Strategy                | Pluggable logic         |
| TaxCalculator   | DIP, LSP        | Strategy + Interface    | Country-specific logic  |
| FXProvider      | DIP             | Adapter                 | External API swap       |
| DiscountPolicy  | SRP, OCP        | Chain of Responsibility | Rule chaining, testable |

---

