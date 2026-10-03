
IP is often described as an "unreliable" protocol because it does not provide any guarantees about the delivery of data packets.
- packets are sent from a source device to a destination device without any acknowledgement of receipt or verification of the packet's successful delivery, meaning that packets can be lost, delayed, or corrupted in transit, and there is no built-in mechanism to detect or recover from these errors.

Even though IP itself it unreliable, [[protocol.TCP]] is often built on top of it, which is in fact a reliable protocol.