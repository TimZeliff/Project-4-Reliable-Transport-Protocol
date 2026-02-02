# FTP Client - README


## High-Level Approach

I was very unfamiliar with the implementation of FTP. I have taken Security+, so I had heard the name before but that was about it. Honestly I was pretty overwhelmed with the project initially. I started by reading through the entirety of the instructions and downloading FileZilla just so I could get some experience with an actual FTP client.

After playing around with FileZilla, I understood the gist of an FTP client and was ready to start brainstorming my implementation. I decided to write my code in Python since it was the language I was most comfortable with and the one I had used on the last project. Additionally, I used Google Colab to draft and test my initial implementation as I did not want to overcomplicate it right away by trying to write it in my terminal and preferred Google Colab's user friendliness.

I took the advice of the instructions for what to start with first and began creating my command line parser. This was not an issue as I had made one for the last assignment. Now I was on to establishing my control channel connection. This was a bit more complex, but again, I was still simply repeating the steps of the last assignment, so no issue there. Now it was on to the more challenging parts, implementing MKD and RMD as well as later implementing PASV and LIST, in which I needed to implement support for creating a data channel. This was where I was really charting unknown waters so to speak, as I had no familiarity with this process. Most of my time was spent researching. I was glad to have made the progress I did on the stuff I did know how to do before approaching the stuff I didn't know how to do, as this was a little overwhelming and demoralizing, but thankfully I was able to build on the momentum I started earlier and push through.

After I was able to figure out the data channel, the rest of the project was more or less smooth sailing. I was not done, but I had made enough progress where I felt confident in my ability to implement the remaining things. Since Google Colab doesn't work super well with the command line parser, I temporarily removed that functionality to opt for hard coded commands to test if things worked properly. I was able to check FileZilla to verify if my code was functioning properly. Once the testing phase was done, I began polishing and finalizing my documentation. This primarily included documenting my code, but also included creating the Makefile. Finally, I reimplemented the command line parser and began moving it over to my terminal so it could be run as intended. The Makefile was very minimal due to my using Python.

Throughout my implementation, I was constantly thinking about error checking. While writing my initial code, I created placeholder catches such as empty functions that I called on server responses, commented out if statements, and placeholder returns. This allowed me to focus on getting the core functionality working first while ensuring I had a structure in place to finalize the error handling later.


## Challenges Faced

I had to do a lot of research and was confused a bit on which commands required a data channel and which ones didn't. It was a little confusing to understand why some require a data channel while others don't. For example, make directory doesn't require it but move does.

It was difficult to implement the more complex commands like mv and cp. These required multiple messages be sent to the server as opposed to just one. They didn't have a matching server command like the others (ls = LIST, mkdir = MKD, etc). The TYPE I, MODE S, and STRU F commands were initially confusing as well.

I struggled a little bit with extracting the port and address from the server's response to PASV, mostly due to its string format. It was also confusing to understand that all commands go on the control channel even if they require the data channel. It took some time to figure out how to use the commands properly.

It was a little difficult figuring out how to listen for the data the server sent as opposed to the messages, but I eventually figured it out. The control channel messages end with a newline character, but the data channel just sends raw bytes until the connection closes.


## Testing Overview

I used FileZilla to verify my operations were working correctly. I tested ls by comparing my output to what FileZilla showed. I tested mkdir and rmdir by creating and removing directories and confirming the changes appeared in FileZilla. For cp, I uploaded and downloaded files and verified the contents matched. For mv, I confirmed the file appeared at the destination and was removed from the source. For rm, I deleted files and confirmed removal in FileZilla. Lastly, I ran my client multiple times with various commands to verify consistent behavior.


## Resources

https://stackoverflow.com/questions/50117522/how-to-connect-to-an-ftp-server-with-sockets

https://docs.python.org/3/library/urllib.parse.html

https://en.wikipedia.org/wiki/List_of_FTP_server_return_codes

https://docs.python.org/3/library/re.html

https://docs.python.org/3/library/os.html

https://docs.python.org/3/library/socket.html

https://www.w3schools.com/python/python_file_handling.asp
