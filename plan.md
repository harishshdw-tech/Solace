# Solace Commercial Solar Website — Implementation Plan

## Project scope and approach

Build the corporate marketing website and operational estimate/lead workflow in the initialized React, Express, tRPC, Drizzle, and managed-MySQL project. Preserve the selected managed server/database and the starter's real Manus OAuth/session flow. Do not add a separate password login or fake development identity. Public visitors can request an estimate without signing in; `/admin` requires an authenticated Solace owner account and rejects non-owner users on the server.

The estimate form on the homepage captures address, building type, and average monthly electricity bill and opens `/estimate` with those inputs retained. The estimate page geocodes the submitted address with the platform Maps proxy, displays a satellite static map returned through a server-side proxy route, and presents clearly labeled indicative system-size, annual-savings, and simple-payback figures. Its subsequent site-assessment form requests name, company, role, work email, phone, and affirmative consent. Submission persists the contact fields, address, building type, bill, estimate inputs/results, consent timestamp, and UTM source/metadata in a new Drizzle-managed leads table; then it sends a best-effort owner notification using the platform owner-notification API. A notification delivery failure must not discard a successfully stored lead. Owner-only `/admin` lists leads with appropriate contact and estimate details and exports the same authorized records as CSV.

Indicative calculations will be transparent rather than represented as a site-specific engineering quote: derive estimated annual usage from a displayed default blended energy-rate assumption; model a conservative portion of that load as solar-served using a building-type factor; use a displayed Texas annual production assumption to derive system kW; apply the same estimated offset to yearly bill savings; and calculate simple payback using a displayed installed-cost assumption. Round results for readability, preserve the exact values used with the lead, and show that actual production, tariff, roof condition, interconnection, incentives, and savings require a site assessment. Do not imply a guaranteed quote or savings.

Use a server route for address geocoding and a validated static-map image proxy with `maptype=satellite`, both calling `$MANUS_API_URL/v1/maps/proxy/...` with the server credential. No Google key is requested or exposed to the browser. The lead alert calls the documented owner-only platform notification endpoint. The initialized project's owner identity/role model will be enforced server-side for every admin query and CSV export.

## Project structure

- `client/src/pages/Home.tsx`: corporate homepage, navigation/dropdowns, hero, estimate entry panel, solutions, industries, statistics, three case studies, financing, dated news, and detailed footer.
- `client/src/pages/Estimate.tsx`: address/building/bill summary, satellite map, estimate breakdown, and assessment-request form.
- `client/src/pages/Admin.tsx`: owner-only leads table, authentication state, empty/error states, and CSV download action.
- `client/src/components/`: reusable site header, navigation menu, estimate form, and footer components where useful.
- `client/src/App.tsx`: register `/`, `/estimate`, and `/admin` explicitly, plus the starter's `/404` handling.
- `client/src/index.css` and `client/index.html`: IBM Plex Sans, brand tokens, responsive corporate layout, square geometry, and interaction styling.
- `server/routers.ts`: typed lead submission, owner-leads, and owner-notification procedures; the client exports only the authorized records returned by the private server procedure.
- `server/maps.ts` or a focused Express map module: proxy geocoding and satellite static-map responses through the platform Maps proxy with bounded inputs and upstream error handling.
- `server/db.ts` and `drizzle/schema.ts`: lead insert/list support, lead table, ownership checks, and typed lead/estimate fields.
- `drizzle/`: committed additive migration for the leads table; no destructive schema changes or test lead writes to the shared database.
- `client/public/manus-routes.json`: source-synchronized page route manifest at the configured Vite public root for `/`, `/estimate`, `/admin`, legal pages, and the 404 route.
- `app.config.ts`: set a durable Solace branding logo URL before checkpointing.

## Design direction

Three possible corporate treatments and low-tail selection weights: restrained infrastructure-editorial (0.08); polished institutional annual-report (0.04); minimal engineering specification (0.02). **Selected: restrained infrastructure-editorial**, aligned with the user's explicit established-energy-company direction.

- **Design Movement:** restrained industrial editorial, like a mature regional utility's annual report and project journal.
- **Core Principles:** operational credibility over startup novelty; information-rich but calm; photography shows actual-scale infrastructure and people at work; every interaction is direct and accessible.
- **Color Philosophy:** a white field and fine neutral dividers keep dense corporate information legible; deep green `#0F3D2E` anchors the brand and headings; amber `#F2A900` is reserved for action buttons and selected active states. Avoid extra accent hues, gradients, glowing solar motifs, and circular icon containers.
- **Layout Paradigm:** broad horizontal editorial sections, asymmetric image/text pairings, generous but practical margins, and an estimate panel that overlaps the hero. Avoid a generic startup centered-hero/card-grid rhythm.
- **Signature Elements:** thin engineering-style rules; compact uppercase section markers; disciplined project metrics with clear units. Square corners throughout.
- **Interaction Philosophy:** navigation dropdowns open by click and keyboard; forms validate and preserve entered address inputs; estimate-to-assessment flow remains clear; reduced-motion preferences are respected.
- **Animation:** restrained hover/focus transitions for controls only; no parallax or image entrance animation; reduced-motion preferences are respected.
- **Typography System:** IBM Plex Sans throughout; substantial but compact headline weight, readable body copy, and tabular/monospaced-looking numeric treatment only where useful for project metrics. Load IBM Plex Sans deliberately rather than inheriting starter defaults.
- **Brand Essence:** an experienced Texas commercial-solar partner that makes complex projects practical for building owners; steady, capable, accountable.
- **Brand Voice:** precise, informed, and plainspoken. Examples: “More power from the roof you already own.” “Designed for the demands of Texas facilities.”
- **Wordmark & Logo:** a restrained custom SOLACE wordmark paired with a simple rectilinear roof-panel line mark; no circular badge.
- **Signature Brand Color:** Solace deep green `#0F3D2E`.

## Content and trust notes

Use the company facts supplied by the user: founded 2011, 420 staff, and 310 MW installed. Create three representative project profiles with named example clients, system kW, and annual savings, and mark them as representative examples unless the user supplies verified customer/project data. News dates and headlines are sample corporate newsroom content. Use Texas office-city labels rather than inventing street addresses. The prompt provides no verified public phone number or contractor license numbers; place conspicuous, easy-to-replace contact/license placeholders in the footer rather than fabricate legal credentials or a working telephone number. Legal links are clearly grouped. Confirm these placeholders before a public launch.

## Dependencies and serving

Use existing pinned project dependencies and starter conventions. Keep public marketing pages accessible without authentication. `OWNER_OPEN_ID` is supplied through protected platform input; on Manus OAuth sign-in, only that identity receives the admin role, and the lead-list procedure checks both role and owner identity server-side. Protect CSV output by exporting only those authorized records. Store durable records in the selected managed MySQL database, including an assumption snapshot for each estimate. The development and production app both use the runtime server on port 3000; implement health behavior through the starter and use relative browser-facing paths so Cloud Preview proxying remains correct. The currently registered TypeScript diagnostics are host-managed and should remain enabled.

## Material constraints

This plan was reviewed and approved before implementation. No verified Solace phone/license information has been provided. The estimate is illustrative until a real tariff, production model, and EPC assumptions are supplied. The platform Maps proxy and owner-notification service use managed runtime credentials; verify responses and degrade gracefully without exposing private credentials. Do not seed or test-write lead rows because development and production share the managed database.
