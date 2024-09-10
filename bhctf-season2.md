``` As the captain of Team U-niverse, I participated to the Block Harbor x VicOne Automotive CTF Season 2 (2024). We couldn't make it to the final rounds. It was, however, a chance for us to learn new things. Personally, I was able to sharpen my reverse engineering skill a little bit. Here under I will place write-ups for the RE challenges that I solved. ```

---

#### 1. "Want a password"

- Overview: A zip file was provided which includes a password-locked zip file and a password generating module. The flag is hidden in the password-locked zip file while the module contains a .exe file along with a DLL file. Running .exe file (with the .DLL file in the same folder) on a Windows machine produced a random password. As hinted by the challenge description, the random password is generated based on the current time and the password to open the locked zip file is the one generated between two specific times.

- Solution
    - Unzip and run the .exe file we see that it generates a random password every time we click on the "Gen Pass" button. I also noticed a .dll file in the same folder to the .exe file.
    - I used dnSpy to disassemble the .exe file, but it didn't let me look inside the .dll file. I decided to use Ghidra to reverse the .dll file. It appeared that the .dll has a function named GenPass which randomly generate a password from two halves. The first half is generated using `timestamp - 1` as random seed while the second half used `timestamp`. I, however, realized that I couldn't rewrite the function in Python, as Python uses a different PRNG to C#.
    - I came up with the idea to not touch .dll, but just modify the .exe file so that whenever I click "Gen Pass" button, it will generate passwords continously for a long-enough time. After that, I just need to adjust my local system time to the time mentioned in the description. Then clicking the button and get passwords generated. I did confirm that the mentioned passwords exist in the list.
    - After getting the list of passwords, I manually try one by one to open the locked .zip file (I can use Hydra or similar tools, but the small number of candidates didn't worth using the tools) and found the correct password.

#### 2. "Gameboy Game"
- Overview: a Gameboy ROM was given. We are asked to score more than 32767 points to get the flag.

- Steps:
    - Step 1: There are many Gameboy emulators that allows debugging the game. I used 


#### 3. "Car game"
- Overview: a game written in SDL2 framework was provided. There was instructions on how should we run the game on Ubuntu 22.04 and the necessary libraries. We need to score more than 1337 points to get the flag.

- Steps:
    - Step 1: I used GDB to debug the game and Ghidra to understand what 
