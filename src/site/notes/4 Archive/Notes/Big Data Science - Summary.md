---
{"dg-publish":true,"permalink":"/4-archive/notes/big-data-science-summary/"}
---

tags:: #output/essay  [[Big Data\|Big Data]] [[3 Resources/Programming\|Programming]]

This is a very dense summary of the topic of _Big Data Science_, i.e., the intersection of Big Data and Data Science. It's my summary for the course with the same name I followed at Ghent University in 2018.

If you're new to the topic and adventurous or interested in what you might not know yet, you can use this to get an idea of the topic and use it as starting point for further research.

## Introduction

Data Science

the science of how to extract insight from data

Big Data

data a statistician sniffs at

The approaches taken in Big Data versus statistics or classical AI are quite different:

<table> <colgroup> <col class="org-left" />

<col class="org-left" />

<col class="org-left" /> </colgroup> <thead> <tr> <th scope="col" class="org-left">Statistics</th> <th scope="col" class="org-left">Classical AI</th> <th scope="col" class="org-left">Big Data</th> </tr> </thead> <tbody> <tr> <td class="org-left">Hypothesis-driven</td> <td class="org-left">Knowledge-driven</td> <td class="org-left">Data-driven</td> </tr>

<tr> <td class="org-left">Careful data gathering</td> <td class="org-left">Expert knowledge + logic</td> <td class="org-left">Get all the data first, figure it out later</td> </tr>

</tbody> </table>

The four V's of Data Science:

-   Volume
    
-   Variety
    
-   Velocity
    
-   Veracity
    

Lack of Veracity is the biggest problem. A lot of time is still spent on cleaning data and you don't want to do that.

_Data Mining_ is beating the data until it confesses. So you still need statistical verification tests to make sure your conclusions are significant.

## Big Data Management

### The Old Model

Data is stored in a store-only system, often geared towards archival so not very fast. In order to do something with the data, you'd need to build an ETL process (Extract, Transform and Load) that fetches the data you need and stores it in a RDB (Relational DataBase), i.e., a very structured data format. Then, you can perform queries and have views on that data.

The problem here is that once you have a decent amount of data, the ETL process is _just too slow_. It tends to run overnight (so there's the daily delay too) but you'll soon get to a point where the ETL process takes too long to complete in order for you to do something with it, before you need to refresh the data.

### Hello, Hadoop

Hadoop turned this process on its head. These are the principles introduced by Hadoop:

_Just store it._ Don't worry about data structure. You can store anything in the Hadoop File System.

_Scale horizontally._ Rather than having big, expensive, single servers, Hadoop works with a large amount of commodity hardware. Failures and distributing work are handled automatically.

_Move the processing to the data._ Hadoop clusters both store data and perform computations. By making calculations without needing to send all the raw data over the network first, you'll get incredible speedups. Note that because of the horizontal scaling, this works massively parallel.

In Hadoop architecture, the following types of nodes are present:

NameNode

keeps track of the location of files (which node has which file/block?).

JobTracker

schedules tasks across the cluster.

Secondary NameNode

handles as backup in case the main NameNode fails.

DataNode

stores data. All data is duplicated three times.

TaskTracker

tracks task completion and sends statistics to the JobTracker.

Note that the NameNode might be a bottleneck (only that node knows where the files are). This is avoided by keeping the amount of requests to the NameNode to a minimum. When a task is launched, the _Job_ tracker figures out where the data is and sends that to all Task trackers who need to use the data.

Hadoop introduced the _MapReduce_ programming model. Here, the framework handles data partitioning, dynamic scheduling, node failure and inter-machine communication. The programmer is only concerned with writing a _map_ function and a _reduce_ function.

The fist version of Hadoop did not allow for an ecosystem by enabling developers to expand its internals, so the ecosystem consisted of code generators. Examples are Pig Latin which is a simpler API and Hive which generates MapReduce code from SQL queries.

Hadoop V2 introduced Yarn, with which _Applications_ can run on the cluster. The _Application Master_ runs on the master node and applications itself in _containers_ in the nodes. Examples of Yarn applications are:

Oozie

manages complex ETL workloads with dependencies between them

SQOOP

allows back-and-forth transfer of data with relational databases

Flume

allows continuous log ingestion: a source collects logs, the channel stores and buffers, a sink extracts and forwards the processed data (e.g., on the HDFS – Hadoop File System)

Kafka

is a message broker and stream processor (think pub/sub)

### Set a Spark to it

Hadoop starts up a Java Virtual Machine for each job, which is slow AF. Think of Hadoop like a (manual transmission) diesel: it's a car all right (we used to have horses, remember) but is not straightforward to operate and accelerates slowly.

Spark solved these problems, with the following design goals:

-   Low latency but still have fault tolerancy and scalability
    
-   Generality and simplicity: MapReduce is a constraint; Spark allows you to do things with less lines of code, you can batch and stream
    
-   Ability to use the _memory_ instead of writing to disk every time (**this is a big one**)
    

Instead of executing immediately, Spark only makes calculations when you really need them. This way, a _lineage graph_ is built so that the framework can look at all your calculations simultaneously and make optimizations for you. This also allows Spark to re-compute parts of the graph if anything failed.

At the front-end, Spark has a few options built-in: Spark SQL, Spark Streaming, MLLib and GraphX.

In Spark v1, the main object type for data manipulation was the RDD (Resilient Distributed Dataset). These are immutable so very useful in cyclic workflows like in machine learning, where you re-use data. If it's not available anymore (e.g., the memory was full) RDD's can just be recomputed. RDD objects are processed by the DAG scheduler (Directed Acyclic Graph), then sent to the task scheduler which assigns tasks to workers.

An RDD consists of:

-   A list of partitions it's associated with
    
-   Its dependencies
    
-   Its preferred locations (where do I want to be for fast computation)
    
-   Partitioning info
    

Since Spark v2, the main type is the Dataset, which is easier to use (similar API to Pandas) and has better optimizations. The Dataset uses named fields, while an RDD is an object or a key-value pair. Maybe even more important, the Dataset has performance that is independent of the client's programming language.

The core of spark is composed of:

-   The Spark Context which lives on the client
    
-   The Cluster Manager which starts and manages workers
    
-   The workers who live in Spark Executors. Executors are long-lived JVMs and assign a thread per task.
    

A few more advanced concepts:

Accumulator variables

are shared variables which are write-only for workers.

Broadcast variables

are read-only variables that are cached on all nodes (good for sharing a big dataset among all nodes).

Partitioning

can be done manually if you re-use a dataset multiple times and your data is key-value formatted.

### Stream Management

Traditional big data solutions focus on _batch_ work: a lot of work is performed at once. With stream management, you want to process high-velocity data as it comes in (for example, website popularity or log analysis).

Use an _event hub_ to ingest your streaming data. This buffers the data and sends it out to everyone that needs it: file systems for permanent storage and stream-managing applications. Kafka is the big player here.

Note that _stream processing_ is not the same as _real-time processing_. The second one involves tight restrictions on processing speed: a result in a small time frame is guaranteed. Stream processing does not have these guarantees; it'll be a bit slower but a lot simpler and more powerful.

We define a couple of types of stream management:

SEP

Simple Event Processing. Processes single events. For example, filtering, routing, splitting. For example, detecting error states in a log.

ESP

Event Stream Processing. Acts on streams. Looks at ordinary data as well. For example, aggregations on order data.

CEP

Complex Event Processing. Can look at multiple events / event streams simultaneously. Will compute statistics, pattern detection, joins on data and might introduce new events.

Let's introduce a few concepts.

**Streaming Models.** How do you interpret "streaming"?

Continuous Streaming

process events instantly. Low-latency but lower throughput. Expensive to implement fault tolerance.

Mini-batch Streaming

process events a couple at a time. Higher latency but higher throughput. Easier to implement fault tolerance.

**Delivery guarantees.** In a streaming system, errors in message delivery _will_ occur. What kind of guarantees do you need?

-   At most once.
    
-   At least once.
    
-   Exactly once. This can be implemented as (a) at least once with duplicate filtering or (b) with checkpoints, where the entire system's status is rolled back in the event of failure.
    

**Backpressure.** When too much events come in for the processing system to handle them, the "pipes" (buffers) start to fill up and at some point, events will start leaking out and data will be lost. How full the pipes are is what we call the backpressure.

**The Messaging Tier.** The component of the architecture between the collection and analysis tiers. This performs message routing and buffers messages as needed. Allows you to decouple collection and processing. Additionally, the messaging system can store data to the file system for persistent storage and handle consumer (e.g., the analytics tier) failure.

**Statefullness.** Some event processing systems are _stateful_ because they need to keep data on their own (e.g., counting number of events). Others like filters are _stateless_. Usually you'll have a distributed store that keeps the processors' state in check. This might be remote (on a different system) or on the system itself (local; e.g., when you split the distributed store in the same way as the processing).

**Time and ordering.** Events can get delayed everywhere so it's likely that they arrive somewhere out of order. The _event time_ is different from the _stream time_. Usually, you'd buffer for a certain amount of time, sort in that buffer and then let it all through. In practice, this is done with _watermarking_ which is a bit more complex; it adds a limit to how long ago data can arrive out-of-order.

**Time window policies.** The _trigger policy_ defines when data should start being processed, while the _eviction policy_ determines when data should leave the window.

A sliding window triggers on the interval time and evicts based on the window length.

A tumbling window can trigger based on time and on the number of events. It evicts on the window length, too.

### Unified Log Processing

In unified log processing, there is a central _Enterprise Event Bus_ that aggregates and distributes its inputs to the processors. The contents in the EEP is what we'd call the "unified log." This are the properties of the unified log:

Unified

it's one technology for all the events

Append-only

events are appended and immutable, deleted when they reach the end of the window

Distributed

the unified log lives on a distributed, sharded, replicated platform

Ordered

every event has a unique offset (within a shard); there's no global state

What you need to know about processing unified logs with Kafka:

-   Awesome scaling capabilities
    
-   You can add ad-hoc consumers and batch consumers
    
-   Automatic recovery of broker failure
    
-   You need custom code, it's not an end-to-end solution and there aren't many libraries to help you
    
-   Kafka doesn't transform data
    

This is how it works:

-   Kafka runs on nodes ("brokers").
    
-   Messages are organized into _topics_ which are replicated and distributed.
    
-   In the replication, only one broker per partition is the master and used for reading.
    
-   Producers push, consumers pull.
    
-   A producer always writes at the end, consumer start reading at a specified offset. The "zookeeper" keeps track of these offsets.
    

### New architectures

**Structured Streaming.** This uses the Dataset API for both batch and streaming operations. As this organizes data into columns, it's highly structured. Fast, fault-tolerant, exactly-once stateful stream processing…except you don't have to reason about streaming.

**The Lambda architecture.** Big Data systems, especially when you add streaming, are getting too complex. In the lambda architecture, you pre-compute _views_ on the raw data in batch, then add in the latest results with some stream processing library. You can then query those views very quickly. Changing data is not allowed, you can only create and read. Instead of deleting, just exclude data from the view—you might mess up and destroy valuable data otherwise. Just keep the data. Keep the data separate from the queries. Kappa allows you to implement it with a _single_ code base that handles the batching and the streaming.

## Algorithms

### Information Retrieval

In order to find documents, you need an _inverted index_: a data structure that maps queries to documents. These documents are sorted by their ID.

An information retrieval system performs these steps:

1.  Grab document lists for each term
    
2.  Combine the lists and rank their items
    
3.  Extract document snippets and return results
    

#### Crawling

A basic crawler follows links on a page (add links on a page to the queue) and processes and stores page data. Challenges are: etiquette (don't DoS every website), distributing computation, detecting (near) duplicates, supporting multiple languages.

#### Creating the Database

You'll need to run the document processing in parallel, so use MapReduce (Hadoop was made for these kinds of workloads). The mapping step is just building the inverted index for each of the worker's documents. Then, you give it a composite key `(term, document_id)` and use the following rules:

-   Partition by term only
    
-   Sort by term and document ID
    

This way, the framework sorts by document ID instead of making the reducer do this (which would only work if all the document payloads for the term could fit in memory). The reducer keeps track (internal state) of all the documents encountered and flushes this list when he encounters a new term.

#### Compression

_Use D-gaps._ Since the list of documents is stored in increasing order, you can store the _difference_ between these IDs so that the numbers are a lot smaller. Now we just need to figure out how to make these actually use less bits, too.

_Variable-length integer coding._ (varInt) The first bit of a byte is the "continuation" bit: if it's 0, the number extends beyond this byte. This is quite slow because there will be many mispredicted branches.

_Group varInt._ (by Jeff Dean) Integers are stored per four. The first byte has 2 bits to denote the length (in number of bytes) for each of the four following integers. Processing this is a lot faster: you can use a lookup table (per four numbers, of course).

_Simple-9._ (word-aligned) Encode per word (32 bits). The first four bits denote how many numbers there are, the other 28 encode those numbers.

_Prefix codes._ (bit-aligned) Truly variable-length encoding.

Note that processing speed goes down as numbers are compressed more strongly.

#### Matching

To store the data, you can either split by row (by term) or by column (by document). Per document is faster: all nodes need to work for every query but you avoid hot spots and bottlenecks. These nodes can work in parallel.

TF-IDF (Term Frequency - Inverse Document Frequency) is the basic query score metric.

First, normalize your words: remove stop words, make it all lowercase, apply stemming, try to fix spelling mistakes.

Then,

$$ R = (K + (1 - K) \frac{f_{t,d}}{\max_\mathrm{word} f_{\mathrm{word},d}}) \log\frac{N}{|\{d \in D: t \in d\}|} $$

In order to rank the results, interpret the query as a mini-document, compute its TF-IDF scores and compute the cosine similarity (for example) between documents and query.

### PageRank

Main idea: build a flow graph where each node distributes its score along its outgoing links. The score of a node is its PageRank. To solve the flow equations, either do it analytically (add a normalization constraint for it to have a single solution) or better, use the power iteration. In per-node form, the update rule is:

$$ r_j \leftarrow \sum_{i \rightarrow j} \frac{r_i}{d_i} $$

In matrix form:

$$ \vec{r} \leftarrow M \vec{r} $$

You can also interpret the rank as the probability that a random walker is at a given node.

With this basic formulation, there are two challenges, though:

-   Spider traps: network structures where the rank stays trapped
    
-   Dead ends: structures where the importance leaks out
    

Both of these challenges are addressed by introducing _teleports_: with a probability $1 - \beta$, the random walker jumps to a random location in the graph.

This addition makes the transition matrix dense which is a problem for storage. However, most elements are just $1/N$ so you can subtract that from the value and add it later, when you fetched something from the matrix.

To prevent leakage, normalize the matrix in every step: calculate the difference of its total weight and distribute that along every element.

Paralellizing computation is simply by performing block matrix multiplication and distributing that along workers.

**Topic-specific rank.** Use a biased teleport set, that teleports within the set of pages of which you know they belong to a topic. That way you get a score within that topic.

**Preventing spam.** Use TrustRank: create a _virtual topic_ that represents "trust". Then, the spam mass $S$ of a page is the following, where $r$ is the PageRank and $t$ the TrustRank.

$$ S = \frac{r - t}{r} $$

You can eliminate pages with a high spam mass (which means almost all of their PageRank comes from non-trusted websites0.

### Online ads: AdWords

The mathematical problem we're trying to solve here is _bipartite matching_. You have a graph with two sets of nodes. Within a set, nodes are not connected; between the sets, as much connections as possible are present. Basically, you want to match each ad to a (good) search query.

In an offline situation, you can use the Hopcroft-Karp algorithm (iteratively find an augmenting path and swap the connected/non-connected parts). In an online situation you'll have to resort to a greedy algorithm, because you only get the preferences of one node at a time. You cannot change choices you made later.

To evaluate online algorithms, we define the _competitive ratio_ as

$$ c = \min_\text{possible inputs} \frac{|M_\text{greedy}|}{|M_\text{optimal}|} $$

This will be larger than 0.5 and you want it as close as possible to 1.

Okay, AdWords. You (as Google) want to maximize your revenue. Maximizing the expected revenue is naive: it favors those with a high CTR (Click-Through Rate) but the only way to measure that is to try placing the ad. CTR depends _a lot_ on where it's placed and when it's shown, so this is a difficult number to optimize.

You also know that each advertiser has a limited budget. So you want to deplete the budget of every advertiser. The balance algorithm by AdWords is basically this: _pick the advertiser with the largest unspent budget._ (assuming all bids are equal) This nicely gives every advertiser a chance to be shown and depletes everyone's budget. The competitive ratio is $1 - 1/e$.

To generalize this to non-equal bids, the _fraction of_ remaining budget is more important; and add a bias towards larger bids. Otherwise, advertisers with a large budget and very small bid are always shown. More specifically, with $x_i$ the bid and $f_i$ the fraction of budget left, the ranking $\psi_i$ is

$$ \psi_i = x_i (1 - e^{-f_i}). $$

With this formulation, the competitive ratio is still $1-1/e$.

### Recommending content

The ideal result of a recommender system is that it would recommend items from the _long tail_: items that are not popular, yet highly valued by the individual.

**Content-Based Recommendations.** Create a feature vector for each item (properties you need to know beforehand). You can similarly compute such a vector for users, by multiplying their rating for each item with the item vector and combining that (normalized). The cosine similarity then is your measure for "compatibility." Note that the _utility matrix_ (the matrix that stores the ratings) is very sparse.

Content-based systems suffer from the _cold start_ problem (you need to build a profile before you can start recommending items), but you need to add all the features yourself. Neither is there a _first-rater_ problem: you can recommend items that are yet unrated. However, these systems tend to have low serendipity (surprising results).

**Collaborative Filtering.** To recommend items to a user, (1) find a number of its _neighbors_: users the most similar ratings (use Pearson correlation or normalized cosine similarity). Then, (2) find items they all rate highly, rank those and return the results. So the predicted rating is the rating of a user's neighbors, weighted by the similarity the user has with that neighbor. This is _user-user_ collaborative filtering.

_User-user_ collaborative filtering is very computationally expensive, since it needs to search through all users for every rating. User preferences tend to change, however, item similarities are much more consistent. Also, there are likely more users than items. So you can swap the order and perform _item-item_ collaborative filtering: (1) find similar items by weighting them with how similar user ratings are, (2) weigh those item similarities by the user's preferences to predict a rating.

Item-item filtering can perform better because a user typically has multiple tastes and so items have simpler "type of similarity" than users. You can precompute the item-item similarity matrix because item similarities don't change that quickly. However, item-item filtering tends to return too similar results (exactly because it doesn't use people's complex tastes as extensively).

### Stream Mining

Challenges when looking at streams:

-   The dataset is not known in advance
    
-   There's an infinite amount of data
    
-   The data is _non-stationary_, i.e. the distribution changes
    
-   The input rate is controlled externally
    

This is what we want to do: make calculations on a stream when our memory is limited beyond what we'd need to perform the calculations in a naive way.

**Fixed-proportion sampling.** You want to keep a representative set but subsample the data. This method still results in potentially infinite storage but reduces the amount. What you need to know: don't randomly pick items, but pick a subset based on the _key_. How you determine the key depends on the queries you'd expect. For example, when counting amount of duplicate search queries per used, sub-sample the _users_ instead of the _queries_.

**Fixed-size sampling.** Use reservoir sampling: say you want to keep $s$ elements, each sampled from the total amount of elements until now $n-1$. To add the $n$'th element, keep it with probability $s/n$ and if you want to keep it, replace one of the existing elements, uniformly random.

**Counting bits.** Use the DGIM algorithm. Summarize in buckets which increase exponentially in size. Here, "size" is defined as "number of 1s". A bucket is defined by its starting time (number of 1s on the most recent side) and its size. All sizes are stored modulo $s$, the window size. When adding a 1, create a new bucket and combine buckets upstream so that there's only one or two of each size. This method needs $\mathrm{O}(\log s)$ bits for the stream.

To count the number of 1s until $k < s$ ago, sum the bucket sizes fully after $k$ and add half the size of the partial bucket. The maximum error is half of the largest bucket size or 50% of the real value.

**Bloom filtering.** Suppose you have a set of $m$ values $s \in S$ that you want to keep in the stream and $n$ bits storage. $n$ should be larger than $m$. Have a set of $k$ independent hash functions $h_i: S \rightarrow [1:n]$. For all $s \in S$, for all hash functions, set the bit at the position of the hash value to 1.

To filter, run the $k$ hash functions and verify whether the value in the bit array is 1 everywhere. All positives will be let through but there will be false positives. The probability of a false positive is $1 - (1 - 1/n)^m = 1 - (1 - 1/n)^{nm/n} \approx 1 - e^{-m/n}$. For $k$ hash functions, the probability of a false positive is:

$$ (1 - e^{km/n})^k $$

**Counting item frequencies.** Use the Count-Min Sketch method: store a matrix with $k$ hash functions in the rows and $w$ columns, the possible outputs for the hash functions. When a new item enters, add 1 in every row to the column corresponding to the hash value (see how embarassingly parallel and distributable this is?). To count an element, take the minimum value over all rows.

**Counting distinct elements.** Use the Flajolet-Martin approach: keep track of the maximum number of 0 bits at the end of the hash value of every element. Say this maximum is $R$. Then, the estimated number of unique items seen is $2^R$. Note how imprecise this is, so use multiple hash functions, then group them and take averages within-group; take the median value of all groups. Note that $\mathbb{E}[2^R] = \infty$.

### Large-scale Machine Learning

**The statistical formulation.** You have a set of hypotheses $\mathcal{H}$ of which you want to pick the one with the smallest risk $R$. The risk is defined as the expected loss $L$ over the input distribution:

$$ L: \mathcal{H} \times X \times Y \rightarrow \mathbb{R}: (f; \vec{x}, y) \mapsto L(f, \vec{x}, y) $$

$$ R: \mathcal{H} \rightarrow \mathbb{R}: f \mapsto R(f) = \mathbb{E}_{(\vec{x}, y) \in X \times Y} [L(f; \vec{x}, y)] $$

Since the input distribution is not known beforehand, we have to estimate this. This is called _Empirical Risk Minimization_ (ERM):

$$ \hat{f} = \min_{f \in \mathcal{H}} \hat{R}(f), $$

where $\hat{R}: \mathcal{H} \rightarrow \mathbb{R}$ calculates the mean risk for a function over all observed inputs.

So machine learning covers two areas:

Optimization

finding $f$ and knowing how that depends on $\mathcal{H}$

Statistics

figuring out what we can infer about $R$ by observing $\hat{R}$ and knowing how this depends on $\mathcal{H}$

**Statistical Learning Theory.** The SLT Theory defines a bound on the error we can make with $\hat{R}$. With $\mathcal{C}_\mathcal{H}$ the _capacity_ of $\mathcal{H}$ (some measure of the amount of possible functions),

$$ \Pr(\exists f \in \mathcal{H}: R(f) > \hat{R}(f) + \epsilon) \leq \mathcal{C}_\mathcal{H} \delta(\epsilon) $$

**Structural Risk Minimization.** SRM restricts the space of possible functions by imposing a penalty for the complexity of $f$. We'll mainly refer to this as _regularization_. With SRM,

$$ \hat{f} = \min_{f \in \mathcal{H}} \hat{R}(f) + \gamma h(f). $$

**Regression.** $\hat{y} = \vec{x}^T \vec{w}$. With Ordinary Least Squares (OLS), the loss function is the squared difference $(\hat{y} - y)^2$. Then, the expected risk is:

$$ \hat{R}(\vec{w}) = \frac{1}{n} (X\vec{w} - \vec{y})^T (X\vec{w} - \vec{y}), $$

where $X$ is the augmented feature matrix (has an extra column of 1's) and the last item of $\vec{w}$ is the bias $b$.

**Ridge Regression.** Applies L2-regularization as SRM to regression.

**Least Absolute Shrinkage and Selection Operator.** Applies L1-regularization as SRM. This favors sparse solutions but has no analytical solution.

**Classification loss.** Also called the 0-1 loss, this is the ideal loss function for classification problems (it's the accuracy) but hard to optimize because it's a combinatorial problem.

**The Perceptron algorithm.** Basically gradient descent for regression. Apply the following update rule for each misprediction:

$$ \vec{w} \leftarrow \vec{w} + y_i \vec{x}_i. $$

The perceptron algorithm guarantees convergence, assuming the data is separable.

**Linear Discriminant Analysis.** A _generative_ approach to classification with a linear boundary. Assume $\Pr (\vec{x} | y)$ is Gaussian and there is a prior distribution over $y$. Then, you can generate instances by using the empirical mean and average for each $y$. If the prior over $y$ is uniformly distributed, the solution for LDA minimizes OLS loss.

**Logistic Regression.** The _discriminative_ version of LDA, this skips the generation of distributions of $\vec{x}$ and directly proposes a linear boundary. The probability of a class is

$$ \Pr (\vec{x}; \vec{w}, b) = \frac{1}{1 + \exp(-y(\vec{x}^T\vec{w} + b))}. $$

This is convex but there's no analytical solution. You can add SRM regularizers easily.

**Support Vector Machines.** SVMs use the hinge loss and add L2-regularization.

**Scalable optimization.** In a closed-form solution, the matrix inversion is most expensive. You can distribute the outer products and sum them, but still need to invert that combined matrix on a single node. Gradient Descent can be more easily distributed, by distributing the update rules for the feature dimensions. Stochastic Gradient Descent uses a random sample of input data for every step and produces better results while making the algorithm more manageable in terms of complexity.

**Neural Networks.** A neuron computes a linear combination of its input neurons and adds a non-linear _activation function_. A neural network is composed of multiple layers of neurons that each perform the same computation albeit with different weights. Forward propagation is the composition of these functions, back propagation is computing the gradient of the whole function and applying gradient descent.

**Activation functions.** Often, the sigmoid/logistic function is used: $x \mapsto 1/(1+e^-x)$. The ReLU (Rectified Linear Unit) is better and favors sparse solutions: $x \mapsto \max(0, x)$.

**K-nearest neighbor.** Trainingless classification algorithm. That means, though, that all the data has to be stored and accessible very quickly. The curse of dimensionality strikes again! Also, when you end up using this, _normalize_ your dimensions since it uses Euclidean distance.

## Visualization