
# Getting Started with Hypermedia APIs in Node.js: Implementing HAL and JSON:API
As an API engineer focused on building maintainable backends, at [Hybrid Web Agency](https://hybridwebagency.com/) I regularly take on self-directed challenges to expand my skills. A recent goal involved implementing emerging hypermedia standards from the ground up.

I set out to create a fully functional order API using Node.js that incorporated the HATEOAS style. To do so thoroughly, I would adhere to the HAL and JSON:API specifications through rigorous testing and experimentation.

Over six months of development, I constructed an order management system serving hypermedia-driven responses. Analyzing how each standard modeled relationships between resources uncovered valuable differences in their approaches.

In this article, I will discuss key learnings from that in-depth process. Code samples demonstrate representing interconnected entities as hyperlinked state.

By sharing insights from implementing these specifications end-to-end, my aim is to benefit fellow engineers evaluating architectural techniques. The exploration also strengthened my abilities for delivering robust, evolvable solutions.

It's through ambitious self-study experiences like this that I continually expand knowledge applied to client work. I remain dedicated to advancing both expertise and best practices in the field.





### What is HATEOAS?
HATEOAS, which stands for Hypermedia as the Engine of Application State, is an architectural style for designing RESTful web APIs. It builds on the idea that a client interacting with a REST API should not have prior knowledge about the structure of that API. Instead, all information needed for the next request should be contained or linked to in the response of the current one.

By including hypermedia links in API responses, HATEOAS enables APIs to be discoverable and explorable. Clients utilizing HATEOAS APIs can dynamically navigate the API by following links embedded in previous responses, without needing to be configured with a fixed set of valid entry points and operations. This allows the API to evolve independently of clients as new resources and relationships are added over time.

For example, a response to a request for an order resource may contain links to retrieve associated data like products, customers, or invoices. It allows flexible transition between resources rather than clients needing to hardcode specific sequences or endpoints. By leveraging hypermedia, HATEOAS aims to make APIs that are flexible, self-descriptive, and accommodating of change.





### Advantages of the Hypermedia-Driven Approach

By designing REST APIs around hypermedia as the representation and governor of application status, several pivotal advantages ensue:

**Conversation Self-Documentation** - Each response implicitly articulates valid transitions contextually through links, removing the need for supplemental specs. 

**Organic Expansion** - Services can unveil new functionality and refactor internally over time by augmenting link payloads, independent of clients.

**Evolution Transparency** - Modifying domain models through structural changes is seamlessly accommodated by all consumers via updated relational clues.

**Post-Launch Discovery** - Responses facilitate unveiling incremental capabilities post-release through an inside-out unfolding strategy.

**Exploratory Interactions** - Clients can uncover hidden options by dynamically traversing relational prompts within responses.

**Situational Adaptability** - HATEOAS permits clients to prioritize navigational paths specifically tailored to each unique runtime situation.

Fundamentally, prioritizing hypermedia as the representer and driver produces inherently self-explicating, long-lasting and malleable REST interfaces able to freely evolve and guide comprehension over time.




## Choosing a Specification - HAL vs JSON:API

Comparing the HAL and JSON:API specifications for representing hypermedia in Node.js APIs.

### HAL Specification

The Hypertext Application Language (HAL) is a simple format for hypermedia enabling the representation of application state through hyperlinks. A HAL representation contains a "_links" property linking the current resource to related other resources.

For example:

```json
{
  "_links": {
    "self": { "href": "/orders/123" }, 
    "orderItem": { "href": "/order/123/items" }
  }
}
```

HAL clearly exposes available transitions and is easily understandable by both machines and humans. However, it is quite basic and additional context data cannot be included.

### JSON:API Specification

JSON:API is a full specification that serves as a guide for building RESTful JSON APIs. It focuses on JSON serialization of resources and relationships between them.

Resources are connected through a dedicated "relationships" attribute listing related IDs and links to retrieve their associated data. Responses also adhere to consistent HTTP conventions.

For example, an Order may have a relationship to line Items:

```
"relationships": {
  "items": {
    "links": {
      "related": "/orders/1/relationships/items" 
    },
    "data": [
      {"id": "5", "type": "item"},
      {"id": "3", "type": "item"}
    ]
  }
}
```

JSON:API provides a richer standard but sacrifices simplicity. It requires more code/processing compared to basic HAL representations.

In summary, HAL best suits minimal hypermedia needs while JSON:API acts as a complete REST design framework. Their appropriateness depends on project scope and requirements.











## Developing a Linked API with HAL

This guide demonstrates how to build a RESTful API that represents relationships between resources using the Hypertext Application Language (HAL) specification with Express.

### Incorporating Dependency Injection 

The `express-hal-builder` module is installed to generate HAL compliant representations of linked resources.

### Modeling Core Data Entities

JavaScript object schemas define the structure of Order and OrderItem resources.

### Constructing HAL Responses

The builder facilitates linking entities by:

1. Representing a parent resource
2. Embedding references to associated child resources 

```js
app.get('/orders/:id', async (req, res) => {

  const order = await Order.findById(req.params.id);

  const builder = new HalBuilder();

  builder.representEntity(order)
    .childLink(order.items, '/orders/' + id + '/items');  

  res.json(builder.get());

});
```

This establishes the framework to develop a REST API connecting related data through hyperlinks as specified by the HAL standard.


## Consuming the HAL+JSON API 

Making requests to explore and discover the API using hypermedia links.

- Issue a GET to the base URL or an entity endpoint
- Parse the response to identify available link relations 
- Follow links by extracting URLs from _links properties
- Build UI/clients dynamically based on link discovery

## Implementing JSON:API in Node.js

Implementing a JSON:API specification-compliant API with Express:

### Installing Dependencies

Use the `jsonapi-serializer` module to generate responses.

```
npm install jsonapi-serializer
```

### Defining Resource Schemas

Create schema definitions for resources like Order and Item.

### Serializing Responses

Serialize related resources and include relationship links:

```js
app.get('/orders/:id', async (req, res) => {

  const order = await Order.findById(req.params.id);

  res.json(order.toJSONAPI({include: 'items'}));

});
```

This establishes a foundational JSON:API to develop linked RESTful services.

# Conclusion

In summary, following established API specifications provides structure, discoverability and future-proofing against changing needs. Aligning ensures intuitive services.

Developing complex, related backends per specifications demands diverse technical skills across domains. Partnering with proficient Node.js experts streamlines this.

As a full-service Node.js company located in Bakersfield, we offer [Node.js Development Services in Bakersfield](https://hybridwebagency.com/bakersfield-ca/custom-laravel-development-services/) to assist with building robust, specification-aligned APIs. Our team has deep experience implementing standards like HAL and JSON:API for relating resources. Whether prototypes, scaling or full projects, we deliver production-ready REST endpoints on time and on budget.

By capitalizing on our Node.js and framework expertise combined with architectural best practices, teams can prioritize core work while we ensure technical success meeting quality and performance standards. Contact us to discuss how our Bakersfield-based services can help expedite your goals.

## References

- https://www.halspec.org/
The official specification website for HAL (Hypertext Application Language), detailing the standard.

- https://jsonapi.org/
The official website for the JSON:API specification, including documentation on compliant API design. 

- https://expressjs.com/en/advanced/best-practice-performance.html
Best practices guide from the Express.js website covering performance, security, and standard compliance for Node.js APIs.  

- https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design
Documentation from Microsoft on API design best practices, including modeling relationships and using standards.

- https://swagger.io/resources/building-blocks/hypermedia/
Swagger overview of hypermedia APIs and industry standards like HAL and JSON:API for enabling discoverability.
