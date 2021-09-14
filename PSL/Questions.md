
Which of the messages can the client C decrypt, which keys does C ever learn:

Keys in the initial knowledge: pk(ath),pk(C),inv(pk(C))

ath sends then c,({|ath,C,g,KCG,T1|}skag), ({|g,KCG,T1,N1|}Ktemp), { tag,{Ktemp}inv(pk(ath))}pk(C). Using pk(C), C can decript {tag,{Ktemp}inv(pk(ath))}.