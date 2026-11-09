# TODO

## Corporate site structure, navigation, and company content
- [x] Build the corporate website for Solace, a commercial solar installer in Texas (founded 2011, 420 staff, 310 MW installed on warehouses, factories and schools). It must look like a real established energy company site, not a startup landing page.
- [x] Include a thin utility bar with Customer portal, Support, Careers, and a phone number.
- [x] Include a main menu with dropdowns: Solutions (Rooftop Solar, Carports, Battery Storage, Operations & Maintenance), Industries, Projects, Financing, About. Add a “Get an estimate” button.
- [x] Include a full-width real aerial photo of a large warehouse roof covered in panels, a short left-aligned headline, and two buttons.
- [x] Include Solutions, industries, a row of company figures, 3 project case studies (client, size in kW, yearly savings), financing options, dated news, and a dense footer with offices, licences and legal links.

## Estimate entry and site-specific estimate
- [x] Add a white panel overlapping the bottom of the hero: “Estimate your savings”, with building address, building type and average monthly bill.
- [x] The estimate panel opens `/estimate` with the submitted building information.
- [x] The estimate page shows a satellite view of the address (use the platform Maps proxy) and an estimate of system size, savings and payback.

## Site-assessment lead capture and owner notification
- [x] After the estimate, “Request a site assessment” asks for name, company, role, work email, phone and consent.
- [x] Save each lead with the address, estimate and UTM source in the database, notify the project owner, and preserve a successfully stored lead if the notification cannot be delivered.

## Private lead administration and export
- [x] Provide an owner-only `/admin` table for leads.
- [x] Provide CSV export of the authorized lead records.

## Visual identity, responsive behavior, and photography
- [x] Use a white background, deep green `#0F3D2E`, one amber `#F2A900` for actions, IBM Plex Sans, square corners, thin dividers, and real-looking photography of installations and crews.
- [x] Do not use gradients, glowing suns, icons in circles, or emoji.
