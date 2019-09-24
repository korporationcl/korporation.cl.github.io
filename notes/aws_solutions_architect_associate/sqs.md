# SQS

- Standard (default) and FIFO (First in First out)
- SQS is pulled based, not push.
- Messages are 256kb size
- Messages can kept in the queue from 1 minute to 14 days.
- Default retention period is 4 days.
- Visibility timeout is the amount of time a message is invisible in the queue after a consumer/reader picks the message. If the message is not
  processed during that time, it will be available in the queue again (this could produce duplicates or messages delivered twice)
- Visibility maximum timeout is 12h.
- SQS guarantees the message will be processed at least once
- SQS long polling is a way to retrieve messages from the queue. Long polling doesn't return response until a message arrives to the queue or
  the long polling times out.
