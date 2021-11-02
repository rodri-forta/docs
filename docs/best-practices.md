# Best Practices

This page describes some of the best practices observed for agent development.

## Return findings in a timely manner

Ensure that your agent returns findings in a timely manner. Agents are considered unresponsive by the scan node if they do not return findings within 5 seconds of a request. If the agent is unresponsive multiple times, it will result in the agent being stopped.

## Break down large agents into smaller files

Your agent may be looking for multiple conditions that you could write in a single file. We recommend keeping each condition in its own file. This will make testing your agent easier and keep the code more maintainable. You would then combine all the agents in the top-level entrypoint file (i.e. agent.js). See [here](https://github.com/forta-protocol/forta-agent-examples/tree/master/high-gas-js) for an example.

## Keep findings lean

There is a `metadata` field in the `Finding` object that you can use to store any extra information that is useful. Try to keep the data here as lean as possible i.e. don't throw the whole TransactionEvent into the metadata since that information is already available on Etherscan.

## Write unit tests

You should write and maintain unit tests for your agent. This will ensure a high quality bar and also allow you to test all edge-cases in your agent. When writing tests that involve log events, you can mock out the `filterLog` function instead of having to fiddle around with topics and signatures.

## Use the initialize handler

Your agent may need to do some initialization when it starts, for example, by fetching data from some external API. You should use the `initialize` handler function for such logic.

## Limit number of network calls

Your agent may need to make network calls to fetch data from external sources e.g. token prices. Be sure to make only the necessary network calls in order to respond in a timely manner. Another useful strategy for this could be to use caching.

## Use caching where possible

Caching is a great way to improve performance. If you need to store the result of a network call or some other calculation, try to use an in-memory cache.

## Use concurrency where possible

Try to make use of concurrency to maximize performance. For example, if firing multiple network calls to do some calculation, you can fire all the requests at the same time using something like `Promise.all` in Javascript.

## Don't include sensitive information

Be sure not to include sensitive information, such as API keys, in your code. Agent images are stored in a public repository where anyone can access and inspect the code.

## Beware of case-sensitivity

When comparing addresses in your code, be mindful of case-sensitivity. The SDK will return addresses in the BlockEvent and TransactionEvent as lowercase, but if you are comparing to a checksum address it will not be equal.