# 3 MONTHS REVIEW
@tada_network

+++
# OUR VALUES
1. Be Helpful
2. Deliver WOW!!
3. Embrace & Drive Change
4. Pursue Learning & Growth
5. Think & Act Like an Owner
6. Be Open, Honest & Constructive
---
# WHAT I DO ?
- Do programming / coding (absolutely)
- Dive into the current Projects to get understand the flow
- Code Refactor
- Thinking about the best practice & coding standard

---
## PROJECTS INVOLVED
1. LAZADA EGift Integration (Bridge) 
2. TADA.id improvements
3. AVBO Site
4. AVCorporate Site

---
## LAZADA EGift Integration (Bridge) 
1. Create simple API library
2. Solve the problem about LAZADA API request
3. Create order processing functions 
4. LOGGING & Tracking
5. Reports

---
## TADA.id improvements
1. Integrate egift purchase (evoucher)
2. Fix & Improvement on feature (cart, design, banner)
3. Code refactoring (cart)

---
## AVBO Site
1. Added feature for download batch of list program
2. Customize informations on order detail
3. Added filter by `orderReference` on order listing

---
## AVCorporate Site
1. Egift corporate invoice report 

---
## SUGGESTION
1. Follow coding standard for **JAVASCRIPT** [https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)

2. Use best practice for project development [https://github.com/wearehive/project-guidelines](https://github.com/wearehive/project-guidelines)

3. Create tada.id content management for product or operator

4. Communication over current state of development with product team(Trello card)

---
## SUGGESTION [2]
5. Keeping eye on current popular technologies (Elixir, Golang, React-Native, etc.)
6. Rebuild/Refactor Tada site to be more efficient, less redundant, less unknown side effect & messy code / functions
7. EGift Processor watcher (report & alert) & manager for LAZADA and next incoming aggregator 
6. Fun project for company benefit
    - Interesting news on LCD (tech, startup, market situation, our system, our transactions count, etc.)
    - Face recognition for employees
    - Anonymous submission for TADA products
    - Sexy rating on requested feature (related to point above) 
---
## Obstacles I Found
1. So many projects with different js framework and flow
2. No internal documentation of every project because we move so fast
3. Must read the code, don't believe in person (takes more time)
4. Technical feature explanations for some flow like redemption, egift, loyalty, etc.

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

+++
### LAZADA API [4]

`makeRequest()`

```
makeRequest(actionApi, params)
{
  let signatureParams = _.assign(this.getBasicParams(), params, { Action: actionApi.actionName });
  let signature = this.getSignature(signatureParams);
  let completeParams = _.assign(signatureParams, {
    Signature: signature
  });

  logger.info(`${LOGGER_PREFIX} REQUEST PARAMS: ${JSON.stringify(completeParams)} \r\n`);

  let responseHandler = (resolve, reject) => {

    return (error, response) => {
      if (error) {
        if (error.response) { 
          logger.error(`${LOGGER_PREFIX} ERROR RESPONSE: ${error.response.text}`);
          reject(JSON.parse(error.response.text)); 
        } else {
          logger.error(`${LOGGER_PREFIX} UNKNOWN ERROR`);
          reject(error);
        }
      } else {
        logger.info(`${LOGGER_PREFIX} RESPONSE: ${JSON.stringify(response.body)} \r\n`);
        resolve(response.body);
      }
    };

  };

  return new Promise((resolve, reject) => {
    // TODO: Log request param here
    if (actionApi.method == 'POST') {
      return request(actionApi.method, this.apiUrl)
        .timeout(this.apiTimeout)
        .send(completeParams)
        .end(responseHandler(resolve, reject));  
    } else {
      return request(actionApi.method, this.apiUrl)
        .timeout(this.apiTimeout)
        .query(completeParams)
        .end(responseHandler(resolve, reject));
    }
  });
}
```

+++
### LAZADA API [5]

`getOrders()`

```
getOrders(params)
{
  return this.makeRequest(ActionApi.GET_ORDERS, params);
}
```

+++
### LAZADA API [6]

`getOrders()`

```
getOrder(params)
{
  return this.makeRequest(ActionApi.GET_ORDER, params);
}
```

+++
### LAZADA API [7]

`getOrderItems()`

```
getOrderItems(params)
{
  return this.makeRequest(ActionApi.GET_ORDER_ITEMS, params);
}
```

+++
### LAZADA API [8]

`setStatusToPackedByMarketPlace()`

```
setStatusToPackedByMarketPlace(params)
{
  params.OrderItemIds = JSON.stringify(params.OrderItemIds);
  return this.makeRequest(ActionApi.SET_STATUS_TO_PACKED, params);
}
```

+++
### LAZADA API [9]

`setStatusToReadyToShip()`

```
setStatusToReadyToShip(params)
{
  params.OrderItemIds = JSON.stringify(params.OrderItemIds);
  return this.makeRequest(ActionApi.SET_STATUS_TO_READY_TO_SHIP, params);
}
```