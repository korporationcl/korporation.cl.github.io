# TCP

# Acknowledgment

The method for a receiver to signal to a sender that a package has arrived is known as `acknowledgment|ack`. The sender sends a packet and waits
for waits for an `ack`. When the receiver gets the packet, it sends an `ack`. When the sender receives the `ack` it sends another packet and
the process continues.

- How long should the sender wait for an ack?
- What if the `ack` is lost?
- What if the packet was received but had errors on it?

