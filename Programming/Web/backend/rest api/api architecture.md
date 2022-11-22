Service
---

A much more monolithic design, can be a bit more purpose built, can be simpler in terms of initial complexity.
A monolithic service is as such that it's functionality is deeply integrated, and some functionality will not be possible to implement on the front-end side without extra changes to the backend.

---

Microservice
---

It's a paradgim of backend api design.
Microservices are self-contained, specific purpose modules, and can be used as part of a different project.

# For example:
Facebook has a microservice api for hosting files, which is independent from the api to make a post or send a message.

This api can not just potentially to be re-used amongst services, as it is detached from the post or message api functionality.

This means that if you were to add another option to facebook which requires this file hosting api, the levels of integration(and difficulty of thereof) required to make functionality happen drops dramatically.


A regular service can be refactored into a microservice, given enough time and resources.

---