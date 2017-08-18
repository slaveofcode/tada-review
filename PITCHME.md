# 3 MONTHS REVIEW
@tada_network

+++
# WHAT I DO ?
- Mostly doing programming (absolutely)
- Dive into the current Projects to get understand the flow
- Code Refactor
- Thinking about the best practice & coding standard

---
## PROJECTS INVOLVED
1. LAZADA EGift Integration
2. TADA.id improvements
3. AVBO Site

---
## LAZADA EGift Integration
1. Create simple API library
2. Solve the problem about LAZADA API request
3. Create order processing functions 
4. LOGGING & Tracking
5. Reports

---
## TADA.id improvements
1. Integrate egift purchase
2. Fixing on feature
3. Code refactoring

---
## AVBO Site
1. Added feature for download batch of list program
2. Customize informations on order detail
3. Added filter by `orderReference` on order listing

## AVCorporate Site
1. Egift corporate invoice report 

---
## SUGGESTION
1. Follow coding standard for **JAVASCRIPT** 

[https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)

2. Use best practice for project development

[https://github.com/wearehive/project-guidelines](https://github.com/wearehive/project-guidelines)

3. Create tada.id content management for product or operator

4. Communication over current state of development with product team(Trello card)

---
## SUGGESTION [2]
5. Invest more on current popular technologies (Elixir, Golang, React-Native, etc.)
6. Rebuild Tada site to be more efficient, less redundant, less unknown side effect & messy code / functions
7. EGift Processor watcher (report) & manager for LAZADA and next incoming aggregator 
6. Fun project for company
    - Interesting news on TV (tech, startup, market situation, our system, etc.)
    - Face recognition for employees
    - Anonymous submission for TADA products
    - Sexy rating on requested feature

---
## CODE REVIEW LAZADA [1]
- LAZADA API: Create simple api library to talk with API service
- LAZADA ORDER PROCESSOR: Build up processing functions to handle order automatically

+++
## LAZADA API [2]
Api functions
- `getSignature()`
- `makeRequest()`
- `getOrders()`
- `getOrder()`
- `getOrderItems()`
- `setStatusToPackedByMarketPlace()`
- `setStatusToReadyToShip()`

+++
## LAZADA API [3]

`getSignature()`

```
getSignature(params)
{
  let encoded = [];
  _.forEach(this.sortByKeysAsc(params), function(item, key){
    encoded.push(encodeURIComponent(key) + '=' + encodeURIComponent(item));
  });
  let concatenated = _.join(encoded, '&');
  let signature = crypto
    .createHmac('sha256', this.apiKey)
    .update(concatenated)
    .digest('hex');
  signature = encodeURIComponent(signature);
  return signature;
}
```
@[3-16]
@[5-8]
@[9]
@[10-13]
@[14]
@[15]
