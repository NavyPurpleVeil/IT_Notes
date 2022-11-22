Http return codes use in REST apis?
----------------------------------
https://developer.mozilla.org/en-US/docs/Web/HTTP/Status


1xx
---

---

2xx
---
200 - confirms the succesfull operations;

---

3xx
---

---

4xx
---

400 - Bas request(invalid message body)
401 - Unauthorized (invalid login)
403 - Forbidden (failed login)

404 - Not Found (failed api request)
410 - Gone, Uncaches the response.

405 - (http) Method not Allowed

429 - Too many requests (api rate limit)

---