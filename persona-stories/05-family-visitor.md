# Persona Story: Family Visitor

## Persona

**Names:** The Patel family: Priya, Daniel, and their two children  
**Role:** Family visitors  
**Primary goal:** Have a fun, low-stress day with easy access, family-friendly guidance, and a safe environment.

## A Day at the Estate

The Patel family arrives with a family pass. Each family member has their own pass inside it, so the adults and children can move through different rides and gates without waiting for a new cloud check every time. They simply scan and go.

Soon after, they ask the guest assistant for a family-friendly route and which rides are quieter at that moment. The answer is based on current crowd levels, not on tracking the family’s movements across the park. It helps them plan the day without turning the experience into a surveillance system.

One of the children asks an awkward question to the assistant. The assistant responds with guarded, age-appropriate content and keeps the conversation safe. Later, the parents ask whether a particular animal looks unwell. The assistant does not try to diagnose anything. It gives a safe response and points them to a member of staff instead.

The family spends the rest of the day moving around the estate without feeling like they are being watched. Their presence contributes to the overall understanding of busy areas, which helps the estate manage crowds better and keep the park enjoyable for everyone.

## System Interaction

- Family passes are validated quickly and locally at each gate and ride. See [ADR-003](../adrs/%5BADR-003%5D%20Asymmetric%20cryptography%20for%20ticketing%20systems.md).
- The estate understands crowd pressure without identifying individual visitors. See [ADR-002](../adrs/%5BADR-002%5D%20Edge%20Computer%20Vision.md).
- The guest assistant gives practical answers using current, approved estate information. See [ADR-010](../adrs/%5BADR-010%5D%20LLM%20Gateway%20for%20Smart%20Reporting%20and%20Chatbot.md).
- Sensitive or safety-related questions are escalated to staff rather than answered automatically. See [ADR-010](../adrs/%5BADR-010%5D%20LLM%20Gateway%20for%20Smart%20Reporting%20and%20Chatbot.md).

## Value to the Family

The family has an easier day with less ticket friction, better queue information, and safer digital assistance for the children. It makes the estate feel more welcoming and helps families want to return, which is exactly the kind of experience that supports growth and repeat visits.
