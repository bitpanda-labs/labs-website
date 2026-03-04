---
layout: experiment
title: "Smart Search"
date: 2026-03-02
categories: ["AI & Machine Learning", "User Experience"]
excerpt: "We redesigned our Search feature to handle real-world behaviors. The new Smart Search combines a scalable search engine with AI-powered features to deliver relevant results no matter how a query is expressed."
action: "Learn more"
---

Finding the right asset should be simple, fast, and accurate. In reality, search is messy. Users make typos, use informal names, or search using vague ideas instead of exact tickers.

We redesigned our Search feature to handle those real-world behaviors. The new Smart Search combines a scalable search engine with AI-powered features to deliver relevant results no matter how a query is expressed.

# Search Foundation

Smart Search's core is built entirely on AWS OpenSearch. All queries are executed against OpenSearch indexes containing asset names, symbols, tags, descriptions and metadata.

Results are ranked using a combination of exact matching, relevance scoring, and additional business logic. OpenSearch provides the core capabilities for text analysis, ranking, filtering, and high-performance querying, giving Smart Search a fast and reliable core.

All advanced features are built on top of this foundation, so search stays predictable and responsive even as new capabilities are added.

# Handling Imperfect Queries

Most searches are not perfect, and Smart Search is designed with that assumption.

### Fuzzy matching for typos

If a user types "bticoin", "etherium", or "solanna", Smart Search still returns Bitcoin, Ethereum, and Solana. Fuzzy matching uses edit distance logic to find close matches and ranks the most similar results highest.

Fuzzy logic is intentionally limited to asset names. It is not applied to symbols because fuzzy matching short symbols would introduce too many incorrect results.

### Compound name normalization

Asset names are automatically split based on capitalisation and common separators. A query for "EthereumPOW" is treated the same as "Ethereum POW", removing the need for users to guess formatting.

### Synonym support

Many assets are commonly known by names that differ from their official listing. Smart Search understands these relationships.

Examples:

- Facebook returns Meta
- Google returns Alphabet
- Binance Coin returns BNB

Synonyms are manually curated by our team to ensure that only correct and meaningful mappings exist.

### Tie-breaker ranking

When several assets match a query equally well, ranking is refined using market capitalization. This ensures that primary assets appear before related products, such as leveraged tokens. For example, ETH ranks above ETH 2L or ETH 1S because Ethereum has a clearly defined market cap, while derivative tokens do not.

### Direct identifier lookups

Smart Search supports direct searches using technical identifiers. You can find assets instantly by entering our platform UUID, an ISIN, or a WKN, and the system will return the exact matching result.

# AI-Powered Search

One of the biggest upgrades in Smart Search is semantic search, also referred to as neural search or AI search.

Instead of only matching keywords, semantic search understands the meaning behind a query. This allows you to search for concepts like "social media companies" or "artificial intelligence stocks" and still find relevant assets, even if those exact words never appear in their names.

Semantic understanding is powered by embeddings generated using models hosted on HuggingFace. We use an optimised model specifically chosen for real-time embeddings generation, ensuring that AI-powered results can be delivered fast enough for a real-time search experience.

To keep performance reliable, semantic search activates only for queries longer than three letters. If the embeddings provider takes longer than one second to respond, Smart Search automatically falls back to traditional results so that searches always remain fast.

# Discovery Through Tags

Tags provide another powerful way to discover assets beyond names and symbols.

Regular tags are multilingual and used across filters. They support fuzzy matching and help surface related assets. Examples include:

- proof of stake
- metaverse
- layer 1
- DeFi
- gaming

In addition to human-curated tags, we generate AI-powered tags using OpenAI models. These add descriptive context such as:

- cloud computing
- payment networks
- electric vehicles

AI tags expand discoverability but are intentionally not fuzzy matched to maintain precision. They are currently available in English only.

# Data Pipelines Behind the Scenes

A great search experience depends on great data.

Our OpenSearch indexes are populated through a hybrid pipeline:

- Critical asset updates flow in real time through Kafka topics.
- New assets, symbol updates, and availability changes appear in search almost instantly.
- Additional metadata is refreshed daily via cronjobs to keep descriptions, tags, and other metadata synchronised.

This approach gives us both immediacy and reliability, so search results are always accurate and current.

# Performance and Reliability

Search needs to be dependable above all else. Several mechanisms are used to achieve this:

### Strict time budgets

Semantic search requests have a hard timeout of one second. If embeddings are not returned within that limit, results are served without semantic ranking. The user never waits longer because of AI processing.

### Layered fallbacks

If the Smart Search service is unavailable, queries automatically revert to the previous, more simple search engine. If only semantic components fail, results are still ranked using all other mechanisms, minus the semantic ranking.

### Caching

Generated embeddings are cached aggressively in Redis to avoid repeated model calls. OpenSearch query results are cached using built-in query caching.

### Scalability

OpenSearch clusters are horizontally scalable and already handle tens of thousands of indexed documents and high query volumes with an average response time of ~170ms.

# Real-World Usage and Performance

Smart Search is already handling significant production traffic. Here are some interesting metrics:

Over the last two weeks, the service processed more than 12.8 million search requests, averaging around 913,000 searches per day. Typical traffic ranges between 1 and 5 requests per second, with peak periods reaching over 45 requests per second.

Caching embeddings plays a major role in keeping responses fast and infrastructure costs low. Redis caching currently delivers an 83 percent cache hit rate, meaning the vast majority of semantic searches are served instantly without needing to recompute the embeddings.

From a latency perspective, performance is consistently strong:

- Median latency (p50): 15 to 20 milliseconds
- High percentile latency (p99): 174 to 350 milliseconds

These numbers represent real end-to-end response times experienced by users in normal operating conditions.

Together, these metrics show that Smart Search is not just feature-rich, but also fast, scalable, and reliable under real production load.

# Final Thoughts

Smart Search brings together traditional keyword matching, AI-driven semantic understanding, and a robust AWS OpenSearch foundation into a single seamless experience.

Whether you search by ticker, nickname, concept, or with a simple typo, Smart Search is designed to guide you to the right asset quickly and effortlessly.

Smart Search is currently available in our mobile applications as well as the new web app.
