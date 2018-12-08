# Batch processing

In olden times, people would write programs on paper; the paper would be
given to someone to type onto punch cards (one command per card); the
cards would be put in a box and given to the computer operator, who
would then put the cards into the card reader; the computer would read
the cards, execute the commands on them, and produce some output; the
person would then pick up their output and their cards when it was done.

Those are still the steps you will take with most of the programs you
will use for neuroimaging.  The outline of your analysis may come from
a web site; instead of giving your program to a typist, you click
buttons; there's no card reader, but the computer still reads something
to do what you want it to do; your output goes into a window on the
screen instead into a box behind the pick-up window at the computer
center.

Using the cluster reintroduces you to some of those intermediate steps.

## How to work with batch processing efficiently

1. Start with the smallest amount of data you can
1. Get the syntax and flow correct for one subject/instance
1. Introduce multiples, e.g., three subjects instead of one
1. Increase the size of the data if it was reduced and rerun
1. Run the final configuration

You will almost certainly use the cluster because you want more of
something: maybe more memory, maybe more subjects, maybe more disk
space.

Do yourself a favor, develop your workflow in small steps, than you
can run relatively quickly and catch errors.  Increase or change
one thing at a time until you can run the whole thing with some
confidence that you won't use 128 GB of memory to get a `Command not
found` message, or, possibly worse yet, generate 200 (or 2000) e-mail
messages to that effect in your Inbox.
