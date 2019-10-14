# SQS

- Two types: Standard (default) and FIFO (First in First out)
- SQS is pulled based, not push.
- **Messages have a maximum size of `256kb`**
- **Messages can kept in the queue from 1 minute to 14 days.**
- **Default retention period is 4 days (maximum is 14 days)**
- Visibility timeout is the amount of time a message is invisible in the queue after a consumer/reader picks the message. If the message is not
  processed during that time, it will be available in the queue again (this could produce duplicates or messages delivered twice)
- **Visibility default timeout is 30 seconds.**
- **Visibility maximum timeout is 12h.**
- SQS guarantees the message will be processed at least once
- **SQS long polling is a way to retrieve messages from the queue. Long polling doesn't return response until a message arrives to the queue or
  the long polling times out. Min value is `1s`, maximum is `20s`.**
- Delay queues allow you to postpone the delivery of new messages for a specific number of seconds.
  - If you create a message for this queue will be invisible for the consumers for the delay period
  - You can set `DelaySeconds` attribute to a value between `0s` (default) and `900s` (15m)
- Server side Encryption of a queue is not enabled by default
- You can call Lambda Functions
- Delay queues and visibility timeouts are similar since they make a message unavailable for a consumer. The difference is that a Delay queue
 hides the messages once **arrives to the queue**, whereas a visibility timeout **hides a messages once has been retrieved by a consumer**.
 - When a message is not delayed nor with visibility timeout is `in flight`.
  - **You can have up to 120,000 messages in flight at any time**
- **SQS identifies a message with a globally unique ID that is returned when a message is delivered to the queue**
- **Queue names are unique within the scope of all the queues**
- **SQS assigns a queue identifier called a `queue url` which includes the name of the queue and other components**
- **Each time you receive a message from the queue, you receive a `receipt handle` for that message. If you want to delete a message you need the `receipt` ID. This means you always need to receive a message before you can delete it.**
- You can include structured metadata (such as timestamps, geospatial data, signatures, and identifiers) with messages using message attributes.
- SQS supports `Dead Letter queues`. Is a queue that other queues can target to send messages that for some reason could not be sucessfully processed.
- **SQS access control allows you to assign policies to queue that grant access to other AWS accounts without having to use an IAM role.**

# References
- https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/working-with-messages.html#setting-up-long-polling

