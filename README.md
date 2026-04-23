#step(1)
s=[]
for i in range(256):
    s.append(i)
#step(2)
key=input('Enter the key: ')
T=[]
for i in range(256):
    T.append(ord(key[i%len(key)]))
#print(T)
#Step(3)
j=0
for i in range(256):
    j=(j+T[i]+s[i])%256
    swap=s[i]
    s[i]=s[j]
    s[j]=swap
#print(s) 



#Step(4)
plain_txt=input('Enter your plain text')
newkey=[]
j=0
i=0
for k in range(len(plain_txt)):
    i=(i+1)
    j=(j+s[i])%256
    s[i],s[j]=s[j],s[i]
    t=(s[i]+s[j])%256
    newkey.append(s[t])
print('the new key is',newkey)



#Encryption
dch_txt=[]
ch_txt=[]
for i in range(len(plain_txt)):
    dch_txt.append(ord(plain_txt[i])^newkey[i])
    ch_txt.append(chr(ord(plain_txt[i])^newkey[i]))
print("the decimal cipher is : ",dch_txt)
print("the cipher text is : ",ch_txt)
#Decryption
p_txt=[]
for i in range(len(ch_txt)):
    p_txt.append(chr(ord(ch_txt[i])^newkey[i]))
print("the plain text is : ",p_txt)
