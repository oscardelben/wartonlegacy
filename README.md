*In the name of God, the Merciful, the Compassionate.*

#The Warton Legacy

This repository contains code written by my late uncle, Robert Warton. He devoted the latter part of his life to the study of prime numbers and the problem of factorization of the products of large primes. This is the result of his many years of effort. Sadly, he was never able to benefit from this work himself, although he tried many times to sell the software developed here. Instead, he entrusted his work to me as his death neared, and he told me that he wanted it to benefit his family. Since there is little hope of me succeeding in selling this, instead, I hope that by releasing it under the GPL it may benefit mankind. I only ask that if you use it, you use it for good, not evil, to liberate and not oppress, and to educate and not to spread ignorance.

Unfortunately, my uncle left behind very little documentation, and the code is poorly structured and commented (if at all). Hopefully, with time, we can document what he has made and make it a useful resource for researchers and students.

What follows is a slightly edited form of some of the documentation he gave me. This is now out of date; I hope to add his notes containing updates at a later point.

# Report on Cryptography System

## Preliminaries

The programs are created from two compilers: UBASIC, a multi precision extension of BASIC and FTN90, a Fortran 90 developed by Numerical Algorithms Group.

## Overview

The system consists of more than one hundred programs in all performing the following functions: RSA code scrambling and unscrambling, code generating via robust prime construction, code breaking and secret sharing. Routines in number theory and algebraic numbers exist both within programs and separately.

## RSA Code Scrambling and Unscrambling on UBASIC

BOBTOT2 determines second public key minimising number of fixed points. On typing RUN the computer will ask for first public key (product of two integers) and then separately the two constituents (which will be known by sendee). It also asks for a trial key, which must be an odd number. The output will be the second public key of lowest number of fixed points. BOBRSA1 scrambles message which is a number less than public key 1. It requests from the user the message, first public key without its factors which are unknown by the sender and the second public key.

BOBRSA3 unscrambles transmission from BOBRSA1. This program can only be operated by the sendee who is the only one who knows the breakdown of public key 1. Requested information: scrambled message, public key 1 and its constituents and public key 2 output is the scrambled message.

## The Code Generators

BOBPRM2 determines whether number input is composite or a strong pseudo primte to twenty random bases. If number is found to be composite the program will find the nearest strong pseudo prime greater than number input. The probability of a number found in this manner not being prime is 1 in 10 to the power 60 approximately.

ANNPRM determines with certainty whether any number of the form h*2^s + 1 with h < 2^s is prime (^ denotes "to the power of").

ANNPRM2 determines with certainty whether any number of the form h*10^s + 1 with h < 10^s is prime.
One number found to be prime in this way is 7630*(10^1000) + 1.

The above three programs are on UBASIC. Still under construction on the Fortran90 system is a primality testing procedure for all numbers. It is a rigorous procedure which exploits the properties of elliptic curves and its running time is considerably longer than the above three programs.

## The Factorizers

These can be divided into two categories --- special number factorizers and general number factorizers. Because the analyst is unlikely to know anything about the number he has to decompose, it is recommended that he take advantage of the very much faster special factorizers and run them in the first instance for a suitable length of time. Then should the numbers happen to be be of the appropriate structure the message will succumb to this method without necessitating the use of the general factorizers.

### The Special Factorizers

* **P-1 Procedure.** Should any prime factor - 1 of a number be smooth (i.e. all factors of this number less than a prescribed number, e.g. 1,000,000, the number will be factorized using this approach. On UBASIC the appropriate program is BOBPQ2 and on FTN90, BOBPQ1.F90. Note that on all Fortran applications the input expected will be in lots of 4 digits where leading zeroes can be replaced by spaces. The first information requested will always be the length of the number radix 10000 i.e. number of batches of 4 digits.

* **P+1 Procedure.** Should any prime factor + 1 of a given number be smooth as defined above this procedure will factorize it. Relevant programs on UBASIC are BOBPLUS2 and ANNPQ2.

* **P^2 + 1 Procedure.** As above with any prime whose P^2 + 1 is smooth. Relevant program is ANN4 on UBASIC.

* **P^2 + P + 1.** As above with any prime whose P^2 + P + 1 is smooth. BOBFER enables numbers of any length whose factors have the same length and leading halves identical to be factorized instantaneously, e.g. 12345679876543 * 12345671234567.

### General Factorizers

In this category there are three main approaches - Elliptic Curves, (Multiple) Polynomial Quadratic Sieves wherein same prime base is used for each polynomial, and (Multiple) General Number Field Sieves. Much Factorization time will be saved if it is discovered early that numbers have the structure r*s^t+u where r and s are small integers. These numbers are most efficiently handled as Aurifeullian and by the Special Number Field Sieve Procedures.

Mention should be made of two Binary Quadratic Forms techniques - BOBCLAS and BOBGR1 (and extension BOBGR2), and Pollard Rho Factorizer (BOBRHO) all on UBASIC. Although these techniques have been fully developed, they are largely obsolete and have been replaced by the more powerful Elliptic Curve routines. They are however used **within** other techniques.

In practice after having tried the special factorizers above, it is a good idea to try Elliptical Curve Methods next. This is because their running times are determined not by the size of the composite but mainly by the size of the smallest factor. Thus much time is saved by snipping off small factors (under say 10^25) where they exist.

For both the UBASIC and FTN90 Elliptic Curve Methods the information that will be requested will be length of minimum factor in powers of 10. A good practical rule here is to input length in denary digits **plus** 1 for safety. Thus if you suspected that the smallest factor was between 10^20 and 10^21 it is a good idea to respond here with the number 22. For the stage 2 multiplier requested a value of 40 is usually near optimal, but in FTN90's BOBECM8, which has the record-breaking special K factor of 250, values as high as 80 should be employed. Due to the fact that UBASIC has not been extended to take advantage of the large memory available it was not possible to incorporate these innovations there.

Of the some dozen Elliptic Curve Techniques on UBASIC the fastest is BOBONEC7 for all but the very big runs which should use BOBONEC4. The information requested in order: "code", "length etc.", "stage 2 multiplier".

On UBASIC's BOBONEC7 provision is made for restarting incomplete jobs. The usual response to system query "elliptic curve number" is the number 3. However, should a run have executed upto curve x without terminating, the run can be resumed by replying with number x+1.

On FTN90 the most commonly used Elliptic Curve Techniques are BOBECM8.F90 and BOBECM4 which potentiates this approach by employing special polynomials to compute powers of points. The information requested in both cases is the same as in the UBASIC counterpoint except that information is expected in batches of 4 digits. 

###Multiple Polynomial Quadratic Sieve Techniques

Unlike the ECM methods above these techniques consist of suites or batches of programs:

* **Ann Suite**. On FTN90 load and go with ANN1.F90, when completed load and go on ANN2.F90, when completed load and go on ANN3.F90, finally when this is completed load and go on ANN4.F90. This suite can be prepared as a batch so that it will only be necessary to load and go once. In either case apart from the run instruction the only input will be length of code radiz 10000 and the code itself in batches of 4 digits. This program will be used to decompose "hard" numbers of between 40 and 68 digits.

* **Greta Suite**. As above except that 5 programs are involved - GRETA1.F90, GRETA2.F90, GRETA3.F90, GRETA4.F90, and GRETA5.F90. This suite is more powerful than the ANN suite as it employs the Double Large Prime Variant. Recommended maximum length of input code is 72 decimal digits. For longer codes (see below) a special linear dependence determiner should be used.

Under construction is a suite yet more powerful of the MPQS genre --- in addition to the large double prime variant it uses fast polynomial multiplication techniques and truncated multiplications, and exploits the sparse nature of the matrices applying techniques which find minimum polynomials of sequences. Programs on FTN90 awaiting incorporation into suites are BBRECUR.F90, BBRECUR1.F90 and BBRECUR2.F90.

### General Number Field Sieve

This process involves seven steps:
1. Determination of suitable polynomials.
2. Finding the zeros of this polynomial modulo every prime within prime base and then using this information in a double lattice sieve rational and algebraic.
3. Matrix preparation using congruences found in (2).
4. The use of quadratic characters to find principle ideals.
5. Determination of linear dependencies.
6. Multiplication of algebraic integers.
7. Determination of square root in a number field using polynomial factorization techniques and lifting procedures.
Special method involves the use of the Chinese Remainder Theorem.

Although the General Number Field Sieve has been exhaustively tested, it is not yet completed and in the form of a suite as ANN and GRETA above. This is because there is a fair amount of pedestrian multi-precisioning to be done. This does not affect the validity of the mathematics and it was felt, at the time, better to await knowledge of the destination configuration before embarking on the exercise.

The system has been run on both non-homogeneous and homogeneous polynomials, the latter to expedite sieving. Programs used: (homogeneous polynomials) BOBGNFS7 on UBASIC to find suitable polynomial, then on FTN90 BOBMPBAK.F90, BOBST.F90, BOBMID.F90, BOBXT.F90, BOBMID2.F90 and AWGIANT.F90.

For non-homogeneous polynomials first find suitable polynomial using BOBGNFS7 on UBASIC. Then in order OABMPBAK.F90, COBST.F90, COBMID.F90, COBXT.F90, COBMID2.F90 and COBGIANZ.F90.

## Some of the General Purpose Routines

1. BOBSQRTP on UBASIC extracts square roots modulo a prime if they exist. This program outputs real roots of real numbers.
2. ANNSQRTP on UBASIC determines complex roots modulo p of complex numbers. It can also of course compute real roots of real numbers.
3. BOBMCORN on UBASIC determines solutions to the Diophantine equation x^2 + d*y^2 = p where p is a prime if there are solutions.
4. BOBCUBE2 on UBASIC: outputs real cube roots of real numbers modulo a prime.
5. BOBCUBER on UBASIC: outputs complex roots of complex numbers modulo a prime.
6. BOB6 on UBASIC: computes regulator and class number of real quadratic fields.
7. BOBNEGD on UBASIC: computes class number of imaginary quadratic fields.
8. BOBDISC2 on UBASIC: computes roots of quartics.
9. BOBRED on UBASIC: used for lattice reduction.
10. BOBEUC on UBASIC: this routine is mainly used in dealing with titanic numbers. The extended Euclids within UBASIC programs can already cope with titanic numbers albeit less efficiently and this routine is an adjunct which can be incorporated at a later stage.
11. BOBFFT4 on FTN90 is a Fast Fourier Transform multiplication system for, in this case, polynomials.

## Secret Sharing Software

This suite has been developed with four participants in mind but it can easily be extended to more. Programs are aptly named BOBSSH1 for secret-splitting and BOBSSH2 for reconstitution of secrets.

# Addendum

Space limited discussion of ANNF4. It is important to mention that for numbers to factorize using this method the smooth prime must be of the form 1 mod 4.
