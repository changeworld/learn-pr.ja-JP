Imagine a news story has just been published covering your organization's breakthrough cancer treatment. This is a terrific milestone, and will undoubtedly bring a large influx of traffic to your website. Will the website handle this traffic increase, or will the load cause the site to be slow or unresponsive?

Here, we'll look at some of the basic principles of ensuring outstanding application performance using scaling and optimization principles.

## What is scaling and performance optimization?

Scaling and performance optimization are about matching the resources available to an application with the demand it is receiving. Performance optimization includes scaling resources, identifying and optimizing potential bottlenecks, and optimizing your application code for peak performance.

### Scaling

Compute resources can be scaled in two different directions:

* Scaling *up* is the action of adding more resources to a single instance.
* Scaling *out* is the addition of instances.

![Scale up and scale out](../media-draft/scale-up-scale-out.png)

Scaling up is concerned with adding more resources, such as CPU or memory, to a single instance. This instance could be a virtual machine or a PaaS service. The act of adding more capacity to the instance increases the resources available to your application, but it does come with a limit. Virtual machines are limited to the capacity of the host they run on, and hosts themselves have physical limitations. Eventually, when you scale up an instance, you can run into these limits, restricting your ability to add further resources to the instance.

Scaling out is concerned with adding additional instances to a service. These can be virtual machines or PaaS services, but instead of adding more capacity by making a single instance more powerful, we add capacity by increasing the overall total number of instances. The advantage of scaling out is that you can conceivably scale out forever if you have more machines to add to the architecture. Scaling out requires some type of load distribution. This could be in the form of a load balancer distributing requests across available servers, or a service discovery mechanism for identifying active servers to send requests to.

In both cases, resources can be reduced, bringing cost optimization into the picture.

### Performance optimization

When optimizing for performance, you'll look at network and storage to ensure performance is acceptable. Both can impact the response time of your application. Selecting the right networking and storage technologies for your architecture will help you ensure you're providing the best experience for your consumers.

Performance optimization will also include understanding how the applications themselves are performing. Errors, poorly performing code, and bottlenecks in dependent systems can all be uncovered through an application performance management tool. Often, these issues may be hidden or obfuscated for end users, developers, and administrators, but can have adverse impact on the overall performance of your application.

## Scalability and performance best practices

Look across all layers of your application and identify and remediate performance bottlenecks in your application. These bottlenecks could be poor memory handling in your application, or even the process of adding indexes into your database. It can be an iterative process, as you may relieve one bottleneck and then uncover another that you were unaware of.

Using caching in your architecture can help improve performance. Caching is a mechanism to store frequently used data or assets (web pages, images) for faster retrieval. Caching can be used at different layers of your application. You can use caching between your application servers and a database, to decrease data retrieval times. You could also use caching between your end users and your web servers, placing static content closer to the user and decreasing the time it takes to return web pages to the end user. This also has a secondary effect of offloading requests from your database or web servers, increasing the performance for other requests.

It's not uncommon to see legacy applications built as one large application to do multiple tasks. These architectures are often limited to only scaling up, and are incapable of scaling out. In these types of architectures, look to modernize your application and decouple services from each other. Decoupling is the act of breaking apart the major functions of an application into separate applications. Once separated, each service could then be scaled-out and scaled-in as needed, independently of each other.

Along with decoupling, adding a messaging layer in between services can have a benefit to performance. Adding a messaging layer creates a buffer for requests between the services so that requests can continue to flow in without error if the application can’t keep up. As the application works through the requests, they will be answered in the order in which they were received.

Depending on the architecture, there's a wide array of places to look to optimize the performance of your application. You will need to evaluate the performance of your application, and look for areas that may not be performing optimally.
