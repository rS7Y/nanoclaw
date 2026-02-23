---
name: property-pdf
description: Generate a luxury property brochure PDF from a property file. Use when asked to "make a PDF", "create a brochure", "export property as PDF", or "send PDF" for any property.
allowed-tools: Bash(agent-browser:*), Write, Read
---

# Property PDF Brochure Generator

Generate a beautifully designed PDF brochure for a property listing using the property's .md file.

## Quick usage

```
make pdf for P001
create brochure for Girim Arjun Vilas
export P002 as PDF
```

## Workflow

### Step 1 — Read the property file

Find the property .md file in `/workspace/group/properties/YYYY-MM/PXXX_slug.md`.

Read it fully to extract all details: name, location, price, specs, features, status, source agent.

### Step 2 — Generate the HTML brochure

Write a self-contained HTML file to `/workspace/group/properties/PXXX_brochure.html`.

Use this template as a base — customise all content for the specific property:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{{PROPERTY_NAME}} — Alishan Properties</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@300;400;600&family=Inter:wght@300;400;500&display=swap');

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Inter', sans-serif;
    background: #faf8f5;
    color: #1a1a1a;
    width: 210mm;
    min-height: 297mm;
  }

  .hero {
    background: linear-gradient(135deg, #0f2027 0%, #1a3a4a 50%, #0f2027 100%);
    padding: 60px 50px 50px;
    position: relative;
    overflow: hidden;
  }

  .hero::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0; bottom: 0;
    background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='0.03'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
    opacity: 0.5;
  }

  .brand {
    font-family: 'Cormorant Garamond', serif;
    font-size: 13px;
    font-weight: 300;
    letter-spacing: 4px;
    text-transform: uppercase;
    color: #c9a96e;
    margin-bottom: 40px;
    position: relative;
  }

  .property-name {
    font-family: 'Cormorant Garamond', serif;
    font-size: 48px;
    font-weight: 300;
    color: #ffffff;
    line-height: 1.1;
    margin-bottom: 12px;
    position: relative;
  }

  .property-location {
    font-size: 14px;
    font-weight: 300;
    letter-spacing: 2px;
    color: #c9a96e;
    text-transform: uppercase;
    position: relative;
  }

  .price-badge {
    position: relative;
    margin-top: 30px;
    display: inline-block;
    background: rgba(201, 169, 110, 0.15);
    border: 1px solid rgba(201, 169, 110, 0.4);
    padding: 12px 24px;
    border-radius: 2px;
  }

  .price-label {
    font-size: 10px;
    letter-spacing: 2px;
    text-transform: uppercase;
    color: #c9a96e;
    display: block;
    margin-bottom: 4px;
  }

  .price-value {
    font-family: 'Cormorant Garamond', serif;
    font-size: 28px;
    font-weight: 600;
    color: #ffffff;
  }

  .content {
    padding: 50px;
  }

  .tagline {
    font-family: 'Cormorant Garamond', serif;
    font-size: 22px;
    font-weight: 300;
    color: #1a3a4a;
    line-height: 1.5;
    margin-bottom: 40px;
    padding-bottom: 40px;
    border-bottom: 1px solid #e8e0d4;
    font-style: italic;
  }

  .section-title {
    font-size: 10px;
    letter-spacing: 3px;
    text-transform: uppercase;
    color: #c9a96e;
    margin-bottom: 20px;
    font-weight: 500;
  }

  .specs-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    margin-bottom: 40px;
  }

  .spec-item {
    padding: 16px;
    background: #ffffff;
    border: 1px solid #e8e0d4;
    border-left: 3px solid #c9a96e;
  }

  .spec-label {
    font-size: 10px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: #888;
    margin-bottom: 6px;
  }

  .spec-value {
    font-family: 'Cormorant Garamond', serif;
    font-size: 18px;
    color: #1a1a1a;
    font-weight: 600;
  }

  .features {
    margin-bottom: 40px;
  }

  .features-list {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    list-style: none;
  }

  .features-list li {
    font-size: 13px;
    color: #444;
    padding: 8px 0;
    border-bottom: 1px solid #f0ebe3;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .features-list li::before {
    content: '◆';
    color: #c9a96e;
    font-size: 8px;
    flex-shrink: 0;
  }

  .description {
    background: #1a3a4a;
    color: #ffffff;
    padding: 30px;
    margin-bottom: 40px;
  }

  .description p {
    font-family: 'Cormorant Garamond', serif;
    font-size: 17px;
    line-height: 1.7;
    font-weight: 300;
  }

  .footer {
    border-top: 1px solid #e8e0d4;
    padding-top: 24px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .footer-brand {
    font-family: 'Cormorant Garamond', serif;
    font-size: 18px;
    color: #1a3a4a;
    letter-spacing: 2px;
  }

  .footer-agent {
    font-size: 12px;
    color: #888;
    text-align: right;
  }

  .status-tag {
    display: inline-block;
    padding: 4px 12px;
    font-size: 10px;
    letter-spacing: 2px;
    text-transform: uppercase;
    background: #e8f4e8;
    color: #2d6a2d;
    border-radius: 2px;
    margin-top: 8px;
  }

  .status-tag.construction {
    background: #fff3e0;
    color: #8a5a00;
  }
</style>
</head>
<body>

<div class="hero">
  <div class="brand">Alishan Properties · Goa</div>
  <div class="property-name">{{PROPERTY_NAME}}</div>
  <div class="property-location">{{LOCATION}}, Goa</div>
  <div class="price-badge">
    <span class="price-label">Starting From</span>
    <span class="price-value">{{PRICE}}</span>
  </div>
</div>

<div class="content">

  <p class="tagline">{{TAGLINE}}</p>

  <div class="section-title">Property Specifications</div>
  <div class="specs-grid">
    <div class="spec-item">
      <div class="spec-label">Configuration</div>
      <div class="spec-value">{{CONFIG}}</div>
    </div>
    <div class="spec-item">
      <div class="spec-label">Built-up Area</div>
      <div class="spec-value">{{AREA}}</div>
    </div>
    <div class="spec-item">
      <div class="spec-label">Plot Area</div>
      <div class="spec-value">{{PLOT}}</div>
    </div>
    <div class="spec-item">
      <div class="spec-label">Status</div>
      <div class="spec-value">{{STATUS}}</div>
    </div>
  </div>

  <div class="features">
    <div class="section-title">Features & Amenities</div>
    <ul class="features-list">
      {{FEATURES_LIST}}
    </ul>
  </div>

  <div class="description">
    <p>{{MARKETING_DESCRIPTION}}</p>
  </div>

  <div class="footer">
    <div class="footer-brand">ALISHAN</div>
    <div class="footer-agent">
      Presented by {{SOURCE_AGENT}}<br>
      <span style="color:#c9a96e">For enquiries, contact via WhatsApp</span>
    </div>
  </div>

</div>
</body>
</html>
```

**Fill in the placeholders:**
- `{{PROPERTY_NAME}}` — project name (e.g. "Girim Arjun Vilas")
- `{{LOCATION}}` — area (e.g. "Girim, North Goa")
- `{{PRICE}}` — starting price (e.g. "₹7.75 Cr")
- `{{TAGLINE}}` — one evocative sentence using marketing psychology: lifestyle, outcome, identity
- `{{CONFIG}}` — e.g. "4BHK + Servant Room"
- `{{AREA}}` — e.g. "3,700 – 4,220 sq ft"
- `{{PLOT}}` — e.g. "370 – 400 sq mts"
- `{{STATUS}}` — e.g. "Ready to Move In" or "Under Construction"
- `{{FEATURES_LIST}}` — one `<li>Feature</li>` per feature
- `{{MARKETING_DESCRIPTION}}` — 2–3 sentences using AIDA: hook on best feature, key specs, investment/lifestyle angle, CTA
- `{{SOURCE_AGENT}}` — agent name from the property file

### Step 3 — Convert to PDF

```bash
agent-browser open file:///workspace/group/properties/PXXX_brochure.html
agent-browser wait 2000
agent-browser pdf /workspace/group/properties/PXXX_brochure.pdf
agent-browser close
```

### Step 4 — Confirm

Tell the user: "PDF saved to `properties/PXXX_brochure.pdf`. You can download it from your workspace."

If multiple villas exist for one project (like P001 with Villa 2 and Villa 5), generate one PDF per villa with villa-specific details, or a combined brochure — ask the user which they prefer.

## Tips

- Write the tagline and marketing description using the principles in your CLAUDE.md — frame around lifestyle and outcomes, not just specs
- For "Under Construction" properties, emphasise the investment angle ("early-entry pricing") and expected handover
- If the user says "make it shareable" or "send this", remind them the PDF is at `/workspace/group/properties/` on the host machine
