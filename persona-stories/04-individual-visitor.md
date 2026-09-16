# Persona Story: Individual Visitor

## Persona

**Name:** Marcus Lee  
**Role:** Individual visitor  
**Primary goal:** Enjoy the day with easy access, short waits, and clear guidance.

## A Day at the Estate

Marcus buys his ticket online before arriving. At the gate, he scans his pass and walks through quickly. The entry check happens locally, so he does not have to wait for a cloud call or a slow connection to be approved.

Later, he wants to know which parts of the estate are quieter and which rides have the shortest lines. He asks the guest assistant for a route and wait estimates, and it gives him a simple answer based on current crowd levels. It does not know who Marcus is or follow him around the estate.

As he walks through the park, Marcus notices a sign near an animal enclosure that is unclear. He reports it through the guest experience flow, and the estate team can pick it up quickly. He also asks whether an animal seems unwell. The assistant does not make a medical judgment; it gives him safe, limited information and directs him to a member of staff.

The system is designed to make the visit smoother without turning the day into a tracking exercise. Marcus gets useful information, quick access, and a clear route to a person if his question needs a real answer.

## System Interaction

- Ticket scanning works quickly even if the wider network is shaky. See [ADR-003](../adrs/%5BADR-003%5D%20Asymmetric%20cryptography%20for%20ticketing%20systems.md) and [ADR-001](../adrs/%5BADR-001%5D%20Event%20Driven%20Architecture.md).
- Crowd information is shared as broad, anonymous estimates rather than details about an individual visitor. See [ADR-002](../adrs/%5BADR-002%5D%20Edge%20Computer%20Vision.md).
- The guest assistant gives helpful guidance based on approved estate data. See [ADR-010](../adrs/%5BADR-010%5D%20LLM%20Gateway%20for%20Smart%20Reporting%20and%20Chatbot.md).
- If the question is sensitive or uncertain, the system points Marcus to a human rather than guessing. See [ADR-010](../adrs/%5BADR-010%5D%20LLM%20Gateway%20for%20Smart%20Reporting%20and%20Chatbot.md).

## Value to Marcus

Marcus enjoys a smoother day with less waiting and clearer directions. He gets the information he needs without feeling watched, and he knows there is always a human available for anything important or sensitive.
