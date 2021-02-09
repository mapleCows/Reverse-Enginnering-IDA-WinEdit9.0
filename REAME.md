# CS 165 Reverse engineering IDA

## Introduction
Part 1:  Local authentication bypass -- capture the flag: 

In part 1 of the project, you will be asked to bypass the authentication used in a toy application created for this class. See authenticate_yourself.exe in the project folder. Upon successful authentication bypass, you will be awarded with a unique string that you can take to submit as part of the project outcome.

Part 2: Real-world authentication bypass:

In part 2 of the project, you will be asked to change a real world application so you can continue using it after the trial period is over. The application can be downloaded at http://www.winedt.com/archive/winedt90-32.exe. Note that the methodology and workflow transfer directly from Part 1. However, the code base will be much more complex and it will be harder to locate the correct function that performed the check on trial period. 

## Flag
Flag: 34gdfh340234

## Modified Binary
| Address| Original | Modified | Original | Modified|
| ------------- | ------------- | ------------- |------------- |:-------------:|
| 004010F0  | 75 5F | 74 5F | jnz short loc_40115D | jz short loc_40115D|
| 00401120  | 75 27| 74 27|jnz  short loc_401156|jz  short loc_401156|

## Thought Process
I started by stepping through the program from the beginning of the program to find where the login sub-function was being called. Found the call at address 00401330 calling subroutine sub_401080.

![stepthrougth](https://user-images.githubusercontent.com/38027847/107305127-37645580-6a37-11eb-97f5-c501a8d09e8f.PNG)

Once I found the subroutine I then looked for where the flag was and how the branches that are needed to reach the flag. I saw two jumps that will error change the username and password. The first jump at 004010F0 checks for the user name by comparing with the saved string “admin”. I flipped the jump from jnz to jz to allow for incorrect usernames to pass the test. 

![first jump](https://user-images.githubusercontent.com/38027847/107305132-392e1900-6a37-11eb-829a-2ad445b62e0f.PNG)

![first jump hex](https://user-images.githubusercontent.com/38027847/107305134-3a5f4600-6a37-11eb-9e3a-b087c210fd15.PNG)

I also flipped the second jump from jzn to jz to allow for incorrect passwords. 

![final](https://user-images.githubusercontent.com/38027847/107305141-40edbd80-6a37-11eb-9699-8d1475c1034f.PNG)

One problem with this approach is if you were to enter the correct credentials the flag would not be revealed. This could be a problem if you wanted to stealthily change the binary so that the correct credentials would not alert the user of any changes. 

![finasa](https://user-images.githubusercontent.com/38027847/107305144-42b78100-6a37-11eb-830b-3b7673426197.PNG)

## Part 2
For part two of the lab I searched the database for “trial” and found the sub call that handles the top banner. (sub_76B074) There is a test for al that checks if the program has been registered. We can simply change the jump parameters to a non conditional jump and jump over all the expired/unregistered labeling. 

## Modified Binary
| Address| Original | Modified | Original | Modified|
| ------------- | ------------- | ------------- |------------- |:-------------:|
| 0076B0A7  | 75 0F | 74 0F | jnz short loc_76B0B8 | jz short loc_76B0B8|

![this jump](https://user-images.githubusercontent.com/38027847/107305486-f02a9480-6a37-11eb-9de7-75624627b205.PNG)

![sub pic](https://user-images.githubusercontent.com/38027847/107305489-f15bc180-6a37-11eb-8eaf-230bfec3790b.PNG)

Top Banner Removal:
![1231232131](https://user-images.githubusercontent.com/38027847/107305491-f28cee80-6a37-11eb-9d1e-107fa09201ff.PNG)



