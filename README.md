[![Build Status](https://travis-ci.org/microservices-demo/microservices-demo.svg?branch=master)](https://travis-ci.org/microservices-demo/microservices-demo)

# Sock Shop: A Microservice Demo Application

The application is the user-facing part of an online shop that sells socks. It is intended to aid the demonstration and testing of microservice and cloud native technologies.

It is built using [Spring Boot](http://projects.spring.io/spring-boot/), [Go kit](http://gokit.io) and [Node.js](https://nodejs.org/) and is packaged in Docker containers.

You can read more about the [application design](./internal-docs/design.md).

## Deployment Platforms

The [deploy folder](./deploy/) contains scripts and instructions to provision the application onto your favourite platform.

Please let us know if there is a platform that you would like to see supported.

## Bugs, Feature Requests and Contributing

We'd love to see community contributions. We like to keep it simple and use Github issues to track bugs and feature requests and pull requests to manage contributions. See the [contribution information](.github/CONTRIBUTING.md) for more information.

## Visualizing the application

Use [Weave Scope](http://weave.works/products/weave-scope/) or [Weave Cloud](http://cloud.weave.works/) to visualize the application once it's running in the selected [target platform](./deploy/).

![Sock Shop in Weave Scope](https://github.com/microservices-demo/microservices-demo.github.io/raw/master/assets/sockshop-scope.png)

## Installing and running the demo
- Install minikube
- Install kubectl

```
$ git clone https://github.com/microservices-demo/microservices-demo
$ cd microservices-demo/
$ minikube start --memory 8192
$ minikube delete
$ minikube config set memory 6144
$ minikube start
$ minikube ssh
$ kubectl create -f deploy/kubernetes/manifests-logging
$ kubectl create -f deploy/kubernetes/manifests/sock-shop-ns.yaml -f deploy/kubernetes/manifests
$ watch kubectl get pods --namespace="sock-shop"
```

# Gatling: A Load Testing Tool

Gatling is used to load test the demo microservices. Cloned from TassSinclair's performance and chaos repo.

## Installing and running the tool against Sock Shop

- Run Docker daemon
```
$ docker pull denvazh/gatling:2.3.1
```
- Create/put simulation script(s) in gatling/user-files/simulations
```
$ docker run -it --rm \
  > -v "$(pwd)"/gatling/conf:/opt/gatling/conf \
  > -v "$(pwd)"/gatling/user-files:/opt/gatling/user-files \
  > -v "$(pwd)"/gatling/results:/opt/gatling/results/ \
  > -e JAVA_OPTS="-Dusers=500 -Dduration=30 -Dhost=http://192.168.99.100:30001" \
  > denvazh/gatling:2.3.1 \
  > -s GatlingSimulation
```
- Open the report from gatling/results/gatlingsimulation-[randomly generated number]/index.html

## Notes about Gatling scripts
- I assume /user-files is where docker find the Gatling simulations
- Again, assuming, /results is where docker puts the results of the load tests
- GatlingSimulation is the class inside gatling/user-files/simulations/Simulation.scala
- -Dusers is how many users are simulated?
- -Dduration; not sure if in seconds?
- -Dhost is the address where Sock Shop microservices are running
- TO-DO: Find out what -v, -e, and -s mean
