# Persona Story: Individual Visitor

## Persona

**Name:** Marcus Lee  
**Role:** Individual visitor  
**Primary goal:** Spend a relaxed day at the estate with minimal waiting and clear information.

## A Day at the Estate

Marcus buys a digital ticket through the visitor portal. At the entrance, the turnstile scans his QR code and verifies the signed ticket locally. He enters without waiting for a live call to the cloud.

At the ride area, the turnstile verifies his ticket entitlement and records the scan. If the estate temporarily loses connectivity, the scan can be stored locally and synchronized later.

Marcus asks the guest chatbot for a quieter route through the estate and current wait times for two rides. The chatbot uses aggregate crowd information from edge vision systems. It does not need to know Marcus's identity or follow his movement from zone to zone.

He later reports that a sign near an animal enclosure is confusing. The feedback becomes an event for the guest-experience team. When Marcus asks about a possible animal welfare issue, the chatbot provides limited guidance and directs him to human staff rather than making an official assessment.

## System Interaction

- The visitor portal handles online ticket purchase.
- Signed tickets support fast, offline-capable entry validation.
- Turnstiles maintain local scan state to reduce replay risk.
- Edge computer vision produces anonymous crowd counts and wait estimates.
- The guest chatbot accesses approved read-only information.
- The LLM gateway applies privacy, safety, scope, and escalation controls.

## Value to Marcus

Marcus gets quick entry, useful crowd information, and assistance without being individually tracked. He also has a clear route to a human when his question is too important or sensitive for an automated response.
