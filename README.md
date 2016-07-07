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

Project Naming
==============
If your project hosts a public API, call it something-api.
If your project hosts an internal service, call it something-serv.
If your project hosts a web site, call it something-web.

Modern Service Infrastructure
=============================
In our cloud services, we use [Kubernetes](http://kubernetes.io) (k8s) to manage our cluster. This provides service
discovery, auto scaling and a host of other great features. Probably the most important point is that it manipulates
networking in such a way that talking to other services is as simple as using the service name and a well known port,
even if there are multiple instances of a container running on a host. So if you wan to call "auth-serv", which is a
secure swagger API, the URL inside your service is just https://auth-serv/, and k8s will make sure it's all hooked up
and load balanced etc. Right now we're running a single k8s cluster in AWS, but the hope is that over time we might
have multiple cloud providers in some sane way.

For development, we use docker-compose to try and emulate the main elements of k8s without having everyone
run their own k8s cluster.

Obviously we have a fairly large existing stack (mainly .NET/MSSQL) which has to figure into the puzzle as well,
and there isn't a detailed plan for how those two worlds will talk to each other yet. Typically we seem to be exporting
things out of SQL into some other read-only store (like Elastic Search) and then writing cloud services that hit that.

Authentication
==============
Front end services are in charge of authorization. Internal services should not perform additional authorization in general,
but assume that the front tier has decided whether or not the stated action is allowed. This provides a great deal of flexibility
in wiring together components. In practice this means that "actor" information is sent as headers or arguments to internal
services.

We use [Kong](https://getkong.org/) to do front end authentication. Kong manages all "auth tokens" for our public APIs whether
those are obtained by someone typing in a username and password, having a preshared key, or an OAuth flow. In development,
it likely makes sense to write tests that insert the things that Kong would have provided rather than going through Kong
in a test.