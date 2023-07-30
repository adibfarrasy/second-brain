#systemdesign

# Steps

1. Define the functional requirements
2. Define the non-functional requirements

## Defining Functional Requirements

1. Write down the user journey. Note: do the user journey twice. One in a CRUD sense, and one in a “normal flow” sense.
2. Identify the important nouns
3. Start small: build the monolith first
4. Write the entity relationship diagrams within a single database
5. Split into microservices, communicating over pub/sub, owning one of the entity table

## Defining Non-Functional Requirements

1. Ask further clarifications regarding the systems for the following:
    1. Scalability
    2. Availability (as in the CAP theorem sense)
    3. Consistency (as in the CAP theorem sense)
    4. Resiliency
    5. Observability
    6. Security