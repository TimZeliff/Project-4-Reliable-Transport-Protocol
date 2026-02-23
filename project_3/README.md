# BGP Router - README


## High-Level Approach

I found starting the project to be pretty difficult. While I had a strong understanding of BGP routers in concept, and I understood their import export rules and the why behind them, I found the coding to be slightly challenging. For starters, I had not had experience dealing with multiple UDP sockets. Before even starting to write my code I took a good chunk of time just learning the documentation and how they worked.

Then, I started with trying to get my router to handle announcements. I wanted to update my forwarding table based on neighbor messages. After that I wanted to then be able to update the appropriate neighbors of routes I received based off of my relationship with the sender and the possible receiver. It was a little tricky to figure out the logic but once I figured it out it was pretty neat. The rules dictate I wanted to update everyone about my customer routes, and only update customers about peer and provider routes. I also obviously didn't want to update the sender about information they just sent me.

From there, I decided to figure out the handling for dump messages. Since I had already implemented my forwarding table and how to update it, the handling of dump messages proved to be pretty simple.

Next I worked on forwarding data messages. I had to loop through each element in my forwarding table, convert their netmask and network to binary, and check if the destination matched using bitwise operations. For every possible match, I had to implement the tie-breaking rules to find the best route. I also had to make sure I was only forwarding data legally based on the relationship between the source and destination.

After that I implemented withdrawal handling. I decided to cache all updates and withdraws, and when a withdrawal came in, I would throw out the entire forwarding table and rebuild it by looping through my cached updates and excluding any that matched the withdrawn routes.

The hardest part that I saved for last was implementing route aggregation. Writing out example networks and figuring out the process I used to aggregate them proved helpful in translating the process into code. I had to find two routes that were numerically adjacent, forwarded to the same next-hop router, and had the same attributes. If all conditions were met, I could combine them into a single route with a shorter netmask. I called this recursively since aggregation on top of aggregation might be possible.


## Challenges Faced

The biggest hurdle of the project was figuring out how to best utilize our dictionary of sockets. This was conceptually complex at first, but once I was able to understand it, being able to iterate through them and get the information I needed to make decisions proved crucial.

Figuring out the logic to check if a destination was in a network/netmask proved to be pretty conceptually complicated. It took me a good bit of time and research to figure it out. Eventually I landed on a strategy of doing bitwise operations, particularly the & operator, and checking if the possible network matched the actual destination.

A big problem I faced was consistently getting confused with what dictionaries and JSON objects held. Due to all the different types of information they held, I often would get confused with what I should check is equal to what, like src vs peer.

Getting aggregation to work was very tricky and I had to do a lot of debugging. Having to find the translation from the logic I used to Python bitwise logic proved to be very difficult. Disaggregation proved to be complex too. I eventually landed on throwing out the whole forwarding table, rebuilding it through cached updates, removing the withdrawn routes, and reaggregating where appropriate.


## Testing Overview

I tested my router by running it against the provided test configurations. I started with the level 1 tests and worked my way up to level 6. For each level, I would run the simulator, check the output for errors, and debug accordingly. Consistently creating and maintaining strong documentation really helped me keep up my productivity throughout the project. Taking multiple sessions and days to work on the project, without documentation, I would have to relearn all the conceptual understanding I had gained from previous sessions. Instead, I was able to manipulate previous code snippets to use for future methods and understand how each method worked with one another. This alone must have saved me at least twice as much time.


## Resources

https://www.geeksforgeeks.org/python/python-operators/
