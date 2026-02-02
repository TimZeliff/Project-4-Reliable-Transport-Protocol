# Wordle Client - README


## High-Level Approach

Before even looking at the word file, my first idea was a simple solution. Go through each word in the file and try the word, if it wasn't correct, try the next one until we got the flag. This approach was a sort of brute force attempt, but with unlimited attempts and no concern for efficiency, I could guarantee that it would always eventually guess the correct word.

I started my coding in Python with Google Colab, just connecting to the server and getting a message back. I used Google Colab throughout the entire project until the finishing steps, since I found it easier to test my code. Additionally, I had never used sockets before so I wanted to get this fundamental piece down before I complicated it with command line parsing and guessing strategies. This was honestly probably the hardest part of the project for me as I had zero previous experience with client-server communication, and it required a lot more reading than coding before I was able to get a working implementation.


## Guessing Strategy

While I was working on the socket connection, I also realized that the word file was alphabetical. Since we get feedback after we guess, we could theoretically eliminate 1/26th of the word list every time we guessed just by looking at the first letter. Additionally, once we identified the first letter, we could both cut down the amount of words we were searching for (5->4) as well as eliminate all other words (not starting with that letter). This sounded pretty good till I attempted to implement it, at which point I realized that while we could do this and then brute force the other 4 letters, we have no easy way of copying that process for the following letters. I also thought of implementing some sort of binary search using lexicographic comparison but quickly realized that wouldn't be possible given the feedback we get for each guess.

Thus far, I had thought about ways of limiting down the possible guesses, but was still primarily relying on brute force. I needed a way to act on the feedback the server was giving me. This was when I landed on my last strategy. The first step was keeping track of all the letters we know have to be in the word by using a list, and skipping every word in the file that didn't have those letters (this accounted for the 1's). Furthermore, we could keep a dictionary that stored position as a key and letter as a value, documenting every letter we know has to be in a given position, again discarding every word that didn't match that criteria (this accounted for the 2's). This approach worked well for me and is the strategy I use in my final implementation.


## Challenges Faced

Once the code ran as intended, I implemented the main function and command line parser. The command line parser works by verifying the correct number of arguments (has to be at least the 2 required ones, but can be no more than 5). Based on the number provided, we can make assumptions about them. For example, if only 2, we can assume they are the 2 required (hostname and username). If 3, we know that it is the 2 required plus the -s flag. We know this because the port flag would require 4 arguments (the flag, the port number, and the 2 required arguments).

Before submitting, I realized I still needed to validate the server's messages. To do this, I created a separate method that checks every response from the server. It verifies that the message has a type field and that each message type contains its required fields (for example, a "start" message must have an "id", a "bye" message must have a "flag", etc.). If the server sends an invalid or unexpected message, the client raises an error.


## Testing Overview

Lastly, I put my code as an executable, created a Makefile, added the words file, and ran it in my terminal. I tested my client by running it against the server multiple times to verify I could successfully retrieve both the regular and TLS flags.


## Resources

https://docs.python.org/3/library/ssl.html

https://docs.python.org/3/howto/sockets.html

https://www.datacamp.com/tutorial/a-complete-guide-to-socket-programming-in-python
