# ESBEN E-Store — Enterprise Data Activation & E-Commerce Platform

**Architecture Showcase** | Designed for high-performance data handling, immaculate SEO, and resilient conversion tracking.

ESBEN E-Store is a highly resilient, enterprise-grade e-commerce platform architected to manage and distribute a live catalog of over 16,000 products. Built to handle heavy data orchestration, it dynamically queries multiple external B2B APIs to ensure 100% accurate, real-time inventory and pricing, while maintaining lightning-fast static generation speeds on the frontend. 

## 🚀 Technology Stack & Integrations

* **Frontend Framework:** Astro (SSG/SSR) for immaculate SEO scores and Google Merchant Center validation.
* **Backend & Orchestration:** Node.js / Java backend capable of processing massive 40MB JSON payloads.
* **Payments & Fulfillment:** MercadoPago API and direct Syscom API integration for automated order generation.
* **State Management:** Native `localStorage` with custom DOM Event dispatching for lightweight, cross-component reactivity.
* **Enterprise Analytics:** Google Analytics 4 (GA4), Meta Pixel, and Google Customer Reviews.

## 🏗️ Core Architecture & Systems Integration

### 1. High-Performance Data Orchestration
Architected a robust backend layer to act as a bridge between heavy B2B supplier feeds and the consumer frontend. 
* **Dynamic API Querying:** The system ingests and parses 40MB JSON payloads, reconciling data across 3 distinct external APIs to establish a single source of truth for inventory and pricing in real-time.
* **Scalable Process:** Ensures the frontend remains ultra-fast and SEO-optimized while abstracting the heavy lifting of inventory synchronization to the backend.

### 2. Resilient Conversion Pipeline & Data Activation
Engineered a bulletproof analytics pipeline to ensure 100% accurate attribution and ROI measurement, directly addressing common race conditions in e-commerce tracking.
* **Protected Event Execution:** All tracking events (`view_item`, `add_to_cart`, `purchase`) are wrapped in strict type-checks (e.g., `typeof gtag === 'function'`) to prevent execution errors if third-party scripts are blocked by ad-blockers.
* **Google Customer Reviews Orchestration:** Dynamically injects order fulfillment data (calculating estimated delivery as `current_date + 5 days`) directly into the post-purchase flow to drive merchant trust scores.

### 3. State-Preserving Checkout Architecture
Designed a decoupled checkout and conversion flow to guarantee data integrity between the order creation backend and the frontend tracking tags.
* **Delayed State Clearing:** During the `checkout` phase, the system generates the order via the Syscom API and redirects the user to the success page (`/gracias`) *without* clearing the local cart state (`esben_carrito`). 
* **Atomic Conversion & Cleanup:** The success page acts as the orchestrator. Upon `DOMContentLoaded`, it reads the intact cart state, calculates the exact mathematical payload (`value = Σ price × quantity`), and reliably fires the GA4 `purchase` and Meta Pixel `Purchase` events.
* **Duplicate Prevention:** Only *after* a successful analytics dispatch does the system wipe the local storage and fire a custom `carritoActualizado` DOM event to instantly update the UI globally. This strictly prevents duplicate $0 conversions if a user refreshes the thank-you page.
