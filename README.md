GasBuddy Standards
==================
This repo aims to provide a view into GasBuddy coding standards in the (many) languages and environments we use.
Given the age of the company and elements of the platform, it doesn't mean all our code follows these standards,
but we can hope, right?

- [C#](csharp/README.md)
- [Javascript](js/README.md)
- [Objective-C](objective-c/README.md)
- [SQL](sql/README.md)
- [Swagger](swagger/README.md)

Modern Service Infrastructure
=============================
To be decided this week. The main requirements are high availability, container management and service discovery.

The contenders are:
- Ubernetes/etcd
- CloudFormation/Consul
- Docker Swarm/Consul

Obviously we have a fairly large existing stack (mainly .NET/MSSQL) which has to figure into the puzzle as well.