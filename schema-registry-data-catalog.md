---
title: "How We Use Confluent Schema Registry with Our Data Catalog"
date: 2024-06-10
tags: [schema registry, confluent, data catalog, kafka, developer]
---

# How We Use Confluent Schema Registry with Our Data Catalog

## Overview

Our data platform combines the power of the Confluent Schema Registry with a custom-built data catalog. This approach gives developers a single source of truth for data definitions, seamless schema evolution, and easy integration with Kafka.

---

## What Is the Data Catalog?

- **Central Dictionary:** The data catalog is where we define all data entities, their fields, types, and descriptions.
- **Schema Mapping:** Each catalog entry is mapped to a schema (Avro, Protobuf, or JSON Schema) and registered in the Confluent Schema Registry.
- **Automatic Sync:** Changes in the catalog are automatically reflected in the registry.

---

## Typical Developer Workflow

### 1. Define or Update a Data Entity

- Use the data catalog UI or API to create or update a data entity.
- The system generates the corresponding schema.

![Creating a new data entity](images/create-entity.gif)
*Figure: Creating a new data entity in the data catalog UI.*

### 2. Schema Registration

- The schema is automatically registered or updated in the Confluent Schema Registry.
- Manual registration (rarely needed) can be done with:
  ```bash
  curl -X POST -H "Content-Type: application/vnd.schemaregistry.v1+json" \
    --data '{"schema": "{\"type\": \"record\", \"name\": \"User\", \"fields\": [{\"name\": \"id\", \"type\": \"string\"}] }"}' \
    http://<schema-registry-host>:8081/subjects/<subject-name>/versions
  ```

### 3. Discover and Use Schemas

- **Find schemas:** Search the data catalog for the entity name to see schema details and version history.

![Searching for a schema](images/search-schema.gif)
*Figure: Searching for a schema in the data catalog.*

- **Fetch from registry:** Use the API:
  ```bash
  curl http://<schema-registry-host>:8081/subjects/<subject-name>/versions/latest
  ```
- **Integrate:** Configure your Kafka client with the Schema Registry URL. Most clients fetch schemas automatically.

---

## Best Practices

- **Evolve schemas with care:** Use backward/forward compatible changes.
- **Consistent naming:** Follow naming conventions for subjects and fields.
- **Document changes:** Use the catalog to record major updates.

---

## Example: End-to-End

1. Define a new entity (e.g., `Order`) in the data catalog.
2. The schema is generated and registered.
3. Developers use the schema in Kafka producers/consumers.
4. Updates in the catalog trigger schema evolution and registry updates.

---

## Troubleshooting

- **Registration errors:** Check for incompatible changes or missing required fields.
- **Deserialization issues:** Ensure the client uses the correct registry URL and subject.
- **Finding schemas:** Always start with the data catalog.

---

## More Resources

- [Confluent Schema Registry Documentation](https://docs.confluent.io/platform/current/schema-registry/index.html)
- Internal data catalog documentation (link)

For help, contact the data platform team. 