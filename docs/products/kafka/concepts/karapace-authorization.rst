Karapace authorization
=================

Karapace Schema Registry
------------------------

Note: Schema Registry authorization is only available in Karapace, not in Confluent Schema Registry.

Schema Registry authorization has been available in Aiven since MM-DD-YYYY. The Kafka services created after that date have it enabled and it's not possible to disable it. The services created before that have configuration option ``schema_registry_authz`` that controls whether authorization is enabled. The default value is false, so the authorization needs to be explicitly enabled. It's possible to toggle the feature on and off. Note if the service nodes need to be upgraded e.g. by running the latest maintenances in order to use the feature.

When calling to Schema Registry using the Service URI that can be seen e.g. in Aiven console, the HTTP call contains username and password. Schema Registry uses that username for authorization, granting or denying access and filtering content. These operations are controlled by Schema Registry ACL. ACL consists of entries that each specify a username, a resource and the operation. The operation is ``Read`` or ``Write``. ``Write`` implies ``Read``. The resource is of format ``Subject:subject_name`` or ``Config:``. In the first case, the entry controls access to Subjects, in the latter to the global compatibility config.

Note: Currently Schema Registry ACL is not visible in the Console.

Some APIs in Schema Registry use subject-based authorization to grant or deny access, e.g. creating a schema needs Write access to the subject and getting schemas requires Read access that subject. The ``subject_name`` can have wildcards "*" (matching any characters) and ? (matching a single character). Some APIs filter content based on the ACL. The user only can see the subjects he has ``Read`` access

The global compatibility APIs require ``Config:`` resource access with ``Read`` permission when getting the config and ``Write`` permission when setting it.

The user Aiven Console, Aiven Client and REST API use when working with Schema Registry is a special superuser that has write access to everything in Schema Registry. This means e.g. that in Console, all schemas can be seen, all schemas can be modified etc. This user and the ACL entries for it are not visible in Console.


Karapace Kafka REST
-------------------

In the near future Kafka REST also supports authorization.

When calling Kafka REST using HTTP the call has username and password. Kafka REST connects to Kafka and authenticates using the username and password it itself receives. Kafka REST itself doesn't authorize the HTTP requests it receives.

When working e.g. with Kafka topics, Kafka will apply the Kafka ACL to grant or deny access to topics.

