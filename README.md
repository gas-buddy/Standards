GasBuddy Standards
==================
This repo aims to provide a view into GasBuddy coding standards in the (many) languages and environments we use.
Given the age of the company and elements of the platform, it doesn't mean all our code follows these standards,
but we can hope, right?

- [C#](csharp/README.md) - not yet ready
- [Javascript](js/README.md)
- [Objective-C](objective-c/README.md)
- [SQL](sql/README.md) - not yet ready
- [Swagger](swagger/README.md)

Modern Service Infrastructure
=============================
To be decided this week. The main requirements are high availability, container management and service discovery.

The contenders are:
- [Kubernetes (mainly Ubernetes)](http://kubernetes.io/)/[etcd](https://coreos.com/etcd/docs/latest/)
- [CloudFormation](https://aws.amazon.com/cloudformation/)/[Consul](https://www.consul.io/)
- [Docker Swarm](https://docs.docker.com/swarm/)/[Consul](https://www.consul.io/)

Obviously we have a fairly large existing stack (mainly .NET/MSSQL) which has to figure into the puzzle as well.