# MQTT Topics

## List of MQTT Topics

* [ ] Explanation what this is for
* [ ] Explanation about RabbitMQ and how to subscribe to topics

### **transactions/{address}**

Subscribing to this topic will send a notification of type Transaction, whenever a transaction for the given address is seen in a new block.

### **transactions/accepted/{address}**

Subscribing to this topic will send a notification of type Transaction, whenever a transaction for the given address is accepted.

### **transactions/unaccepted/{address}**

Subscribing to this topic will send a notification of type Transaction whenever a transaction for the given address is unaccepted. This can happen when there is a re-organization of the blocks in the DAG. In this case, a transaction that was previously by a specific block may become unaccepted from that block, and may or may not be accepted in a different block \(receivable in the _transaction/accepted_ topic\).

### **dag/selected-tip**

Subscribing to this topic will send a notification of type Block whenever the longest parent chain becomes longer, as a result of a new block extending it, or a re-organization.

