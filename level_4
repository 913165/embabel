Great 👍 now we move to **Level 4 — Agent calling REST API**
This is where students start seeing **real enterprise value**.

---

# Level 4 — Agent calling REST API

## Use Case: Order Status Agent 📦

User asks:

> Where is my order 12345?

Agent will:

1. call REST API
2. get order status
3. generate human friendly response

---

# Architecture

```text
UserInput
   ↓
Agent
   ↓
Java Tool (REST API call)
   ↓
Order data
   ↓
LLM generates response
```

---

# Step 1 — Domain Model

### Order

```java
package com.example.agent.model;

public record Order(

    String orderId,

    String status,

    String expectedDeliveryDate

) {}
```

---

### OrderResponse

```java
package com.example.agent.model;

public record OrderResponse(

    String message

) {}
```

---

# Step 2 — REST Client Tool

Normal Spring service calling REST API.

Example API:

```
GET https://api.example.com/orders/{id}
```

---

### OrderService

```java
package com.example.agent.service;

import com.example.agent.model.Order;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class OrderService {

    private final RestTemplate restTemplate = new RestTemplate();

    public Order getOrder(String orderId) {

        // dummy example
        // replace with real API

        return new Order(
                orderId,
                "SHIPPED",
                "2026-04-10"
        );

        /*
        real API call example:

        return restTemplate.getForObject(
            "https://api.example.com/orders/" + orderId,
            Order.class
        );

        */
    }
}
```

---

# Step 3 — Agent

```java
package com.example.agent;

import com.embabel.agent.api.annotations.Agent;
import com.embabel.agent.api.annotations.Action;
import com.embabel.agent.api.annotations.AchievesGoal;
import com.embabel.common.ai.Ai;
import com.example.agent.model.Order;
import com.example.agent.model.OrderResponse;
import com.example.agent.service.OrderService;
import org.springframework.ai.chat.prompt.UserInput;

@Agent(description = "Check order status")
public class OrderStatusAgent {

    private final OrderService orderService;

    public OrderStatusAgent(OrderService orderService) {
        this.orderService = orderService;
    }


    @Action(description = "Fetch order details")
    public Order fetchOrder(UserInput input) {

        String orderId = input.getContent();

        return orderService.getOrder(orderId);
    }


    @AchievesGoal(description = "Generate order status response")
    @Action(description = "Create response")
    public OrderResponse generateResponse(
            Order order,
            Ai ai) {

        return ai
            .withDefaultLlm()
            .creating(OrderResponse.class)
            .fromPrompt("""
                Order details:

                Order Id: %s
                Status: %s
                Delivery date: %s

                Create friendly response message.
                """.formatted(
                    order.orderId(),
                    order.status(),
                    order.expectedDeliveryDate()
                ));
    }
}
```

---

# Step 4 — Example Run

Run:

```bash
x "12345"
```

---

# Example Output

```json
{
 "message":
 "Your order 12345 has been shipped and is expected to arrive on 10 April 2026."
}
```

---

# What students learn here

| Concept                 | Explanation               |
| ----------------------- | ------------------------- |
| agent calling java code | integrates business logic |
| REST API integration    | real world connectivity   |
| multi-step flow         | planner selects order     |
| structured output       | consistent response       |
| hybrid AI system        | AI + API                  |

---

# Flow understood by Embabel planner

```text
UserInput (orderId)
        ↓
fetchOrder()
        ↓
Order
        ↓
generateResponse()
        ↓
OrderResponse (GOAL)
```

---

# Real world extensions

can integrate:

* ecommerce API
* banking API
* shipment tracking API
* CRM API
* ticketing system API

---

# Next Level (5)

Agent with RAG:

Example:

agent searches company policy documents before answering.

---

If you want, I can also provide:

* Postman mock API
* JSON sample API response
* controller version instead of shell
* diagram of flow
* error handling example
* multiple API calls example
