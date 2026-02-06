# System Design: Notification System

## Prompt
Design a scalable notification system that can send real-time and scheduled notifications (email, SMS, push) to millions of users. Discuss how you would handle message delivery guarantees, user preferences, and system reliability.

## Key Requirements
- Real-time and scheduled notifications
- Multiple channels: email, SMS, push
- Scalability for millions of users
- Delivery guarantees (at-least-once, exactly-once)
- User preferences (opt-in/out, channel selection)
- Reliability and fault tolerance

## Steps/Strategy to Answer
1. **Clarify Requirements**
   - Types of notifications
   - Delivery guarantees
   - User preferences
   - Scale and performance expectations
2. **High-Level Architecture**
   - Components: API, Notification Service, Channel Adapters, Database, Queue
   - Real-time vs scheduled flows
3. **Data Model**
   - User preferences
   - Notification templates
   - Delivery logs
4. **Message Delivery**
   - Use of message queues (e.g., Kafka, RabbitMQ)
   - Retry logic, dead-letter queues
   - Channel-specific adapters
5. **Scalability**
   - Horizontal scaling
   - Partitioning/sharding
   - Caching
6. **Reliability & Fault Tolerance**
   - Monitoring, alerting
   - Failover strategies
   - Idempotency
7. **User Preferences**
   - Opt-in/out management
   - Channel selection
8. **Interaction with Interviewer**
   - Ask clarifying questions
   - Discuss trade-offs (e.g., delivery guarantees vs latency)
   - Suggest improvements (e.g., analytics, A/B testing)

## Example Questions to Ask
- What channels are most important?
- Should notifications be real-time or can they be batched?
- How strict are delivery guarantees?
- How should user preferences be managed?
- What is the expected peak load?
