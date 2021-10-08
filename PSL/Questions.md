# Protocol Security Lab

## Exercise 1
### Exercise 1.1: Kerberos PKInit

Which of the messages can the client C decrypt, which keys does C ever learn:

Keys in C's initial knowledge: pk(ath),pk(C),inv(pk(C))

- ath generates the key KCG  and encrypts it with the shared key between (ath, g) "skag", and with a new key Ktemp.

- ath then sends to C the message  " c,({|ath,C,g,KCG,T1|}skag), ({|g,KCG,T1,N1|}Ktemp), { tag,{Ktemp}inv(pk(ath))}pk(C) ".

- Using *pk(C)*, *C* can decode  " tag,{Ktemp}inv(pk(ath)) ", and get Ktemp, which has ath's signature.

- At this point C uses Ktemp to decode " |g,KCG,T1,N1| ". Now C also knows the symmetric key KCG. 
The message part encrypted with the key skag stays encrypted to C.

- C then sends to g a message containing "s, N2, ({|ath,C,g,KCG,T1|}skag),({|C,T1|}KCG)".
g can use the key skag to decode the message from ath, and get KCG as well. Now C and g share the symmetric key KCG.

- g answers to C with a symmetric key KCS , encrypted with the key skgs, and also  with the key skgs. C sends the part of the message encoded with skgs to s. s can now send the Payload to C, encoding it with KCS.

C learns the keys Ktemp, KCG and KCS.

C authenticates s on KCS
s authenticates C on KCS
KCS sectret between s, C, G

### Exercise 1.2

OFMC finds a weak authorization attack on the protocol. This happens as the intruder can impersonate C, and decode both Ktemp and KGS from the message sent from ath to C. Then he can modify the message and impersonate ath, making C believe that he received a legit message from ath. To fix this protocol, one can simply modify the first message from ath to C, adding concatenating Ktemp with C, and sign it with pk(ath). We would have then :

ath -> C: C,
	({|ath,C,g,KCG,T1|}skag),
        ({|ath,g,KCG,T1,N1|}Ktemp),
        {tag,{Ktemp, C}inv(pk(ath))}pk(C)


### Exercise 1.3
Using a Diffie-Hellman key-exchange, the PKINIT protocol can be made safe by changing the two initial messages. In the first message we send exp(N1, T2) signed by C. The second message has exp(N1, N2) signed by ath. This way, C can authenticate ath on exp(exp(N1, N2),T2).

C -> ath: C,g,N1,{exp(N1,N2),T0,N1,hash(C,g,N1)}inv(pk(C))   

ath -> C: C,
	({|ath,C,g,KCG,T1|}skag),
        ({|ath,g,KCG,T1,N1|}exp(exp(N1, N2),T2)),
        {tag,{exp(N1, T2),C}inv(pk(ath))}pk(C)


## Exercise 2

### Exercise 2.1

The protocol tries to establish a secure channel between A and B.


### Exercise 2.2
EMMA

### Exercise 2.3
home->B:  B,A,mac(pw(A,home),m3,B,exp(g,X),exp(g,Y)),
          mac(pw(B,home),m4,B,A,exp(g,X),exp(g,Y), mac(pw(A,home),m3,B,exp(g,X),exp(g,Y)))


### Exercise 2.4
A,B,exp(g,X),mac(pw(A,home),m1,A,B,exp(g,X))

It's a dictionary attack, where the intruder guesses the password by matching mac(pw(x20,home),m1,A,B,exp(g,X(1))) .
The intruder is able to do this, as he can read A, B and exp(g,X) from the inital message. Since we know 

### Exercise 2.5

Because Home is not a trusted third party
