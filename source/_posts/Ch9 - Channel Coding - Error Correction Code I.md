---
title: Ch 9 - Channel Coding - Error Correction Code I
date: 2023-04-02
tags:
  - teaching
  - information-theory
---

## Repetition Code  
反復符号  

This might be the simplest error correction code that we can think of. As we showed in previous chapter, an example of repetition code is simply repeat each binary digit three times. And the receiver can adopt “best 2 out of 3” strategy to correct 1-bit error.

$m = 10110 \\$

$m$﻿ is original message and $w$﻿ is our repetition code. If there is any error during information transferring through our channel, the codeword turns to

$w' = 110\ 100\ 111\ 101\ 001$

We are able to decode this message back to $m = 10110$﻿ .

But we can easily notice that there are some problems.

- If more than 1 error occurs for a 3-bit block, we cannot decode it correctly.
- The repetition is a huge waste, we add 2 redundancy bits for 1 message bit, the efficiency is low (33%)

We can make it more reliable by adding more redundancy bits, for example, we repeat each bit five times, then we are able to correct 2 bits of error in 5-bit block.

However, in most real world situations, error is expected to be pretty rare, maybe 2 among 100 bits. And we would like to have better **efficiency**. This is where Hamming Code comes into play.

## Hamming Code  
ハミング符号  

Hamming code is an important error correction code scheme. We will first have an intuitive look at how it works, and then use linear equations and matrices to formulate it.

### Intuitive Understanding

We will follow the track of how Hamming invented Hamming code. the essential idea was:

> [!important]  
> Apply parity check not on the entire message, but on carefully selected subsets.  

Assume we have 16 bits and write a block for it. Here is how we choose the subsets.

|   |   |   |   |
|---|---|---|---|
|0|1|0|1|
|1|0|1|0|
|0|1|1|0|
|0|1|1|0|

|   |   |   |   |
|---|---|---|---|
|0|1|0|1|
|1|0|1|0|
|0|1|1|0|
|0|1|1|0|

|   |   |   |   |
|---|---|---|---|
|0|1|0|1|
|1|0|1|0|
|0|1|1|0|
|0|1|1|0|

|   |   |   |   |
|---|---|---|---|
|0|1|0|1|
|1|0|1|0|
|0|1|1|0|
|0|1|1|0|

It’s kind of like YES or NO questions, asking “is there an error among these highlighted cells?”

We reserve 4 cells for the answers to these YES or NO questions.

|   |   |   |   |
|---|---|---|---|
|0|==**1**==|0|1|
|1|0|1|0|
|0|1|1|0|
|0|1|1|0|

|   |   |   |   |
|---|---|---|---|
|0|1|==**0**==|1|
|1|0|1|0|
|0|1|1|0|
|0|1|1|0|

|   |   |   |   |
|---|---|---|---|
|0|1|0|1|
|==**1**==|0|1|0|
|0|1|1|0|
|0|1|1|0|

|   |   |   |   |
|---|---|---|---|
|0|1|0|1|
|1|0|1|0|
|==**0**==|1|1|0|
|0|1|1|0|

|   |   |   |   |
|---|---|---|---|
|0|==**1**==|==**0**==|1|
|**==1==**|0|1|0|
|==**0**==|1|1|0|
|0|1|1|0|

And the four bits are reserved for parity check, the rest are data bits.

|   |   |   |   |
|---|---|---|---|
|0|||1|
||0|1|0|
||1|1|0|
|0|1|1|0|

> [!important]  
> What if the first cell flipped?  

|   |   |   |   |
|---|---|---|---|
|==1==|||1|
||0|1|0|
||1|1|0|
|0|1|1|0|

No one can know if this is happening!

Then how to solve this problem? ⇒ Simply remove the first cell

|   |   |   |   |
|---|---|---|---|
|N/A|||1|
||0|1|0|
||1|1|0|
|0|1|1|0|

We have 11 data bits and 4 check bits, total 15 bits. This is denoted as **(15, 11) Hamming Code.**

Now we have an applicable method to correct 1-bit error in our received message. The process are as follows:

- Sender:
    - divide the message into 11-bit chunks
    - put them into the 4 * 4 block
    - add 4 check bits
- Receiver:
    - Check the parity for the four groups
    - Correct the error if there is one

> [!important]  
> Can we make use of the first cell?  

Sure! We can have a parity check for the entire 4 * 4 block. And this makes it possible to detect 2-bit error. This is called **Extended Hamming Code**. Let’s see an example:

- Sender: send 10101100110
    - put data bits in the block
        
        |   |   |   |   |
        |---|---|---|---|
        ||||1|
        ||0|1|0|
        ||1|1|0|
        |0|1|1|0|
        
    - add four check bits
        
        |   |   |   |   |
        |---|---|---|---|
        ||**1**|**0**|1|
        |**1**|0|1|0|
        |**0**|1|1|0|
        |0|1|1|0|
        
    - add the parity check at the first cell
        
        |   |   |   |   |
        |---|---|---|---|
        |**0**|**1**|**0**|1|
        |**1**|0|1|0|
        |**0**|1|1|0|
        |0|1|1|0|
        
- Receiver
    - No error
        
        |   |   |   |   |
        |---|---|---|---|
        |**0**|**1**|**0**|1|
        |**1**|0|1|0|
        |**0**|1|1|0|
        |0|1|1|0|
        
    - 1-bit error at data bit
        
        |   |   |   |   |
        |---|---|---|---|
        |**0**|**1**|**0**|1|
        |**1**|0|1|0|
        |**0**|1|==0==|0|
        |0|1|1|0|
        
    - 1-bit error at check bit
        
        |   |   |   |   |
        |---|---|---|---|
        |**0**|**1**|**0**|1|
        |==**0**==|0|1|0|
        |**0**|1|1|0|
        |0|1|1|0|
        
    - 1-bit error at first cell
        
        |   |   |   |   |
        |---|---|---|---|
        |==**1**==|**1**|**0**|1|
        |**1**|0|1|0|
        |**0**|1|1|0|
        |0|1|1|0|
        
    - 2-bit error
        
        |   |   |   |   |
        |---|---|---|---|
        |**0**|**1**|**0**|1|
        |**1**|0|1|0|
        |**0**|==0==|1|0|
        |0|==0==|1|0|
        

Q 6-1:

Encode the data using extended hamming code: 0110 1100 001

- A 6-1:
    
    |   |   |   |   |
    |---|---|---|---|
    ||||0|
    ||1|1|0|
    ||1|1|0|
    |0|0|0|1|
    
    |   |   |   |   |
    |---|---|---|---|
    ||**1**|**1**|0|
    |**1**|1|1|0|
    |**1**|1|1|0|
    |0|0|0|1|
    
    |   |   |   |   |
    |---|---|---|---|
    |**1**|**1**|**1**|0|
    |**1**|1|1|0|
    |**1**|1|1|0|
    |0|0|0|1|
    
    The encoded series: 1110 1110 1110 0001
    

Q 6-2:

Having a received data block: 1110 1110 1110 0011

|   |   |   |   |
|---|---|---|---|
|**1**|**1**|**1**|0|
|**1**|1|1|0|
|**1**|1|1|0|
|0|0|1|1|

Reconstruct the original message

- A 6-2:
    
    |   |   |   |   |
    |---|---|---|---|
    |==**1**==|**1**|==**1**==|0|
    |==**1**==|1|1|0|
    |==**1**==|1|1|0|
    |0|0|==1==|1|
    
    |   |   |   |   |
    |---|---|---|---|
    |==**1**==|**1**|==**1**==|0|
    |==**1**==|1|1|0|
    |==**1**==|1|1|0|
    |0|0|0|1|
    
    The original message is: 0110 1100 001
    

### Encode Formulation

Naturally, we can represent the check bits for hamming code by equations. To make things easier, here we consider (7, 4) Hamming Code instead.

The repetition code we’ve talked before takes 1-bit message and forms a 3-bit codeword.

$0 \to 000 \quad 1 \to 111$

H(7, 4) takes 4-bit message and forms a 7-bit codeword

$1001 \to 1001001 \quad 0010 \to 0010011$

We can see that the efficiency of hamming code is significantly higher than repetition code

$\frac{4}{7} \approx 57.1\%$

We’ve discussed H(15, 11), but we can build H(7, 4) with similar idea

|   |   |   |   |
|---|---|---|---|
||**c1**|**c2**|x1|
|**c3**|x2|x3|x4|

Let’s denote the original 4-bit message as

$\textbf{\textit{m}} = m_1 m_2 m_3 m_4$

We add 3 redundancy bits

$\textbf{\textit{c}} = c_1 c_2 c_3$

Where in GF(2), or simply think the “addition” as “exclusive or” operation

$c_1 = x_1 + x_2 + x_4 \\$

concatenate them together, we have our codeword

$\textbf{\textit{w}} = w_1 w_2 w_3 w_4 w_5 w_6 w_7 = m_1 m_2 m_3 m_4 c_1 c_2 c_3$

Eg:

$\textbf{\textit{m}} = m_1 m_2 m_3 m_4 = 1001$

$c_1 = x_1 + x_2 + x_4 = 0\\$

therefore, we append the check bits to original message to form our codeword

$\textbf{\textit{w}} = m_1 m_2 m_3 m_4 c_1 c_2 c_3 = 1001001$

### Error Fixing

Now we understand how to encode the message, but how do we fix errors?

Consider we have an original message

$100000110100$

to be transferred using H(7, 4). The first step is to separate it into blocks

$1000\quad 0011\quad 0100$

then we add the extra bits for each block, remember our function is

$\begin{equation}$

the H(7, 4) code is

$1000110\quad 0011100\quad 0100101$

let’s assume that there are errors during transmission and our received message is

$1000010\quad 0010100\quad 0110001$

now we check if the three check bits matches with message bits

$1000010\quad 0010100\quad 0110001\\$

For the first block, $c_1$﻿ does not match but $c_2$﻿ and $c_3$﻿ matches, this means there is an error on $c_1$﻿, we can simply correct it.

$1000010 \to 1000110 \to 1000$

For the second block, all the three check bits does not match, observe that in (1), $m_4$﻿ is included in $c_1$﻿ , $c_2$﻿ and also $c_3$﻿ , therefore we know that the fourth bit in message is flipped and we can fix it.

$0010100 \to 00111100 \to 0011$

As for the third block, we need to be careful, if we apply the same process, we will get

$0110001 \to 0111001 \to 0111$

which is not the original message block $0100$﻿ . This is because we have 2 errors in one block, and H(7, 4) cannot fix 2 bit errors. But as we said before, in most cases the rate of error is pretty low and H(7, 4) will work out just fine.

### Check Bit State and Error State

> [!important]  
> Why we need 3 redundancy bits for 4 message bits? Is it possible for H(6, 4) or H(7, 5)?  

$\textbf{\textit{w}} = m_1 m_2 m_3 m_4 c_1 c_2 c_3$

Our codeword contains of three redundancy bits, and they have 8 possible values

$000, 001, 010, 011, 100, 101, 110, 111$

and this actually corresponds to 8 possible states, that is

- No error
- Error in 1st bit
- Error in 2nd bit
- Error in 3rd bit

- Error in 4th bit
- Error in 5th bit
- Error in 6th bit
- Error in 7th bit

and this shows that three check bits can have 8 possible values is very useful, it gives us all the possible cases with up to 1 bit error. If we need to fix cases with 2 bit of errors, we need to add more check bits in order to represent more error states. Now we understand the 7 and 4 in H(7, 4) are no accident.

Q: We know that to send 4-bit message block, we need 3 redundancy bits and build H(7, 4), to send 11-bit message block, we need 4 redundancy bits and build H(15, 11). To send 26-bit message block, how long is the hamming codeword we will be using?

A: Let $m$﻿ be the length of original message block and $r$﻿ be the number of redundancy bits. Check bit state must be larger or equal than error states (including the no error case).

$m + r + 1 \leq 2^r$

When $m = 26$﻿ , the smallest $r$﻿ is $5$﻿ , therefore, the hamming code is H(31, 26).

> [!important]  
> Where does our equations for check bits come from?  

$\begin{split}$

To answer this question, we need to know how we map the check bits to error states. However, mapping values of the check bits to the error states makes no sense, because what we need to look at is not “the values of the check bits”, but “whether the check bits is consistent with other bits”. So the 8 values we are looking at should be listed as follows

$\checkmark \checkmark \checkmark, \times \checkmark \checkmark, \checkmark \times \checkmark, \checkmark \checkmark \times, \\$

these are also called **syndromes**, and it can also be written in binary form.

|   |   |   |
|---|---|---|
|Check Bit State (syndrome)|Error State|$m_1 m_2 m_3 m_4 c_1 c_2 c_3$|
|$\checkmark \checkmark \checkmark$|No error|$\checkmark \checkmark \checkmark \checkmark \checkmark \checkmark \checkmark$|
|$\times \checkmark \checkmark$|$c_1$﻿ is wrong|$\checkmark \checkmark \checkmark \checkmark \times \checkmark \checkmark$|
|$\checkmark \times \checkmark$|$c_2$﻿ is wrong|$\checkmark \checkmark \checkmark \checkmark \checkmark \times \checkmark$|
|$\checkmark \checkmark \times$|$c_3$﻿ is wrong|$\checkmark \checkmark \checkmark \checkmark \checkmark \checkmark \times$|
|$\times \times \checkmark$|$m_1$﻿ is wrong|$\times \checkmark \checkmark \checkmark \checkmark \checkmark \checkmark$|
|$\times \checkmark \times$|$m_2$﻿ is wrong|$\checkmark \times \checkmark \checkmark \checkmark \checkmark \checkmark$|
|$\checkmark \times \times$|$m_3$﻿ is wrong|$\checkmark \checkmark \times \checkmark \checkmark \checkmark \checkmark$|
|$\times \times \times$|$m_4$﻿ is wrong|$\checkmark \checkmark \checkmark \times \checkmark \checkmark \checkmark$|

Base on this table, we can invent the aforementioned equations. Focus on the last four error states, we know that the first check bit should not match when $m_1$﻿ or $m_2$﻿ or $m_4$﻿ is wrong, so we have to include them in the equation for $c_1$﻿

$c_1 = m_1 + m_2 + m_4$

Same applies for $c_2$﻿ and $c_3$﻿

$\begin{split}$

So the equations for check bits comes from the association between check bits states, error states.

We can actually change the permutation in the association, and we will be able to derive a set of different equations but still, it is a valid H(7, 4) code.

## Generator Matrix

### Valid Codeword and Invalid Codeword

To begin with, let’s look at the English word “food”. Think about changing only one letter, we have valid words like

- ford
- foot
- good
- wood
- etc.

But also we have even more invalid words like

- aood
- foqd
- etc.

If we are sending the message “I would like some food.”, but one letter is wrong, the receiver gets “I would like some wood.”. Then we have no idea that an error happened because the message still make sense. The problem here is that the valid words are too close together.

On the other hand, if the receiver gets the word “alligabor” and we know that there are at most one letter gets changed, we can probably guess that the original word that we meant to send is “alligator”. This is because alligator is surrounded mostly by invalid words and it is the closest valid word to “alligabor”.

**alligator** - allitator - aglitator - **agitator**

**alligator** -allogator - **allocator**

**alligator** - alligabor - allilabor - allelabor - llelabor - **belabor**

The fact that the valid words are spaced out and separated by invalid words means it’s easier to correct errors just by moving it to the nearest valid word.

Let’s see how the idea works for repetition code and hamming code.

### Repetition Code

Original Message:

$00101 \to 000\ 000\ 111\ 000\ 111$

we are actually using 1-bit blocks for the original message, and we can consider each block lives in 1 dimensional space.

$0 - 1$

we cannot fix any error here since the valid words are packed together with no space in between.

As for the repetition code, there are only 2 valid codeword in 3 dimensional space (arrange them in a cube)

$000\\$

If we receive any invalid codeword, that will tell us an error has happened. For example, if we have

$00101 \\$

we can fix the errors by finding the nearest valid codeword in the cube.

> [!important]  
> The process of encoding a message into an error correction code is really the process of moving it into a higher dimensional space where valid codewords are separated by invalid codewords.  

For the repetition code, we expanded the space from 1 dimension to 3 dimension, and we travel between these spaces using **Generator Matrix.**

$\textbf{\textit{m}} G = \textbf{\textit{w}}$

Here, the generator matrix is

$G = [1\ 1\ 1]$

and

$[0] [1\ 1\ 1] = [0\ 0\ 0]\\$

### Hamming Code

Similarly, for H(7, 4), we are mapping items in 4D space into 7D space.

$0010\ 1100 \to 0010011\ 1100011$

This is hard to visualize though… But we can list all possible codewords in 7D space (only 16 among 128 codewords are valid)

|   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|
|0000 000||||||||
||||||||0001 111|
||||0010 011|||||
|||||0011 100||||
||||||0100 101|||
|||0101 010||||||
|||||||0110 110||
||0111 001|||||||
|||||||1000 110||
||1001 001|||||||
||||||1010 101|||
|||1011 010||||||
||||1100 011|||||
|||||1101 100||||
|1110 000||||||||
||||||||1111 111|

As we discussed before, we are actually separating valid codewords in order to fix errors. And the generator matrix for hamming code would look like this:

$G = $

Notice that each column corresponds to one bit in codeword.

$[m_1 m_2 m_3 m_4]$

> [!important]  
> The first four columns are identity matrix which will copy @import url('https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/katex.min.css')m1m2m3m4m_1 m_2 m_3 m_4m1​m2​m3​m4​﻿ to the output  
  
> [!important]  
> The last three columns are generating the parity check bits as we’ve seen in equations  

$\begin{split}$

## Hamming Weight and Hamming Distance  
ハミング重みとハミング距離  

### Definition of Hamming Weight

Hamming weight is the number of 1s in a codeword.

If we define $u_i$﻿ as the i-th digit in the codeword, then its hamming weight is

$w = \sum_i u_i$

Examples:

$w(1) = 1 \\$

### Definition of Hamming Distance

Hamming distance is the number of digits where two codewords are different.

$\delta(u, v) = $

$d = \sum_i \delta(u_i, v_i)$

Examples:

$d(000, 111) = 3 \\$

And now, it is obvious that

$d(\textbf{\textit{c}}, \vec{\textbf{0}}) = w(\textbf{\textit{c}})$

### Minimum Distance

Minimum distance is the smallest hamming distance between any two valid codewords.

Take the repetition code as an example

$[1\ 1\ 1]$

|   |   |
|---|---|
|message|codeword|
|0|000|
|1|111|

There are only two valid codewords, therefore the hamming distance between them is exactly the minimum distance, which is 3.

Other examples:

$\begin{bmatrix}$

$\begin{bmatrix}$

> [!important]  
> Why do we care about minimum distance?  

|   |   |   |   |
|---|---|---|---|
|$d_{min}$||error detection ability|error correction ability|
|1|v - v|0|0|
|2|v - x - v|1|0|
|3|v - x - x - v|2|1|
|4|v - x - x - x - v|3|1|
|5|v - x - x - x - x - v|4|2|
|6|v - x - x - x - x - x - v|5|2|
|$d$||$d - 1$|$\lfloor \frac{d - 1}{2} \rfloor$|

> [!important]  
> How can we find the minimum distance for hamming code?  

It’s so annoying to compare every pairs of codewords, but luckily we have tricks to do it.

Hamming code is a linear code, which means that any linear combination of valid codewords is still a valid codeword. Therefore we have

$\begin{split}$

This means the minimum distance is the same as the minimum hamming weight of all non-zero codewords.

For H(7, 4) code, the minimum hamming weight is 3, and the minimum distance is 3, and thus it can correct 1 bit error.

## Parity Check Matrix

If there is no error, the codeword should satisfy

$w_1 + w_2 + w_4 + w_5 = x_1 + x_2 + x_4 + c_1 = 0 \\$

We can also write this condition as

$\left\{$

In matrix form

$\begin{bmatrix}$

$H \textbf{\textit{w}}^T = \textbf{0}$

Where H is the **Parity Check Matrix**

$H = $

> [!important]  
> How do we perform 1-bit error correction?  

For example, if the receiver gets $1101100$﻿

$\begin{bmatrix}$

this shows that no error occurred.

There is an interesting fact about the parity check matrix. Assume there is a 1-bit error occurred, write the error as a vector with only 1 bit of 1.

$\textbf{\textit{e}} = [e_1, e_2, e_3, e_4, e_5, e_6, e_7] $

$\sum_i e_i = 1$

We denote the receiver side of the message as

$\textbf{\textit{w'}} = \textbf{\textit{w}} + \textbf{\textit{e}}$

Therefore

$\begin{split}$

For example, let’s assume that there is an error on the 2nd bit, when the receiver do the parity check, he/she gets

$\begin{bmatrix}$

this result is called **syndrome**. Wait, does this term sounds familiar? Yes! It is the same thing as we discussed in previous chapter!

We can then solve the equation to figure out where is the error bit.

$\left\{$

But there is a simpler way, we notice that the syndrome vector is the same as the **2nd** column of the parity check matrix. Therefore, the error must happen on the **2nd** bit.

$H = $

- Q 6-3:
    
    If the receiver got the message 1010 000, and there is at most 1 bit error during the transmission. The generator matrix of the hamming code is
    
    $G = $
    
    restore the original message
    
    $w_1 = x_1\\$
    
    Parity check
    
    $w_1 + w_2 + w_3 + w_5 = 0\\$
    
    Therefore, Parity Check Matrix is
    
    $H =$
    
    The receiver calculate
    
    $y = 1010000$
    
    $y H^T = 011 \neq 0$
    
    the 4th bit is wrong
    
    $w = 1011000$
    
- A 6-3:
    
    $(1010000) H^T = (011)$
    
    $\textbf{\textit{e}} = 0001000$
    
    $e_4 = 1$
    
    The fourth bit has an error, the corrected message is 1011 000
    

> [!important]  
> How to get generator matrix from parity check matrix  

In parity check matrix

- Each row stands for a check bit
- Each column stands for a syndrome, which corresponds to a error state

$\qquad \quad m_1\ m_2\ m_3\ m_4\ \ c_1\ \ c_2\ \ c_3 \\$

In generator matrix

- Each row stands for a message bit
- Each column stands for a codeword bit, including message bits and check bits

$G = $

> [!important]  
> The left part of H is actually the transpose of the right part of G
