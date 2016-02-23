## onyx-amazon-sqs

Onyx plugin for Amazon SQS.

#### Installation

In your project file:

```clojure
[onyx-amazon-sqs "0.8.12.0-SNAPSHOT"]
```

In your peer boot-up namespace:

```clojure
(:require [onyx.plugin.sqs-input]
          [onyx.plugin.sqs-output])
```

#### Functions

##### Input Task

Catalog entry:

```clojure
{:onyx/name <<TASK_NAME>>
 :onyx/plugin :onyx.plugin.sqs-input/input
 :onyx/type :input
 :onyx/medium :sqs
 :onyx/batch-size 10
 :onyx/batch-timeout 1000
 :sqs/attribute-names []
 :sqs/idle-backoff-ms idle-backoff-ms
 :sqs/queue-name queue-name
 :onyx/doc "Reads segments from an SQS queue"}
```

#### Attributes

|key                           | type      | description
|------------------------------|-----------|------------
|`:sqs/deserializer-fn`        | `keyword` | A keyword pointing to a fully qualified function that will deserialize the :body of the queue entry from a string
|`:sqs/queue-name`             | `string`  | The SQS queue name
|`:sqs/attribute-names`        | `[string]`| A list of attributes to fetch for the queue entry 
|`:sqs/idle-backoff-ms`        | `int`     | Backoff on empty read for backoff ms in order to reduce SQS per request costs


Possible attribute names can be found in the <a href="http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sqs/model/ReceiveMessageRequest.html#withAttributeNames(java.util.Collection)">API documentation</a>.

##### Output Task

Catalog entry:

```clojure
{:onyx/name <<TASK_NAME>>
 :onyx/plugin :onyx.plugin.sqs-output/output
 :sqs/queue-name <<OPTIONAL_QUEUE_NAME>>
 :sqs/queue-url <<OPTIONAL_QUEUE_URL>>
 :sqs/serializer-fn :clojure.core/pr-str
 :onyx/type :output
 :onyx/medium :sqs
 :onyx/batch-size 10
 :onyx/doc "Writes segments to SQS queues"}
```

Segments received at this task must have a body key in string form, which will be written to the queue defined in the key `:sqs/queue-name`, OR `:sqs/queue-url` in the task-map, or via the key `:queue-url` in the segment. Note that queue-name and queue-url are in different formats, with the queue-name being in the form "yourqueuename" and queue-url in the form `https://sqs.us-east-1.amazonaws.com/039384834151/e3668c38-4`.

#### Attributes

|key                           | type      | description
|------------------------------|-----------|------------
|`:sqs/queue-name`             | `string`  | Optional SQS queue name. If not present, segments will be routed via the `:queue-url` key of the segment
|`:sqs/queue-url`              | `string`  | Optional SQS queue url. If not present, segments will be routed via the `:queue-url` key of the segment
|`:sqs/serializer-fn`          | `keyword` | A keyword pointing to a fully qualified function that will serialize the :body key of the segment to a string

#### Acknowledgments

Many thanks to [LockedOn](http://www.lockedon.com) for allowing this work to be open
sourced and contributed back to the Onyx Platform community.

#### Contributing

Pull requests into the master branch are welcomed.

#### License

Copyright © 2016 Distributed Masonry LLC

Distributed under the Eclipse Public License, the same as Clojure.
