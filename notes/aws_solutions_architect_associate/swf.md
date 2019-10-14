# AWS Simple Work Flow

- SWF vs SQS
  - SWF executions can last up to 1 year.
- SWF API is task-oriented. SQS API is message-oriented API.
- SWF ensures a task is assigned once and is never duplicated. SQS you need to handle duplicated messages and may also need to make sure
  the message is only processed once.
- SWF keeps tracks of all events and tasks in an application. in SQS, you need to implement your own application tracking.
- SWF is composed of:
  - Workflow starters: they can initiate (start) a workflow.
  - Deciders: control the flow of the activity in a workflow. If something failed or finished, a Decider decides what to do.
  - Activity workers: nodes that carry out the activity tasks.