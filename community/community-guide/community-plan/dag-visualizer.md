---
description: Community project requirements
---

# DAG Visualizer

The purpose of a DAG Visualizer is to draw a visualization of an ever-growing DAG of blocks.

The DAG Visualizer should be able to receive an ongoing steam of blocks \(order of magnitude of 1 to 10 block per second\), each block having a pointer to a list of past parent blocks, and upon receiving each block, draw it.

The DAG Visualizer will come in handy for testing simulations and for helping newcomers understand the speed of Kaspa's underlying architecture.

![Origin block on the left, time moves from left to right, with new blocks forming on the right side](https://lh6.googleusercontent.com/W-v03qdqQp_1rQsHFz00A5p14z3Bklo3Ag09-a16aJNlXXpbOOEzhCdpTtnhROEO_A9e1TDghXRhTD21wVt4oO9lUhfezsGt6F8NQXSwzmWL-bvwvuMPEp4iPX5zn1U1CwFjHhwT)

### Challenges

Avoiding overburdening the user interface with drawing too many objects \(blocks and links\) when the DAG grows.

### Getting Started

To get a stream of blocks, you may use the [Kasparov API Server](../../../reference/kasparov-api-server/) and subscribe to block update notifications MQTT topic.

A usable package for the visualization is [d3js.org](https://d3js.org/) specifically something like Mike Bostock's [Build Your Own Graph!](https://bl.ocks.org/mbostock/929623)

{% hint style="info" %}
This is an independent project. If you are interested in doing this project, you can come talk to the community on Discord who can help get you started.
{% endhint %}

### Next Steps

Blocks have additional attributes that are worth visualizing, such as being a blue or a red block, being part of the selected parent chain, block byte-size, fee to transaction amount ratio, etc.

Blocks can be clicked to show included transactions.

