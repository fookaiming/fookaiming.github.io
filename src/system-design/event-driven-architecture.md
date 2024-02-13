# Event Driven Architecture

## What is Event Driven

> Distributed System that practise **_at least one_** the following pattern:
> - [Even Notification](#event-notification)
> - [Even Carried State Transfer](#event-carried-state-transfer)
> - [Event Sourcing](#event-sourcing)
> - [CQRS implemented with Event pattern](#command-query-responsibility-segregationcqrs)

## Event Notification

> events or changes in a system trigger actions to notify other systems of its change by sending an event object(
> asynchronously) as notification, rather than the synchronized request-response base model

- event can be simply as something changed(eg, address changed), the downstream will decide if any further action
  should be taken

### Pros

- loose coupling
    - indirect relationship
        - Unlike the request-response base model, where the sender relies on the response to proceed, event notification
          **_reverse the dependencies_**. The sender does not depend on the response; instead, it is the receiver that
          decides the following action.

- extensibility
    - new system can hook up to the system easily

### Cons

- Losing overall behavior
    - notification flow over entire system, there is no clear path to track what the system does when a change is made

### Anti-Pattern

- make event as command
    - event should be passive, command based event destroy the indirect relationship between sender and receiver

---

## Event Carried State Transfer

> A variant of event notification, where instead of sending a simple event, it carries the state that all downstream
> systems need.

### Pros

- reduce traffic
    - as state have sent within the event, not further communication is needed
- self autonomy
    - upstream and downstream are further decoupled, allowing downstream systems to continue processing even when the
      upstream system is temporarily unavailable
- easy bug fixes:
    - When a bug occurs in the upstream system, replying the data in the stream can trigger all the downstream systems
      to fix the data as well. This allows for easier bug fixes and ensures data consistency across the system

### Cons

- increase event size

### Pros

- Event versioning
    - migration is needed when the structure of event is changed
- data integrate
    - when multiple event carried same state, eg:(CustomerCreatedMessage and CustomerUpdatedMessage), both carried state
      of customer, the order of consumption must be guaranteed for the same customer

---

## Event Sourcing

> a design pattern that persisting all **_state changes_** as events (storing data as a stream of event), makes the
> event log becomes the principal source of truth

- need not be asynchronous
- event is immutable and always append to the event log
-

### Pros

- recoverable
    - easy to rebuild, just replay all events
- evidential and easy to reasoning
    - easy to audit
- rewind in time
    - easy to test out, eg: rewind in someday and test out the result of another data set
- analysis
    - the detailed changed log can be used for analysis purpose

### Cons

- performance
    - purely storing state as event is slow, as very read may trigger a chronological reduce
        - create snapshot from time to time
- large storage
- complex operation for event versioning, replay

---

## Command Query Responsibility Segregation(CQRS)

> separate data structure for reading and writing

- need not be implemented as event, but usually implement with the above patterns

### Pros

- optimize read model and write model separately
    - typical web application read much more than write

### Bear in mind

- Eventually Consistency
    - as the read and write operation is separate, the most updated data may not be present from read

# Ref

- [What do you mean by “Event-Driven”?](https://martinfowler.com/articles/201701-event-driven.html)
- [Event Sourcing and Event Storage with Apache Kafka](https://www.youtube.com/playlist?list=PLa7VYi0yPIH1TXGUoSUqXgPMD2SQXEXxj)