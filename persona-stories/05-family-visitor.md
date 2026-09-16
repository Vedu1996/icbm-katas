# Persona Story: Family Visitor

## Persona

**Names:** The Patel family: Priya, Daniel, and their two children  
**Role:** Family visitors  
**Primary goal:** Enjoy a safe, low-stress day with activities suitable for everyone.

## A Day at the Estate

The Patel family arrives with a family pass. Each family member has a signed sub-ticket, allowing the parents and children to enter rides independently without requiring a cloud request for every scan.

The family asks the guest chatbot where to find the piranha display and which rides currently have shorter queues. The chatbot uses current zone occupancy and estimated wait times to suggest a route. The estate receives only anonymous crowd measurements rather than a persistent movement history for the family.

One child asks the chatbot an inappropriate question. The response is filtered through child-safety and topic controls. Later, the parents ask about an apparent animal welfare concern. The chatbot avoids making a medical or operational judgment and provides a route to human staff.

As the family moves through the estate, their presence contributes only to aggregate occupancy and queue statistics. Those statistics help staff manage busy areas without identifying the family.

## System Interaction

- Family passes produce independently verifiable signed sub-tickets.
- Turnstiles validate ride access locally and record scans for later synchronization.
- Edge computer vision measures zone occupancy without identifying visitors.
- The chatbot provides visitor information using approved data sources.
- The LLM gateway applies child-safety, content, privacy, and escalation controls.
- Human staff remain responsible for safety and animal welfare decisions.

## Value to the Family

The family spends less time dealing with ticket logistics and more time enjoying the estate. They receive practical queue guidance, children get safer automated assistance, and serious concerns can be handed to staff without relying on an AI response.
