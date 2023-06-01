Most of the code in 3700send is contained in the run() function, where for loop continuously loops through the sockets, differentiating between system input, or new packets to be sent, and acks that have been received. Before this happens however, run() checks to make sure that the oldest un-ACKed packet has not been dropped. If it has, then the packet is dropped and the function executes multiplicative decrease. If the socket is an ack message, the message is checked to be uncorrupted. If the ACK is a duplicate, all in-flight packets are retransmitted, if the ACK belongs to a packet that is not in flight, it is ignored, and if an ACK corresponds to a packet that is currently in flight the packet is removed from in-flight and either slow start or multiplicative increase is called upon. If the socket is a system input, run() checks to see how big the current window is and takes in that many inputs, sending them as packets to 3700recv and increasing and adding the sequence number to in-flight.

In 3700send there are also helper functions. Slow start() increases the window size based on its relation to the slow start threshold. Multiplicative_decrease() decreases the slow start threshold to half and sets the window size to 1. Fast_retransmit() retransmits all in-flight packets if there are three duplicate ACKs in a row. Check_hashes() checks to make sure the hashes of the packet and the recieved ack are the same to make sure the file wasnt corrupted. And better_send() sends a given message to 3700recv.

Like 3700send, most of 3700recv's code is contained within run(). Within run(),  the message is broken down, checked for corruption and data loss, and prints out packets once they have been received in the correct order, as found by the given sequence numbers.

In 3700recv there are some helper functions as well. sort_and_merge_buffer() sorts the buffer in sequence number numerical order and prints out the messages that have been received in order. Get_last_num() gets the last sequence number that was recieved by the program.

The biggest challenge with this project was making it more efficient. Being able to send messages without corruptions and resend when dropped etc. was fairly simple, but once that was complete we had to make sure the program ran fast and efficient. To do this, we implemented fast retransmit and recovery, slow start and multiplicative increase so we would be able to increase the amount of packets sent at once.

Some features we thought we designed well were:
	The implementation of slow start and multiplicative increase/decrease
	The ability to detect dropped packets
	and bring able to detect corruption and duplication
When creating the program, we tested our code thoroughly with the given test config files and logging various statements during the simulation. These statements helped us gain an understanding of how data was being passed around and where our code was going wrong.